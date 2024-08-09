The [Connection YAML](https://microsoft.github.io/promptflow/how-to-guides/develop-a-dag-flow/quick-start.html) is used to connect to an API (ie. a language model).

The definition of the connection is located in `connection.yaml`

Connect to an OpenAI API hosted in Azure:
```yaml
$schema: https://azuremlschemas.azureedge.net/promptflow/latest/AzureOpenAIConnection.schema.json
name: open_ai_connection
type: azure_open_ai
api_key: <test_key>
api_base: <test_base>
api_type: azure
api_version: <test_version>
```

Connect to an OpenAI API hosted on OpenAI:
```yaml
$schema: https://azuremlschemas.azureedge.net/promptflow/latest/OpenAIConnection.schema.json
name: open_ai_connection
type: open_ai
api_key: "APIKEY"
organization: "" # optional
```

## Create connection via Python API

```python
from promptflow.client import PFClient
from promptflow.connections import AzureOpenAIConnection

# https://microsoft.github.io/promptflow/reference/python-library-reference/promptflow-devkit/promptflow.client.html
pf_client = PFClient()

# Connection to an Azure OpenAI model
con_name = "MY_CONNECTION_NAME" # use the name as defined in Azure
try:
    conn = pf_client.connections.get(name = con_name)
except:
    connection = AzureOpenAIConnection(
        name = con_name,
        api_key = "MODEL_API_KEY", # API Key for the model instance
        api_base = "https://XXX.openai.azure.com", # URL for the model endpoint
        api_type = "azure",
        api_version = "2024-01-01-preview"
    )
    conn = pf_client.connections.create_or_update(connection)

print(conn)
```
