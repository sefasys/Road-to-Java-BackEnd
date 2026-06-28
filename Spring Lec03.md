
![[Pasted image 20260628175612.png|573]]

![[Pasted image 20260628175729.png]]


----

![[Pasted image 20260628175753.png]]

-> Burası ilk derste bahsettiğimiz o kullanacağımız dalları seçtiğimiz alan.
Dependency table.

----
web üzerinden de spring projesi start.spring.io adresinden oluşturulabilir. burada aynı yukarıdaki gibi seçeneklere basarız ve spring sitesi o initial project file'ı zip olarak oluşturup indirir. Sonrasında bunu manuel olarak extract ederiz ve workspace'teki alakalı project path'e(mesela Maven folder'ına) atarız. Daha eski ve zor bir yöntem ancak halen kullanılabilir. 
Sonrasında Suite üzerinden bu folder'ı import ederiz. (pom.xml'i import et.)

---

# EN TEMEL DÜZEYDE BİR SPRİNG PROJESİNİN DOSYA YAPISI

![[Pasted image 20260628180547.png]]

### pom.xml nedir? 

Projeyle alakalı meta bilgilerin tutulduğu yer diyebiliriz.


![[Pasted image 20260628185512.png]]
yukarıdaki 4.1.0 kullandığımız spring'in versiyonu iken
alttaki 0.0.1-SNAPSHOT yazan kendi geliştirdiğimiz projenin sürümüdür. YAni mesela büyük bir güncelleme yaptık. 0.0.1'i, 1.0.0-beta şeklinde güncelleyebiliriz.

name: projenin ismi
description: projenin açıklaması

![[Pasted image 20260628185752.png]]

burada java versiyonunu görebilriz.

![[Pasted image 20260628185904.png]]

En önemli kısım ise bu yukarıdaki kısım gördüğğümüz üzere ilk başta seçtiğimiz o dependency'ler `<dependencies>` isimli label'ın içinde ve seçtiğimiz tüm bu bağımlılıklar ayrıca `<dependency>` olarak tekli şekilde tutulmakta. 

buraya yeni bir dependency eklediğimiz zaman folder'daki directory olan Maven dependencies güncellenir ve içerisine eklediğimiz o dependency ile alakalı dosyalar yüklenir.

VE yeni bir dependency vs. eklememiz gerektiğinde buradan ekliyoruz. buraya anyı bu şekilde groupId artifactId falan o gereken bilgileri girip işimizi görüyoruz. 

Bu dependencyler "Maven repository" sitesinden ulaşılabilir. Yani bunları bir kütüphane gibi düşünebiliriz aslında. Maven Repository sitesinden istediğimiz aratıp onunla alakalı bilgileri pom.xml dosyasına ekledikten sonra kaydettikten sonra sistem onu indirecektir. Ve projemiz için gerekli olan dependency'leri bu şekilde elde etmiş oluruz.

-----------------

Aynı şekilde JRE System Library önceki derste bahsetiğimiz Java'nın kendisinde bulunan class'lar libraryler falan yani default javanın folder'ı.

-----------


![[Pasted image 20260628191118.png]]



application.properties içerisinde spring aplikasyonunu database ile buradan bağlıyoruz. 

-----------

![[Pasted image 20260628191318.png]]

ve src/main/java içerisinde de application ile alakalı kodlarımızın bulunduğu ana kısım burası.

burada sağ tık yapıp run as'den spring boot'u seçince kendisi


----



![[Pasted image 20260628192957.png]]

port8080 üzerinde tomcat kullanarak sistemi ayağa kaldırdı localhostta bu şekilde erişilebilir bir şekilde sistemi çalıştırdık.

tomcat üzerinden bu şekilde kurmuş olduk.

-----

tüm ayarlar window > preferences kısmında.
burada her türlü ayarı bulabiliriz.

java build'ini değiştirmek vs. istediğimizde de yine aynı şekilde preferences kısmına gelmemiz gerekiyor.

----

Tam olarak bu Spring framework'ün yapısını iyice anlamak için direkt folder'ları üzerinde biraz dolaşarak zaman geçirmek gerekiyor.


----

