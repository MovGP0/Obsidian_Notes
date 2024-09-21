## Installation

Install via WinGet
```shell
winget install -e --id Nushell.Nushell
```

Install via [[cargo]]
```shell
cargo install nu
```

### Register in Windows Terminal

**Note** This step might be omitted, since it's now part of the Nu Shell installer

```shell
cd %AppData%\Local\Packages\Microsoft.WindowsTerminal_8wekyb3d8bbwe\LocalState
code settings.json
```
Add the following entry to `profiles/list`
```json
{
	"guid": "{47302f9c-1ac4-566c-aa3e-8cf29889d6ab}",
	"hidden": false,
	"name": "Nushell",
	"source": "nu"
}
```
