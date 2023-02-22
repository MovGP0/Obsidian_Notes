Declare variable
```http
@hostname=localhost
```

Use variable
```http
GET https://{{hostname}}/api/
```

GET-Request
```http
GET https://visualstudio.com
```

Declare headers
```http
GET https://localhost/api/
Accept: applcation/json
```

Separate multiple requests using `###`

## Example

```http
@hostname=localhost
@port=443
@host={{hostname}}:{{port}}
@searchTerm=tool

GET https://visualstudio.com

###

GET https://{{host}}/api/search/{{searchTerm}}
Accept: applcation/json
Content-Type: application/x-www-form-urlencoded

identity=somevalue

### 

PUT https://{{host}}/api/demo
Accept: text/plain
Content-Type: application/json

{
    "author": "John Doe",
    "name": "foobar"
}
```
