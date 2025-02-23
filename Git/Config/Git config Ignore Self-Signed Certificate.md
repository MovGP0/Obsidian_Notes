Do not do this!
```bash
git config --global http.sslVerify false
git config --global http.version 'HTTP/1.1'
```

Might want to do this
```bash
git config --global http.https://DOMAINNAME.sslVerify false
git config --global http.sslVerify true
git config --global http.version 'HTTP/1.1'
```

## See also

- [GitCredentialManager](https://github.com/GitCredentialManager/git-credential-manager/blob/main/docs/netconfig.md#tls-verification)
