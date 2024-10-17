<p align="center">
<img style="align:center;" src="resources/logo.png" alt="Paynocchio Logo" width="100"/>
<h1 align="center">Paynocchio</h1>
</p>

**[Quickstart](#Quickstart)** |
**[Documentation](https://github.com/PAYNOCCHIO/paynocchio-api-alpha/tree/doc/readme-feature/API)** |
**[Site](https://paynocchio.com/)** |
**[License](#License)** |
**[Team](#Team)** |
**[Getting help](#FAQ)** |

## Overview
PAYNOCCHIO is more than a typical payment processing service. We provide a comprehensive solution, operating as a ledger infrastructure. Our integrated services include roles as an issuing processor and program manager, along with offering closed-loop wallets. This enhances our extensive capabilities in payment processing.

Welcome to the **Wallet API**. Our API allows developers to integrate with our platform to manage wallets, transactions, and rewards seamlessly. Follow the steps below to register, configure environments, and start using the API.

## Getting Started

To begin using our API, you need to follow these steps:

1. Visit the [Paynocchio site](https://paynocchio.com).
2. Provide your email and necessary company information.
3. Confirm registration
4. Create and setup wallet group at Paynocchio Control Panel
5. Add API key at the Development section of the Control Panel

Please pay attention that you can use our API without KYC procedure. In this case do not forget to pass X-Test-Mode-Switch = on header to all requests.

## Table of contents

- [Healthcheck](https://github.com/PAYNOCCHIO/paynocchio-api-alpha/tree/main/API/health.md)
  - [DB status](https://github.com/PAYNOCCHIO/paynocchio-api-alpha/tree/main/API/health.md#Check_Database_Health_Status)
  - [Signature validation](https://github.com/PAYNOCCHIO/paynocchio-api-alpha/tree/main/API/health.md#Validate_API_Key_and_Wallet_Group_UUID)
- [Wallet manipulation](https://github.com/PAYNOCCHIO/paynocchio-api-alpha/tree/main/API/wallet.md)
  - [Create Wallet](https://github.com/PAYNOCCHIO/paynocchio-api-alpha/tree/main/API/wallet.md#Create_Wallet)
  - [Update Wallet Status](https://github.com/PAYNOCCHIO/paynocchio-api-alpha/tree/main/API/wallet.md#Update_Wallet_Status)
  - [Get Wallet Group Structure](https://github.com/PAYNOCCHIO/paynocchio-api-alpha/tree/main/API/wallet.md#Get_Wallet_Group_Structure)
  - [Get Wallet Transaction history](https://github.com/PAYNOCCHIO/paynocchio-api-alpha/tree/main/API/wallet.md#Get_Wallet_Transaction_history)
  - [Calculate Commissions and Bonuses](https://github.com/PAYNOCCHIO/paynocchio-api-alpha/tree/main/API/wallet.md#Calculate_Commissions_and_Bonuses)
  - [Retrieve Wallets by Wallet Group](https://github.com/PAYNOCCHIO/paynocchio-api-alpha/tree/main/API/wallet.md#Retrieve_Wallets_by_Wallet_Group)
  - [Retrieve Wallets by User in a Wallet Group](https://github.com/PAYNOCCHIO/paynocchio-api-alpha/tree/main/API/wallet.md#Retrieve_Wallets_by_User_in_a_Wallet_Group)
  - [Retrieve Wallet Details](https://github.com/PAYNOCCHIO/paynocchio-api-alpha/tree/main/API/wallet.md#Retrieve_Wallet_Details)
- [Operations](https://github.com/PAYNOCCHIO/paynocchio-api-alpha/tree/main/API/operation.md)
  - [Top Up Wallet](https://github.com/PAYNOCCHIO/paynocchio-api-alpha/tree/main/API/operation.md#Top_Up_Wallet)
  - [Payment from Wallet](https://github.com/PAYNOCCHIO/paynocchio-api-alpha/tree/main/API/operation.md#Payment_from_Wallet)
  - [Withdraw from Wallet](https://github.com/PAYNOCCHIO/paynocchio-api-alpha/tree/main/API/operation.md#Withdraw_from_Wallet)
- [Orders](https://github.com/PAYNOCCHIO/paynocchio-api-alpha/tree/main/API/order.md)
  - [Get Order by UUID](https://github.com/PAYNOCCHIO/paynocchio-api-alpha/tree/main/API/order.md#Get_Order_by_UUID)
  - [Get All Orders by Wallet UUID](https://github.com/PAYNOCCHIO/paynocchio-api-alpha/tree/main/API/order.md#Get_All_Orders_by_Wallet_UUID)
- [Wallet statuses](https://github.com/PAYNOCCHIO/paynocchio-api-alpha/tree/main/API/status.md)

### Troubleshooting and Error handing

In most cases developers will see standard error codes like 200, 201, 401, 404. 
We tried to make our API as user-friendly as possible so most of the responses are accompanied by descriptions.

Testing our own API we figured out that most errors occur from sending incorrect data, when the production environment and the test environment are confused. So please be very attentive to headers.


### X-WALLET-SIGNATURE generation logic
```python
def calculate_sha256(api_key, env_id, user_id):
    # Concatenate the decrypted API key, env_id, and user_id
    data_to_hash = f"{api_key}|{env_id}|{user_id}"
    # Calculate the SHA256 hash
    sha256_hash = hashlib.sha256(str(data_to_hash).lower().encode()).hexdigest()
 
    return sha256_hash
```

## Team

- __Abay Serkebayev__        | CEO / Founder
- __Mikhail Moiseev__        | System Analyst
- __Anastasia Maximova__     | Business Analyst
- __Alexander Mochinov__     | BackEnd Engineer
- __Semyon Berezovsky__      | BackEnd Engineer
- __Ivan Tsenilov__          | FrontEnd Engineer
- __Sergey Anishchenko__     | FrontEnd Engineer
- __Ilya Poplavsky__         | QA Engineer
- __Anton Abramenko__        | DevOps Engineer

## FAQ

- Where can I find information about the project
    - [Answer 1](https://paynocchio.com/)
- Where can I find the api documentation
    - [Answer 2](https://github.com/PAYNOCCHIO/paynocchio-api-alpha/)

## Changelog
See [CHANGELOG.md](https://github.com/PAYNOCCHIO/paynocchio-api-alpha/blob/main/CHANGELOG.md) for details.

## License
MIT [license](https://github.com/PAYNOCCHIO/paynocchio-api-alpha/blob/main/LICENSE)

## Stay in touch
[our site](https://paynocchio.com/team)

### Dev Meeting

We have videoconference meetings `<every week>` where we discuss what we have been working on and get feedback from one another.

[our mettings]()
