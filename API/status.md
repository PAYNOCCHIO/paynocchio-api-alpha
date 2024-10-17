## Retrieves Available Wallet Statuses

This method fetches a list of status objects that represent the various states a wallet can be in.

---

### Endpoint
`GET /status`

### Example code

```python
import requests

url = "https://wallet.paynocchio.com/status/"

payload = {}
headers = {
  'accept': 'application/json'
}

response = requests.request("GET", url, headers=headers, data=payload)
```

### Response

#### Example Response Body
```json
{
    "statuses": [
        {
            "uuid": "ae1b841f-2e56-4fb9-a935-2064304f8639",
            "updated_at": null,
            "title": "Status",
            "code": null
        },
        {
            "uuid": "ef8da49e-a9e3-4726-8c26-f8d2bfd6a093",
            "updated_at": null,
            "title": "Active",
            "code": "ACTIVE"
        },
        {
            "uuid": "88bba6c1-742d-4d8f-8d57-4c65a04bc19a",
            "updated_at": null,
            "title": "Suspend",
            "code": "SUSPEND"
        },
        {
            "uuid": "0eb3bf10-e806-4f21-b3fa-51f7fa568928",
            "updated_at": null,
            "title": "Blocked",
            "code": "BLOCKED"
        }
    ]
}
```

#### Response Fields

A dictionary containing the following key:

- **statuses** (`list`): A list of status objects, each containing:
  - **uuid** (`str`): The unique identifier for the status.
  - **updated_at** (`str`, ISO 8601 format): The date and time when the status was last updated.
  - **title** (`str`): A human-readable title describing the status.
  - **code** (`str`): A machine-readable code representing the status.