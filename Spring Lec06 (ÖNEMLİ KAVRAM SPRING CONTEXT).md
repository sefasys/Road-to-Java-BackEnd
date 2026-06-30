
Bu konu Spring'in en önemli konularından biri.
# Spring Context & Bean Kavramı

![[Pasted image 20260630092920.png]]

Spring Context - IoC Context - Application Context
üç tanım da bu context'e uyuyor.
Bean dediğimiz aslında nesne olarak ürettiğimiz Java classları.

----------

Bu konu Spring'in kalbi diyebilirim. Eğer **Spring Context** ve **Bean** kavramlarını gerçekten anlarsan, Spring'in %70'lik mantığı oturur. Birçok kişi bu kavramları ezberliyor ama neden var olduklarını anlamıyor. Biz biraz daha mühendis gözüyle bakalım.

---

# Önce problemi anlayalım

Spring olmasaydı şöyle bir kod yazardın:

```java
public class UserController {

    private UserService userService = new UserService();

}
```

`UserService` de:

```java
public class UserService {

    private UserRepository repository = new UserRepository();

}
```

Sonra `UserRepository`:

```java
private DatabaseConnection db = new DatabaseConnection();
```

Yani herkes kendi bağımlılığını (`dependency`) kendisi oluşturuyor.

Şöyle bir zincir oluşuyor:

```
UserController
      │
      ▼
new UserService()
      │
      ▼
new UserRepository()
      │
      ▼
new DatabaseConnection()
```

Başta masum görünüyor.

---

## Peki sorun ne?

Diyelim ki sistemde 40 tane Controller var.

Hepsi

```java
new UserService()
```

diyor.

Artık bellekte kaç tane `UserService` oluştu?

```
Controller1 ---> UserService
Controller2 ---> UserService
Controller3 ---> UserService
...
Controller40 ---> UserService
```

40 tane.

Halbuki çoğu zaman **bir tane UserService yeterlidir.**

---

Başka bir problem.

Yarın

```java
MySQLRepository
```

yerine

```java
PostgreRepository
```

kullanmak istiyorsun.

Şimdi bütün kodlarda

```java
new MySQLRepository()
```

arayıp değiştireceksin.

Kod tamamen birbirine bağımlı hale geliyor.

Buna **tight coupling** denir.

---

# Spring diyor ki

> "Nesneleri sen oluşturma."

Ben oluşturacağım.

Sen sadece kullan.

İşte burada Spring Context devreye giriyor.

---

# Spring Context nedir?

En basit tanımı:

> **Spring Context, Spring'in yönettiği bütün nesnelerin bulunduğu konteynerdir.**

Ben bunu şöyle hayal ediyorum.

```
+--------------------------------------+
|          Spring Context              |
|                                      |
|  UserService                         |
|  UserRepository                      |
|  DatabaseConnection                  |
|  MailService                         |
|  JwtService                          |
|  OrderService                        |
|                                      |
+--------------------------------------+
```

Bütün önemli nesneler burada tutuluyor.

---

# Bean nedir?

İşte burada en önemli tanım geliyor.

> **Bean = Spring Context içinde bulunan Java nesnesidir.**

Yani

```
UserService
```

tek başına bir Java objesi olabilir.

Ama

```
Spring Context

↓

UserService
```

olursa artık o bir **Bean** olur.

Bean demek

> Spring tarafından oluşturulan, yönetilen ve yaşam döngüsü kontrol edilen nesne.

---

# Kim oluşturuyor?

Sen artık

```java
new UserService();
```

demiyorsun.

Spring uygulama başlarken diyor ki

```
Tamam.

UserService lazım.

Ben oluşturuyorum.

Repository lazım.

Ben oluşturuyorum.

Controller lazım.

Ben oluşturuyorum.
```

Sonra hepsini Context içine koyuyor.

```
Application Start

↓

Spring Context

↓

Bean oluştur

↓

Bean oluştur

↓

Bean oluştur

↓

Hazır.
```

