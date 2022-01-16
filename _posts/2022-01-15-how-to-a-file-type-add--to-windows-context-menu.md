---
layout: post
title: Windows 10'da Yeni Bir Dosya Tipi Masaüstü Menüsüne Nasıl Eklenir? (How to a New File Type Add to Windows 10 Context Menu)

---

Arama kutusuna ***Regedit*** yazılır ve enter'a basılır ve ***Kayıt Defteri Düzenleyicisi*** açılır.
***Bilgisayar\HKEY_CLASSES_ROOT*** adresine gidilir.<br/>

![Regedit]({{site.baseurl}}/public/images/2022-01-15/1.PNG)

Sonra ilgili uzantının klasörüne gidilir.
Örneğin masaüstü menüsünde bir ```javascript``` dosyası oluşturmak için ***Bilgisayar\HKEY_CLASSES_ROOT\\.js***
veya bir ```html``` dosyası oluşturmak için ***Bilgisayar\HKEY_CLASSES_ROOT\\.html***
yoluna gidilir.

![Regedit2]({{site.baseurl}}/public/images/2022-01-15/2.PNG)

*Mouse sağ tık -> Yeni -> Anahtar* denilerek ```Anahtar``` oluşturulur ve ismi ```ShellNew``` olarak değiştirilir.

![Regedit3]({{site.baseurl}}/public/images/2022-01-15/3.png)

Bu anahtar seçili iken *Mouse sağ tık -> Yeni -> Dize Değeri* denilerek yeni bir ```Dize Değeri``` oluşturulur.

![Regedit4]({{site.baseurl}}/public/images/2022-01-15/4.png)

Bu dizenin adı ```NullFile``` olarak değiştirilir ve ```Veri``` alanı boş bırakılır ve çıkış yapılır.

![Regedit5]({{site.baseurl}}/public/images/2022-01-15/5.PNG)

Ve Final...

![Regedit6]({{site.baseurl}}/public/images/2022-01-15/6.png)