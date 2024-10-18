## Check Database Health Status

This method is used to check the health status of the database. It performs a simple health check to ensure that the database connection is functional and responsive.

---

### Endpoint
`GET /health`

### Example code
cURL
```curl
curl --location --request GET 'https://wallet.paynocchio.com/health/' \
--header 'Content-Type: application/json' \
--data '{
  "environment_uuid": "8c6b143d-df21-42ee-8a53-c7f19b274982",
  "secret_key": "367dcd6e-0ebf-42e6-8c20-3fcf529e694f"
}'
```
Python
```python
import requests

url = "https://wallet.paynocchio.com/health/"

payload = {}
headers = {}

response = requests.request("GET", url, headers=headers, data=payload)
```
Java
```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("application/json");
RequestBody body = RequestBody.create(mediaType, "{\n  \"environment_uuid\": \"8c6b143d-df21-42ee-8a53-c7f19b274982\",\n  \"secret_key\": \"367dcd6e-0ebf-42e6-8c20-3fcf529e694f\"\n}");
Request request = new Request.Builder()
  .url("https://wallet.paynocchio.com/health/")
  .method("GET", body)
  .addHeader("Content-Type", "application/json")
  .build();
Response response = client.newCall(request).execute();
```
Javascript
```js
const requestOptions = {
  method: "GET",
  redirect: "follow"
};

fetch("https://wallet.paynocchio.com/health/", requestOptions)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.error(error));
```


### Response
Example Response Body
```json
{
    "database": "ok"
}
```

#### Response Fields

A dictionary containing the database health status:

- **database** (`str`): A string indicating the health of the database. Possible value:
  - `"ok"`: The database is functioning properly.


## Validate API Key and Wallet Group UUID

This method checks whether the provided API key and wallet group UUID are valid for integration purposes. It ensures that the API key is recognized and that the wallet group is correctly configured.

---

### Endpoint
`POST /healthcheck`

### Request Parameters
- `environment_uuid` (UUID, required): The unique identifier of the wallet group where the operation is performed.
- `secret_key` (UUID, required): The API key of the wallet group obtained from Paynocchio control panel.

#### Example Request Body
```json
{
    "environment_uuid": "8c6b143d-df21-42ee-8a53-c7f19b274982",
    "secret_key": "450f6b66-f7d6-4f4b-a849-ddf3636778a6"
}
```
### Example code
cURL
```curl
curl --location 'https://wallet.paynocchio.com/healthcheck/' \
--header 'Content-Type: application/json' \
--data '{
  "environment_uuid": "8c6b143d-df21-42ee-8a53-c7f19b274982",
  "secret_key": "367dcd6e-0ebf-42e6-8c20-3fcf529e694f"
}'
```
Python
```python
import requests
import json

url = "https://wallet.paynocchio.com/healthcheck/"

payload = json.dumps({
  "environment_uuid": "8c6b143d-df21-42ee-8a53-c7f19b274982",
  "secret_key": "450f6b66-f7d6-4f4b-a849-ddf3636778a6"
})
headers = {
  'Content-Type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data=payload)
```
Java
```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("application/json");
RequestBody body = RequestBody.create(mediaType, "{\n  \"environment_uuid\": \"8c6b143d-df21-42ee-8a53-c7f19b274982\",\n  \"secret_key\": \"367dcd6e-0ebf-42e6-8c20-3fcf529e694f\"\n}");
Request request = new Request.Builder()
  .url("https://wallet.paynocchio.com/healthcheck/")
  .method("POST", body)
  .addHeader("Content-Type", "application/json")
  .build();
Response response = client.newCall(request).execute();
```
Javascript
```js
const myHeaders = new Headers();
myHeaders.append("Content-Type", "application/json");

const raw = JSON.stringify({
  "environment_uuid": "8c6b143d-df21-42ee-8a53-c7f19b274982",
  "secret_key": "367dcd6e-0ebf-42e6-8c20-3fcf529e694f"
});

const requestOptions = {
  method: "POST",
  headers: myHeaders,
  body: raw,
  redirect: "follow"
};

fetch("https://wallet.paynocchio.com/healthcheck/", requestOptions)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.error(error));
```
### Response
Example Response Body
```json
{
    "integration_status": "200",
    "message": "Integration is approved",
    "status_code": 200
}
```

#### Response Fields

A dictionary containing the integration status:

- **integration_status** (`str`): HTTP status code indicating the result of the validation (e.g., `"200"`).
- **message** (`str`): A message providing additional context, typically confirming successful validation (e.g., `"Integration is approved"`).
- **status_code** (`int`): The HTTP status code of the response.