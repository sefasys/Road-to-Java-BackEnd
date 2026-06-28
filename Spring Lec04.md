# Maven Nedir? 

==Maven gibi bağımlılık(dependency) yönetim araçları yokken geliştiriciler projelerine başka bir framework ya da kütüphaneyi eklemek istediğinde o projenin sitesinden JAR dosyalarını indirip kendi projelerine ekliyorlardı.== Projeye Hibernate eklemek istiyorsanız Hibernate sitesine girip JAR dosyalarını indirip projeye ekliyordunuz. Ancak projede kullanacağınız dependency sayısı artınca bu durum içinden çıkılmaz bir hal alıyor. Her eklenecek bağımlılık için JAR dosyalarını elle eklemek geliştirici için büyük yük. İşte Maven burada devreye giriyor. Maven tüm bunları sizin için yapıyor. Üstelik bu eklediğiniz bağımlılıkların bağımlı olduğu başka dependencyler varsa onları da sizin için projenize dahil ediyor.

Yani gerçekten Spring'de Maven sayesinde ateş tekrardan keşfedilmiş. Öyle korkunç bir durum.

Yani anlayacağımız üzere dependency management tool diyebiliriz maven için açıklaması çok da zor bir şey değil. Dependency ile alakalı birçok durumu kendisi otomatik şekilde düzenler. Kurulum için vs. işleri fazlasıyla kolaylaştırır.

Tuğrul Bayrak Medium sayfasında daha detaylı bilgi edinilebilir.




