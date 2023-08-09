# Python Library

Lamini Library is a Python package designed to build Language Learning Models (LLMs) for natural language processing tasks. It provides an engine for creating and running your own LLMs. With Lamini, you can train language models on large text corpora and improve them following your guidelines, which can then be used for generating and extracting text.

## Input and output types

First, you want to construct some data types: (1) input types as arguments into the LLM and (2) output types as return values from the LLM.

You can use the `Type` and `Context` classes in the library create them.

For example, you can create a `UserQuery` type as follows:

```python
from llama import Type, Context

class UserQuery(Type):
    text: str = Context("what is the user's intent")
```

Each `Type` requires at least one attribute, such as `text` here. They can be anything you would like. Be sure to add a `Context` field to each attribute, with a natural language description of the attribute. That is required to tell the model what you mean by each attribute.

## Running the LLM

Next, you want to instantiate your LLM engine with `LLM`.

```python
llm = LLM.from_model_name(name="chatbot")
```

Now, you can now run your LLM.

```python
# Define an output type
class ThoughtOutput(Type):
    has_url: str = Context(
        "whether the user's intent contains a URL. Boolean: true or false")
    process: str = Context("step by step reasoning process")
    thought: str = Context("final thought")

user_query = UserQuery(text="Brainstorm 20 compelling headlines for a Facebook ad promoting the Best Business Financing Options for [Business Owners]. Format the output as a table.")
thought_output = llm(user_query, output_type=ThoughtOutput)
```

## Training a LLM

You can train a LLM as follows:

```python
data = load_data()

llm = LLM(name="chatbot")

job = llm.submit_training_job(
    data=data,
)

status = llm.check_job_status(job.id)

```
