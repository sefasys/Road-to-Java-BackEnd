
# Lombok Nedir? 

Getter Setter gibi fonksiyonları yazmaktan kurtaran yardımcı bir araç kısaca.

Lombok, Java yazarken sürekli tekrar ettiğin **boilerplate code**'ları otomatik oluşturan bir kütüphanedir.

En basit tanımı:

> **Lombok, yazman gereken gereksiz kodları senin yerine yazan bir Java kütüphanesidir.**

---

## Problem ne?

Diyelim ki şöyle bir sınıfın var:

```java
public class User {

    private String name;
    private int age;

}
```

Bu haliyle pek işe yaramaz.

Şunları yazman gerekir:

- Getter
    
- Setter
    
- Constructor
    
- `toString()`
    
- `equals()`
    
- `hashCode()`
    

Mesela sadece getter ve setter bile:

```java
public class User {

    private String name;
    private int age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

Kodun büyük kısmı aslında hiçbir iş mantığı içermiyor.

---

# Lombok ile

Aynı şeyi şöyle yazarsın:

```java
import lombok.Getter;
import lombok.Setter;

@Getter
@Setter
public class User {

    private String name;
    private int age;

}
```

Getter ve setter'lar derleme sırasında otomatik oluşturulur.

---

## En sık kullanılan anotasyonlar

### `@Getter`

```java
@Getter
public class User {

    private String name;

}
```

↓

Spring/Java bunu görür ve derleme sırasında şunu oluşturur:

```java
public String getName() {
    return name;
}
```

---

### `@Setter`

Setter oluşturur.

---

### `@ToString`

```java
@ToString
public class User {
    ...
}
```

Otomatik olarak

```java
toString()
```

oluşturur.

---

### `@NoArgsConstructor`

```java
@NoArgsConstructor
```

↓

```java
public User() {

}
```

---

### `@AllArgsConstructor`

↓

```java
public User(String name, int age) {

}
```

---

### `@RequiredArgsConstructor`

Spring projelerinde en çok göreceğin anotasyonlardan biri.

Mesela:

```java
@Service
@RequiredArgsConstructor
public class UserService {

    private final UserRepository repository;

}
```

Lombok bunu

```java
public UserService(UserRepository repository) {
    this.repository = repository;
}
```

şeklinde otomatik oluşturur.

Bu yüzden constructor'ı elle yazmana gerek kalmaz.

---

### `@Data`

En meşhur anotasyon.

```java
@Data
public class User {

    private String name;
    private int age;

}
```

Bu tek başına şunları ekler:

- Getter
    
- Setter
    
- toString
    
- equals
    
- hashCode
    
- RequiredArgsConstructor (uygun durumlarda)
    

Yani tek satırla birçok şeyi kazanırsın.

---

# Spring'de neden çok kullanılıyor?

Mesela Entity yazıyorsun.

Lombok olmadan:

```java
public class Product {

    private Long id;
    private String name;
    private Double price;

    // 100 satır getter setter...

}
```

Lombok ile:

```java
@Data
public class Product {

    private Long id;
    private String name;
    private Double price;

}
```

Kod çok daha okunabilir oluyor.

---

# Püf noktası

Lombok **sihir yapmıyor.**

IDE'de kodu görmüyorsun ama derleme sırasında gerçekten şu metodları oluşturuyor:

```java
getName()

setName()

toString()

equals()

...
```

Yani JVM çalışırken bunlar normal Java metodlarıdır.

---

# Günümüzde dikkat edilen bir nokta

Eskiden birçok kişi her sınıfa `@Data` koyuyordu.

Artık daha seçici davranılıyor.

Örneğin bir Service sınıfında genellikle:

```java
@RequiredArgsConstructor
```

kullanılır.

Entity sınıflarında ise duruma göre:

```java
@Getter
@Setter
```

veya sadece

```java
@Getter
```

tercih edilebilir.

Bunun sebebi `@Data`'nın otomatik oluşturduğu `equals()`, `hashCode()` ve `toString()` metodlarının özellikle veri tabanı entity'lerinde bazen istenmeyen davranışlara yol açabilmesidir.

---

### Ben olsam ilk etapta şu 4 anotasyonu öğrenirdim:

- `@Getter`
    
- `@Setter`
    
- `@RequiredArgsConstructor`
    
- `@Data` (ne yaptığını bilerek)
    

Spring Boot projelerinin büyük çoğunluğunda bu dördünü sık sık göreceksin.


==peki sence bu zaten ne kadar doğru bir yaklaşım olur? yani biliyorum tabii ki fonksiyonlar öyle büyük bir yük bindirmiyor ama sürekli her sınıf için compiler'da @data kullanıp da fonksiyonlar falan oluşturmak compile esnasında yük bindirmez mi?==

Evet, teorik olarak **compile süresine küçük bir yük bindirir**, çünkü Lombok annotation processing yapar. Ama pratikte çoğu Spring projesinde bu yük **önemsiz seviyededir**.

Asıl mesele performans değil, **tasarım kalitesi**.

`@Data` her yere basılınca şunlar problem olabilir:

```java
@Data
public class User {
    private Long id;
    private List<Order> orders;
}
```

`@Data` şunları üretir:

```text
getter
setter
toString
equals
hashCode
```

Problem şu: `toString`, `equals`, `hashCode` özellikle **JPA Entity** sınıflarında ilişkiler varsa saçma sonuçlar doğurabilir. Mesela `User -> Order -> User` gibi çift yönlü ilişki varsa `toString()` sonsuz döngüye girebilir.

Benim önerim:

DTO / Request / Response sınıflarında:

```java
@Data
```

kullanılabilir.

Service sınıflarında:

```java
@RequiredArgsConstructor
```

kullan.

Entity sınıflarında daha kontrollü ol:

```java
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
```

ve gerekirse `equals/hashCode/toString`'i özellikle seç.

Yani cevap: **Compile yükünden dolayı değil, kontrolsüz metod üretiminden dolayı `@Data` her yere basmak iyi yaklaşım değil.**


--------

# LOMBOK NASIL KURULUR?

STS'ye gir.
Üst navigasyonda Help > Install New Software kısmına git. 
Sonrasında "Work with:" kısmına lombok'un sitesinden verilen link'i girelim.
`https://projectlombok.org/p2`

ondan sonraki kurulum next next diyerek ilerlediğimiz kısımlar. Kurulum tamamlandıktan sonra birkaç trust the publisher tarzı sorular çıkacak yes yes de geç sonrasında da restart et programı ve tamamdır kurulum başarılı.

----

AYRICA Lombok'u proje başında dependency olarak eklemeyi unutma. 


-----

Lombok anotasyonlarını kullanabilmek için de birkaç işlem gerekiyor src/main/java kısmında sağ tık yap. package kısmına gel.
orada isme com.projenin_ismi.entity veya com.projenin_ismi.model şeklinde yazılır.
Sonrasında o modele sağ tık yapıp new class seçip oradan da kod yazacağımız class'ı oluşturabiliriz.

![[Pasted image 20260630115642.png]]


-----

