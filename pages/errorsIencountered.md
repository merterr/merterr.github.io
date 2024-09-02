---
layout : page
title : Yazılımda Karşılaştığım Hatalar
order : 4
---

<p class="message">
Yazılımcı hatayı bulacağım diyen, hatayı bulan ve onu giderebilendir! 🤯
</p>
Program yazarken en sık yaptığım hatalar genelde en basit hatalar oluyor. Söz dizimi hatası, fonksiyonlara eksik yada hatalı parametre verme, sınıf implementasyonu unutmak, gerekli kütüphaneleri import etmeyi unutmak gibi. Bu gibi hatalara burada vermeyeceğim. Fakat yeni bir teknoloji öğrenirken yaptığım hatalara burada yer vereceğim. Hem kendim hem de ziyaretçiler için yararlı olacağını düşünüyorum.

### Tavsiyeler
1. Bir yazılım projesini incelerken önce proje başlangıç dosyasını bulun. (örneğin: ASP.NET Core için Program.cs). Ardından adım adım ilerleyin. (debugging yani)

2. Bir yazılımda hata oluştuğunda, hatanın oluştuğu satırdan bir önceki satırı dikkatlice incelemekte fayda var. Eğer program birden fazla dosyadan oluşuyorsa bir önceki dosyayı incelemek gerekir. Bu şekilde gerekirse programın en başına doğru gidilmelidir.

3. Başka bir yapılan hata da çağrılan fonksiyonun ne döndürdüğünün ve hangi tipte döndürdüğünün tespit edilmemesidir. Örnek: int değer döndüren fonksiyondan gelen cevabı string değişkene atamak. Bu durumda tip dönüşümü yapılmalı. Yada AClass tipinden bir List'i BClass tipinden bir List'e atamak. Bu durumda mapping yapılmalı.

4. Yazdığınız kodunuzun doğru olduğuna inanıyorsanız ve github'da olan açık kaynaklı bir kütüphane kullanıyorsanız ilgili kütüphanenin repository'sindeki issues'i kontrol edin. Belki de o kütüphanede hata olabilir. Ek olarak kütüphanenin sürümüne de dikkat edin.

---

## Hatalar

1. "primary key constraint duplicate key error" Hatası ve Çözümü

Bu hata genellikle PK'sı otomatik olarak artan tablolara elle kayıt eklenmesinden sonra oluşur.
PK'yı en son kaldığı yerden başlatmak gereklidir.


PSQL'de çözümü

```sql
SELECT setval('"table_name_id_seq"', (SELECT MAX("pk_col_name") FROM "table_name"));
```

MSSQL'de çözümü

```sql
DBCC checkident ('TableName', reseed, last_pk)
```
