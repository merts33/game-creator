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

