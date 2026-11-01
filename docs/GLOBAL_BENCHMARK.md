# SKOR! Global Benchmark ve Ürün Denetimi

Son güncelleme: 29 Ağustos 2026

## Yönetici özeti

SKOR! bugün oynanabilir bir prototipten daha ileride: Türkçe futbol/basketbol
içeriği, asenkron 1v1, davet kodu, rövanş, penaltı, profil ve otomatik soru
üretim hattı çalışıyor. Ürünün en güçlü vaadi şudur:

> Bilgi yarışması çözmezsin; bilgini kullanarak bir maç oynarsın.

Global ürünlerde günlük geri dönüş döngüsü ve farklı soru mekanikleri güçlü;
ancak bilgi sorusunu saha, gol, penaltı ve fetih duygusuyla tek bir spor maçı
kurgusuna bağlayan ürün sayısı az. SKOR! bu alanda ayrışabilir.

Yayın öncesindeki en büyük eksik yeni bir oyun modu değil, rekabet
bütünlüğüdür. Mevcut istemci doğru cevapları ve skor hesabını taşıyor; mevcut
Firestore kuralları da giriş yapmış kullanıcıların oyunları listelemesine izin
veriyor. Global sıralama veya ödül eklenmeden önce bu katman sertleştirilmelidir.

## İncelenen ürünler

