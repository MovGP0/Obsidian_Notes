## Create pipeline
| Pipeline       | Description                               |
| -------------- | ----------------------------------------- |
| `IDataView`    | load training data into memory            |
| Pipeline       | maps the data to vales for the model      |
| `Fit()`        | starts the training of the model          |
| `Save()`       | saves the model to a file                 |
| `ITransformer` | loads the model to memory for predictions |
| `Evaluate()`   | evaluates the model                       |

ML.NET can load models from
- TensorFlow
- ONNX
- Infer.Net
- CNTK
- PyTorch
