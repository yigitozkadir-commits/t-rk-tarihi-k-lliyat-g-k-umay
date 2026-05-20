# /cevir

Araştırma metnini Türkçeye çevir, kayıp anlamları işaretle.

## Kullanım
```
/cevir [metin] --kaynak=[AR|FA|RU|MN|KZ|UZ|EN]
/cevir [metin] --seviye=[1|2|3]  (tarama/çalışma/yayın)
/cevir [dosya]
```

## Adımlar
1. language-bridge'i çağır
2. Dil tespiti + dönem analizi
3. Çeviri + terminoloji notları
4. Kayıp anlam uyarıları
5. Oyun AR-GE bağlantısı

## Seviyeler
1 = Hızlı tarama (oyunla ilgili mi?)
2 = Çalışma çevirisi (AR-GE için)
3 = Yayına hazır (akademik)

## Çıktı
→ Türkçe çeviri
→ Terminoloji tablosu
→ Kayıp anlam uyarıları
