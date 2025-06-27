## Function based flow

[Function based flow](https://microsoft.github.io/promptflow/how-to-guides/develop-a-flex-flow/function-based-flow.html)

```python
from promptflow import tool

@tool
def echo(input: str) -> str:
	return input
```

```python
from promptflow.tracing import trace

class Reply(TypedDict):
    output: str

@trace
def my_flow(question: str) -> Reply:
    # flow logic goes here
    pass
```

| Attribute | Description                                          |
| --------- | ---------------------------------------------------- |
| `trace`   | Invocation of the method creates a log entry         |
| `tool`    | Method processes data and may call external services |

## Class based flow

Use a [Class based flow](https://microsoft.github.io/promptflow/how-to-guides/develop-a-flex-flow/class-based-flow.html) when state and metric aggregation is required.

```python
class Reply(TypedDict):
    output: str

class MyFlow:
    def __init__(self, model_config: AzureOpenAIModelConfiguration, flow_config: dict):
      # initialize the model
      self.model_config = model_config
      self.flow_config = flow_config

    def __call__(question: str) -> Reply:
      # excute the flow step
      return Reply(output=output)

    def __aggregate__(self, line_results: List[str]) -> dict:
      # aggregate metrics in the dictionary and return with the updated values
      return {"key": val}
```

Implement wrapper logic for execution:
```python
class MyFlow:
    pass
if __name__ == "__main__":
    flow = MyFlow(model_config, flow_config)
    output = flow(question)
    metrics = flow.__aggregate__([output])
    # check metrics here
```