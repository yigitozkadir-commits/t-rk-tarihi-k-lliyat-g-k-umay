# Apache Airflow — İş Akışı Orkestratörü

## Repo
github.com/apache/airflow
Lisans: Apache 2.0 | Dil: Python | Yıldız: 38.000+ | Aktif: Evet

## Ne Yapar
Veri pipeline'larını zamanlama, izleme ve orkestre etme platformu.
"Her Pazartesi arşivleri tara, Perşembe külliyatı güncelle" gibi
otomatik, hata toleranslı iş akışları.

## Gök Umay İçin Değeri
- Haftalık otomatik OAI-PMH tarama: yeni belgeler otomatik algılanır
- Yeni OpenAlex/Semantic Scholar makaleleri günlük kontrol
- Başarısız indirmeler otomatik yeniden deneme (archive.org timeout)
- Külliyat boşluk raporu her Cuma otomatik oluşturma
- Kategori A → Kategori B → Kategori C iş akışı bağlantısı

## Kurulum
```bash
pip install apache-airflow
airflow db init
airflow webserver &
airflow scheduler &
```

## Gök Umay DAG Örnekleri
```python
from airflow import DAG
from airflow.operators.python import PythonOperator
from datetime import datetime, timedelta

with DAG(
    dag_id='gokumay_haftalik_arsiv_tarama',
    schedule_interval='@weekly',
    start_date=datetime(2026, 1, 1),
    catchup=False,
    default_args={'retries': 3, 'retry_delay': timedelta(minutes=5)},
) as dag:

    def oai_tara():
        """metha veya requests ile OAI-PMH tarama."""
        from tools.web_harvesting import oai_pmh_cek
        arsivler = [
            "https://digital.bodleian.ox.ac.uk/oai/",
            "https://gallica.bnf.fr/OAI/OAI2.php",
        ]
        for arsiv in arsivler:
            kayitlar = oai_pmh_cek(arsiv)
            print(f"{arsiv}: {len(kayitlar)} kayıt")

    def openalex_yenileri_kontrol():
        """Bu hafta yayımlanan oyun tarihi makaleleri."""
        import requests
        from datetime import date, timedelta
        bugun = date.today()
        gecen_hafta = bugun - timedelta(days=7)
        r = requests.get("https://api.openalex.org/works", params={
            'search': 'board games history Central Asia',
            'filter': f'from_publication_date:{gecen_hafta}',
            'per_page': 50,
        })
        yeniler = r.json().get('results', [])
        print(f"Bu hafta {len(yeniler)} yeni makale")

    def bosluk_raporu_olustur():
        """Külliyat boşluk haritasını güncelle."""
        pass  # DuckDB sorgusu + Markdown rapor

    t1 = PythonOperator(task_id='oai_tara', python_callable=oai_tara)
    t2 = PythonOperator(task_id='openalex_kontrol',
                        python_callable=openalex_yenileri_kontrol)
    t3 = PythonOperator(task_id='bosluk_raporu',
                        python_callable=bosluk_raporu_olustur)

    t1 >> t2 >> t3
```

## Agent Bağlantısı
→ corpus-builder otomatik külliyat güncelleme scheduler'ı
→ archive-navigator periyodik arşiv kontrolü
→ research-analyst yeni makale uyarıları
