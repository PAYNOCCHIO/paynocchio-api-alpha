## Create Wallet

Creates a wallet for a specified user with a given type.

This method allows for the creation of a wallet, linking it to a user in the external system
    while specifying the wallet group in which the wallet will operate.
---

### Endpoint
`POST /wallet`

### Headers
- `X-Wallet-Signature` (str): A SHA256 signature for authentication (ensure it's correct).
- `X-Test-Mode-Switch` (str): A flag to enable or disable test mode (values: "on" or "off").

### Request Parameters
- `environment_uuid` (UUID, required): The unique identifier of the wallet group where the operation is performed.
- `user_uuid` (UUID, required): The unique identifier of the user whose wallet will be credited.

### Example Code
cURL
```curl
curl --location 'https://wallet.paynocchio.com/wallet' \
--header 'X-Wallet-Signature: d549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5' \
--header 'X-Test-Mode-Switch: on' \
--header 'Content-Type: application/json' \
--data '{
"user_uuid": "450f6b66-f7d6-4f4b-a849-ddf3636778a6",
"environment_uuid": "8c6b143d-df21-42ee-8a53-c7f19b274982"
}'
```
Python
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
Java
```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("application/json");
RequestBody body = RequestBody.create(mediaType, "{\r\n\"user_uuid\": \"450f6b66-f7d6-4f4b-a849-ddf3636778a6\",\r\n\"environment_uuid\": \"8c6b143d-df21-42ee-8a53-c7f19b274982\"\r\n}");
Request request = new Request.Builder()
  .url("https://wallet.paynocchio.com/wallet")
  .method("POST", body)
  .addHeader("X-Wallet-Signature", "d549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5")
  .addHeader("X-Test-Mode-Switch", "on")
  .addHeader("Content-Type", "application/json")
  .build();
Response response = client.newCall(request).execute();
```
Javascript
```js
const myHeaders = new Headers();
myHeaders.append("X-Wallet-Signature", "d549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5");
myHeaders.append("X-Test-Mode-Switch", "on");
myHeaders.append("Content-Type", "application/json");

const raw = JSON.stringify({
  "user_uuid": "450f6b66-f7d6-4f4b-a849-ddf3636778a6",
  "environment_uuid": "8c6b143d-df21-42ee-8a53-c7f19b274982"
});

const requestOptions = {
  method: "POST",
  headers: myHeaders,
  body: raw,
  redirect: "follow"
};

fetch("https://wallet.paynocchio.com/wallet", requestOptions)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.error(error));
```
### Response Fields

When the wallet is successfully created, the response will return a dictionary with the following fields:

- `uuid` (`str`): The unique identifier for the newly created wallet (UUID4).
- `user_uuid` (`str`): The UUID of the user associated with the wallet (UUID4).
- `status` (`dict`): A dictionary representing the wallet's status:
  - `uuid` (`str`): The unique identifier for the status (UUID).
  - `updated_at` (`str`): The date and time (in ISO 8601 format) when the status was last updated.
  - `title` (`str`): A human-readable title describing the status.
  - `code` (`str`): A machine-readable code representing the status.
- `balance` (`dict`): A dictionary representing the wallet's balance:
  - `uuid` (`str`): The unique identifier for the balance (UUID).
  - `updated_at` (`str`): The date and time (in ISO 8601 format) when the balance was last updated.
  - `iso_currency_code` (`str`): The ISO currency code (e.g., "USD").
  - `current` (`int`): The current balance amount.
- `number` (`int`): The wallet number.
- `created_at` (`str`): The date and time (in ISO 8601 format) when the wallet was created.
- `rewarding_balance` (`int`): The rewarding balance associated with the wallet.
- `currency_uuid` (`str`): The unique identifier for the currency used in the wallet (UUID).

#### Example Response body
```json
{
    "uuid": "d559b7d7-c9fd-407e-8d72-f515f8231b1e",
    "updated_at": null,
    "user_uuid": "b74efaf6-bd46-404f-b4e8-fd87fdf985a0",
    "type": null,
    "status": {
        "uuid": "fac19a86-b1d2-4018-bdea-130554fd4d27",
        "updated_at": null,
        "title": "Active",
        "code": "ACTIVE"
    },
    "balance": {
        "uuid": "a9081a3e-b803-45f5-a0ae-bf8c19e72f66",
        "updated_at": null,
        "iso_currency_code": "USD",
        "current": 0.0
    },
    "number": 6874419269574273,
    "created_at": "2024-10-16T11:26:51.970745Z",
    "rewarding_balance": 0.0,
    "currency_uuid": "e3491e72-df67-4e1c-8c51-1b1f845a8d5e"
}
```

## Update Wallet Status

Updates the status of a wallet identified by its UUID.

This method allows you to suspend, activate, or block a wallet.  
`ATTENTION!`: Once a wallet is blocked, it cannot be activated again.
---

### Endpoint
`PATCH /wallet`

### Headers
- `X-Wallet-Signature` (str): A SHA256 signature for authentication (ensure it's correct).
- `X-Test-Mode-Switch` (str): A flag to enable or disable test mode (values: "on" or "off").

### Request Parameters
- `environment_uuid` (UUID, required): The unique identifier of the wallet group where the operation is performed.
- `user_uuid` (UUID, required): The unique identifier of the user whose wallet will be credited.
- `user_uuid` (str): The UUID of the user from the external system (required).
- `status_uuid` (str): The UUID of the new status to be assigned to the wallet (required).

_Note:_ Status codes could be obtained from `/status` route

### Example Code
cURL
```curl
curl --location --request PATCH 'https://wallet.paynocchio.com/wallet/' \
--header 'X-Wallet-Signature: d549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5' \
--header 'X-Test-Mode-Switch: on' \
--header 'Content-Type: application/json' \
--data '{
"uuid": "57b3809f-a80a-4fee-829a-35424213e943",
"environment_uuid": "8c6b143d-df21-42ee-8a53-c7f19b274982",
"user_uuid": "450f6b66-f7d6-4f4b-a849-ddf3636778a6",
"status_uuid": "ef8da49e-a9e3-4726-8c26-f8d2bfd6a093" 
}'
```
Python
```python
import requests
import json

url = "https://wallet.paynocchio.com/wallet/"

payload = json.dumps({
  "uuid": "4b797111-9abf-4319-94f9-3687b03f8b1f",
  "environment_uuid": "287d5047-a999-458f-86b9-c29bdf8ed745",
  "user_uuid": "450f6b66-f7d6-4f4b-a849-ddf3636778a6",
  "status_uuid": "ef8da49e-a9e3-4726-8c26-f8d2bfd6a093"
})
headers = {
  'X-Wallet-Signature': '82e3a2b6cdfcf86bbe0dc1c078366ba4cbbe943952c9c602d12e933bf9a020d5',
  'X-Test-Mode-Switch': 'on',
  'Content-Type': 'application/json'
}

response = requests.request("PATCH", url, headers=headers, data=payload)
```
Java
```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("application/json");
RequestBody body = RequestBody.create(mediaType, "{\n\"uuid\": \"57b3809f-a80a-4fee-829a-35424213e943\",\n\"environment_uuid\": \"8c6b143d-df21-42ee-8a53-c7f19b274982\",\n\"user_uuid\": \"450f6b66-f7d6-4f4b-a849-ddf3636778a6\",\n\"status_uuid\": \"ef8da49e-a9e3-4726-8c26-f8d2bfd6a093\" \n\n}");
Request request = new Request.Builder()
  .url("https://wallet.paynocchio.com/wallet/")
  .method("PATCH", body)
  .addHeader("X-Wallet-Signature", "d549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5")
  .addHeader("X-Test-Mode-Switch", "on")
  .addHeader("Content-Type", "application/json")
  .build();
Response response = client.newCall(request).execute();
```
Javascript
```js
const myHeaders = new Headers();
myHeaders.append("X-Wallet-Signature", "d549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5");
myHeaders.append("X-Test-Mode-Switch", "on");
myHeaders.append("Content-Type", "application/json");

const raw = JSON.stringify({
  "uuid": "57b3809f-a80a-4fee-829a-35424213e943",
  "environment_uuid": "8c6b143d-df21-42ee-8a53-c7f19b274982",
  "user_uuid": "450f6b66-f7d6-4f4b-a849-ddf3636778a6",
  "status_uuid": "ef8da49e-a9e3-4726-8c26-f8d2bfd6a093"
});

const requestOptions = {
  method: "PATCH",
  headers: myHeaders,
  body: raw,
  redirect: "follow"
};

fetch("https://wallet.paynocchio.com/wallet/", requestOptions)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.error(error));
```
### Response Fields

A dictionary containing the updated details of the wallet with the following fields:

- `uuid` (`str`): The unique identifier for the wallet (UUID4).
- `updated_at` (`str`, ISO 8601 format): The date and time when the wallet status was last updated.
- `user_uuid` (`str`): The UUID of the user associated with the wallet (UUID4).
- `type` (`dict`): A dictionary representing the type of the wallet, containing:
  - `uuid` (`str`): The unique identifier for the wallet type (UUID).
  - `updated_at` (`str`, ISO 8601 format): The date and time when the wallet type was last updated.
  - `title` (`str`): A human-readable title describing the wallet type.
  - `description` (`str`): A detailed description of the wallet type.
- `status` (`dict`): A dictionary representing the current status of the wallet, containing:
  - `uuid` (`str`): The unique identifier for the status (UUID).
  - `updated_at` (`str`, ISO 8601 format): The date and time when the status was last updated.
  - `title` (`str`): A human-readable title describing the status.
  - `code` (`str`): A machine-readable code representing the status.
- `balance` (`dict`): A dictionary representing the wallet's balance, containing:
  - `uuid` (`str`): The unique identifier for the balance (UUID).
  - `updated_at` (`str`, ISO 8601 format): The date and time when the balance was last updated.
  - `iso_currency_code` (`str`): The ISO currency code (e.g., "USD").
  - `current` (`int`): The current balance amount.
- `number` (`int`): The wallet number.
- `created_at` (`str`, ISO 8601 format): The date and time when the wallet was created.
- `rewarding_balance` (`int`): The rewarding balance associated with the wallet.
- `currency_uuid` (`str`): The unique identifier for the currency used in the wallet (UUID).

#### Example Response body
```json
{
    "uuid": "57b3809f-a80a-4fee-829a-35424213e943",
    "updated_at": "2024-10-17T13:40:24.293263Z",
    "user_uuid": "450f6b66-f7d6-4f4b-a849-ddf3636778a6",
    "type": {
        "uuid": "93ac9017-4960-41bf-be6d-aa123884451d",
        "updated_at": null,
        "title": "ACTIVE",
        "description": "ACTIVE"
    },
    "status": {
        "uuid": "ef8da49e-a9e3-4726-8c26-f8d2bfd6a093",
        "updated_at": null,
        "title": "Active",
        "code": "ACTIVE"
    },
    "balance": {
        "uuid": "835ea0e3-d739-4ffe-8465-6a1a07839141",
        "updated_at": null,
        "iso_currency_code": "USD",
        "current": 0.0
    },
    "number": 6568850618975225,
    "created_at": "2024-10-16T12:48:46.648789Z",
    "rewarding_balance": 0.0,
    "currency_uuid": "e3491e72-df67-4e1c-8c51-1b1f845a8d5e"
}
```


## Get Wallet Group Structure

Retrieves the current structure of the wallet group, including settings and rewarding groups with rules.

This method provides detailed information about the wallet group, such as company UUID, created date,
    transaction limits, and rules for rewarding groups.
---

### Endpoint
`GET /wallet/environment-structure`

### Headers
- `X-Wallet-Signature` (str): A SHA256 signature for authentication (ensure it's correct).
- `X-Test-Mode-Switch` (str): A flag to enable or disable test mode (values: "on" or "off").

### Request Parameters
- `environment_uuid` (UUID, required): The unique identifier of the wallet group where the operation is performed.
- `user_uuid` (UUID, required): The unique identifier of the user whose wallet will be credited.

### Example Code
cURL
```curl
curl --location 'https://wallet.paynocchio.com/wallet/environment-structure?user_uuid=450f6b66-f7d6-4f4b-a849-ddf3636778a6&environment_uuid=8c6b143d-df21-42ee-8a53-c7f19b274982' \
--header 'X-Wallet-Signature: d549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5' \
--header 'X-Test-Mode-Switch: on'
```
Python
```python
import requests

url = "https://wallet.paynocchio.com/wallet/environment-structure?user_uuid=450f6b66-f7d6-4f4b-a849-ddf3636778a6&environment_uuid=8c6b143d-df21-42ee-8a53-c7f19b274982"

payload = {}
headers = {
  'X-Wallet-Signature': 'd549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5',
  'X-Test-Mode-Switch': 'on'
}

response = requests.request("GET", url, headers=headers, data=payload)
```
Java
```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("text/plain");
RequestBody body = RequestBody.create(mediaType, "");
Request request = new Request.Builder()
  .url("https://wallet.paynocchio.com/wallet/environment-structure?user_uuid=450f6b66-f7d6-4f4b-a849-ddf3636778a6&environment_uuid=8c6b143d-df21-42ee-8a53-c7f19b274982")
  .method("GET", body)
  .addHeader("X-Wallet-Signature", "d549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5")
  .addHeader("X-Test-Mode-Switch", "on")
  .build();
Response response = client.newCall(request).execute();
```
Javascript
```js
const myHeaders = new Headers();
myHeaders.append("X-Wallet-Signature", "d549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5");
myHeaders.append("X-Test-Mode-Switch", "on");

const requestOptions = {
  method: "GET",
  headers: myHeaders,
  redirect: "follow"
};

fetch("https://wallet.paynocchio.com/wallet/environment-structure?user_uuid=450f6b66-f7d6-4f4b-a849-ddf3636778a6&environment_uuid=8c6b143d-df21-42ee-8a53-c7f19b274982", requestOptions)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.error(error));
```
### Response Fields

When retrieving the wallet group, the response will return a dictionary containing the following fields:

- `uuid` (`str`): The unique identifier for the wallet group (UUID4).
- `title` (`str`): The title or name of the wallet group.
- `company_uuid` (`str`): The unique identifier for the company associated with this environment (UUID4).
- `created_at` (`str`, ISO 8601 format): The date and time when the wallet group was created.
- `keys_number` (`int`): The number of keys associated with the wallet.
- `card_balance_limit` (`int`): The limit on the card balance.
- `daily_transaction_limit` (`int`): The limit on daily transactions.
- `multiple_accounts_limit` (`int`): The limit on multiple accounts that can be created.
- `minimum_topup_amount` (`int`): The minimum amount that can be topped up.
- `bonus_conversion_rate` (`float`): The conversion rate for bonuses.
- `allow_withdraw` (`bool`): Indicates whether withdrawals are permitted.
- `rewarding_groups` (`list`): A list of rewarding groups associated with this wallet group, containing:
  - `title` (`str`): The title of the rewarding group.
  - `date_from` (`str`, ISO 8601 format): The start date of the rewarding group.
  - `date_to` (`str`, ISO 8601 format): The end date of the rewarding group.
  - `active` (`bool`): Indicates whether the rewarding group is currently active.
  - `rewarding_rules` (`list`): A list of rules associated with the rewarding group, containing:
    - `title` (`str`): The title of the rewarding rule.
    - `operation_type` (`str`): The type of operation to which this rule applies.
    - `value` (`int`): The value associated with the rule.
    - `value_type` (`str`): The type of value (e.g., "fixed", "percentage").
    - `min_amount` (`int`): The minimum amount applicable under this rule.
    - `max_amount` (`int`): The maximum amount applicable under this rule.
    - `conversion_rate` (`float`): The conversion rate for the rewarding rule.

#### Example Response body
```json
{
    "uuid": "8c6b143d-df21-42ee-8a53-c7f19b274982",
    "title": "Academy",
    "company_uuid": "25287fa6-3ddc-4847-a57b-ec50d69b5313",
    "created_at": "2024-10-15T14:27:54.228741Z",
    "keys_number": 1,
    "card_balance_limit": 500,
    "daily_transaction_limit": 2000,
    "multiple_accounts_limit": 10000,
    "minimum_topup_amount": 5,
    "bonus_conversion_rate": 0.01,
    "allow_withdraw": false,
    "rewarding_groups": [
        {
            "title": "Academy",
            "date_from": "2024-10-01T12:00:00",
            "date_to": "2024-11-30T12:00:00",
            "active": true,
            "rewarding_rules": [
                {
                    "title": "",
                    "operation_type": "payment_operation_add_money",
                    "value": 10,
                    "value_type": "percentage",
                    "min_amount": 1.0,
                    "max_amount": 1000.0,
                    "conversion_rate": 1.0
                }
            ]
        }
    ]
}
```


## Get Wallet Transaction history

Retrieves a paginated list of the wallet's transaction history.

This method allows users to fetch their wallet transaction records, with pagination support to navigate through multiple pages of results.

---
### Endpoint
`GET /wallet/transaction-history`

### Headers
- `X-Wallet-Signature` (str): A SHA256 signature for authentication (ensure it's correct).
- `X-Test-Mode-Switch` (str): A flag to enable or disable test mode (values: "on" or "off").

### Request Parameters
  - `user_uuid` (`str`): The UUID of the user in the external system (required).
  - `environment_uuid` (`str`): The UUID of the admin panel wallet group (required).
  - `wallet_uuid` (`str`): The UUID of the specific wallet (required).
  - `page` (`int`, optional): The page number to retrieve (default is `1`).
  - `size` (`int`, optional): The number of records per page (default is `30`).
  - `start` (`datetime`, optional): The start date for filtering transactions (default is `None`).
  - `end` (`datetime`, optional): The end date for filtering transactions (default is `None`).

### Example Code
cURL
```curl
curl --location 'https://wallet.paynocchio.com/wallet/transaction-history/?environment_uuid=8c6b143d-df21-42ee-8a53-c7f19b274982&user_uuid=450f6b66-f7d6-4f4b-a849-ddf3636778a6&wallet_uuid=57b3809f-a80a-4fee-829a-35424213e943&page=1&size=10' \
--header 'X-Wallet-Signature: d549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5' \
--header 'X-Test-Mode-Switch: on' \
--data ''
```
Python
```python
import requests

url = "https://wallet.paynocchio.com/wallet/transaction-history/?environment_uuid=8c6b143d-df21-42ee-8a53-c7f19b274982&user_uuid=450f6b66-f7d6-4f4b-a849-ddf3636778a6&wallet_uuid=57b3809f-a80a-4fee-829a-35424213e943&page=1&size=10"

payload = ""
headers = {
  'X-Wallet-Signature': 'd549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5',
  'X-Test-Mode-Switch': 'on'
}

response = requests.request("GET", url, headers=headers, data=payload)
```
Java
```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("text/plain");
RequestBody body = RequestBody.create(mediaType, "");
Request request = new Request.Builder()
  .url("https://wallet.paynocchio.com/wallet/transaction-history/?environment_uuid=8c6b143d-df21-42ee-8a53-c7f19b274982&user_uuid=450f6b66-f7d6-4f4b-a849-ddf3636778a6&wallet_uuid=57b3809f-a80a-4fee-829a-35424213e943&page=1&size=10")
  .method("GET", body)
  .addHeader("X-Wallet-Signature", "d549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5")
  .addHeader("X-Test-Mode-Switch", "on")
  .build();
Response response = client.newCall(request).execute();
```
Javascript
```js
const myHeaders = new Headers();
myHeaders.append("X-Wallet-Signature", "d549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5");
myHeaders.append("X-Test-Mode-Switch", "on");

const raw = "";

const requestOptions = {
  method: "GET",
  headers: myHeaders,
  body: raw,
  redirect: "follow"
};

fetch("https://wallet.paynocchio.com/wallet/transaction-history/?environment_uuid=8c6b143d-df21-42ee-8a53-c7f19b274982&user_uuid=450f6b66-f7d6-4f4b-a849-ddf3636778a6&wallet_uuid=57b3809f-a80a-4fee-829a-35424213e943&page=1&size=10", requestOptions)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.error(error));
```
### Response Fields

- **item** (`list`): A list of transaction records, where each record contains:
  - `uuid` (`str`): The unique identifier for the transaction (UUID4).
  - `external_request_id` (`str`): The external request identifier (UUID4).
  - `created_at` (`str`, ISO 8601 format): The date and time when the transaction was created.
  - `company_id` (`str`): The unique identifier for the company associated with the transaction (UUID4).
  - `payment_method` (`str`): The payment method used for the transaction (e.g., "credit_card", "bank_transfer").
  - `amount` (`int`): The amount involved in the transaction (in the smallest currency unit, e.g., cents).
  - `currency_id` (`str`, optional): The unique identifier for the currency (UUID4).
  - `wallet_uuid` (`str`): The UUID of the wallet related to the transaction (UUID4).
  - `user_uuid` (`str`): The UUID of the user who performed the transaction (UUID4).
  - `status` (`str`): The current status of the transaction (e.g., "completed", "pending").
- **total** (`int`): The total number of transactions matching the query.
- **page** (`int`): The current page number of the results.
- **size** (`int`): The number of records returned per page.
- **pages** (`int`): The total number of pages available.

#### Example Response body
```json
{
    "item": [],
    "total": 0,
    "page": 0,
    "size": 10,
    "pages": 0
}
```


## Calculate Commissions and Bonuses

Calculates commissions and bonuses based on the specified transaction amount for various operation types.

This method retrieves the commission and bonus calculations for a given transaction amount, considering different types of operations.

---
### Endpoint
`GET /wallet/structure_calculation`

### Headers
- `X-Wallet-Signature` (str): A SHA256 signature for authentication (ensure it's correct).
- `X-Test-Mode-Switch` (str): A flag to enable or disable test mode (values: "on" or "off").

### Request Parameters
 - **params** (`dict`): A dictionary containing the following parameters:
  - `user_uuid` (`str`): The UUID of the user in the external system (required).
  - `environment_uuid` (`str`): The UUID of the admin panel wallet group (required).
  - `wallet_uuid` (`str`): The UUID of the wallet (required).
  - `amount` (`float`): The amount of the transaction (required).
  - `operation_type` (`str`, optional): The name of the operation for which commissions and bonuses are calculated.
  - `wallet_balance_check` (`bool`, optional): A flag to enable or disable the check of the user's wallet balances (default is `False`).

### Example Code
cURL
```curl
curl --location 'https://wallet.paynocchio.com/wallet/structure_calculation?environment_uuid=8c6b143d-df21-42ee-8a53-c7f19b274982&amount=10&wallet_uuid=57b3809f-a80a-4fee-829a-35424213e943&user_uuid=450f6b66-f7d6-4f4b-a849-ddf3636778a6&operation_type=payment_operation_add_money&wallet_balance_check=false' \
--header 'X-Wallet-Signature: d549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5' \
--header 'X-Test-Mode-Switch: on'
```
Python
```python
import requests

url = "https://wallet.paynocchio.com/wallet/structure_calculation?environment_uuid=8c6b143d-df21-42ee-8a53-c7f19b274982&amount=10&wallet_uuid=57b3809f-a80a-4fee-829a-35424213e943&user_uuid=450f6b66-f7d6-4f4b-a849-ddf3636778a6&operation_type=payment_operation_add_money&wallet_balance_check=false"

payload = {}
headers = {
  'X-Wallet-Signature': 'd549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5',
  'X-Test-Mode-Switch': 'on'
}

response = requests.request("GET", url, headers=headers, data=payload)
```
Java
```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("text/plain");
RequestBody body = RequestBody.create(mediaType, "");
Request request = new Request.Builder()
  .url("https://wallet.paynocchio.com/wallet/structure_calculation?environment_uuid=8c6b143d-df21-42ee-8a53-c7f19b274982&amount=10&wallet_uuid=57b3809f-a80a-4fee-829a-35424213e943&user_uuid=450f6b66-f7d6-4f4b-a849-ddf3636778a6&operation_type=payment_operation_add_money&wallet_balance_check=false")
  .method("GET", body)
  .addHeader("X-Wallet-Signature", "d549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5")
  .addHeader("X-Test-Mode-Switch", "on")
  .build();
Response response = client.newCall(request).execute();
```
Javascript
```js
const myHeaders = new Headers();
myHeaders.append("X-Wallet-Signature", "d549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5");
myHeaders.append("X-Test-Mode-Switch", "on");

const requestOptions = {
  method: "GET",
  headers: myHeaders,
  redirect: "follow"
};

fetch("https://wallet.paynocchio.com/wallet/structure_calculation?environment_uuid=8c6b143d-df21-42ee-8a53-c7f19b274982&amount=10&wallet_uuid=57b3809f-a80a-4fee-829a-35424213e943&user_uuid=450f6b66-f7d6-4f4b-a849-ddf3636778a6&operation_type=payment_operation_add_money&wallet_balance_check=false", requestOptions)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.error(error));
```
### Response Fields
A dictionary containing the commission and bonus calculations with the following fields:

- **environment_uuid** (`str`): The UUID of the wallet group in which the operation is performed.
- **wallet_uuid** (`str`): The UUID of the wallet related to the operation.
- **conversion_rate** (`float`): The conversion rate applicable to the transaction.
- **operations_data** (`list`): A list of operation details, where each entry contains:
  - `type_operation` (`str`): The type of operation (e.g., "payment_operation_add_money", "payment_operation_for_services").
  - `full_amount` (`float`): The total amount involved in the operation.
  - `bonuses_amount` (`int`): The amount of bonuses earned from the operation.
  - `commission_amount` (`float`): The commission charged for the operation.

#### Example Response body
```json
{
    "environment_uuid": "8c6b143d-df21-42ee-8a53-c7f19b274982",
    "wallet_uuid": "57b3809f-a80a-4fee-829a-35424213e943",
    "conversion_rate": "1.0",
    "operations_data": [
        {
            "type_operation": "payment_operation_add_money",
            "full_amount": "9.41",
            "bonuses_amount": "0",
            "commission_amount": "0.59"
        }
    ]
}
```


## Retrieve Wallets by Wallet Group

Retrieves a list of wallets associated with a specific wallet group.

This method fetches all wallets within the given wallet group, providing relevant details such as status, balance, and creation date.

---
### Endpoint
`GET /wallet/{env_uuid}/get_list`

### Headers
- `X-Wallet-Signature` (str): A SHA256 signature for authentication (ensure it's correct).
- `X-Test-Mode-Switch` (str): A flag to enable or disable test mode (values: "on" or "off").

### Request Parameters
- **environment_uuid** (`str`): The UUID of the wallet group for which to retrieve the wallet list (required).
- **params** (`dict`): A dictionary containing optional query parameters for the request. Possible keys include:
  - `user_uuid` (`str`, optional): The UUID of a specific user to filter the wallets.
  - `status_code` (`str`, optional): The status code to filter wallets based on their current status.
  - `size` (`int`, optional): The maximum number of wallets to return in the response.
  - `page` (`int`, optional): The page number for pagination (default is `1`).

### Example Code
cURL
```curl
curl --location 'https://wallet.paynocchio.com/wallet/8c6b143d-df21-42ee-8a53-c7f19b274982/get_list?user_uuid=450f6b66-f7d6-4f4b-a849-ddf3636778a6&page=1&size=10' \
--header 'X-Wallet-Signature: d549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5' \
--header 'X-Test-Mode-Switch: on' \
--data ''
```
Python
```python
import requests

url = "https://wallet.paynocchio.com/wallet/8c6b143d-df21-42ee-8a53-c7f19b274982/get_list?user_uuid=450f6b66-f7d6-4f4b-a849-ddf3636778a6&page=1&size=10"

payload = ""
headers = {
  'X-Wallet-Signature': 'd549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5',
  'X-Test-Mode-Switch': 'on'
}

response = requests.request("GET", url, headers=headers, data=payload)
```
Java
```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("text/plain");
RequestBody body = RequestBody.create(mediaType, "");
Request request = new Request.Builder()
  .url("https://wallet.paynocchio.com/wallet/8c6b143d-df21-42ee-8a53-c7f19b274982/get_list?user_uuid=450f6b66-f7d6-4f4b-a849-ddf3636778a6&page=1&size=10")
  .method("GET", body)
  .addHeader("X-Wallet-Signature", "d549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5")
  .addHeader("X-Test-Mode-Switch", "on")
  .build();
Response response = client.newCall(request).execute();
```
Javascript
```js
const myHeaders = new Headers();
myHeaders.append("X-Wallet-Signature", "d549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5");
myHeaders.append("X-Test-Mode-Switch", "on");

const raw = "";

const requestOptions = {
  method: "GET",
  headers: myHeaders,
  body: raw,
  redirect: "follow"
};

fetch("https://wallet.paynocchio.com/wallet/8c6b143d-df21-42ee-8a53-c7f19b274982/get_list?user_uuid=450f6b66-f7d6-4f4b-a849-ddf3636778a6&page=1&size=10", requestOptions)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.error(error));
```
### Response Fields
A dictionary containing the following fields:

- **item** (`list`): A list of wallet details, where each wallet is represented as a dictionary with the following structure:
  - **uuid** (`str`): The UUID of the wallet.
  - **user_uuid** (`str`): The UUID of the user associated with the wallet.
  - **status** (`dict`): A dictionary representing the status of the wallet, containing:
    - **uuid** (`str`): The UUID of the status.
    - **updated_at** (`datetime`): The timestamp when the status was last updated.
    - **title** (`str`): A human-readable title for the status.
    - **code** (`str`): A code representing the status.
  - **balance** (`dict`): A dictionary representing the wallet balance, containing:
    - **uuid** (`str`): The UUID of the balance record.
    - **updated_at** (`datetime`): The timestamp when the balance was last updated.
    - **iso_currency_code** (`str`): The ISO 4217 currency code for the wallet's currency.
    - **current** (`int`): The current balance amount in the specified currency.
  - **number** (`int`): The wallet number or identifier.
  - **created_at** (`datetime`): The timestamp when the wallet was created.
  - **updated_at** (`datetime | None`): The timestamp when the wallet was last updated, or `None` if not applicable.
  - **rewarding_balance** (`int`): The rewarding balance amount associated with the wallet.
  - **currency_uuid** (`str`): The UUID of the currency used for the wallet.

#### Example Response body
```json
{
    "item": [
        {
            "uuid": "fa2d03ef-3c28-48a2-8bc9-0d23963bb3f2",
            "updated_at": null,
            "user_uuid": "b74efaf6-bd46-404f-b4e8-fd87fdf985a0",
            "status": {
                "uuid": "fac19a86-b1d2-4018-bdea-130554fd4d27",
                "updated_at": null,
                "title": "Active",
                "code": "ACTIVE"
            },
            "balance": {
                "uuid": "7a2c1ed3-1650-4fd8-b40a-17721c276354",
                "updated_at": null,
                "iso_currency_code": "USD",
                "current": 0.0
            },
            "number": 6141529991213323,
            "created_at": "2024-10-15T13:53:08.857023Z",
            "rewarding_balance": 0.0,
            "currency_uuid": "e3491e72-df67-4e1c-8c51-1b1f845a8d5e"
        }
    ],
    "total": 2,
    "page": 1,
    "size": 50,
    "pages": 1
}
```


## Retrieve Wallets by User in a Wallet Group

Retrieves a list of wallets associated with a specific user in a given wallet group.

This method fetches all wallets that belong to the specified user within the provided wallet group.

---
### Endpoint
`GET /wallet/{env_uuid}/get_wallet_from_uuid/{user_uuid}`

### Headers
- `X-Wallet-Signature` (str): A SHA256 signature for authentication (ensure it's correct).
- `X-Test-Mode-Switch` (str): A flag to enable or disable test mode (values: "on" or "off").

### Request Parameters
- **environment_uuid** (`str`): The UUID of the wallet group from which to retrieve the wallets (required).
- **user_uuid** (`str`): The UUID of the user whose wallets are being retrieved (required).
- **params** (`dict`): A dictionary containing optional query parameters for the request. Possible keys include:
  - `size` (`int`, optional): The maximum number of wallets to return in the response.
  - `page` (`int`, optional): The page number for pagination (default is `1`).
  - `status_code` (`str`, optional): The status code to filter wallets based on their current status.

### Example Code
Python
cURL
```curl
curl --location 'https://wallet.paynocchio.com/wallet/8c6b143d-df21-42ee-8a53-c7f19b274982/get_wallet_from_uuid/450f6b66-f7d6-4f4b-a849-ddf3636778a6?page=1&size=50' \
--header 'X-Wallet-Signature: d549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5' \
--header 'X-Test-Mode-Switch: on' \
--data ''
```
```python
import requests

url = "https://wallet.paynocchio.com/wallet/8c6b143d-df21-42ee-8a53-c7f19b274982/get_wallet_from_uuid/450f6b66-f7d6-4f4b-a849-ddf3636778a6?page=1&size=50"

payload = ""
headers = {
  'X-Wallet-Signature': 'd549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5',
  'X-Test-Mode-Switch': 'on'
}

response = requests.request("GET", url, headers=headers, data=payload)
```
Java
```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("text/plain");
RequestBody body = RequestBody.create(mediaType, "");
Request request = new Request.Builder()
  .url("https://wallet.paynocchio.com/wallet/8c6b143d-df21-42ee-8a53-c7f19b274982/get_wallet_from_uuid/450f6b66-f7d6-4f4b-a849-ddf3636778a6?page=1&size=50")
  .method("GET", body)
  .addHeader("X-Wallet-Signature", "d549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5")
  .addHeader("X-Test-Mode-Switch", "on")
  .build();
Response response = client.newCall(request).execute();
```
Javascript
```js
const myHeaders = new Headers();
myHeaders.append("X-Wallet-Signature", "d549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5");
myHeaders.append("X-Test-Mode-Switch", "on");

const raw = "";

const requestOptions = {
  method: "GET",
  headers: myHeaders,
  body: raw,
  redirect: "follow"
};

fetch("https://wallet.paynocchio.com/wallet/8c6b143d-df21-42ee-8a53-c7f19b274982/get_wallet_from_uuid/450f6b66-f7d6-4f4b-a849-ddf3636778a6?page=1&size=50", requestOptions)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.error(error));
```
### Response Fields
A dictionary containing the following fields:

- **item** (`list`): A list of wallet details, where each wallet is represented as a dictionary with the following structure:
  - **uuid** (`str`): The UUID of the wallet.
  - **user_uuid** (`str`): The UUID of the user associated with the wallet.
  - **status** (`dict`): A dictionary representing the status of the wallet, containing:
    - **uuid** (`str`): The UUID of the status.
    - **updated_at** (`datetime`): The timestamp when the status was last updated.
    - **title** (`str`): A human-readable title for the status.
    - **code** (`str`): A code representing the status.
  - **balance** (`dict`): A dictionary representing the wallet balance, containing:
    - **uuid** (`str`): The UUID of the balance record.
    - **updated_at** (`datetime`): The timestamp when the balance was last updated.
    - **iso_currency_code** (`str`): The ISO 4217 currency code for the wallet's currency.
    - **current** (`int`): The current balance amount in the specified currency.
  - **number** (`int`): The wallet number or identifier.
  - **created_at** (`datetime`): The timestamp when the wallet was created.
  - **updated_at** (`datetime | None`): The timestamp when the wallet was last updated, or `None` if not applicable.
  - **rewarding_balance** (`int`): The rewarding balance amount associated with the wallet.
  - **currency_uuid** (`str`): The UUID of the currency used for the wallet.

#### Example Response body
```json
{
    "item": [
        {
            "uuid": "fa2d03ef-3c28-48a2-8bc9-0d23963bb3f2",
            "updated_at": null,
            "user_uuid": "b74efaf6-bd46-404f-b4e8-fd87fdf985a0",
            "status": {
                "uuid": "fac19a86-b1d2-4018-bdea-130554fd4d27",
                "updated_at": null,
                "title": "Active",
                "code": "ACTIVE"
            },
            "balance": {
                "uuid": "7a2c1ed3-1650-4fd8-b40a-17721c276354",
                "updated_at": null,
                "iso_currency_code": "USD",
                "current": 0.0
            },
            "number": 6141529991213323,
            "created_at": "2024-10-15T13:53:08.857023Z",
            "rewarding_balance": 0.0,
            "currency_uuid": "e3491e72-df67-4e1c-8c51-1b1f845a8d5e"
        }
    ],
    "total": 2,
    "page": 1,
    "size": 50,
    "pages": 1
}
```


## Retrieve Wallet Details

Retrieves detailed information about a specific wallet, including its balance.

This method fetches the wallet's details using its unique identifier, along with the associated user and wallet group information.

---
### Endpoint
`GET /wallet/{wallet_uuid}`

### Headers
- `X-Wallet-Signature` (str): A SHA256 signature for authentication (ensure it's correct).
- `X-Test-Mode-Switch` (str): A flag to enable or disable test mode (values: "on" or "off").

### Request Parameters
- **wallet_id** (`str`): The UUID of the wallet to be retrieved (required).
- **params** (`dict`): A dictionary of optional query parameters, which may include:
  - `environment_uuid` (`str`): The UUID of the admin panel wallet group associated with the wallet.
  - `user_uuid` (`str`): The UUID of the external system user associated with the wallet.

### Example Code
cURL
```curl
curl --location 'https://wallet.paynocchio.com/wallet/57b3809f-a80a-4fee-829a-35424213e943?environment_uuid=8c6b143d-df21-42ee-8a53-c7f19b274982&user_uuid=450f6b66-f7d6-4f4b-a849-ddf3636778a6' \
--header 'X-Wallet-Signature: d549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5' \
--header 'X-Test-Mode-Switch: on' \
--data ''
```
Python
```python
import requests

url = "https://wallet.paynocchio.com/wallet/57b3809f-a80a-4fee-829a-35424213e943?environment_uuid=8c6b143d-df21-42ee-8a53-c7f19b274982&user_uuid=450f6b66-f7d6-4f4b-a849-ddf3636778a6"

payload = ""
headers = {
  'X-Wallet-Signature': 'd549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5',
  'X-Test-Mode-Switch': 'on'
}

response = requests.request("GET", url, headers=headers, data=payload)
```
Java
```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("text/plain");
RequestBody body = RequestBody.create(mediaType, "");
Request request = new Request.Builder()
  .url("https://wallet.paynocchio.com/wallet/57b3809f-a80a-4fee-829a-35424213e943?environment_uuid=8c6b143d-df21-42ee-8a53-c7f19b274982&user_uuid=450f6b66-f7d6-4f4b-a849-ddf3636778a6")
  .method("GET", body)
  .addHeader("X-Wallet-Signature", "d549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5")
  .addHeader("X-Test-Mode-Switch", "on")
  .build();
Response response = client.newCall(request).execute();
```
Javascript
```js
const myHeaders = new Headers();
myHeaders.append("X-Wallet-Signature", "d549194d0f718d2e07029939b0265fe9f5045d5a3b812ec95c5f7b84544155f5");
myHeaders.append("X-Test-Mode-Switch", "on");

const raw = "";

const requestOptions = {
  method: "GET",
  headers: myHeaders,
  body: raw,
  redirect: "follow"
};

fetch("https://wallet.paynocchio.com/wallet/57b3809f-a80a-4fee-829a-35424213e943?environment_uuid=8c6b143d-df21-42ee-8a53-c7f19b274982&user_uuid=450f6b66-f7d6-4f4b-a849-ddf3636778a6", requestOptions)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.error(error));
```

### Response Fields
A dictionary containing the following fields:

- **uuid** (`str`): The UUID of the wallet.
- **user_uuid** (`str`): The UUID of the user associated with the wallet.
- **status** (`dict`): A dictionary representing the status of the wallet, containing:
  - **uuid** (`str`): The UUID of the status.
  - **updated_at** (`datetime`): The timestamp when the status was last updated.
  - **title** (`str`): A human-readable title for the status.
  - **code** (`str`): A code representing the status.
- **balance** (`dict`): A dictionary representing the wallet balance, containing:
  - **uuid** (`str`): The UUID of the balance record.
  - **updated_at** (`datetime`): The timestamp when the balance was last updated.
  - **iso_currency_code** (`str`): The ISO 4217 currency code for the wallet's currency.
  - **current** (`int`): The current balance amount in the specified currency.
- **number** (`int`): The wallet's unique identifier or number.
- **created_at** (`datetime`): The timestamp when the wallet was created.
- **updated_at** (`datetime | None`): The timestamp when the wallet was last updated, or `None` if not applicable.
- **rewarding_balance** (`int`): The current rewarding balance associated with the wallet.
- **currency_uuid** (`str`): The UUID of the currency used for the wallet.

#### Example Response body
```json
{
    "uuid": "4b797111-9abf-4319-94f9-3687b03f8b1f",
    "updated_at": null,
    "user_uuid": "b74efaf6-bd46-404f-b4e8-fd87fdf985a0",
    "type": null,
    "status": {
        "uuid": "fac19a86-b1d2-4018-bdea-130554fd4d27",
        "updated_at": null,
        "title": "Active",
        "code": "ACTIVE"
    },
    "currency": null,
    "balance": {
        "uuid": "94b4e35f-70e7-47ef-9bc6-874d90558946",
        "updated_at": null,
        "iso_currency_code": "USD",
        "current": 0.0
    },
    "number": 6321915855966267,
    "created_at": "2024-10-15T13:03:07.347750Z",
    "rewarding_balance": 0.0,
    "currency_uuid": "e3491e72-df67-4e1c-8c51-1b1f845a8d5e"
}
```
