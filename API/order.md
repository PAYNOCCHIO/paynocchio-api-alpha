## Get Order by UUID

Retrieves detailed information about a specific order using its UUID. This method also includes user UUID and wallet group UUID in the request, as well as handles signature validation and test mode switching via headers.

---

### Endpoint
`GET /orders/{order_uuid}`

### Headers
- `X-Wallet-Signature` (str): A SHA256 signature for authentication (ensure it's correct).
- `X-Test-Mode-Switch` (str): A flag to enable or disable test mode (values: "on" or "off").


### Request Parameters
- `environment_uuid` (UUID, required): The unique identifier of the wallet group where the operation is performed.
- `user_uuid` (UUID, required): The unique identifier of the user whose wallet will be credited.

### Example code

```python
import requests

url = "https://wallet.paynocchio.com/orders/287d5047-a999-458f-86b9-c29bdf8ed745?environment_uuid=8c6b143d-df21-42ee-8a53-c7f19b274982&user_uuid=450f6b66-f7d6-4f4b-a849-ddf3636778a6"

payload = {}
headers = {
  'X-Wallet-Signature': 'd549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5',
  'X-TEST-MODE-SWITCH': 'on'
}

response = requests.request("GET", url, headers=headers, data=payload)
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

#### Response Fields

**dict**: A dictionary containing the following fields:
* `uuid` (str): Unique identifier of the order.
* `wallet_uuid` (str): Unique identifier of the wallet linked to the order.
* `external_user_id` (str): External identifier of the user.
* `external_order_id` (str): External identifier of the order.
* `request_uuid` (str): Identifier associated with the request.
* `created_at` (datetime): Timestamp when the order was created.
* `amount` (float): The order amount.
* `status` (dict): A dictionary representing the status of the order, containing:
* `uuid` (str): UUID of the status.
* `updated_at` (datetime): Timestamp when the status was last updated.
* `title` (str): Human-readable title for the status.
* `code` (str): Code representing the status.
* `bonus_amount` (float): Bonus amount applied to the order.
* `full_amount` (float): Total amount including bonuses.


## Get All Orders by Wallet UUID

This section describes how to retrieve all orders associated with a specific wallet UUID using the `get_orders_by_wallet_uuid` method from the client.

---

### Endpoint
`GET /orders/{order_uuid}`

### Headers
- `X-Wallet-Signature` (str): A SHA256 signature for authentication (ensure it's correct).
- `X-Test-Mode-Switch` (str): A flag to enable or disable test mode (values: "on" or "off").


### Request Parameters
- `environment_uuid` (UUID, required): The unique identifier of the wallet group where the operation is performed.
- `user_uuid` (UUID, required): The unique identifier of the user whose wallet will be credited.

### Example code

```python
import requests

url = "https://wallet.paynocchio.com/orders/287d5047-a999-458f-86b9-c29bdf8ed745?environment_uuid=8c6b143d-df21-42ee-8a53-c7f19b274982&user_uuid=450f6b66-f7d6-4f4b-a849-ddf3636778a6"

payload = {}
headers = {
  'X-Wallet-Signature': 'd549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5',
  'X-TEST-MODE-SWITCH': 'on'
}

response = requests.request("GET", url, headers=headers, data=payload)
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

#### Response Fields

**dict**: A dictionary containing the following fields:
* `uuid` (str): Unique identifier of the order.
* `wallet_uuid` (str): Unique identifier of the wallet linked to the order.
* `external_user_id` (str): External identifier of the user.
* `external_order_id` (str): External identifier of the order.
* `request_uuid` (str): Identifier associated with the request.
* `created_at` (datetime): Timestamp when the order was created.
* `amount` (float): The order amount.
* `status` (dict): A dictionary representing the status of the order, containing:
* `uuid` (str): UUID of the status.
* `updated_at` (datetime): Timestamp when the status was last updated.
* `title` (str): Human-readable title for the status.
* `code` (str): Code representing the status.
* `bonus_amount` (float): Bonus amount applied to the order.
* `full_amount` (float): Total amount including bonuses.