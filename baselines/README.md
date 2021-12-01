# Billboard WMT20 ZH-EN/EN-DE data and baselines

Here we provide three transformer baselines for WMT20 ZH-EN and WMT20 EN-DE.

## WMT20 ZH-EN
### Data
- [wmt20-zh-en_bpe32k.tar.gz](https://arkdata.cs.washington.edu/billboard/wmt20-zh-en/data/wmt20-zh-en_bpe32k.tar.gz). We applied [Moses](https://github.com/moses-smt/mosesdecoder) tokenization to English and [Jieba](https://github.com/fxsjy/jieba) and [additional text cleaning](https://github.com/xwgeng/WMT17-scripts) to Chinese. fastBPE with 32K BPE operations are learned from the training data separately for Chinese and English.
- [wmt20-zh-en_bpe32k_data-bin.tar.gz](https://arkdata.cs.washington.edu/billboard/wmt20-zh-en/data/wmt20-zh-en_bpe32k_data-bin.tar.gz). Data binarized with [Fairseq-preprocess](https://github.com/pytorch/fairseq). If you use [fairseq](https://github.com/pytorch/fairseq), **you only need this**.
- [wmt20-zh-en.tar.gz](https://arkdata.cs.washington.edu/billboard/wmt20-zh-en/data/wmt20-zh-en.tar.gz). Raw training data. It is created by concatenating all [WMT20 ZH-EN datasets](https://www.statmt.org/wmt20/translation-task.html). The development set is `newstest2020zhen`. Since newstest2019, source text is all originally written in the source language; this mitigates the translationese effect in evaluation ([Barrault et al., 2019](https://aclanthology.org/W19-5301/)).

### Transformer Baselines
- [trans-base_wmt20-zh-en.tar.gz](https://arkdata.cs.washington.edu/billboard/wmt20-zh-en/models/trans-base_wmt20-zh-en.tar.gz). A transformer base model with a 6-layer encoder and decoder (trained for 14 epochs).
- [trans-large_wmt20-zh-en.tar.gz](https://arkdata.cs.washington.edu/billboard/wmt20-zh-en/models/trans-large_wmt20-zh-en.tar.gz). A transformer large model with a 6-layer encoder and decoder (trained for 14 epochs).
- [trans-large-ensemble_wmt20-zh-en.tar.gz](https://arkdata.cs.washington.edu/billboard/wmt20-zh-en/models/trans-large-ensemble_wmt20-zh-en.tar.gz). 4 transformer large models with different initializations.

## WMT20 EN-DE
### Data
- [wmt20-en-de_bpe32k.tar.gz](https://arkdata.cs.washington.edu/billboard/wmt20-en-de/data/wmt20-en-de_bpe32k.tar.gz). We applied [Moses](https://github.com/moses-smt/mosesdecoder) tokenization to both English and German. fastBPE with 32K BPE operations are learned from the training data **jointly** for English and German.
- [wmt20-en-de_bpe32k_data-bin.tar.gz](https://arkdata.cs.washington.edu/billboard/wmt20-en-de/data/wmt20-en-de_bpe32k_data-bin.tar.gz). Data binarized with [Fairseq-preprocess](https://github.com/pytorch/fairseq). If you use [fairseq](https://github.com/pytorch/fairseq), **you only need this**.
- [wmt20-en-de.tar.gz](https://arkdata.cs.washington.edu/billboard/wmt20-en-de/data/wmt20-en-de.tar.gz). Raw training data. It is created by concatenating all [WMT20 EN-DE datasets](https://www.statmt.org/wmt20/translation-task.html). The development set is `newstest2020ende`.

### Transformer Baselines
- [trans-base_wmt20-en-de.tar.gz](https://arkdata.cs.washington.edu/billboard/wmt20-en-de/models/trans-base_wmt20-en-de.tar.gz). A transformer base model with a 6-layer encoder and decoder (trained for 13 epochs).
- [trans-large_wmt20-en-de.tar.gz](https://arkdata.cs.washington.edu/billboard/wmt20-en-de/models/trans-large_wmt20-en-de.tar.gz). A transformer large model with a 6-layer encoder and decoder (trained for 13 epochs).
- [trans-large-ensemble_wmt20-en-de.tar.gz](https://arkdata.cs.washington.edu/billboard/wmt20-en-de/models/trans-large-ensemble_wmt20-en-de.tar.gz). 4 transformer large models with different initializations.

## Installation
All baselines models are trained with the [fairseq](https://github.com/pytorch/fairseq) library.
Follow their instruction to install the library. We provide an example installation process here.
```bash
git clone https://github.com/pytorch/fairseq
cd fairseq
pip install --editable ./
```
## Get data
```bash
wget https://arkdata.cs.washington.edu/billboard/wmt20-zh-en/data/wmt20-zh-en_bpe32k_data-bin.tar.gz
wget https://arkdata.cs.washington.edu/billboard/wmt20-en-de/data/wmt20-en-de_bpe32k_data-bin.tar.gz
```
## Inference
Generate translations:
```bash
# Single Model
fairseq-generate wmt20-zh-en_bpe32k_data-bin/ --path checkpoint.pt --beam 5 --remove-bpe --lenpen 0.6 > test.out
# Ensemble
fairseq-generate wmt20-zh-en_bpe32k_data-bin/ --path checkpoint1.pt:checkpoint2.pt:checkpoint3:checkpoint4 --beam 5 --remove-bpe --lenpen 0.6 > test.out
```
Postprocess:
```bash
cat test.out | grep -P '^H' | cut -c3- | sort -n -k 1 |uniq | cut -f 3 > test.txt
perl mosesdecoder/scripts/tokenizer/detokenizer.perl <  test.txt >  test.detok.txt
```
## Training
We assume 8-GPU training. Adjust `--update-freq` based on the number of GPUs. For example, `--update-freq 2` for 4-GPU training.
Run training:
```bash
```
## Citation
```
@misc{kasai2021billboard,
    title   = {Bidimensional Leaderboards: Generate and Evaluate Language Hand in Hand},
    author  = {Jungo Kasai and Keisuke Sakaguchi and Ronan Le Bras and Lavinia Dunagan and Jacob Morrison and Alexander R. Fabbri and Yejin Choi and Noah A. Smith},
    year    = {2021},
    url     = {}, 
}
```