---

# Sonra ne oluyor?

Mesela

```java
@RestController
public class UserController {

    private UserService service;

}
```

Spring bakıyor.

```
Context

↓

UserService Bean var mı?

↓

Var.

↓

Al bunu Controller'a ver.
```

Sen

```java
new UserService()
```

demiyorsun.

Spring veriyor.

---

# Buna ne deniyor?

Dependency Injection.

Yani

```
Ben oluşturmayacağım.

Birisi bana verecek.
```

Bu "birisi"

Spring Context.

---

# Bir benzetme

Bir okul düşün.

Müdür odası:

```
+---------------------+
|     Müdür           |
|                     |
|  Öğretmenler        |
|  Güvenlik           |
|  Sekreter           |
|  Temizlik           |
+---------------------+
```

Yeni öğretmen geldi.

Öğrenci gidip

```
new Teacher()
```

demiyor.

Müdüre diyor.

```
Bana Matematik öğretmeni lazım.
```

Müdür

```
Al.

Bu öğretmen.
```

İşte

Müdür = Spring Context

Öğretmen = Bean

---

# Peki Bean nasıl oluşturuluyor?

En yaygın yöntem

```java
@Service
public class UserService {

}
```

Spring bunu görüyor.

Diyor ki

```
@Service

↓

Bean oluştur.
```

Aynısı

```java
@Repository
```

```java
@Controller
```

```java
@RestController
```

```java
@Component
```

Hepsi aslında Bean oluşturuyor.

---

# Bunların ortak noktası

Hepsi aslında bunun özel hali.

```java
@Component
```

Yani

```
@Component

↓

Bean
```

```
@Service

↓

@Component

↓

Bean
```

```
@Repository

↓

@Component

↓

Bean
```

```
@RestController

↓

@Controller

↓

@Component

↓

Bean
```

Yani sonunda hepsi Bean oluyor.

---

# Context neden önemli?

Çünkü Context sayesinde

## Singleton oluyor.

Mesela

```
UserService
```

bir kere oluşturuluyor.

Sonra

```
Controller A

↓

aynı UserService
```

```
Controller B

↓

aynı UserService
```

```
Controller C

↓

aynı UserService
```

Hepsi aynı objeyi kullanıyor.

Bellek tasarrufu.

---

## Merkezi yönetim

Mesela uygulama kapanıyor.

Spring diyor

```
Tamam.

Bütün Bean'leri kapatıyorum.
```

Database bağlantıları kapanıyor.

Threadler duruyor.

Resource sızıntısı olmuyor.

---

## Bağımlılıkları otomatik çözüyor

Mesela

```
OrderService
```

şuna ihtiyaç duyuyor.

```
PaymentService
```

O da

```
NotificationService
```

istiyor.

Spring zinciri otomatik kuruyor.

---

# Bean'in yaşam döngüsü

Bean'in de bir ömrü var.

```
Spring başlar

↓

Bean oluşturulur

↓

Constructor

↓

Dependency Injection

↓

@PostConstruct

↓

Kullanılır

↓

Uygulama kapanır

↓

@PreDestroy

↓

Yok edilir
```

Bunu ileride ayrıntılı göreceksin.

---

# En önemli püf noktaları

✅ **Bean = Spring'in yönettiği Java nesnesidir.** Her Java nesnesi Bean değildir.

✅ **Spring Context = Bean'lerin saklandığı ve yönetildiği konteynerdir.**

✅ **Context, uygulama başlarken oluşturulur.** Varsayılan olarak Bean'ler de bu sırada yaratılır (eager initialization).

✅ **Varsayılan Bean kapsamı (scope) `singleton`'dır.** Yani her Bean'den genellikle uygulama boyunca tek bir örnek bulunur.

