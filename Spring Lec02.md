Üç tane kurmamız gereken app var:

1 - STS (Spring Tool Suit)
2 - Maven
3 - Java Dev Kit 17 ve daha yeni sürümleri.

-----

![[Pasted image 20260628165222.png]]

Java Development Kit iki alt ana yapıdan oluşuyor:
 - Java Runtime Environment
 - Java Virtual Machine

Java Runtime Environment, Java programlama dili ile alakalı onu oluşturan yapıyı, entegre kütüphaneleri(default olarak import ettiğimiz) ve yani tamamen source kodun getirdiği yetenekleri kapsayan sistem.

Java Virtual Machine ise, Java dilinin compiler'ıdır. Java platform independent'tır bu yapı sayesinde çünkü program aslında bir virtual machine üzerinde çalıştırılır, C-C++ gibi değildir. 

----

# Fedora Linux'ta Maven Nasıl Kurulur?

1 - JDK'nin kurulu olduğundan emin ol.
	`java -version`
2 - Terminalden kur
	`sudo dnf install maven`
3 - version control
	`mvn -version`
4 - Normalde Windows'ta sistem değişkeni olarak tanıtmak gerekirdi, Linux'ta veya genel olarak terminal üzerinde kurulum yapıldığında bu tarz işlemlere gerek yok. 

------------------

Ders kapsamında STS'yi Eclipse IDE üzerinde kullanacağız.

# Fedora Linux'ta STS Nasıl Kurulur?

İlk olarak Eclipse IDE'yi kuruyoruz: 

## Yöntem 1 (Önerdiğim): Eclipse Installer

1. Eclipse'in indirme sayfasına git:

**[https://www.eclipse.org/downloads/](https://www.eclipse.org/downloads/)**

2. Linux x86_64 için **Eclipse Installer** (`.tar.gz`) dosyasını indir.
3. İndirdiğin klasöre git:

```
cd ~/Downloads
```

4. Arşivi aç:

```
tar -xzf eclipse-inst-*.tar.gz
```

5. Installer'ı çalıştır:

```
./eclipse-inst/eclipse-inst
```

6. Açılan pencereden **Eclipse IDE for Enterprise Java and Web Developers** seç.

Bu paket Spring ve Java backend geliştirme için daha uygundur.

---

Sonrasında STS'yi kuruyoruz:

Aynı şekilde STS'nin sitesinden uygun tar.gz dosyasını indirip extract ediyoruz. Sonrasında o dosya üzerinden kullanılabilir oluyor.

fedora'ya özel şekilde desktop için birkaç komut gerekiyor. O kısmı da yaptıktan sonra direkt desktop'tan da executable şekilde kullanılabilir. 

----


