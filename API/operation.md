 ## Top Up Wallet

Tops up a user's wallet with a specified amount using a bank card.

---

### Endpoint
`POST /operation/topup`

### Headers
- `X-Wallet-Signature` (str): A SHA256 signature for authentication (ensure it's correct).
- `X-Test-Mode-Switch` (str): A flag to enable or disable test mode (values: "on" or "off").

### Request Parameters
- `environment_uuid` (UUID, required): The unique identifier of the wallet group where the operation is performed.
- `user_uuid` (UUID, required): The unique identifier of the user whose wallet will be credited.
- `wallet_uuid` (UUID, required): The unique identifier of the wallet to be credited.
- `amount` (float, required): The amount of money to be added to the wallet.
- `redirect_url` (string, required): The URL to redirect the user to after the top-up process is completed.

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
#### Example code
Python
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
Javascript
```json
const myHeaders = new Headers();
myHeaders.append("X-Wallet-Signature", "d549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5");
myHeaders.append("X-TEST-MODE-SWITCH", "on");
myHeaders.append("Content-Type", "application/json");

const raw = JSON.stringify({
  "environment_uuid": "8c6b143d-df21-42ee-8a53-c7f19b274982",
  "user_uuid": "450f6b66-f7d6-4f4b-a849-ddf3636778a6",
  "wallet_uuid": "57b3809f-a80a-4fee-829a-35424213e943",
  "amount": 100,
  "redirect_url": "https://moodle.local"
});

const requestOptions = {
  method: "POST",
  headers: myHeaders,
  body: raw,
  redirect: "follow"
};

fetch("https://wallet.paynocchio.com/operation/topup", requestOptions)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.error(error));
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

#### Response Fields

- `id` (string): The unique identifier for the payment link.
- `currency` (string): The currency used for the transaction (e.g., "usd").
- `metadata` (object): Contains additional details regarding the transaction:
  - `amount` (string): The amount added in the smallest currency unit (e.g., "10000" for $100.00).
  - `company_id` (UUID): The unique identifier of the company handling the transaction.
  - `company_stripe_account` (string): The Stripe account ID for the company (e.g., "acct_1QAB4bPuDcQQWLF5").
  - `currency` (string): The currency of the transaction (e.g., "USD").
  - `environment_uuid` (UUID): The unique identifier of the wallet group where the transaction occurs.
  - `interaction_method` (string): The method used for the transaction (e.g., "web").
  - payment_request_uuid` (UUID): A unique identifier for the payment request.
  - `stripe_product_id` (string): The Stripe product associated with the top-up.
  - `type_operation` (string): The type of operation (e.g., "payment_operation_add_money").
  - `user_uuid` (UUID): The unique identifier of the user initiating the top-up.
  - `wallet_uuid` (UUID): The unique identifier of the wallet being credited.
  - `x_forwarded_for` (string): The originating IP address.
  - `x_test_mode_switch` (string): Indicates whether the request is in test mode (e.g., "on").
- `url (string)`: The URL for the payment page.
- `type_interactions` (string): Describes the interaction type (e.g., "success.interaction").
- `interaction` (string): The payment provider used for the transaction (e.g., "stripe").

## Payment from Wallet

Initiates a payment from a user's wallet for customer services.

---

### Endpoint
`POST /operation/payment`

### Headers
- `X-Wallet-Signature` (str): A SHA256 signature for authentication (ensure it's correct).
- `X-Test-Mode-Switch` (str): A flag to enable or disable test mode (values: "on" or "off").


### Request Parameters
- `environment_uuid` (UUID, required): The unique identifier of the wallet group where the payment is made.
- `user_uuid` (UUID, required): The unique identifier of the user making the payment.
- `wallet_uuid` (UUID, required): The unique identifier of the user's wallet to be charged.
- `external_order_id` (UUID, required): A unique identifier for the external order related to the payment.
- `amount` (float, required): The primary amount to be charged from the wallet.
- `full_amount` (float, required): The total amount for the transaction, which could include bonuses or other adjustments.
- `bonus_amount` (float, optional): Any additional bonus amount applied to the transaction.

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
#### Example code
Python
```python
import requests
import json

url = "https://wallet.paynocchio.com/operation/payment"

payload = json.dumps({
  "environment_uuid": "8c6b143d-df21-42ee-8a53-c7f19b274982",
  "user_uuid": "450f6b66-f7d6-4f4b-a849-ddf3636778a6",
  "wallet_uuid": "57b3809f-a80a-4fee-829a-35424213e943",
  "external_order_id": "606589a2-190a-4b5e-ac9a-aef7d989dbbc",
  "amount": 10,
  "full_amount": 10,
  "bonus_amount": 0
})
headers = {
  'X-Wallet-Signature': 'd549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5',
  'X-TEST-MODE-SWITCH': 'on',
  'Content-Type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data=payload)
```
Javascript
```json
const myHeaders = new Headers();
myHeaders.append("X-Wallet-Signature", "d549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5");
myHeaders.append("X-TEST-MODE-SWITCH", "on");
myHeaders.append("Content-Type", "application/json");

const raw = JSON.stringify({
  "environment_uuid": "8c6b143d-df21-42ee-8a53-c7f19b274982",
  "user_uuid": "450f6b66-f7d6-4f4b-a849-ddf3636778a6",
  "wallet_uuid": "57b3809f-a80a-4fee-829a-35424213e943",
  "external_order_id": "606589a2-190a-4b5e-ac9a-aef7d989dbbc",
  "amount": 10,
  "full_amount": 10,
  "bonus_amount": 0
});

const requestOptions = {
  method: "POST",
  headers: myHeaders,
  body: raw,
  redirect: "follow"
};

fetch("https://wallet.paynocchio.com/operation/payment", requestOptions)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.error(error));
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

#### Response Fields

- `schemas` (object): Details of the payment:
    - `amount` (float): The amount charged from the wallet.
      - `card_uuid` (UUID): The unique identifier of the card used for the payment.
  - `external_order_id` (UUID): The unique identifier for the external order processed in the payment.
  - `full_amount` (float): The total amount of the transaction.
  - `bonus_amount` (float): Any bonus amount applied to the transaction.
- `type_interactions` (string): Describes the type of interaction (e.g., "success.interaction").
- `interaction` (string): The payment provider used for the transaction (e.g., "stripe").

#### Exceptions and Warnings

`Warnings (200 OK but with message)`

Insufficient main balance:
```json
{
    "schemas": {
        "is_error": false,
        "message": "Insufficient main balance"
    },
    "type_interactions": "warning.interaction",
    "interaction": "stripe"
}
```

Insufficient rewarding balance:
```json
{
    "schemas": {
        "is_error": false,
        "message": "Insufficient rewarding balance"
    },
    "type_interactions": "warning.interaction",
    "interaction": "stripe"
}
```

## Withdraw from Wallet

This API method allows users to withdraw funds from their wallet. It requires the wallet group, user, wallet details, the currency for the transaction, and the withdrawal amount.

---

### Endpoint
`POST /operation/withdraw`

### Headers
- `X-Wallet-Signature` (str): A SHA256 signature for authentication (ensure it's correct).
- `X-Test-Mode-Switch` (str): A flag to enable or disable test mode (values: "on" or "off").


### Request Parameters
- `environment_uuid` (UUID, required): The unique identifier of the wallet group where the transaction occurs.
- `user_uuid` (UUID, required): The unique identifier of the user withdrawing funds.
- `wallet_uuid` (UUID, required): The unique identifier of the user's wallet to withdraw from.
- `currency` (string, required): The 3-letter ISO currency code for the transaction (e.g., `"USD"`).
- `amount` (float, required): The amount to withdraw from the wallet. Must be a positive value.

#### Example Request Body
```json
{
    "environment_uuid": "a2860217-a6b2-4fb3-9a7b-32e123651e16",
    "user_uuid": "a2820337-a6b2-4fb9-9a1b-32q217651e55",
    "wallet_uuid": "5eea41d8-e459-4f41-91cd-763fe7708f8e",
    "currency": "USD",
    "amount": 100.0
}
```
#### Example code
Python
```python
import requests
import json

url = "https://wallet.paynocchio.com/operation/withdraw"

payload = json.dumps({
  "environment_uuid": "8c6b143d-df21-42ee-8a53-c7f19b274982",
  "user_uuid": "450f6b66-f7d6-4f4b-a849-ddf3636778a6",
  "currency": "USD",
  "amount": 20,
  "wallet_uuid": "57b3809f-a80a-4fee-829a-35424213e943"
})
headers = {
  'X-Wallet-Signature': 'd549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5',
  'X-Test-Mode-Switch': 'on',
  'Content-Type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data=payload)
```
Javascript
```json
const myHeaders = new Headers();
myHeaders.append("X-Wallet-Signature", "d549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5");
myHeaders.append("X-Test-Mode-Switch", "on");
myHeaders.append("Content-Type", "application/json");

const raw = JSON.stringify({
  "environment_uuid": "8c6b143d-df21-42ee-8a53-c7f19b274982",
  "user_uuid": "450f6b66-f7d6-4f4b-a849-ddf3636778a6",
  "currency": "USD",
  "amount": 20,
  "wallet_uuid": "57b3809f-a80a-4fee-829a-35424213e943"
});

const requestOptions = {
  method: "POST",
  headers: myHeaders,
  body: raw,
  redirect: "follow"
};

fetch("https://wallet.paynocchio.com/operation/withdraw", requestOptions)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.error(error));
```
### Response
`Example Successful Response (200 OK)`
```json
{
    "schemas": {
        "environment_uuid": "a2860217-a6b2-4fb3-9a7b-32e123651e16",
        "user_uuid": "a2820337-a6b2-4fb9-9a1b-32q217651e55",
        "wallet_uuid": "5eea41d8-e459-4f41-91cd-763fe7708f8e",
        "type_operation": "withdraw",
        "currency": "USD",
        "amount": 100.0,
        "x_forwarded_for": "192.168.1.1"
    },
    "type_interactions": "success.interaction",
    "interaction": "stripe"
}
```
#### Exceptions and Warnings
`Warnings (200 OK but with message)`

Insufficient main balance
```json
{
    "schemas": {
        "is_error": false,
        "message": "Insufficient main balance"
    },
    "type_interactions": "warning.interaction",
    "interaction": "stripe"
}
```