✅ **`@Component`, `@Service`, `@Repository` ve `@Controller` gibi anotasyonlar, sınıfın Spring tarafından taranıp Bean olarak kaydedilmesini sağlar.**

---

# Konunun özeti

Bunu aklında şöyle tutabilirsin:

```
Java Object
        │
        ▼
Spring yönetmiyor
        │
        ▼
Normal Object


Java Object
        │
@Component
        │
        ▼
Spring oluşturuyor
        │
        ▼
Bean
        │
        ▼
Spring Context içinde saklanıyor
```

Ve bu iki cümle Spring'in özünü anlatır:

> **Bean**, Spring'in yönettiği nesnedir.

> **Spring Context**, bu Bean'leri oluşturan, saklayan, birbirine bağlayan ve yaşam döngülerini yöneten konteynerdir.

Bu temel oturduğunda, sıradaki büyük konu olan **IoC (Inversion of Control)** ve **Dependency Injection** çok daha doğal gelecektir; aslında Bean ve Spring Context, bu iki kavramın pratikte nasıl çalıştığını sağlayan mekanizmadır.



==biraz karmaşık geldi. aslında mantığını temelde kavradım. anlamadığım nokta daha çok Spring bunu nasıl kontrol ediyor? Yani ben mesela iki farklı Car objesi üretmek istiyorum. Bu mümkün değil mi yani? Hani iki Car nesnesi farklı özelliklere sahip olabilir. Bunu nasıl yönetiyor ki Spring?== 

İşte tam bu noktada çoğu kişinin kafası karışıyor. Aslında sen doğru soruyu sordun. Çünkü burada önemli olan şey şu:

> **Spring "bütün objeleri" yönetmez. Sadece senin yönetmesini istediğin objeleri yönetir.**

Bunu bir örnekle açıklayayım.

---

## Normal Java

```java
Car car1 = new Car("BMW");
Car car2 = new Car("Mercedes");
```

Burada tamamen sen kontrol ediyorsun.

Bellekte gerçekten iki farklı nesne oluşuyor.

```
car1 -----> Car(BMW)

car2 -----> Car(Mercedes)
```

Spring buna karışmaz.

---

## Peki Spring neyi yönetiyor?

Mesela şunu yazdın:

```java
@Service
public class CarService {

}
```

Artık diyorsun ki:

> "Bu sınıfı ben yönetmeyeceğim. Spring yönetsin."

Spring uygulama açılırken bunu görüyor.

```
@Service

↓

Bean oluştur

↓

Context'e koy
```

Artık bu nesneyi Spring oluşturuyor.

---

## Neden Car örneği biraz yanıltıcı?

Çünkü **Car genellikle veri (data) temsil eder.**

Mesela:

```java
Car bmw = new Car(...);

Car audi = new Car(...);

Car mercedes = new Car(...);
```

Bir uygulamada yüz binlerce araba olabilir.

Spring bunların hepsini Bean yapmaz.

Mantıklı da olmaz.

---

Spring daha çok şunları yönetir:

```
UserService

OrderService

PaymentService

MailService

UserRepository
```

Bunlar uygulamanın **servisleri**.

Bir tane olması yeterlidir.

---

## Car neden Bean olmaz?

Şunu düşün.

Bir araba satış sitesi yazıyorsun.

Database'de

```
BMW

Audi

Mercedes

Tesla

Toyota

...
```

500.000 araba var.

Spring bunların hepsini açılışta oluştursa:

```
500.000 Bean
```

Bilgisayar çöker. 😄

O yüzden bunlar normal Java objesidir.

Database'den okunur.

İhtiyaç olunca oluşturulur.

---

## Peki istersem Car Bean yapabilir miyim?

Evet.

```java
@Component
public class Car {

}
```

dersen olur.

Ama artık Spring der ki

```
Car Bean
```

Varsayılan olarak **tek tane** oluşturacağım.

---

Sen diyorsun ki

> Ben iki tane Car istiyorum.

Spring:

