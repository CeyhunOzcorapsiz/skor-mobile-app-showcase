# SKOR! — 1v1 Spor Bilgi Yarışması
## Uçtan Uca Proje Planı

> Çalışma adı "SKOR!" — istediğin zaman değiştiririz.
> Son güncelleme: 29 Ağustos 2026

---

## 1. Vizyon ve Konumlandırma

**Tek cümle:** Arkadaşınla basketbol/futbol bilgini kapıştırdığın, sıra tabanlı (asenkron) 1v1 bilgi yarışması.

**Neden var olmalı (farklılaşma özeti):**
- Genel trivia uygulamaları (Trivia Crack vb.) sporda yüzeysel; spor uygulamaları ise trivia'da zayıf.
- Türkçe pazarda kaliteli, güncel, derinlikli spor trivia'sı neredeyse yok.
- "Arkadaşını yen" duygusu spor muhabbetinin doğal uzantısı — maç sonrası tartışmanın oyunlaşmış hali.

**Hedef kitle (ilk halka):** 16-35 yaş, maç izleyen, arkadaş grubuyla spor konuşan, telefonda kısa oturumlu oyun oynayan kullanıcı. İlk kullanıcılar: kendi arkadaş çevren (test grubu olarak da işlev görür).

**Başarı ölçütleri (MVP için mütevazı ve gerçekçi):**
- 20+ gerçek kullanıcı en az 1 tam oyun tamamlasın
- Oyun başlatanların %60'ı oyunu bitirsin (tamamlama oranı)
- Bir kullanıcı ortalama 3+ oyun oynasın (rövanş mekaniği çalışıyor demektir)

---

## 2. Oyun Tasarımı (Core Loop)

### 2.1 Temel döngü
```
Oyun kur → Kategori seç → 5 soruyu cevapla → Arkadaşını davet et
→ (bildirim) Arkadaş aynı 5 soruyu cevaplar → Sonuç ekranı → RÖVANŞ
```
Rövanş butonu döngüyü kapatır — bağlılığın motoru budur.

### 2.2 Soru mekaniği
- **5 soru / oyun** (kısa oturum: toplam ~90 saniye oynanış)
- Çoktan seçmeli, 4 şık
- **Soru başına 15 saniye.** Süre dolarsa yanlış sayılır.
- **Puanlama:** Doğru = 100 puan + hız bonusu (kalan saniye × 5, maks +75).
  Böylece hem bilgi hem refleks ödüllenir; beraberlik nadir olur.
- Beraberlik olursa: 1 altın soru (sudden death) — v1.1'e ertelendi, MVP'de beraberlik "berabere" biter.

### 2.3 Zorluk dengesi
Her oyunda karışım sabit: 2 kolay + 2 orta + 1 zor. Böylece yeni oyuncu ezilmez, iyi oyuncu sıkılmaz.

