---
icon: google-pay
---

# S2S APM Brands (Appendix B)

Version: 3.6.2\
\
Released: 2025/02/26\


### Supported brands

Use the following brand names as values for the `brand` parameter in the requests:

`airtel` (available for Credit2Virtual, see Appendix C for details)\
`allpay`\
`applepay` (see Appendix B for Sale operation details)\
`araka`\
`astropay`\
`axxi-cash`\
`axxi-pin` (see Appendix B for Sale operation details)\
`a2a_transfer`\
`beeline` (see Appendix B for Sale operation details)\
`billplz` (see Appendix B for Sale operation details)\
`bitolo` (available for Credit2Virtual, see Appendix C for details)\
`bpwallet`\
`cardpaymentz`\
`citizen`\
`cnfmo`\
`crypto-btg` (available for Credit2Crypto, see Appendix D for details)\
`dcp`\
`dl`\
`dlocal`\
`doku-hpp` (see Appendix B for Sale operation details)\
`dpbanktransfer`\
`fairpay` (see Appendix B for Sale operation details; available for Credit2Virtual - see Appendix C for details)\
`fawry` (send phone number as identifier parameter for SALE operation)\
`feexpaycard`\
`gigadat`\
`googlepay` (see Appendix B for Sale operation details)\
`hayvn` (see Appendix B for Sale operation details)\
`hayvn-wdwl` (see Appendix B for Sale operation details)\
`helio`\
`help2pay` (available for Credit2Virtual, see Appendix C for details)\
`ideal_crdz`\
`instant-bills-pay`\
`ipasspay`\
`jvz` (available for Credit2Virtual, see Appendix C for details)\
`kashahpp`\
`m2p-debit` (see Appendix B for Sale operation details)\
`m2p-withdrawal` (see Appendix B for Sale operation details)\
`mcpayhpp`\
`mcpayment`\
`mercury` (available for Credit2Virtual, see Appendix C for details)\
`moov-money`\
`moov-togo`\
`mpesa` (available for Credit2Virtual, see Appendix C for details)\
`mtn-mobile-money`\
`naps`\
`netbanking-upi` (available for Credit2Virtual, see Appendix C for details)\
`next-level-finance`\
`nimbbl`\
`noda` (available for Credit2Virtual, see Appendix C for details)\
`nuvvex`\
`nv-apm`\
`om-wallet`\
`one-collection` (available for Credit2Virtual, see Appendix C for details)\
`pagsmile` (see Appendix B for Sale operation details)\
`panapay-netbanking`\
`panapay-upi`\
`papara` (available for Credit2Virtual, see Appendix C for details)\
`payablhpp`\
`payftr-in` (available for Credit2Virtual - see Appendix C for details)\
`payhere`\
`paymentrush`\
`payneteasyhpp` (required to additionally send parameters in the SALE request: payer\_first\_name, payer\_last\_name, payer\_address, payer\_city, payer\_state, payer\_zip, payer\_country, payer\_phone, payer\_email; available for Credit2Virtual - see Appendix C for details)\
`payok-payout` (available for Credit2Virtual, see Appendix C for details)\
`payok-promptpay`\
`payok-upi` (see Appendix B for Sale operation details)\
`paypal`\
`paythrough-upi` (see Appendix B for Sale operation details)\
`paytota` (available for Credit2Virtual, see Appendix C for details)\
`pix`\
`pr-cash` (available for Credit2Virtual, see Appendix C for details)\
`pr-creditcard` (available for Credit2Virtual, see Appendix C for details)\
`pr-cryptocurrency`\
`pr-online` (available for Credit2Virtual, see Appendix C for details)\
`ptbs`\
`ptn-email` (see Appendix B for Sale operation details; available for Credit2Virtual - see Appendix C for details)\
`ptn-inapp`\
`ptn-sms` (see Appendix B for Sale operation details; available for Credit2Virtual - see Appendix C for details)\
`pyk-bkmexpress` (see Appendix B for Sale operation details)\
`pyk-dana`\
`pyk-linkaja`\
`pyk-momo` (see Appendix B for Sale operation details)\
`pyk-nequipush` (see Appendix B for Sale operation details)\
`pyk-ovo`\
`pyk-paparawallet` (see Appendix B for Sale operation details)\
`pyk-payout` (available for Credit2Virtual, see Appendix C for details)\
`pyk-pix`\
`pyk-promptpay` (see Appendix B for Sale operation details)\
`pyk-shopeepay`\
`pyk-truemoney` (see Appendix B for Sale operation details)\
`pyk-upi` (see Appendix B for Sale operation details)\
`pyk-viettelpay` (see Appendix B for Sale operation details)\
`pyk-zalopay` (see Appendix B for Sale operation details)\
`sepa`\
`sepainstant`\
`sofortuber`\
`stcpay`\
`stripe-js`\
`swifipay-deposit`\
`sz-in-imps`\
`sz-in-paytm`\
`sz-in-upi` (see Appendix B for Sale operation details)\
`sz-jp-p2p`\
`sz-kr-p2p`\
`sz-my-ob`\
`sz-th-ob`\
`sz-th-qr`\
`sz-vn-ob`\
`sz-vn-p2p`\
`tabby` (see Appendix B for Sale operation details)\
`tamara` (see Appendix B for Sale operation details)\
`togocom`\
`trustgate`\
`unipayment`\
`vcard` (see Appendix B for Sale operation details)\
`vouchstar`\
`vpayapp_upi`\
`webpaygate`\
`winnerpay` (available for Credit2Virtual - see Appendix C for details)\
`xprowirelatam-ted`\
`xprowirelatam-cash`\
`xprowirelatam-bank-transfer`\
`xprowirelatam-bank-slip`\
`xprowirelatam-picpay`\
`xprowirelatam-pix`\
`xswitfly`\
`yapily` (see Appendix B for Sale operation details)\
`yo-uganda-limited` (see Appendix B for Sale operation details; available for Credit2Virtual - see Appendix C for details)\
`zeropay` (available for Credit2Virtual, see Appendix C for details)\


### Appendix B

***

You have to add to your SALE request specific list of parameters which is determined by the value for the `brand` parameter.

You should get additional information from account manager.

## vcard

To make payments on Test connector kindly set the value “vcard” for the `brand` parameter.

<details>

<summary>Sample curl request</summary>

```
curl -d "action=SALE&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&brand=vcard&
order_id=ORDER12345&order_amount=1.99&order_currency=USD&order_description=Product&
identifier=success@gmail.com&payer_ip=123.123.123.123&
return_url=https://client.site.com/return.php&hash=2702ae0c4f99506dc29b5615ba9ee3c0" https://test.apiurl.com/post-va -k
```

</details>

To simulate success scenario use `identifier` parameter value success@gmail.com and fail@gmail.com for decline.

## Apple Pay

To use the Apple Pay payment method, configure the Wallets setting in the admin dashboard. Ensure that the payment provider has confirmed the Apple Pay certificates and verified your domain in the Apple Pay account.

To enable payers to make Apple Pay payments, you can either connect to the Checkout page or work directly via the S2S APM protocol:

**Checkout Page**: No additional development is required on your side.

**S2S APM Protocol**: You must be able to obtain the Apple Pay payment token and include it in the SALE request.

Regardless of the protocol you have to make settings in the admin panel and set the following configurations:

