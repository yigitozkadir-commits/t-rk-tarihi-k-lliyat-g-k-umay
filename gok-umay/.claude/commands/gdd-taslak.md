# /gdd-taslak

Belirtilen oyun için GDD taslağı üret.

## Kullanım
```
/gdd-taslak [oyun adı]
/gdd-taslak [oyun adı] --format=[tam|özet]
```

## Adımlar
1. O oyunla ilgili tüm analiz raporlarını derle
2. arge-reporter'ı çağır
3. GDD Taslağı üret (kural haritası + eksikler + öneriler)

## Çıktı
→ Yapılandırılmış GDD Taslağı (.md)
→ Eksik kural listesi
→ Sonraki AR-GE adımı
