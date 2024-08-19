A FromptFlow project is a folder that contains:

- `README.md` and `LICENSE.md` files (optional)
- A [[PromptFlow DAG YAML]] file that declares the flow
- Python scripts to execute (see [[PromptFlow Python Nodes]])
- LLM template files (see [[PromptFlow LLM Nodes]])
- A `requirements.txt` file that is executed by [pip](https://pypi.org/project/pip/) to install Python dependencies
- A `connection.yaml` file to connect to a language model API
- `.promptflow/flow.tools.json` contains tool metadata referenced by `flow.dag.yaml`
