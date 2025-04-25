# Payment methods

***

You can choose the payment methods that you want to display on the payment form.\
Use `methods` array in the Authentication request and specify the values that correspond to the payment methods.\
As well, you can use the value of the payment method to add specific parameters to the Authentification request for the `paramaters` object. These parameters will be sent to the acquirer to pass the necessary data for successful payment.

## card

If the payer chooses the `card` payment method, fields with card data will be displayed on the Checkout page.

For some connector services, it is necessary to send additional parameters in the Authenticaion request for the `paramaters` object (for more information, contact your manager).

### Additional parameters - Set 1 (BNG)

| **Parameter**          | **Type**                        | **Mandatory** |                                                                                                                             **Description**                                                                                                                             |
| ---------------------- | ------------------------------- | ------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| `bnrg_installm_def`    | _Numeric justified to 2 digits_ | Required      | <p>Indicates the number of months that will elapse from the purchase until the total or partial charge is made to the cardholder's account (initial deferral).<br><br>Possible values:<br><br><code>01</code> - one month<br><br><code>00</code> - no delay initial</p> |
| `bnrg_installm_months` | _Numeric justified to 2 digits_ | Required      |                                                         <p>Indicates the number of monthly payments in which the total amount of the transaction will be divided.<br><br>Example: <code>03</code> - 3 months</p>                                                        |
| `bnrg_installm_plan`   | _Numeric justified to 2 digits_ | Required      |                    <p>Indicates if the promotion It will be ́ with interest or without interest.<br><br>Possible values:<br><br><code>03</code> - no interest<br><br><code>05</code> - with interest<br><br><code>07</code> - defer only initial.</p>                   |

<details>

<summary>Authentication request example</summary>

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/session' \
--header 'Content-Type: application/json' \
--data-raw '{
    "merchant_key": "xxxxx-xxxxx-xxxxx",
    "operation": "purchase",
    "methods": [
        "card"
    ],
        "parameters": {
                "card": {
            "bnrg_installm_def": "03",
            "bnrg_installm_months": "03",
            "bnrg_installm_plan": "03"
        }
    },   
    "order": {
        "number": "order-1234",
        "amount": "1000.19",
        "currency": "MXN",
        "description": "Important gift"
    },
    "cancel_url": "https://example.com/cancel",
    "success_url": "https://example.com/success",
     "customer": {
        "name": "John Doe",
        "email": "test@gmail.com"
    },
    "billing_address": {
        "country": "US",
        "state": "CA",
        "city": "Los Angeles",
        "address": "Moor Building 35274",
        "zip": "123456",
        "phone": "347771112233"
    },
    "hash": "{{session_hash}}",
}
'
```

</details>

### Additional parameters - Set 2 (FCP)

| **Parameter**     | **Type** | **Mandatory** |                          **Description**                         |
| ----------------- | -------- | ------------- | :--------------------------------------------------------------: |
| `document_type`   | _String_ | Required      | Type of the document number specified above. Possibe values: cpf |
| `document_number` | _String_ | Required      |                     Client’s document number                     |
| `social_name`     | _String_ | Required      |                       Client's social name                       |
| `fiscal_country`  | _String_ | Required      |       An alpha-2 country code of the customer’s tax country      |

<details>

<summary>Authentication request example</summary>

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/session' \
--header 'Content-Type: application/json' \
--data-raw '{
    "merchant_key": "xxxxx-xxxxx-xxxxx",
    "operation": "purchase",
    "methods": [
        "card"
    ],
        "parameters": {
                "card": {
            "document_type": "cpf",
            "document_number": "231.002.999-00",
            "social_name": "Harry Potter",
            "fiscal_country": "BR",
            "toBankAccountId": "a1a1134a-32c6-442c-90c9-66b587d5be00"
        }
    },   
    "order": {
        "number": "order-1234",
        "amount": "1000.19",
        "currency": "BRL",
        "description": "Important gift"
    },
    "cancel_url": "https://example.com/cancel",
    "success_url": "https://example.com/success",
     "customer": {
        "name": "John Doe",
        "email": "test@gmail.com"
    },
    "billing_address": {
        "country": "US",
        "state": "CA",
        "city": "Los Angeles",
        "address": "Moor Building 35274",
        "zip": "123456",
        "phone": "347771112233"
    },
    "hash": "{{session_hash}}",
}'
```

</details>

### Additional parameters - Set 3 (LBP)

| **Parameter** | **Type**                 | **Mandatory** |     **Description**    |
| ------------- | ------------------------ | ------------- | :--------------------: |
| `descriptor`  | _String_ (20 characters) | Optional      | Transaction descriptor |

<details>

<summary>Authentication request example</summary>

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/session' \
--header 'Content-Type: application/json' \
--data-raw '{
    "merchant_key": "xxxxx-xxxxx-xxxxx",
    "operation": "purchase",
    "methods": [
        "card"
    ],
        "parameters": {
                "card": {
            "descriptor": "Testy International LTD"
        }
    },   
    "order": {
        "number": "order-1234",
        "amount": "1000.19",
        "currency": "BRL",
        "description": "Important gift"
    },
    "cancel_url": "https://example.com/cancel",
    "success_url": "https://example.com/success",
     "customer": {
        "name": "John Doe",
        "email": "test@gmail.com"
    },
    "billing_address": {
        "country": "US",
        "state": "CA",
        "city": "Los Angeles",
        "address": "Moor Building 35274",
        "zip": "123456",
        "phone": "347771112233"
    },
    "hash": "{{session_hash}}",
}
'
```

</details>

### Additional parameters - set 4 TWD

| **Parameter**       | **Type** | **Mandatory** |                                   **Description**                                   |
| ------------------- | -------- | ------------- | :---------------------------------------------------------------------------------: |
| `customer_phone`    | _String_ | Optional      |                            Phone number of the customer.                            |
| `customer_telnocc`  | _String_ | Optional      |                         Country phone code of the customer.                         |
| `shipping_street1`  | _String_ | Optional      |        Building name, and/or street name of the customer's shipping address.        |
| `shipping_city`     | _String_ | Optional      |                       City of the customer's shipping address.                      |
| `shipping_state`    | _String_ | Optional      |                 State or region of the customer's shipping address.                 |
| `shipping_postcode` | _String_ | Optional      | <p>Postal code/ Zip code of the customer's shipping address.<br>Max – 9 digits.</p> |
| `shipping_country`  | _String_ | Optional      |               <p>Country of the shipping address.<br>3-digits code</p>              |

<details>

<summary>Authentication request example</summary>

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/session' \
--header 'Content-Type: application/json' \
--data-raw '{
  "merchant_key": "xxxxx-xxxxx-xxxxx",
  "operation": "purchase",
  "methods": [
    "card"
  ],
  "parameters": {
    "card": {
      "customer_phone": "1234567890",
      "customer_telnocc ": "123",
      "shipping_street1": " Moor Building 35274",
      "shipping_ city": "Los Angeles",
      "shipping_state": "CA",
      "shipping_postcode": "809654321",
      "shipping_ country": "US"
    }
  },
  "order": {
    "number": "order-1234",
    "amount": "1000.19",
    "currency": "USD",
    "description": "Important gift"
  },
  "cancel_url": "https://example.com/cancel",
  "success_url": "https://example.com/success",
  "customer": {
    "name": "John Doe",
    "email": "test@gmail.com"
  },
  "billing_address": {
    "country": "US",
    "state": "CA",
    "city": "Los Angeles",
    "address": "Moor Building 35274",
    "zip": "123456",
    "phone": "347771112233"
  },
  "hash": "{{session_hash}}"
}
'
```

</details>

### Additional parameters - Set 5 (ERS)

This set should be used for debit operation.

| Parameter                 | Type     | Mandatory |                           Description                          |
| ------------------------- | -------- | --------- | :------------------------------------------------------------: |
| `payer_identity_type`     | _String_ | Optional  |           The type of Senders identification document          |
| `payer_identity_id`       | _String_ | Optional  |          The number of Senders identification document         |
| `payer_identity_country`  | _String_ | Optional  | The country of issuance of the Senders identification document |
| `payer_identity_exp_date` | _String_ | Optional  |   The expiration date of the Senders identification document   |
| `payer_nationality`       | _String_ | Optional  |                       Senders nationality                      |
| `payer_country_of_birth`  | _String_ | Optional  |                    Senders country of birth                    |
| `payee_first_name`        | _String_ | Optional  |                      Receivers first name                      |
| `payer_last_name`         | _String_ | Optional  |                       Receivers last name                      |
| `payee_middle_name`       | _String_ | Optional  |                      Receivers middle name                     |
| `payee_address`           | _String_ | Optional  |                        Receivers street                        |
| `payee_city`              | _String_ | Optional  |                         Receivers city                         |
| `payee_state`             | _String_ | Optional  |                         Receivers state                        |
| `payee_country`           | _String_ | Optional  |                        Receivers country                       |
| `payee_zip`               | _String_ | Optional  |                      Receivers postal code                     |
| `payee_phone`             | _String_ | Optional  |                     Receivers phone number                     |
| `payee_birth_date`        | _String_ | Optional  |                     Receivers date of birth                    |
| `payee_identity_type`     | _String_ | Optional  |          The type of Receivers identification document         |
| `payee_identity_id`       | _String_ | Optional  |         The number of Receivers identification document        |
| `payee_identity_country`  | _String_ | Optional  |  The country of issuance of Receivers identification document  |
| `payee_identity_exp_date` | _String_ | Optional  |    The expiration date of Receivers identification document    |
| `payee_nationality`       | _String_ | Optional  |                      Receivers nationality                     |
| `payee_country_of_birth`  | _String_ | Optional  |                   Receivers country of birth                   |

