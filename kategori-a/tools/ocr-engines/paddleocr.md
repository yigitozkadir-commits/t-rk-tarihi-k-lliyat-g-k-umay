# PaddleOCR — Çok Dilli OCR Motoru

## Repo
github.com/PaddlePaddle/PaddleOCR
Lisans: Apache 2.0 | Kurum: Baidu | Yıldız: 76.000+ | Aktif: Evet (2026)

## Ne Yapar
Tesseract'ı geride bırakarak GitHub'ın en çok yıldızlanan OCR projesi.
111 dil desteği, tablo/formül/grafik tanıma, PP-OCRv5 modeli.

## Gök Umay İçin Değeri
- Arapça (sağdan sola) için güçlü destek
- Osmanlıca/Farsça baskı metinlerde Kraken alternatifi
- Uyghurca, Kiril Kazakça → 111 dil kapsamında
- Tablo yapıları: Osmanlı tahrir defterlerindeki cetvel okuma
- PaddleOCR-VL-1.5: görsel belge anlama, minyatürle birlikte metin

## Kurulum
```bash
pip install paddlepaddle paddleocr
```

## Temel Kullanım
```python
from paddleocr import PaddleOCR

ocr = PaddleOCR(lang='arabic', use_angle_cls=True)
result = ocr.ocr('belge.jpg', cls=True)

for line in result[0]:
    bbox, (text, confidence) = line
    print(f"Metin: {text} | Guven: {confidence:.2f}")
```

## Gök Umay Pipeline Entegrasyonu
```python
def belge_tara(goruntu_yolu: str, dil: str = 'arabic') -> list:
    """
    Kraken'in basarisiz oldugu baski metinler icin.
    dil: 'arabic', 'cyrillic', 'en'
    """
    from paddleocr import PaddleOCR
    ocr = PaddleOCR(lang=dil, use_angle_cls=True)
    result = ocr.ocr(goruntu_yolu, cls=True)

    satirlar = []
    for page in result:
        for line in (page or []):
            bbox, (text, conf) = line
            satirlar.append({'metin': text, 'guven': conf, 'konum': bbox})
    return satirlar
```

## Kraken ile Karsilastirma
```
PaddleOCR:  Baski metin, cok dil, hizli kurulum
Kraken:     El yazmasi, ozel model egitimi
Oneri:      Ilk gecis -> PaddleOCR; el yazmasi -> Kraken + eScriptorium
```

## Agent Baglantisi
-> manuscript-reader ikincil OCR motoru olarak
-> script-decoder baski metin tespitinde
