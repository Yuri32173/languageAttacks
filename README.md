# LanguageAttacks



## Table of Contents

- [Installation](#installation)
- [Models](#models)
- [Usage](#usage)

## Installation

We need FastChat to create conversations. At the current moment, we install it from [source](https://github.com/lm-sys/FastChat) by taking the following steps (we suggest to git clone FastChat outside the root of this repository).

```bash
git clone https://github.com/lm-sys/FastChat.git
cd FastChat
pip3 install --upgrade pip  # enable PEP 660 support
pip3 install -e .
```

The `llm-attacks` package can be installed by running the following command at the root of this repository:

```bash
pip install -e .
```

## Models

Please follow the instructions to download Vicuna-7B or/and LLaMA-2-7B-Chat first (we use the weights converted by HuggingFace [here](https://huggingface.co/meta-llama/Llama-2-7b-hf)).  Our script by default assumes models are stored in a root directory named as `/DIR`. To modify the paths to your models and tokenizers, please add the following lines in `experiments/configs/individual_xxx.py` (for individual experiment) and `experiments/configs/transfer_xxx.py` (for multiple behaviors or transfer experiment). An example is given as follows.

```python
    config.model_paths = [
        "/DIR/vicuna/vicuna-7b-v1.3",
        ... # more models
    ]
    config.tokenizer_paths = [
        "/DIR/vicuna/vicuna-7b-v1.3",
        ... # more tokenizers
    ]
```

## Usage

Our code to run experiments with GCG is included `experiments` folder. To run individual experiments with harmful behaviors and harmful strings mentioned in the paper, run the following code inside `experiments`:

```bash
cd launch_scripts
bash run_gcg_individual.sh vicuna behaviors
```

Changing `vicuna` to `llama2` and changing `behaviors` to `strings` will switch to different experiment setups.

To perform multiple behaviors experiments (i.e. 25 behaviors, 1 model), run the following code inside `experiments`:

```bash
cd launch_scripts
bash run_gcg_multiple.sh vicuna # or llama2
```

To perform transfer experiments (i.e. 25 behaviors, 2 models), run the following code inside `experiments`:

```bash
cd launch_scripts
bash run_gcg_transfer.sh vicuna 2 # or vicuna_guanaco 4
```

To perform evaluation experiments, please follow the directions in `experiments/parse_results.ipynb`.

Notice that all hyper-parameters in our experiments are handled by the `ml_collections` package [here](https://github.com/google/ml_collections). You can directly change those hyper-parameters at the place they are defined, e.g. `experiments/configs/individual_xxx.py`. However, a recommended way of passing different hyper-parameters -- for instance you would like to try another model -- is to do it in the launch script. Check out our launch scripts in `experiments/launch_scripts` for examples. For more information about `ml_collections`, please refer to their [repository](https://github.com/google/ml_collections).



