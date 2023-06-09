## End to End Speech Summarization

This recipe can be used to build E2E Speech Summarization models using restricted self-attention on the HowTo corpus of instructional videos.

# Data Download and Preparation
HowTo 2000h fbank-pitch features have been released to enable reproduction of this recipe. Please raise data related issues on the How-2 dataset (Github)[https://github.com/srvk/how2-dataset]

You can request the use of this data using our (data request form)[https://docs.google.com/forms/d/e/1FAIpQLSfW2i8UnjuoH2KKSU0BvcKRbhnk_vL3HcNlM0QLsJGb_UEDVQ/viewform]

For ASR and Summarization, please request the data labeled "(audio_2000) fbank+pitch features in Kaldi scp/ark format for 2000 hours"

You will recieve a data download link shortly after you submit the form.
Then you can prepare the data directory by providing your link as follows:


```bash
./run.sh --local_data_opts "--data_url <insert-link-here>"
```

# Two-stage Training

Training is done in two stages, (a) ASR Pretraining, and (b) Summarization fine-tuning

First run ASR pretraining as follows:
The recipe is based on asr1

```bash
local/run_asr.sh --asr_tag asr_pretrain
```
Then run the finetuning on summarization using the previously trained model as the initialization

```bash
./run.sh --asr_tag sum_finetune --asr_args "--init_param exp/asr_asr_pretrain/valid.acc.ave_10best.pth:::ctc"
```

# Results on ASR


## asr_base_conformer_lf_mix

### WER

|dataset|Snt|Wrd|Corr|Sub|Del|Ins|Err|S.Err|
|---|---|---|---|---|---|---|---|---|
|decode_asr_model_valid.acc.best/dev5_test|3016|55215|93.1|4.8|2.1|1.9|8.8|56.7|

### CER

|dataset|Snt|Wrd|Corr|Sub|Del|Ins|Err|S.Err|
|---|---|---|---|---|---|---|---|---|
|decode_asr_model_valid.acc.best/dev5_test|3016|276377|97.1|1.1|1.9|1.9|4.8|56.7|

### TER

|dataset|Snt|Wrd|Corr|Sub|Del|Ins|Err|S.Err|
|---|---|---|---|---|---|---|---|---|
|decode_asr_model_valid.acc.best/dev5_test|3016|82484|94.1|3.5|2.4|2.2|8.0|56.7|



# Results on Summarization

## asr_ft_sum
### SUMM
- Model link: [huggingface](https://huggingface.co/espnet/roshansh_how2_asr_raw_ft_sum_valid.acc)
- ASR config: [./conf/train_sum_conformer_lf.yaml](./conf/train_sum_conformer_lf.yaml)
- Inference config: [./conf/decode_sum.yaml](./conf/decode_sum.yaml)

|dataset|Snt|Wrd|ROUGE-1|ROUGE-2|ROUGE-L|METEOR|BERTScore|
|---|---|---|---|---|---|---|---|
|decode_sum_asr_model_valid.acc.best/dev5_test_sum|2127|69795|60.72|44.7|56.1|29.36|91.53|



Please cite the following paper if you use this recipe:
```Bibtex
@inproceedings{sharma2022speech,
  author={Sharma, Roshan and Palaskar, Shruti and Black, Alan W and Metze, Florian},
  booktitle={ICASSP 2022 - 2022 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP)},
  title={End-to-End Speech Summarization Using Restricted Self-Attention},
  year={2022},
  volume={},
  number={},
  pages={8072-8076},
  doi={10.1109/ICASSP43922.2022.9747320}}
}

```
