# Tesseract — Klasik OCR Motoru

## Repo
github.com/tesseract-ocr/tesseract
Lisans: Apache 2.0 | Kurum: Google | Yildiz: 64.000+ | Aktif: Evet

## Ne Yapar
20 yilin en cok kullanilan acik kaynak OCR motoru.
116 dil destegi, LSTM mimarisi, her platformda calisir.

## Gok Umay Icin Degeri
- Arapca, Farsca, Kiril: hazirane dil paketleri
- Kazakca, Ozbekce, Tatarca: tesseract-ocr-kaz, uzb, tat
- eScriptorium ve Docling ile entegre — zaten ecosystem'de
- Uygurca (Arap alfabesi): tur/ara modeliyle yakinlastirma

## Kurulum
```bash
apt-get install tesseract-ocr tesseract-ocr-ara tesseract-ocr-tur
apt-get install tesseract-ocr-kaz tesseract-ocr-uzb tesseract-ocr-rus
pip install pytesseract
```

## Temel Kullanim
```python
import pytesseract
from PIL import Image

img = Image.open('belge.jpg')

# Arapca metin
metin_ar = pytesseract.image_to_string(img, lang='ara')

# Turkce metin
metin_tr = pytesseract.image_to_string(img, lang='tur')

# Cok dil (karma metin)
metin_karma = pytesseract.image_to_string(img, lang='ara+tur+fas')
print(metin_karma)
```

## Baglam Bilgisi ile Kullanim
```python
def dil_tespit_ve_oku(goruntu_yolu: str, tahmin_dil: str = None) -> dict:
    """
    Yazı sistemi belirlendikten sonra Tesseract ile oku.
    script-decoder ciktisini kullanir.
    """
    import pytesseract
    from PIL import Image

    DİL_MAP = {
        'arabic':    'ara',
        'persian':   'fas',
        'ottoman':   'tur+ara',  # Osmanli: Turkce+Arapca karisimi
        'russian':   'rus',
        'kazakh':    'kaz',
        'uzbek':     'uzb',
        'mongolian': 'mon',
    }

    img = Image.open(goruntu_yolu)
    lang_kodu = DİL_MAP.get(tahmin_dil, 'ara')

    metin = pytesseract.image_to_string(img, lang=lang_kodu,
                                        config='--oem 3 --psm 6')
    guven = pytesseract.image_to_data(img, lang=lang_kodu,
                                      output_type=pytesseract.Output.DICT)
    return {'metin': metin, 'guven_verisi': guven}
```

## Sinirlar
- El yazmasi: yetersiz (Kraken + eScriptorium kullan)
- Osmanica: dogrudan model yok; ara+tur kombinasyonu kismi calisir
- PaddleOCR'a gore daha yavás ve daha az dil ozellestirmesi

## Agent Baglantisi
-> manuscript-reader baski metin durumlari
-> Docling ile entegre (docling-project.github.io)
