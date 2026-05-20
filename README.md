# Basit Nesne Yonelimli Oyun Motoru Altyapisi

Bu proje, bilgisayar muhendisligi tasarim oruntuleri dersi kapsaminda gelistirilmis, temiz kod (clean code) prensiplerine ve esneklik kurallarina dayanan fiktif bir oyun motoru core yapisidir. Projenin temel amaci, bir oyunda karsimiza cikabilecek karakter yonetimi, ses entegrasyonu ve basari takip sistemlerini en az bagimlilikla bir araya getirmektir.

Gelistirme sureci adim adim (3 farkli fazda) ilerletilmis ve her asamada sisteme yeni kabiliyetler kazandirilmistir.

---

## 🛠️ Projede Kullanilan Tasarim Oruntuleri (Design Patterns)

### Faz 1: Creational (Yaratisal) Yapilar
* **Factory Method Pattern:** Oyundaki `Oyuncu` ve `Dusman` gibi temel nesnelerin doğrudan ana kod (`Main`) icinde `new` anahtar kelimesiyle kontrolsuzce uretilmesini engeller. Nesne uretim yetkisini tek bir fabrikaya (`OyunNesnesiFabrikasi`) devrederek sistemi daha derli toplu hale getirir.

### Faz 2: Structural (Yapisal) Yapilar
* **Decorator Pattern:** Mevcut karakter kodlarina dokunmadan, oyun sirasinda bir karaktere dinamik olarak `Kalkan` ozelligi eklememizi saglar. Boylece sadece kalkanli bir oyuncu icin yeni bir alt sinif olusturup sistemi sismekten kurtarir.
* **Adapter Pattern:** Projeye dahil etmek istedigimiz ama metot isimleri bizim ana yapimiza uymayan dis kütüphaneleri (ornegin hazir bir ses motorunu) sisteme sorunsuzca entegre etmek icin bir kopru gorevi gorur.

### Faz 3: Behavioral (Davranissal) Yapilar
* **Observer Pattern:** Oyundaki karakterlerin yaptigi hamleleri (hareket etme, saldirma vb.) arka planda calisan `BasariTakipcisi` sistemine gevsek bagli (loose coupling) sekilde bildirir. Karakter kendi isine bakar, takip sistemi otomatik beslenir.
* **Strategy Pattern (OCP Ispati):** Karakterlerin saldiri turlerini (Normal, Kritik, Sihirli) calisma aninda degistirebilmemizi saglar. **Acik/Kapali Prensibine (OCP)** tam uyum saglar; cunku yarini bir gun oyuna "Zehirli Saldiri" gibi yeni bir yetenek eklemek istedigimizde eski kodlarin hicbirine dokunmadan sadece yeni bir strateji sinifi yazmamiz yeterlidir.

---

## 📊 Mimari Sinif Diyagrami (UML)

Projenin son halindeki tum siniflarin birbiriyle olan baglantisini gosteren dökümantasyon semasi:

```mermaid
classDiagram
    class Gozlemci { <<interface>> +haberVer(String mesaj)* }
    class BasariTakipcisi { +haberVer(String mesaj) }
    Gozlemci <|.. BasariTakipcisi

    class Saldiristratejisi { <<interface>> +hasarHesapla(int guc)* }
    class NormalSaldiri { +hasarHesapla() }
    class KritikSaldiri { +hasarHesapla() }
    class SihirliSaldiri { +hasarHesapla() }
    Saldiristratejisi <|.. NormalSaldiri
    Saldiristratejisi <|.. KritikSaldiri
    Saldiristratejisi <|.. SihirliSaldiri

    class OyunNesnesi {
        <<abstract>>
        +List~Gozlemci~ gozlemciler
        +Saldiristratejisi saldiriStratejisi
        +gozlemciEkle()
        +stratejiDegistir()
    }
    OyunNesnesi o-- Gozlemci
    OyunNesnesi o-- Saldiristratejisi

    class Oyuncu { }
    class Dusman { }
    OyunNesnesi <|-- Oyuncu
    OyunNesnesi <|-- Dusman
    
    class NesneDecorator { }
    class KalkanDecorator { }
    OyunNesnesi <|-- NesneDecorator
    NesneDecorator <|-- KalkanDecorator

    class OyunNesnesiFabrikasi { +nesneUret() }