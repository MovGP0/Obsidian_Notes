```powershell
# Windows native credential manager
git credential-store wincred

# MacOS native credential manager
git credential-store osxkeychain

# file in user directory
git credential-store --file ~/git.store
```

## Windows

- Open `Credential Mananger`
- Select `Windows Credentials`

Address: `git:https://somecompany.com/`
Username: `GROUP\USER`
Password: `********`
