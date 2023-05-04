# Lamini API

Lamini API is an API designed to build Language Learning Models (LLMs) for natural language processing tasks. It makes the Lamini engine available for creating and running your own LLMs. With Lamini API, you can train language models on large text corpora and improve them following your guidelines, which can then be used for generating and extracting text.

## Running the LLM

```
POST https://api.lamini.ai/v1/llama/run_program
```

Runs a lamini program.

Example Request

```curl
curl https://api.lamini.ai/v1/llama/run_program \
    -H "Content-Type: application/json"
    -H "Authorization: Bearer $LAMINI_API_KEY" \
    -d '{
            "input_value": {
                "data": {
                    "text": "Brainstorm 20 compelling headlines for a Facebook ad promoting the Best Business Financing Options for [Business Owners]. Format the output as a table.",
                },
                "type": {
                    "title": "UserQuery",
                    "type": "object",
                    "properties": {
                        "text": {
                            "description": "what is the user's intent",
                            "type": "string"
                        }
                    },
                }
            },
            "output_type": {
                "title": "ThoughtOutput",
                "type": "object",
                "properties": {
                    "has_url": {
                        "description": "whether the user's intent contains a URL. Boolean: true or false",
                        "type": "string"
                    },
                    "process": {
                        "description": "step by step reasoning process",
                        "type": "string"
                    },
                    "thought": {
                        "description": "final thought",
                        "type": "string"
                    },
                },
            }
        }'
```

Example Response

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

```
POST https://api.lamini.ai/v1/llama/train
```

Trains a lamini program.

Example Request

```curl
curl https://api.lamini.ai/v1/llama/run_program \
    -H "Content-Type: application/json"
    -H "Authorization: Bearer $LAMINI_API_KEY" \
    -d '{
            "input_type": {
                "title": "UserQuery",
                "type": "object",
                "properties": {
                    "text": {
                        "description": "what is the user's intent",
                        "type": "string"
                    }
                },
            },
            "output_type": {
                "title": "ThoughtOutput",
                "type": "object",
                "properties": {
                    "has_url": {
                        "description": "whether the user's intent contains a URL. Boolean: true or false",
                        "type": "string"
                    },
                    "process": {
                        "description": "step by step reasoning process",
                        "type": "string"
                    },
                    "thought": {
                        "description": "final thought",
                        "type": "string"
                    },
                },
            }
            "data": [{
                "UserQuery": {
                    "text": "Brainstorm 20 compelling headlines for a Facebook ad promoting the Best Business Financing Options for [Business Owners]. Format the output as a table.",
                },
                "ThoughtOutput": {
                    "has_url": false,
                    "process": "I need to brainstorm 20 headlines|there is no url|thus",
                    "thought": "I need to write"
                }
            }, ...
            ],
        }'
```

Example Response

```json
{
    "model": "2223b00361a396177a9cb410ff61f20015ad",
    "created_at": 1614812345,
    "status": "RUNNING"
}
```
