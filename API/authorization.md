> <h1>POST</h1>
> https://url.api.com/auth
- Headers
    - auth_token: sha256
    - access_token: str
- Response
    - Status code: 200 is OK
    - Status code: 403 is not right tokens

Example
```python
import request

response = request.post(
    "https://url.api.com/auth",
    headers={
        "auth_token": "0xqwe...132",
        "access_token": "some_token",
    }
)
response.status_code == 200  # True
```
<br>

> <h1>GET</h1>
> https://url.api.com/health
- Headers
    - auth_token: sha256
    - access_token: str
<br>