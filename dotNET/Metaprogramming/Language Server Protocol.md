Add the [OmniSharp Language Server](https://github.com/OmniSharp/csharp-language-server-protocol) package.

```powershell
dotnet add package OmniSharp.Extensions.LanguageServer
```

## Implement the LSP server

```csharp
using System.Threading.Tasks;
using OmniSharp.Extensions.LanguageServer.Protocol.Server;
using OmniSharp.Extensions.LanguageServer.Server;

namespace MyLanguageServer;

class Program
{
	static async Task Main(string[] args)
	{
		var server = await LanguageServer.From(options =>
			options.WithInput(Console.OpenStandardInput())
				   .WithOutput(Console.OpenStandardOutput())
				   .WithHandler<TextDocumentHandler>());

		await server.WaitForExit;
	}
}
```

## Implement Handlers

Implement handlers for all the required Language Server Protocol (LSP) features

### Text Document Handler

```csharp
public class TextDocumentHandler : ITextDocumentSyncHandler
{
	private readonly DocumentSelector _documentSelector = new DocumentSelector(
		new DocumentFilter() { Pattern = "**/*.myscript" }
	);

	public TextDocumentSyncKind Change { get; } = TextDocumentSyncKind.Full;

	public TextDocumentChangeRegistrationOptions GetRegistrationOptions()
	{
		return new TextDocumentChangeRegistrationOptions()
		{
			DocumentSelector = _documentSelector,
			SyncKind = Change
		};
	}

	public async Task<Unit> Handle(DidChangeTextDocumentParams request, CancellationToken ct)
	{
		// Handle text document changes
		return Unit.Task;
	}

	public Task<Unit> Handle(DidOpenTextDocumentParams request, CancellationToken ct)
	{
		// Handle document opening
		return Unit.Task;
	}

	public Task<Unit> Handle(DidCloseTextDocumentParams request, CancellationToken ct)
	{
		// Handle document closing
		return Unit.Task;
	}

	public Task<Unit> Handle(DidSaveTextDocumentParams request, CancellationToken ct)
	{
		// Handle document saving
		return Unit.Task;
	}
}
```

### Language completion handler

```csharp
public class CompletionHandler : ICompletionHandler
{
	private readonly DocumentSelector _documentSelector = new DocumentSelector(
		new DocumentFilter() { Pattern = "**/*.myscript" }
	);

	public CompletionRegistrationOptions GetRegistrationOptions()
	{
		return new CompletionRegistrationOptions()
		{
			DocumentSelector = _documentSelector
		};
	}

	public async Task<CompletionList> Handle(CompletionParams request, CancellationToken cancellationToken)
	{
		var items = new List<CompletionItem>
		{
			new CompletionItem
			{
				Label = "keyword1",
				Kind = CompletionItemKind.Keyword,
				Documentation = "Documentation for keyword1"
			},
			new CompletionItem
			{
				Label = "keyword2",
				Kind = CompletionItemKind.Keyword,
				Documentation = "Documentation for keyword2"
			}
		};

		return new CompletionList(items);
	}
}
```

## Create VSCode Extension

In `package.json`

```json
{
    "name": "myscript",
    "displayName": "MyScript",
    "description": "Support for MyScript language",
    "version": "0.1.0",
    "engines": {
        "vscode": "^1.50.0"
    },
    "activationEvents": [
        "onLanguage:myscript"
    ],
    "main": "./out/extension.js",
    "contributes": {
        "languages": [
            {
                "id": "myscript",
                "extensions": [".myscript"]
            }
        ]
    },
    "scripts": {
        "vscode:prepublish": "npm run compile",
        "compile": "tsc -p ./"
    },
    "devDependencies": {
        "typescript": "^4.0.3",
        "vscode": "^1.1.36",
        "@types/node": "^14.14.6",
        "@types/vscode": "^1.50.0"
    }
}
```

In `extension.ts`

```typescript
import * as vscode from 'vscode';
import * as cp from 'child_process';
import { LanguageClient, LanguageClientOptions, ServerOptions, TransportKind } from 'vscode-languageclient/node';

let client: LanguageClient;

export function activate(context: vscode.ExtensionContext) {
    let serverOptions: ServerOptions = {
        run: { command: 'dotnet', args: ['run', '--project', '<path_to_your_project>'] },
        debug: { command: 'dotnet', args: ['run', '--project', '<path_to_your_project>'] }
    };

    let clientOptions: LanguageClientOptions = {
        documentSelector: [{ scheme: 'file', language: 'myscript' }]
    };

    client = new LanguageClient(
        'MyLanguageServer',
        'My Language Server',
        serverOptions,
        clientOptions
    );

    client.start();
}

export function deactivate(): Thenable<void> | undefined {
    if (!client) {
        return undefined;
    }
    return client.stop();
}
```

Execute server
```bash
npm install
npm run compile
```
