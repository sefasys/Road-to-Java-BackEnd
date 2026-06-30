
# BEAN OLUŞTURMA 

![[Pasted image 20260630142548.png]]



![[Pasted image 20260630134641.png]]

görseldeki gibi ihtiyacın olduğunda context'ten çek bunu. Tekrardan yeni bir userService oluşturma. Öyle userService oluşturulduğu durumda tamamen yeni bir obje olarak oluşturmuş oluruz ve userService'in içerisi boş görünür, bazı yöntemlerle tabii ki kopyalayarak ilerlenebilir ancak bu da sistem donanımlarını aşırı ve gereksiz şekilde tüketir. 
Onun yerine bu oluşturduğumuz userService1'i Spring Context'ten çekerek tekrardan bir yeni initialization yapmadan direkt bunu kullanabiliriz. 


Bu tarz userService gibi yapılarda amacımız onu bir kez oluşturmak.
Sonrasında da "bir kenara koymak".
İşini hallet, oluştur Service yapısını sonrasında bittikten sonra bir köşeye koy
(Burada o köşe Spring Context oluyor.)
İhtiyacın olduğunda da o köşeden çağır al. Tekrar işini gör geri koy. Bu şekilde çalışıyor. 

Yani tekrar tekrar new kullanarak yeni objeler yaratmaya gerek de yok. Fazlasıyla amaçsız gereksiz bir kullanım. 

Sistem kaynaklarını bununla harcamak yerine Spring Context - Bean işlemi uygularız.

----------

Peki bu oluşturduğumuz class constructorları biz nasıl spring context'e atabiliriz.
Bunun için Stereotype Annotation dediğimiz anotasyon yöntemlerini kullanırız. 
Bean Annotation vs. çeşitli annotation yöntemleri vardır. 

----

# Ne yaparız? 

İlk olarak config class'ı oluştururuz. 
Bunun için önce new Package oluştururuz.
com.deneme.config isminde bir package oluşturup sonrasında içerisinde AppConfig class'ı oluştururuz.

bu package'lara ayırmanın mantığını da hani hepimiz kıyafetlerimizi, çoraplarımızı, gömleklerimizi ayrı ayrı yerlerde tutarız ya. Tam anlamıyla budur. Yarattığımız classları işlevlerine göre kullandığımız yerlere göre ayrı ayrı packagelarda tutarız ki hani karışmasın, neyin nerde olduğunu bilelim, tertibat olsun. 

Tek package'da da yazmak mümkün ama güzel değil, hoş değil, karışık.

-----

Package'ı oluşturduktan sonra içerisinde AppConfig isminde bir dosya oluşturalım.
Sonrasında bu class'ın üzerine @Configuration yazarak biz bu class'ı Spring Context'e almış oluruz. tabi bu @Configuration gibi annotationlar biraz daha ezber gibidir. Yani bu böyledir. 

Farklı olarak Eclipse IDE'sinde kodu düzgün şekilde formatlamak için:

Ctrl + Shift + F

Sonrasında bu class'ın içerisinde yarattığımız metodu da @Bean annotasyonu ile Spring Context'e eklemiş oluruz. 
Ayrıca Return Type olarak da void şu bu değil direkt metodun içerisinde yarattığımız objenin return edilmesi istenir:

![[Pasted image 20260630144224.png]]

tam olarak böyle!

Biz artık programı runladığımızda artık Spring Context kısmında oluşturulur ve sonrasında siz sadece onu call'larsınız ama tekrardan object generate etmeyiz.

Ctrl + Shift + O ile kullanılmayan paketleri sileriz.

![[Pasted image 20260630144937.png]]

class içinde configuration olarak annotate ettiğimiz classları bu şekilde main'den çağırırız.

![[Pasted image 20260630150317.png]]

ApplicationContext, AnnotationConfigApplicationContext classının en yukarıdaki parent class'ıdır o yüzde bu şekilde kullanmamızda bir problem yok. 

Sonrasında UserService userListService kısmı ise aslında tam anlamıyla oluşturduğumuz spring context'teki UserService'i main'e çektiğimiz kod kısmı oluyor. 
Sonrasında yaptığımız diğer kısım ise Liste olarak tuttuğumuz userList değerlerinin print edilmesi üzerine.


![[Pasted image 20260630151311.png]]


Tam olarak burada yapılan işlem bu aslında. İki farklı yerlerden userService'i çağırmamıza rağmen her ikisi de aynı yere gitti Spring Context'e gitti ve oradan login kısmını getirdi. 

-----

Çoğu zaman bu tarz manuel annotationlar yapmak yerine RestController, Repository, Service gibi annotatonlar gibi kullanarak otomatik şekilde gerçekleştiririz.

---
