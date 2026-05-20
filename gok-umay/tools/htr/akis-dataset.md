# Akis Dataset — Osmanlıca HTR Test Verisi

## Repo
github.com/verimsu/Akis-Dataset
Lisans: Açık | Dil: Veri seti

## Ne Yapar
Osmanlıca baskı metin için HTR modeli test veri seti.
Derin öğrenme tabanlı Osmanlıca OCR modelinin benchmark'ı.
%88.86 ham, %96.12 normalize, %97.37 birleşik karakter doğruluğu.

## Gök Umay İçin Değeri
- Osmanlıca modelini test etmek için referans
- Kraken veya eScriptorium modelini bu veri setiyle değerlendir
- Arşiv belgelerinin okunabilirliğini ölçmek için

## Kullanım
```python
# Test veri setini yükle ve model doğruluğunu ölç
import os
from pathlib import Path

def model_degerlendir(model, test_dizini: str) -> dict:
    """Akis dataset ile model doğruluğunu ölç"""
    dogru = 0
    toplam = 0
    
    for goruntu in Path(test_dizini).glob('*.jpg'):
        ground_truth = goruntu.with_suffix('.txt').read_text()
        tahmin = model.predict(goruntu)
        
        # Karakter hata oranı
        cer = karakter_hata_orani(ground_truth, tahmin)
        toplam += len(ground_truth)
        dogru += len(ground_truth) - cer
    
    return {
        'dogruluk': dogru / toplam,
        'hedef': '%88+ (Akis benchmark)'
    }
```

## Agent Bağlantısı
→ manuscript-reader Osmanlıca belgelerde bu benchmark'a atıfta bulunur
