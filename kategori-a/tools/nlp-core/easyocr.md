# EasyOCR — Kullanicı Dostu Cok Dilli OCR

## Repo
github.com/JaidedAI/EasyOCR
Lisans: Apache 2.0 | Kurum: Jaided AI | Yildiz: 25.000+ | Aktif: Evet

## Ne Yapar
80+ dil destekli, GPU/CPU calisabilen kullanimi kolay OCR kutuphanesi.
Derin ogrenme tabanli, bagimsiz kurulabilen model paketi.

## Gok Umay Icin Degeri
- Arapca, Farsca, Rusca hazir model
- Uyghurca Arap alfabesi: 'ug' dil kodu ile
- Kiril yazili Kazakca, Ozbekce: 'kz', 'uz'
- Hizli prototipleme: 2 satir kod ile OCR test
- GPU yoksa da makul performans: arastirma ortaminda pratik

## Kurulum
```bash
pip install easyocr
```

## Temel Kullanim
```python
import easyocr

# Arapca + Ingilizce karma belge
reader_ar = easyocr.Reader(['ar', 'en'])
result = reader_ar.readtext('belge.jpg')

for (bbox, metin, guven) in result:
    if guven > 0.5:
        print(f"Metin: {metin} | Guven: {guven:.2f}")

# Kiril Kazakca
reader_kz = easyocr.Reader(['kz', 'ru'])
result_kz = reader_kz.readtext('kazakca_metin.jpg')
```

## Toplu Isleme
```python
import easyocr
from pathlib import Path

def arsiv_klasoru_tara(klasor: str, diller: list) -> list:
    """
    Bir arsiv klasorundeki tum goruntuleri toplu OCR ile isle.
    diller: ['ar'], ['fa'], ['kz', 'ru'], ['ug', 'ar']
    """
    reader = easyocr.Reader(diller)
    sonuclar = []

    for goruntu in Path(klasor).glob('*.jpg'):
        result = reader.readtext(str(goruntu))
        metin = ' '.join([r[1] for r in result if r[2] > 0.4])
        sonuclar.append({'dosya': goruntu.name, 'metin': metin})

    return sonuclar
```

## Kraken ve PaddleOCR ile Karsilastirma
```
EasyOCR:    Kolay kurulum, makul dogruluk, 80+ dil
PaddleOCR:  Daha yuksek dogruluk, 111 dil, daha karmasık kurulum
Kraken:     El yazmasi, tarihsel belge, model egitimi gerekli
Oneri:      Hizli test -> EasyOCR; uretim -> PaddleOCR; el yazmasi -> Kraken
```

## Agent Baglantisi
-> manuscript-reader hizli on kontrol OCR
-> script-decoder yazı sistemi dogrulama
