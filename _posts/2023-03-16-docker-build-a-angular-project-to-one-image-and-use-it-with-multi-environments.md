---
layout: post
title: Docker Build ile Bir Angular Projesini Tek Docker Image'a Dönüştürüp Çoklu Ortamda Kullanma

---


# Environment Dosyaları değiştirme

## 1. Adım
Angular projenizdeki `src/assets/` yoluna gidelim.
Burada `environments` isimli bir klasör oluşturalım. Bu klasörün içine `env.json`, `env.prod.json`, `env.test.json`
isimlerinde json dosyalarını oluşturalım.

`env.json` dosyasının içine config'leri yazın. Örneğin;

```json

{
  "production": false,
  "isLogConsole": false,
  "apiUrl": "http://localhost:4200",
  "tokenRefreshInterval": 3600
}
```

`env.prod.json` dosyasının içine config'leri yazalım. Örneğin;

```json

{
  "production": true,
  "isLogConsole": true,
  "apiUrl": "https://api.prod.com",
  "tokenRefreshInterval": 3600
}
```
`env.test.json` dosyasına da benzer şekilde config'lerinizi yazalım.

### Config Dosyasının (env.json) Kullanımı

`config-model.ts` isminde bir dosya oluşturalım. Bu dosya `env.json` dosyasındaki verileri okumayı kolaylaştırmak içindir.

```ts
export type ConfigModel = {
  production: boolean;
  isLogConsole: true;
  apiUrl: string;
  tokenRefreshInterval: number;
};

```

configuration-loader.ts isminde bir dosya oluşturalım. Dosya içeriği aşağıdaki gibidir. Aşağıdaki kodlarla env.json dosyasındaki config'ler elde edilir.
```ts
import {Injectable} from '@angular/core';
import { HttpClient } from '@angular/common/http';
import {ConfigModel} from "../model/config-model";

@Injectable({
  providedIn: "root"
})

export class ConfigurationLoader {
  private readonly CONFIG_FILE_URL = "./assets/environments/env.json";
  private config: ConfigModel;

  constructor(private http: HttpClient) {}

  public loadConfiguration(): Promise<ConfigModel> {
    return this.http
      .get(this.CONFIG_FILE_URL)
      .toPromise()
      .then((config: any) => {
        this.config = config;
        return config;
      })
      .catch((error: any) => {
        console.error(error);
      });
  }

  getConfiguration(): ConfigModel {
    return this.config;
  }
}

```
Daha sonra `app.module.ts` isimli dosyayı bulun. Aşağıdaki kodları bu dosyaya ekleyelim. Modül başlatılırken env.json dosyası okunacaktır ve config'ler hazır hale gelecektir.

```ts
import {ConfigurationLoader} from "./service/configuration-loader";
import {ConfigModel} from "./model/config-model";

...
export function loadConfiguration(configService: ConfigurationLoader): () => Promise<ConfigModel> {
  return () => configService.loadConfiguration();
}

...

@NgModule({
  ...
   providers: [
    ...
    {
      provide: APP_INITIALIZER,
      useFactory: loadConfiguration,
      deps: [ConfigurationLoader],
      multi: true
    }
  ]
  ...
})
...

```


`base-service.ts` isimli bir servisimiz olsun. Bu servisin içine config'lerin okunması için aşağı kodu ekleyelim.

```ts

@Injectable()
export class BaseService {
  environment: ConfigModel;
  constructor(private configLoader: ConfigurationLoader) {
    this.environment = configLoader.getConfiguration();
  }

  getList(apiControllerName: string, action: string){
    return this.http.get(this.environment.apiUrl + '/' + apiControllerName + '/' + action, ...);
  }
}

```

## 2. Adım

`angular.json` dosyasını açın. `build`'in altındaki  `configurations` kısmını aşağıdaki şekilde güncelleyelim.

```json
...
"build": {
  ...
  "configurations": {
    "prod": {
      "fileReplacements": [
        {
          "replace": "src/assets/environments/env.json",
          "with": "src/assets/environments/env.prod.json"
        }
      ],
      ...
    },
    "test": {
      "fileReplacements": [
        {
          "replace": "src/assets/environments/env.json",
          "with": "src/assets/environments/env.test.json"
        }
      ],
      ...
    }
  }
  ...
}
...

```
## 3. Adım

package.json dosyasını açalım. Build için bir config tanımlayalım. _test_ veya _prod_ olarak environment kullanacak olsak dahi _prod_ için build alabiliriz. Config dosyasını proje başlatılırken zaten değiştireceğiz. Burada amaç `angular.json` 'da prod için belirtilen diğer configleri projemize uygulamak. `angular.json` dosyasına _test_ ve _prod_ için ortak config'de yazılabilir. O config'i de build alırken geçebilirdik.

```json
...
scripts:{
  ...
  "build_prod": "ng build --configuration prod --outputPath=dist/angular --namedChunks=false --aot --output-hashing=all --sourceMap=false",
  ...
}
...

```

## 4. Adım

main.ts dosyasını açın ve aşağıdaki şekilde güncelleyelim. Proje başlatılırken docker run ile geçeceğimiz _test_ yada _prod_ environment'ına göre bu dosyalardan birinin içeriğini env.json dosyasına kopyalanacak ve dosya içeriği okunacaktır.

```js

fetch('assets/environments/env.json').then(response => {
  return response.json();
}).then(data => {
  if (data.production) {
    enableProdMode();
  }
  platformBrowserDynamic().bootstrapModule(AppModule)
    .catch(err => console.error(err));
});
```

Angular tarafında işimiz bitti.

## 5. Adım

Angular projesindeki ana dizinde (Dockerfile'ın bulunduğu) `entrypoint.sh` bir dosya oluşturalım.
Bu dosyanın içine aşağıdaki komutları ekleyelim.

```sh
chmod +x /entrypoint.sh
chmod -R 755 /usr/share/nginx/html/
chown -R nginx:nginx /usr/share/nginx/html/
cp /usr/share/nginx/html/assets/appsettings/env.${ENVIRONMENT}.json /usr/share/nginx/html/assets/appsettings/env.json
/usr/sbin/nginx -g "daemon off;"
```

Daha sonra Dockerfile'ı açalım. Ve aşağıdaki kodları dosyamıza ekleyelim.

```Dockerfile
...
FROM node:10-alpine as build-step
RUN mkdir -p /app
WORKDIR /app
COPY package.json /app
RUN npm install
COPY . /app
RUN npm run build_prod

FROM nginx:stable
COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=build-step /app/dist/angular /usr/share/nginx/html
COPY ["entrypoint.sh", "/entrypoint.sh"]
CMD ["sh", "/entrypoint.sh"]
```

## 6. Adım
### Docker image'ın çalıştırılması

Image Oluşturma

`docker image -t deneme-image:latest .`

Container'ı Ayapa Kaldırma

`docker run -dit  -e ENVIRONMENT=test -e TZ=Europe/Istanbul -p 80:80 --name=cont_test --restart=always deneme-image:latest`

`docker run -dit  -e ENVIRONMENT=prod -e TZ=Europe/Istanbul -p 80:80 --name=cont_prod --restart=always deneme-image:latest`


---
Kaynaklar:

_https://itnext.io/how-does-app-initializer-work-so-what-do-you-need-to-know-about-dynamic-configuration-in-angular-718e7c345971_
_https://gabrielemilan.dev/publish-same-docker-image-with-angular-application-to-any-environments-edbfc454c6bb_

