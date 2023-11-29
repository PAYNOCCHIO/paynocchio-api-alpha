> <h1>GET</h1>
> /transaction/

    Returns a paginated list of transactions for the company. The response may contain fewer than page_size of results.

- Headers
    - X-Wallet-Signature: str
- Response
    - Status code: 200 is OK
    - Status code: 401 
        - "msg": "Could not validate credentials"
    - Status code: 400  
        - "msg": "env_id and user_id is required"
    - Body:
        - id: UUID
        - created_at: str
        - request_id: UUID
        - wallet_id: UUID
        - amount: double
        - currency: str
        - from_account: UUID
        - to_account: UUID
        - type: str
        - description: str
        - links.next: int
        - links.prev: int

Example
```python
import request
import json

response = request.get(
    f"{url}/transaction/",
    headers={
        "X-Wallet-Signature": "secret_t1CdN9S8yicG5eWLUOfhcWaOscVnFXns",
    }
)
response.status_code == 200  # True
```
<br>
