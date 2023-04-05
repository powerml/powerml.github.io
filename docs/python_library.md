# Python library

Llama is a Python package designed to build Language Learning Models (LLMs) for natural language processing tasks. It provides an engine for creating and running your own LLMs. With Llama, you can train language models on large text corpora and improve them following your guidelines, which can then be used for generating and extracting text.

#
# LLM

Class that instantiates the LLM engine.

```python
LLM(name, model_name, config)
```

## Attributes

-   name: `str` - name of the LLM engine instance
-   model_name: `str` (Optional) - name of the base model, defaults to OpenAI's `text-davinci-003`.
-   config: `dict` (Optional) - auth-related parameters, e.g. token

### Example
```python
llm = LLM(name="my_llm_name")

# With optional parameters
llm = LLM(
    name="my_llm_name",
    model_name="chat-gpt",
    config={
        "production": {
            "key": "A very very very long string",
            "url": "https://api.powerml.co",
        }
    },
)

```

## Call

Runs the instantiated LLM engine.

```
llm(input, output_type, input_type)
```

### Parameters

-   input: `<class llama.Type>` - name of the LLM engine instance
-   output_type: `<class llama.Type>` - the type of the output
-   input_type: `<class llama.Type>` (Optional) - the type of the input (also inferred by the engine with `input`, so it is optional)

### Returns

output: `<class 'llama.Type>` - output of the LLM, based on `input`, in the type specified by `output_type`

## Example

```python
llm = LLM(name="my_llm_name")

my_output = llm(my_input, output_type=MyOutputType)
```

#
# Type and Context

Type is a base class to extend for creating a structured type for input or output into the LLM.


## Usage

Type accepts attributes, with specified types and contexts, as follows:

```python
class MyType(Type):
    my_attribute_name: my_type = Context(my_context)
    my_other_attribute_name: my_other_type = Context(my_other_context)
    ...
```

## Attribute names
Attribute names are like key names into a dictionary or (in the pythonic way) a pydantic model.

Type attributes can be `str`, `int`, `float`, or a custom `Type`.

##  Context
`Context` is used in each attribute in `Type` to specify a natural language description of that attribute to the LLM.
### Parameters

- context: `str` - natural language description of each attribute in a custom `Type`

## Example

```python
class Animal(Type):
    name: str = Context("name of the animal")
    n_legs: int = Context("number of legs that animal has")

class Speed(Type):
    speed: float = Context("how fast something can run")

llama_animal = Animal(name="Larry", n_legs=4)
speed = llm(llama_animal, output_type=Speed)
```

#
# llm.add_data

Adds data to the LLM.

```python
llm.add_data(my_data)

llm.add_data([my_data, my_other_data, ...]

llm.add_data([[input_data, output_data], [more_input_data, more_output_data], ...])
```

## Parameters

- data: `<class 'llama.types.type.Type'>` or `List[<class 'llama.types.type.Type'>]` or `List[List[<class 'llama.types.type.Type'>]]` - data structured as `Type`'s, grouped in lists because they're related to each other, and added in bulk through lists. 1-100k data elements are recommended at a time.

## Example

```python
llama_animal = Animal(name="Larry", n_legs=4)
centipede_animal = Animal(name="Cici", n_legs=100)

my_data = [llama_animal, centipede_animal]

dog_animal = Animal(name="Sally", n_legs=4)
dog_speed = Speed(speed=30.0)

my_data.append([dog_animal, dog_speed])

llm.add_data(my_data)
```

#
# llm.improve

Improves the LLM to produce better output, following your natural language criteria.

```python
llm.improve(on, to)
```

## Parameters

- on: `str` - attribute in an output's `Type` to improve on
- to: `str` - natural language description of how to improve the LLM

## Example
```python
llm.improve(on="speed", to="give the average speed, not the max speed")
```

#
## Full example

Start with data

```python
class Animal(Type):
    name: str = Context("name of the animal")
    n_legs: int = Context("number of legs that animal has")

class Speed(Type):
    speed: float = Context("how fast something can run")

llama_animal = Animal(name="Larry", n_legs=4)
centipede_animal = Animal(name="Cici", n_legs=100)

my_data = [llama_animal, centipede_animal]

dog_animal = Animal(name="Sally", n_legs=4)
dog_speed = Speed(speed=30.0)

my_data.append([dog_animal, dog_speed])
```

Instantiate the LLM engine, add data, add improvements (as many as you like), and run the LLM engine.

```python
llm = LLM(name="animal_speeds")

llm.add_data(my_data)
llm.improve(on="speed", to="give the average speed, not the max speed")

speed = llm(llama_animal, output_type=Speed)
```

A common workflow is to run the LLM engine and see issues in the LLM outputs, then add an improve statement and run the LLM engine again.


