PromptFlows are declared in a [[YAML]] file that represents the flow as a Directed Acyclic Graphs (DAGs).

Located in `flow.dag.yaml`

| Variable  | Description                                   |
| --------- | --------------------------------------------- |
| `id`      | Identifier of the prompt flow                 |
| `name`    | Human readable name of the flow               |
| `entry`   | Entry method to start the flow                |
| `sample`  | Sample input for testing                      |
| `inputs`  | Input variables used by the nodes of the flow |
| `outputs` | Output variables from the nodes of the flow   |
| `nodes`   | Individual nodes (steps) of the flow          |

```yaml
$schema: https://azuremlschemas.azureedge.net/promptflow/latest/Flow.schema.json

# Metadata
id: "test_flow"
display_name: "My Test Flow"
description: "Some decription what the flow does"
tags: "list of tags"

# Programming language: "python" or "csharp"
language: "python"

# open scriptfile.py module and execute main() function
entry: "scriptfile:main"

# environment: ???

# Environment variables can be used for parameters and secrets
environment_variables:
    AZURE_OPENAI_API_KEY: ${open_ai_connection.api_key}
    AZURE_OPENAI_ENDPOINT: ${open_ai_connection.api_base}

# Initialization when starting the flow
init:

# sample input
sample:
	inputs:
		question: "what is the meaning of life?"

# input variables for the individual nodes
inputs: 
	topic: # name of the variable
		type: string
		description: The topic to subscribe to
		default: test
		required: true
		secret: false
		value: test

# output variables; result from executing a node
outputs:
    topic: # name of the variable
        type: string
        description: The topic to publish to
        default: test
        required: true
        secret: false
        value: test

# include additional files
additional_includes:
- "../classification/classify_image.py"

# nodes are the individual steps to execute within the flow
nodes:
- name: "test"
    type: python
    source:
	    type: code
	    path: "test.py"
    inputs:
        url: $(inputs.topic) # reference to the variable definition

- name: "llm"
    type: llm
    source:
        type: code
        path: llm.jinja2
    inputs:
        deployment_name: gpt-35-turbo
        max_tokens: 128
        query: ${inputs.query}
     connection: open_ai_connection
     api: chat
```

## Conditions

Nodes can be executed conditionally on a previous step using the `activate` config:
```yaml
- name: default_result
  type: python
  source:
    type: code
    path: default_result.py
  inputs:
    question: ${inputs.question}
  activate:
    when: ${content_safety_check.output}
    is: false
```
