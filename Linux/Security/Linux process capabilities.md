Capabilities control the permissions to kernel capabilities of an application. Should be prefered to running the application as admin user.

Use `ps` to get the PID. Then use `getpcaps PID` to list the capabilities of the app.

Set additional capabilities to app file
```bash
setcap 'cap_net_raw+p' /bin/ping
```

Get the capabilities of the current user
```bash
capsh --print
```

## See also
- [kernel capabilities](https://man7.org/linux/man-pages/man7/capabilities.7.html)