
`Set-Service ssh-agent -StartupType Manual`
`Start-Service ssh-agent`


Create the SSH key using `ssh-keygen -C "movgp0@gmail.com"`

Add the private key to the SSH Service `ssh-add ~\.ssh\id_rsa`

Configure to use SSH for the server in the `~\.ssh\config` file
```
Host *.foobar.com
    HostkeyAlgorithms +ssh-rsa
    PubkeyAcceptedAlgorithms +ssh-rsa
```

Register the content of the private key (`~\.ssh\id_rsa.pub`) on the Git server
```
ssh-rsa XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX movgp@hostame
```

`~/.gitconfig` file
```
[core]
    editor = \"C:\\Users\\movgp\\AppData\\Local\\Programs\\Microsoft VS Code\\bin\\code\" --wait
    autocrlf = true
[filter "lfs"]
    required = true
    clean = git-lfs clean -- %f
    smudge = git-lfs smudge -- %f
    process = git-lfs filter-process
[user]
    email = movgp0@gmail.com
    name = Johann Dirry
[http]
    sslVerify = false
    version = HTTP/1.1
[credential "http://my-git-server.com"]
    provider = generic
[credential]
    helper = wincred
```

Clone the Git repository using SSH `git clone ssh://...`