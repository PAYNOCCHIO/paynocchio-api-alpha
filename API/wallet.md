## Create Wallet

Creates a wallet for a specified user with a given type.

This method allows for the creation of a wallet, linking it to a user in the external system
    while specifying the environment in which the wallet will operate.
---

### Endpoint
`POST /wallet`

### Headers
- **X-Wallet-Signature** (str): A SHA256 signature for authentication (ensure it's correct).
- **X-Test-Mode-Switch** (str): A flag to enable or disable test mode (values: "on" or "off").

### Request Parameters
- **environment_uuid** (UUID, required): The unique identifier of the environment where the operation is performed.
- **user_uuid** (UUID, required): The unique identifier of the user whose wallet will be credited.

## Example Code
### Python
```python
import requests
import json

url = "https://wallet.paynocchio.com/wallet"

payload = json.dumps({
  "user_uuid": "b74efaf6-bd46-404f-b4e8-fd87fdf985a0",
  "environment_uuid": "287d5047-a999-458f-86b9-c29bdf8ed745"
})
headers = {
  'X-Wallet-Signature': 'b75e0c98c7c05a46c3397666452863d89433388eb016a31d88e5117721885c34',
  'X-Test-Mode-Switch': 'on',
  'Content-Type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data=payload)
```

### Response
Example Response Body
```json
{
    "uuid": "123e4567-e89b-12d3-a456-426614174000",
    "wallet_uuid": "5eea41d8-e459-4f41-91cd-763fe7708f8e",
    "external_user_id": "user123",
    "external_order_id": "order456",
    "request_uuid": "request789",
    "created_at": "2024-10-17T12:34:56Z",
    "amount": 100.50,
    "status": {
        "uuid": "status-uuid",
        "updated_at": "2024-10-17T12:34:56Z",
        "title": "Completed",
        "code": "completed"
    },
    "bonus_amount": 10.00,
    "full_amount": 110.50
}

```


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
> /wallet/< uuid >

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
> /wallet/< uuid >/status/

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