| Ürün | Güçlü döngü | SKOR! için çıkarım |
|---|---|---|
| [OXO Football](https://www.oxofootball.com/) | Günlük tahta, canlı PvP, arkadaş odası, kariyer/name/blitz/survival/scout mini oyunları | Dört şıklı quiz tek başına yetmez; spor verisinden farklı mekanikler üretilebilir |
| [FootEx](https://www.footex-app.com/) | Her gün dört oyun, Ball IQ, özel ligler, günlük/season ranking | Günlük skorun uzun vadeli bir “spor zekâsı” puanına dönüşmesi güçlü |
| [NBA Play](https://www.nba.com/news/nba-play-how-to-play-and-faqs) | Rank, IQ, Player Path, Higher/Lower ve günlük içerik | Kariyer, sıralama ve karşılaştırma verisi ayrı oyun tiplerine dönüşmeli |
| [Blacktop Trivia](https://apps.apple.com/us/app/blacktop-trivia/id6747852724) | Fast Break, 82-0 draft, Stat Stack, Higher/Lower, 15 kişilik lobby | Basketbol tarafında draft ve istatistik mekanikleri ciddi farklılık yaratıyor |
| [Starting 5 Trivia](https://apps.apple.com/us/app/starting-5-trivia/id6759997813) | 4 çeyrek, 4. çeyrek çift puan, sezon, avatar, detaylı box score | Maç dramaturjisi ve maç sonrası rapor SKOR!’un ana kimliği olmalı |
| [Snapback Sports Trivia](https://apps.apple.com/us/app/snapback-sports-trivia/id6746725854) | Günlük oyun, arşiv, jackpot, arkadaş kurtarma, itiraz sistemi | Arkadaş desteği ve soru itirazı sosyal bağlılık sağlar |
| [Trivia Crack](https://www.triviacrack2.com/en/) | 1v1, günlük challenge, takım, koleksiyon, tower duel | Koleksiyon/lig uzun vadeli ilerleme sağlar; çekirdek maçtan sonra gelmeli |
| [Sporcle](https://support.sporcle.com/hc/en-us/articles/37912631701901-What-is-the-Sporcle-Mobile-App) | Çok büyük içerik, rozet, playlist, seri, derin istatistik | İçerik arşivi ve keşif güçlüdür; kalite etiketleme olmadan büyütülmemeli |

## Rakip boşluk haritası

### SKOR!’da şimdiden güçlü olanlar

- Türkçe futbol + basketbol odağı.
- Aynı sorularla asenkron arkadaş düellosu.
- Bilgiyi “topun kaleye ilerlemesi”, gol/smaç ve penaltıyla dramatize etme.
- Rövanş ve WhatsApp grubuna uygun kısa oyun döngüsü.
- Firestore tabanlı içerik güncelleme ve insan onaylı Wikidata üretim hattı.

### Global pazarda masa başı özellikler, SKOR!’da eksik

- Günlük herkese aynı challenge, seri ve paylaşılabilir sonuç.
- Farklı mekanikler: Higher/Lower, sırala, oyuncu yolu, 3 ipucunda bul.
- Özel arkadaş ligi ve haftalık tablo.
- Bildirim, derin link ve davetten tek dokunuşla oyuna giriş.
- Güvenilir global/season leaderboard.
- Hesap kurtarma ve cihaz değiştirme.
- Ürün analitiği, çökme takibi ve gerçek kullanıcı hunisi.
- Soru itirazı, hata bildirimi ve performansa göre otomatik zorluk.

## Teknik denetim bulguları

### P0 — yayın öncesi zorunlu

1. **Oyunların listelenebilmesi:** `games` için tüm giriş yapmış kullanıcılara
   okuma izni verilmiş. Açık oyunlar ve gömülü cevap anahtarları taranabilir.
2. **Doğru cevap istemcide:** `correctIndex` hem soru hem oyun dokümanında.
   App Check bunu zorlaştırır fakat gerçek çözüm sunucu taraflı cevaplamadır.
3. **Skor istemciye güveniyor:** oyuncu `score` alanını değiştirilmiş istemciyle
   gönderebilir. Global sıralama/ödül için Cloud Functions zorunlu.
4. **Penaltı bitirme kuralı:** herhangi bir katılımcı creator-penalty geçişini
   yapabiliyor; yalnız kurucuya sınırlandırılmalı.
5. **Oyun süresi yok:** açık oyunlar süresiz kalıyor. Davetler 7 günde bitmeli.
6. **Uygulamadan çıkma koruması yok:** planda olmasına rağmen soru sırasında
   arka plana geçiş cevabı yakmıyor.
7. **Kural testleri yok:** yalnız widget ve yerel soru JSON testleri bulunuyor.

### P1 — kapalı beta için

- Firebase App Check’i önce ölçüm, sonra enforcement ile açmak.
- Firebase Analytics: oyun_kuruldu, davet_katilim, soru_cevap, oyun_bitti,
  rovans gibi olaylar.
- Crashlytics ve kullanıcıya anlaşılır hata/retry durumları.
- Google/Apple hesap bağlama ve anonim hesabı yükseltme.
- Günlük Maç, seri, paylaşım kartı ve geçmiş günlüklerin arşivi.
- FCM: sıra sende, sonuç hazır, rövanş geldi.
- Soru tekrarını kullanıcı bazında azaltma ve adaptif zorluk.

### P2 — ürün-pazar uyumu görüldükten sonra

- Bil ve Fethet saha modu.
- 5–10 kişilik özel ligler ve haftalık fikstür.
- Canlı 1v1 ve rastgele eşleşme.
- Ball IQ / SKOR IQ: kategori, hız ve zorluğa göre uzun vadeli puan.
- Yeni mekanikler: Higher/Lower, Sırala, Kadro Kur, Kariyer Bul, İpucu İpucu.
- İngilizce dil ve seçilebilir lig/era paketleri.

## Önerilen ürün omurgası

1. **Günlük Maç:** Herkese aynı 5 soru; futbol/basketbol günleri dönüşümlü.
2. **Arkadaş Düellosu:** Mevcut asenkron 1v1; ürünün sosyal çekirdeği.
3. **Stadyum Yolculuğu:** Amatör → Lig → Avrupa → Efsane kariyer ilerlemesi.
4. **Bil ve Fethet:** Bilgiyle saha bölgelerini ele geçiren strateji modu.
5. **SKOR IQ:** Son 30 günlük gerçek performansı özetleyen güvenilir spor bilgisi puanı.

Bu beş katmanın tamamı aynı soru ve oyuncu veri altyapısını kullanmalı; ayrı
uygulamalar gibi büyütülmemelidir.

## Uygulama sırası

| Sıra | Paket | Başarı koşulu |
|---|---|---|
| 1 | Güvenlik P0 | Oyunlar listelenemez, yalnız kodla getirilir; durum geçişleri doğrulanır |
| 2 | Ölçüm | Temel huni ve hata oranı görülebilir |
| 3 | Günlük Maç | Günlük tamamlama, seri ve paylaşım çalışır |
| 4 | Yeni soru mekanikleri | En az Kariyer Bul + Higher/Lower + Sırala |
| 5 | Sosyal büyüme | Deep link, FCM ve özel lig |
| 6 | Güvenilir rekabet | Sunucu skor doğrulama + season leaderboard |

## Bu denetimde uygulanan paket

- Oyun doküman kimliği 6 haneli davet koduna dönüştürüldü; `list` yalnız
  katılımcının `arrayContains` sorgusuna açık, yabancılar yalnız bildikleri açık
  kodu `get` edebilir.
- Davetlere 7 günlük süre eklendi ve çift katılım yarış koşulu transaction ile
  kapatıldı.
- Skor aralığı, oyuncu alanları, soru sayıları ve durum geçişleri Firestore
  kurallarında doğrulandı; penaltıyı yalnız kurucu bitirebilir.
- Taslak/doğrulanmamış sorular istemci erişimine kapatıldı.
- Soru sırasında uygulamadan çıkma o soruyu yanlış sayıyor.
- Firestore emülatöründe yetkisiz okuma, oyun tarama, sahte skor ve penaltı
  manipülasyonu senaryolarını kapsayan otomatik kural testleri eklendi.
- Günün Maçı: Türkiye saatiyle günlük ortak set, tek hak, kişisel seri ve sonuç
  paylaşımı eklendi.
- App Check istemci entegrasyonu (debug/Play Integrity) ve PII içermeyen temel
  Analytics olayları eklendi. Enforcement, Play Console kaydı sonrası açılacak.

## Kesinlikle şimdilik yapılmaması gerekenler

- Sunucu doğrulaması olmadan global para/ödül veya ciddi leaderboard.
- Moderasyon ve kaynak kaydı olmadan kullanıcı üretimli soruları otomatik yayınlama.
- Futbol/basketbol çekirdeği kanıtlanmadan F1, tenis gibi yeni sporlar.
- Çok sayıda para birimi, sandık ve karmaşık ekonomi.
