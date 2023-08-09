# Lamini Query Language

Lamini Query Language is a Query Language designed to query Language Learning Models (LLMs) for natural language processing tasks. It provides an engine for creating and running your own LLMs. With LaminiQL, you can train language models on large text corpora and improve them following your guidelines, which can then be used for generating and extracting text.

## Input and output types

First, you want to construct some data types: (1) input types as arguments into the LLM and (2) output types as return values from the LLM.

You can use the `Type` and `Context` classes in the library create them.

For example, you can create a `UserQuery` type as follows:

```python
type UserQuery {
    text: str: "what is the user's intent"
}
```

Each `Type` requires at least one attribute, such as `text` here. They can be anything you would like. It helps to add a third `Context` field to each attribute, with a natural language description of the attribute. That is required to tell the model what you mean by each attribute.

## Running the LLM

```javascript
var { laminiql, buildSchema } = require("laminiql");

var inputSchema = buildSchema(`
    type UserQuery {
        text: str: "Brainstorm 20 compelling headlines for a Facebook ad promoting the Best Business Financing Options for [Business Owners]. Format the output as a table."
    }
`);

var outputSchema = buildSchema(`
    type ThoughtOutput {
        has_url: str: "whether the user's intent contains a URL. Boolean: true or false"
        process: str: "step by step reasoning process"
        thought: str: "final thought"
    }
`);

var defaultValue = {
    has_url: () => {
        return "No";
    },
    process: () => {
        return "";
    },
    thought: () => {
        return "";
    },
};

laminiql({
    inputSchema,
    outputSchema,
    source: "{text: 'Brainstorm 20 compelling headlines for a Facebook ad promoting the Best Business Financing Options for [Business Owners]. Format the output as a table.'}",
    defaultValue,
}).then((response) => {
    console.log(response);
});
```

If you run this with

```bash
node server.js
```

You should see the LaminiQL response printed out:

```json
{
    "ThoughtOutput": {
        "has_url": false,
        "process": "I need to brainstorm 20 headlines|there is no url|thus",
        "thought": "I need to write"
    }
}
```

## Training a LLM

```javascript
var { laminiTrain, buildSchema } = require("laminiql");

var inputSchema = buildSchema(`
    type UserQuery {
        text: str: "name of the animal"
    }
`);

var outputSchema = buildSchema(`
    type ThoughtOutput {
        has_url: str: "whether the user's intent contains a URL. Boolean: true or false"
        process: str: "step by step reasoning process"
        thought: str: "final thought"
    }
`);

laminiTrain({
    inputSchema,
    outputSchema,
    data: [{
        "UserQuery": {
            "text": "Brainstorm 20 compelling headlines for a Facebook ad promoting the Best Business Financing Options for [Business Owners]. Format the output as a table.",
        },
        "ThoughtOutput": {
            "has_url": false,
            "process": "I need to brainstorm 20 headlines|there is no url|thus",
            "thought": "I need to write"
        }
    }, ... ],
}).then((response) => {
    console.log(response);
});
```

If you run this with

```bash
node server.js
```

You should see the LaminiQL response printed out:

```json
{
    "model": "2223b00361a396177a9cb410ff61f20015ad",
    "created_at": 1614812345,
    "status": "RUNNING"
}
```
