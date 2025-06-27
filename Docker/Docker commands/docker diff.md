# docker diff

Lists changes in the file system of an image. Each change is desribed using a letter.

| Letter | Description  |
| ------ | ------------ |
| A      | file added   |
| C      | file changed |
| D      | file deleted |

## Examples

```powershell
docker diff 'imagename'
```