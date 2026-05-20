---
name: script-decoder
description: |
  Farklı yazı sistemlerini tanır ve çözümler. Osmanlı, Arap, Uygur,
  Moğol dikey, Kiril, Soğd, Brahmi — Türkî halkların kullandığı her alfabe.
  Kullan: "bu hangi yazı sistemi", "bu harfleri tanı"
  Kullan: "Uygur alfabesini nasıl okuyayım"
  Kullan: "bu yazı Osmanlıca mı Arapça mı"
---

# Yazı Sistemi Çözümleyici

## Rolüm
Harfleri tanırım. Tarihin farklı dönemlerinde Türkî halklar
ondan fazla farklı yazı sistemi kullandı.
Hangisi hangi dönemde, hangi coğrafyada, nasıl okunur — bilirim.

## TOKEN KURALI
Genel alfabe tarihi değil — bu belge için spesifik okuma rehberi.

---

## Türkî Halkların Yazı Sistemleri Kronolojisi

```
DÖNEM          YAZI SİSTEMİ           KULLANILAN YER
────────────────────────────────────────────────────────────
6-10. yy       Göktürk (Runik)        Orta Asya bozkırı
8-18. yy       Uygur alfabesi         Orta Asya, İpek Yolu
10-13. yy      Soğd (geçiş)           Orta Asya ticaret
13-20. yy      Arap alfabesi          Osmanlı, Çağatay, tüm Türkler
13. yy→        Moğol dikey            Moğolistan, İç Moğolistan
1928           Latin (Türkiye)        Türkiye
1929-1940      Latin (Orta Asya)      Sovyet Orta Asya
1940-1991      Kiril (Orta Asya)      Sovyet dönemi
1991→          Geçiş dönemleri        Cumhuriyetler bağımsız
```

---

## Yazı Sistemi Tanıma Rehberi

### Osmanlı Arap Alfabesi
```
TANI:
  → Sağdan sola yazılıyor
  → Noktalar harflerin altında/üstünde
  → Harfler birbirine bağlı (kursif)
  → Uzun ünlüler var (elif, vav, ye)
  → Kısa ünlüler yazılmayabilir (harekesiz metin)

ALT TÜRLER:
  Nesih  → Düzgün, okunabilir, çoğu belgede
  Sülüs  → Kalın, resmi belgeler, başlıklar
  Ta'lik → Eğik, İran etkili, edebi metinler
  Divanî → Osmanlı kancelleri, ferman
  Rık'a  → Günlük yazışma, daha serbest

ZORLUKLAR:
  → Nokta eksikliği (ب/پ/ت/ث birbirine karışır)
  → Harekesiz metin → ünlü belirsiz
  → El yazısı varyasyonları çok geniş
  → Dönemin dili değişiyor (15. yy ≠ 19. yy Osmanlıca)
```

### Uygur Alfabesi
```
TANI:
  → Dikey veya yatay (sağdan sola)
  → Soğd alfabesinden türemiş
  → Moğolca'nın da atası
  → Eski Uygurca metinlerde

ZORLUKLAR:
  → Çok az kişi okuyabiliyor
  → Önce uzman desteği al (Transkribus Uygur modeli)
  → Britsh Library'nin Hoernle koleksiyonu referans
```

### Moğol Dikey Yazı
```
TANI:
  → Yukarıdan aşağıya, soldan sağa
  → Sayfada dikey sütunlar
  → İç Moğolistan'da hâlâ aktif
  → Moğolistan'da 1941'de Kiril'e geçildi

OKUMA:
  → Unicode desteği var (U+1800–U+18AF)
  → Online araç: transliteration.eki.ee
  → Fiziksel belge: profesyonel transkripsiyonist
```

### Göktürk Runik
```
TANI:
  → Sağdan sola veya soldan sağa (değişken)
  → Köşeli, çizgi ağırlıklı
  → Taş üzerine kazınmış genellikle
  → Orhun Yazıtları (8. yy) — en önemli örnek

GÖK UMAY BAĞLAMI:
  Oyun içeriği bu yazıda çok az.
  Ama dönem atmosferi için değerli.
  Kül Tigin ve Bilge Kağan yazıtları — savaş değil ama kültür bağlamı.
```

---

## Harf Tanıma Tablosu — Arap/Osmanlı

```
HARF    OSMANLI ADI    MODERN    DİKKAT
  ا      elif           a/e       Uzun ünlü veya başlangıç
  ب      be             b/p       Altında 1 nokta = ب
  پ      pe             p         Altında 3 nokta = پ (Farsça/Osmanlı)
  ت      te             t         Üstünde 2 nokta
  ث      se             s/th      Üstünde 3 nokta
  ج      cim            c/j       Ortada 1 nokta
  چ      çim            ç         Ortada 3 nokta (Farsça/Osmanlı)
  ح      ha             h         Noktasız — dikkat
  خ      hı             h/kh      Üstünde 1 nokta
  د      dal            d         Noktasız
  ر      re             r         Kıvrımlı
  ز      ze             z         Üstünde nokta
  س      sin            s         Üç dişli, noktasız
  ش      şın            ş         Üç dişli, üstte üç nokta
  ع      ayın           '         Yutak sesi — Türkçede yutulur
  غ      ğayın          ğ         Üstünde nokta
  ف      fe             f         Üstünde 1 nokta
  ق      kaf            k/q       Üstünde 2 nokta
  ك      kef            k         Farklı biçim
  گ      gef            g         (Farsça/Osmanlı)
  ل      lam            l         
  م      mim            m         
  ن      nun            n         Üstünde 1 nokta
  و      vav            v/u/ü/o/ö Çok işlevli! Bağlama dikkat
  ه      he             h/e/a     Kelime sonunda ünlü olabilir
  ي/ى    ye             y/i/ı     Çok işlevli
```

---

## Pratik Araçlar

```
GÖRÜNTÜ → METİN:
  Transkribus (transkribus.eu)
  → Osmanlı nesih/ta'lik için iyi
  → Ücretsiz başlangıç

  Google Lens
  → Modern Arapça için iyi
  → Tarihi el yazması için zayıf

  Claude Vision (bu sistem)
  → Genel tanıma + bağlam analizi
  → manuscript-reader ile birlikte kullan

METİN → ANLAM:
  Osmanlıca Sözlük → nisanyanmap.com
  Arapça sözlük → almaany.com, lisaan.net
  Farsça sözlük → dehkhoda.com
  Kazakça → sozdik.kz
  Moğolca → monglish.mn

TRANSLİTERASYON ARAÇLARI:
  Osmanlıca → Latin: ottomanscript.com
  Arap → Latin: transliteration araçları
  Moğol dikey → Kiril: transliteration.eki.ee
```

---

## Çıktı Formatı

```markdown
## 🔤 Yazı Sistemi Analizi

**Tespit Edilen Yazı:** [sistem adı]
**Alt Tür:** [nesih / ta'lik / ...]
**Dönem:** [tahmini]
**Dil:** [Arapça / Osmanlıca / Farsça / ...]
**Okuma Yönü:** [sağdan sola / soldan sağa / yukarıdan aşağıya]

### Tanıma Gerekçesi
[Neden bu yazı sistemi olduğunu düşünüyorum]

### Okuma Güçlükleri
- [...] → önerilen çözüm

### Önerilen Araç
[Transkribus modeli / uzman / Claude Vision]

### Sonraki Adım
→ manuscript-reader: [transkripsiyon için]
→ language-bridge: [çeviri için]
```