<details>

<summary>Authentication request example</summary>

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/session' \
--header 'Content-Type: application/json' \
--data-raw '{
  "merchant_key": "xxxxx-xxxxx-xxxxx",
  "operation": "purchase",
  "methods": [
    "card"
  ],
  "parameters": {
    "card": {
      "payer_identity_type": "Passport",
      "payer_identity_id": "ABC123456",
      "payer_identity_country": "KZ",
      "payer_identity_exp_date": "2026-05-15",
      "payer_nationality": "Kazakhstani",
      "payer_country_of_birth": "Kazakhstan",
      "payee_first_name": "John",
      "payer_last_name": "Doe",
      "payee_middle_name": "Smith",
      "payee_address": "123 Main Street",
      "payee_city": "Almaty",
      "payee_state": "Almaty Province",
      "payee_country": "Kazakhstan",
      "payee_zip": "050000",
      "payee_phone": "77123456789",
      "payee_birth_date": "1990-01-01",
      "payee_identity_type": "National ID",
      "payee_identity_id": "XYZ987654",
      "payee_identity_country": "Kazakhstan",
      "payee_identity_exp_date": "2030-12-31",
      "payee_nationality": "Kazakhstani",
      "payee_country_of_birth": "Kazakhstan"
    }
  },
  "order": {
    "number": "order-1234",
    "amount": "1000.19",
    "currency": "USD",
    "description": "Important gift"
  },
  "cancel_url": "https://example.com/cancel",
  "success_url": "https://example.com/success",
  "customer": {
    "name": "John Doe",
    "email": "test@gmail.com"
  },
  "billing_address": {
    "country": "US",
    "state": "CA",
    "city": "Los Angeles",
    "address": "Moor Building 35274",
    "zip": "123456",
    "phone": "347771112233"
  },
  "hash": "{{session_hash}}"
}
'
```

</details>

## afsbenefit

payment\_method = `afsbenefit`

## airtel

payment\_method = `airtel`

## paydefi

If the payer chooses the `allpay` payment method, the customer’s confirmation via app may be necessary to finish the payment.

## Apple Pay

To use the `applepay` payment method, configure the Wallets setting in the admin dashboard. Ensure that the payment provider has confirmed the Apple Pay certificates and verified your domain in the Apple Pay account.

To enable payers to make Apple Pay payments, you can either connect to the Checkout page or work directly via the S2S protocol:

**Checkout Page**: No additional development is required on your side.

**S2S Protocol**: You must be able to obtain the Apple Pay payment token and include it in the SALE request.

Regardless of the protocol you have to make settings in the admin panel and set the following configurations:

* Merchant Identifier, Country and Shop Name – according to the merchant’s data from [Apple Pay Developer account](https://idmsa.apple.com/IDMSWebAuth/signin?appIdKey=891bd3417a7776362562d2197f89480a8547b108fd934911bcbea0110d07f757\&path=%2Faccount%2Fresources%2Fidentifiers%2Flist%2Fmerchant\&rv=1)\

* Certificate and Private Key – generated Apple Pay certificate and private key\

* Merchant Capabilities and Supported Networks – configurations for Apple Pay payments\

* Processing Private Key – private key that uses for payment token decryption (requires if payment provider supports).\


We also recommend checking out the demo provided by Apple for Apple Pay: https://applepaydemo.apple.com/

***

**Set up Apple Pay**

If you are using the Checkout protocol for Apple Pay payments, you will need to configure your Apple Developer account. Follow these steps to set up Apple Pay in your Apple Developer account:

**1.** **Create a Merchant ID** in the "Certificates, Identifiers & Profiles" section.

**2.** **Register and verify the domains** involved in the interaction with Apple Pay (e.g., the checkout page URL where the Apple Pay button is placed) in the "Merchant Domains" section. Download the verification file and provide it to support to complete the domain verification.

**3.** **Create a Merchant Identity Certificate** in the "Merchant Identity Certificate" section:

* Generate a pair of certificates (\*.csr and \*.key) and upload the \*.csr file in the "Merchant Identity Certificate" section.
* Create a Merchant Identity Certificate based on the generated CSR file.
* Download the \*.pem file from the portal.

⚠️ **Note**\


> The \*.csr file can be obtained from the payment provider that processes Apple Pay, and the \*.pem file should be uploaded to your payment provider's dashboard, following their instructions.

**4.** **Create a Processing Private Key** in the "Apple Pay Payment Processing Certificate" section:

* Generate a pair of certificates (\*.csr and \*.key) and upload the \*.csr file.
* Create a Payment Processing Certificate based on the generated CSR file.

For more detailed instructions on setting up Apple Pay, refer to the following resource: [Learn more about setting up Apple Pay](https://developer.apple.com/documentation/passkit_apple_pay_and_wallet/apple_pay/setting_up_apple_pay)\


Next, enter the data from your Apple Pay developer account into the platform's admin panel:\
Go to the "Merchants" section, initiate editing, open the "Wallets" tab, navigate to the Apple Pay settings, and fill in the following fields:

* **Merchant Identifier:** Enter the Merchant ID.
* **Certificate:** Upload the \*.pem file downloaded from the "Merchant Identity Certificate" section.
* **Private Key:** Upload the \*.key file from the certificate pair generated for the Merchant Identity Certificate.
* **Processing Private Key (required for token decryption)**: Paste the text from the Processing Private Key file. Ensure it is a single line of text (without spaces or breaks) placed between "BEGIN" and "END."

***

**Apple Pay Payment Flow**

By default, all Apple Pay payments on the platform are classified as virtual. As a result:\


* Card details are not stored for these transactions.
* Functionality is limited for DMS payments and the creation of recurring transactions.\


You can maximize the benefits of Apple Pay payments by leveraging card flow for digital wallets:

* Access to post-transaction operations, such as capture in DMS mode
* Support for recurring payments (MIT) – scheduled or on-demand
* Smart routing for optimized payment processing
* Payment cascading for improved success rates

> Note! Ensure your provider supports these operations.

To access the card flow for Apple Pay payments, you need to:

* Set up the Processing Private Key in the admin panel settings ("Merchants" section → "Wallets" tab) to enable token decryption.
* Verify that your payment provider supports card flow processing (ask your support if available).\


How It Works:

* If both requirements are met, the system will always decrypt the Apple Pay token during payment and store the decrypted card data for future transactions.
* You can view decrypted card details in the Transaction Details section of the admin panel.
* If any requirement is not met, the payment will be processed using the virtual flow instead.
* If your provider supports card flow but does not accept decrypted data, platform can still store the card details for processing. However, some features (e.g., smart routing and cascading) will not be available.

## araka

payment\_method = `araka`

## astropay

If the payer chooses the `astropay` payment method, the redirection to another page will happen to finish the payment.

## awcc

You can add to the Authentication request a specific list of parameters which are possible for the `awcc` payment method.

| **Parameter**  | **Type**  | **Mandatory** |                                                                                                      **Description**                                                                                                      |
| -------------- | --------- | ------------- | :-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| `network_type` | _String_  | Optional      | <p>Network Type of cryptoType, if applicable.<br>Currently, only the symbol USDT have different network types.<br>The following network types are available for USDT: <code>eth</code> (default) or <code>tron</code></p> |
| `bech32`       | _Boolean_ | Optional      |                                              <p>Defaults to false.<br>If set to true, it will return bech32 segwit address for BTC address, or BCH cash address for BCH.</p>                                              |

<details>

<summary>Authentication request example</summary>

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/session' \
--header 'Content-Type: application/json' \
--data-raw '{
    "merchant_key": "xxxxx-xxxxx-xxxxx",
    "operation": "purchase",
    "methods": [
        "awcc"
    ],
        "parameters": {
                "awcc": {
            "network_type": "eth",
            "bech32": "false"
        }
    },   
    "order": {
        "number": "order-1234",
        "amount": "1000.19",
        "currency": "BRL",
        "description": "Important gift"
    },
    "cancel_url": "https://example.com/cancel",
    "success_url": "https://example.com/success",
     "customer": {
        "name": "John Doe",
        "email": "test@gmail.com"
    },
    "billing_address": {
        "country": "US",
        "state": "CA",
        "city": "Los Angeles",
        "address": "Moor Building 35274",
        "zip": "123456",
        "phone": "347771112233"
    },
    "hash": "{{session_hash}}",
}
'
```

</details>

## axxi-cash

