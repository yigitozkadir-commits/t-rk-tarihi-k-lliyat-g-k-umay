# pandas — Veri Analizi Kütüphanesi

## Repo
github.com/pandas-dev/pandas
Lisans: BSD | Dil: Python | Yıldız: 44.000+ | Aktif: Evet

## Ne Yapar
Python'un standart veri analizi ve manipülasyon kütüphanesi.
CSV, JSON, Excel, SQL, Parquet — tüm tablolar pandas DataFrame'e.

## Gök Umay İçin Değeri
- OpenAlex/Semantic Scholar JSON çıktısını temizle, filtrele
- Zotero dışa aktarımını analiz et: hangi oyun, hangi yıl, hangi dil
- OAI-PMH metadata toplamını tabloya dönüştür
- Atıf sayılarına göre sırala, boşluk haritası oluştur
- DuckDB ile entegre: pandas ↔ DuckDB arası geçiş sorunsuz

## Kurulum
```bash
pip install pandas openpyxl
```

## Külliyat Analizi
```python
import pandas as pd
import json

def openalex_json_isle(json_dosyasi: str) -> pd.DataFrame:
    """
    OpenAlex arama sonucunu analiz için hazırla.
    Makale tarama → atıf analizi → boşluk tespiti.
    """
    with open(json_dosyasi) as f:
        veriler = json.load(f)

    df = pd.DataFrame(veriler)
    df['yazar_listesi'] = df['authorships'].apply(
        lambda x: ', '.join([a['author']['display_name'] for a in x])
        if isinstance(x, list) else ''
    )
    df['ac_erisim'] = df['open_access'].apply(
        lambda x: x.get('is_oa', False) if isinstance(x, dict) else False
    )
    df = df[['title', 'yazar_listesi', 'publication_year',
             'cited_by_count', 'ac_erisim', 'doi']]
    df = df.sort_values('cited_by_count', ascending=False)
    return df

def zotero_analiz(zotero_csv: str) -> dict:
    """
    Zotero dışa aktarımından külliyat istatistikleri.
    corpus-builder Boşluk Haritası'nı otomatik günceller.
    """
    df = pd.read_csv(zotero_csv)

    return {
        'toplam_kaynak': len(df),
        'dile_gore': df['Language'].value_counts().to_dict(),
        'ture_gore': df['Item Type'].value_counts().to_dict(),
        'yila_gore': df['Publication Year'].value_counts()
                        .sort_index().to_dict(),
        'etikete_gore': df['Manual Tags'].str.split(';')
                           .explode().str.strip()
                           .value_counts().head(20).to_dict(),
    }

def bosluk_haritasi_raporu(df: pd.DataFrame, oyunlar: list) -> pd.DataFrame:
    """
    Her oyun için kaynak durumu tablosu.
    'Buga Shadra: 0 kaynak' gibi kritik boşluklar.
    """
    satirlar = []
    for oyun in oyunlar:
        filtreli = df[df['Manual Tags'].str.contains(oyun, na=False)]
        satirlar.append({
            'Oyun': oyun,
            'Toplam': len(filtreli),
            'TR': len(filtreli[filtreli['Language'] == 'tr']),
            'EN': len(filtreli[filtreli['Language'] == 'en']),
            'RU': len(filtreli[filtreli['Language'] == 'ru']),
            'Tam Metin': len(filtreli[filtreli['URL'].notna()]),
        })
    return pd.DataFrame(satirlar).sort_values('Toplam')
```

## Agent Bağlantısı
→ corpus-builder külliyat istatistik ve analiz
→ research-analyst makale tarama sonucu işleme
→ citation-manager Zotero verisi analizi
