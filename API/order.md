> <h1>GET</h1>
> /wallet/<uuid>/orders/

    Orders list

- Headers
    - X-Wallet-Signature: str
- Params
    - created_at.from: date
    - created_at.to: date
    - env_id: UUID
    - wallet_id: UUID
    - amount.from: float
    - amount.to: float
    - product_id: UUID
- Response
    - Status code: 200 is OK
    - Status code: 401 
        - "msg": "Could not validate credentials"
    - Status code: 422  
        - "msg": "env_id and user_id is required"
    - Body:
        - id: str
        - created_at: datetime
        - wallet_id: str
        - user_id: str
        - product_id: UUID
        - amount: double
        - status_code: str

Example
```python
import request
import json

response = request.get(
    f"{url}/wallet/{123}/orders/",
    headers={
        "X-Wallet-Signature": "secret_t1CdN9S8yicG5eWLUOfhcWaOscVnFXns",
    },
    params={
        "env_id": 123,
	    "user_id": 123,
        "product_id": 123,
    }
)
response.status_code == 200  # True
```
<br>

> <h1>GET</h1>
> /wallet/<uuid>/orders/<uuid>

    Orders by id

- Headers
    - X-Wallet-Signature: str
- Params
    - env_id: UUID
    - user_id: UUID
- Response
    - Status code: 200 is OK
    - Status code: 401 
        - "msg": "Could not validate credentials"
    - Status code: 404  
        - "msg": "Order with the provided ID is not found"
    - Body:
        - id: str
        - wallet_id: str
        - user_id: str
        - product_id: UUID
        - amount: double
        - status_code: str

Example
```python
import request
import json

response = request.get(
    f"{url}/wallet/{123}/orders/{123}",
    headers={
        "X-Wallet-Signature": "secret_t1CdN9S8yicG5eWLUOfhcWaOscVnFXns",
    },
)
response.status_code == 200  # True
```
<br>