If the payer chooses the `axxi-cash` payment method, the _ID document_ field will be additionally displayed on the Checkout page.\
After the _Pay_ button is clicked, the payer will be redirected to the page with the PDF payment ticket.

## axxi-transfer

If the payer chooses the `axxi-transfer` payment method, the additional fields will be displayed on the Checkout page. The set of the fields depends on the payer's billing country.

There is a list of possible additional fields on the Checkout page:

* _Choose your bank_ ;
* _Person Type_ ;
* _Document Type_ ;
* _ID document_ .

A list of the fields depends on payer's country.

After the fields are filled, the payer will be redirected to the bank's site to complete the transfer.

However, for some billing countries (for example, Mexico), redirection does not happen. Instead, the payment details will be displayed on the Checkout page. The payer needs to use them in order to complete the transfer directly at the bank.

## axxi-pin

If the payer chooses the `axxi-pin` payment method, the PIN voucher field will be displayed on the Checkout page. The payer should fill in the field to redeem the voucher and see the message with the result of the operation.

## a2a\_transfer

payment\_method = `a2a_transfer`

## beeline

If the payer chooses the `beeline` payment method, it needs to be specified the phone numbers of payer and (optionally) of receiver. The OTP code will be sent to the payer to confirm the payment.

## billplz

If the payer chooses the `billplz` payment method, the customer’s confirmation via app may be necessary to finish the payment.\
You can add to the Authentication request a specific list of parameters which are possible for the `billplz` payment method.

| Parameter             | Type     | Mandatory |              Description              |
| --------------------- | -------- | --------- | :-----------------------------------: |
| `bank_code`           | _String_ | Required  | SWIFT Bank Code that represents bank. |
| `bank_account_number` | _String_ | Required  |          Bank account number.         |

<details>

<summary>Authentication request example</summary>

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/session' \
--header 'Content-Type: application/json' \
--data-raw '{
  "merchant_key": "xxxxx-xxxxx-xxxxx",
  "operation": "purchase",
  "methods": [
    "billplz"
  ],
  "parameters": {
    "billplz": {
      "bank_code": "DUMMYBANKVERIFIED",
      "bank_account_number ": "232673199763"
    }
  },
  "order": {
    "number": "order-1234",
    "amount": "1000.19",
    "currency": "MYR",
    "description": "Important gift"
  },
  "cancel_url": "https://example.com/cancel",
  "success_url": "https://example.com/success",
  "customer": {
    "name": "John Doe",
    "email": "test@gmail.com"
  },
  "billing_address": {
    "country": "US",
    "state": "CA",
    "city": "Los Angeles",
    "address": "Moor Building 35274",
    "zip": "123456",
    "phone": "347771112233"
  },
  "hash": "{{session_hash}}"
}
'
```

</details>

## bitolo-inr

payment\_method = `bitolo-inr`

## bitolo-brl

payment\_method = `bitolo-brl`

## bpwallet

If the payer chooses the `bpwallet` payment method, the redirection to another page will happen to finish the payment.

## cardpaymentz

If the payer chooses the `cardpaymentz` payment method, the redirection to another page will happen to finish the payment.

## citizen

payment\_method = `citizen`

## cnfmo

If the payer chooses the `cnfmo` payment method, the redirection to another page will happen to finish the payment.

## coinspaid

payment\_method = `coinspaid`

## cup-on

payment\_method = `cup-on`

## crypto-btg

Use the `crypto-btg` payment method to perform cryptocurrency payments without specifying a fixed amount. If the payer selects the `crypto-btg` payment method, a crypto wallet address will be generated and displayed at checkout.

## cybersource

payment\_method = `cybersource`

## dcp

If the payer chooses the `dcp` payment method, the redirection to another page will happen to finish the payment.

## dl

If the payer chooses the `dl` payment method, the redirection to another page will happen to finish the payment.\
The following parameter is required:

| **Parameter**     | **Type** | **Mandatory** |                                                                    **Description**                                                                    |
| ----------------- | -------- | ------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------: |
| `document_number` | _String_ | Conditional   | <p>Customer's ID.<br><strong>Condition:</strong> If the parameter is NOT specified in the request, then it will be collected on the Checkout page</p> |

<details>

<summary>Authentication request example</summary>

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/session' \
--header 'Content-Type: application/json' \
--data-raw '{
  "merchant_key": "xxxxx-xxxxx-xxxxx",
  "operation": "purchase",
  "methods": [
    "dl"
  ],
  "parameters": {
      "dl": {
        "document_number": "document_number"
      }
   },
  "order": {
    "number": "order-1234",
    "amount": "100",
    "currency": "USD",
    "description": "Important gift"
  },
  "cancel_url": "https://example.domain.com/cancel",
  "success_url": "https://example.domain.com/success",
  "customer": {
    "name": "John Doe",
    "email": "test@gmail.com"
  },
  "billing_address": {
    "country": "BR",
    "city": "city",
    "address": "address",
    "house_number": "17/2",
    "zip": "123456",
    "phone": "347771112233"
  },
  "hash": "{{session_hash}}"
}'
```

</details>

## dlocal

If the payer chooses the `dlocal` payment method, the redirection to another page will happen to finish the payment.\
The following parameter is required:

| **Parameter**     | **Type** | **Mandatory** |                                                                    **Description**                                                                    |
| ----------------- | -------- | ------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------: |
| `document_number` | _String_ | Conditional   | <p>Customer's ID.<br><strong>Condition:</strong> If the parameter is NOT specified in the request, then it will be collected on the Checkout page</p> |

<details>

