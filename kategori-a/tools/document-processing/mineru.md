# MinerU — Belge Icerik Cikarici

## Repo
github.com/opendatalab/MinerU
Lisans: AGPL-3.0 | Kurum: Shanghai AI Lab | Yildiz: 30.000+ | Aktif: Evet (2025)

## Ne Yapar
PDF belgelerden yuksek kaliteli yapilandirilmis veri cikarimi.
Formul, tablo, minyatur, dipnot — hepsini ayirt ederek Markdown/JSON uretir.
PaddleOCR ile entegre, akademik ve arsiv belgelerine optimize.

## Gok Umay Icin Degeri
- Akademik makale PDFleri: Sovyet etnografya dergisi makaleleri
- Tablo cikarimi: tahrir defterlerindeki cetvel yapilarini JSON'a
- Formul/simge tanima: satranc diyagramlari icin kullanisli
- Minyatur-metin ayirimi: Osmanli el yazmalarindaki gorsel/metin bolgeleri
- OpenITI metin arsivindeki PDF versiyonlari icin

## Kurulum
```bash
pip install mineru
# GPU ile (onerilir)
pip install mineru[full]
```

## Temel Kullanim
```python
from mineru.process import process_pdf

# PDF islem
result = process_pdf(
    pdf_path="sovyet_etnografya_1965.pdf",
    output_dir="./cikti",
    output_format="markdown"
)

# Tablo cikarimi
for tablo in result.tables:
    print(f"Tablo: {tablo.to_dict()}")

# Minyatur/gorsel bolgeleri
for gorsel in result.images:
    print(f"Gorsel bolgesi: sayfa={gorsel.page_no}, konum={gorsel.bbox}")
```

## Gok Umay Isleme Pipeline
```python
def sovyet_makale_isle(pdf_yolu: str) -> dict:
    """
    Sovyet etnografya makalelerini yapilandirilmis veriye donustur.
    corpus-builder'a beslemek icin.
    """
    from mineru.process import process_pdf

    result = process_pdf(pdf_yolu, output_format="json")

    return {
        'metin': result.full_text,
        'tablolar': [t.to_dict() for t in result.tables],
        'bolumler': result.sections,
        'dil': 'ru',  # Rusca kaynak
    }
```

## Agent Baglantisi
-> corpus-builder Rusca/Arapca akademik makale isleme
-> manuscript-reader tablo ve diyagram cikarimi
