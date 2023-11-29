> <h1>POST</h1>
> /wallet/payment/

    To allow users to initiate a payment for customer services

- Headers
    - X-Wallet-Signature: str
- Body
    - env_id: UUID Admin panel environment ID
    - user_id: UUID External system user id
    - wallet_id: UUID
    - amount: float
    - product_id: UUID External system product or service ID
- Response
    - Status code: 200 is OK
    - Status code: 401 
        - "msg": "Signature is invalid"tokens
    - Status code: 422 
        - "msg": "env_id and user_id is required"

Example
```python
import request
import json

response = request.post(
    f"{url}/wallet/payment/",
    headers={
        "X-Wallet-Signature": "secret_t1CdN9S8yicG5eWLUOfhcWaOscVnFXns",
    },
    json=json.dumps({
        "env_id": 123,
	    "user_id": 123,
	    "wallet_id": 123,
	    "amount": 123.12,
	    "product_id": "asd123"
    })
)
response.status_code == 200  # True
```
<br>

> <h1>POST</h1>
> /wallet/topup/

    To top up wallet with desired sum by bank card

- Headers
    - X-Wallet-Signature: str
- Body
    - env_id: UUID Admin panel environment ID
    - user_id: UUID External system user id
    - wallet_id: UUID
    - amount: float
- Response
    - Status code: 200 is OK
    - Status code: 401 
        - "msg": "Signature is invalid"tokens
    - Status code: 422 
        - "msg": "env_id and user_id is required"

Example
```python
import request
import json

response = request.post(
    f"{url}/wallet/topup/",
    headers={
        "X-Wallet-Signature": "secret_t1CdN9S8yicG5eWLUOfhcWaOscVnFXns",
    },
    json=json.dumps({
        "env_id": 123,
	    "user_id": 123,
	    "wallet_id": 123,
	    "amount": 123.12,
    })
)
response.status_code == 200  # True
```
<br>

> <h1>POST</h1>
> /wallet/withdraw/

    To withdraw money from the wallet back to the user’s bank card

- Headers
    - X-Wallet-Signature: str
- Body
    - env_id: UUID Admin panel environment ID
    - user_id: UUID External system user id
    - wallet_id: UUID
    - amount: float
- Response
    - Status code: 200 is OK
    - Status code: 401 
        - "msg": "Signature is invalid"tokens
    - Status code: 422 
        - "msg": "env_id and user_id is required"

Example
```python
import request
import json

response = request.post(
    f"{url}/wallet/withdraw/",
    headers={
        "X-Wallet-Signature": "secret_t1CdN9S8yicG5eWLUOfhcWaOscVnFXns",
    },
    json=json.dumps({
        "env_id": 123,
	    "user_id": 123,
	    "wallet_id": 123,
	    "amount": 123.12,
    })
)
response.status_code == 200  # True
```
<br>

> <h1>POST</h1>
> /wallet/

    Create wallet for specified user with specified currency and type

- Headers
    - X-Wallet-Signature: str
- Body
    - env_id: UUID Admin panel environment ID
    - user_id: UUID External system user id
- Response
    - Status code: 200 is OK
    - Status code: 401 
        - "msg": "Signature is invalid"tokens
    - Status code: 422 
        - "msg": "env_id and user_id is required"
    - Body:
        - wallet_id: UUID

Example
```python
import request
import json

response = request.post(
    f"{url}/wallet/",
    headers={
        "X-Wallet-Signature": "secret_t1CdN9S8yicG5eWLUOfhcWaOscVnFXns",
    },
    json=json.dumps({
        "env_id": 123,
	    "user_id": 123,
    })
)
response.status_code == 200  # True
```
<br>

> <h1>GET</h1>
> /wallet/

    To get list of wallets of the authorized user

- Headers
    - X-Wallet-Signature: str
- Params
    - env_id: UUID Admin panel environment ID
    - user_id: UUID External system user id
- Response
    - Status code: 200 is OK
    - Status code: 401 
        - "msg": "Signature is invalid"tokens
    - Status code: 422 
        - "msg": "env_id and user_id is required"
    - Body:
        - wallets: list
            - wallet.id: UUID
            - wallet.user_id: UUID
            - wallet.balance: double
            - wallet.currency: str
            - wallet.status: str

Example
```python
import request
import json

response = request.get(
    f"{url}/wallet/",
    headers={
        "X-Wallet-Signature": "secret_t1CdN9S8yicG5eWLUOfhcWaOscVnFXns",
    },
    params={
        "env_id": 123,
	    "user_id": 123,
    }
)
response.json() == {
    "wallets": [
        {
            id: "123456789",
            user_id: "987654321",
            balance: 1000.00,
            currency: "USD", 
			status: "active"
        },
        {
            id: "123456789",
            user_id: "987654321",
            balance: 1000.00,
            currency: "USD", 
			status: "active"
        },
    ]
}  # True
```
<br>

> <h1>GET</h1>
> /wallet/<uuid>

    To get wallet by id with balance

- Headers
    - X-Wallet-Signature: str
- Params
    - env_id: UUID Admin panel environment ID
    - user_id: UUID External system user id
    - wallet_id: UUID
- Response
    - Status code: 200 is OK
    - Status code: 401 
        - "msg": "Signature is invalid"tokens
    - Status code: 422 
        - "msg": "env_id and user_id is required"
    - Body:
        - env_id: UUID
        - user_id: UUID
        - balance: double
        - currency: str

Example
```python
import request
import json

response = request.get(
    f"{url}/wallet/{123}/",
    headers={
        "X-Wallet-Signature": "secret_t1CdN9S8yicG5eWLUOfhcWaOscVnFXns",
    },
    params={
        "env_id": 123,
	    "user_id": 123,
    }
)
response.json() == {
	"id": "123456789",
	"balance": 1000.00,
	"currency": "USD"
}  # True
```
<br>

> <h1>PATCH</h1>
> /wallet/<uuid>/status/

    To change wallet status

- Headers
    - X-Wallet-Signature: str
- Body
    - env_id: UUID Admin panel environment ID
    - user_id: UUID External system user id
    - wallet_id: UUID
- Response
    - Status code: 200 is OK
    - Status code: 401 
        - "msg": "Signature is invalid"tokens
    - Status code: 422 
        - "msg": "env_id and user_id is required"
    - Body:
        - env_id: UUID
        - user_id: UUID
        - balance: double
        - currency: UUID
        - status: str

Example
```python
import request
import json

response = request.patch(
    f"{url}/wallet/{123}/status/",
    headers={
        "X-Wallet-Signature": "secret_t1CdN9S8yicG5eWLUOfhcWaOscVnFXns",
    },
    json=json.dumps({
        "env_id": "123",
        "user_id": "123",
        "amount": 12.12
    })
)
response.json() == {
	"id": "123",
	"balance": 12.12,
	"currency": "USD",
	"status": "active"
}  # True
```
<br>