<summary>Authentication request example</summary>

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/session' \
--header 'Content-Type: application/json' \
--data-raw '{
  "merchant_key": "xxxxx-xxxxx-xxxxx",
  "operation": "purchase",
  "methods": [
    "dlocal"
  ],
  "parameters": {
      "dlocal": {
        "document_number": "document_number"
      }
   },
  "order": {
    "number": "order-1234",
    "amount": "100",
    "currency": "USD",
    "description": "Important gift"
  },
  "cancel_url": "https://example.domain.com/cancel",
  "success_url": "https://example.domain.com/success",
  "customer": {
    "name": "John Doe",
    "email": "test@gmail.com"
  },
  "billing_address": {
    "country": "BR",
    "city": "city",
    "address": "address",
    "house_number": "17/2",
    "zip": "123456",
    "phone": "347771112233"
  },
  "hash": "{{session_hash}}"
}'
```

</details>

## doku-hpp

If the payer chooses the `doku-hpp` payment method, the redirection to another page will happen to finish the payment.\
You can add to the Authentication request a specific list of parameters which may be required for the `doku-hpp` payment method.

| Parameter              | Type     | Mandatory   |                                                                                          Description                                                                                          |
| ---------------------- | -------- | ----------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| `product_reference_id` | _String_ | Conditional | SKU/item ID of the item in this transaction. This parameter is mandatory if you want to use Akulaku, Kredivo and Indodana. Allowed chars: alphabetic, numeric, special chars. Max Length: 64. |
| `product_name`         | _String_ | Conditional |           Name of the product item. This parameter is mandatory if you want to use Kredivo, Jenius and Indodana. Allowed chars: alphabetic, numeric, special chars. Max Length: 255           |
| `product_quantity`     | _Number_ | Conditional |                     Quantity of the product item. This paramater mandatory if you want to use Kredivo, Akulaku, Indodana and Jenius. Allowed chars: numeric. Max Length: 4                    |
| `product_sku`          | _String_ | Conditional |                                              SKU of the product item. This paramater mandatory if you want to use Kredivo, Akulaku and Indodana.                                              |
| `product_category`     | _String_ | Conditional |                                            Category of the product item. This paramater mandatory if you want to use Kredivo, Akulaku and Indodana.                                           |
| `product_url`          | _String_ | Conditional |                                          URL to the product item on merchant site. This paramater mandatory if you want to use Kredivo and Indodana.                                          |
| `product_image_url`    | _String_ | Conditional |                                            URL (image) of the product item on merchant site. This paramater mandatory if you want to use Indodana.                                            |
| `product_type`         | _String_ | Conditional |                                                   Type of the item in this transaction. This paramater mandatory if you want to use Indodana                                                  |

<details>

<summary>Authentication request example</summary>

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/session' \
--header 'Content-Type: application/json' \
--data-raw '{
  "merchant_key": "xxxxx-xxxxx-xxxxx",
  "operation": "purchase",
  "methods": [
    "doku-hpp"
  ],
  "parameters": {
    "doku-hpp`": {
      "product_reference_id": "1",
      "product_name": "Fresh flowers",
      "product_quantity": "1",
      "product_sku": "FF01",
      "product_category": "gift-and-flowers",
      "product_url": "http://example.com/",
      "product_image_url": "http://example.com/",
      "product_type": "ABC"
    }
  },
  "order": {
    "number": "order-1234",
    "amount": "1000",
    "currency": "IDR",
    "description": "Important gift"
  },
  "cancel_url": "https://example.com/cancel",
  "success_url": "https://example.com/success",
  "customer": {
    "name": "John Doe",
    "email": "test@gmail.com"
  },
  "billing_address": {
    "country": "US",
    "state": "CA",
    "city": "Los Angeles",
    "address": "Moor Building 35274",
    "zip": "123456",
    "phone": "347771112233"
  },
  "hash": "{{session_hash}}"
}
'
```

</details>

## dpbanktransfer

If the payer chooses the `dpbanktransfer` payment method, the additional information must be provided by the payer on the Checkout page.

The payer should enter the details about the beneficiary and select an account from the list of accounts available for transfer.

The confirmation code can be requested at any stage of the transfer.

## e-xezine

payment\_method = `e-xezine`

## fairpay

If the payer chooses the `fairpay` payment method, the set of the fields on the Checkout page depends on the payer's billing country.\
Personal Identification Number is required if payer specifies one of the following countries: Brazil, Chile, and Colombia.

## fawry

If the payer chooses the `fawry` payment method, the customer’s confirmation via app may be necessary to finish the payment.

## fxmb-india

payment\_method = `fxmb-india`

## fxmb-netbanking

payment\_method = `fxmb-netbanking`

## gigadat

If the payer chooses the `gigadat` payment method, the redirection to another page will happen to finish the payment.

## Google Pay

To provide the payers with the possibility of Google Pay™ payment you can connect to the Checkout or work directly via S2S protocol.

If you are using **Checkout integration** to work with `googlepay` payment method, it requires no additional code implementation.\


If you are using **S2S integration** to work with `googlepay` payment method, you must be able to obtain the Google Pay payment token and include it in the SALE request.

To work with Google Pay™ payments through gateway, you need to make settings in the admin panel. You can set the following configurations:\
\
• choose the environment: `TEST` or `PRODUCTION`\
\
• specify "Allowed Auth Method" - `PAN_ONLY` or `CRYPTOGRAM_3DS`\
\
• determine "Supported Networks" - MASTERCARD, VISA, AMEX, DISCOVER, JCB\
\
These configuration will be applied and used when platform interacts with Google Pay.\


⚠️ **Pay attention**\


> _in the case of PAN\_ONLY, the responsibility for passing 3ds is transferred to the acquirer, through which the payment is processed_

***

**Set up Google Pay**

To set up Google Pay, you first need to determine who acts as the gateway when processing payments: the platform or the payment provider. This depends on which side decrypts the Google Pay payment token.

* If the platform decrypts the token and then sends the decrypted payment data to the payment provider, the platform acts as the gateway.
* If the payment provider decrypts the token (i.e., expects to receive an encrypted payment token in the payment request), the payment provider is the gateway.

The gateway (whether it is the platform or the payment provider processing Google Pay) must provide the following information:

* **Gateway**: The name of the gateway.
* **Gateway Merchant ID**: The merchant identifier in the gateway's system.

⚠️ **Note**\


> If the payment provider is the gateway, it is important to clarify whether they expect the token to include additional information, such as the payer's address or phone number.

The next step is to enter the data into the admin panel.

Go to the "Merchants" section, initiate editing, open the "Wallets" tab, navigate to the Google Pay settings, and fill in the following fields:

* **Merchant Identifier**: Enter the identifier from the Google Business Console. If the platform is the gateway, you can duplicate the Gateway Merchant ID here.
* **Gateway**: The gateway for interaction with Google Pay.
* **Gateway Merchant ID**: The merchant identifier in the Google gateway.
* **Private Key (required for decrypted token)**: The key for decrypting the payment token. If the platform is the gateway, the key should be provided by the platform. If the payment provider is the gateway, the key is not required.
* **Include Additional Parameters to Google Token**: Depending on the payment provider's requirements.

Additionally, to work with Google Pay payments, you must verify the checkout domains from which Google Pay is processed in the Google Business Console. For domain verification, you may need to contact support.

***

**Google Pay Payment Flow**

By default, all Google Pay payments on the platform are classified as virtual. As a result:

* Card details are not stored for these transactions.
* Functionality is limited for DMS payments and the creation of recurring transactions.\


You can maximize the benefits of Google Pay payments by leveraging card flow for digital wallets:\


* Access to post-transaction operations, such as capture in DMS mode
* Support for recurring payments (MIT) – scheduled or on-demand
* Smart routing for optimized payment processing
* Payment cascading for improved success rates

> **Note!** Ensure your provider supports these operations.\
>

To access the card flow for Google Pay payments, you need to:

* Set up the Private Key in the admin panel settings ("Merchants" section → "Wallets" tab) to enable token decryption.
* Verify that your payment provider supports card flow processing (ask your support if available).\


How It Works:

* If both requirements are met, the system will always decrypt the Google Pay token during payment and store the decrypted card data for future transactions.
* You can view decrypted card details in the Transaction Details section of the admin panel.
* If any requirement is not met, the payment will be processed using the virtual flow instead.
* If your provider supports card flow but does not accept decrypted data, platform can still store the card details for processing. However, some features (e.g., smart routing and cascading) will not be available.

## hayvn

If the payer chooses the `hayvn` payment method, it needs to be specified the crypto currency for payment. After creating a payment, the payer will be shown the data necessary to complete the transaction.

## helio

If the payer chooses the `helio` payment method, the redirection to widget page will happen to finish the payment.

## help2pay

payment\_method = `help2pay`

## ideal\_crdz

If the payer chooses the ideal\_crdz payment method, the redirection to another page will happen to finish the payment.

## instant-bills-pay

If the payer chooses the `instant-bills-pay` payment method, the redirection to another page will happen to finish the payment.

## ipasspay

payment\_method = `ipasspay`

## jvz

If the payer chooses the `jvz` payment method, the confirmation necessary to finish the payment via APP.

## kashahpp

If the payer chooses the `kashahpp` payment method, the redirection to another page will happen to finish the payment.

## m2p-debit

You should add to the Authentication request a specific list of parameters in the “parameters” object that are needed for the\
`m2p-debit` payment method.

| **Parameter**         | **Type** | **Mandatory** |                      **Description**                      |
| --------------------- | -------- | ------------- | :-------------------------------------------------------: |
| `paymentGatewayName`  | _String_ | Required      |   Name of payment gateway that will be used for deposit   |
| `paymentCurrency`     | _String_ | Required      | CRYPTO currency Symbol of currency that will be deposited |
| `tradingAccountLogin` | _String_ | Optional      |               Depositor’s trading account id              |

<details>

<summary>Authentication request example</summary>

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/session' \
--header 'Content-Type: application/json' \
--data-raw '{
    "merchant_key": "xxxxx-xxxxx-xxxxx",
    "operation": "purchase",
    "methods": [
        "m2p-debit"
    ],
        "parameters": {
                "m2p-debit": {
            "paymentGatewayName": "gateway",
            "paymentCurrency": "BTC",
            "tradingAccountLogin": "login"
        }
    },   
    "order": {
        "number": "order-1234",
        "amount": "1000.19",
        "currency": "BRL",
        "description": "Important gift"
    },
    "cancel_url": "https://example.com/cancel",
    "success_url": "https://example.com/success",
     "customer": {
        "name": "John Doe",
        "email": "test@gmail.com"
    },
    "billing_address": {
        "country": "US",
        "state": "CA",
        "city": "Los Angeles",
        "address": "Moor Building 35274",
        "zip": "123456",
        "phone": "347771112233"
    },
    "hash": "{{session_hash}}",
}
'
```

</details>

## m2p-withdrawal

You should add to the Authentication request a specific list of parameters in the “parameters” object that are needed for the `m2p-withdrawal` payment method.

| **Parameter**         | **Type** | **Mandatory** |                        **Description**                        |
| --------------------- | -------- | ------------- | :-----------------------------------------------------------: |
| `withdrawCurrency`    | _String_ | Required      | CRYPTO currency Cryptocurrency to which currency is converted |
| `address`             | _String_ | Required      |         User cryptocurrency wallet address to withdraw        |
| `tradingAccountLogin` | _String_ | Optional      |      Trading account ID of a user requesting a withdrawal     |

<details>

