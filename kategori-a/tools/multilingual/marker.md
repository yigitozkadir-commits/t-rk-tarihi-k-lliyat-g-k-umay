# Marker — PDF/Belge Isaretleyici

## Repo
github.com/VikParuchuri/marker
Lisans: GPL-3.0 | Kurum: Vik Paruchuri | Yildiz: 21.000+ | Aktif: Evet (2025)

## Ne Yapar
PDF'leri Markdown, JSON ve HTML'e donusturur.
Surya OCR uzerine insadir; duzeni, tablolari, formulleri korur.
Toplu isleme icin optimize edilmis.

## Gok Umay Icin Degeri
- Dijital Osmanli/Arapca PDFlerini Markdown'a cevir
- Cetvel ve tablolar: tahrir/vakif defterleri
- OpenITI metinlerini yapilandirilmis formata donustur
- Toplu islem: yuzlerce arsiv belgesi icin pipeline

## Kurulum
```bash
pip install marker-pdf
```

## Temel Kullanim
```python
from marker.convert import convert_single_pdf
from marker.models import load_all_models

models = load_all_models()

# Tek PDF
full_text, images, metadata = convert_single_pdf(
    "osmanli_risale.pdf",
    models,
    langs=["Arabic"],
    batch_multiplier=2
)

print(full_text[:2000])  # Ilk 2000 karakter

# Tablo koruma
if metadata.get('table_of_contents'):
    for bolum in metadata['table_of_contents']:
        print(f"Bolum: {bolum['title']} | Sayfa: {bolum['page_id']}")
```

## Toplu Arsiv Isleme
```python
import os
from marker.convert import convert_single_pdf
from marker.models import load_all_models

def arsiv_toplu_isle(kaynak_dizin: str, hedef_dizin: str, dil: str = 'Arabic'):
    """
    Arsiv dizinindeki tum PDFleri Markdown'a cevir.
    corpus-builder'a beslemek icin.
    """
    models = load_all_models()
    os.makedirs(hedef_dizin, exist_ok=True)

    for pdf_dosyasi in os.listdir(kaynak_dizin):
        if not pdf_dosyasi.endswith('.pdf'):
            continue
        yol = os.path.join(kaynak_dizin, pdf_dosyasi)
        metin, _, _ = convert_single_pdf(yol, models, langs=[dil])

        cikti_yolu = os.path.join(hedef_dizin, pdf_dosyasi.replace('.pdf', '.md'))
        with open(cikti_yolu, 'w') as f:
            f.write(metin)

    print(f"Tamamlandi: {hedef_dizin}")
```

## Agent Baglantisi
-> corpus-builder OpenITI ve dijital arsiv metinleri icin
-> manuscript-reader Docling alternatifi PDF isleme
