## Install PromptFlow

Install Minicoda:
```powershell
winget install -e --id Anaconda.Miniconda3
```

Register Minicona in Windows Terminal:

| Setting            | Variable                                                                                                                                                                                                                           |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Name               | `Anaconda PowerShell`                                                                                                                                                                                                              |
| Command line       | `%windir%\System32\WindowsPowerShell\v1.0\powershell.exe -ExecutionPolicy ByPass -NoExit -Command "& '%AppData%\Local\miniconda3\shell\condabin\conda-hook.ps1' ; conda activate '%AppData%\Local\miniconda3'` |
| Starting directory | `%HOMEPATH%`                                                                                                                                                                                                                       |
| Icon               | `%AppData%\Local\miniconda3\Menu\anaconda_powershell_console.ico`                                                                                                                                                                  |

Install [PromptFlow Packages](https://pypi.org/search/?q=promptflow) with PIP:
```bash
pip install promptflow promptflow-tools
```

## Create an empty PromptFlow

```bash
pf flow init --flow "name_of_the_flow" --type chat
pf flow init --flow "name_of_the_flow" --entry "tool_path" --function "tool_function_name" --prompt-template name=value
```

## Exectue a PromptFlow

Executes `scriptfile.py` and calls the `main` method:
```bash
pf flow test --flow 'scriptfile:main' --inputs question="What is the meaning of life?"  --ui
```

Use a [JSON Lines](https://jsonlines.org/) input file:
```bash
pf flow test --flow 'scriptfile:main' --data './inputs.jsonl'
```

Serve a flow locally:
```bash
pf flow serve --source "./flow.foo.yaml"  --port 8088 --host localhost
```

## PromptFlow project

A FromptFlow project is a folder that contains:
- `README.md` and `LICENSE.md` files (optional)
- A [[PromptFlow DAG YAML]] file that declares the flow
- Python scripts to execute (see [[PromptFlow Python Nodes]])
- LLM template files (see [[PromptFlow LLM Nodes]])
- A `requirements.txt` file that is executed by [pip](https://pypi.org/project/pip/) to install Python dependencies
- A `connection.yaml` file to connect to a language model API
- `.promptflow/flow.tools.json` contains tool metadata referenced by `flow.dag.yaml`

## References

- [PromptFlow Documentation](https://microsoft.github.io/promptflow/index.html)
