# Zero-mt

|Source|Target|CT2 model|Sample|
|:-------------:|:---------------:|:---------------:|:---------------:|
|Traditional Chinese|English|[Huggingface](https://huggingface.co/aarontseng/zero-mt-zh_hant-en)|[translated/zh_hant-en](https://github.com/zape-aat/zero-mt/tree/main/translated/zh_hant-en)

## How to use

```
git lfs install
git clone https://huggingface.co/aarontseng/zero-mt-zh_hant-en
```

```
pip install ctranslate2
pip install sentencepiece
```
## Basic Usage

```
import ctranslate2
import sentencepiece

src_model = sentencepiece.SentencePieceProcessor()
src_model.load("zero-mt-zh_hant-en/source.model")
tgt_model = sentencepiece.SentencePieceProcessor()
tgt_model.load("zero-mt-zh_hant-en/target.model")

translator = ctranslate2.Translator("zero-mt-zh_hant-en", device="cuda")  # "cpu" or "cuda"

encoded_line = src_model.encode_as_pieces("在世界上的許多地方，揮手都是一種表示「你好」的友善手勢」。")

results = translator.translate_batch([encoded_line], batch_type="tokens", max_batch_size=1024)

decoded_line = tgt_model.decode(results[0].hypotheses[0])

print(decoded_line) # In many places around the world, waving is a friendly gesture of "hello".
```

## Batch translation
```
import ctranslate2
import sentencepiece

src_path = "dev.cmn_Hant"
tgt_path = "translated.txt"

src_model = sentencepiece.SentencePieceProcessor()
src_model.load("zero-mt-zh_hant-en/source.model")
tgt_model = sentencepiece.SentencePieceProcessor()
tgt_model.load("zero-mt-zh_hant-en/target.model")

translator = ctranslate2.Translator("zero-mt-zh_hant-en", device="cuda")  # "cpu" or "cuda"

src_file = open(src_path, 'r', encoding="utf-8")
src_lines = src_file.readlines()

encoded_lines = src_model.encode_as_pieces(src_lines)

results = translator.translate_batch(encoded_lines, batch_type="tokens", max_batch_size=1024)
translations = [translation.hypotheses[0] for translation in results]

decoded_lines = tgt_model.decode(translations)

tgt_file = open(tgt_path, "w", encoding="utf-8", newline='')

for line in decoded_lines:
    tgt_file.write(line)
    tgt_file.write('\n')
```