• Merchant Identifier, Country and Shop Name – according to the merchant’s data from [Apple Pay Developper account](https://idmsa.apple.com/IDMSWebAuth/signin?appIdKey=891bd3417a7776362562d2197f89480a8547b108fd934911bcbea0110d07f757\&path=%2Faccount%2Fresources%2Fidentifiers%2Flist%2Fmerchant\&rv=1)\
\
• Certificate and Private Key – generated Apple Pay certificate and private key\
\
• Merchant Capabilities and Supported Networks – configurations for Apple Pay payments\
\
• Processing Private Key – private key that uses for payment token decryption (requires if payment provider supports).\


⚠️ Pay attention\


> _Before using Apple Pay via S2S APM protocol, you should review next section carefully._> \
> _There is a description on how you can obtain Apple Pay payment token (it refers to the official_ [_Apple Pay documentation_](https://developer.apple.com/documentation/apple_pay_on_the_web)_)_\
>

1. Each payment must be checked for [Apple Pay accessibility](https://developer.apple.com/documentation/apple_pay_on_the_web/apple_pay_js_api/checking_for_apple_pay_availability)\

2. [Show Apple Pay button](https://developer.apple.com/documentation/apple_pay_on_the_web/displaying_apple_pay_buttons_using_css) to your payers\

3. [Validate the merchant](https://developer.apple.com/documentation/apple_pay_on_the_web/apple_pay_js_api/requesting_an_apple_pay_payment_session) identity\

4. Provide a payment request and [create a session](https://developer.apple.com/documentation/apple_pay_on_the_web/apple_pay_js_api/creating_an_apple_pay_session)\

5. Receive Apple Pay payment token (paymentData object)

After you implement Apple Pay payment tokens generation on your site, you need to pass for the SALE request:

• `brand` = applepay\
\
• `parameters[paymentToken]` - Apple Pay payment token

<details>

<summary>Request example</summary>

```
curl -d "action=SALE&client_key=YOUR_CLIENT_KEY&brand=applepay&order_id=ORDER12345&order_amount=100.01&order_currency=USD&order_description=Product&payer_ip=123.123.123.123&return_url=http://client.site.com/return.php&identifier=USER_IDENTIFIER&parameters[paymentToken]={\"paymentData\":{\"data\":\"YOUR_ENCRYPTED_DATA\",\"signature\":\"YOUR_SIGNATURE\",\"header\":{\"publicKeyHash\":\"YOUR_PUBLIC_KEY_HASH\",\"ephemeralPublicKey\":\"YOUR_EPHEMERAL_PUBLIC_KEY\",\"transactionId\":\"YOUR_TRANSACTION_ID\"},\"version\":\"EC_v1\"},\"paymentMethod\":{\"displayName\":\"Visa 6244\",\"network\":\"Visa\",\"type\":\"credit\"},\"transactionIdentifier\":\"YOUR_TRANSACTION_IDENTIFIER\"},{\"transactionId\":\"TRANSACTION_ID_2\",\"version\":\"EC_v1\"},\"paymentMethod\":{\"displayName\":\"Visa 0224\",\"network\":\"Visa\",\"type\":\"debit\"},\"transactionIdentifier\":\"TRANSACTION_IDENTIFIER_2\"}&hash=YOUR_HASH" https://test.apiurl.com/post-va -k
```

</details>

We also recommend checking out the demo provided by Apple for Apple Pay: https://applepaydemo.apple.com/

***

**Set Up Apple Pay**

If you are using the S2S protocol for Apple Pay payments, you will need to configure your Apple Developer account. Follow these steps to set up Apple Pay in your Apple Developer account:

**1.** **Create a Merchant ID** in the "Certificates, Identifiers & Profiles" section.\
**2.** **Register and verify the domains** involved in the interaction with Apple Pay (e.g., the checkout page URL where the Apple Pay button is placed) in the "Merchant Domains" section.\
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

#### axxi-pin

If you set the value “axxi-pin” for the `brand` parameter you have to specify in your request the next parameters as well:

| **Parameter** | **Description**              | **Values**           | **Required** |
| ------------- | ---------------------------- | -------------------- | :----------: |
| `pin`         | Flexepin 16-digit pin number | String 16 characters |       +      |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=SALE&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&brand=axxi-pin&order_id=ORDER12345
&order_amount=1.99&order_currency=USD&order_description=Product
&identifier=0987654321-abcd&payer_first_name=John&payer_last_name=Doe&payer_address=BigStreet
&payer_country=US&payer_state=CA&payer_city=City&payer_zip=123456&payer_email=doe@example.com
&payer_phone=199999999&payer_ip=123.123.123.123&parameters[pin]=0123456789012345
&hash=2702ae0c4f99506dc29b5615ba9ee3c0" https://test.apiurl.com/post-va -k
```

</details>

## beeline

| **Parameter**     | **Description**                          | **Values** | **Required** |
| ----------------- | ---------------------------------------- | ---------- | :----------: |
| `productReceiver` | Recipient's phone number/contract number | _String_   |       -      |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=SALE&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&
brand=beeline&order_id=ORDER12345&order_amount=1.99&order_currency=USD&
order_description=Product&identifier=7053031133&payer_first_name=John&
payer_last_name=Doe&payer_address=BigStreet&payer_country=US&payer_state=CA&
payer_city=City&payer_zip=123456&payer_email=doe@example.com
&payer_phone=199999999&payer_ip=123.123.123.123&parameters[productReceiver]= 7053031177
&hash=2702ae0c4f99506dc29b5615ba9ee3c0" https://test.apiurl.com/post-va –k
```

</details>

## billplz

Take into account that `payer_first_name` and `payer_last_name` parameters are required for this brand.\
If you set the `billplz` value for the brand parameter you have to specify in your request the next parameters as well:

| Parameter             | Description         | Values   | Required |
| --------------------- | ------------------- | -------- | -------- |
| `bank_code`           | SWIFT Bank Code     | _String_ | +        |
| `bank_account_number` | Bank account number | _String_ | +        |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=SALE&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&
brand=billplz&order_id=ORDER12345&order_amount=1.99&order_currency=USD&
order_description=Product&identifier=NA&payer_first_name=John&
payer_last_name=Doe&payer_address=BigStreet&payer_country=US&payer_state=CA&
payer_city=City&payer_zip=123456&payer_email=doe@example.com&
payer_phone=199999999&payer_ip=123.123.123.123&parameters[bank_account_number]=1234567890&parameters[bank_code]=1234567890&
hash=2702ae0c4f99506dc29b5615ba9ee3c0" https://test.apiurl.com –k
```

</details>

## dl

If you set the value `dl` for the brand parameter you have to specify in your request the next parameters as well:

| **Parameter**     | **Description**        | **Values** | **Required** |
| ----------------- | ---------------------- | ---------- | :----------: |
| `document_number` | Customer's personal ID | String     |       +      |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=SALE&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&
brand=dl&order_id=ORDER12345&order_amount=1.99&order_currency=USD&order_description=Product&identifier=NA&
payer_first_name=John&payer_last_name=Doe&payer_address=BigStreet&payer_country=US&payer_state=CA&payer_city=City&
payer_zip=123456&payer_email=doe@example.com&payer_phone=199999999&
payer_ip=123.123.123.123&parameters[document_number]=1234567890&
hash=2702ae0c4f99506dc29b5615ba9ee3c0" https://test.apiurl.com –k
```

</details>

## dlocal

If you set the value `dlocal` for the brand parameter you have to specify in your request the next parameters as well:

| **Parameter**     | **Description**        | **Values** | **Required** |
| ----------------- | ---------------------- | ---------- | :----------: |
| `document_number` | Customer's personal ID | String     |       +      |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=SALE&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&
brand=dlocal&order_id=ORDER12345&order_amount=1.99&order_currency=USD&order_description=Product&identifier=NA&
payer_first_name=John&payer_last_name=Doe&payer_address=BigStreet&payer_country=US&payer_state=CA&payer_city=City&
payer_zip=123456&payer_email=doe@example.com&payer_phone=199999999&
payer_ip=123.123.123.123&parameters[document_number]=1234567890&
hash=2702ae0c4f99506dc29b5615ba9ee3c0" https://test.apiurl.com –k
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

<summary>Sample curl request</summary>

```
curl -d "action=SALE&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&brand=doku-hpp&order_id=ORDER12345&order_amount=1.99&order_currency=USD&order_description=Product&identifier=NA&payer_first_name=John&payer_last_name=Doe&payer_address=BigStreet&payer_country=US&payer_state=CA&payer_city=City&payer_zip=123456&payer_email=doe@example.com&payer_phone=199999999&payer_ip=123.123.123.123&parameters[product_reference_id]=1&parameters[product_name]=Fresh flowers&parameters[product_quantity]=1&parameters[product_sku]=FF01&parameters[product_category]=gift-and-flowers&parameters[product_url]=http://example.com&parameters[product_image_url]=http://example.com&parameters[product_type]=ABC&hash=2702ae0c4f99506dc29b5615ba9ee3c0" https://test.apiurl.com –k
```

</details>

## fairpay

If you set the value `fairpay` for the brand parameter you have to specify in your request the next parameters as well:

| **Parameter** | **Description**                                                                                                                                                                  | **Values** | **Required** |
| ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | :----------: |
| `personalId`  | Customer's personal ID. It is only required for Brazil and Chile and Colombia (For Brazil, only numeric symbols are allowed, for Chile and Colombia letters can be used as well) | String     |       -      |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=SALE&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&brand=fairpay&order_id=ORDER12345
&order_amount=1.99&order_currency=USD&order_description=Product&identifier=doe@example.com
&payer_first_name=John&payer_last_name=Doe&payer_address=BigStreet&payer_country=BR&payer_city=City
&payer_email=doe@example.com&payer_phone=5512345678901&payer_ip=123.123.123.123
&parameters[personalId]=98680500&hash=2702ae0c4f99506dc29b5615ba9ee3c0
```

</details>

## Google Pay

To provide the payers with the possibility of Google Pay™ payment you can connect to the Checkout or work directly via S2S APM protocol.\
Before using `googlepay` payment method, you should review this section carefully to meet all the Google Pay™ requirements:

1. Overview the documentation on Google Pay Integration first:\
   \
   • for sites - [Google Pay Web developer documentation](https://developers.google.com/pay/api/web)\
   \
   • for Android app - [Google Pay Android developer documentation](https://developers.google.com/pay/api/android)\

2. Make sure you complete all the items on the integration checklist:\
   \
   • for sites - [Google Pay Web integration checklist](https://developers.google.com/pay/api/web/guides/test-and-deploy/integration-checklist)\
   \
   • for Android app - [Google Pay Android integration checklist](https://developers.google.com/pay/api/android/guides/test-and-deploy/integration-checklist)\

3. To brand your site with Google Pay elements, you must meet the requirements:\
   \
   • for sites - [Google Pay Web Brand Guidelines](https://developers.google.com/pay/api/web/guides/brand-guidelines)\
   \
   • for Android app - [Google Pay Android brand guidelines](https://developers.google.com/pay/api/android/guides/brand-guidelines)\

4. The merchants must adhere to the Google Pay APIs [Acceptable Use Policy](https://payments.developers.google.com/terms/aup) and accept the terms that the [Google Pay API Terms of Service](https://payments.developers.google.com/terms/sellertos) defines.\


To work with Google Pay™ payments through gateway, you need to make settings in the admin panel. You can set the following configurations:\
\
• choose the environment: `TEST` or `PRODUCTION`\
\
• specify "Allowed Auth Method" - `PAN_ONLY` or `CRYPTOGRAM_3DS`\
\
• determine "Supported Networks" - MASTERCARD, VISA, AMEX, DISCOVER, JCB\
\
These configuration will be applied and used when platform interacts with Google Pay.\
\
If you are using **Checkout integration** to work with `googlepay` payment method, it requires no additional code implementation.\


If you are using **S2S APM integration** to work with `googlepay` payment method, you need to pass the additional parameters in the SALE request:\
\
• `brand` = googlepay\
\
• `parameters[paymentToken]` - Google Pay payment token.\
\
To obtain Google Pay payment token you should get the PaymentData according to the Google Pay API Integration documentation (see the links above).\
\
Pass the following parameters to receive PaymentData:\
\
• `allowPaymentMethods`: CARD\
\
• `tokenizationSpecification` = { "type": "PAYMENT\_GATEWAY"}\
\
• `allowedCardNetworks` = \['MASTERCARD', 'VISA', 'AMEX', 'DISCOVER', 'JCB'];\
\
• `allowedCardAuthMethods` = \['PAN\_ONLY', 'CRYPTOGRAM\_3DS'];\
\
• `gateway` = value provided by your support manager\
\
• `gatewayMerchantId` = CLIENT\_KEY (API key)\


<details>

<summary>Example of data to get PaymentData</summary>

```
{
    "apiVersionMinor": 0,
    "apiVersion": 2,
    "paymentMethodData": {
        "description": "Mastercard •••• 5179",
        "tokenizationData": {
            "type": "PAYMENT_GATEWAY",
            "token": {
                "signature": "MEYCIQC+IHxUu9Wwra2Vu6tBa2wJPCMT3pWtN1VphLGYtNVLLwIhAOkdbNje//eLrXVc+n6LOlc4PqxWOUcqwrmki/CNA1ur",
                "intermediateSigningKey": {
                    "signedKey": "{\"keyValue\":\"MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEwhRrvGB0kZ1248MEJAPwX0YfrQInhyyRM7nyGFjQuzGSViZG3QC9NMvXR9Zd6uVcVzAz+6K/+NEGIWbX8zbk6A\\u003d\\u003d\",\"keyExpiration\":\"1571208568000\"}",
                    "signatures": [
                        "MEUCIQCR6vIEiyHB8qmlho4/tLd7W8CIGrZDJlCI48CBHeFPvQIgLZJIe4cZv6pYtYYa56QCI/vvg1GqXTP3bTBjO49r5mI="
                    ]
                },
                "protocolVersion": "ECv2",
                "signedMessage": {
                    "encryptedMessage": "k85CGUMmd5PaGmQlHFqHc0HrDZmjM1yG82rFB/p31ZyUleN5nihzjOZif/BKXwg3XA2oLXqBSSWcAdfZwXxCEOqvAQE+kpTC7qVO4wTPip6RruraT6vO9M9FtIf0kcqzYSAN7pl1SUA5jC9rHrqucPoh0Sspup78SWBt8TnmVKC9O3sQhIZ3SMoGOG4mdjtIrgCwWH2cbrxUH0dHAqOe5ANGOY/mII4nTEp7xaKu41hK/kFE15zVyqgPJXP9bPxzMVk/ozEZSfhSzVTS+9stjkUkT5EibKB3O1bbxyybKcGQ5vxBOtNOBKKdNuqNmN2eEK3UbSKb1M3gv45gT9cCeEvlzO7Wg0N100vUBp9RPDN9TH4tj4w578sWFKfTi6FsSrZB27SGWcU0EaKHO9buo94mRBY5yqffF3bKg5mAMPzjDhyHSujqWKAs9C5fBzMEuEr2z7A23kfLqBceH5uS9LJMiZyVKCwfpY9u2XyCjKdp7Iu003d",
                    "ephemeralPublicKey": "BAMDAtfgcPNuzItrwGLigGj3rtxmyHhZLwx1B4RJZ2oo11jpFf3SA6a3utryCMmlCEcqLfLn6WDH2ArrNBGnEwu003d",
                    "tag": "TqhZXY53Fe4QBKafm6NqS6EzwVeiKXhRlp8NeWrAu003d"
                }
            }
        },
        "type": "CARD",
        "info": {
            "cardNetwork": "MASTERCARD",
            "cardDetails": "5179"
        }
    }
}
```

</details>

As a result you receive you will be returned a dataset with `PaymentData.token` object contains data that you need add to the SALE request as `parameters[paymentToken]` parameter.

<details>

<summary>Request Example</summary>

```
curl -d "action=SALE&client_key=YOUR_CLIENT_KEY&brand=googlepay&order_id=ORDER_ID&order_amount=100.01&order_currency=USD&order_description=Product&payer_ip=123.123.123.123&return_url=http://client.site.com/return.php&identifier=USER_IDENTIFIER&parameters[paymentToken]={\"apiVersionMinor\": 0, \"apiVersion\": 2, \"paymentMethodData\": {\"description\": \"Mastercard •••• 5179\", \"tokenizationData\": {\"type\": \"PAYMENT_GATEWAY\", \"token\": {\"signature\": \"YOUR_SIGNATURE\", \"intermediateSigningKey\": {\"signedKey\": \"YOUR_SIGNED_KEY\", \"signatures\": [\"YOUR_SIGNATURE_2\"]}, \"protocolVersion\": \"ECv2\", \"signedMessage\": {\"encryptedMessage\": \"ENCRYPTED_MESSAGE\", \"ephemeralPublicKey\": \"EPHEMERAL_PUBLIC_KEY\", \"tag\": \"TAG\"}}}, \"type\": \"CARD\", \"info\": {\"cardNetwork\": \"MASTERCARD\", \"cardDetails\": \"5179\"}}}&hash=YOUR_HASH" https://test.apiurl.com/post-va -k
```

</details>

⚠️ **Pay attention**\


> _in the case of PAN\_ONLY, the responsibility for passing 3ds is transferred to the acquirer, through which the payment is processed_

***

**Set up Google Pay**

The admin panel settings apply to merchants integrated with the platform via the Checkout protocol.\
\
\
For merchants using the S2S protocol and obtaining the Google Pay payment token independently, these settings will not apply.\
\
\
Exceptions are "Environment" and “Private Key” configurations.\
\
\
The "Environment" configuration remains relevant, as it defines the environment where the token was generated and from which the platform expects to receive payment data. Ensure that the environment used to generate the payment token matches the corresponding setting in the admin panel to avoid processing issues.\
\
\
The “Private Key” is required for Google token decryption. If it is absent, the card flow cannot be applied to the payment.\
\
\
Additionally, to work with Google Pay payments, you must verify the website domains from which Google Pay is processed in the Google Business Console.

## hayvn

| **Parameter**      | **Description**                                                                                                       | **Values** | **Required** |
| ------------------ | --------------------------------------------------------------------------------------------------------------------- | ---------- | :----------: |
| `payment_currency` | <p>Currency for the specified payment amount.<br><strong>Available values:</strong><br>BTC<br>ETH<br>USDC<br>USDT</p> | _String_   |       +      |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=SALE&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&
brand=hayvn&order_id=ORDER12345&order_amount=1.99&order_currency=USD&
order_description=Product&identifier=NA&payer_first_name=John&
payer_last_name=Doe&payer_address=BigStreet&payer_country=US&payer_state=CA&
payer_city=City&payer_zip=123456&payer_email=doe@example.com
&payer_phone=199999999&payer_ip=123.123.123.123&parameters[payment_currency]= BTC
&hash=2702ae0c4f99506dc29b5615ba9ee3c0" https://test.apiurl.com/post-va –k
```

</details>

## hayvn-wdwl

| **Parameter** | **Description**                                                  | **Values** | **Required** |
| ------------- | ---------------------------------------------------------------- | ---------- | :----------: |
| `address`     | Address of the recipient.                                        | _String_   |       +      |
| `currency`    | <p>Recipient address currency.<br>BTC<br>ETH<br>USDC<br>USDT</p> | _String_   |       +      |
| `network`     | <p>Recipient address network.<br>BTC<br>ETH<br>BSC<br>TRX</p>    | _String_   |       +      |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=SALE&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&
brand= hayvn-wdwl&order_id=ORDER12345&order_amount=1.99&order_currency=USD&
order_description=Product&identifier=NA&payer_first_name=John&
payer_last_name=Doe&payer_address=BigStreet&payer_country=US&payer_state=CA&
payer_city=City&payer_zip=123456&payer_email=doe@example.com
&payer_phone=199999999&payer_ip=123.123.123.123&
parameters[address]= tb1qz8xs2xrun82tzaug9uaj206p34l3eu0tjw52qd
&parameters[currency]=BTC&parameters[network]=BTC
&hash=2702ae0c4f99506dc29b5615ba9ee3c0" https://test.apiurl.com/post-va –k
```

</details>

## m2p-debit

If you set the value `m2p-debit` for the brand parameter you have to specify in your request the next parameters as well:

| **Parameter**         | **Description**                                        | **Values** | **Required** |
| --------------------- | ------------------------------------------------------ | ---------- | :----------: |
| `paymentGatewayName`  | Name of payment gateway that will be used for deposit  | String     |       +      |
| `paymentCurrency`     | Name of payment currency that will be used for deposit | String     |       +      |
| `tradingAccountLogin` | Depositor’s trading account id                         | String     |       -      |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=SALE&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&brand=m2p-debit&order_id=ORDER12345
&order_amount=1.99&order_currency=USD&order_description=Product
&identifier=0987654321-abcd&payer_first_name=John&payer_last_name=Doe&payer_address=BigStreet
&payer_country=US&payer_state=CA&payer_city=City&payer_zip=123456&payer_email=doe@example.com
&payer_phone=199999999&payer_ip=123.123.123.123&parameters[paymentGatewayName]=gateway&parameters[paymentCurrency]=BTC&parameters[tradingAccountLogin]=login
&hash=2702ae0c4f99506dc29b5615ba9ee3c0" https://test.apiurl.com/post-va -k
```

</details>

## m2p-withdrawal

If you set the value `m2m-withdrawal` for the brand parameter you have to specify in your request the next parameters as well:

| **Parameter**         | **Description**                                               | **Values** | **Required** |
| --------------------- | ------------------------------------------------------------- | ---------- | :----------: |
| `withdrawCurrency`    | CRYPTO currency Cryptocurrency to which currency is converted | String     |       +      |
| `address`             | User cryptocurrency wallet address to withdraw                | String     |       +      |
| `tradingAccountLogin` | Trading account ID of a user requesting a withdrawal          | String     |       -      |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=SALE&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&brand=m2p-withdrawal&order_id=ORDER12345
&order_amount=1.99&order_currency=USD&order_description=Product
&identifier=0987654321-abcd&payer_first_name=John&payer_last_name=Doe&payer_address=BigStreet
&payer_country=US&payer_state=CA&payer_city=City&payer_zip=123456&payer_email=doe@example.com
&payer_phone=199999999&payer_ip=123.123.123.123&parameters[withdrawalCurrency]=BTC&parameters[address]=43j5hg734rhbfj&parameters[tradingAccountLogin]=login
&hash=2702ae0c4f99506dc29b5615ba9ee3c0" https://test.apiurl.com/post-va -k
```

</details>

## pagsmile

If you set the value `pagsmile` for the brand parameter you have to specify in your request the next parameters. The following parameters are required, except for transactions in Guatemala, Panama, or Costa Rica:

| **Parameter**     | **Type** | **Mandatory** |                                                                             **Description**                                                                            |
| ----------------- | -------- | ------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| `document_number` | _String_ | Conditional   |          <p>Customer's ID.<br><strong>Condition:</strong> If the parameter is NOT specified in the request, then it will be collected on the Checkout page</p>         |
| `document_type`   | _String_ | Conditional   | <p>Customer's identification type.<br><strong>Condition:</strong> If the parameter is NOT specified in the request, then it will be collected on the Checkout page</p> |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=SALE&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&
brand=pagsmile&order_id=ORDER12345&order_amount=1.99&order_currency=BRL&
order_description=Product&identifier=NA&payer_first_name=John&
payer_last_name=Doe&payer_address=BigStreet&payer_country=BR&
payer_city=City&payer_zip=123456&payer_email=doe@example.com&
payer_phone=199999999&payer_ip=123.123.123.123&
parameters[document_number]=11032341882&
parameters[document_type]=CPF&
hash=2702ae0c4f99506dc29b5615ba9ee3c0" https://test.apiurl.com –k
```

</details>

## payok-upi

If you set the value `payok-upi` for the brand parameter you have to specify in your request the next parameter required for India:

| Parameter     | Description                                                                                                                                             | Values | Required |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | -------- |
| `personal_id` | <p>UPI ID<br><strong>Condition:</strong> If the parameter is NOT specified in the request for India, then it will be collected on the Checkout page</p> | String | +/-      |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=SALE&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&
brand=payok-upi&order_id=ORDER12345&order_amount=1.99&order_currency=USD&
order_description=Product&identifier=NA&payer_first_name=John&
payer_last_name=Doe&payer_address=BigStreet&payer_country=US&payer_state=CA&
payer_city=City&payer_zip=123456&payer_email=doe@example.com&
payer_phone=199999999&payer_ip=123.123.123.123&parameters[personal_id]=1234567890&
hash=2702ae0c4f99506dc29b5615ba9ee3c0" https://test.apiurl.com –k
```

</details>

## paythrough-upi

If you set the value `paythrough-upi` for the brand parameter you have to specify in your request the next parameters as well:

| **Parameter** | **Description** | **Values** | **Required** |
| ------------- | --------------- | ---------- | :----------: |
| `upi_id`      | UPI ID          | _String_   |       +      |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=SALE&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&brand=paythrough-upi
&order_id=ORDER12345&order_amount=100.99&order_currency=INR&order_description=Product
&identifier=0987654321-abcd&payer_first_name=John&payer_last_name=Doe
&payer_address= Shop No.12, Rendezvous, Raviraj Oberoi Cmplx
&payer_country=IN&payer_state= Maharashtra&payer_city=Mumbai
&payer_zip= 400053&payer_email=doe@example.com&payer_phone= 9818218660 &payer_ip=123.123.123.123&parameters[upi_id]= joyoberoihrl@okaxis
&hash=2702ae0c4f99506dc29b5615ba9ee3c0" https://test.apiurl.com/post-va -k
```

</details>

## ptn-email

If you set the value `ptn-email` for the brand parameter you have to specify in your request the next parameters as well:

| **Parameter**            | **Description**                                                                                               | **Values** | **Required** |
| ------------------------ | ------------------------------------------------------------------------------------------------------------- | ---------- | :----------: |
| `SecurityQuestion`       | The security question                                                                                         | _String_   |       -      |
| `SecurityQuestionAnswer` | <p>SecurityQuestionAnswer standards:<br>- No spaces.<br>- Length must be between 3 and 25 characters long</p> | _String_   |       +      |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=SALE&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&
brand=ptn-email&order_id=ORDER12345&order_amount=1.99&order_currency=USD&
order_description=Product&identifier=NA&payer_first_name=John&
payer_last_name=Doe&payer_address=BigStreet&payer_country=US&payer_state=CA&
payer_city=City&payer_zip=123456&payer_email=doe@example.com
&payer_phone=199999999&payer_ip=123.123.123.123&
parameters[SecurityQuestion]=color&
parameters[SecurityQuestionAnswer]=red
&hash=2702ae0c4f99506dc29b5615ba9ee3c0" https://test.apiurl.com –k
```

</details>

## ptn-sms

If you set the value `ptn-sms` for the brand parameter you have to specify in your request the next parameters as well:

| **Parameter**            | **Description**                                                                                               | **Values** | **Required** |
| ------------------------ | ------------------------------------------------------------------------------------------------------------- | ---------- | :----------: |
| `SecurityQuestion`       | The security question                                                                                         | _String_   |       -      |
| `SecurityQuestionAnswer` | <p>SecurityQuestionAnswer standards:<br>- No spaces.<br>- Length must be between 3 and 25 characters long</p> | _String_   |       -      |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=SALE&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&
brand= ptn-sms&order_id=ORDER12345&order_amount=1.99&order_currency=USD&
order_description=Product&identifier=NA&payer_first_name=John&
payer_last_name=Doe&payer_address=BigStreet&payer_country=US&payer_state=CA&
payer_city=City&payer_zip=123456&payer_email=doe@example.com
&payer_phone=199999999&payer_ip=123.123.123.123&
parameters[SecurityQuestion]=color&
parameters[SecurityQuestionAnswer]=red
&hash=2702ae0c4f99506dc29b5615ba9ee3c0" https://test.apiurl.com –k
```

</details>

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

If you set the value `pyk-upi` for the brand parameter you have to specify in your request the next parameter required for India:

| Parameter     | Description                                                                                                                                                           | Values | Required |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | -------- |
| `personal_id` | <p>Payer's Identity ID.<br><strong>Condition:</strong> If the parameter is NOT specified in the request for India, then it will be collected on the Checkout page</p> | String | +/-      |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=SALE&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&
brand=pyk-upi&order_id=ORDER12345&order_amount=1.99&order_currency=USD&
order_description=Product&identifier=NA&payer_first_name=John&
payer_last_name=Doe&payer_address=BigStreet&payer_country=US&payer_state=CA&
payer_city=City&payer_zip=123456&payer_email=doe@example.com&
payer_phone=199999999&payer_ip=123.123.123.123&parameters[personal_id]=1234567890&
hash=2702ae0c4f99506dc29b5615ba9ee3c0" https://test.apiurl.com –k
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

## sz-in-upi

If you set the value `sz-in-upi` for the brand parameter you have to specify in your request the next parameters as well:

| **Parameter** | **Description**     | **Values** | **Required** |
| ------------- | ------------------- | ---------- | :----------: |
| `upiAddress`  | Payer's UPI address | String     |       +      |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=SALE&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&brand=sz-in-upi&order_id=ORDER12345
&order_amount=1.99&order_currency=INR&order_description=Product&identifier=doe@example.com
&payer_first_name=John&payer_last_name=Doe&payer_email=doe@example.com&payer_ip=123.123.123.123
&parameters[upiAddress]=address@upi&hash=2702ae0c4f99506dc29b5615ba9ee3c0
```

</details>

## tabby

If you set the value `tabby` for the brand parameter you have to specify in your request the next parameters as well:

| **Parameter**            | **Description**                                                                                                                                                                                            | **Values** | **Required** |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | :----------: |
| `category`               | <p>Required as name of high-level category (Clothes, Electronics,etc.);<br>or a tree of category-subcategory1-subcategory2;<br>or id of the category and table with category-ids data mapped provided.</p> | String     |       +      |
| `buyer_registered_since` | Time the customer got registred with you, in UTC, and displayed in ISO 8601 datetime format.                                                                                                               | String     |       +      |
| `buyer_loyalty_level`    | <p>Default: 0<br>Customer's loyalty level within your store, should be sent as a number of successfully placed orders in the store with any payment methods.</p>                                           | Number     |       +      |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=SALE&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&brand=tabby
&order_id=ORDER12345&order_amount=100.99&order_currency=SAR
&order_description=Product&identifier=0987654321-abcd
&payer_first_name=John&payer_last_name=Doe&payer_address=BigStreet
&payer_country=US&payer_state=CA&payer_city=City&payer_zip=123456
&payer_email=doe@example.com&payer_phone=0987654321&payer_ip=123.123.123.123
&parameters[category]=Clothes&parameters[buyer_registered_since]=2019-08-2414:15:22&parameters[buyer_loyalty_level]=0
&hash=2702ae0c4f99506dc29b5615ba9ee3c0" https://test.apiurl.com/post-va -k
```

</details>

## tamara

If you set the value `tamara` for the brand parameter you have to specify in your request the next parameters as well:

| **Parameter**          | **Description**                                | **Values** | **Required** |
| ---------------------- | ---------------------------------------------- | ---------- | :----------: |
| `shipping_amount`      | Shipping amount                                | Number     |       +      |
| `tax_amount`           | Tax amount                                     | Number     |       +      |
| `product_reference_id` | The unique id of the item from merchant's side | String     |       +      |
| `product_type`         | Product type                                   | String     |       +      |
| `product_sku`          | Product sku                                    | String     |       +      |
| `product_amount`       | Product amount                                 | Number     |       +      |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=SALE&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004
&brand=tamara&order_id=ORDER12345&order_amount=100.99&order_currency=SAR
&order_description=Product&identifier=0987654321-abcd
&payer_first_name=John&payer_last_name=Doe&payer_address=BigStreet
&payer_country=US&payer_state=CA&payer_city=City&payer_zip=123456
&payer_email=doe@example.com&payer_phone=0987654321&payer_ip=123.123.123.123
&parameters[shipping_amount]=1.01&parameters[tax_amount]=1.01
&parameters[product_reference_id]=item125430&parameters[product_type]=Clothes
&parameters[product_sku]=ABC-12345-S-BL&parameters[product_amount]=998.17
&hash=2702ae0c4f99506dc29b5615ba9ee3c0" https://test.apiurl.com/post-va -k
```

</details>

## yapily

If you set the value `yapily` for the brand parameter you have to specify in your request the next parameters as well:

| Parameter               | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Values   | Required |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | -------- |
| `payee_name`            | Provide here the account provider code of the institution holding the account indicated in the Account parameter.                                                                                                                                                                                                                                                                                                                                                                                                                     | _String_ | +        |
| `payee_country`         | This parameter enables you to attach a file to the transaction. This is useful, for example, in the case where you may want to attach a scanned receipt, or a scanned payment authorization, depending on your internally established business rules. This parameter requires you to provide the name of the file you are attaching, as a string, for example “receipt.doc” or “receipt.pdf”. Note that the contents of this parameter are ignored if you have not provided the contents of the file using NarrativeFileBase64 below. | _String_ | +        |
| `payee_identifications` | The account identifications that identify the Payer bank account. Array should contain the objects with the Identifications combinations: `type` and `identification` parameters.                                                                                                                                                                                                                                                                                                                                                     | _Array_  | +        |
| `type`                  | <p>Used to describe the format of the account.<br>Possible values:<br>• SORT_CODE<br>• ACCOUNT_NUMBER<br>• IBAN<br>• BBAN<br>• BIC<br>• PAN<br>• MASKED_PAN<br>• MSISDN<br>• BSB<br>• NCC<br>• ABA<br>• ABA_WIRE<br>• ABA_ACH<br>• EMAIL<br>• ROLL_NUMBER<br>• BLZ<br>• IFS<br>• CLABE<br>• CTN<br>• BRANCH_CODE<br>• VIRTUAL_ACCOUNT_ID</p>                                                                                                                                                                                          | _String_ | +        |
| `identification`        | Account Identification. The value associated with the account identification type.                                                                                                                                                                                                                                                                                                                                                                                                                                                    | _String_ | +        |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=SALE&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&brand=yapily&order_id=ORDER12345&order_amount=1.99&order_currency=GBP&order_description=Product&identifier=0987654321-abcd&payer_first_name=John&payer_last_name=Doe&payer_address=BigStreet&payer_country=GB&payer_state=Region&payer_city=City&payer_zip=123456&payer_email=doe@example.com&payer_phone=199999999&payer_ip=123.123.123.123&parameters[payee_name]=Jane Smith&parameters[payee_country]=GB&parameters[payee_identifications][0][type]=SORT_CODE&parameters[payee_identifications][0][identification]=123456&hash=2702ae0c4f99506dc29b5615ba9ee3c0" https://test.apiurl.com/post-va -k

```

</details>

## yo-uganda-limited

If you set the value `yo-uganda-limited` for the brand parameter you have to specify in your request the next parameters as well:

| **Parameter**           | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | **Values** | **Required** |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | :----------: |
| `AccountProviderCode`   | Provide here the account provider code of the institution holding the account                                                                                                                                                                                                                                                                                                                                                                                                                                                         | String     |       -      |
| `NarrativeFileName`     | This parameter enables you to attach a file to the transaction. This is useful, for example, in the case where you may want to attach a scanned receipt, or a scanned payment authorization, depending on your internally established business rules. This parameter requires you to provide the name of the file you are attaching, as a string, for example “receipt.doc” or “receipt.pdf”. Note that the contents of this parameter are ignored if you have not provided the contents of the file using NarrativeFileBase64 below. | String     |       -      |
| `NarrativeFileBase64`   | This parameter enables you to attach a file to the transaction. This is useful, for example, in the case where you may want to attached a scanned receipt, or a scanned payment authorization, depending on your business rules. This parameter requires you to provide the contents of the file you are attaching, encoded in base-64 encoding. Note that the contents of this parameter are ignored if you have not provided a file name using NarrativeFileName above.                                                             | String     |       -      |
| `ProviderReferenceText` | In this field, enter text you wish to be present in any confirmation message which the mobile money provider network sends to the subscriber upon successful completion of the transaction. Some mobile money providers automatically send a confirmatory text message to the subscriber upon completion of transactions. This parameter allows you to provide some text which will be appended to any such confirmatory message sent to the subscriber.                                                                              | String     |       -      |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=SALE&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&brand=yo-uganda-limited&order_id=ORDER12345
&order_amount=1.99&order_currency=UGX&order_description=Product
&identifier=0987654321-abcd&payer_first_name=John&payer_last_name=Doe&payer_address=BigStreet
&payer_country=US&payer_state=CA&payer_city=City&payer_zip=123456&payer_email=doe@example.com
&payer_phone=199999999&payer_ip=123.123.123.123&parameters[AccountProviderCode]=provider54321
&parameters[NarrativeFileName]=file.img&parameters[NarrativeFileBase64]= iVBORw0KGgoAAAANSUhE
UgAAAAEAAAADCAYAAABS3WWCAAAABHNCSVQICAgIfAhkiAAAABl0RVh0U29mdHdhcmUAZ25vbWUtc2NyZWVuc2hvdO8Dvz
4AAAA1aVRYdENyZWF0aW9uIFRpbWUAAAAAANGB0YAsIDEwLdGC0YDQsC0yMDIzIDEyOjA4OjQ3ICswMzAwEHyhlwAAABdJ
REFUCJlj+P///38mBgYGBiYGBgYGADH7BAGR0RGuAAAAAElFTkSuQmCC&parameters[ProviderReferenceText]=thank you
&hash=2702ae0c4f99506dc29b5615ba9ee3c0" https://test.apiurl.com/post-va -k
```

</details>

### Appendix C

***

You have to add to your CREDIT2VIRTUAL request specific list of parameters which is determined by the value for the `brand` parameter.

You should get additional information from account manager.

## airtel

If you set the value `airtel` for the brand parameter you have to specify in your request the next parameters as well:

| **Parameter**   | **Description**                                   | **Values** | **Required** |
| --------------- | ------------------------------------------------- | ---------- | :----------: |
| `payee_country` | Transaction country (2-letters code)              | String     |       +      |
| `payee_phone`   | Mobile Number validated according to ITU-T E.164. | String     |       +      |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=CREDIT2VIRTUAL&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&brand=airtel&order_id=ORDER12345&order_amount=100
&order_currency=UGX&order_description=Product&parameters[payee_country]=UG&parameters[payee_phone]=0987654321
&hash=a1a6de416405ada72bb47a49176471dc" https://test.apiurl.com/post -k

```

</details>

## astropay

If you set the value `astropay` for the brand parameter you have to specify in your request the next parameters as well:

| **Parameter**      | **Description**                                                                                                                                   | **Values**                 | **Required** |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------- | :----------: |
| `country`          | Transaction Country ISO Code                                                                                                                      | String 2 characters        |       +      |
| `merchant_user_id` | A unique identifier on the merchant's end for the user (user email).                                                                              | String up to 64 characters |       +      |
| `phone`            | User's phone number. Must contain country code. If you are using the phone number the country code must be added to the number. Ex: 5561900001234 | String up to 50 characters |       +      |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=CREDIT2VIRTUAL&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&brand=astropay
&order_id=ORDER12345&order_amount=1.99&order_currency=USD&order_description=Product
&parameters[country]=BR&parameters[merchant_user_id]=test@test.com&parameters[phone]=5561900001234
&hash=a1a6de416405ada72bb47a49176471dc" https://test.apiurl.com/post -k
```

</details>

## bitolo (INR)

If you set the value `bitolo-inr` for the brand parameter you have to specify in your request the next parameters as well:

| **Parameter**              | **Description**                      | **Values** | **Required** |
| -------------------------- | ------------------------------------ | ---------- | :----------: |
| `account_holder_firstname` | First name of the user               | String     |       +      |
| `account_holder_lastname`  | Last name of the user                | String     |       +      |
| `bank_name`                | Bank name. Example: AXIS             | String     |       +      |
| `account_no`               | Account number. Example: 12xxxxx1231 | String     |       +      |
| `ifsc_code`                | IFSC code                            | String     |       +      |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=CREDIT2VIRTUAL&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&brand=bitolo-inr&order_id=ORDER12345&order_amount=1000.99&order_currency=INR&order_description=Product&parameters[account_holder_firstname]=John&parameters[account_holder_lastname]=Smith&parameters[bank_name]=AXIS&parameters[account_no]=0987654321&parameters[ifsc_code]=1234567890&hash=a1a6de416405ada72bb47a49176471dc" https://test.apiurl.com/post -k
```

</details>

## bitolo (BRL)

If you set the value `bitolo-brl` for the brand parameter you have to specify in your request the next parameters as well:

| **Parameter**              | **Description**                                                            | **Values** | **Required** |
| -------------------------- | -------------------------------------------------------------------------- | ---------- | :----------: |
| `mode_of_payout`           | Mode of payout. Values: BANK or PIX                                        | String     |       +      |
| `account_holder_firstname` | First name of the user                                                     | String     |       +      |
| `account_holder_lastname`  | Last name of the user                                                      | String     |       +      |
| `bank_code`                | Bank code will be shared as a document. Required if mode of payout is BANK | String     |       -      |
| `bank_branch`              | Branch code of the bank. Required if Mode of Payout is BANK                | String     |       -      |
| `account_no`               | Account number of the receiver. Required if Mode of Payout is BANK         | String     |       -      |
| `tax_id`                   | CPF of the receiver                                                        | String     |       +      |
| `pix_key`                  | PIX key. Required if mode of payout is PIX                                 | String     |       -      |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=CREDIT2VIRTUAL&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&brand=bitolo-brl&order_id=ORDER12345
&order_amount=1000.99&order_currency=BRL&order_description=Product&parameters[mode_of_payout]=PIX
&parameters[account_holder_firstname]=John&parameters[account_holder_lastname]=Smith&parameters[tax_id]=0987654321
&parameters[pix_key]=1234567890&hash=a1a6de416405ada72bb47a49176471dc" https://test.apiurl.com/post -k
```

</details>

## fairpay

If you set the value `fairpay` for the brand parameter you have to specify in your request the next parameters as well:

| **Parameter**    | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                              | **Values**   | **Required** |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | :----------: |
| `method`         | <p>The payout method. List of values:<br>• pix - for PIX, Brasil<br>• bank-transfer-br - for Bank Transfer, Brasil<br>• spei - for SPEI, Mexico<br>• bank-transfer-cl - for Bank transfer, Chile<br>• bank-transfer-pe - for Bank transfer, Peru<br>• bank-transfer-ec - Bank transfer, Ecuador<br>• bank-transfer-co - for Colombia -Bank transfer</p>                                                                                      | String       |       +      |
| `firstName`      | Beneficiary's first name                                                                                                                                                                                                                                                                                                                                                                                                                     | String       |       +      |
| `lastName`       | Beneficiary's last name                                                                                                                                                                                                                                                                                                                                                                                                                      | String       |       +      |
| `documentNumber` | Beneficiary's document number.                                                                                                                                                                                                                                                                                                                                                                                                               | String       |       +      |
| `documentType`   | <p>Beneficiary's document type. List of values:<br>• Brasil: One of CPF, CNPJ<br>• Mexico: One of RFC, CURP or ND.<br>• Chile: Should be one of RUT, RUN, PAS, CE.<br>• Peru-Bank transfer: Should be one of DNI, RUC, PAS, CE.<br>• Ecuador-Bank transfer: Should be one of CEDULA, RUC, PAS.</p>                                                                                                                                           | String       |       +      |
| `bankCode`       | Beneficiary's bank code. Optional for payment method PIX (PIX, Brasil)                                                                                                                                                                                                                                                                                                                                                                       | String       |       +      |
| `account`        | Beneficiary's account number.                                                                                                                                                                                                                                                                                                                                                                                                                | String       |       +      |
| `accountType`    | <p>Beneficiary's account type. List of values:<br>• PIX, Brasil: One of: CPF, CNPJ, EVP, PHONE, EMAIL<br>• Bank Transfer, Brasil: One of: SAVINGS, CHECKING<br>• SPEI, Mexico: One of debit, phone or clabe.<br>• Bank transfer, Chile: Should be one of CHECKING, SAVINGS, VISTA, RUT, SALARY.<br>• Bank transfer, Peru: Should be one of CHECKING, SAVINGS.<br>• Bank transfer, Ecuador:  Should be one of CHECKING, SAVINGS, VIRTUAL.</p> | String       |       +      |
| `accountDigit`   | Account Digit. Required only for payment method `bank-transfer-br` (Bank Transfer, Brasil)                                                                                                                                                                                                                                                                                                                                                   | String       |       -      |
| `branch`         | Branch. Required only for payment method `bank-transfer-br` (Bank Transfer, Brasil)                                                                                                                                                                                                                                                                                                                                                          | Max length 4 |       -      |
| `region`         | Region. Required only for payment method `bank-transfer-pe` (Bank transfer, Peru)                                                                                                                                                                                                                                                                                                                                                            | String       |       -      |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=CREDIT2VIRTUAL&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004
&brand=fairpay&order_id=ORDER12345&order_amount=1.99&order_currency=USD
&order_description=Product&parameters[firstName]=John&parameters[lastName]=Doe
&parameters[method]=bank-transfer-br&parameters[documentNumber]=21532652313
&parameters[documentType]=CPF&parameters[bankCode]=001&parameters[account]=12345678
&parameters[accountType]=CHECKING&parameters[accountDigit]=4&parameters[branch]=1234
&hash="a1a6de416405ada72bb47a49176471dc" https://test.apiurl.com/post -k
```

</details>

## jvz

If you set the value `jvz` for the brand parameter you have to specify in your request the next parameters as well:

| **Parameter**      | **Description**                                                                   | **Values** | **Required** |
| ------------------ | --------------------------------------------------------------------------------- | ---------- | ------------ |
| `holder_name`      | Bank account holder name (mandatory for bank transfer).                           | String     | +            |
| `bank_acc_number`  | Bank account number (mandatory for bank transfer).                                | String     | +            |
| `branch_id`        | Branch ID (optional).                                                             | String     | -            |
| `name_of_bank`     | Name of the bank (mandatory for bank transfer).                                   | String     | +            |
| `banking_code`     | Swift code or other banking code (mandatory for bank transfer).                   | String     | +            |
| `branch_bank`      | Branch of the bank (optional).                                                    | String     | -            |
| `address_of_bank`  | Address of the bank branch where the account is opened (optional).                | String     | -            |
| `account_type`     | Type of account (e.g., savings account, current account).                         | String     | -            |
| `receiver_name`    | Recipient’s full name (required if needed as per regional compliance).            | String     | -            |
| `receiver_address` | Recipient’s address (optional).                                                   | String     | -            |
| `user_fname`       | First name of the source user.                                                    | String     | -            |
| `user_lname`       | Last name of the source user.                                                     | String     | -            |
| `user_email`       | Email of the source user (e.g., xyz@abc.com).                                     | String     | +            |
| `user_address`     | Address of the source user.                                                       | String     | -            |
| `user_city`        | City of the source user.                                                          | String     | -            |
| `user_state`       | State of the source user.                                                         | String     | -            |
| `user_country`     | 2 or 3-digit transaction country code.                                            | String     | +            |
| `user_zip`         | Zip code of the source user.                                                      | String     | -            |
| `user_phone`       | Contact number of the source user.                                                | String     | +            |
| `user_doc_id`      | Payer user ID used for verification (mandatory as per regional compliance).       | String     | -            |
| `user_doc_type`    | Payer user document used for verification (mandatory as per regional compliance). | String     | -            |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=CREDIT2VIRTUAL&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&brand=jvz&order_id=ORDER12345&order_amount=100&order_currency=INR&order_description=Product&parameters[holder_name]=John%20Doe&parameters[bank_acc_number]=1234567890&parameters[name_of_bank]=ExampleBank&parameters[banking_code]=EXAMPLECODE&parameters[branch_id]=001&parameters[branch_bank]=MainBranch&parameters[address_of_bank]=123%20Bank%20Street&parameters[account_type]=savings&parameters[receiver_name]=Jane%20Doe&parameters[receiver_address]=456%20Receiver%20Street&parameters[user_fname]=John&parameters[user_lname]=Doe&parameters[user_email]=xyz@abc.com&parameters[user_address]=789%20User%20Street&parameters[user_city]=Kampala&parameters[user_state]=Central&parameters[user_country]=IN&parameters[user_zip]=12345&parameters[user_phone]=0987654321&parameters[user_doc_id]=987654321&parameters[user_doc_type]=Passport&hash=a1a6de416405ada72bb47a49176471dc" https://test.apiurl.com/post -k
```

</details>

## help2pay

If you set the value `help2pay` for the brand parameter you have to specify in your request the next parameters as well:

| **Parameter**         | **Description**                        | **Values**     | **Required** |
| --------------------- | -------------------------------------- | -------------- | :----------: |
| `ClientIP`            | Server IP                              | String (1-80)  |       +      |
| `bankCode`            | Bank code                              | String (1-50)  |       +      |
| `toBankAccountName`   | Recipient Account Name                 | String (1-50)  |       +      |
| `toBankAccountNumber` | Recipient Account Number               | String (1-50)  |       +      |
| `toProvince`          | Customer Bank located within a country | String (1-200) |       -      |
| `toCity`              | Customer Bank located City             | String (1-200) |       -      |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=CREDIT2VIRTUAL&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&brand=help2pay&order_id=ORDER12345&order_amount=1.99
&order_currency=MYR&order_description=Product
&parameters[ClientIP]=127.987.65.432&parameters[bankConde]=AFF &parameters[toBankAccountName]=Name-54321
&parameters[toBankAccountNumber]=number-12345&parameters[toProvince]=KL
&parameters[toCity]=Kuala Lumpur&hash=a1a6de416405ada72bb47a49176471dc" https://test.apiurl.com/post -k
```

</details>

## mercury

If you set the value `mercury` for the brand parameter you have to specify in your request the next parameters as well:

| **Parameter**                     | **Description**                                                                                                                               | **Values** | **Required** |
| --------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | :----------: |
| `externalMemo`                    | Optional external memo                                                                                                                        | String     |       -      |
| `recipient_name`                  | Recipient’s name                                                                                                                              | String     |       +      |
| `recipient_email`                 | Recipient’s e-mail                                                                                                                            | String     |       +      |
| `recipient_accountNumber`         | Recipient account number                                                                                                                      | String     |       +      |
| `recipient_routingNumber`         | Recipient’s routing number                                                                                                                    | String     |       +      |
| `recipient_electronicAccountType` | <p>Recipient’s electronic account type.<br>Possible values:<br>businessChecking<br>businessSavings<br>personalChecking<br>personalSavings</p> | String     |       +      |
| `recipient_address1`              | Recipient’s Address line 1                                                                                                                    | String     |       +      |
| `recipient_address2`              | Recipient’s Address line 1                                                                                                                    | String     |       -      |
| `recipient_city`                  | Recipient’s city                                                                                                                              | String     |       +      |
| `recipient_region`                | Recipient’s region. Either a two-letter US state code like "CA" for California or a free-form identification of a particular region worldwide | String     |       +      |
| `recipient_postalCode`            | Recipient’s postal code                                                                                                                       | String     |       +      |
| `recipient_country`               | Recipient’s country. Use [ISO 3166](https://www.iban.com/country-codes) Alpha2 code                                                           | String     |       +      |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=CREDIT2VIRTUAL&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&brand=mercury&order_id=ORDER12345&order_amount=1.99
&order_currency=USD&order_description=Product
&parameters[externalMemo]=Memo for customer
&parameters[recipient_name]=Recipient Name
&parameters[recipient_email]=recipient3@mail.com
&parameters[recipient_accountNumber]=0987654321
&parameters[recipient_routingNumber]=1234567890
&parameters[recipient_electronicAccountType]=businessSavings
&parameters[recipient_address1]= 5604 Clarke Address Dr
&parameters[recipient_address2]=build 1a&parameters[recipient_ city]= Memphis
&parameters[recipient_region]=TN&&parameters[recipient_postalCode]=38115
&parameters[recipient_ country]=US&hash=a1a6de416405ada72bb47a49176471dc" https://test.apiurl.com/post -k
```

</details>

## mpesa

If you set the value `mpesa` for the brand parameter you have to specify in your request the next parameters as well:

| **Parameter** | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                       | **Values** | **Required** |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | :----------: |
| `CommandID`   | <p>This is a unique command that specifies B2C transaction type.<br>• SalaryPayment: This supports sending money to both registered and unregistered M-Pesa customers.<br>• BusinessPayment: This is a normal business to customer payment, supports only M-PESA registered customers.<br>• PromotionPayment: This is a promotional payment to customers. The M-PESA notification message is a congratulatory message. Supports only M-PESA registered customers.</p> | _String_   |       +      |
| `PartyB`      | This is the customer mobile number to receive the amount. The number should have the country code (254) without the plus sign.                                                                                                                                                                                                                                                                                                                                        | _String_   |       +      |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=CREDIT2VIRTUAL&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&brand=mpesa&order_id=ORDER12345&order_amount=100
&order_currency=UGX&order_description=Product&parameters[CommandID]= SalaryPayment&parameters[PartyB]=0987654321
&hash=a1a6de416405ada72bb47a49176471dc" https://test.apiurl.com/post -k
```

</details>

## noda

If you set the value `noda` for the brand parameter you have to specify in your request the next parameters as well:

| **Parameter**     | **Description**                                                                                             | **Values** | **Required** |
| ----------------- | ----------------------------------------------------------------------------------------------------------- | ---------- | :----------: |
| `iban`            | <p>International Bank Account Number (IBAN) of the Payout Recipient.<br>Example: DE75512108001245126199</p> | String     |       +      |
| `beneficiaryName` | The full name of the payee as known to their banks                                                          | String     |       +      |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=CREDIT2VIRTUAL&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&brand=noda&order_id=ORDER12345
&order_amount=1.99&order_currency=MYR&order_description=Product&parameters[iban]=GB33BUKB20201555555555
&parameters[beneficiaryName]=John Smith&hash=a1a6de416405ada72bb47a49176471dc" https://test.apiurl.com/post -k
```

</details>

## one-collection

If you set the value "one-collection" for the `brand` parameter you have to specify in your request the next parameters as well:

| **Parameter**   | **Description**                                                          | **Values** | **Required** |
| --------------- | ------------------------------------------------------------------------ | ---------- | :----------: |
| `FullName`      | Encrypted submission using platform public key, with RSA mode encryption | String     |       +      |
| `BankName`      | Bank Name                                                                | String     |       +      |
| `BankCode`      | Bank Code                                                                | String     |       +      |
| `BranchName`    | Branch Name                                                              | String     |       +      |
| `BranchCode`    | Branch Code                                                              | String     |       +      |
| `AccountNumber` | Account Number                                                           | String     |       +      |

<details>

<summary>Sample curl request</summary>

```
curl –d "action=CREDIT2VIRTUAL&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004
&brand=one-collection&order_id=ORDER12345&order_amount=1.99&order_currency=USD
&order_description=Product&parameters[FullName]= LzA0Px8pIUSi0VYwyFweLwWAJ/b6y
vJaefGUTwWwIiG6Qv031EM6GjMlhAwxQax2c1Zrd0riWSVb0ph7YntebMKY93ZiLpcYisjpBNnqqpx
owhyvdN79EKAysaajGQwvT5WlLShKvfS/73zubTiLPd8YhxdNJwmH0KQ6ScXyNZv4YKc9gEj+PDIzW
P3SY7LwJtqDWuWAPi2jp5eCo5LJNUs2X44+BXFP6aALA7t8SaaNfw7/9gk1dG2rIuyjp2AuIXiRDFh
X4LNOND7jC5El3xSyfGdmkcZS8g0EO+EmPQAxpcZ8goBJ4uOINtNJWgo0aGOsUZd4jkzT6za8TOyLAw==
&parameters[BankName]=SBI&parameters[BankCode]=0038&parameters[BranchName]=Branch
&parameters[BranchCode]=619&parameters[AccountNumber]=account54321
&parameters[BankCode]=0038&hash=a1a6de416405ada72bb47a49176471dc" https://test.apiurl.com/post –k
```

</details>

## papara

If you set the value `papara` for the brand parameter you have to specify in your request the next parameters as well:

| **Parameter**   | **Description**                | **Values** | **Required** |
| --------------- | ------------------------------ | ---------- | ------------ |
| `memberId`      | Customer ID                    | String     | +            |
| `NameSurname`   | Customer Full Name             | String     | +            |
| `AccountNumber` | The Account Number for payment | String     | +            |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=CREDIT2VIRTUAL&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&brand=papara&order_id=ORDER12345&order_amount=100&order_currency=TRY&order_description=Product&parameters[memberId]=123456789&parameters[NameSurname]=John%20Doe&parameters[AccountNumber]=987654321&hash=a1a6de416405ada72bb47a49176471dc" https://test.apiurl.com/post -k
```

</details>

## Paygate10 Netbanking/UPI

If you set the value `netbanking-upi` for the brand parameter you have to specify in your request the next parameters as well:

| **Parameter**    | **Description**                           | **Values**                  | **Required** |
| ---------------- | ----------------------------------------- | --------------------------- | :----------: |
| `country_code`   | Customer’s country. Must be 2-letter code | String 2 characters         |       +      |
| `user_name`      | Merchant’s customer “user name”           | String up to 100 characters |       +      |
| `account_name`   | Beneficiary Bank Account Name             | String up to 100 characters |       +      |
| `account_number` | Beneficiary Bank Account Number           | String up to 20 characters  |       +      |
| `bank_name`      | Bank name                                 | String up to 100 characters |       +      |
| `bank_ifsc`      | Bank IFSC code                            | String up to 20 characters  |       +      |
| `bank_branch`    | Bank branch name                          | String up to 100 characters |       +      |
| `bank_address`   | Bank address                              | String up to 250 characters |       +      |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=CREDIT2VIRTUAL&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&brand=netbanking-upi&order_id=ORDER12345&order_amount=1.99
&order_currency=USD&order_description=Product&parameters[country_code]=IN
&parameters[user_name]=test&parameters[account_name]=test
&parameters[account_number]=1234567890&parameters[bank_name]=apnaBank
&parameters[bank_ifsc]=ASBL00000007&parameters[bank_branch]=Mumbai
&parameters[bank_address]=Sakinaka&hash=a1a6de416405ada72bb47a49176471dc" https://test.apiurl.com/post -k
```

</details>

## payneteasyhpp

If you set the value `payneteasyhpp` for the brand parameter you have to specify in your request the next parameters as well:

| Parameter             | Description                                                       | Values       | Required                                                                                        |
| --------------------- | ----------------------------------------------------------------- | ------------ | ----------------------------------------------------------------------------------------------- |
| \`\`\`first\_name\`\` | Receiver's first name. Can also be sent as first\_name            | String (128) | <p>Required for countries:<br>Uganda (UGX)<br>Kenia (KES)</p>                                   |
| `last_name`           | Receiver's last name. Can also be sent as last\_name              | String (128) | <p>Required for countries:<br>Uganda (UGX)<br>Kenia (KES)<br></p>                               |
| `payee_first_name`    | Receiver's first name. Can also be sent as first\_name            | String (128) | <p>Required for countries:<br>Ghana (GHS)<br></p>                                               |
| `payee_last_name`     | Receiver's last name. Can also be sent as last\_name              | String (128) | <p>Required for countries:<br>Ghana (GHS)</p>                                                   |
| `phone`               | The phone parameter is the wallet, where the money would be sent. | String       | <p>Required for countries:<br>Uganda (UGX)<br>Kenia (KES)</p>                                   |
| `account_number`      | Bank account number                                               | String (32)  | <p>Required for countries:<br>Nigeria (NGN)<br>Cameron (XAF)<br>Ghana (GHS)<br>Zambia (ZMW)</p> |
| `account_name`        | Bank account name                                                 | String (128) | <p>Required for countries:<br>Nigeria (NGN)<br>Cameron (XAF)<br>Ghana (GHS)</p>                 |
| `payee_phone`         | Receiver's phone. Can also be sent as phone                       | String (128) | <p>Required for countries:<br>Nigeria (NGN)<br>Cameron (XAF)<br>Ghana (GHS)<br>Zambia (ZMW)</p> |
| `payee_email`         | Receiver's email. Can also be sent as email.                      | String (128) | <p>Required for countries:<br>Nigeria (NGN)</p>                                                 |
| `bank_name`           | Name of the bank                                                  | String (255) | <p>Required for<br>Nigeria (NGN)</p>                                                            |
| `bank_code`           | Bank code                                                         | String (32)  | <p>Required for<br>Nigeria (NGN)</p>                                                            |
| `payee_country`       | Receiver's country code. Can also be sent as country              | String (3)   | Required for Ghana (GHS)                                                                        |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=CREDIT2VIRTUAL&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004
&brand=payneteasyhpp&order_id=ORDER12345&order_amount=1000&order_currency=GHS
&order_description=Product&parameters[payee_first_name]=John&parameters[payee_last_name]=Doe
&parameters[payee_phone]=12345678901&parameters[payee_country]=GH
&parameters[account_name]=QWE&parameters[account_number]=12345678901
&hash=a1a6de416405ada72bb47a49176471dc" https://test.apiurl.com/post -k

```

</details>

## payftr-in

If you set the value `payftr-in` for the brand parameter you have to specify in your request the next parameters as well:

| Parameter             | Description              | Values | Required |
| --------------------- | ------------------------ | ------ | -------- |
| `country`             | Transaction country code | String | +        |
| `account_holder_name` | Account holder name      | String | +        |
| `account_number`      | Account Number           | String | +        |
| `bank_name`           | Bank Name                | String | +        |
| `bank_code`           | Bank code                | String | +        |
| `phone`               | Phone                    | String | +        |
| `email`               | Email                    | String | +        |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=CREDIT2VIRTUAL
&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004
&brand=payftr-in
&order_id=ORDER12345
&order_amount=100.00
&order_currency=INR
&order_description=Product
&parameters[country]=IN
&parameters[account_holder_name]=Jane Doe
&parameters[account_number]=1233455790
&parameters[bank_name]=India Bank
&parameters[bank_code]=HDFC0000123
&parameters[phone]=1234567890
&parameters[email]=sakinaka@email.com
&hash=a1a6de416405ada72bb47a49176471dc" https://test.apiurl.com/post -k
```

</details>

## payok-payout

If you set the value `payok-payout` for the brand parameter you have to specify in your request the next parameters as well:

| Parameter        | Description                                   | Values       | Required    |
| ---------------- | --------------------------------------------- | ------------ | ----------- |
| `countryCode`    | The Country Code of the corresponding country | String (4)   | +           |
| `number`         | Beneficiary Account Number                    | String (30)  | +           |
| `orgId`          | Bank ID                                       | String (20)  | +           |
| `orgCode`        | Bank Code                                     | String (20)  | +           |
| `orgName`        | Bank Name                                     | String (45)  | +           |
| `holderName`     | Bank Card Holder Name                         | String (200) | +           |
| `firstName`      | Beneficiary's First Name                      | String (20)  | +           |
| `lastName`       | Beneficiary's Last Name                       | String (20)  | +           |
| `email`          | User's email                                  | String (45)  | +           |
| `phone`          | The cardholder's phone number                 | String (20)  | +           |
| `personalId`     | Identity ID of the payee                      | String (12)  | Conditional |
| `personalType`\* | ID type of the payee                          | String (10)  | Conditional |
| `accountType`\*  | Account type of the payee                     | String (10)  | Conditional |

\* - conditional parameters are required for specific method, region and currency that is determined by the payment providers rules.

<details>

<summary>Sample curl request</summary>

```
curl -d "action=CREDIT2VIRTUAL&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&brand=payok-payout&order_id=ORDER12345&order_amount=100&order_currency=USD&order_description=Product&parameters[countryCode]=IN&parameters[number]=33440987654321&parameters[orgId]=0005&parameters[orgCode]=BANKCODE&parameters[orgName]=Bank Name&parameters[holderName]=Jane Doe&parameters[firstName]=Homer&parameters[lastName]=Simpson&parameters[email]=hsimpson@email.com&parameters[phone]=0987654321&hash=a1a6de416405ada72bb47a49176471dc" https://test.apiurl.com/post -k

```

</details>

## paytota (MTN)

If you set the value `paytota` for the brand parameter you have to specify in your request the next parameters as well:

| **Parameter** | **Description**                                   | **Values** | **Required** |
| ------------- | ------------------------------------------------- | ---------- | :----------: |
| `payee_phone` | Mobile Number validated according to ITU-T E.164. | String     |       +      |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=CREDIT2VIRTUAL&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&brand=paytota&order_id=ORDER12345&order_amount=100&order_currency=UGX
&order_description=Product&parameters[payee_phone]=0987654321
&hash=a1a6de416405ada72bb47a49176471dc" https://test.apiurl.com/post -k
```

</details>

## payretailers

If you set the value `payretailers` for the brand parameter you have to specify in your request the next parameters as well:

| **Parameter**                  | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | **Values** | **Required** |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | :----------: |
| `BeneficiaryFirstName`         | Beneficiary first name                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | _String_   |       +      |
| `BeneficiaryLastName`          | Beneficiary last name                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | _String_   |       +      |
| `PayoutBeneficiaryTypeCode` \* | Payout beneficiary type code                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | _String_   |  Conditional |
| `DocumentType`                 | Document type                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | _String_   |       +      |
| `DocumentNumber`               | Document number                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | _String_   |       +      |
| `Email`                        | Beneficiary e-mail                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | _String_   |       +      |
| `Phone` \*                     | Beneficiary phone                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | _String_   |  Conditional |
| `City` \*                      | Beneficiary city                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | _String_   |  Conditional |
| `Address` \*                   | Beneficiary address                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | _String_   |  Conditional |
| `Country`                      | Beneficiary country, 2-letter code                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | _String_   |       +      |
| `BankName`                     | <p>Bank name. Possible values:<br>001 - Banco do Brasil S.A.<br>341 - Banco Itaú S.A.<br>033 - Banco Santander (Brasil) S.A.<br>237 - Banco Bradesco S.A.<br>237 - Next Tecnologia e Serviços Digitais<br>104 - Caixa Econômica Federal<br>422 - Banco Safra S.A.<br>748 - Banco Cooperativo Sicredi S.A.<br>041 - Banco do Estado do Rio Grande do Sul S.A. BANRISUL<br>208 - BANCO BTG PACTUAL S.A.<br>655 - Neon Pagamentos<br>077 - Banco Inter S.A.<br>121 - Banco Agibank S.A.<br>212 - Banco Original S.A.<br>260 - Nubank<br>336 - Banco C6 S.A.</p> | _String_   |       +      |
| `AbaSwift`                     | The swift number of the bank used by the customer.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | _String_   |  Conditional |
| `AccountAgencyNumber` \*       | Account agency number                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | _String_   |       -      |
| `AccountNumber`                | Account number                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | _String_   |       +      |
| `PayoutChannel` \*             | <p>Channel through which the payout will be processed.<br>Values: CASH or ONLINE</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | _String_   |  Conditional |
| `RecipientPixKey` \*           | The PIX key of the customer.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | _String_   |  Conditional |
| `PayoutAccountTypeCode`        | <p>Payout account type. Possible values:<br>0001 - Poupança<br>0002 - Conta Corrente</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | _String_   |       +      |
| `TermsApproved`                | Possible values: Y/N                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | _String_   |       -      |

\* - conditional parameters are required for specific method, region and currency that is determined by the payment providers rules.

<details>

<summary>Sample curl request (Country = BR)</summary>

```
curl -d curl -d "action=CREDIT2VIRTUAL&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&brand=payretailers&order_id=ORDER12345&order_amount=100&order_currency=EUR&order_description=Product&parameters[BeneficiaryFirstName]=Jonh&parameters[BeneficiaryLastName]=Doe&parameters[DocumentType]=1&parameters[DocumentNumber]=12346&parameters[Email]=jonhdoe@email.com&parameters[Phone]=112233445566&parameters[Country]=BR&parameters[BankName]=033&parameters[AccountAgencyNumber]=8585&parameters[AccountNumber]=002555343456&parameters[PayoutAccountTypeCode]=002&hash=a1a6de416405ada72bb47a49176471dc" https://test.apiurl.com/post -k
```

</details>

<details>

<summary>Sample curl request (Country = CL)</summary>

```
curl -d "action=CREDIT2VIRTUAL&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&brand=payretailers&order_id=ORDER12345&order_amount=100&order_currency=EUR&order_description=Product&parameters[BeneficiaryFirstName]=Jonh&parameters[BeneficiaryLastName]=Doe&parameters[PayoutBeneficiaryTypeCode]=person&parameters[DocumentType]=1&parameters[DocumentNumber]=12346&parameters[Email]=jonhdoe@email.com&parameters[Country]=CL&parameters[BankName]=033&parameters[AccountNumber]=002555343456&parameters[PayoutAccountTypeCode]=002&hash=a1a6de416405ada72bb47a49176471dc" https://test.apiurl.com/post -k
```

</details>

<details>

<summary>Sample curl request (Country = EC or PE)</summary>

```
curl -d "action=CREDIT2VIRTUAL&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&brand=payretailers&order_id=ORDER12345&order_amount=100&order_currency=EUR&order_description=Product&parameters[BeneficiaryFirstName]=Jonh&parameters[BeneficiaryLastName]=Doe&parameters[DocumentType]=1&parameters[DocumentNumber]=12346&parameters[Email]=jonhdoe@email.com&parameters[City]=Magdalena Del Mar&parameters[Address]=Jirón Raúl Porras, 426&parameters[Phone]=112233445566&parameters[Country]=PE&parameters[BankName]=033&parameters[AccountNumber]=002555343456&parameters[PayoutAccountTypeCode]=002&parameters[TermsApproved]=false&hash=a1a6de416405ada72bb47a49176471dc" https://test.apiurl.com/post -k
```

</details>

## ptn-email

If you set the value `ptn-email` for the brand parameter you have to specify in your request the next parameters as well:

| **Parameter**            | **Description**                                                                                               | **Values**   | **Required** |
| ------------------------ | ------------------------------------------------------------------------------------------------------------- | ------------ | :----------: |
| `CustomerName`           | Indicates Customer Name                                                                                       | _String(60)_ |       +      |
| `Email`                  | Indicates Customer's Email                                                                                    | _String(50)_ |       -      |
| `SecurityQuestion`       | The security question                                                                                         | _String(40)_ |       +      |
| `SecurityQuestionAnswer` | <p>SecurityQuestionAnswer standards:<br>- No spaces.<br>- Length must be between 3 and 25 characters long</p> | _String(25)_ |       +      |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=CREDIT2VIRTUAL&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&brand=ptn-email&
order_id=ORDER12345&order_amount=100
&order_currency=CAD&order_description=Product&
parameters[CustomerName]=Jonh Doe&parameters[Email]=email@cutomer.com
&parameters[SecurityQuestion]=color&
parameters[SecurityQuestionAnswer]=red
&hash=a1a6de416405ada72bb47a49176471dc" https://test.apiurl.com/post -k
```

</details>

## ptn-sms

If you set the value `ptn-sms` for the brand parameter you have to specify in your request the next parameters as well:

| **Parameter**            | **Description**                                                                                               | **Values**    | **Required** |
| ------------------------ | ------------------------------------------------------------------------------------------------------------- | ------------- | :----------: |
| `CustomerName`           | Indicates Customer Name                                                                                       | _String (60)_ |       +      |
| `PhoneNumber`            | Indicates customer's phone number with Country Code                                                           | _Integer(10)_ |       +      |
| `SecurityQuestion`       | The security question                                                                                         | _String(40)_  |       +      |
| `SecurityQuestionAnswer` | <p>SecurityQuestionAnswer standards:<br>- No spaces.<br>- Length must be between 3 and 25 characters long</p> | _String(25)_  |       +      |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=CREDIT2VIRTUAL&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&brand=ptn-sms&
order_id=ORDER12345&order_amount=100
&order_currency=CAD&order_description=Product&
parameters[CustomerName]=Jonh Doe&parameters[PhoneNumber]=0987654321&
parameters[SecurityQuestion]=color&
parameters[SecurityQuestionAnswer]=red&
hash=a1a6de416405ada72bb47a49176471dc" https://test.apiurl.com/post -k
```

</details>

## pyk-payout

If you set the value `pyk-payout` for the brand parameter you have to specify in your request the next parameters as well:

| Parameter        | Description                                   | Values       | Required    |
| ---------------- | --------------------------------------------- | ------------ | ----------- |
| `countryCode`    | The Country Code of the corresponding country | String (4)   | +           |
| `number`         | Beneficiary Account Number                    | String (30)  | +           |
| `orgId`          | Bank ID                                       | String (20)  | +           |
| `orgCode`        | Bank Code                                     | String (20)  | +           |
| `orgName`        | Bank Name                                     | String (45)  | +           |
| `holderName`     | Bank Card Holder Name                         | String (200) | +           |
| `firstName`      | Beneficiary's First Name                      | String (20)  | +           |
| `lastName`       | Beneficiary's Last Name                       | String (20)  | +           |
| `email`          | User's email                                  | String (45)  | +           |
| `phone`          | The cardholder's phone number                 | String (20)  | +           |
| `personalId`     | Identity ID of the payee                      | String (12)  | Conditional |
| `personalType`\* | ID type of the payee                          | String (10)  | Conditional |
| `accountType`\*  | Account type of the payee                     | String (10)  | Conditional |

\* - conditional parameters are required for specific method, region and currency that is determined by the payment providers rules.

<details>

<summary>Sample curl request</summary>

```
curl -d "action=CREDIT2VIRTUAL&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&brand=pyk-payout&order_id=ORDER12345&order_amount=100&order_currency=USD&order_description=Product&parameters[countryCode]=IN&parameters[number]=33440987654321&parameters[orgId]=0005&parameters[orgCode]=BANKCODE&parameters[orgName]=Bank Name&parameters[holderName]=Jane Doe&parameters[firstName]=Homer&parameters[lastName]=Simpson&parameters[email]=hsimpson@email.com&parameters[phone]=0987654321&hash=a1a6de416405ada72bb47a49176471dc" https://test.apiurl.com/post -k

```

</details>

## yo-uganda-limited

If you set the value `yo-uganda-limited` for the brand parameter you have to specify in your request the next parameters as well:

| **Parameter**           | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | **Values** | **Required** |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | :----------: |
| `payee_phone`           | This is a numerical value representing the account number of the mobile money account where you wish to transfer the funds to. This is typically the telephone number of the mobile phone receiving the amount. Telephone numbers MUST have the international code prepended, without the “+” sign. An example of a mobile money account number which would be valid for the MTN Uganda network is 256771234567.                                                                                                                      | _String_   |       +      |
| `AccountProviderCode`   | Provide here the account provider code of the institution holding the account indicated in the Account parameter.                                                                                                                                                                                                                                                                                                                                                                                                                     | _String_   |       -      |
| `NarrativeFileName`     | This parameter enables you to attach a file to the transaction. This is useful, for example, in the case where you may want to attach a scanned receipt, or a scanned payment authorization, depending on your internally established business rules. This parameter requires you to provide the name of the file you are attaching, as a string, for example “receipt.doc” or “receipt.pdf”. Note that the contents of this parameter are ignored if you have not provided the contents of the file using NarrativeFileBase64 below. | _String_   |       -      |
| `NarrativeFileBase64`   | This parameter enables you to attach a file to the transaction. This is useful, for example, in the case where you may want to attached a scanned receipt, or a scanned payment authorization, depending on your business rules. This parameter requires you to provide the contents of the file you are attaching, encoded in base-64 encoding. Note that the contents of this parameter are ignored if you have not provided a file name using NarrativeFileName above.                                                             | _String_   |       -      |
| `ProviderReferenceText` | In this field, enter text you wish to be present in any confirmation message which the mobile money provider network sends to the subscriber upon successful completion of the transaction. Some mobile money providers automatically send a confirmatory text message to the subscriber upon completion of transactions. This parameter allows you to provide some text which will be appended to any such confirmatory message sent to the subscriber.                                                                              | _String_   |       -      |

<details>

<summary>Sample curl request</summary>

```
curl –d "action=CREDIT2VIRTUAL&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&brand= yo-uganda-limited
&order_id=ORDER12345&order_amount=1.99&order_currency=UGX&order_description=Product&parameters[payee_phone]=+ 256771234567
&parameters[AccountProviderCode]=provider54321&parameters[NarrativeFileName]=file.img&parameters[NarrativeFileBase64]= iVBORw0KGgoAAAANSUhEUgAAAAEAAAADCAYAAABS3WWCAAAABHNCSVQICAgIfAhkiAAAABl0RVh0U29mdHdhcmUAZ25vbWUtc2NyZWVuc2hvdO8Dvz4AAA
A1aVRYdENyZWF0aW9uIFRpbWUAAAAAANGB0YAsIDEwLdGC0YDQsC0yMDIzIDEyOjA4OjQ3ICswMzAwEHyhlwAAABdJREFUCJlj+P///38mBgYGBiYGBgYG
ADH7BAGR0RGuAAAAAElFTkSuQmCC&parameters[ProviderReferenceText]=thank you &hash=a1a6de416405ada72bb47a49176471dc" https://test.apiurl.com/post –k
```

</details>

## winnerpay

If you set the value `winnerpay` for the brand parameter you have to specify in your request the next parameters as well:

| **Parameter** | **Description**                                                                                                                                                                                                 | **Values** | **Required** |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | :----------: |
| `contact_id`  | The Bitso-generated, unique identifier of your receiving account. You get this identifier back when you create the contact account.                                                                             | String     |      +/-     |
| `beneficiary` | The recipient of the withdrawn funds.It is required if you don’t send “contact\_id” parameter.                                                                                                                  | String     |      +/-     |
| `clabe`       | The unique, 18-number identifier of the receiving bank account that follows the Mexican standard for account numbering (Clave Bancaria Estandarizada).It is required if you don’t send “contact\_id” parameter. | String     |      +/-     |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=CREDIT2VIRTUAL
&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004
&brand=winnerpay
&order_id=ORDER12345
&order_amount=100.00
&order_currency=MXN
&order_description=Product
&parameters[beneficiary]= recipient name
&parameters[clabe]= 6543210987654321
&hash=a1a6de416405ada72bb47a49176471dc" https://test.apiurl.com/post -k
```

</details>

## zeropay (BTC Globals)

If you set the value “zeropay” for the `brand` parameter you have to specify in your request the next parameters as well:

| **Parameter**     | **Description**                                                                                                                                                                                                                                | **Values** | **Required** |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | :----------: |
| `accountNumber`   | This is mandatory for payment methods **other** than UPI.                                                                                                                                                                                      | String     |  Conditional |
| `beneficiaryName` | This is mandatory for payment methods **other** than UPI.                                                                                                                                                                                      | String     |  Conditional |
| `ifscCode`        | 11 digit bank code, this is mandatory for payment methods **other** than **UPI**.                                                                                                                                                              | String     |  Conditional |
| `upiId`           | This is mandatory only if paymentMethod selected is **UPI**.                                                                                                                                                                                   | String     |  Conditional |
| `payoutType`      | = EXPRESS                                                                                                                                                                                                                                      | String     |       +      |
| `paymentMethod`   | <p>Allowed Values:<br>IMPS<br>NEFT<br>RTGS<br>UPI<br>Payment method the merchant wants to use. If opting for UPI payment method, upiId is a mandatory parameter and for other payment methods accountNumber &#x26; ifscCode are mandatory.</p> | String     |       +      |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=CREDIT2VIRTUAL&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004&brand=zeropay&order_id=ORDER12345&order_amount=1.99
&order_currency=INR&order_description=Product
&parameters[upiId]=2267896548@ybl&parameters[payoutType]=EXPRESS
&parameters[paymentMethod]=UPI&hash=a1a6de416405ada72bb47a49176471dc" https://test.apiurl.com/post -k
```

</details>

### Appendix D

***

You have to add to your CREDIT2CRYPTO request specific list of parameters which is determined by the value for the `brand` parameter.

You should get additional information from account manager.

## crypto-btg

If you set the value `crypto-btg` for the brand parameter you have to specify in your request the next parameters as well:

| **Parameter** | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | **Values** | **Required** |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | :----------: |
| `address`     | <p>Destination address<br>Max length: &#x3C;= 250 characters<br>Example: 2MvrwRYBAuRtPTiZ5MyKg42Ke55W3fZJfZS.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | String     |       -      |
| `type`        | <p>Required for transactions from MPC wallets. "acceleration" speeds up transactions with a certain nonce by adjusting the gas setting. "accountSet" is for XRP AccountSet transactions. "enabletoken" is for SOL. "stakingLock" and "stakingUnlock" are for Stacks delegations. "transfer" is for native-asset transfers. "trustline" is for Stellar trustline transactions.<br>Possible types include: [acceleration, accountSet, enabletoken, stakingLock, stakingUnlock, transfer, transfertoken, trustline]<br><br>For AVAX, possible types include: 'addValidator', 'export', and 'import'.<br><br>For XRP, possible types include: 'payment' and 'accountSet'. The default is 'payment'.<br><br>For STX, type is required.</p> | String     |       -      |
| `memo_id`     | <p>Memo ID (for XLM (Stellar) currency)<br>Integer that is typically used to refer to an ID in the recipient's system (such as an invoice number or a customer ID).</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | String     |       -      |

<details>

<summary>Sample curl request</summary>

```
curl -d "action=CREDIT2VIRTUAL&client_key=c2b8fb04-110f-11ea-bcd3-0242c0a85004
&brand=crypto-btg&order_id=ORDER12345&order_amount=10&order_currency=XRP
&order_description=Product&parameters[address]=2MvrwRYBAuRtPTiZ5MyKg42Ke55W3fZJfZS 
&parameters[type]=payment&parameters[memo_id]=0987654321
&hash=a1a6de416405ada72bb47a49176471dc" https://test.apiurl.com/post -k
```

</details>