### 2.4 Adalet / hile önleme (asenkron modelde kritik)
- İki oyuncu **aynı 5 soruyu** görür (adil karşılaştırma).
- Sorular cevaplanmadan istemciye **doğru cevap gönderilmez** (doğrulama Cloud Function'da) — v1.1. MVP'de basit tutulur, doğru index istemcide olur (arkadaş arası oyun için kabul edilebilir risk).
- Soru ekranında uygulamadan çıkıp dönme = o soru yanmış sayılır (ekran görüntüsü alıp Google'lamayı caydırır).

---

## 3. Özellik Kapsamı — Sürüm Sürüm

### MVP (v1.0) — hedef ~5-6 hafta
| Özellik | Detay |
|---|---|
| Anonim giriş | Kullanıcı adı seç, şifre yok (Firebase Anonymous Auth) |
| 2 kategori | Basketbol, Futbol |
| Oyun kurma | Kategori seç, 5 soruyu cevapla |
| Davet | 6 haneli kod + paylaşılabilir link (WhatsApp'a "Beni yenebilir misin?" mesajı) |
| Asenkron oynanış | Rakip müsait olunca oynar |
| Sonuç ekranı | Skor, soru soru kıyaslama, hız farkları |
| Rövanş | Tek dokunuşla aynı rakibe yeni oyun |
| Bildirim | "Sıra sende" + "Sonuç hazır" (FCM) |
| Basit profil | Kullanıcı adı, G/M/B istatistiği, seri |

### v1.1 — cila ve derinlik (~2-3 hafta)
- Google ile giriş (hesap cihazlar arası taşınır)
- **Günün Maçı — TAMAMLANDI:** herkese aynı 5 soru, günde tek hak, seri ve paylaşılabilir sonuç
- **Maç Günü soruları** (gerçek maçlardan 24 saat içinde manuel eklenen güncel sorular, "Güncel" rozetiyle)
- Altın soru (beraberlik bozucu)
- **Kafa kafaya geçmiş — TAMAMLANDI:** sonuç ekranında "Ahmet karşısında 7-3 öndesin"
- Kategori bazlı istatistik ("Basketbolda %78, futbolda %61 doğruluk")
- **Sonuç paylaşım kartı — TAMAMLANDI:** görsel skor kartı (PNG) 1v1 ve Günün Maçı için
- Ses efektleri, haptic feedback, animasyonlar

### v1.2 — kimlik
- **Fanatiklik modu:** takımını seç; kendi takımının sorularında çift puan / çift risk, "Gerçek taraftar mısın?" testi ve taraftarlık yüzdesi

### KARAR (15 Tem 2026) — yeni eklenen fikirler
- **Gole ilerleme sunumu (öncelikli, v1.1 — YAPILIYOR):** oyun ekranında minyatür saha;
  her doğru cevapta top kaleye bir adım ilerler, yanlışta savunma keser; 5/5 = GOL
  animasyonu (+50 bonus). Basketbolda pota/smaç eşdeğeri. Sunum katmanı — düşük risk.
- **Penaltı serisi (öncelikli, v1.1 — YAPILIYOR):** berabere biten maçta 3 soruluk
  penaltı çekişmesi; süre 7 sn, her soru bir vuruş (bilirsen gol, bilemezsen kaleci
  kurtarır). Asenkron: rakip beraberliği görünce penaltılarını atar, kurucuya
  "sıra sende: penaltılar" düşer. 3 sonrası hâlâ eşitse berabere kalır.
- **Bil ve Fethet modu (v2.0):** saha 5-7 bölgeye ayrılır (kale→defans→orta→hücum→kale);
  bölge başına soru düellosu, bileni bölgeyi fetheder; tüm sahayı alan maçı kazanır.
  Asenkron hamle modeliyle uyumlu.
- **Maç kurgusu (v1.1-1.2) — spiker dilli maç raporu TAMAMLANDI (düdük/skorboard sunumu açık):** oyun baştan sona maç sunumu — düdük, soru=dakika,
  skorboard; sonuç ekranı spiker diliyle "maç raporu".
- **Joker sistemi (v1.2) — 50-50 ve +10 sn TAMAMLANDI (VAR ertelendi):** maç başına 1 joker — VAR (yanlışı bir kez iptal),
  50-50 (iki şık elenir), Taraftar Desteği (+10 sn). Günlük ödül halkasına bağlanabilir.
- **Rozet/başarım sistemi (v1.2) — TAMAMLANDI (Altın Ayakkabı sunucu tarafına kaldı; + Penaltı Kralı, Ezeli Rakip):** Hat-trick (3 galibiyet üst üste), Buzzer Beater
  (son 2 sn'de doğru), Kale Duvar (rakibe 5'te 0), Altın Ayakkabı (haftanın en yüksek
  puanı). Profil vitrini + paylaşım kartına rozet.
- **Kışkırtma mesajları (v1.1 — ucuz):** davet ve rövanşta hazır trash-talk seçenekleri.
- **Kariyer modu (v2.0):** tek kişilik hikâyeli mod — Amatör → 1. Lig → Süper Lig →
  Avrupa → Efsane; her sezon 10 maç, zorluk katmanlarıyla. Offline oynanabilir.
- **Haftanın Maçı (v1.2-2.0):** herkese aynı 5 soru, tek hak, haftalık skor tablosu;
  grup ligine köprü.

### Güvenlik yol haritası (veri sızması önlemleri)
1. App Check istemci entegrasyonu tamamlandı; Play Integrity kaydı ve enforcement bekliyor
2. Release derlemede kod karartma: --obfuscate --split-debug-info (bu paketle uygulanıyor)
3. Sunucu taraflı cevap doğrulama (Blaze/Cloud Functions gerektirir) —
   şu an correctIndex istemciye iniyor; sıralama/ödül eklenmeden önce ŞART
4. Google ile giriş bağlama (hesap kurtarma + kimlik sağlamlığı, v1.1)
5. Gizlilik politikası: toplanan veri sadece takma ad + skorlar (Play Store zorunluluğu)
   — taslak `docs/gizlilik-politikasi.md` hazır (iletişim e-postası + yayın adresi eksik)
6. Uygulama içi hesap silme (Play veri silme şartı) — TAMAMLANDI; kuralların deploy edilmesi gerekir

### v2.0 — büyüme özellikleri
- **Arkadaş grubu ligi:** 5-10 kişilik özel lig, herkes herkesle, haftalık puan durumu (en viral özellik)
- **Canlı mod (senkron):** aynı anda, aynı soru, kim hızlıysa. Teknik gösteriş burada.
- Rastgele rakip eşleştirme + basit ELO
- Yeni kategoriler (NBA / Avrupa futbolu / Süper Lig ayrımı, F1, tenis...)
- Türkçe + İngilizce dil desteği

---

## 4. Farklılaşma — Bizi Sıradan Trivia'dan Ayıracak Fikirler

Öncelik sırasına göre (etki / zorluk dengesi):

### 4.1 "Maç Günü" soruları ⭐ (en güçlü fikir)
Gerçek maçlardan **24 saat içinde** üretilen güncel sorular: "Dün akşamki derbide ilk golü kim attı?" Trivia Crack bunu asla yapamaz — sorular statik. Sen küçüksün, çeviksin; her hafta 10-15 güncel soru elle bile eklenebilir. Bu, uygulamayı "arada açılan oyun"dan "maç ertesi ritüeli"ne çevirir. *(v1.1'de manuel, ileride yarı otomatik.)*

### 4.2 Arkadaş grubu ligi ⭐
5-10 kişilik özel lig kur, herkes herkesle oynar, haftalık puan durumu. Spor zaten grup halinde tüketiliyor (maç grupları, halı saha ekipleri) — WhatsApp grubunun oyunlaşmış hali. Viral katsayısı en yüksek özellik: bir kişi ligi kurunca 8 kişiyi birden getirir. *(v2.0)*

### 4.3 Fanatiklik modu
Takım seç (Fenerbahçe, Lakers...). Kendi takımın hakkındaki sorularda ekstra puan ama yanlışta ekstra kayıp: "Gerçek taraftar mısın?" testi. Kimlik + duygu = paylaşım. *(v1.1-v2.0 arası)*

### 4.4 Günlük tek soru ("Günün Sorusu")
Wordle mantığı: herkese aynı gün aynı zor soru, günde bir hak, seri sayacı. Uygulamayı her gün açtıran kanca; sonucu "🏀 12 gündür bilıyorum" formatında paylaşılabilir. *(v1.1 — maliyeti çok düşük, etkisi yüksek)*

> **KARAR (13 Tem 2026):** 4.1–4.4 yol haritasına alındı. İddia modu ve topluluk soru katkı sistemi kapsam DIŞI bırakıldı.
> Sürüm dağılımı: Günlük Soru → v1.1 · Maç Günü → v1.1 (manuel) · Fanatiklik modu → v1.2 · Grup ligi → v2.0

**Bilinçli olarak YAPMAYACAKLARIMIZ:** para ödüllü yarışma (yasal bataklık), canlı skor/istatistik servisi (API maliyeti + mevcut devler), bahis benzeri her şey.

---

## 5. Ekran Akışı ve UX

```
[Açılış/Splash]
   └─> ilk açılış → [Kullanıcı adı seç]
   └─> sonrası    → [Ana Ekran]

[Ana Ekran]
   ├─ "YENİ OYUN" (büyük buton)
   ├─ Aktif oyunlar listesi:
   │    "⏳ Ahmet'in sırası" / "🎯 Sıra sende!" / "🏁 Sonuç hazır!"
   ├─ Koda katıl (arkadaşın kodunu gir)
   └─ Profil ikonu → [Profil]

[Yeni Oyun] → [Kategori Seç: 🏀 / ⚽] → [Oyun Ekranı ×5 soru]
   → [Davet Ekranı: kod + WhatsApp paylaş butonu]

[Oyun Ekranı]
   ├─ Üstte: soru sayacı (3/5), süre çubuğu (15sn, azaldıkça kırmızılaşır)
   ├─ Ortada: soru metni
   ├─ Altta: 4 şık (cevaplayınca anında yeşil/kırmızı geri bildirim)
   └─ Cevap sonrası 1sn bekle → sonraki soru (akış kesintisiz)

[Sonuç Ekranı]
   ├─ Büyük skor: SEN 420 — 385 AHMET
   ├─ Soru soru kıyas: ✅✅❌✅✅ vs ✅❌✅✅❌ + süreler
   ├─ "RÖVANŞ" (birincil buton) + "Paylaş"
   └─ Kafa kafaya geçmiş (v1.1)
```

**Tasarım dili:** Koyu zemin + canlı vurgu rengi (enerjik, "maç gecesi" hissi). Turuncu (basketbol) / yeşil (futbol) kategori renkleri. Büyük tipografi, tek elle oynanabilir (şıklar ekranın alt yarısında).

---

## 6. Teknik Mimari

### 6.1 Stack
| Katman | Seçim | Gerekçe |
|---|---|---|
| Mobil | **Flutter (Dart)** | Tek kodla Android+iOS, güçlü ekosistem, öğrenmesi kolay |
| State management | **Riverpod** | Modern, test edilebilir, dokümantasyonu iyi |
| Backend | **Firebase** | Sunucusuz; auth + veritabanı + bildirim tek pakette, ücretsiz kota yeter |
| Veritabanı | **Cloud Firestore** | Gerçek zamanlı dinleme (rakip oynayınca ekran anında güncellenir) |
| Bildirim | **Firebase Cloud Messaging** | Ücretsiz push |
| Sunucu mantığı | **Cloud Functions** (v1.1+) | Puan doğrulama, bildirim tetikleme |
| Analitik | **Firebase Analytics** | Hangi ekranda kopuyorlar, görmek için |
| Crash takibi | **Crashlytics** | Ücretsiz, kurulumu 10 dk |

### 6.2 Firestore veri modeli
```
users/{uid}
  username: string          // benzersiz, 3-15 karakter
  createdAt: timestamp
  stats: { wins, losses, draws, streak, bestStreak }
  categoryStats: { basketball: {correct, total}, football: {...} }  // v1.1
  fcmToken: string

questions/{questionId}
  category: "basketball" | "football"
  difficulty: 1 | 2 | 3
  text: string
  options: [string, string, string, string]
  correctIndex: 0-3
  active: bool               // hatalı soruyu yayından kaldırmak için
  timesAsked: int            // hangi soru çok çıkıyor takibi

games/{gameId}
  code: string               // "XK4F2A" — 6 haneli davet kodu ve doküman ID'si
  category: string
  questionIds: [5 adet]
  status: "waiting_opponent" | "p1_done" | "completed" | "expired"
  createdAt / completedAt: timestamp
  players: {
    p1: { uid, username, answers: [{selectedIndex, timeMs}], score },
    p2: { ... } | null
  }
  winner: uid | "draw" | null
```

**Kritik kararlar:**
- Sorular oyuna **ID referansıyla** girer; oyuncu2 oynarken aynı sorular çekilir.
- `games` koleksiyonunda **denormalize username** tutulur (her seferinde users okumaya gerek kalmaz → maliyet düşer).
- 7 gün cevaplanmayan davete katılım güvenlik kurallarıyla kapanır.

### 6.3 Güvenlik kuralları (Firestore Rules) — özet
- `users`: herkes okur (kullanıcı adı için), sadece sahibi yazar.
- `questions`: herkes okur, **kimse istemciden yazamaz** (sorular admin scriptiyle yüklenir).
- `games`: sadece oyuncuları okur/yazar; `answers` alanına yalnızca kendi tarafına yazabilir; tamamlanmış oyun değiştirilemez.

### 6.4 Proje yapısı (Flutter)
```
lib/
  main.dart
  core/            // tema, sabitler, yardımcılar
  models/          // UserProfile, Game, Question (freezed)
  services/        // auth_service, game_service, question_service
  providers/       // Riverpod provider'ları
  screens/
    onboarding/  home/  game/  result/  profile/
  widgets/         // ortak bileşenler (süre çubuğu, şık butonu...)
assets/
  questions/       // geliştirme aşaması yerel soru JSON'ları
```

---

## 7. Soru Havuzu Stratejisi (projenin gerçek kalbi)

**Hedef:** Lansmanda kategori başına 150 soru (toplam 300). Bu, 5'li oyunlarda ~30 oyun boyunca tekrar hissettirmez.

**Dağılım (kategori başına):** 60 kolay / 60 orta / 30 zor.

**Üretim hattı (KARAR 13 Tem 2026 — elle yazımdan otomatik üretime evrilen mimari):**

`questions.json` veritabanı DEĞİL, üretim hattının taslak/onay formatıdır. Çalışma zamanında sorular Firestore'dan gelir (milyonlarca dokümana ölçeklenir). Akış:

```
[Üretim motorları] → taslak JSON → [İNSAN ONAYI: verified=true] → Firestore'a yükleme scripti → oyun
```

Üç üretim motoru, aşamalı devreye girer:

1. **Elle yazım (MVP):** İlk ~100-150 soru. Hızlı başlamak için.
2. **Şablon + veri motoru (v1.1):** Wikidata'daki yapılandırılmış kariyer verisinden
   otomatik `career` ve `compare` sorusu üretimi. Tek şablon → binlerce soru,
   cevap veriden geldiği için yanlış olamaz. Çeldiriciler benzer profilli
   oyunculardan otomatik seçilir (aynı lig/pozisyon/dönem → zorluk ayarı da otomatik).
3. **LLM motoru (v1.1-v1.2):** Claude API ile `moment`/`clues` tipi yaratıcı sorular
   toplu taslak üretimi. Spor API'lerinden (ücretsiz katman) haftalık `current` /
   Maç Günü soruları: "Dün X-Y maçında ilk golü kim attı?"

**Değişmez kural:** Hiçbir otomatik üretilen soru insan onayı olmadan yayına girmez.
Trivia'da doğruluk ürünün kendisidir; tek yanlış cevap güveni bitirir. Otomasyonla
iş "soru yazmak"tan "soru onaylamak"a iner (~10× hız). İleride basit bir admin onay
ekranı (web, tek sayfa) yapılır; o zamana kadar onay JSON üzerinde `verified: true` ile.

**Soru tipleri (KARAR 13 Tem 2026 — düz ansiklopedi sorusu değil, akıllı sorular):**

| Tip | Örnek | Havuz payı |
|---|---|---|
| **Kariyer tahmini** (`career`) | "Galatasaray → Atlético Madrid → Barcelona → Başakşehir. Bu kariyer kime ait?" | ~%35 |
| **İpucu ipucu** (`clues`) | 3 ipucu sırayla açılır: "Solak. / 21 sezon tek takımda. / 2011'de Miami'yi devirdi." Az ipucuyla bilen çok puan (1. ipucu: 150p, 2.: 100p, 3.: 50p) | ~%15 |
| **Karşılaştırma** (`compare`) | "Hangisinin NBA şampiyonluğu daha fazla?" (Russell / Jordan / Kobe / LeBron) | ~%15 |
| **Güncel / Maç Günü** (`current`) | "Bu sezon Süper Lig'de gol krallığında kim önde?" — raf ömrü kısa, `expiresAt` alanı var | ~%15 |
| **An / olay** (`moment`) | "2023 Şampiyonlar Ligi finali hangi şehirde oynandı?" gibi hatırlanan anlar | ~%10 |
| **Klasik bilgi** (`classic`) | Kural/tarih bilgisi — sadece gerçekten iyi olanlar | ~%10 |

`clues` tipi ayrı bir ekran mekaniği gerektirir (ipuçları sırayla açılır) — MVP'de `career` ile başla, `clues` v1.1'de gelir.

**Kalite kuralları:**
- Çeldiriciler mantıklı olmalı (4 şıkın 4'ü de "olabilirlik" hissi vermeli)
- Tek doğru cevap, tartışmasız (kaynağı belli)
- "Hangi yılda..." tarzı kuru ezber sorusu neredeyse hiç olmasın
- Güncel sorularda `expiresAt` zorunlu (sezon bitince otomatik havuzdan düşer)
- Türkiye ağırlıklı denge: futbolda Süper Lig + Avrupa; basketbolda NBA + BSL + EuroLeague

---

## 8. Zaman Çizelgesi (haftalık, gerçekçi)

| Hafta | Hedef | Çıktı |
|---|---|---|
| **1** | Kurulum + iskelet | Flutter/Firebase kurulu, proje yapısı, tema, ekran iskeletleri |
| **2** | Tek kişilik oynanış | Yerel JSON'dan 5 soru, süre çubuğu, puanlama, sonuç ekranı. **Oynanabilir demo!** |
| **3** | Firebase entegrasyonu | Anonim auth, kullanıcı adı, sorular Firestore'dan, oyun kaydı |
| **4** | 2 kişilik asenkron mod | Oyun kurma, davet kodu+link, rakip oyunu, gerçek sonuç ekranı |
| **5** | Bildirim + rövanş + cila | FCM, rövanş akışı, ana ekran oyun listesi, hata durumları |
| **6** | Test + kapalı beta | 5-10 arkadaşla gerçek test, geri bildirim, kritik düzeltmeler |
| **7-8** | Yayın hazırlığı | İkon, mağaza görselleri, gizlilik politikası, Play Store içsel test → açık yayın |

Paralel her hafta: ~40 soru yazımı.

**Yayın notları:**
- Önce **sadece Android** (Play Store: 25$ tek seferlik). iOS sonra (Apple: 99$/yıl + Mac gerekir) — v1.1'de değerlendirilir.
- Play Store artık yeni kişisel hesaplarda 12 test kullanıcısı × 14 gün kapalı test şartı arıyor — 6. haftadaki beta bu şartı da karşılar, planla örtüşüyor.

---

## 9. Maliyet

| Kalem | Maliyet |
|---|---|
| Flutter, Firebase (Spark planı) | 0 ₺ |
| Google Play geliştirici hesabı | ~25 $ (tek seferlik) |
| Domain (opsiyonel, davet linki için) | ~300 ₺/yıl (Firebase Dynamic Links yerine `skorapp.com/d/XK4F2` gibi) |
| **Toplam MVP** | **~1.500 ₺ altı** |

Firebase ücretsiz kota: 50K okuma/gün, 20K yazma/gün — binlerce günlük oyuna kadar bedava. Para kazanma (reklam vb.) MVP kapsamı DIŞINDA; önce insanlar oynasın.

---

## 10. Riskler ve Önlemler

| Risk | Olasılık | Önlem |
|---|---|---|
| Soru yazımı sıkıcı gelir, havuz eksik kalır | Yüksek | Haftalık küçük hedef (40), Claude ile taslak üretimi, önce 100 soruyla beta |
| Davet edilen arkadaş uygulamayı indirmez | Yüksek | Sürtünmeyi azalt: link → mağaza → kod otomatik dolu gelsin (deferred deep link, v1.1); MVP'de kod elle girilir |
| Flutter öğrenme eğrisi süreyi uzatır | Orta | 2. hafta sonunda demo hedefi erken uyarı sinyali; gerekirse kapsam kırpılır (bildirim v1.1'e kayar) |
| Tek geliştirici motivasyon düşüşü | Orta | Her hafta "gösterilebilir" bir çıktı; arkadaşlara erken göster |
| İsim/marka çakışması | Düşük | Yayın öncesi Play Store + marka araması |

**Kapsam kırpma sırası (sıkışırsak önce bunlar atılır):** bildirimler → davet linki (sadece kod kalır) → profil istatistikleri. Çekirdek asla kırpılmaz: oyun kur → oyna → davet → sonuç → rövanş.

---

## 11. İlk Adımlar (bu hafta)

1. [ ] Flutter SDK + Android Studio kurulumu, `flutter doctor` temiz
2. [ ] `flutter create skor_app` + Git deposu başlat
3. [ ] Firebase projesi oluştur, FlutterFire bağla
4. [ ] Tema + ekran iskeletleri (boş sayfalar arası gezinme)
5. [ ] İlk 20 soruluk deneme JSON'u (10 basketbol + 10 futbol)
