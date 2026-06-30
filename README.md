# Performans Günlüğü — Kurulum Talimatları

Bu site GitHub Pages üzerinde ücretsiz yayınlanır ve Firebase (ücretsiz katman) ile tüm cihazlarından senkronize olur. Adımları sırayla takip et, hiçbir kod bilgisi gerekmiyor.

## ADIM 1 — GitHub Reposu Oluştur

1. github.com'a git, hesabın yoksa ücretsiz oluştur.
2. Sağ üstten **+ → New repository**.
3. İsim: `performans-gunlugu` (istediğin ismi verebilirsin).
4. **Public** seç (Pages ücretsiz katmanı public repo gerektirir).
5. **Create repository**.

## ADIM 2 — Dosyayı Yükle

1. Az önce oluşturduğun repoda **Add file → Upload files**.
2. Bu klasördeki `index.html` dosyasını sürükle bırak.
3. **Commit changes**.

## ADIM 3 — GitHub Pages'i Aç

1. Repo içinde **Settings → Pages**.
2. "Branch" altında `main` seç, klasör olarak `/ (root)`, **Save**.
3. 1-2 dakika bekle, sayfanın en üstünde bir link çıkacak: `https://kullaniciadin.github.io/performans-gunlugu/`
4. Bu link artık senin sitenin — telefon, bilgisayar, tablet, her cihazdan açılır.

## ADIM 4 — Firebase ile Cihazlar Arası Senkronu Aktif Et

Şu anki haliyle site her cihazda **ayrı ayrı** çalışır (tarayıcı hafızasında). Cihazlar arası gerçek senkron için ücretsiz bir Firebase projesi kurman gerekiyor — 10 dakika sürer.

1. console.firebase.google.com adresine git, Google hesabınla giriş yap.
2. **Add project** → bir isim ver (örn. `enes-performans`) → Google Analytics'i kapat (gerekmiyor) → **Create project**.
3. Proje açıldıktan sonra sol menüden **Build → Firestore Database**.
4. **Create database** → **Start in test mode** seç (önemli — production mode farklı kurallar ister) → bölge olarak sana yakın birini seç (örn. `eur3`) → **Enable**.
5. Sol menüden çark ikonuna tıkla → **Project settings**.
6. Aşağıda "Your apps" bölümünde `</>` (Web) ikonuna tıkla.
7. Bir takma ad ver (örn. `web`), **Register app**.
8. Karşına çıkan kod bloğunda `firebaseConfig` objesini göreceksin, şuna benzer:
   ```js
   const firebaseConfig = {
     apiKey: "AIzaSy...",
     authDomain: "enes-performans.firebaseapp.com",
     projectId: "enes-performans",
     ...
   };
   ```
9. Bu üç değeri (`apiKey`, `authDomain`, `projectId`) kopyala.

## ADIM 5 — Kodu Güncelle

1. GitHub reposunda `index.html` dosyasını aç, sağ üstten kalem (Edit) ikonuna tıkla.
2. Dosya içinde şu satırları bul (Ctrl+F ile "REPLACE_ME" ara):
   ```js
   const firebaseConfig = {
     apiKey: "AIzaSyDUMMY_REPLACE_ME",
     authDomain: "REPLACE_ME.firebaseapp.com",
     projectId: "REPLACE_ME",
   };
   ```
3. Bu üç değeri Firebase'den kopyaladığın gerçek değerlerle değiştir.
4. **Commit changes**.
5. 1 dakika içinde GitHub Pages otomatik güncellenir.

## ADIM 6 — Firestore Güvenlik Kuralları (Önemli)

Test modu 30 gün sonra otomatik kapanır ve herkes okuyup yazabilir hale gelir (sadece senin kullanacağın bir uygulama için risk düşük ama yine de ayarlayalım).

1. Firebase Console → Firestore Database → **Rules** sekmesi.
2. Şunu yapıştır:
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /epg/{document} {
         allow read, write: if true;
       }
     }
   }
   ```
3. **Publish**.

Bu kural herkesin senin "senkron kodunu" bilmesi durumunda veriye erişebileceği anlamına gelir — bu yüzden sitede kullanacağın senkron kodunu tahmin edilmesi zor bir şey yap (örn. `enes-7f3k-2026`), basit bir şey değil (örn. `1234`).

## ADIM 7 — Kullanım

1. Siteyi aç: `https://kullaniciadin.github.io/performans-gunlugu/`
2. En üstteki "Senkron kodu" kutusuna kendi belirlediğin kodu yaz (örn. `enes-7f3k-2026`), **Bağlan**.
3. Aynı kodu telefonunda da gir — veriler otomatik eşleşir.
4. Günlük formu doldur, **Günü Kaydet**.

## Site Özellikleri

- **Kilo eğrisi grafiği** — hedef çizgisiyle birlikte
- **4 canlı metrik** — güncel kilo, haftalık kayıp hızı (renk kodlu), bel ölçüsü, kayıt serisi
- **Günlük form** — kilo, bel, adım, kalori, protein, enerji/açlık/uyku, antrenman tipi ve notu
- **Yağ kaybı simülasyonu** — adaptif TDEE modeliyle (kilo düştükçe TDEE de düşer) hedefe kaç haftada ulaşacağını hesaplar, kalori/adım senaryolarını değiştirip test edebilirsin
- **Cihazlar arası senkron** — Firebase Firestore ile

## Bilinmesi Gerekenler

- **AI koç analizi bu sitede yok.** Public bir sitenin koduna Anthropic API anahtarı koymak güvenli değil (herkes görüp kullanabilir, sana fatura çıkar). Haftalık verilerini buraya, Claude sohbetine yapıştırmaya devam et, analiz ben yaparım.
- Firebase ücretsiz katmanı (Spark plan) bu kullanım için fazlasıyla yeterli — günde binlerce okuma/yazma hakkı var, sen günde birkaç kayıt yapacaksın.
- `index.html` dosyasını her düzenlediğinde GitHub otomatik commit history tutar, geri alabilirsin.
