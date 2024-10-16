 ## Top Up Wallet

### Description
Tops up a user's wallet with a specified amount using a bank card.

### Endpoint
`POST /operation/topup`

### Request Parameters
- **environment_uuid** (UUID, required): The unique identifier of the environment where the operation is performed.
- **user_uuid** (UUID, required): The unique identifier of the user whose wallet will be credited.
- **wallet_uuid** (UUID, required): The unique identifier of the wallet to be credited.
- **amount** (float, required): The amount of money to be added to the wallet.
- **redirect_url** (string, required): The URL to redirect the user to after the top-up process is completed.

#### Example Request Body
```json
{
    "environment_uuid": "8c6b143d-df21-42ee-8a53-c7f19b274982",
    "user_uuid": "450f6b66-f7d6-4f4b-a849-ddf3636778a6",
    "wallet_uuid": "57b3809f-a80a-4fee-829a-35424213e943",
    "amount": 100.00,
    "redirect_url": "https://example.com/redirect"
}
```
Example code

```python
import requests
import json

url = "https://wallet.paynocchio.com/operation/topup"

payload = json.dumps({
  "environment_uuid": "8c6b143d-df21-42ee-8a53-c7f19b274982",
  "user_uuid": "450f6b66-f7d6-4f4b-a849-ddf3636778a6",
  "wallet_uuid": "57b3809f-a80a-4fee-829a-35424213e943",
  "amount": 100,
  "redirect_url": "https://moodle.local"
})
headers = {
  'X-Wallet-Signature': 'd549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5',
  'X-TEST-MODE-SWITCH': 'on',
  'Content-Type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data=payload)
```

### Response
Example Response Body
```json
{
    "id": "plink_1QAX8kPuDcQQWLF5sosdqFjP",
    "currency": "usd",
    "metadata": {
        "amount": "10000",
        "company_id": "25287fa6-3ddc-4847-a57b-ec50d69b5313",
        "company_stripe_account": "acct_1QAB4bPuDcQQWLF5",
        "currency": "USD",
        "environment_uuid": "8c6b143d-df21-42ee-8a53-c7f19b274982",
        "interaction_method": "web",
        "payment_request_uuid": "3775b006-37fc-4160-a897-abe4a2dc7add",
        "stripe_product_id": "prod_R2FapZ2TdXCdQO",
        "type_operation": "add money",
        "user_uuid": "450f6b66-f7d6-4f4b-a849-ddf3636778a6",
        "wallet_uuid": "57b3809f-a80a-4fee-829a-35424213e943",
        "x_forwarded_for": "10.40.96.229",
        "x_test_mode_switch": "on"
    },
    "url": "https://buy.stripe.com/test_7sIdUOcKUgPAeQgfZ5",
    "type_interactions": "success.interaction",
    "interaction": "stripe"
}
```

Response Fields

- **(string)**: The unique identifier for the payment link.
- **currency** (string): The currency used for the transaction (e.g., "usd").
- **metadata** (object): Contains additional details regarding the transaction:
  - **amount** (string): The amount added in the smallest currency unit (e.g., "10000" for $100.00).
  - **company_id** (UUID): The unique identifier of the company handling the transaction.
  - **company_stripe_account** (string): The Stripe account ID for the company (e.g., "acct_1QAB4bPuDcQQWLF5").
  - **currency** (string): The currency of the transaction (e.g., "USD").
  - **environment_uuid** (UUID): The unique identifier of the environment where the transaction occurs.
  - **interaction_method** (string): The method used for the transaction (e.g., "web").
  - payment_request_uuid** (UUID): A unique identifier for the payment request.
  - **stripe_product_id** (string): The Stripe product associated with the top-up.
  - **type_operation** (string): The type of operation (e.g., "add money").
  - **user_uuid** (UUID): The unique identifier of the user initiating the top-up.
  - **wallet_uuid** (UUID): The unique identifier of the wallet being credited.
  - **x_forwarded_for** (string): The originating IP address.
  - **x_test_mode_switch** (string): Indicates whether the request is in test mode (e.g., "on").
- **url (string)**: The URL for the payment page.
- **type_interactions** (string): Describes the interaction type (e.g., "success.interaction").
- **interaction** (string): The payment provider used for the transaction (e.g., "stripe").

## Payment from Wallet

### Description
Initiates a payment from a user's wallet for customer services.

### Endpoint
`POST /operation/payment`

### Request Parameters
- **environment_uuid** (UUID, required): The unique identifier of the environment where the payment is made.
- **user_uuid** (UUID, required): The unique identifier of the user making the payment.
- **wallet_uuid** (UUID, required): The unique identifier of the user's wallet to be charged.
- **external_order_id** (UUID, required): A unique identifier for the external order related to the payment.
- **amount** (float, required): The primary amount to be charged from the wallet.
- **full_amount** (float, required): The total amount for the transaction, which could include bonuses or other adjustments.
- **bonus_amount** (float, optional): Any additional bonus amount applied to the transaction.

#### Example Request Body
```json
{
    "environment_uuid": "8c6b143d-df21-42ee-8a53-c7f19b274982",
    "user_uuid": "450f6b66-f7d6-4f4b-a849-ddf3636778a6",
    "wallet_uuid": "57b3809f-a80a-4fee-829a-35424213e943",
    "external_order_id": "606589a2-190a-4b5e-ac9a-aef7d989dbbc",
    "amount": 10,
    "full_amount": 20,
    "bonus_amount": 10
}
```

### Response
Example Response Body
```json
{
    "schemas": {
        "amount": 10.0,
        "card_uuid": "d7196a87-ab07-4210-acb0-c98a7ed0492d",
        "external_order_id": "606589a2-1901-4b5e-ac9a-aef7d989dbbd",
        "full_amount": 20.0,
        "bonus_amount": 10
    },
    "type_interactions": "success.interaction",
    "interaction": "stripe"
}

```

### Response Fields

- **schemas** (object): Details of the payment:
    - **amount** (float): The amount charged from the wallet.
      - **card_uuid** (UUID): The unique identifier of the card used for the payment.
  - **external_order_id** (UUID): The unique identifier for the external order processed in the payment.
  - **full_amount** (float): The total amount of the transaction.
  - **bonus_amount** (float): Any bonus amount applied to the transaction.
- **type_interactions** (string): Describes the type of interaction (e.g., "success.interaction").
- **interaction** (string): The payment provider used for the transaction (e.g., "stripe").

<br>

> <h1>POST</h1>
> /wallet/withdraw/

    To withdraw money from the wallet back to the user’s bank card
    
    Withdrawal operation should be activated at environment settings

- Headers
    - X-Wallet-Signature: str
    - X-Test-Mode-Switch: on/off
    - Content-Type: 'application/json'
- Body
    - environment_uuid: UUID Admin panel environment ID
    - user_uuid: UUID External system user id
    - wallet_uuid: UUID
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