
## Tomcat Nedir? 

> **Tomcat, Java uygulamalarını internette çalıştıran bir web sunucusu (daha doğrusu bir Servlet Container/Web Server)dır.**

Şimdi bunu adım adım açalım.

---

# Önce bir web sitesi nasıl çalışıyor?

Sen tarayıcıya yazıyorsun:

```
http://localhost:8080/hello
```

Tarayıcı bir HTTP isteği gönderiyor.

```
Chrome    │    │ GET /hello    ▼?????????    ▼Java kodun
```

Buradaki soru işareti ne?

İşte **Tomcat**.

---

## Tomcat'in görevi

Tomcat sürekli bilgisayarında bekler.

```
Chrome    │GET /hello    │    ▼Tomcat    │    ▼Spring    │    ▼Controller
```

Yani Tomcat der ki:

> "8080 portuna biri istek attı."

Sonra Spring'e söyler.

Spring de uygun metodu çalıştırır.

---

## Mesela şu kodu yazdın

```
@RestControllerpublic class HelloController {    @GetMapping("/hello")    public String hello() {        return "Merhaba";    }}
```

Sen hiçbir yerde

```
ServerSocket socket = ...
```

yazmıyorsun.

Ya da

```
while(true){    accept();}
```

yazmıyorsun.

Bunların hepsini Tomcat yapıyor.

-------

Farklı farklı aynı görevi yapan servletler var.

window > show view > servers kısmından farklı serverlar seçilebilir.

-------
