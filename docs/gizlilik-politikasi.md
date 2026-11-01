# SKOR! Gizlilik Politikası

> **TASLAK — yayından önce:** `[İLETİŞİM E-POSTASI]` alanını doldur, bu metni
> herkese açık bir adreste yayınla (ör. GitHub Pages / Firebase Hosting) ve
> adresi Play Console > Uygulama içeriği > Gizlilik politikası alanına gir.
> Play Console "Veri güvenliği" formu bu metindeki listeyle uyumlu doldurulmalı.

Son güncelleme: 30 Eylül 2026

SKOR!, arkadaşlarınla spor bilgini kapıştırdığın bir bilgi yarışması
uygulamasıdır. Mümkün olan en az veriyi toplamak için tasarlandı: e-posta,
telefon numarası, gerçek ad, konum veya rehber bilgisi **istemeyiz**.

## Topladığımız veriler

| Veri | Neden | Nerede |
|---|---|---|
| Anonim kullanıcı kimliği (rastgele üretilir) | Oyunlarını sana bağlamak | Firebase Authentication |
| Seçtiğin kullanıcı adı | Rakiplerin seni tanısın | Cloud Firestore |
| Maç kayıtları: sorular, verdiğin cevaplar, süreler, puanlar, kullandığın joker | Asenkron 1v1 oyun ve sonuç ekranı | Cloud Firestore |
| Günün Maçı sonuçları ve serin | Günlük meydan okuma | Cloud Firestore |
| Uygulama kullanım olayları (ör. "oyun tamamlandı", kategori, puan) ve cihaz/uygulama sürümü bilgisi | Uygulamayı iyileştirmek | Google Analytics for Firebase |
| Cihaz bütünlüğü doğrulaması | Sahte istemcileri ve hileyi engellemek | Firebase App Check (Play Integrity) |

Analitik olaylarına kullanıcı adın, davet kodların veya soru metinleri
**eklenmez**.

## Verilerin paylaşılması

- **Rakiplerin**, birlikte oynadığınız maçlarda kullanıcı adını, cevaplarını
  ve puanını görür.
- Veriler Google'ın Firebase altyapısında (Avrupa bölgesi) saklanır. Google,
  bu hizmetleri bizim adımıza sağlayan veri işleyendir.
- Verilerini satmayız, reklam için kullanmayız, üçüncü taraflarla
  pazarlama amacıyla paylaşmayız.

## Saklama ve silme

- Uygulamada **Profil > Hesabımı sil** ile hesabını istediğin an
  silebilirsin. Bu işlem kullanıcı profilini, Günün Maçı geçmişini ve serini
  kalıcı olarak siler ve anonim hesabı kapatır.
- Tamamlanmış maçlar rakibinin geçmişinin de parçası olduğundan, o maç
  kayıtlarındaki kullanıcı adın ve skorun rakibinin maç listesinde kalır.
  Bu kayıtların da silinmesini istersen bize yaz.
- Uygulamayı kaldırmak hesabı silmez; anonim hesaba yeniden erişilemez hale
  gelir.

## Çocuklar

SKOR! 13 yaş altındaki çocuklara yönelik değildir ve bilerek bu yaş
grubundan veri toplamaz.

## Güvenlik

Veriler aktarım sırasında şifrelenir (HTTPS/TLS). Firestore güvenlik
kuralları, her kullanıcının yalnızca kendi verisine ve kendi maçlarına
erişmesini sağlar.

## Değişiklikler

Bu politikayı güncellersek tarihi yukarıda değiştiririz; önemli
değişiklikleri uygulama içinde duyururuz.

## İletişim

Sorular ve silme talepleri için: `[İLETİŞİM E-POSTASI]`
