# HTR Tool — Agent Bağlantısı

## manuscript-reader bu klasördeki tool'ları kullanır:

```
BELGE TÜRÜ              TOOL            DOSYA
─────────────────────────────────────────────────
Osmanlıca baskı metin   Kraken + Akis   kraken.md
Osmanlıca el yazması    eScriptorium    escriptorium.md
Arapça el yazması       eScriptorium    escriptorium.md
Farsça ta'lik           Kraken          kraken.md
Moğol dikey yazı        Kraken          kraken.md
Kiril metin             TurkicNLP       ../turkic-languages/turkicnlp.md
```

## script-decoder bu klasördeki tool'ları kullanır:

```
GÖREV                   TOOL            DOSYA
─────────────────────────────────────────────────
Yazı sistemi tespiti    TurkicNLP       ../turkic-languages/turkicnlp.md
Sayfa segmentasyonu     Kraken          kraken.md
Model değerlendirme     Akis Dataset    akis-dataset.md
```

## Pipeline Sırası

```
1. Görüntü geldi
   ↓
2. script-decoder → yazı sistemi tespit (TurkicNLP)
   ↓
3. manuscript-reader → Kraken ile segmentasyon
   ↓
4. eScriptorium → HTR uygula
   ↓
5. Metin çıktısı → language-bridge'e gönder
```
