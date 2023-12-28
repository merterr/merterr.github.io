---
layout: post
title: Windows'ta Batch Script Dosyası (.bat) Kullanarak Toplu İş Yapmak

---


.bat dosyasında kullanılan komutlar batch script olarak geçer.
Önce işimize yarar birkaç komut görelim ve bunları komut satırında deneyelim.

## Klasör İşlemleri
### Klasör Oluşturmak
```cmd
$ mkdir test-klasoru
```

### Birden Fazla Klasör Oluşturmak
```cmd
$ mkdir test-klasoru-1 test-klasoru-2
```

### Klasör Silmek
```cmd
$ rmdir test-klasoru-1
```

### Birden Fazla Klasör Silmek
```cmd
$ rmdir test-klasoru-1 test-klasoru-2
```


### Klasörü Yeni Pencerede Açmak
```cmd
$ start klasör_yolu
```

ör: C klasörünü açmak
```cmd
$ start c:\
```

## Dosya İşlemleri

### Dosya Oluşturmak

Boş dosya oluşturmak

```cmd
$ type nul > test-klasoru/test.bat
```
Dosyaya veri eklemek

**(>)** işareti var olan veriyi silip sıfırdan ekleme yapar.

```cmd
$ echo metin_icerigi > test-klasoru/test.bat
```

**(>>)** işareti var olan verinin üzerine ekleme yapar.

```cmd
$ echo metin_icerigi_1 >> test-klasoru/test.bat
```

### Dosyayı Silmek
```cmd
$ del test-klasoru/test.bat
```


### Bir dosyayı Açmak
ör: notepad++ ile `start_apps.bat` dosyasını açmak
```cmd
$ start notepad++.exe start_apps.bat
```

ör: Visual Studio 2022 ile klasör açmak
```cmd
$ start devenv.exe dotnet-core-excel-helper
```

ör: Visual Studio 2022 ile .net projesi açmak
```cmd
$ start devenv.exe dotnet-core-excel-helper/ExcelHelperProject.sln
```

ör: WebStorm ile proje klasörü açmak
```cmd
$ start webstorm64.exe angular15-sample-project
```


## Batch Script Dosyası Oluşturma
Bu ön bilgilerden sonra start_apps.bat adında bir batch script dosyası oluşturalım. Amacımız bilgisayarımız açıldığında start_apps.bat dosyasını çalıştırarak isteğimiz birçok programı tek seferde açmak.

```cmd
$ type nul > start_apps.bat
```

Dosyamızı notepad++ ile açalım.
```cmd
$ start notepad++.exe start-apps.bat
```

Bat dosyamızın içine toplu açmak isteğimiz programların .exe uzantılı dosyalarının bulunduğu yolu ekleyelim.

```cmd
start C:\PROGRA~1\Google\Chrome\Application\chrome.exe
start datagrip64.exe
start devenv.exe D:\projects\dotnet-core-excel-helper\ExcelHelperProject.sln
start C:\"Program Files"\JetBrains\"WebStorm 2021.1.3"\bin\webstorm64.exe D:\projects\angular15-sample-project

```

__Açıklama:__

`C:\PROGRA~1` "C:\Program Files" yolunu temsil eder. Bu şekilde de kullanılablir. Boşluklu klasör adı veya yolu varsa çift tırnak ("") kullanılmalıdır.
Programlar `the sytem environment variables -> Path`' a ekli ise direkt .exe olarak da çalışır (`start datagrip64.exe` gibi)' aksi halde tüm yol girilmelir.

... ve son olarak start_apps.bat dosyasını çalıştıralım.

---

