# DuckDB — Gömülü Analitik Veritabanı

## Repo
github.com/duckdb/duckdb
Lisans: MIT | Dil: C++ (Python dahil) | Yıldız: 32.000+ | Aktif: Evet (v1.5, 2026)

## Ne Yapar
Kurulum gerektirmeyen, süreç-içi analitik SQL veritabanı.
Pandas DataFrame, Parquet, CSV, JSON doğrudan sorgular.
SQLite'ın analitik dünyadaki halefi.

## Gök Umay İçin Değeri
- Külliyat veritabanı: tüm kaynaklar, atıf sayıları, erişim tarihleri tek yerde
- Zotero dışa aktarımını SQL ile sorgula
- OAI-PMH metadata toplamını DuckDB'ye yükle, filtrele
- arxiv/OpenAlex/Semantic Scholar JSON çıktılarını analiz et
- "Hangi oyun için kaç kaynak var?" → tek SQL sorgusu

## Kurulum
```bash
pip install duckdb
```

## Temel Kullanım
```python
import duckdb

con = duckdb.connect("gokumay_kulliyat.duckdb")

# Zotero BibTeX dışa aktarımını içe al
con.execute("""
    CREATE TABLE IF NOT EXISTS kaynaklar AS
    SELECT * FROM read_json_auto('zotero_export.json')
""")

# Oyuna göre kaynak sayısı
con.execute("""
    SELECT 
        tags->>'oyun' AS oyun,
        COUNT(*) AS kaynak_sayisi,
        MIN(CAST(date AS INT)) AS en_eski,
        MAX(CAST(date AS INT)) AS en_yeni
    FROM kaynaklar
    GROUP BY oyun
    ORDER BY kaynak_sayisi DESC
""").df()
```

## Külliyat Yönetimi
```python
import duckdb

DBPATH = "gokumay_kulliyat.duckdb"
con = duckdb.connect(DBPATH)

def kaynak_ekle(kaynak: dict):
    """corpus-builder'dan gelen kaynağı veritabanına kaydet."""
    con.execute("""
        INSERT INTO kaynaklar VALUES (
            ?, ?, ?, ?, ?, ?, ?
        )
    """, [
        kaynak['baslik'], kaynak['yazar'], kaynak['yil'],
        kaynak['oyun_etiketi'], kaynak['dil'], kaynak['erisim_url'],
        kaynak['notlar']
    ])

def bosluk_haritasi() -> dict:
    """
    Hangi oyun için kaç kaynak var, kritik boşluklar nerede?
    corpus-builder.md Boşluk Haritası bölümünü otomatik günceller.
    """
    return con.execute("""
        SELECT 
            oyun_etiketi,
            COUNT(*) AS kaynak_sayisi,
            SUM(CASE WHEN dil = 'tr' THEN 1 ELSE 0 END) AS turkce,
            SUM(CASE WHEN dil = 'ru' THEN 1 ELSE 0 END) AS rusca,
            SUM(CASE WHEN tam_metin = true THEN 1 ELSE 0 END) AS tam_metin
        FROM kaynaklar
        GROUP BY oyun_etiketi
        ORDER BY kaynak_sayisi ASC
    """).df().to_dict('records')
```

## Makale Metadata Analizi
```python
# OpenAlex JSON çıktısını doğrudan sorgula
sonuclar = con.execute("""
    SELECT title, publication_year, cited_by_count
    FROM read_json_auto('openalex_board_games.json')
    WHERE 'Central Asia' IN (
        SELECT unnest(concepts[*].display_name)
    )
    ORDER BY cited_by_count DESC
    LIMIT 20
""").df()
```

## Agent Bağlantısı
→ corpus-builder külliyat depolama ve boşluk analizi
→ research-analyst makale metadata sorgulama
→ citation-manager Zotero verisi analizi
