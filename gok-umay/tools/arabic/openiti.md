# OpenITI — İslamat Metinleri Külliyatı

## Repo
github.com/OpenITI/RELEASE
Lisans: CC BY 4.0 | Kurum: Maryland, AKU, Hamburg | Aktif: Evet

## Ne Yapar
Arapça, Farsça ve Osmanlıca premodern İslamat metinlerinin
makine ile işlenebilir açık külliyatı.
Satranç risaleleri, saray kayıtları, seyahatnameler — dijital formatta.

## Gök Umay İçin Değeri
- al-Adli (9. yy), al-Suli (10. yy) satranç metinleri dijital mevcut
- Aranabilir, makine ile işlenebilir format
- CAMeL Tools ile doğrudan entegre

## Erişim
```bash
# GitHub'dan tam release indir (büyük!)
git clone https://github.com/OpenITI/RELEASE.git

# Sadece belirli metin için:
# openiti.org/projects/OpenITI%20Corpus.html üzerinden ara
# → Metin ID'si al → GitHub'dan doğrudan dosya indir

# Python ile
pip install openiti
```

## Temel Kullanım
```python
from openiti.helper.ara import normalize_ara

# Metin yükle (indirilmiş OpenITI dosyasından)
with open('0255Jahiz.Hayawan.JK000001-ara1', 'r') as f:
    metin = f.read()

# Normalize et
normalized = normalize_ara(metin)

# Satranç terimi ara
import re
ARAMA = ['شطرنج', 'الشطرنج', 'شطرنج']
for terim in ARAMA:
    eslesme = re.findall(f'.{{50}}{terim}.{{50}}', metin)
    for e in eslesme:
        print(f"Bağlam: {e}\n")
```

## Gök Umay İçin Öncelikli Metinler
```
SATRANÇ RİSALELERİ:
  al-Adli     → 0228Adli.Shatranj (eğer mevcut)
  al-Suli     → 0335Suli.Shatranj (eğer mevcut)
  al-Lajlaj   → 0360Lajlaj.Shatranj (eğer mevcut)

GENEL BAŞVURU (mevcut metinler):
  İbn Battuta seyahatnamesi → oyun referansları var
  Biruni eserleri → Orta Asya oyunları üzerine
  Makrizi → Mısır saray oyunları

ARAMA STRATEJİSİ:
  openiti.org arama motorunu kullan:
  → "شطرنج" (shatranj)
  → "النرد" (nard/tavla)
  → "اللعب" (la'ib/oyun)
```

## openiti.org Arama Motoru
```
URL: openiti.org
Arama kutusu → Arapça terim gir
Sonuçlar: metin adı + bağlam paragrafı
Tıkla: tam metin görüntüle veya indir

Gök Umay için yapılacak:
  1. "شطرنج" ara → tüm satranç referanslarını bul
  2. Her referans için tarih + yazar not al
  3. corpus-builder'a ekle
```

## Agent Bağlantısı
→ corpus-builder birincil Arapça/Farsça kaynak tabanı
→ document-archaeologist dijital metin analizi
→ CAMeL Tools ile birlikte kullan (bkz. arabic/camel-tools.md)
