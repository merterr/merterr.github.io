---
layout: page
title: Node.js Versiyonları Nasıl Yönetilir?
---

*Birden fazla angular projesini aynı bilgisayarda kullanmak istediğimizde node version hataları ile hepimiz karşılaşmışızdır. Bu sorunu nvm-windows ile çözeceğiz.*

*Node.js sürümlerini nvm-windows aracılığıyla indireceğiz.*


## Windows için nvm-windows uygulamasını indirme 

[nvm-windows](https://github.com/coreybutler/nvm-windows/releases/tag/1.2.2) uygulamasını github üzerinden indirelim ve kuralım.

Uygulamanın başarılı bir şekilde yüklendiğini kontrol etmek için komut satırında aşağıdaki komutunu çalıştıralım.

```sh
$ nvm -v 
```

## node.js sürümlerini indirelim.
Örnek olarak 2 farklı node.js sürümü indirelim.
Örneğin Angular 18 projesi için node.js 22.22.0 versiyonu seçelim.

```sh
$ nvm install 22.22.0
```
Ya da Angular 14 projesi için de node.js 16.10.0 versiyonunu seçelim.
```sh
$ nvm install 16.10.0
```

## Projeye göre node paketi seçme

node.js 22.22.0 versiyonunu kullanmak için aşağıdaki komutu kullanalım.

```sh
$ nvm use 22.22.0
```

Yada node.js 16.10.0 versiyonunu kullanmak için aşağıdaki komutu kullanalım.

```sh
$ nvm use 16.10.0
```

Hangi node.js sürümlerinin yüklü olduğunu ve hangi node.js sürümünün aktif olduğunu görmek için aşağıdaki komutu çalıştıralım.

```sh
$ nvm list
```
