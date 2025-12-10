🧠 Genel Mantık

Uygulama, martingale türevi adımlı oyun mantığını takip eder:

Belirli bir kasa ve plan oranı ile başlarsın.

Her kayıptan sonra adım (tur) ilerler.

Canlı Plan, ilgili döngüde her turda yatırılması gereken tutarı ve olasık kazancı hesaplar.

Kuponları kaydeder, kazanma/kaybetme durumuna göre kasayı otomatik günceller.

Her şey tek ekranda üç ana blokta toplanmıştır:

Özet ve Kasa Seçimi

Ayarlar + Dashboard + Aktif Kupon Alanı + Geçmiş

Canlı Plan Tablosu

🧩 Ekran Yapısı ve Bileşenler
1️⃣ Özet Kartı (Üstteki Mavi Kart)

Burada tüm döngü için kısa bir finansal özet görürsün:

Başlangıç → Kasanın tahmini başlangıç seviyesi (realize PnL’e göre hesaplanır).

Son Kasa → Şu anki güncel kasa tutarın.

Net Kâr → Toplam kazanç / kayıp (TL bazında).

Büyüme → Başlangıca göre yüzde değişim (%).

Net Kâr ve Büyüme alanları pozitif/negatif duruma göre yeşil/kırmızı renkle gösterilir.

2️⃣ Kasa Seçimi (KASA 1 / KASA 2 / KASA 3)

Hemen özet kartının altında 3 adet sekme vardır:

KASA 1

KASA 2

KASA 3

Her bir kasa:

Kendi ayarlarına (kasa, oran, adım sayısı, hedef),

Kendi kupon geçmişine,

Kendi canlı planına sahiptir.

Sekmeler arasında geçiş yaptığında:

Başlıktaki yazı Ayarlar (Kasa X) olarak değişir.

Girilen veriler o kasaya özel olduğundan kaybolmaz (localStorage’tan çekilir).

3️⃣ Ayarlar Bölümü

Bu bölümde mevcut kasa ve plan parametrelerini belirlersin.

Alanlar:

Kasa (TL)
Mevcut kasanı yazarsın. Kupon oynadıkça buradan düşer, kazandıkça buraya eklenir.

Plan Oranı
Martingale planında kullanacağın referans oran (ör. 2.00).
Canlı Plan buna göre adım başı tutar hesaplar.

Tur (Adım)
Döngüde kaç adım kullanacağını belirler (ör. 4 adım).

Hedef
Bu kasa için toplam kaç tutturulan kupon hedeflediğini gösterir (ör. 10).

🎛 Sıfırlama Butonları

Bu Kasayı Sıfırla
Sadece aktif kasanın:

Kupon geçmişini,

Ayarlarını
sıfırlar.

Tüm Kasaları Sıfırla (varsa)
3 kasayı da komple siler ve ilk hale döner.
Bu işlem geri alınamaz, tüm localStorage verin temizlenir.

4️⃣ Dashboard İstatistikleri

Ayarlar kartının altında “mini dashboard” bulunur:

Kalan → Hedeflenen toplam tutturma sayısından, şu ana kadar kazanılanların çıkarılmış hali.

Tutturulan → Kazanmış kupon adedi.

Kaybedilen → Kaybetmiş kupon adedi.

Ort. Oran → Kazanan kuponların ortalama oranı.

Tahmini → Ortalama oran ve plana göre, hedefe ulaşıldığında öngörülen kasa büyüklüğü.

“Tahmini” değer, istatistiksel bir projeksiyondur; sadece simüle edilmiş hesaptır.

5️⃣ Aktif Kupon Alanı

Bu alan, o an oynadığın / oynayacağın kuponu yönetmek içindir.

Gösterilen başlık:

ADIM X / N formatında (ör. ADIM 2 / 4)

Eğer adım, planlanan toplam adımı aşarsa uyarı mesajı: RİSKLİ BÖLGE

Alanlar:

Oynanan Tutar

Kupon Oranı

Buton:

Kuponu Kaydet (Bekliyor)

Tutarı kasadan düşer,

Kuponu “bekliyor” statüsüyle geçmişe ekler,

Canlı plan ve dashboard güncellenir.

NOT: Canlı Plan, aktif adım için otomatik önerilen tutarı Oynanan Tutar alanına doldurur.
Sen istersen manuel değiştirebilirsin.

6️⃣ Kupon Geçmişi

“Aktif Kupon” alanının altındaki kartta tablo şeklinde gösterilir.

Kolonlar:

