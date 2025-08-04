---
title: Remote Repositories
---

> [!Note]
> [[dolt]] requires a gRPC-based *ChunkStoreService* to upload/download data chunks over HTTPS.
> 
> **Options:**
> - [DoltHub](https://www.dolthub.com/) (Cloud-based)
> - [DoltLab](https://www.doltlab.com/) (On-Premise)

### Authenticate with remote
```sh
dolt login
```

### Add Remote

Using **DoltHub**:
```sh
# maps to https://doltremoteapi.dolthub.com/yourUser/yourRepo
dolt remote add origin yourUser/yourRepo
```

Using **DoltLab**:
```sh
dolt remote add origin 'https://your-server.com/org/repo'
```

## Fetch / Pull / Push
```sh
dolt fetch origin main
dolt pull origin main
dolt push origin main
```
