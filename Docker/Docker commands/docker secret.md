
Create a secret
```powershell
docker secret create 'secretname' 'secretvalue'
```

The secret is mounted to `/run/secrets/` when creating the container (ie. service)
```powershell
docker service create --secret="secretname" wordpress:latest
```

When using the `secrets:` secctions in a [[docker compose]] file, the secret gets also populated into a file.

### Example

[[docker compose]] YAML file example:
```yaml
version: "3.9"

services:
   db:
     image: mysql:latest
     volumes:
       - db_data:/var/lib/mysql
     environment:
       MYSQL_ROOT_PASSWORD_FILE: /run/secrets/db_root_password
       MYSQL_DATABASE: wordpress
       MYSQL_USER: wordpress
       MYSQL_PASSWORD_FILE: /run/secrets/db_password
     secrets:
       - db_root_password
       - db_password

   wordpress:
     depends_on:
       - db
     image: wordpress:latest
     ports:
       - "8000:80"
     environment:
       WORDPRESS_DB_HOST: db:3306
       WORDPRESS_DB_USER: wordpress
       WORDPRESS_DB_PASSWORD_FILE: /run/secrets/db_password
     secrets:
       - db_password


secrets:
   db_password:
     file: db_password.txt
   db_root_password:
     file: db_root_password.txt

volumes:
    db_data:
```

## See also

- [The Complete Guide to Docker Secrets](https://earthly.dev/blog/docker-secrets/)