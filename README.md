# OP-Mix

## Environment

Create a Python environment and install the repo dependencies:

```bash
pip install -r requirements-sdft.txt
```

The core training path in `train.py` also imports `hf_olmo` and `olmo_core`, so those packages must be available in the environment you use for OLMo-style training and evaluation.

## Standard training

Example:

```bash
python train.py \
  --model_dir /path/to/model \
  --train_data_file data-mixes/arxiv_train.txt data-mixes/stackexchange_train.txt \
  --train_weights 0.5 0.5 \
  --eval_data_file data-mixes/arxiv_eval.txt data-mixes/stackexchange_eval.txt \
  --output_dir runs/train_example \
  --data_root /path/to/tokenized-data
```

Useful optional flags include `--max_steps`, `--batch_size`, `--seq_len`, `--learning_rate`, `--gradient_accumulation_steps`, `--use_lora`, and `--use_wandb`.

Launch commands should call the tracked Python entrypoints directly, or import modules from `pipeline/` for custom orchestration.

## Standard evaluation

Example:

```bash
python eval.py \
  --model_dir /path/to/model-or-checkpoint \
  --data_file data-mixes/arxiv_eval.txt data-mixes/stackexchange_eval.txt \
  --output_dir runs/eval_example \
  --data_root /path/to/tokenized-data
```

`eval.py` also supports linear mode connectivity evaluation by passing `--model_b`.

## SDFT training

Example:

```bash
export SDFT_DATA_ROOT=/path/to/sdft-data

python train_sdft.py \
  --output_dir runs/sdft_example \
  --model_name Qwen/Qwen2.5-7B-Instruct \
  --train_domains tooluse_data medical_data \
  --train_weights 0.5 0.5
```

Optional flags include `--sft`, `--use_lora`, `--max_steps`, `--num_train_epochs`, and `--no_eval`.

## SDFT evaluation

Example:

```bash
export SDFT_DATA_ROOT=/path/to/sdft-data

python eval_sdft.py \
  --model_a Qwen/Qwen2.5-7B-Instruct \
  --model_b /path/to/checkpoint \
  --eval_domains medical_data science_data tooluse_data \
  --alphas 0.0 0.5 1.0 \
  --output_dir runs/lmc_sdft_example
```
