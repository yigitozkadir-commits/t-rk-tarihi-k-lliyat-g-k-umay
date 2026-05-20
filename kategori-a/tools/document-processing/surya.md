# Surya — Cok Dilli Belge OCR

## Repo
github.com/datalab-to/surya
Lisans: GPL-3.0 | Kurum: Vik Paruchuri | Yildiz: 16.000+ | Aktif: Evet

## Ne Yapar
90+ dilde satir duzeyinde metin tespiti ve tanima.
Duzen analizi, okuma sirasi tespiti, tablo tanima.
Tesseract'i dogruluk ve hizda gecen derin ogrenme modeli.

## Gok Umay Icin Degeri
- Arapca sag-sol okuma sirasi: RTL belgeler icin optimize
- Tarihsel belge benchmark'i: tapuscorpus ile test edilmis
- Sayfa segmentasyonu: Osmanlıca risale yapisi analizi
- Duzen analizi: minyatur + metin birlikte belgelerde

## Kurulum
```bash
pip install surya-ocr
```

## Temel Kullanim
```python
from surya.recognition import batch_recognition
from surya.detection import batch_text_detection
from surya.model.detection.model import load_model as load_det_model
from surya.model.recognition.model import load_model as load_rec_model
from PIL import Image

det_model = load_det_model()
rec_model = load_rec_model()

img = Image.open("risale_sayfasi.jpg")
predictions = batch_text_detection([img], det_model)
text_result = batch_recognition([img], [["ar"]], rec_model, predictions)

for line in text_result[0].text_lines:
    print(f"Metin: {line.text} | Guven: {line.confidence:.2f}")
```

## Gok Umay Kullanim Senaryolari
```
Senaryo 1 — Osmanlica risale sayfa analizi:
  Surya -> sayfa segmentasyonu -> Kraken -> HTR

Senaryo 2 — Arapca el yazmasi duzeni:
  Surya -> RTL okuma sirasi tespiti -> eScriptorium

Senaryo 3 — Karisik minyatur + metin sayfasi:
  Surya -> bolge tespiti -> minyatur ayir -> metin bolumune OCR
```

## Agent Baglantisi
-> manuscript-reader sayfa duzeni on analizi
-> script-decoder ile birlikte yazı sistemi + duzen tespiti
