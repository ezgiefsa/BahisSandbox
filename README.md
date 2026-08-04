# BahisSandbox


TEKNOFEST 2026 "Bağımlılıklarla Mücadelede Teknolojik Uygulamalar" projesi BahisKalkanı'nın demo sandbox bileşeni. Final günü jüri demosu bu uygulama üzerinde yapılacak: aynı telefonda sandbox + kalkan kurulu olacak, jüri feed'i kaydırdıkça kalkan bahis gönderilerini canlı yakalayıp kapatacak.

Geliştirici: Ezgi Efsa Güleç

---

## Nedir?

Instagram benzeri sahte sosyal medya uygulaması. Tamamen offline çalışır, gerçek kullanıcı verisi içermez. BahisKalkanı ile birlikte aynı cihazda çalışarak yasadışı bahis içeriklerini kullanıcıya ulaşmadan kapatır.


---

## Özellikler

- **40 gönderi:** %30 bahis içeriği (düz yazım + sansürlü "b0nus"/"ç3vrim" + kupon ve Telegram davet dili), %70 normal içerik
- **Tuzak gönderiler:** "Betül", "alfabetik" gibi bahis kelimesine benzer ama masum içerikler eklendi — yanlış pozitif olmadığını göstermek için
- **Tamamen offline:** Internet izni yok, tüm veriler yerel JSON dosyasından okunuyor
- **BahisKalkanı uyumu:** `semantics(mergeDescendants = true)` ile kartın tüm metni tek düğümde birleşiyor, kalkan kartın tamamını kapatıyor
- **Gerçekçi görünüm:** Renkli profil daireleri, kullanıcı adı baş harfi, beğeni ve yorum ikonları
- **Özel logo ve splash screen**
- **Uygulama adı "SosyalApp":** Bahis kelimesi geçmiyor, kalkan yanlışlıkla kapatmıyor

---

## Nasıl Çalışır?

1. Kullanıcı SosyalApp'i açar, splash screen'de logo görünür
2. Feed ekranı açılır, kullanıcı aşağı kaydırır
3. BahisKalkanı arka planda erişilebilirlik servisi olarak çalışır
4. Bahis içeriği tespit edilince gönderi siyah kapakla kapatılır, uyarı gösterilir
5. Kullanıcı "Yine de göster" butonuna basarak içeriği açabilir

---

## Teknik Detaylar

| Özellik | Değer |
|---|---|
| Dil | Kotlin |
| UI Framework | Jetpack Compose |
| Veri Kaynağı | Yerel JSON (posts.json) |
| Internet İzni | Yok (KVKK uyumlu) |
| Erişilebilirlik | semantics(mergeDescendants = true) |
| minSdk | 24 (Android 7.0+) |
| Feed Yapısı | LazyColumn |
| Gönderi Sayısı | 40 |

---

## Dosya Yapısı

app/src/main/
├── java/com/example/bahissandbox/
│ └── MainActivity.kt # Ana ekran, feed ve gönderi kartı
├── assets/
│ └── posts.json # 40 gönderi verisi
└── res/
├── values/
│ ├── strings.xml # Uygulama adı
│ └── themes.xml # Splash screen teması
└── mipmap/
└── ic_launcher.png # Özel logo


---

## Kalkan Uyumu

BahisKalkanı'nın AccessibilityService'i ekrandaki metinleri okuyarak çalışır. Bu nedenle:

- Gönderi metinleri normal `Text` composable ile çiziliyor (resim/Canvas içine gömülmedi)
- Kart kök bileşenine `Modifier.semantics(mergeDescendants = true)` eklendi
- Kart boyutu minimum 320x220 px üzerinde (kalkan uyarı + buton gösterebiliyor)
- WebView kullanılmadı, tamamen Native Compose

---

## Test Sonuçları

Realme C53 telefonda BahisKalkanı ile birlikte test edildi:

| Test | Sonuç |
|---|---|
| Bahis içerikleri kapatıldı mı? | ✅ Evet |
| Normal içerikler açık bırakıldı mı? | ✅ Evet |
| Sansürlü yazımlar yakalandı mı? (b0nus, ç3vrim) | ✅ Evet |
| Tuzak kelimeler yanlış kapatıldı mı? | ✅ Hayır (doğru davranış) |
| "Yine de göster" butonu çalışıyor mu? | ✅ Evet |
| Uçak modunda çalışıyor mu? | ✅ Evet |

---

## KVKK / Gizlilik

- Hiçbir kullanıcı verisi kaydedilmiyor
- Internet izni yok, hiçbir ağ isteği atılmıyor
- Tüm veriler cihazda kalıyor
- Uçak modunda tam çalışma doğrulandı

---

## İlgili Repolar

- [BahisKalkanı (Ana Repo)](https://github.com/Ebubekir23/BahisKalkani) — Ebubekir Yılmaz
- [Chrome Eklentisi](https://github.com/Aylin-akagndz/bahiskalkani-chrome) — Aylin Akagündüz
