# Getting Started

## Installation

Llama can be installed using pip, the package manager for Python. To install Llama, open a command prompt and type:

```sh
pip install llama-llm
```

This will download and install the latest version of Llama and its dependencies. Make sure you're on the latest version.

## Setup your keys

Go to [https://powerml.co](https://powerml.co).  Log in to get your API key and purchase credits.

Create `~/.powerml/configure_llama.yaml` and put a key in it.

```sh
production:
    key: "<YOUR-KEY-HERE>"
```

## Import the LLM engine

The LLM class is the core component of the `llama` package. It provides an easy-to-use interface for building, training, and running LLMs.

### Usage of LLM:

To use LLM in your Python code, you first need to import it from the `llama` package. You can do this using the following code:

```python
from llama import LLM
```

Once you have imported the LLM class, you can instantiate a model by providing a name for your LLM, as shown below:

```python
llm = LLM(name="marketing")
```

## Try an example

- [Marketing Copy Generation in Google Colab](https://colab.research.google.com/drive/1Ij5xATu0DDtQNimvhzxyP--ttPO-TFES)

- [Tweet Generation in Google Colab](https://powerml.co/tweet)

