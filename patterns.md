## 1.fabrika yöntemi;
* neredekullanıldı:nesneUret()kısmında kullanıldı nesneler direkt üretilmek yerine bu metodu çagırıyor
* neden secildi:oyun projesindeki karakter ve nesne yaratma sorumlulugunu tek merkeze tasıdı maksat somut alt sınıflara dogrudan bagımlı olmasını engellemek istendi
* ne kazandım:nesne olusturma mantığı tek yerde toplandı boylece yeni nesne eklendiğinde diğer kodlarıda değiştirmek gerekmeyecek 