```
Hayır.

Bu Bean singleton.

Tek tane var.
```

---

## Peki iki tane Bean oluşturamaz mıyım?

Olabiliyor.

Mesela:

```java
@Configuration
public class Config {

    @Bean
    public Car bmw() {
        return new Car("BMW");
    }

    @Bean
    public Car mercedes() {
        return new Car("Mercedes");
    }

}
```

Şimdi Context'te

```
BMW Bean

Mercedes Bean
```

oldu.

İkisi de Car tipinde.

Ama isimleri farklı.

---

Hatta istersem şöyle alabilirim.

```java
@Autowired

@Qualifier("bmw")
Car car;
```

veya

```java
@Autowired

@Qualifier("mercedes")
Car car;
```

---

## Daha da ilginci

Bean'lerin **scope**'u vardır.

Varsayılan

```
Singleton
```

Yani

```
1 Bean
```

Ama istersem

```java
@Scope("prototype")
```

derim.

```java
@Component
@Scope("prototype")
public class Car {

}
```

Bu sefer Spring der ki

> "Her istediğinde yeni bir Car oluşturacağım."

Yani artık

```
Car1

Car2

Car3

Car4
```

oluşabilir.

---

# Peki Spring bunu nasıl kontrol ediyor?

İşte en güzel soru bu.

Spring'in içinde aslında devasa bir harita (map) var gibi düşünebilirsin.

Kabaca şöyle:

```java
Map<String, Object> context;
```

Mesela

```
"userService"
        ↓
UserService nesnesi

"orderService"
        ↓
OrderService nesnesi

"paymentService"
        ↓
PaymentService nesnesi
```

Bir yerde

```java
@Autowired

UserService service;
```

görünce Spring diyor ki

```
Map'e bak.

UserService var mı?

↓

Var.

↓

Al bunu ver.
```

Yani Spring sihir yapmıyor.

Arka planda Bean'leri oluşturuyor, saklıyor ve gerektiğinde doğru olanı sana veriyor.

---

## Benim sana ilk haftada ezberlemeni istediğim tek cümle şu olurdu:

> **Spring bütün nesneleri yönetmez. Sadece uygulamanın altyapısını oluşturan, tekrar tekrar kullanılacak nesneleri (servisler, repository'ler, controller'lar vb.) yönetir.**

`Car`, `User`, `Order`, `Product` gibi **veri nesneleri** ise çoğunlukla normal Java objeleridir; onları gerektiğinde sen veya kullandığın kütüphaneler oluşturur.

Bu ayrımı yaptığında Spring Context ve Bean mantığı bir anda çok daha doğal gelmeye başlar.

-------


==bilsen iyi olur dediğin başka bir şey var mı bu konuyla alakalı?==



Evet. Hatta bence Spring'in ilk haftasında ezberlemen gereken **5 kritik nokta** var. Bunları anlarsan ileride `@Autowired`, `@Service`, `@Component`, `@Bean` gibi kavramlar çok daha anlamlı gelir.

---

# 1. Bean = Singleton değildir.

Çoğu kişi bunu sanıyor.

Doğrusu şu:

> **Bean, Spring tarafından yönetilen nesnedir.**

Varsayılan olarak (`default scope`) **singleton** olur.

Yani

```
Bean
    ↓
(default)
Singleton
```

ama istersem

```java
@Scope("prototype")
```

diyebilirim.

O zaman her seferinde yeni nesne üretir.

Yani

```
Bean ≠ Singleton

Bean → yönetilen nesne

Singleton → oluşturulma şekli
```

Bu ayrım önemli.

---

# 2. Bean oluşturmanın iki yolu var.

### Birinci yol

Annotation

```java
@Service
```

```java
@Repository
```

```java
@Component
```

```java
@Controller
```

Spring bunları tarar.

---

İkinci yol

```java
@Bean
```

```java
@Configuration
public class Config {

    @Bean
    public Car car() {
        return new Car();
    }

}
```

