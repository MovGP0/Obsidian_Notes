## Zoxide Completer

Install Zoxide:
```shell
winget install ajeetdsouza.zoxide
```

Create the `.zoxide.nu` script
```shell
zoxide init nushell
```

Edit `config.nu`
```shell
code ~\AppData\Roaming\nushell\config.nu
```

Add the following line to `config.nu`
```shell
source ~/.zoxide.nu
```

## Configure Oh-My-Posh

Install Oh-My-Posh
```shell
winget install -e --id JanDeDobbeleer.OhMyPosh
```

Initialize the Nu script with the proper theme:
```
oh-my-posh init nu --config '~\AppData\Local\Programs\oh-my-posh\themes\marcduiker.omp.json'
```

Edit `config.nu`
```
code ~\AppData\Roaming\nushell\config.nu
```

Add the following line to `config.nu`
```shell
source ~/.oh-my-posh.nu
```

## Configure completions

Clone the Nu Scripts repository
```shell
git clone https://github.com/nushell/nu_scripts.git
```

Edit `config.nu`
```shell
code ~\AppData\Roaming\nushell\config.nu
```

Add the following lines to `config.nu` for completions:
```shell
source D:\nu_scripts\custom-completions\cargo\cargo-completions.nu
source D:\nu_scripts\custom-completions\docker\docker-completions.nu
source D:\nu_scripts\custom-completions\dotnet\dotnet-completions.nu
source D:\nu_scripts\custom-completions\git\git-completions.nu
source D:\nu_scripts\custom-completions\rustup\rustup-completions.nu
source D:\nu_scripts\custom-completions\vscode\vscode-completions.nu
source D:\nu_scripts\custom-completions\winget\winget-completions.nu
```
