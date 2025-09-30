# Implementation of RustTest

This repository is the official implementation of **Multi-Stage Generation of Rust Unit Tests with LLMs**.

You first need to extract replication_package.tar.gz.


## Requirements

To install requirements:

- ```bash
  pip install -r requirements.txt
  ```

## `dataset_construction`

The `code` subdirectory contains the code for constructing experimental data.

1. Crates are located in the `./dataset_construction/crates/` . If you wish to crawl them yourself:

   - ```bash
     python crate_crawler.py
     ```

2. All datasets used for fine-tuning and evaluation are stored in the `./dataset_construction/dataset/dataset/` . If you wish to construct them yourself:

   - ```bash
     python dataset_construction.py
     ```

## `fine-tuning`

The `code` subdirectory contains the code for fine-tuning the model.

1. First, download the desired model (e.g., [CodeLlama-7B](https://huggingface.co/codellama/CodeLlama-7b-hf)) from [Hugging Face](https://huggingface.co/).

2. To fine-tune your model:

   - ```bash
     python finetuning.py --model_path='YourModelPath' 
     ```

3. To merge LoRA adapter weights with the base model:

   - ```bash
     python lora_merge_weight.py \
       --base_model='YourBaseModelPath' \
       --finetune_dir='YourLoRAAdapterPath' \
       --output_path='YourOutputPath'
     ```

## `inferencing`

The `code` subdirectory contains the code for generating unit tests via model inference.

1. To generate unit tests:

   - ```bash
     python inferencing.py --model_path='YourModelPath'
     ```

## `evaluation`

The `code` subdirectory contains the code for evaluating the results.

1. The test dataset is located in the `./evaluation/dataset/test/` .

2. To evaluate **compilation success rate**:

   - ```bash
     python evaluation.py --command="build"
     ```

3. To evaluate **execution success rate**:

   - ```bash
     python evaluation.py --command="modtest"
     ```

4. To evaluate **line coverage**:

   - ```bash
     python line_coverage.py --prefix_absolute="AbsolutePath/evaluation/dataset/crates/"
     ```

5. To evaluate **branch coverage**:

   - ```bash
     python branch_coverage.py --prefix_absolute="AbsolutePath/evaluation/dataset/crates/"
     ```