Adım → O kuponun ait olduğu plan adımı.

Tutar → Kupon için yatırılan tutar.

Oran → Kupon oranı.

Olası K. → Kupon kazanırsa elde edilecek brüt kazanç.

Durum/İşlem → Kupon sonucu ve butonlar.

Sil → Kupon kaydını tamamen silme butonu (🗑).

Durum Butonları

Her satırda:

Bekliyor durumunda:

✔ → Kazandı olarak işaretle.

✖ → Kaybetti olarak işaretle.

Kazandı/Kaybetti durumunda:

↺ → Sonucu geri al, tekrar “Bekliyor” yap.

Silerken (🗑):

Kasa otomatik düzeltilir:

Bekleyen / kaybeden kuponlar → yatırılan tutar kasaya iade edilir.

Kazanan kupon → önce kazanç düşülür, ardından yatırılan tutar eklenir (yani hiç oynanmamış gibi).

7️⃣ Canlı Plan

Sağ tarafta yer alan “Canlı Plan” kartı, mevcut döngü için teoriye uygun adım adım yatırım planını gösterir.

Plan Bazı → Döngü başındaki varsayılan kasa (Cycle Base).
Son kazançtan sonraki kasa + o kazanç noktasına kadar olan kayıp/bekleyen bahislerin geri eklenmiş hali.

Hesaplanan Döngü → Planlanan toplam adım sayısı (Tur).

Tablo kolonları:

Tur → 1, 2, 3, ... N

Yatırılacak → O turda yatırılması önerilen tutar (plan mantığına göre).

Olası Kazanç → Kupon kazanırsa kasaya girecek brüt para.

Aktif adım (current step):

Satır, açık mavi (current-row) arka plan ile işaretlenir.

Satırın başında küçük bir “👉” ikonu görünür.

Bu tur için önerilen tutar, Oynanan Tutar alanına otomatik yazılır.

💾 Veri Saklama (localStorage)

Uygulama tüm veriyi, tarayıcı üzerinde localStorage ile saklar:

Ana anahtar: kasaV14_fixedMath

Saklananlar:

3 kasa için:

Kupon geçmişleri (history),

Ayarlar (savedInputs: kasa, oran, adım, hedef),

Aktif kasa indeksi (activeIndex).

Önemli noktalar:

Başka bir cihazdan veya başka bir tarayıcıdan açarsan boş başlar.

Aynı cihazda, aynı tarayıcıda açtığında kaldığın yerden devam eder.

“Bu Kasayı Sıfırla” veya “Tüm Kasaları Sıfırla” dediğinde ilgili veriler localStorage’tan silinir.

📱 Mobil Kullanım ve PWA Hissi

Sayfa mobil dostu build edilmiştir.

Uygulama, mobil tarayıcıya eklendiğinde (Add to Home Screen):

Tam ekran bir uygulama gibi çalışabilir.

Tema rengi üst bar ile uyumlu görünür (destekleyen tarayıcılarda).

Öneri:

Android / iOS’ta:

Siteyi aç → Tarayıcı menüsünden “Ana Ekrana Ekle”

Kısa yol üzerinden tam ekran, bağımsız bir uygulama gibi kullan.

⚠️ Uyarı

Bu araç, tamamen matematiksel bir simülasyon ve kasa takip aracıdır:

Herhangi bir şekilde bahis tavsiyesi vermez.

Gerçek para ile yapılacak oyunlar için sorumluluk kullanıcıya aittir.

Martingale ve benzeri sistemler, uzun vadede ciddi risk içerir ve kasanın tükenmesine sebep olabilir.


Lütfen:

Gerçek para ile kullanmadan önce mantığını iyi anla,

Riskini yönet,

Kendi sorumluluğunda olduğunu unutma.

# 📊 Kasa Takip (Fix Sürüm)

Martingale mantığına göre **kasa yönetimi** ve **bahis adımı takibi** yapabileceğin, tamamen tarayıcı üzerinde çalışan bir araçtır.

- ✅ Her kullanıcı için veriler **kendi tarayıcısında** saklanır (localStorage).
- ✅ Sunucuya veri gönderilmez.
- ✅ 3 ayrı kasa (portföy) arasında hızlı geçiş yapabilirsin.
- ✅ Canlı plan, geçmiş kuponlar ve özet istatistikler tek ekranda.

---

## 🔗 Nasıl Açılır?

Eğer GitHub Pages üzerinden yayındaysa (örnek):

```text
https://douded.github.io/martingale/
