## Overview

| Product                                                      | Description                                        |
| ------------------------------------------------------------ | -------------------------------------------------- |
| [Copilot Studio](https://copilotstudio.microsoft.com/)       | Low-Code Copilot Orchestration in Office 365       |
| [Azure AI Studio](https://ai.azure.com/)                     | Pro-Code Copilot Orchestration for Microsoft Azure |
| [Prompt Flow](https://github.com/microsoft/promptflow)       | Orchestration using VS Code                        |
| [AI Toolkit](https://github.com/microsoft/vscode-ai-toolkit) | Execute Models in VS Code                          |

## Basic Concepts

| Concept      | Description                                                                                 |
| ------------ | ------------------------------------------------------------------------------------------- |
| Topic        | Topic a conversation is about; groups a conversation flow                                   |
| Action       | Invokes an action via an API; input an outputs are in JSON                                  |
| Entities     | Describes data types for the generative AI; used to describe the data fields of the Actions |
| Flow         | Prompt flow definition                                                                      |
| Run          | Executions of a prompt flow                                                                 |
| Connection   | Integration with data sources and model instances                                           |
| Runtime      | Docker image that executes flows                                                            |
| Vector index | Connection with a vector database                                                           |
| Compute      | Virtual machine that hosts the docker image                                                 |
| Serp         | Google Search API (Google, Bing)                                                            |

## Copilot Studio

- Integrated into Office 365
- Low-Code environment
- Document retrieval from Office 365 and Web

## Azure AI Studio

- Azure hosted AI Models 
	- ie. for Speech and Image processing
- Custom data sources and APIs
	- Azure AI search
	- Document and relational databases
- Control over System Prompts and AI Model parameters
- Support for custom Orchestrators (Agents)
	- LangChain (implementation of Agents in Python)
	- [[Semantic Kernel]] (implementation of Agent in .NET)
	- AutoGen (Orchestration of multiple Agents)

## Prompt Flow

- Create Orchestration in VS Code

## AI Toolkit (BETA)

- Previously named **Windows AI Studio**
- Locally executed AI Models
- Prototyping and small AI models 
	- Phi-3 or Mistral
- Allows Fine-Tuning

- Alternatives: 
	- [LM Studio](https://lmstudio.ai/)
	- [LocalAI](https://www.localai.app/)
	- [GPT4all](https://www.nomic.ai/gpt4all)

## References

- [Which AI should you use? Copilot, Copilot Studio, Azure AI Studio and more!])(https://www.youtube.com/watch?v=ArRpwLGA2Hk)
- [Getting Started With Azure ML Prompt Flow - Azure AI Studio - Azure OpenAI](https://www.youtube.com/watch?v=z3yPMaQHgWQ&list=PLb9nVWmuEUuGSHVUOp9zYOMeCq_CbiwTO&index=1)