Bu yöntem özellikle **üçüncü parti (third-party) kütüphaneler** için kullanılır.

Mesela sınıf sana ait değildir.

Sen gidip

```java
@Component
```

ekleyemezsin.

O zaman

```java
@Bean
```

ile oluşturursun.

Bu ileride çok karşına çıkacak.

---

# 3. Spring sadece Context içindekileri bilir.

Mesela

```java
UserService
```

Bean.

Ama

```java
new UserService();
```

yaptın.

Artık iki tane oluştu.

```
Spring Context

↓

UserService (Bean)



Sen

↓

new UserService()
```

İkisi farklı objedir.

Spring senin oluşturduğunu bilmez.

O yüzden Spring projelerinde mümkün olduğunca **`new` kullanmaktan kaçınırsın.**

---

# 4. Constructor Injection neden öneriliyor?

Eskiden insanlar

```java
@Autowired
private UserService service;
```

yazıyordu.

Şimdi önerilen yöntem

```java
private final UserService service;

public UserController(UserService service){
    this.service = service;
}
```

Spring bunu otomatik dolduruyor.

Bunun avantajları:

- immutable (`final`)
    
- test etmek kolay
    
- null olamaz
    
- bağımlılıklar açıkça görünür
    

Bugün Spring dünyasında en yaygın yaklaşım bu.

---

# 5. Spring aslında bir fabrika gibi çalışıyor.

Bu bence en güzel benzetme.

Düşün ki dev bir fabrika var.

```
Spring Factory

↓

UserService

↓

MailService

↓

Repository

↓

Controller

↓

Logger
```

Sen

```
Bana UserService lazım.
```

diyorsun.

Spring

```
Al.
```

diyor.

Sen

```
new UserService()
```

demiyorsun.

Bu yüzden Spring'e bazen

> **Object Factory**

gibi de bakabilirsin.

---

# Bir de çok önemli bir ayrım

Bunu birçok kaynak ilk başta söylemiyor.

Spring'de iki çeşit sınıf var gibi düşün.

### 1) Infrastructure Objects

Bunlar Bean olur.

```
Controller

Service

Repository

Configuration

Security

MailService

JwtService
```

Bunlar uygulamanın altyapısını oluşturur.

---

### 2) Domain Objects

```
User

Car

Order

Product

Invoice
```

Bunlar genellikle Bean değildir.

Çünkü bunlar veri taşır.

Her kullanıcı için yeni User oluşur.

Her sipariş için yeni Order oluşur.

Her araba için yeni Car oluşur.

Bu ayrımı yaparsan Spring çok daha mantıklı geliyor.

---

# Son olarak...

Sana bir "aha!" yaşatacak bilgi vereyim.

Spring'in en büyük amacı aslında **Bean oluşturmak değildir.**

Asıl amacı şudur:

> **Sınıfların birbirine olan bağımlılığını azaltmak (Loose Coupling).**

Mesela şu kod:

```java
UserService service = new UserService();
```

`UserController`, `UserService`'e sıkı sıkıya bağlıdır (**tight coupling**).

Ama Spring'de:

```java
public UserController(UserService service) {
    this.service = service;
}
```

`UserController` artık "Ben `UserService`'i nasıl oluşturacağımı bilmiyorum. Bana sadece çalışacak bir `UserService` ver." der.

İşte Spring'in en büyük felsefesi budur. Bean, Context, IoC, Dependency Injection... Bunların hepsi aslında bu hedefe hizmet eder.

---

Bence bundan sonraki adım olarak **IoC (Inversion of Control)** konusuna geçelim. Çünkü Context ve Bean'i anladıktan sonra "Spring neden nesneleri kendisi oluşturuyor?" sorusunun cevabı tam olarak IoC ile yerine oturuyor. Bu üç konu (Context, Bean, IoC) aslında tek bir bütünün parçalarıdır.


---------

