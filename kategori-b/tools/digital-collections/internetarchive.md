# internetarchive — Archive.org Python Kütüphanesi

## Repo
github.com/jjjake/internetarchive
Lisans: AGPL | Dil: Python | Aktif: Evet (resmi)

## Ne Yapar
archive.org'a programatik erişim.
Arama, metadata çekme, toplu indirme, koleksiyon yönetimi.

## Gök Umay İçin Değeri
Telif hakkı süresi dolmuş oyun tarihi kitaplarının tamamı burada:
Murray (1913), Bell (1969), Falkener (1892), Parlett vb.
Tek komutla indir, doğrudan analiz et.

## Kurulum
```bash
pip install internetarchive
ia configure  # archive.org hesabıyla giriş (ücretsiz)
```

## Gök Umay Öncelikli Kaynaklar
```python
GOK_UMAY_KAYNAKLAR = {
    # Temel oyun tarihi kitapları — TAM METİN ücretsiz
    'historyofchess00murruoft': {
        'baslik': 'Murray — A History of Chess (1913)',
        'oncelik': 1,
        'sayfa_araligi': '342-398',  # Timurlenk bölümü
    },
    'historyofboardga00murr': {
        'baslik': 'Murray — History of Board Games (1952)',
        'oncelik': 1,
        'sayfa_araligi': 'tumu',
    },
    'gamesancientorie00falk': {
        'baslik': 'Falkener — Games Ancient and Oriental (1892)',
        'oncelik': 2,
        'sayfa_araligi': 'tumu',
    },
    'boardtablegamesh00bell': {
        'baslik': 'Bell — Board and Table Games (1969)',
        'oncelik': 2,
        'sayfa_araligi': 'tumu',
    },
}
```

## Temel Kullanım
```python
import internetarchive as ia

# Arama
sonuclar = ia.search_items('subject:"board games" AND subject:"history"')
for sonuc in sonuclar:
    print(sonuc['identifier'])

# Metadata al
item = ia.get_item('historyofchess00murruoft')
print(item.metadata['title'])
print(item.metadata['creator'])
print(item.metadata['date'])

# PDF indir
item.download(files='historyofchess00murruoft.pdf',
              destdir='kaynaklar/')

# Tam metin indir (varsa)
item.download(glob_pattern='*_djvu.txt',
              destdir='kaynaklar/metin/')
```

## Oyun Tarihi Otomatik Tarama
```python
def oyun_tarihi_ara(sorgu: str, dil: str = 'eng') -> list:
    """
    archive.org'da oyun tarihi kaynağı ara.
    Telif hakkı süresi dolmuş kitaplar önce listelenir.
    """
    arama = ia.search_items(
        f'({sorgu}) AND mediatype:texts AND language:{dil}',
        fields=['identifier', 'title', 'creator', 'date', 'downloads']
    )
    
    sonuclar = []
    for item in arama:
        sonuclar.append({
            'id': item['identifier'],
            'baslik': item.get('title', ''),
            'yazar': item.get('creator', ''),
            'yil': item.get('date', ''),
            'indirme': item.get('downloads', 0),
            'url': f"https://archive.org/details/{item['identifier']}"
        })
    
    # İndirme sayısına göre sırala (popüler = güvenilir)
    return sorted(sonuclar, key=lambda x: x['indirme'], reverse=True)

# Murray tam metin indir ve analiz için hazırla
def murray_hazirla(cikti_dizini: str = 'kaynaklar/murray'):
    import os
    os.makedirs(cikti_dizini, exist_ok=True)
    
    item = ia.get_item('historyofchess00murruoft')
    
    # Önce metin versiyonunu dene
    item.download(
        glob_pattern='*_djvu.txt',
        destdir=cikti_dizini
    )
    
    # Yoksa PDF indir
    if not os.listdir(cikti_dizini):
        item.download(
            glob_pattern='*.pdf',
            destdir=cikti_dizini
        )
    
    print(f"Murray indirildi: {cikti_dizini}")
```

## CLI Kullanımı
```bash
# Arama
ia search "shatranj history"

# İndir
ia download historyofchess00murruoft

# Belirli dosya
ia download historyofchess00murruoft historyofchess00murruoft_djvu.txt

# Metadata
ia metadata historyofchess00murruoft
```

## Agent Bağlantısı
→ corpus-builder temel kaynak indirme
→ archive-navigator dijital erişim katmanı
→ document-archaeologist indirilen metni analiz eder
