# Python library V2

Lamini Library V2 is a Python package designed to build Language Learning Models (LLMs) for natural language processing tasks. It provides an engine for creating and running your own LLMs. With Lamini, you can train language models on large text corpora and improve them following your guidelines, which can then be used for generating and extracting text.

## Input and output types

First, you want to construct some data types: (1) input types as arguments into the LLM and (2) output types as return values from the LLM.

You can use the `Type` and `Context` classes in the library create them.

For example, you can create a `UserQuery` type as follows:

```python
from llama import Type, Context

class UserQuery(Type):
    text: str = Context("what is the user's intent")

class ThoughtOutput(Type):
    has_url: str = Context(
        "whether the user's intent contains a URL. Boolean: true or false")
    process: str = Context("step by step reasoning process")
    thought: str = Context("final thought")
```

Each `Type` requires at least one attribute, such as `name` and `n_legs` here. They can be anything you would like. Be sure to add a `Context` field to each attribute, with a natural language description of the attribute. That is required to tell the model what you mean by each attribute.

## Running the LLM

Next, you want to instantiate your LLM engine with `LLM`.

```python
llm = LLM(name="animal_stories")

# If you want to use a different base model or add your config options here
llm = LLM(
    name="my_llm_name",
    model_name="chat-gpt",
    config={
        "production": {
            "key": "<API-KEY-HERE>",
        }
    },
)
```

Now, you can now run your LLM.

```python
# Define an output type
class Story(Type):
    story: str = Context("Story of an animal")

llama_animal = Animal(name="Larry", n_legs=4)
llama_story = llm(llama_animal, output_type=Story)
```

## Training a LLM
