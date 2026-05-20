# eScriptorium — HTR Arayüzü

## Repo
gitlab.com/scripta/escriptorium
Lisans: MIT | Dil: Python/Django | Aktif: Evet (INRIA + OpenITI)

## Ne Yapar
Kraken motorunun kullanıcı dostu web arayüzü.
El yazması yükle → segmente et → tanı → düzelt → dışa aktar.
Model eğitimi ve ince ayar da yapılabilir.

## Gök Umay İçin Değeri
- Osmanlıca modeli aktif geliştiriliyor (OpenITI AOCP Phase II, 2025)
- Arapça el yazması için en iyi açık kaynak platform
- Farsça Timurlu metinler için modeller mevcut
- Ekip çalışması için ideal — birden fazla kullanıcı aynı belgede

## Kurulum Seçenekleri

### Seçenek 1 — Hazır Instance (Hemen Kullan)
```
manuscriptologIA (msIA): escriptorium.fr
simorgh (Maryland Üniv.): escriptorium.umd.edu
Mannheim Üniversitesi kütüphanesi instance'ı
→ Hesap aç, belge yükle, başla. Kod gerekmez.
```

### Seçenek 2 — Docker (Kendi Sunucunda)
```bash
git clone https://gitlab.com/scripta/escriptorium.git
cd escriptorium
docker-compose up -d
# → localhost:8080
```

### Seçenek 3 — Tam Kurulum
```bash
# Python 3.10+, PostgreSQL, Redis gerekli
git clone https://gitlab.com/scripta/escriptorium.git
cd escriptorium
pip install -r requirements.txt
python manage.py migrate
python manage.py runserver
```

## Kullanım Akışı
```
1. Belge yükle (PDF veya görüntü)
2. "Segment" → sayfa düzenini otomatik tanı
3. "Transcribe" → model seç, metni tanı
4. Hataları manuel düzelt (görüntü yanında)
5. Dışa aktar: TXT / ALTO XML / PAGE XML
```

## Gök Umay İçin Önerilen Modeller
```
Osmanlıca baskı:
  → "Ottoman Turkish print" (Transkribus'tan transfer edilebilir)
  → OpenITI AOCP Osmanlıca modeli (2025 sonunda)

Arapça el yazması:
  → OpenITI ACDC modelleri
  → "Arabic Manuscripts" genel modeli

Farsça ta'lik:
  → OpenITI Farsça modelleri
```

## API Entegrasyonu
```python
import requests

ESCRIPTORIUM_URL = "https://escriptorium.umd.edu"  # veya kendi instance

def belge_yukle(dosya_yolu: str, token: str) -> dict:
    """eScriptorium API ile belge yükle"""
    headers = {'Authorization': f'Token {token}'}
    with open(dosya_yolu, 'rb') as f:
        response = requests.post(
            f'{ESCRIPTORIUM_URL}/api/documents/',
            headers=headers,
            files={'file': f}
        )
    return response.json()

def transkripsiyon_indir(belge_id: int, token: str) -> str:
    """Tamamlanan transkripsiyonu indir"""
    headers = {'Authorization': f'Token {token}'}
    response = requests.get(
        f'{ESCRIPTORIUM_URL}/api/documents/{belge_id}/export/',
        headers=headers,
        params={'format': 'text'}
    )
    return response.text
```

## Sınırlar
- Sunucu kurulumu teknik bilgi gerektirir
- Büyük belgeler için işlem süresi uzun
- İnternet bağlantısı gerekli (hazır instance için)

## İlgili Tool
→ tools/htr/kraken.md (motor)
→ tools/htr/akis-dataset.md (Osmanlıca test verisi)

## Agent Bağlantısı
→ manuscript-reader bu tool'u birincil arayüz olarak kullanır
