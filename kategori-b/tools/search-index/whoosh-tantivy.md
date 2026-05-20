# Tantivy / Whoosh — Hafif Tam Metin Arama

## Repo
github.com/quickwit-oss/tantivy  (Rust, Python binding: tantivy-py)
github.com/whoosh-community/whoosh  (saf Python)
Tantivy Yıldız: 10.000+ | Whoosh: 1.500+

## Ne Yapar
Elasticsearch kadar ağır bir sunucu gerektirmeden
yerel, dosya tabanlı tam metin arama indeksi.
Araştırma ortamı için: dizüstü bilgisayarda çalışır.

## Gök Umay İçin Değeri
- Elasticsearch olmadan külliyat arama: yerel, hızlı, kurulum yok
- Murray + Bell + Falkener metinlerini indeksle, anında ara
- Sahada: internet bağlantısı olmadan külliyat arama
- DuckDB ile birlikte: SQL + tam metin arama kombinasyonu

## Kurulum
```bash
# Tantivy (Rust tabanlı, hızlı)
pip install tantivy

# Whoosh (saf Python, daha kolay)
pip install whoosh
```

## Whoosh ile Külliyat Arama
```python
from whoosh import index, fields, qparser
from pathlib import Path

INDEKS_DIZINI = "gokumay_indeks"

def indeks_olustur():
    """Külliyat metin dosyaları için Whoosh indeksi kur."""
    schema = fields.Schema(
        dosya_id=fields.ID(stored=True, unique=True),
        baslik=fields.TEXT(stored=True),
        metin=fields.TEXT(stored=False),
        yazar=fields.TEXT(stored=True),
        yil=fields.NUMERIC(stored=True),
        oyun=fields.KEYWORD(stored=True, commas=True),
        dil=fields.KEYWORD(stored=True),
    )
    Path(INDEKS_DIZINI).mkdir(exist_ok=True)
    return index.create_in(INDEKS_DIZINI, schema)

def metin_ekle(idx, kayitlar: list):
    """Kaynak metinleri toplu indeksle."""
    writer = idx.writer()
    for k in kayitlar:
        writer.add_document(
            dosya_id=k['id'],
            baslik=k['baslik'],
            metin=k['metin'],
            yazar=k['yazar'],
            yil=k['yil'],
            oyun=k['oyun'],
            dil=k['dil'],
        )
    writer.commit()

def kulliyat_ara(sorgu_metni: str, filtre_dil: str = None) -> list:
    """
    Tüm külliyatta tam metin arama.
    'şatranc kuralları' → Murray, Bell, risaleler...
    """
    idx = index.open_dir(INDEKS_DIZINI)
    with idx.searcher() as searcher:
        parser = qparser.MultifieldParser(
            ['baslik', 'metin'], idx.schema
        )
        q = parser.parse(sorgu_metni)

        if filtre_dil:
            from whoosh.query import Term, And
            q = And([q, Term('dil', filtre_dil)])

        sonuclar = searcher.search(q, limit=20)
        return [{'baslik': r['baslik'], 'yazar': r['yazar'],
                 'yil': r['yil'], 'id': r['dosya_id']}
                for r in sonuclar]
```

## Agent Bağlantısı
→ corpus-builder yerel külliyat arama altyapısı
→ research-analyst internet gerektirmeyen kaynak tarama
