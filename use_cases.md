# Create wallet for user
### For new user we must create new wallet inside paynocchio system for continue another wallet operation. Creation wallet using your system external user id(uuid). Paynocchio system create wallet inside our platform and you can get all information about wallet.
```python
import request
import json

# Create new user wallet.
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

# Get user wallet by user id from previous step
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
            user_id: "123",
            balance: 1000.00,
            currency: "USD", 
			status: "active"
        },
    ]
}  # True

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


# User withdraw from platform to user's bank account.
### For new user we must create new wallet inside paynocchio system for continue another wallet operation. If user want withdraw money we use this schema user -> your platform -> paynocchio system -> stripe -> paynocchio system get url from stripe for withdraw -> your platform get stripe url from paynocchio system for withdraw -> user fill all required fields like bank account information, first name, last name etc.
```python
import request
import json

# Create new user wallet.
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

# Get user wallet bu user id from prev step
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
            user_id: "123",
            balance: 1000.00,
            currency: "USD",
			status: "active"
        },
    ]
}  # True

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
response.json()["url"]  # Stripe withdraw url
```

# Transfer
### Transfer beetwen wallets not implemented yet

# Topup

```python
import request
import json

# Get user wallet
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
            user_id: "123",
            balance: 1000.00,
            currency: "USD",
			status: "active"
        },
    ]
}  # True

response = request.post(
    f"{url}/wallet/topup/",
    headers={
        "X-Wallet-Signature": "secret_t1CdN9S8yicG5eWLUOfhcWaOscVnFXns",
    },
    json=json.dumps({
        "env_id": 123,
	    "user_id": response.json()["wallets"][0]["user_id"],
	    "wallet_id": 123,
	    "amount": 100,
    })
)

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
response.json()["wallets"][0]["balance"] == 1100.00  # True
```

# Payments


```python
import request
import json

# Get user wallet
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
            user_id: "123",
            balance: 1000.00,
            currency: "USD",
			status: "active"
        },
    ]
}  # True

response = request.post(
    f"{url}/wallet/payment/",
    headers={
        "X-Wallet-Signature": "secret_t1CdN9S8yicG5eWLUOfhcWaOscVnFXns",
    },
    json=json.dumps({
        "env_id": 123,
	    "user_id": response.json()["wallets"][0]["user_id"],
	    "wallet_id": response.json()["wallets"][0]["id"],
	    "amount": 100,
	    "product_id": "asd123"
    })
)
response.status_code == 200  # True

response = request.get(
    f"{url}/wallet/",
    headers={
        "X-Wallet-Signature": "secret_t1CdN9S8yicG5eWLUOfhcWaOscVnFXns",
    },
    params={
        "env_id": 123,
	    "user_id": response.json()["wallets"][0]["user_id"],
    }
)
response.json()["wallets"][0]["balance"] == 900.00  # True

response = request.get(
    f'{url}/wallet/{response.json()["wallets"][0]["env_id"]}/orders/{response.json()["wallets"][0]["user_id"]}',
    headers={
        "X-Wallet-Signature": "secret_t1CdN9S8yicG5eWLUOfhcWaOscVnFXns",
    },
)
response.status_code == 200  # True
```