
> <h1>POST</h1>
> /wallet/

    Create wallet for specified user with specified environment

- Headers
    - X-Wallet-Signature: str
    - X-Test-Mode-Switch
- Body
    - environment_id: UUID Admin panel environment ID
    - user_uuid: UUID External system user id
- Response
    - Status code: 200 is OK
    - Status code: 401 
        - "msg": "Signature is invalid"tokens
    - Status code: 422 
        - "msg": "env_id and user_id is required"
    - Body:
        - wallet_id: UUID

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
### Javascript Fetch
```ejs
const myHeaders = new Headers();
myHeaders.append("X-Wallet-Signature", "b75e0c98c7c05a46c3397666452863d89433388eb016a31d88e5117721885c34");
myHeaders.append("X-Test-Mode-Switch", "on");
myHeaders.append("Content-Type", "application/json");

const raw = JSON.stringify({
  "user_uuid": "b74efaf6-bd46-404f-b4e8-fd87fdf985a0",
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
### PHP cURL
```php
$curl = curl_init();

curl_setopt_array($curl, array(
  CURLOPT_URL => 'https://wallet.stage.paynocchio.com/wallet',
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => '',
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 0,
  CURLOPT_FOLLOWLOCATION => true,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => 'POST',
  CURLOPT_POSTFIELDS =>'{
"user_uuid": "b74efaf6-bd46-404f-b4e8-fd87fdf985a0",
"environment_uuid": "8c6b143d-df21-42ee-8a53-c7f19b274982"
}',
  CURLOPT_HTTPHEADER => array(
    'X-Wallet-Signature: {{wallet_signature}}',
    'X-Test-Mode-Switch: on',
    'Content-Type: application/json'
  ),
));

$response = curl_exec($curl);

curl_close($curl);
echo $response;
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