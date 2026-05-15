BENİM GÖRDÜKLERİM;  
ocp ihlali: kodda surekli yeni bir if else blogunun içine girmek gerekiyo bu karmaşık

srp ihlali:koddakibir birim birden fazla işlevi barındırıyor

spagetti kod yapısı:kod oldukça karmaşık

okunabilirlik:kod okunması oldukça güç ve anlamak değiştirmek zor
genişletilebilirlik:kod ilerde genişlemeye açık değil oldukca uzun uğraştırır takibi zordur
AI BULDUKLARI;
## Tespit Edilen 5 Temel Sorun
1. **Tek Sorumluluk Prensibi (SRP) İhlali:** `OyunNesnesi` sınıfı hem verileri saklıyor hem de hareket/saldırı gibi tüm mantığı yönetiyor; sorumluluğu çok fazla[cite: 24, 25].
2.**Açık/Kapalı Prensibi (OCP) İhlali:** Yeni bir nesne tipi (örn: BOSS) eklemek için mevcut tüm metotları (hareket, saldırı) değiştirmek gerekiyor[cite: 15, 24].
3.**Sıkı Bağlılık (Tight Coupling):** Tüm davranışlar `Tip` isimli enum yapısına tamamen bağımlı, bu da sistemin esnekliğini yok ediyor[cite: 15, 25].
4.**Kod Tekrarı:** Benzer hareket kontrolleri ve mantıksal sorgular farklı bloklar içinde sürekli tekrarlanıyor[cite: 24].
5. **Bakım Zorluğu:** Metot içindeki iç içe geçmiş `if-else` blokları, kodun okunmasını ve hata ayıklamasını (debug) zorlaştırıyor[cite: 24, 25].
