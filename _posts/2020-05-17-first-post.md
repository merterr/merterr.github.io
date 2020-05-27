---
title: Donanımdan Yazılıma Doğru
layout: post
description : Bilgisayar nedir?
---

# Bilgisayar nedir?

>İlk  bilgisayar olarak kabul edilen ENIAC ağırlıklı olarak hava tahminlerinde, atom enerjisi hesaplamalarında, kozmik ışın çalışmalarında, termal tetikleme, rastgele sayı bulunmasında, rüzgâr tüneli dizaynında ve diğer bilimsel araştırmalarda kullanıldı. 
> - (wikipedia)

Sözlük tanımıyla, önceden belleğine yüklenmiş bir izlenceye (yazılıma) göre komuta edilerek, çok sayıda ve karmaşık mantıksal ve aritmetiksel işlemlerden oluşan bir işi çok kısa sürede yapıp sonuçlandırabilen aygıt.

Kabaca verilen girdileri işleyip bu girdilere göre belli cıktılar veren, bu cıktıları saklayabilen ve tekrar kullanılmasına imkan veren bir hesaplama makinesidir. 

## Bilgisayarın Bölümleri

Bilgi girişi, bilginin işlenmesi ve bilgi çıkışı sırasında kullanılan bilgisayar parçalarını yaptıkları işe göre 5 gruba ayırabiliriz.


### Girdi Üniteleri
Bilgilerin bilgisayara aktarılmasını sağlar. Örneğin: Klavye, fare, tarayıcı, kamera ya da mikrofon.

### Merkezi İşlem Birimi
Veriyi işlemeyi sağlar. Bilgisayardaki sadece 1 tane merkezi işlem birimi vardır, o da işlemcidir.

### Çıktı Üniteleri
İşlemcinin çıkan sonuçları dışarı aktarabilmesini sağlar. 
Örneğin: Monitör, yazıcı, sürücüler ya da hoparlör.
Ekran, hoparlör,yazıcı

### Hafıza Birimi
Bilgisayar tarafından işlenecek bilgileri, programları depolamayı sağlar. 
Örneğin: Ram bellek, sabit disk, CD ya da DVD.

### Veriyolu
Bilgisayarda, bir birimden diğerine veri aktarmak için veriyolları kullanılır.
Örneğin: Klavyeden basılan bir tuşun bilgisi işlemciye veriyolu vasıtasıyla iletilir.

# Bilgisayar Nasıl Çalışır?

Bigilerin alınması, işlenmesi ve çıktı olarak istenilen yere gönderilmesi işlemcinin yönetiminde gerçekleşir. İşlemciyi yöneten şey ise programdaki komutlardır. Bu komutlar 0 ve 1'lerden oluşmuş sayı dizileridir. Bu kısım şimdilik bu kadar olsun.

# Klavyeden Girilen Bir Karakterin Yaşamı

Şimdilik bilgisayarda bir metin belgesi açalım. Biz bir metin belgesini açtığımızda o belge belleğe yüklenir.
Metin belgesine bir şeyler yazdığımızda yazılar hala bellekte bulunur. Kaydet tuşuna bastığımızda belgeyi kalıcı olarak sabit diske kaydetmiş oluruz.

Peki bu metin belgesine yazdığımız yazılar nasıl bir işlemden geçerek sabit diske kaydedilir?

Klavyedeki her bir karakterin ASCII tablosunda bir sayısal karşılığı vardır. 
Peki ASCII tablosu nedir? ASCII tablosu tüm bilgisayar için standartlaşmış bir karakter kümesidir. Klavyedeki her bir karakterin 2'lik sistemdeki bir sayıya dönüşmesine aracılık eden sayısal değerleri tutan bir tablodur. [ASCII tablosu için tıklayın.](https://tr.wikipedia.org/wiki/ASCII) Bilgisayar, klavyeden girilen her bir karakterin bu tablodaki sayısal karşılığını kullanır. 

Örneğin: Klavyeden A (büyük harf) tuşuna bastığımızı düşünelim. Bu harfin ascii karşılığı 65 (onluk sayı siteminde)' tir.
Fakat bilgiyar 2'lik sistemde çalışır. Bu yuzden 65'i 2'lik sisteme çevirmeliyiz. Sonuç: 1000001
Artık A harfi ram'e yazıldı.

Sadece metin belgesi yazma sırasında değil, yazılan kod derlenirken de benzer işlem gerçekleşir.

# Yazılım Nedir
İşlemciye iş yaptırmak için kullanılan komutlardır.
Mesela işletim sistemi bir yazılımdır. Biz işletim sistemini kullarak bilgisayarı yönetiriz.
Hatta bu işletim sistemi sayesinde özelleşmiş yazılımlar da üretebiliriz. Bu yazılımlarla bilgisayara pek çok iş de yaptırabiliriz. Örneğin: Oyun oynamak, mail göndermek, görüntülü konuşmak.

Bilgisayar yazılımları genel olarak 3 ana grupta incelenebilir.

## Sistem yazılımları
Bilgisayarın kendisinin işletilmesini sağlayan, işletim sistemi, çeşitli donatılar (facility) gibi yazılımlardır. Donanımı kontrol eden sürücüler de bu gruba dahildir.

## Uygulama yazılımları
Bir kullanıcının istekleri doğrultusunda özelleştirilmiş yazılımlardır. İçerik yönetim sistemleri, oyunlar, mail programları, stok kontrol ve kütüphane programları örnek olarak verilebilir.

## Çevirici yazılımlar
Herhangi bir dilde yazılan programı makine diline çeviren yazılımlardır. Bu yazılımlara derleyici yazılımlar denir.

# Yazılım dilleri 
Yazılım dili, yazılımcının bir algoritmayı bilgisayarın anlaması için kullanıldığı sözdizimidir.
Aslında yazılım dilleri makine ile insan arasında bir köprüdür.

# Yazılım Nasıl Çalışır?
Her yazılım dilinin kendine ait bir söz dizimi vardır. Hatta her yazılım dilinin kendine ait kabiliyetleri de vardır. Her işi her yazılım dili yapamaz ya da zor yapar. Mesela yapay zeka için python kullanılabilir. Çünkü kütüphaneleri fazladır.Yada  lisp gibi kullanılır. Çünkü yapay zeka için kütüphaneleri fazladır.

---
## Kaynakça

* [Wikibooks](https://tr.wikibooks.org/wiki/Programlama_Temelleri/Bilgisayar%C4%B1n_%C3%87al%C4%B1%C5%9Fma_Mant%C4%B1%C4%9F%C4%B1)

* [Wikipedia - Bilgisayar](https://tr.wikipedia.org/wiki/Bilgisayar)
* [Wikipedia - Yazılım](https://tr.wikipedia.org/wiki/Yaz%C4%B1l%C4%B1m)





