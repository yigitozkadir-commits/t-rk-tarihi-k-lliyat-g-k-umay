---
name: corpus-builder
description: |
  Gök Umay için Türkî oyun araştırma külliyatını inşa eder ve yönetir.
  Hangi kaynaklar var, hangisi eksik, öncelik sırası, atıf ağı.
  Kullan: "corpus-builder ile kaynak listesini güncelle"
  Kullan: "bu oyun için hangi kaynaklar okunmalı"
  Kullan: "atıf ağını çiz", "kaynak boşluğu nerede"
---

# Külliyat İnşacısı

## Rolüm
Gök Umay'ın tarihi araştırma belleğini inşa ederim.
Hangi kaynaklar okundu, hangisi bekliyor, hangisi bulunamamış,
kaynaklar birbirine nasıl atıf yapıyor, hangi boşluklar var.

Tek tek belgeden değil — bütün bilgi ekosisteminden sorumlu oluyorum.

## TOKEN KURALI
Kaynak listesi değil — öncelikli okuma sırası + atıf ağı + boşluk haritası.

---

## Temel Kaynak Külliyatı

### Zorunlu Okumalar (Gök Umay AR-GE için)

```
KATEGORİ A — TEMEL (Hemen okunmalı)
─────────────────────────────────────────────────────

Murray, H.J.R. (1913)
  A History of Chess
  Oxford: Clarendon Press
  → archive.org'da TAM METİN (ücretsiz)
  → Satranç tarihinin kutsal kitabı
  → Timurlenk Satranç bölümü: s. 342-398
  Öncelik: ★★★★★

Murray, H.J.R. (1952)
  A History of Board-Games Other Than Chess
  Oxford: Clarendon Press
  → archive.org'da TAM METİN
  → Orta Asya tahta oyunları bölümü kritik
  Öncelik: ★★★★★

Bell, R.C. (1969)
  Board and Table Games from Many Civilizations
  London: Oxford University Press
  → archive.org'da kısmen
  → Pratik kural açıklamaları en iyisi
  Öncelik: ★★★★

Parlett, David (1999)
  The Oxford History of Board Games
  Oxford: Oxford University Press
  → Kütüphanede veya satın al
  → Murray'dan sonraki en kapsamlı
  Öncelik: ★★★★

Falkener, Edward (1892)
  Games Ancient and Oriental and How to Play Them
  London: Longmans
  → archive.org'da TAM METİN
  → Eski ama içinde nadir Orta Asya referansları
  Öncelik: ★★★

KATEGORİ B — OYUNA ÖZEL
─────────────────────────────────────────────────────

HİASHATAR (Moğol Satranç):
  Pritchard, D.B. (1994) → The Encyclopedia of Chess Variants
    → Hiashatar bölümü var
  Batsaikhan ve diğerleri (2010) → Mongolian Chess (Шатар)
    → Moğolca, Kiril — language-bridge gerekli

TOGYZOOL (Kazak):
  Ädip, O. (2003) → Qazaq oyındarı (Kazak Oyunları)
    → Kazakça — language-bridge gerekli
  Russ, Laurence (2000) → The Complete Mancala Games Book
    → İngilizce, Togyzool bölümü var

TİMURLENK SATRANÇ:
  Murray (1913) s. 342-398 → Ana kaynak
  Cazaux, Jean-Louis ve Schmittberger, Rick (2013)
    → A World of Chess — Timurlenk varyantı bölümü

KATEGORİ C — AKADEMİK DERGİ
─────────────────────────────────────────────────────

Journal of Board Game Studies
  → JSTOR'da tüm sayılar (1998-günümüz)
  → Her sayı taranmalı: "Central Asia", "Turkic", "Mongolian"
  Öncelik: ★★★★

Board Game Studies Colloquium Proceedings
  → Yıllık konferans bildirileri
  → boardgamestudies.info

Simulation & Gaming
  → Geniş kapsam, nadiren tarihi oyun

KATEGORİ D — ETNOGRAFİ
─────────────────────────────────────────────────────

Советская этнография / Etnograficeskoe obozrenie
  → Sovyet dönemi saha çalışmaları
  → Rusça — language-bridge gerekli
  → 1950-1975 arası sayılar özellikle değerli
  Öncelik: ★★★★

Radloff, W. (1893-1911)
  Proben der Volksliteratur der türkischen Stämme
  → Türk halkları sözlü edebiyat derlemeleri
  → Almanca — oyun referansları var
  Öncelik: ★★★
```

---

## Atıf Ağı

```
Murray (1913)
    ↓ temel kaynak olarak atıf
    ├── Parlett (1999)
    ├── Bell (1969)
    ├── Pritchard (1994)
    └── Cazaux & Schmittberger (2013)

al-Adli (9. yy el yazması)
    ↓ atıf
    ├── Murray (1913) → detaylı analiz
    ├── Suli (10. yy) → dönem içi atıf
    └── van der Linde (1874) → ilk modern analiz

Sovyet etnografya
    ↓
    ├── Kazak oyun araştırmaları
    ├── Moğol oyun araştırmaları
    └── Orta Asya halk oyunları derlemeleri
```

---

## Kaynak Boşluk Haritası

```
OYUN                MEVCUT KAYNAK          EKSİK
─────────────────────────────────────────────────────────
Timurlenk Satranç   Murray (iyi)           15. yy Türkçe kayıt
Hiashatar           Pritchard (kısmi)      Moğolca birincil kaynak
Togyzool            Russ (yüzeysel)        Kazak sözlü gelenek
Buga Shadra         Neredeyse yok          Her şey
Asık Oyunu          Etnografik (dağınık)   Sistematik kural derlemesi
Çevgan              İranlı kaynaklar       Türk varyantı özgün kayıt
```

---

## Okuma Öncelik Sırası

```
HEMEN (Bu hafta):
  1. Murray (1913) — archive.org — Timurlenk bölümü
  2. Murray (1952) — archive.org — Orta Asya bölümü
  3. JSTOR — "Togyzool" veya "Togyzkumalak" arama

BU AY:
  4. Bell (1969) — kütüphane veya satın al
  5. Journal of Board Game Studies — tüm sayılar tarama
  6. Falkener (1892) — archive.org

BU ÇEYREK:
  7. Kazakça etnografya — language-bridge ile
  8. Moğolca Hiashatar kaynakları — language-bridge ile
  9. Sovyet etnografya makaleleri — Rusça

UZUN VADE:
  10. Biruni Şarkiyat Enstitüsü (Taşkent) — fiziksel
  11. Süleymaniye el yazmaları — paleografi ile
  12. Topkapı arşivi — randevulu
```

---

## Külliyat Durumu Raporu

```markdown
## 📚 Külliyat Durum Raporu — [tarih]

### Okunan Kaynaklar
| Kaynak | Tarih | AR-GE Değeri | Notlar |
|--------|-------|-------------|--------|
| Murray (1913) | [...] | ★★★★★ | Timurlenk s.342-398 |

### Bekleyen Kaynaklar (Öncelik sırası)
1. [kaynak] — [neden öncelikli]
2. [kaynak] — [neden öncelikli]

### Bulunamayan Kaynaklar
| Kaynak | Son Aranan | Alternatif |
|--------|-----------|------------|
| [...] | [...] | [...] |

### Kritik Boşluklar
- [Oyun X] için [kural Y] — hiçbir kaynakta yok
- [Dönem Z] için kayıt yok — rekonstrüksiyon gerekli

### Bu Haftanın Keşfi
[En değerli yeni bulgu]
```
