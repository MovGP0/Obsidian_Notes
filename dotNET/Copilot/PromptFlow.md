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

## Deploy a PromptFlow

Build [[Docker]] image:
```bash
# create the docker build file
pf flow build --source ".\mypromptflowfolder" --output ".\dist" --format docker

# build the docker image
docker build --file ".\dist" --tag "mycompany\myflow"

# start a new instance of the Docker image
docker run \
    -p 8080:8080 \
    -e OPEN_AI_CONNECTION_API_KEY="SECRET" \
    -e PROMPTFLOW_WORKER_NUM=4 \
    -e PROMPTFLOW_WORKER_THREADS=4 \
    "mycompany\myflow"

# invoke API
curl http://localhost:8080/score --data '{"url":"https://play.google.com/store/apps/details?id=com.twitter.android"}' -X POST  -H "Content-Type: application/json"
```

## References

- [PromptFlow Documentation](https://microsoft.github.io/promptflow/index.html)
