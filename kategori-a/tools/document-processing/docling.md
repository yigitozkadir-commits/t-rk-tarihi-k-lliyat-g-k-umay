# Docling — Belge Donusturme Pipeline

## Repo
github.com/docling-project/docling
Lisans: MIT | Kurum: IBM Research | Yildiz: 31.000+ | Aktif: Evet (2025)

## Ne Yapar
PDF, Word, taranan belgeleri yapilandirilmis formata donusturur.
Markdown, JSON, sayfa duzenini koruyarak cikti.
Tesseract + Surya OCR ile entegre.

## Gok Umay Icin Degeri
- Osmanli arsiv PDFlerini Markdown'a cevirme
- Sayfa duzenini koruyarak tablo, baslık, paragraf ayirimi
- OpenITI dijital metinleri icin on isleme pipeline'i
- Arapca RTL PDF: Tesseract ara + Docling duzen koruyor

## Kurulum
```bash
pip install docling
```

## Temel Kullanim
```python
from docling.document_converter import DocumentConverter

converter = DocumentConverter()

# PDF'i Markdown'a cevir
result = converter.convert("arsiv_belgesi.pdf")
print(result.document.export_to_markdown())

# Tum sayfalar
for page in result.pages:
    print(f"Sayfa {page.page_no}: {page.export_to_markdown()}")
```

## Gok Umay Pipeline Entegrasyonu
```python
from docling.document_converter import DocumentConverter
from docling.datamodel.pipeline_options import PdfPipelineOptions, TesseractOcrOptions

def arsiv_pdf_isle(pdf_yolu: str, dil: str = 'ara') -> dict:
    """
    Osmanli/Arapca/Farsca arsiv PDFini yapilandirilmis metne cevir.
    """
    pipeline_opts = PdfPipelineOptions()
    pipeline_opts.do_ocr = True
    pipeline_opts.ocr_options = TesseractOcrOptions()

    converter = DocumentConverter()
    result = converter.convert(pdf_yolu)

    return {
        'markdown': result.document.export_to_markdown(),
        'sayfa_sayisi': len(result.pages),
        'tablolar': [t for t in result.document.tables],
    }
```

## Agent Baglantisi
-> manuscript-reader PDF belgeler icin on isleme
-> corpus-builder toplu PDF donusum pipeline'i