<summary>Authentication request example</summary>

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/session' \
--header 'Content-Type: application/json' \
--data-raw '{
    "merchant_key": "xxxxx-xxxxx-xxxxx",
    "operation": "purchase",
    "methods": [
        "m2p-withdrawal"
    ],
        "parameters": {
                "m2p-withdrawal": {
            "withdrawCurrency": "BTC",
            "address": "mkzePZfMeoYsoGcavSJFT7UKSjsU2BEgzt",
            "tradingAccountLogin": "login"
        }
    },   
    "order": {
        "number": "order-1234",
        "amount": "1000.19",
        "currency": "BRL",
        "description": "Important gift"
    },
    "cancel_url": "https://example.com/cancel",
    "success_url": "https://example.com/success",
     "customer": {
        "name": "John Doe",
        "email": "test@gmail.com"
    },
    "billing_address": {
        "country": "US",
        "state": "CA",
        "city": "Los Angeles",
        "address": "Moor Building 35274",
        "zip": "123456",
        "phone": "347771112233"
    },
    "hash": "{{session_hash}}",
}
'
```

</details>

## mcpayhpp

If the payer chooses the `mcpayhpp` payment method, the redirection to another page will happen to finish the payment.

## mcpayment

If the payer chooses the `mcpayment` payment method, the QR-code will be swown additionally. The customer needs to scan the QR-code to finish the payment.

## moov-money

This method is used for mobile numbers in Benin. If the payer chooses `moov-money` payment method, confirmation is required through a mobile device.

## moov-togo

This method is used for mobile numbers in Togo. If the payer chooses `moov-togo` payment method, confirmation is required through a mobile device.

## mpesa

If the payer chooses the `mpesa` payment method, the QR-code will be swown additionally. The customer needs to scan the QR-code to finish the payment.

## mtn-mobile-money

This method is used for mobile numbers in Benin. If the payer chooses `mtn-mobile-money` payment method, confirmation is required through a mobile device.

## naps

payment\_method = `naps`

## netbanking-upi

payment\_method = `netbanking-upi`

## next-level-finance

If the payer chooses the `next-level-finance` payment method, the redirection to another page will happen to finish the payment.

## nimbbl

If the payer chooses the `nimbbl` payment method, the confirmation necessary to finish the payment via APP.&#x20;

## nuvvex

If the payer chooses the `nuvvex` payment method, the redirection to another page will happen to finish the payment.

## nv-apm

If the payer chooses the `nv-apm` payment method, the redirection to another page will happen to finish the payment.

## om-wallet

If the payer chooses the `om-wallet` payment method, the field for entering the phone number in international format will be additionally displayed on the Checkout page.

## one-collection

If the payer chooses the `one-collection` payment method, the additional fields will be required on the Checkout page.\
The payer needs to specify Bank name, Bank Code, Branch Name, Branch Code, and Account Number.\
After clicking the Pay button, the information required to complete the payment will be displayed.

## pagsmile

If the payer chooses the `pagsmile` payment method, the redirection to another page will happen to finish the payment. The following parameters are required, except for transactions in Guatemala, Panama, or Costa Rica:

| **Parameter**     | **Type** | **Mandatory** |                                                                             **Description**                                                                            |
| ----------------- | -------- | ------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| `document_number` | _String_ | Conditional   |          <p>Customer's ID.<br><strong>Condition:</strong> If the parameter is NOT specified in the request, then it will be collected on the Checkout page</p>         |
| `document_type`   | _String_ | Conditional   | <p>Customer's identification type.<br><strong>Condition:</strong> If the parameter is NOT specified in the request, then it will be collected on the Checkout page</p> |

<details>

<summary>Authentication request example</summary>

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/session' \
--header 'Content-Type: application/json' \
--data-raw '{
  "merchant_key": "xxxxx-xxxxx-xxxxx",
  "operation": "purchase",
  "methods": [
    "pagsmile"
  ],
  "parameters": {
      "pagsmile": {
        "document_number": "document_number",
        "document_type": "document_type"
      }
   },
  "order": {
    "number": "order-1234",
    "amount": "100",
    "currency": "BRL",
    "description": "Important gift"
  },
  "cancel_url": "https://example.domain.com/cancel",
  "success_url": "https://example.domain.com/success",
  "customer": {
    "name": "John Doe",
    "email": "test@gmail.com"
  },
  "billing_address": {
    "country": "BR",
    "city": "city",
    "address": "address",
    "house_number": "17/2",
    "zip": "123456",
    "phone": "347771112233"
  },
  "hash": "{{session_hash}}"
}'
```

</details>

## panapay-netbanking

If the payer chooses the `panapay-netbanking` payment method, the payer will be shown the data necessary to complete the payment.

## panapay-upi

If the payer chooses the `panapay-upi` payment method, the payer will be shown the data necessary to complete the payment.

## papara

If the payer chooses the `papara` payment method, the redirection to another page will happen to finish the payment. You have to specify in your request the next parameter:

| **Parameter** | **Type** | **Mandatory** |                                                                   **Description**                                                                   |
| ------------- | -------- | ------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------: |
| `memberId`    | _String_ | Conditional   | <p>Customer ID.<br><strong>Condition:</strong> If the parameter is NOT specified in the request, then it will be collected on the Checkout page</p> |

<details>

