# Kasa Takip (Martingale Plan Takip Aracı)

Bu proje, bahis / kupon serilerinde kullanılan **çok adımlı (martingale benzeri) kasa yönetim planını** takip etmek, simüle etmek ve istatistiksel olarak projekte etmek için geliştirilmiş bir web uygulamasıdır.

Tamamen **tarayıcı üzerinde**, **localStorage** kullanarak çalışır. Sunucuya veri göndermez, hesaplar cihazın içinde tutulur.

---

## Özellikler

- 📂 **3 Ayrı Kasa (Portföy) Desteği**
  - KASA 1 / KASA 2 / KASA 3 arasında hızlı geçiş
  - Her kasa için ayrı:
    - Kasa bakiyesi
    - Plan oranı
    - Adım sayısı (tur)
    - Hedef kazanan kupon sayısı
    - Kupon geçmişi ve istatistikler

- 📊 **Özet Kartı**
  - Başlangıç Kasa (hesaplanan)
  - Son Kasa (güncel)
  - Net Kâr
  - Büyüme Yüzdesi

- 📈 **Dashboard İstatistikleri**
  - Kalan Kazanılması Gereken Kupon Sayısı
  - Tutturulan Kupon
  - Kaybedilen Kupon
  - **Hit Oranı (%)**
  - Kazanan Kuponların Ortalama Oranı
  - **Tahmini Son Kasa** (istatistiksel projeksiyon)

- 🧮 **Canlı Plan Tablosu**
  - Plan oranına göre **geometrik bölüşüm**
  - Her tur için:
    - Yatırılacak tutar
    - Olası kazanç
  - Aktif adım satırı vurgulu (👉 işaretli)

- 🧾 **Kupon Geçmişi**
  - Adım, tutar, oran, olası kazanç
  - Durum:
    - ⚪ Bekliyor
    - ✅ KAZANDI
    - ❌ KAYBETTİ
  - İşlemler:
    - Kazandı / Kaybetti işaretleme
    - Geri al (↺)
    - Sil (🗑) – bakiyeyi otomatik düzeltir

- 💾 **Otomatik Kayıt (localStorage)**
  - Tarayıcıyı kapatsan bile:
    - Tüm kasalar
    - Kupon geçmişleri
    - Ayarlar
  - Otomatik kaydedilir ve tekrar açıldığında yüklenir.

- 📱 **Mobil Uyumlu / PWA Hazır**
  - iOS & Android tarayıcılarda rahat kullanım
  - `apple-mobile-web-app-capable` ve `theme-color` meta etiketleri ile PWA uyumu

---

## Kullanım

1. **Kasa Ayarları**
   - `Kasa (TL)`: Mevcut kasadaki para.
   - `Plan Oranı`: Planladığın kupon oranı (örneğin 2.00).
   - `Tur (Adım)`: Döngü uzunluğu (örneğin 4 adım).
   - `Hedef`: Toplam kaç kazanan kupona göre plan yapıyorsun? (örneğin 100).

2. **Canlı Plan**
   - Plan oranına göre, seçtiğin adım sayısı içinde kasayı geometrik şekilde dağıtır.
   - Örneğin:
     - Oran: 2.00
     - Adım: 4
     - Kasa: 5000 TL
   - Plan, ilk adımda daha küçük, sonlara doğru büyüyen bir risk dağılımı oluşturur.

3. **Kupon Girişi**
   - `Oynanan Tutar`: Bu kupon için yatırdığın para.
   - `Kupon Oranı`: Kuponun gerçek oranı.
   - `Kuponu Kaydet (Bekliyor)` butonuna tıkla.
   - Kupon açıldığında:
     - ⚪ Bekliyor durumunda kalır.
     - Sonra kuponu **KAZANDI** veya **KAYBETTİ** olarak işaretleyebilirsin.

4. **Sonuç İşaretleme**
   - `✔` → Kupon kazandı:
     - Kasa: `+ (tutar × oran)` kadar artar.
   - `✖` → Kupon kaybetti:
     - Kasa: zaten kayıp yazılmış olduğu için ekstra işlem yok.
   - `↺` → Sonucu geri al:
     - Gerekirse kazanç geri düşülür, kupon tekrar **Bekliyor** durumuna gelir.

5. **Kasa Sıfırlama**
   - `Bu Kasayı Sıfırla` → Yalnızca aktif kasayı sıfırlar.
   - `Tüm Kasaları Sıfırla` → Tüm kasaları ve geçmişi temizler.

---

## Canlı Plan Mantığı (Geometrik Dağılım)

Plan tablosu şu mantıkla hesaplanır:

- `growthFactor = oran / (oran - 1)`
- Ağırlıklar:
  - 1. adım: 1
  - 2. adım: 1 × growthFactor
  - 3. adım: önceki × growthFactor
  - ...
- Toplam ağırlık = `w1 + w2 + ... + wn`
- Her adıma yatırılacak tutar:
  - `tutar_i = (Kasa / toplam_ağırlık) × ağırlık_i`

Bu sayede, hangi adımda kazanırsan kazan:
- Plan **aynı oransal kazancı** hedefler.
- Martingale’nin kaba “katla, tekrar dene” mantığı yerine daha kontrollü/geometrik bir sermaye dağılımı kullanılır.

---

## Tahmini Son Kasa Hesabı (dispProjected)

Tahmin alanında tek bir değer gösterilir:

> **"Hedefteki toplam kazanan kupon sayısına ulaştığımda, tahmini kasam ne olur?"**

Burada kullanılan mantık:

1. **Ortalama oran (avgOdds)**  
   - Önce **kazanan kuponların** ortalama oranı hesaplanır.
   - Eğer henüz hiç kazanan yoksa:
     - `avgOdds = Plan Oranı` (örneğin 2.00)
   - `avgOdds` her yeni win’de **dinamik** olarak güncellenir, tahmin de buna göre değişir.

2. **Simülasyon Baz Kasa (cycleBaseBalance)**
   - Ekrandaki “Plan Bazı” değeri tahmin için başlangıç kabul edilir.
   - Yani o anki döngüde, kayıplar geri eklenmiş, henüz yeni kuponu oynamadan önceki “sanal başlangıç” kullanılır.

3. **Şu Andaki Adımda Kazanıyormuş Gibi Kabul**
   - Canlı plandaki `currentStep` için:
     - O adımda yatırılacak tutar: `stake_current`
   - Bu kupon **hemen kazanmış gibi** hesap yapılır:
     - `balanceAfterNextWin = cycleBaseBalance + stake_current × (avgOdds - 1)`

4. **Her Win’den Sonra Büyüme Faktörü**
   - Büyüme etkisini oransal görmek için 1 birimlik kasa ile aynı geometrik plan tekrar hesaplanır.
   - Bu planın 1. adımında kazanıldığında elde edilen kâr oranı:
     - `profitNorm = stake0 × (avgOdds - 1)`
   - Her win sonrası:
     - `growthFactorPerWin = 1 + profitNorm`

5. **Kalan Kazanan Kupon Sayısı**
   - Hedef: `targetWins`
   - Şu ana kadar kazanılan: `wins`
   - Kalan: `remainingWins = targetWins - wins`
   - Bu turun kuponunu kazandığını zaten varsaydığımız için:
     - Gelecek için **kalan win sayısı**: `remainingWins - 1` (0’dan küçükse 0 alınır)
