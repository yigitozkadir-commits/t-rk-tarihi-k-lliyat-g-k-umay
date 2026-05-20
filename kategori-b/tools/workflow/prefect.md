# Prefect — Modern Python Pipeline Orkestratörü

## Repo
github.com/PrefectHQ/prefect
Lisans: Apache 2.0 | Dil: Python | Yıldız: 16.000+ | Aktif: Evet (2025)

## Ne Yapar
Airflow'dan daha Pythonic, daha az konfigürasyon gerektiren
veri pipeline orkestratörü. Dekoratör tabanlı, yerel çalışır.
Hata takibi, yeniden deneme, önbellek — hepsi Python fonksiyonu olarak.

## Gök Umay İçin Değeri
- Airflow'dan daha kolay başlangıç: sunucu gerektirmeden yerel çalışır
- Külliyat güncelleme pipeline'ı: metha → DuckDB → rapor
- IIIF indirme → Kraken HTR → language-bridge otomasyonu
- Başarısız API çağrılarında akıllı yeniden deneme (@retry)
- Kategori A ↔ B ↔ C pipeline köprüsü

## Kurulum
```bash
pip install prefect
```

## Gök Umay Pipeline
```python
from prefect import flow, task
from prefect.tasks import task_input_hash
from datetime import timedelta

@task(cache_key_fn=task_input_hash, cache_expiration=timedelta(hours=6))
def oai_metadata_cek(arsiv_url: str) -> list:
    """
    OAI-PMH ile metadata çek.
    6 saat cache: aynı URL tekrar çekilmez.
    """
    import requests
    # ... OAI-PMH implementasyonu
    return []

@task(retries=3, retry_delay_seconds=60)
def iiif_goruntu_indir(manifest_url: str, cikti_dir: str) -> list:
    """
    IIIF görüntü indir — başarısız olursa 3 kez tekrar.
    archive.org timeout hatalarına karşı.
    """
    # ... IIIF indirme kodu
    return []

@task
def duckdb_guncelle(yeni_kayitlar: list):
    """Yeni bulunan kaynakları DuckDB'ye ekle."""
    import duckdb
    con = duckdb.connect("gokumay_kulliyat.duckdb")
    for kayit in yeni_kayitlar:
        con.execute("INSERT OR IGNORE INTO kaynaklar VALUES (?)", [str(kayit)])

@task
def bosluk_raporu_yaz(cikti_dosyasi: str = "bosluk_raporu.md"):
    """DuckDB'den boşluk haritasını çek, Markdown rapor yaz."""
    import duckdb
    con = duckdb.connect("gokumay_kulliyat.duckdb")
    df = con.execute("""
        SELECT oyun_etiketi, COUNT(*) AS kaynak_sayisi
        FROM kaynaklar GROUP BY oyun_etiketi ORDER BY kaynak_sayisi
    """).df()

    with open(cikti_dosyasi, 'w') as f:
        f.write("## Külliyat Boşluk Haritası\n\n")
        f.write(df.to_markdown(index=False))

@flow(name="Gök Umay Haftalık Külliyat Güncelleme")
def haftalik_guncelleme():
    """Ana orkestrasyon akışı."""
    ARSIVLER = [
        "https://digital.bodleian.ox.ac.uk/oai/",
        "https://gallica.bnf.fr/OAI/OAI2.php",
    ]

    tum_kayitlar = []
    for arsiv in ARSIVLER:
        kayitlar = oai_metadata_cek(arsiv)
        tum_kayitlar.extend(kayitlar)

    duckdb_guncelle(tum_kayitlar)
    bosluk_raporu_yaz()
    print(f"Tamamlandı: {len(tum_kayitlar)} kayıt işlendi.")

if __name__ == "__main__":
    haftalik_guncelleme()
```

## Airflow ile Karşılaştırma
```
Prefect:  Yerel çalışır, Python-native, kolay başlangıç
Airflow:  Sunucu gerekir, kurumsal ölçek, daha fazla entegrasyon
Öneri:    Başlangıç/orta ölçek → Prefect; büyük ölçek → Airflow
```

## Agent Bağlantısı
→ corpus-builder külliyat güncelleme pipeline orkestratörü
→ archive-navigator otomatik arşiv kontrolü
→ Kategori A–B–C arası köprü pipeline'ı
