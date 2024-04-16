---
layout: post
title: Bir ASP.NET Framework Web Projesini Windows Docker Container Olarak Yayınlama

---
## Giriş

*Bu örnekte .NET Framework 4.8 ve ortam olarak Visual Studio IDE kullanacağız.*

## Proje Oluşturma
**Test.UI** adında bir **ASP.NET Web Application (.NET Framework) MVC** projesi oluşturalım. Solution ismi "Test" olsun.
Kompleks bir örnek olması adına solution'a bir katman daha ekleyelim. Bu katman **Test.Service** adında **Class Library (.NET Framework)** olsun.

## Proje Yapısı

Test.UI katmanının Test.Service katmanı ile haberleşmesi için Test.Service katmanını Test.UI katmanının referanslarına ekleyelim.
Test.UI katmanına sağ tık yapalım. Açılan menüden "Add->Reference" tıklayıp "Projects->Solution->Test.Service" katmanını seçelim.

Test.Service katmanının görevi veritabanından veriyi alıp Test.UI katmanına aktarmak, mapping olayları vs. olabilir.

Kullanıcı etkileşimi Test.UI katmanıyla yapılmaktadır.
Test.UI katmanı çalıştırabilir bir katmandır ve projenin başlangıç katmanıdır. Bu yüzden build ve publish işlemleri bu katman için yapılacaktır.

![Proje Yapisi](/public/images/2024-04-16/proje-yapisi.png)

## Build ve Publish Alma
Bir publish profile oluşturalım.

Visual Studio'daki üst menüden "Build->Publish" butonuna tıklayalım. "Target->Folder" olarak seçelim. `Configuration : Release` olması gerekmektedir. Tercihen `Delete existing files : true` olabilir.
`Folder Location : bin\app.publish\` olsun. Publish penceresinde "More actions->Rename" butuna tıklayarak profile dosyasının ismi değiştirebilir. Bizim publish dosyasının adı **Publish.pubxml** olsun.

Oluşturulan bu publish profile dosyası "Test.UI->Properties->PublishProfiles" konumundadır.



## Komut İstemi ile Publish Alma
**Publish.pubxml** adındaki profile göre build alma

Projenin ana dizinine gidelim ve aşağıdaki kodu terminal aracılığıyla çalıştıralım.

```bash
$ msbuild /p:PublishProfile=Publish /p:DeployOnBuild=true
```

"Test/Test.UI/bin/app.publish/" yoluna eriştiğimizde projenin derlenmiş halini görmeliyiz.

![Projenin Derlenmis Hali](/public/images/2024-04-16/derlenen-proje.png)

Bu kısım önemli çünkü bu kısmı Dockerfile'a ekleyeceğiz.

## Projeyi Dockerize Etme
Projenin **ana dizinine** Dockerfile ve .dockerignore dosyalarını ekleyin.

### .Dockerignore
.dockerignore dosyasının içeriği aşağıdaki gibi olabilir.
```
.dockerignore
.env
.git
.gitignore
.vs
.vscode
docker-compose.yml
docker-compose.*.yml
*/bin
*/obj
README.md
```

### Dockerfile

```Dockerfile
FROM mcr.microsoft.com/dotnet/framework/sdk:4.8 AS build
WORKDIR /app
EXPOSE 80

# .csproj dosyalarini kopyala ve paketleri yukle
COPY *.sln .
COPY ./Test.UI/*.csproj ./Test.UI/
COPY ./Test.UI/*.config ./Test.UI/
COPY ./Test.Service/*.csproj ./Test.Service/
RUN nuget restore

# katmanlari kopyala ve build al.
COPY Test.UI/. ./Test.UI/
COPY Test.Service/. ./Test.Service/
WORKDIR /app/Test.UI
RUN msbuild /p:PublishProfile=Publish /p:DeployOnBuild=true


# uygulamayi calistirma
FROM mcr.microsoft.com/dotnet/framework/aspnet:4.8-windowsservercore-ltsc2022 AS runtime
# image boyutu 5GB dan biraz fazla
WORKDIR /inetpub/wwwroot
COPY --from=build /app/Test.UI/bin/app.publish/ ./
```

### Ek Bilgi

Dockerfile'daki `msbuild` komutu ile publish profile olusturulmadan da build alinabilir.
Fakat derlenmiş çıktının hangi klasörde oluştuğuna dikkat edilmelidir.

Örnek:
```Dockerfile
WORKDIR /app/Test.UI
RUN msbuild Test.UI.csproj /p:Configuration=Release /p:OutputPath=./bin/app.publish
# bu sekilde derleme yapildiginda derlenen cikti /app/Test.UI/bin/app.publish/_PublishedWebsites/Test.UI yolunda bulunur.

# uygulamayi calistirma
FROM mcr.microsoft.com/dotnet/framework/aspnet:4.8-windowsservercore-ltsc2022 AS runtime
# image boyutu 5GB dan biraz fazla
WORKDIR /inetpub/wwwroot
COPY --from=build /app/Test.UI/bin/app.publish/_PublishedWebsites/Test.UI/ ./
```
### Uygulamanın Docker Image'ini Oluşturma
```bash
$ docker build -t test-net2022 .
```
### Docker ile Uygulamayı Ayağa Kaldırma

```bash
$ docker run --name test_ui_cont -p 88:80 -dit test-net2022
```


![Sonuc](/public/images/2024-04-16/result.png)

