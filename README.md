# CogVLM

<p align="center">
⚒️ <a href="https://github.com/THUDM/SwissArmyTransformer" target="_blank">SwissArmyTransformer (sat)</a> • 🐦 <a href="https://twitter.com/thukeg" target="_blank">Twitter</a> 
</p>
<p align="center">
•  📃 <a href="https://arxiv.org/abs/2105.13290" target="_blank">[CogView@NeurIPS 21]</a>  <a href="https://github.com/THUDM/CogView" target="_blank">[GitHub]</a> • 📃 <a href="https://arxiv.org/abs/2103.10360" target="_blank">[GLM@ACL 22]</a> <a href="https://github.com/THUDM/GLM" target="_blank">[GitHub]</a> <br>
</p>

## Introduction

We introduce CogVLM, a powerful open-source visual language foundation model. Different from the popular shallow-align method which maps image features into the input space of language model, CogVLM bridges the gap between the frozen pretrained language model and image encoder by a trainable visual expert module in the attention and FFN layers. As a result, CogVLM enables deep fusion of visual language features without sacrificing any performance on NLP tasks. CogVLM-17B achieves state-of-the-art performance on 9 classic cross-modal benchmarks, including NoCaps, Flicker30k captioning, RefCOCO, RefCOCO+, RefCOCOg, Visual7W, GQA, ScienceQA, VizWiz VQA and TDIUC, and rank the 2nd on VQAv2, OKVQA, TextVQA, COCO captioning, etc., surpassing or matching PaLI-X 55B. Codes and checkpoints are available at Github.

## Inference

```bash
pip install -r requirements.txt
python cli_demo.py --from_pretrained cogvlm-base-224 --version base --english --bf16 --no_prompt
python cli_demo.py --from_pretrained cogvlm-base-490 --version base --english --bf16 --no_prompt
python cli_demo.py --from_pretrained cogvlm-chat --version chat --english --fp16
python cli_demo.py --from_pretrained cogvlm-grounding-base --version base --english --bf16
python cli_demo.py --from_pretrained cogvlm-grounding-generalist --version base --english --bf16
# We also support model parallel inference, which splits model to multiple (2/4/8) GPUs.
torchrun --standalone --nnodes=1 --nproc-per-node=2 cli_demo.py --from_pretrained cogvlm-chat --version chat --english --fp16
```

If you have trouble in accessing huggingface.co, you can add `--local_tokenizer /path/to/vicuna-7b-v1.5` to load the tokenizer.

## Fine-tuning

Start by downloading the [Captcha Images dataset](https://www.kaggle.com/datasets/aadhavvignesh/captcha-images). Once downloaded, extract the contents of the ZIP file.

To create a train/validation/test split in the ratio of 80/5/15, execute the following:

```bash
python scripts/split_dataset.py
```

Kickstart the fine-tuning process with this command:

```bash
bash scripts/finetune_lora.sh
```

To evaluate the performance of your model, use:

```bash
bash scripts/evaluate.sh checkpoints/your_model_path
```

* If you wish to evaluate using the Exponential Moving Average (EMA) checkpoints, rename the ema folder to a unique integer. Subsequently, update the `latest` file located in the checkpoints/model folder with this integer.
* For instance, upon evaluating the 800-ema checkpoint, anticipated results are approximately **96.27%** accuracy. When disregarding letter cases, the accuracy is around **97.13%**.


## Citation