<summary>Authentication request example</summary>

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/session' \
--header 'Content-Type: application/json' \
--data-raw '{
  "merchant_key": "xxxxx-xxxxx-xxxxx",
  "operation": "purchase",
  "methods": [
    "papara"
  ],
  "parameters": {
    "papara": {
      "memberId": "member Id"
   },
  "session_expiry": 60,
  "order": {
    "number": "order-1234",
    "amount": "100",
    "currency": "TRY",
    "description": "Important gift"
  },
  "cancel_url": "https://example.domain.com/cancel",
  "success_url": "https://example.domain.com/success",
  "expiry_url": "https://example.domain.com/expiry",
  "url_target": "_blank",
  "customer": {
    "name": "John Doe",
    "email": "test@gmail.com"
  },
  "billing_address": {
    "country": "TR",
    "city": "Ankara",
    "address": "Moor Building 35274",
    "house_number": "17/2",
    "zip": "123456",
    "phone": "347771112233"
  },
  "hash": "{{session_hash}}"
}
```

</details>

## payablhpp

If the payer chooses the `payablhpp` payment method, the redirection to another page will happen to finish the payment.

## paydefi

If the payer chooses the `allpay` payment method, the customer’s confirmation via app may be necessary to finish the payment.

## payftr-in

If the payer chooses the `payftr-in` payment method, the redirection to another page will happen to finish the payment.

## payneteasyhpp

If the payer chooses the `payneteasyhpp` payment method, the redirection to to widget page happen to finish the payment.

## payok-promptpay

If the payer chooses the `payok-promptpay` payment method, the QR-code will be displayed to the payer. The payer should use the code to finish the payment.

## payok-upi

If the payer chooses the `payok-upi` payment method, the customer’s confirmation necessary to finish the payment.\
You can add to the Authentication request a specific list of parameters which are possible for the `payok-upi` payment method.

| **Parameter** | **Type** | **Mandatory** |                                                                              **Description**                                                                             |
| ------------- | -------- | ------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| `personal_id` | _String_ | Conditional   | <p>Payer's Identity ID.<br><strong>Condition:</strong> If the parameter is NOT specified in the request for India,<br>then it will be collected on the Checkout page</p> |

<details>

<summary>Authentication request example</summary>

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/session' \
--header 'Content-Type: application/json' \
--data-raw '{
    "merchant_key": "xxxxx-xxxxx-xxxxx",
    "operation": "purchase",
    "methods": [
        "payok-upi"
    ],
        "parameters": {
            "payok-upi": {
            "personal_id": "personal id"
        }
    },   
    "order": {
        "number": "order-1234",
        "amount": "1000.19",
        "currency": "MYR",
        "description": "Important gift"
    },
    "cancel_url": "https://example.com/cancel",
    "success_url": "https://example.com/success",
     "customer": {
        "name": "John Doe",
        "email": "test@gmail.com"
    },
    "billing_address": {
        "country": "US",
        "state": "CA",
        "city": "Los Angeles",
        "address": "Moor Building 35274",
        "zip": "123456",
        "phone": "347771112233"
    },
    "hash": "{{session_hash}}"
}
'
```

</details>

## paypal

payment\_method = `paypal`

## paythrough-upi

You should add to the Authentication request a specific list of parameters in the “parameters” object that are needed for the `paythrough-upi` payment method.

| **Parameter** | **Type** | **Mandatory** | **Description** |
| ------------- | -------- | ------------- | :-------------: |
| `upi_id`      | _String_ | Required      |      Upi Id     |

<details>

<summary>Authentication request example</summary>

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/session' \
--header 'Content-Type: application/json' \
--data-raw '{
    "merchant_key": "xxxxx-xxxxx-xxxxx",
    "operation": "purchase",
    "methods": [
        "paythrough-upi"
    ],
        "parameters": {
                "paythrough-upi": {
            "upi_id": "joyoberoihrl@okaxis"
        }
    },   
    "order": {
        "number": "order-1234",
        "amount": "1000.19",
        "currency": "INR",
        "description": "Important gift"
    },
    "cancel_url": "https://example.com/cancel",
    "success_url": "https://example.com/success",
     "customer": {
        "name": "John Doe",
        "email": "test@gmail.com"
    },
    "billing_address": {
        "country": "IN",
        "state": "Maharashtra",
        "city": "Mumbai",
        "address": " Shop No.12, Rendezvous, Raviraj Oberoi Cmplx",
        "zip": "400053",
        "phone": "9818218660"
    },
    "hash": "{{session_hash}}"
}
'
```

</details>

## paytota

payment\_method = `paytota`

## phone-pe

If the payer chooses the `phone-pe` payment method, the customer’s confirmation via app may be necessary to finish the payment.

## pix

If the payer chooses the `pix` payment method, the field for entering the document number will be additionally displayed on the Checkout page.\
After the _Pay_ button is clicked, the QR-code could be displayed to the payer. The payer should use the code to finish the payment.

## ppro-alipay

payment\_method = `ppro-alipay`

## ppro-bitpay

payment\_method = `ppro-bitpay`

## ppro-dragonpay

payment\_method = `ppro-dragonpay`

## ppro-fpx

payment\_method = `ppro-fpx`

## ppro-ideal

payment\_method = `ppro-ideal`

## ppro-klarna

payment\_method = `ppro-klarna`

## ppro-przelewy24

payment\_method = `ppro-przelewy24`

## ppro-safetypay

payment\_method = `ppro-safetypay`

## ppro-skrill

payment\_method = `ppro-skrill`

## ppro-sofort

payment\_method = `ppro-sofort`

## ppro-trustly

payment\_method = `ppro-trustly`

## ppro-unionpay

payment\_method = `ppro-unionpay`

## ppro-wechat5

payment\_method = `ppro-wechat5`

## ppro-wechatonline

payment\_method = `ppro-wechatonline`

## pr-cash

payment\_method = `pr-cash`

## pr-creditcard

payment\_method = `pr-creditcard`

## pr-cryptocurrency

payment\_method = `pr-cryptocurrency`

## pr-online

payment\_method = `pr-online`

## ptbs

If the payer chooses the `ptbs` payment method, the redirection to another page will happen to finish the payment.

## ptn-email

If the payer chooses the `ptn-email` payment method, the email will be sent to the Payer with the link to payment page.

You can send additional parameters for this payment method:

| **Parameter**            | **Type** | **Mandatory** |                                                 **Description**                                                |
| ------------------------ | -------- | ------------- | :------------------------------------------------------------------------------------------------------------: |
| `SecurityQuestion`       | _String_ | Optional      |                                             The security question.                                             |
| `SecurityQuestionAnswer` | _String_ | Optional      | <p>SecurityQuestionAnswer standards:<br>- No spaces.<br>- Length must be between 3 and 25 characters long.</p> |

## ptn-inapp

If the payer chooses the `ptn-inapp` payment method, the redirection to another page will happen to finish the payment.

## ptn-sms

If the payer chooses the `ptn-sms` payment method, the sms will be sent to the Payer with the link to payment page.

You can send additional parameters for this payment method:

| **Parameter**            | **Type** | **Mandatory** |                                                 **Description**                                                |
| ------------------------ | -------- | ------------- | :------------------------------------------------------------------------------------------------------------: |
| `SecurityQuestion`       | _String_ | Optional      |                                             The security question.                                             |
| `SecurityQuestionAnswer` | _String_ | Optional      | <p>SecurityQuestionAnswer standards:<br>- No spaces.<br>- Length must be between 3 and 25 characters long.</p> |

## pyk-bkmexpress

If you set the value `pyk-bkmexpress` for the brand parameter you have to specify in your request the next parameter required for Turkey:

| **Parameter**   | **Type** | **Mandatory** |                                                                                                                           **Description**                                                                                                                          |
| --------------- | -------- | ------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| `orgCode`       | _String_ | Conditional   |                                         <p>Payer's Institution (Bank) Code.<br><strong>Condition:</strong> If the parameter is NOT specified in the request for Turkey, then it will be collected on the Checkout page</p>                                         |
| `accountName`   | _String_ | Conditional   |  <p>Payer's name, length 1~64 characters; Example: Colin Ford. Must be exactly the same as the actual payer's name.<br><strong>Condition:</strong> If the parameter is NOT specified in the request for Turkey, then it will be collected on the Checkout page</p> |
| `accountNumber` | _String_ | Conditional   | <p>Payer's Institution (Bank) account number. Must be exactly the same as the account number of the actual payment.<br><strong>Condition:</strong> If the parameter is NOT specified in the request for Turkey, then it will be collected on the Checkout page</p> |

## pyk-momo

If you set the value `pyk-momo` for the brand parameter you have to specify in your request the next parameter required for Vietnam:

| **Parameter** | **Type** | **Mandatory** |                                                                             **Description**                                                                             |
| ------------- | -------- | ------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| `personal_id` | _String_ | Conditional   | <p>Payer's Identity ID.<br><strong>Condition:</strong> If the parameter is NOT specified in the request for Vietnam, then it will be collected on the Checkout page</p> |

## pyk-nequipush

If you set the value `pyk-nequipush` for the brand parameter you have to specify in your request the next parameter required for Colombia:

| **Parameter** | **Type** | **Mandatory** |                                                                              **Description**                                                                             |
| ------------- | -------- | ------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| `personal_id` | _String_ | Conditional   | <p>Payer's Identity ID.<br><strong>Condition:</strong> If the parameter is NOT specified in the request for Colombia, then it will be collected on the Checkout page</p> |

## pyk-paparawallet

If you set the value `pyk-paparawallet` for the brand parameter you have to specify in your request the next parameter required for Turkey:

| **Parameter**   | **Type** | **Mandatory** |                                                                                                                           **Description**                                                                                                                          |
| --------------- | -------- | ------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| `orgCode`       | _String_ | Conditional   |                                         <p>Payer's Institution (Bank) Code.<br><strong>Condition:</strong> If the parameter is NOT specified in the request for Turkey, then it will be collected on the Checkout page</p>                                         |
| `accountName`   | _String_ | Conditional   |  <p>Payer's name, length 1~64 characters; Example: Colin Ford. Must be exactly the same as the actual payer's name.<br><strong>Condition:</strong> If the parameter is NOT specified in the request for Turkey, then it will be collected on the Checkout page</p> |
| `accountNumber` | _String_ | Conditional   | <p>Payer's Institution (Bank) account number. Must be exactly the same as the account number of the actual payment.<br><strong>Condition:</strong> If the parameter is NOT specified in the request for Turkey, then it will be collected on the Checkout page</p> |

## pyk-promptpay

If the payer chooses the `pyk-promptpay` payment method, the QR-code will be displayed to the payer. The payer should use the code to finish the payment.

If you set the value `pyk-promptpay` for the brand parameter you have to specify in your request the next parameter required for Thailand:

| **Parameter**   | **Type** | **Mandatory** |                                                                                                                            **Description**                                                                                                                           |
| --------------- | -------- | ------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| `orgCode`       | _String_ | Conditional   |                                         <p>Payer's Institution (Bank) Code.<br><strong>Condition:</strong> If the parameter is NOT specified in the request for Thailand, then it will be collected on the Checkout page</p>                                         |
| `accountName`   | _String_ | Conditional   |  <p>Payer's name, length 1~64 characters; Example: Colin Ford. Must be exactly the same as the actual payer's name.<br><strong>Condition:</strong> If the parameter is NOT specified in the request for Thailand, then it will be collected on the Checkout page</p> |
| `accountNumber` | _String_ | Conditional   | <p>Payer's Institution (Bank) account number. Must be exactly the same as the account number of the actual payment.<br><strong>Condition:</strong> If the parameter is NOT specified in the request for Thailand, then it will be collected on the Checkout page</p> |

## pyk-truemoney

If you set the value `pyk-truemoney` for the brand parameter you have to specify in your request the next parameter required for Thailand:

| **Parameter**   | **Type** | **Mandatory** |                                                                                                                            **Description**                                                                                                                           |
| --------------- | -------- | ------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| `orgCode`       | _String_ | Conditional   |                                         <p>Payer's Institution (Bank) Code.<br><strong>Condition:</strong> If the parameter is NOT specified in the request for Thailand, then it will be collected on the Checkout page</p>                                         |
| `accountName`   | _String_ | Conditional   |  <p>Payer's name, length 1~64 characters; Example: Colin Ford. Must be exactly the same as the actual payer's name.<br><strong>Condition:</strong> If the parameter is NOT specified in the request for Thailand, then it will be collected on the Checkout page</p> |
| `accountNumber` | _String_ | Conditional   | <p>Payer's Institution (Bank) account number. Must be exactly the same as the account number of the actual payment.<br><strong>Condition:</strong> If the parameter is NOT specified in the request for Thailand, then it will be collected on the Checkout page</p> |

## pyk-upi

If the payer chooses the `pyk-upi` payment method, the customer’s confirmation necessary to finish the payment.\
You can add to the Authentication request a specific list of parameters which are possible for the `pyk-upi` payment method.

| **Parameter** | **Type** | **Mandatory** |                                                                              **Description**                                                                             |
| ------------- | -------- | ------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| `personal_id` | _String_ | Conditional   | <p>Payer's Identity ID.<br><strong>Condition:</strong> If the parameter is NOT specified in the request for India,<br>then it will be collected on the Checkout page</p> |

<details>

<summary>Authentication request example</summary>

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/session' \
--header 'Content-Type: application/json' \
--data-raw '
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/session' \
--header 'Content-Type: application/json' \
--data-raw '{
    "merchant_key": "xxxxx-xxxxx-xxxxx",
    "operation": "purchase",
    "methods": [
        "pyk-upi"
    ],
        "parameters": {
            "pyk-upi": {
            "personal_id": "personal id"
        }
    },   
    "order": {
        "number": "order-1234",
        "amount": "1000.19",
        "currency": "MYR",
        "description": "Important gift"
    },
    "cancel_url": "https://example.com/cancel",
    "success_url": "https://example.com/success",
     "customer": {
        "name": "John Doe",
        "email": "test@gmail.com"
    },
    "billing_address": {
        "country": "US",
        "state": "CA",
        "city": "Los Angeles",
        "address": "Moor Building 35274",
        "zip": "123456",
        "phone": "347771112233"
    },
    "hash": "{{session_hash}}"
}
'
```

</details>

## pyk-viettelpay

If you set the value `pyk-viettelpay` for the brand parameter you have to specify in your request the next parameter required for Vietnam:

| **Parameter** | **Type** | **Mandatory** |                                                                             **Description**                                                                             |
| ------------- | -------- | ------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| `personal_id` | _String_ | Conditional   | <p>Payer's Identity ID.<br><strong>Condition:</strong> If the parameter is NOT specified in the request for Vietnam, then it will be collected on the Checkout page</p> |

## pyk-zalopay

If you set the value `pyk-zalopay` for the brand parameter you have to specify in your request the next parameter required for Vietnam:

| **Parameter** | **Type** | **Mandatory** |                                                                             **Description**                                                                             |
| ------------- | -------- | ------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| `personal_id` | _String_ | Conditional   | <p>Payer's Identity ID.<br><strong>Condition:</strong> If the parameter is NOT specified in the request for Vietnam, then it will be collected on the Checkout page</p> |

## paypal

payment\_method = `paypal`

## paythrough-upi

You should add to the Authentication request a specific list of parameters in the “parameters” object that are needed for the `paythrough-upi` payment method.

| **Parameter** | **Type** | **Mandatory** | **Description** |
| ------------- | -------- | ------------- | :-------------: |
| `upi_id`      | _String_ | Required      |      Upi Id     |

<details>

<summary>Authentication request example</summary>

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/session' \
--header 'Content-Type: application/json' \
--data-raw '{
    "merchant_key": "xxxxx-xxxxx-xxxxx",
    "operation": "purchase",
    "methods": [
        "paythrough-upi"
    ],
        "parameters": {
                "paythrough-upi": {
            "upi_id": "joyoberoihrl@okaxis"
        }
    },   
    "order": {
        "number": "order-1234",
        "amount": "1000.19",
        "currency": "INR",
        "description": "Important gift"
    },
    "cancel_url": "https://example.com/cancel",
    "success_url": "https://example.com/success",
     "customer": {
        "name": "John Doe",
        "email": "test@gmail.com"
    },
    "billing_address": {
        "country": "IN",
        "state": "Maharashtra",
        "city": "Mumbai",
        "address": " Shop No.12, Rendezvous, Raviraj Oberoi Cmplx",
        "zip": "400053",
        "phone": "9818218660"
    },
    "hash": "{{session_hash}}"
}
'
```

</details>

## saopayments

payment\_method = `saopayments`

## securepaycard

payment\_method = `securepaycard`

## sepa

If the payer chooses the `sepa` payment method, the redirection to another page will happen to finish the payment.

## sepainstant

If the payer chooses the `sepainstant` payment method, the redirection to another page will happen to finish the payment.

## skrill

If the payer chooses the `skrill` payment method, the redirection to another page will happen to finish the payment.

## sofortuber

If the payer chooses the `sofortuber` payment method, the payer will be shown the data necessary to complete the payment.

## stcpay

payment\_method = `stcpay`

## stripe-js

If the payer chooses the `stripe-js` payment method, the redirection to another page will happen to finish the payment.

## swifipay-deposit

If the payer chooses the `swifipay-deposit` payment method, the redirection to another page will happen to finish the payment.

## sz-in-imps

If the payer chooses the `sz-in-imps` payment method, the redirection to another page will happen to finish the payment.

## sz-in-paytm

If the payer chooses the `sz-in-paytm` payment method, the redirection to another page will happen to finish the payment.

## sz-in-upi

You should add to the Authentication request a specific list of parameters in the “parameters” object that are needed for the `sz-in-upi` payment method.

| **Parameter** | **Type** | **Mandatory** |   **Description**   |
| ------------- | -------- | ------------- | :-----------------: |
| `upiAddress`  | _String_ | Required      | Payer's UPI address |

<details>

<summary>Authentication request example</summary>

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/session' \
--header 'Content-Type: application/json' \
--data-raw '{
    "merchant_key": "xxxxx-xxxxx-xxxxx",
    "operation": "purchase",
    "methods": [
        "sz-in-upi"
    ],
        "parameters": {
                "sz-in-upi": {
            "upiAddress: "address@upi" 
        }
    },   
    "order": {
        "number": "order-1234",
        "amount": "1000.19",
        "currency": "INR",
        "description": "Important gift"
    },
    "cancel_url": "https://example.com/cancel",
    "success_url": "https://example.com/success",
     "customer": {
        "name": "John Doe",
        "email": "test@gmail.com"
    },
    "billing_address": {
        "country": "US",
        "state": "CA",
        "city": "Los Angeles",
        "address": "Moor Building 35274",
        "zip": "123456",
        "phone": "347771112233"
    },
    "hash": "{{session_hash}}",
}
'
```

</details>

## sz-jp-p2p

If the payer chooses the `sz-jp-p2p` payment method, the redirection to another page will happen to finish the payment.

## sz-kr-p2p

If the payer chooses the `sz-kr-p2p` payment method, the redirection to another page will happen to finish the payment.

## sz-my-ob

If the payer chooses the `sz-my-ob` payment method, the redirection to another page will happen to finish the payment.

## sz-th-ob

If the payer chooses the `sz-th-ob` payment method, the redirection to another page will happen to finish the payment.

## sz-th-qr

If the payer chooses the `sz-th-qr` payment method, the redirection to another page will happen to finish the payment.

## sz-vn-ob

If the payer chooses the `sz-vn-ob` payment method, the redirection to another page will happen to finish the payment.

## sz-vn-p2p

If the payer chooses the `sz-vn-p2p` payment method, the redirection to another page will happen to finish the payment.

## trustgate

If the payer chooses the `trustgate` payment method, the redirection to another page will happen to finish the payment.

## tabby

You should add to the Authentication request a specific list of parameters in the “parameters” object that are needed for the `tabby` payment method.

| **Parameter**            | **Type** | **Mandatory** |                                                                                        **Description**                                                                                        |
| ------------------------ | -------- | ------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| `category`               | _String_ | Required      | Required as name of high-level category (Clothes, Electronics,etc.); or a tree of category-subcategory1-subcategory2; or id of the category and table with category-ids data mapped provided. |
| `buyer_registered_since` | _String_ | Required      |                                                  Time the customer got registred with you, in UTC, and displayed in ISO 8601 datetime format.                                                 |
| `buyer_loyalty_level`    | _Number_ | Required      |                <p>Default: 0<br>Customer's loyalty level within your store, should be sent as a number of successfully placed orders in the store with any payment methods.</p>               |

<details>

<summary>Authentication request example</summary>

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/session' \
--header 'Content-Type: application/json' \
--data-raw '{
    "merchant_key": "xxxxx-xxxxx-xxxxx",
    "operation": "purchase",
    "methods": [
        "tabby"
    ],
        "parameters": {
                "tabby": {
            "category": " Clothes ",
            "buyer_registered_since": " 2019-08-2414:15:22",
            "buyer_loyalty_level": 0
        }
    },   
    "order": {
        "number": "order-1234",
        "amount": "1000.19",
        "currency": "SAR",
        "description": "Important gift"
    },
    "cancel_url": "https://example.com/cancel",
    "success_url": "https://example.com/success",
     "customer": {
        "name": "John Doe",
        "email": "test@gmail.com"
    },
    "billing_address": {
        "country": "US",
        "state": "CA",
        "city": "Los Angeles",
        "address": "Moor Building 35274",
        "zip": "123456",
        "phone": "347771112233"
    },
    "hash": "{{session_hash}}"
}
'
```

</details>

## tamara

You should add to the Authentication request a specific list of parameters in the “parameters” object that are needed for the `tamara` payment method.

| **Parameter**          | **Type** | **Mandatory** |                 **Description**                |
| ---------------------- | -------- | ------------- | :--------------------------------------------: |
| `shipping_amount`      | _Number_ | Required      |                 Shipping amount                |
| `tax_amount`           | _Number_ | Required      |                   Tax amount                   |
| `product_reference_id` | _String_ | Required      | The unique id of the item from merchant's side |
| `product_type`         | _String_ | Required      |                  Product type                  |
| `product_sku`          | _String_ | Required      |                   Product sku                  |
| `product_amount`       | _Number_ | Required      |                 Product amount                 |

<details>

<summary>Authentication request example</summary>

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/session' \
--header 'Content-Type: application/json' \
--data-raw '{
    "merchant_key": "xxxxx-xxxxx-xxxxx",
    "operation": "purchase",
    "methods": [
        "tamara"
    ],
        "parameters": {
                "tamara": {
            " shipping_amount ": 1.01,
            "tax_amount": 1.01,
            "product_reference_id": "item54321",
            "product_type": "Clothes",
            "product_sku": "ABC-12345-S-BL",
            "product_amount": 998.17
        }
    },   
    "order": {
        "number": "order-1234",
        "amount": "1000.19",
        "currency": "SAR",
        "description": "Important gift"
    },
    "cancel_url": "https://example.com/cancel",
    "success_url": "https://example.com/success",
     "customer": {
        "name": "John Doe",
        "email": "test@gmail.com"
    },
    "billing_address": {
        "country": "US",
        "state": "CA",
        "city": "Los Angeles",
        "address": "Moor Building 35274",
        "zip": "123456",
        "phone": "347771112233"
    },
    "hash": "{{session_hash}}"
}
'
```

</details>

## togocom

This method is used for mobile numbers in Togo. If the payer chooses `togocom` payment method, confirmation is required through a mobile device.

## unipay

payment\_method = `unipay`

## unipayment

If the payer chooses the `unipaymentpayment` method, the redirection to another page will happen to finish the payment.

## vcard

payment\_method = `vcard`

## vouchstar

If the payer chooses the `vouchstar` method, the redirection to another page will happen to finish the payment.

## vp-wallet

payment\_method = `vp-wallet`

## vpayapp\_upi

If the payer chooses the `vpayapp_upi` payment method, the UPI address will be requested additionally on the Checkout page. After the Pay button is clicked, the payer will be redirected to another page to finish the payment.

## webpaygate

If the payer chooses the `webpaygate` payment method, the payer will be shown the data necessary to complete the payment.

## winnerpay

If the payer chooses the `winnerpay` payment method, the payer will be shown the data necessary to complete the payment.

## xprowirelatam-ted

If the payer chooses the `xprowirelatam-ted` payment method, the customer’s confirmation via app may be necessary to finish the payment.

## xprowirelatam-cash

If the payer chooses the `xprowirelatam-cash` payment method, the customer’s confirmation via app may be necessary to finish the payment.

## xprowirelatam-bank-transfer

If the payer chooses the `xprowirelatam-bank-transfer` payment method, the customer’s confirmation via app may be necessary to finish the payment.

## xprowirelatam-bank-slip

If the payer chooses the `xprowirelatam-bank-slip` payment method, the customer’s confirmation via app may be necessary to finish the payment.

## xprowirelatam-picpay

If the payer chooses the `xprowirelatam-picpay` payment method, the customer’s confirmation via app may be necessary to finish the payment.

## xprowirelatam-pix

If the payer chooses the `xprowirelatam-pix` payment method, the customer’s confirmation via app may be necessary to finish the payment.

## xswitfly

If the payer chooses the `xswitfly` payment method, the redirection to another page will happen to finish the payment.

## yapily

If the payer chooses the `yapily` payment method, the redirection to another page will happen to finish the payment.\
As well, the customer could be asked to enter additional data on Checkout.

Payment flow and list of the required parameters depend on specific region and payment providers rules.

| **Parameter**              | **Type** | **Mandatory** | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| -------------------------- | -------- | ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `payee_name`               | _String_ | Required      | Provide here the account provider code of the institution holding the account indicated in the Account parameter.                                                                                                                                                                                                                                                                                                                                                                                                                     |
| `payee_country`            | _String_ | Required      | This parameter enables you to attach a file to the transaction. This is useful, for example, in the case where you may want to attach a scanned receipt, or a scanned payment authorization, depending on your internally established business rules. This parameter requires you to provide the name of the file you are attaching, as a string, for example “receipt.doc” or “receipt.pdf”. Note that the contents of this parameter are ignored if you have not provided the contents of the file using NarrativeFileBase64 below. |
| **payee\_identifications** | Array    | Required      | The account identifications that identify the Payer bank account. Array should contain the objects with the Identifications combinations: `type` and `identification` parameters.                                                                                                                                                                                                                                                                                                                                                     |
| `type`                     | _String_ | Required      | <p>Used to describe the format of the account. Possible values:<br>- SORT_CODE<br>- ACCOUNT_NUMBER<br>- IBAN<br>- BBAN<br>- BIC<br>- PAN<br>- MASKED_PAN<br>- MSISDN<br>- BSB<br>- NCC<br>- ABA<br>- ABA_WIRE<br>- ABA_ACH<br>- EMAIL<br>- ROLL_NUMBER<br>- BLZ<br>- IFS<br>- CLABE<br>- CTN<br>- BRANCH_CODE<br>- VIRTUAL_ACCOUNT_ID</p>                                                                                                                                                                                             |
| `identification`           | _String_ | Required      | Account Identification. The value associated with the account identification type.                                                                                                                                                                                                                                                                                                                                                                                                                                                    |

<details>

<summary>Authentication request example</summary>

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/session' \
--header 'Content-Type: application/json' \
--data-raw '{
  "merchant_key": "xxxxx-xxxxx-xxxxx",
  "operation": "purchase",
  "methods": [
    "yapily"
  ],
  "parameters": {
    "yapily": {
      "payee_name": "Payee Name",
      "payee_country": "GB",
      "payee_identifications": [
        {
          "type": "SORT_CODE",
          "identification": "123456"
        },
        {
          "type": "ACCOUNT_NUMBER",
          "identification": "12345678"
        }
      ]
    }
  },
  "order": {
    "number": "order-1234",
    "amount": "1000.19",
    "currency": "UGX",
    "description": "Important gift"
  },
  "cancel_url": "https://example.com/cancel",
  "success_url": "https://example.com/success",
  "customer": {
    "name": "John Doe",
    "email": "test@gmail.com"
  },
  "billing_address": {
    "country": "US",
    "state": "CA",
    "city": "Los Angeles",
    "address": "Moor Building 35274",
    "zip": "123456",
    "phone": "347771112233"
  },
  "hash": "{{session_hash}}"
}
'
```

</details>

## yo-uganda-limited

You can add to the Authentication request a specific list of parameters in the “parameters” object that are needed for the\
`yo-uganda-limited` payment method.

| **Parameter**           | **Type** | **Mandatory** |                                                                                                                                                                                                                                                            **Description**                                                                                                                                                                                                                                                            |
| ----------------------- | -------- | ------------- | :-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| `AccountProviderCode`   | _String_ | Optional      |                                                                                                                                                                                                           Provide here the account provider code of the institution holding the account indicated in the Account parameter.                                                                                                                                                                                                           |
| `NarrativeFileName`     | _String_ | Optional      | This parameter enables you to attach a file to the transaction. This is useful, for example, in the case where you may want to attach a scanned receipt, or a scanned payment authorization, depending on your internally established business rules. This parameter requires you to provide the name of the file you are attaching, as a string, for example “receipt.doc” or “receipt.pdf”. Note that the contents of this parameter are ignored if you have not provided the contents of the file using NarrativeFileBase64 below. |
| `NarrativeFileBase64`   | _String_ | Optional      |                               This parameter enables you to attach a file to the transaction. This is useful, for example, in the case where you may want to attached a scanned receipt, or a scanned payment authorization, depending on your business rules. This parameter requires you to provide the contents of the file you are attaching, encoded in base-64 encoding. Note that the contents of this parameter are ignored if you have not provided a file name using NarrativeFileName above.                               |
| `ProviderReferenceText` | _String_ | Optional      |                                        In this field, enter text you wish to be present in any confirmation message which the mobile money provider network sends to the subscriber upon successful completion of the transaction. Some mobile money providers automatically send a confirmatory text message to the subscriber upon completion of transactions. This parameter allows you to provide some text which will be appended to any such confirmatory message sent to the subscriber.                                       |

<details>

<summary>Authentication request example</summary>

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/session' \
--header 'Content-Type: application/json' \
--data-raw '{
    "merchant_key": "xxxxx-xxxxx-xxxxx",
    "operation": "purchase",
    "methods": [
        "yo-uganda-limited"
    ],
        "parameters": {
                "yo-uganda-limited": {
            "AccountProviderCode": "89128734651234",
            "NarrativeFileName": "file.img",
            "NarrativeFileBase64": "iVBORw0KGgoAAAANSUhEUgAAAAEAAAADCAYAAABS3WWCAAAABHNCSVQICAgIfAhkiAAAABl0RVh0U29mdHdhcmUAZ25vbWUtc2NyZWVuc2hvdO8Dvz4AAAA1aVRYdENyZWF0aW9uIFRpbWUAAAAAANGB0YAsIDEwLdGC0YDQsC0yMDIzIDEyOjA4OjQ3ICswMzAwEHyhlwAAABdJREFUCJlj+P///38mBgYGBiYGBgYGADH7BAGR0RGuAAAAAElFTkSuQmCC",
            "ProviderReferenceText"”: "thank you",
            "AccountProviderCode": "provider1234"
        }
    },   
    "order": {
        "number": "order-1234",
        "amount": "1000.19",
        "currency": "UGX",
        "description": "Important gift"
    },
    "cancel_url": "https://example.com/cancel",
    "success_url": "https://example.com/success",
     "customer": {
        "name": "John Doe",
        "email": "test@gmail.com"
    },
    "billing_address": {
        "country": "US",
        "state": "CA",
        "city": "Los Angeles",
        "address": "Moor Building 35274",
        "zip": "123456",
        "phone": "347771112233"
    },
    "hash": "{{session_hash}}",
}
'
```

</details>

## zeropay

payment\_method = `zeropay`
