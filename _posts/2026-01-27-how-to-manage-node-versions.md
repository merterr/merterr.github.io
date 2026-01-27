---
layout: page
title: Node Versiyonları Nasıl Yönetilir?
---

*Birden fazla angular projesini aynı bilgisayarda kullanmak istediğimizde node version hataları ile hepimiz karşılaşmışızdır. Bu sorunu nvm-windows ile çözeceğiz.*

*Nodejs sürümlerini nvm-windows aracılığıyla indireceğiz.*


## Windows için nvm-windows uygulamasını indirme 

[nvm-windows](https://github.com/coreybutler/nvm-windows/releases/tag/1.2.2) uygulamasını github üzerinden indirelim ve kuralım.

Uygulamanın başarılı bir şekilde yüklendiğini kontrol etmek için komut satırında aşağıdaki komutunu çalıştıralım.

```sh
$ nvm -v 
```

## Nodejs sürümlerini indirelim.
Örnek olarak 2 farklı nodejs sürümü indirelim.
Örneğin Angular 18 projesi için nodejs 22.22.0 versiyonu seçelim.

```sh
$ nvm install 22.22.0
```
Ya da Angular 14 projesi için de nodejs 16.10.0 versiyonunu seçelim.
```sh
$ nvm install 16.10.0
```

## Projeye göre node paketi seçme

nodejs 22.22.0 versiyonunu kullanmak için aşağıdaki komutu kullanalım.

```sh
$ nvm use 22.22.0
```

Yada nodejs 16.10.0 versiyonunu kullanmak için aşağıdaki komutu kullanalım.

```sh
$ nvm use 16.10.0
```

Hangi nodejs sürümlerinin yüklü olduğunu ve hangi nodejs sürümünün aktif olduğunu görmek için aşağıdaki komutu çalıştıralım.

```sh
$ nvm list
```
