## 1.fabrika yöntemi;
* neredekullanıldı:nesneUret()kısmında kullanıldı nesneler direkt üretilmek yerine bu metodu çagırıyor
* neden secildi:oyun projesindeki karakter ve nesne yaratma sorumlulugunu tek merkeze tasıdı maksat somut alt sınıflara dogrudan bagımlı olmasını engellemek istendi
* ne kazandım:nesne olusturma mantığı tek yerde toplandı boylece yeni nesne eklendiğinde diğer kodlarıda değiştirmek gerekmeyecek 
önce ;
classDiagram
    class Main {
        +main(args)
    }
    class OyunNesnesi {
        +String nesneAdi
        +String tip (OYUNCU/DUSMAN)
        +int canDegeri
        +hareketEt()
        +saldir()
    }
    Main ..> OyunNesnesi : "doğrudan new ile üretir (Sıkı Bağ)"
    sonra;
    classDiagram
    class OyunNesnesi {
        <<abstract>>
        +String nesneAdi
        +int canDegeri
        +hareketEt()*
        +saldir()*
    }
    class Oyuncu {
        +hareketEt()
        +saldir()
    }
    class Dusman {
        +hareketEt()
        +saldir()
    }
    class OyunNesnesiFabrikasi {
        +nesneUret(tip) OyunNesnesi
    }
    class Main {
        +main(args)
    }

    OyunNesnesi <|-- Oyuncu : "Turer"
    OyunNesnesi <|-- Dusman : "Turer"
    OyunNesnesiFabrikasi ..> OyunNesnesi : "Üretir"
    Main ..> OyunNesnesiFabrikasi : "Sadece fabrikayı çağırır"


    FAZ 2:

    ## Faz 2: Structural (Yapısal) Örüntüler

### 1. Decorator (Süsleyici) Örüntüsü
* **Nerede Kullanıldı:** `NesneDecorator` soyut sınıfı ve ondan türeyen `KalkanDecorator` sınıfı aracılığıyla `OyunNesnesi` mimarisi üzerinde uygulandı. `Main` sınıfı içerisinde dinamik olarak bir oyuncu nesnesi kalkanla süslendi.
* **Neden Seçildi:** Oyundaki karakterlere (Oyuncu, Düşman vb.) kodlarını değiştirmeden veya alt sınıfları gereksiz yere şişirmeden (Subclass Explosion), oyun esnasında çalışma anında (Runtime) dinamik olarak kalkan, ekstra hasar veya hız gibi yeni yetenekler ekleyebilmek için seçildi.
* **Ne Kazandınım:** Esneklik kazanıldı. Sisteme yeni bir zırh, kalkan veya kutsama efekti eklemek istendiğinde mevcut karakter kodlarına dokunmadan sadece yeni bir süsleyici (Decorator) sınıfı yazarak Açık/Kapalı Prensibine (OCP) tam uyum sağlandı.

### 2. Adapter (Adaptör) Örüntüsü
* **Nerede Kullanıldı:** Sistemimize doğrudan entegre olamayan fiktif `DisSesEfektMotoru` sınıfı ile bizim sistemimizin beklediği `SesSistemi` arayüzü (Interface) arasında `SesAdaptoru` sınıfı yazılarak uygulandı.
* **Neden Seçildi:** Projemize dışarıdan dahil edilen, kodlarına müdahale edemediğimiz hazır bir 3. parti ses kütüphanesinin metot imzalarını (`sesCal`), kendi sistemimizin beklediği metot yapısına (`efektOynat`) mevcut sistemi kırmadan uydurabilmek için seçildi.
* **Ne Kazandınım:** Sistemin dış kütüphanelere olan bağımlılığı (Tight Coupling) izole edildi. İleride harici ses kütüphanesini tamamen değiştirip başka bir kütüphaneye geçmek istersek, ana oyun kodlarında hiçbir satırı değiştirmeden sadece yeni bir Adaptör sınıfı yazarak kod kalitesini koruma avantajı kazandık.

---

### 📊 Faz 2 Güncellenmiş Mimari Sınıf Diyagramı (UML)

Mevcut yaratımsal fabrika yapısı korunarak, sisteme eklenen yeni Decorator ve Adapter mimari ilişkileri aşağıdaki şemada güncellenmiştir:

```mermaid
classDiagram
    %% FAZ 1'DEN GELEN MEVCUT YAPI
    class OyunNesnesi {
        <<abstract>>
        +String nesneAdi
        +int canDegeri
        +hareketEt()*
        +saldir()*
    }
    class Oyuncu {
        +hareketEt()
        +saldir()
    }
    class Dusman {
        +hareketEt()
        +saldir()
    }
    class OyunNesnesiFabrikasi {
        +nesneUret(tip) OyunNesnesi
    }
    
    OyunNesnesi <|-- Oyuncu : "Turer"
    OyunNesnesi <|-- Dusman : "Turer"
    OyunNesnesiFabrikasi ..> OyunNesnesi : "Uretir"

    %% FAZ 2: DECORATOR ENTEGRASYONU
    class NesneDecorator {
        <<abstract>>
        #OyunNesnesi sarilanNesne
    }
    class KalkanDecorator {
        +int kalkanGuvenligi
    }
    
    OyunNesnesi <|-- NesneDecorator : "Miras Alır"
    NesneDecorator o-- OyunNesnesi : "Composition (Sarılan Nesne)"
    NesneDecorator <|-- KalkanDecorator : "Turer"

    %% FAZ 2: ADAPTER ENTEGRASYONU
    interface SesSistemi {
        +efektOynat(String isim)
    }
    class SesAdaptoru {
        -DisSesEfektMotoru disMotor
        +efektOynat(String isim)
    }
    class DisSesEfektMotoru {
        +sesCal(String ad, int vol)
    }
    
    SesSistemi <|.. SesAdaptoru : "Implement Eder"
    SesAdaptoru --> DisSesEfektMotoru : "Adapte Eder (Nesne Bağı)"

    %% ANA PROGRAM AKIŞI
    class Main {
        +main(args)
    }
    Main ..> OyunNesnesiFabrikasi
    Main ..> SesSistemi
    Main ..> KalkanDecorator

