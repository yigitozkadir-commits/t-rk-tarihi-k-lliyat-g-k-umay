# Kraken — HTR Motoru

## Repo
github.com/mittagessen/kraken
Lisans: Apache 2.0 | Dil: Python | Aktif: Evet

## Ne Yapar
Tarihi el yazmalarını makine ile okunan metne dönüştürür.
Özellikle sağdan sola ve dikey yazı sistemleri için optimize edilmiş.

## Gök Umay İçin Değeri
- Osmanlıca nesih/ta'lik → %97-99 doğruluk (baskı metin)
- Arapça el yazması → %97+ doğruluk
- Farsça ta'lik → iyi destek
- Moğol dikey yazı → segmentasyon desteği var
- Kiril → tam destek

## Kurulum
```bash
pip install kraken

# GPU ile (önerilen büyük belgeler için)
pip install kraken[cuda]
```

## Temel Kullanım
```bash
# 1. Belgeyi segmente et + OCR yap
kraken -i belge.tif cikti.txt segment -bl ocr

# 2. Mevcut modeli indir (Arapça)
kraken get arabic

# 3. Osmanlıca baskı metin için Transkribus modelini kullan
# → eScriptorium arayüzü ile daha kolay (bkz. escriptorium.md)
```

## Python API
```python
from kraken import blla, rpred
from kraken.lib import models
from PIL import Image

# Model yükle
model = models.load_any('path/to/model.mlmodel')

# Görüntü yükle
im = Image.open('belge.jpg')

# Segmentasyon
baseline_seg = blla.segment(im)

# Tanıma
predictions = rpred.rpred(model, im, baseline_seg)
for line in predictions:
    print(line.prediction)
```

## Gök Umay Pipeline'a Entegrasyon
```python
def belge_oku(goruntu_yolu: str, dil: str = 'arabic') -> str:
    """
    El yazmasını oku, ham metin döndür.
    dil: 'arabic', 'ottoman', 'persian', 'mongolian'
    """
    from kraken import blla, rpred
    from kraken.lib import models
    from PIL import Image

    MODEL_MAP = {
        'arabic':   'path/to/arabic_model.mlmodel',
        'ottoman':  'path/to/ottoman_model.mlmodel',
        'persian':  'path/to/persian_model.mlmodel',
    }

    im = Image.open(goruntu_yolu)
    model = models.load_any(MODEL_MAP.get(dil, MODEL_MAP['arabic']))
    seg = blla.segment(im)
    lines = list(rpred.rpred(model, im, seg))
    return '\n'.join(line.prediction for line in lines)
```

## Mevcut Modeller
```
Osmanlıca baskı metin:
  → Transkribus: "Ottoman Turkish print" modeli
  → Akis-Dataset (verimsu) üzerinde eğitilmiş model

Arapça el yazması:
  → OpenITI AOCP modelleri (eScriptorium arayüzü ile)
  → Zenodo: ACDC proje modelleri

Farsça:
  → OpenITI Farsça modelleri

Model indirme:
  kraken list          → mevcut modeller
  kraken get [model]   → indir
```

## Sınırlar
- El yazması için eğitilmiş model gerekli — hazır model her zaman yok
- Harekesiz Arapça metin → belirsizlik artar
- Karmaşık sayfa düzeni → eScriptorium ile birlikte kullan
- Minimum 300 DPI görüntü önerilen

## İlgili Tool
→ tools/htr/escriptorium.md (arayüz)
→ tools/htr/akis-dataset.md (Osmanlıca test)

## Agent Bağlantısı
→ manuscript-reader bu tool'u çağırır
→ script-decoder segmentasyon için kullanır
