# Gök Umay Editör — Tool Ekosistemi

## Nasıl Kullanılır
Agent dosyaları bu klasöre atıfta bulunur.
Yeni repo eklendiğinde sadece ilgili tool dosyası güncellenir.
Agent dosyalarına dokunmak gerekmez.

## Klasör Yapısı

```
tools/
  htr/                  → El yazması okuma (Kraken, eScriptorium)
  turkic-languages/     → 24 Türkî dil (TurkicNLP, kaznlp, ...)
  arabic/               → Arapça NLP (CAMeL Tools, OpenITI)
  persian/              → Farsça NLP (Hazm, Parsivar)
  mongolian-russian/    → Moğolca + Rusça (mongolian-nlp, corus)
  corpus/               → Metin külliyatları (OpenITI, ottominer, ...)
```

## Hızlı Referans

| İhtiyaç | Tool | Klasör |
|---------|------|--------|
| El yazması oku | Kraken + eScriptorium | htr/ |
| Yazı sistemi tanı | TurkicNLP | turkic-languages/ |
| Kazakça işle | TurkicNLP + kaznlp | turkic-languages/ |
| Moğolca işle | TurkicNLP + mongolian-nlp | mongolian-russian/ |
| Osmanlıca NLP | TurkicNLP + ottominer | turkic-languages/ |
| Arapça morfoloji | CAMeL Tools | arabic/ |
| Farsça işle | Hazm | persian/ |
| Rusça etnografya | corus | mongolian-russian/ |
| Metin külliyatı | OpenITI + ottominer | corpus/ |
| Uygurca tarihi | historical-uyghur-corpus | corpus/ |

## Agent — Tool Bağlantısı

```
script-decoder      → htr/ + turkic-languages/
manuscript-reader   → htr/ + turkic-languages/
language-bridge     → turkic-languages/ + arabic/ + persian/ + mongolian-russian/
corpus-builder      → corpus/ + arabic/
```
