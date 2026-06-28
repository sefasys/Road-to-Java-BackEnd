Spring Framework'ü Java'nın aşırı kompleks ve fazlasıyla manuel şekilde gerçekleştirmemiz gereken yapısını sunduğu özellikler sayesinde kolaylaştıran bir yapı.

![[Pasted image 20260628162815.png]]

Yukarıda gördüğümüz analojide ağacın gövdesi Spring Framework'ü ve tüm bu yapraklar, dallar framework'teki farklı özellikleri gösteriyor.
Görsel, Spring komple bir framework gibi değil de bu dalları tekli şekilde de kullanabildiğimizi gösteriyor. Yani örneğin framework'teki sadece A özelliğini kapsayan kısmı projemize import edebiliyoruz.

- HTTP istekleri
- Database bağlantısı
- Authentication
- Dependency yönetimi
- Config dosyaları
- Logging
- Transaction yönetimi
- JSON dönüştürme
- Exception handling
- Caching
- Testing

Bunların hepsini sıfırdan yazmaya kalksan binlerce satır kod yazarsın.

Spring diyor ki:

> "Bunları ben senin yerine halledeyim."


--------

Spring Kotlin Developlerları tarafından da benimsenmiştir. 

-------

Java: Toprak

Çanakçı Spring Abi: İşe yarar çanak yapan, çanak yapmaya yardımcı olan aletler üreten bir esnaf.

-----

Bağlantı yönetimi, arayüz katmanı geliştirme, nesne yönelimli programlama, AOP ve test gibi özellikleri desteklemesi , proje geliştirirken bazı işleri arkada yaparak işimizi kolaylaştırması ve web uygulamaları, mobil uygulamalar, masaüstü uygulamaları, servisler geliştirebilmek tercih sebeplerinin en büyük paydaşları olmaktadır.

Spring’i günümüzde Google, Amazon, Facebook, Twitter, Netflix gibi şirketler tercih etmektedirler.

-----

Spring Neden Tercih Ediliyor? 

### Spring Esnektir

Nesnelerin yaşam döngüsünü yönetmek, bağımlılıkları enjekte etmek ve hizmetleri soyutlamak için bir dizi araç sağlar. Bu nedenle esnektir denilmektedir. Temel anlamda **Inversion of Control (IoC) and Dependency Injection (DI)** özellikleri sağlar.

### Spring Verimlidir

Spring Boot, mikro hizmet geliştirmeyi geliştirmek için uygulama bağlamı ve otomatik olarak yapılandırılmış, gömülü web sunucusu gibi gereksinimleri birleştirir.

### Spring Hızlıdır

Spring resmi sitesine göre, Spring projeleri, daha fazla verimlilik için reaktif (bloklamayan) programlama modelini de giderek daha fazla desteklemektedir. Geliştirici üretkenliği, Spring’in süper gücüdür. Spring mühendisleri, geliştiricilere “Spring ile varsayılan olarak hızlı başlatma, hızlı kapatma ve optimize edilmiş yürütmeyi fark edeceksiniz” olduğunu ifade ediyor.

### Spring Güvenlidir

Üçüncü taraf bağımlılıklarını yakından izler. Verilerimizi ve uygulamamızı güvenli hale getiren düzenli güncelleme yayınlanır. Spring Security çerçevesini kullanarak uygulamamızı güvenli hale getirebiliriz.

-------

#### INVERSION OF CONTROL(IoC) VE DEPENDENCY INJECTION(DI) SPRING'IN EN ÖNEMLİ OLAYIDIR.

![[Pasted image 20260628164454.png]]

------------

