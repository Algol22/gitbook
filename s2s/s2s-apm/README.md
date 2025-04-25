---
icon: cc-apple-pay
---

# S2S APM

Version: 3.6.2\
\
Released: 2025/02/26\


### Introduction

***

This document describes integration procedures and VIRTUAL PAYMENT GATEWAY protocol usage for e-commerce merchants.

VIRTUAL PAYMENT GATEWAY protocol implements acquiring payments (purchases) with using specific API interaction.

### Integration Process

***

#### Integration process

With all Payment Platform POST requests at Notification URL the Merchant must return the string OK if he/she successfully received data or return ERROR.

⚠️ **Pay attention**

> _Note that the notification URL may be temporarily blocked due to consistently receiving timeouts in response to the callback._> \
> _If five timeouts accumulate within five minutes for a merchant’s notification URL, it will be blocked for 15 minutes. During this block, all merchants associated with the URL will not receive notifications._> \
> _The block automatically lifts after 15 minutes._> \
> _Additionally, it is possible to manually unblock the URL through the admin panel by navigating to Configuration → Merchants → Edit Merchant. In this case, the block will be removed immediately._> \
> _The timeout counter resets if a callback response is successfully processed. For instance, if there are four timeouts within five minutes but a successful response on the sixth minute, the counter resets._

⚠️ **Pay attention**

> _In the case of cascading, the logic for sending callbacks differs._> \
> _If cascading is triggered for the order, you will receive callbacks for the first payment attempt, where the decline occurred and which caused the cascading, as well as for the last payment attempt, where the final status of the order (settled or declined) is determined._> \
> _Additionally, for the last attempt, the callback will include only the final status. That is, callbacks regarding 3DS or redirects will not be sent._> \
> _Callbacks for intermediate attempts (between the first decline and the last payment attempt) are not sent._

⚠️ **Pay attention**

> If the required method is `GET` and the `redirect_url` contains query parameters, such as:\
> `redirect_url=https://example.domain.com/?parameter=1`
>
> There are two possible approaches for implementing the redirect:
>
> _**Option 1**_: Redirecting the customer by sending query parameters within the form inputs\
>> \
> Parse the query parameters from the `redirect_url` and pass them as input elements in an HTML> \
> form:
>
> **Example:**
>
> ```
> <form action="https://example.domain.com" method="GET">
>  <input name="parameter" value="1">
>  <input type="submit" value="Go">
> </form>
> ```
>
> _**Option 2**_: Redirect Using JavaScript\
>> \
> Use JavaScript to redirect the customer to the specified `redirect_url` with the query parameters:
>
> **Example:**
>
> ```
> document.location = 'https://example.domain.com/?parameter=1';
> ```

#### Client Registration

Before you get an account to access Payment Platform, you must provide the following data to the Payment Platform administrator.

| **Data**       | **Description**                                                                                             |
| -------------- | ----------------------------------------------------------------------------------------------------------- |
| Callback URL   | URL which will be receiving the notifications of the processing results of your request to Payment Platform |
| Contact e-mail | Client\`s contact email                                                                                     |
| IP List        | List of your IP addresses, from which requests to Payment Platform will be sent.                            |

With all Payment Platform POST requests at Callback URL the Client must return the string "OK" if he/she successfully received data or return "ERROR".

You should get the following information from administrator to begin working with the Payment Platform.

| **Parameter** | **Description**                                                                                                                                                                 |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CLIENT\_KEY   | Unique key to identify the account in Payment Platform (used as request parameter). In the administration platform this parameter corresponds to the "Merchant key" field       |
| PASSWORD      | Password for Client authentication in Payment Platform (used for calculating hash parameter). In the administration platform this parameter corresponds to the "Password" field |
| PAYMENT\_URL  | URL to request the Payment                                                                                                                                                      |

#### Protocol Mapping

It is necessary to check the existence of the protocol mapping before using the S2S integration.\
Merchants can’t make payments if the S2S APM protocol is not mapped.

#### Payment Platform Interaction

For the transaction you must send the **server to server HTTPS POST** request to the Payment Platform URL (PAYMENT\_URL). In response Payment Platform will return the JSON ([http://json.org/)](http://json.org/)) encoded string.

⚠️ **Pay attention**\


> The S2S (Server-to-Server) protocol requires requests to be sent with the following content type:> \
> Header:\
>> \
> Content-Type: application/x-www-form-urlencoded

#### List of possible actions in Payment Platform

When you make request to Payment Platform, you need to specify action that needs to be done. Possible actions are:

| **Action**         | **Description**                                      |
| ------------------ | ---------------------------------------------------- |
| SALE               | Creates SALE transaction                             |
| CREDITVOID         | Creates REFUND transaction                           |
| VOID               | Creates VOID transaction                             |
| CREDIT2VIRTUAL     | Creates CREDIT2VIRTUAL transaction                   |
| CREDIT2CRYPTO      | Creates CREDIT transaction                           |
| DEBIT2VIRTUAL      | Creates DEBIT transaction as a part of transfer flow |
| GET\_TRANS\_STATUS | Gets status of transaction in Payment Platform       |

#### List of possible transaction results and statuses

**Result** - value that system returns on request. Possible results are:

| **Result** | **Description**                                                                 |
| ---------- | ------------------------------------------------------------------------------- |
| SUCCESS    | Action was successfully completed in Payment Platform                           |
| DECLINED   | Result of unsuccessful action in Payment Platform                               |
| REDIRECT   | Additional action required from requester                                       |
| ACCEPTED   | Action was accepted by Payment Platform, but will be completed later            |
| INIT       | Additional action required from customer, final status will be sent in callback |
| ERROR      | Request has errors and was not validated by Payment Platform                    |

**Status** - actual status of transaction in Payment Platform. Possible statuses are:

| **Status** | **Description**                                                                  |
| ---------- | -------------------------------------------------------------------------------- |
| PREPARE    | Status is undetermined, final status will be sent in callback                    |
| REDIRECT   | The transaction awaits SALE                                                      |
| SETTLED    | Successful transaction                                                           |
| VOID       | Transaction for which void was made                                              |
| REFUND     | Transaction for which refund was made                                            |
| PENDING    | The payment has been initiated and additional actions are required from customer |
| DECLINED   | Not successful transaction                                                       |

#### Callback Events

Merchants receive callbacks when creating transactions with the following statuses:

| **Transaction**         | **Status**                        |
| ----------------------- | --------------------------------- |
| SALE, CREDITVOID, DEBIT | SUCCESS, FAIL, WAITING, UNDEFINED |
| VOID, CREDIT2VIRTUAL    | SUCCESS, FAIL, UNDEFINED          |

## Transactions requests

### SALE request

***

Use SALE request to create a payment in the Payment Platform.

If you want to send a payment for the specific sub-account (channel), you need to use `channel_id` that specified in your Payment Platform account settings.

This request is sent by POST in the background (e.g. through PHP CURL).

#### Request Parameters

| **Parameter**        | **Description**                                                                                                                                                                                                                                                                                                                                                                    | **Limitations**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | **Required** |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------: |
| `action`             | Action to perform (=SALE)                                                                                                                                                                                                                                                                                                                                                          | = SALE                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |       +      |
| `client_key`         | Unique client key (CLIENT\_KEY)                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |       +      |
| `channel_id`         | Payment channel (Sub-account)                                                                                                                                                                                                                                                                                                                                                      | String up to 16 characters                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |       -      |
| `crypto_network`     | Blockchain network                                                                                                                                                                                                                                                                                                                                                                 | <p>String up to 50 characters.<br>You can use an arbitrary value or choose one of the following:<br>• ERC20<br>• TRC20<br>• BEP20<br>• BEP2<br>• OMNI<br>• solana<br>• polygon<br></p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |       -      |
| `brand`              | Brand through which the transaction is performed                                                                                                                                                                                                                                                                                                                                   | String up to 36 characters(Appendix B)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |       +      |
| `order_id`           | Transaction ID in the Clients system                                                                                                                                                                                                                                                                                                                                               | String up to 255 characters                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |       +      |
| `order_amount`       | <p>The amount of the transaction.<br><br><strong>For purchase operation with crypto:</strong><br>Do not send parameter at all if you want to create a transaction with an unknown amount - for cases when the amount is set by the payer outside the merchant site (for example, in an app). As well, you have to omit "amount" parameter for hash calculation for such cases.</p> | <p>Format depends on currency.<br>Send Integer type value for currencies with zero-exponent.<br><strong>Example:</strong> 1000<br>Send Float type value for currencies with exponents 2, 3, 4.<br>Format for 2-exponent currencies: XX.XX<br><strong>Example:</strong> 100.99<br><strong>Pay attention</strong> that currencies 'UGX', 'JPY', 'KRW', 'CLP' must be send in the format XX.XX, with the zeros after comma.<br><strong>Example:</strong> 100.00<br>Format for 3-exponent currencies: XXX.XXX<br><strong>Example:</strong> 100.999.<br>Format for 4-exponent currencies: XXX.XXXX<br><strong>Example:</strong> 100.9999<br><br>⚠️ <strong>Note!</strong> For crypto currencies use the exponent as appropriate for the specific currency.</p> |       +      |
| `order_currency`     | Currency                                                                                                                                                                                                                                                                                                                                                                           | 3 characters for fiat currencies and from 3 to 6 characters for crypto currencies                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |       +      |
| `order_description`  | Description of the transaction (product name)                                                                                                                                                                                                                                                                                                                                      | String up to 1024 characters                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |       +      |
| `identifier`         | <p>Extra parameter for transaction. It could be token, account information, additional descriptor etc.<br>This is optional if crypto currency is used</p>                                                                                                                                                                                                                          | String up to 255 characters                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |       +      |
| `payer_first_name`   | Customer's name                                                                                                                                                                                                                                                                                                                                                                    | String up to 32 characters                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |       -      |
| `payer_last_name`    | Customer's surname                                                                                                                                                                                                                                                                                                                                                                 | String up to 32 characters                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |       -      |
| `payer_address`      | Customer's address                                                                                                                                                                                                                                                                                                                                                                 | String up to 255 characters                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |       -      |
| `payer_house_number` | Customer's house or building number                                                                                                                                                                                                                                                                                                                                                | String up to 9 characters                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |       -      |
| `payer_country`      | Customer's country                                                                                                                                                                                                                                                                                                                                                                 | 2-letter code                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |       -      |
| `payer_state`        | Customer's state                                                                                                                                                                                                                                                                                                                                                                   | String up to 32 characters                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |       -      |
| `payer_city`         | Customer's city                                                                                                                                                                                                                                                                                                                                                                    | String up to 40 characters                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |       -      |
| `payer_district`     | Customer's district of city                                                                                                                                                                                                                                                                                                                                                        | String up to 32 characters                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |       -      |
| `payer_zip`          | ZIP-code of the Customer                                                                                                                                                                                                                                                                                                                                                           | String up to 32 characters                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |       -      |
| `payer_email`        | Customer's email                                                                                                                                                                                                                                                                                                                                                                   | String up to 256 characters                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |       -      |
| `payer_phone`        | Customer's phone                                                                                                                                                                                                                                                                                                                                                                   | String up to 32 characters                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |       -      |
| `payer_birth_date`   | Customer's birthday                                                                                                                                                                                                                                                                                                                                                                | Date format is "YYYY-MM-DD"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |       -      |
| `payer_ip`           | IP-address of the Customer. Both versions, IPv4 and IPv6, can be used. If you are sending IPv6, make sure the payment provider that processes the payments supports it.                                                                                                                                                                                                            | XXX.XXX.XXX.XXX                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |       +      |
| `return_url_target`  | Name of, or keyword for a browsing context where Customer should be returned according to HTML specification.                                                                                                                                                                                                                                                                      | <p>String up to 1024 characters<br><br>Possible values: <code>_blank</code>, <code>_self</code>, <code>_parent</code>, <code>_top</code> or custom iframe name (default <code>_top</code>).<br><br>Find the result of applying the values in the HTML standard description (<a href="https://html.spec.whatwg.org/#valid-browsing-context-name">Browsing context names</a>)</p>                                                                                                                                                                                                                                                                                                                                                                           |       -      |
| `return_url`         | URL to which Customer should be returned after operation in third-party system                                                                                                                                                                                                                                                                                                     | String up to 1024 characters                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |       +      |
| `parameters`         | Array of the parameters for specific brand                                                                                                                                                                                                                                                                                                                                         | (Appendix B)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |       +      |
| `custom_data`        | <p>Array with the custom data<br>This block duplicates the arbitrary parameters that were passed in the payment request</p>                                                                                                                                                                                                                                                        | <p>Format:<br><code>custom_data [param1]: value1</code><br><code>custom_data [param2]: value2</code><br><code>custom_data [paramN]: valueN</code></p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |       -      |
| `hash`               | Special signature to validate your request to Payment Platform                                                                                                                                                                                                                                                                                                                     | See Appendix A, Sale signature.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |       +      |

#### Response Parameters

You will get JSON encoded string with transaction result.

**INIT transaction response (for crypto flow)**

| **Parameter**    | **Description**                                                                                                                                     |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| `action`         | SALE                                                                                                                                                |
| `result`         | SUCCESS                                                                                                                                             |
| `status`         | PENDING                                                                                                                                             |
| `order_id`       | Transaction ID in the Client's system                                                                                                               |
| `trans_id`       | Transaction ID in the Payment Platform                                                                                                              |
| `trans_date`     | Transaction date in the Payment Platform                                                                                                            |
| `descriptor`     | This is a string which the owner of the credit card will see in the statement from the bank. In most cases, this is the Customer's support web-site |
| `amount`         | Order amount                                                                                                                                        |
| `currency`       | Currency                                                                                                                                            |
| `crypto_address` | Public address of a cryptocurrency wallet                                                                                                           |
| `crypto_network` | Blockchain network where the transaction is taking place                                                                                            |

**Successful sale response**

| **Parameter** | **Description**                                                                                                                                    |
| ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| `action`      | SALE                                                                                                                                               |
| `result`      | SUCCESS                                                                                                                                            |
| `status`      | SETTLED                                                                                                                                            |
| `order_id`    | Transaction ID in the Client's system                                                                                                              |
| `trans_id`    | Transaction ID in the Payment Platform                                                                                                             |
| `trans_date`  | Transaction date in the Payment Platform                                                                                                           |
| `descriptor`  | This is a string which the owner of the credit card will see in the statement from the bank. In most cases, this is the Customers support web-site |
| `amount`      | Order amount                                                                                                                                       |
| `currency`    | Currency                                                                                                                                           |

**Unsuccessful sale response**

| **Parameter**    | **Description**                             |
| ---------------- | ------------------------------------------- |
| `action`         | SALE                                        |
| `result`         | DECLINED                                    |
| `status`         | DECLINED                                    |
| `order_id`       | Transaction ID in the Client's system       |
| `trans_id`       | Transaction ID in the Payment Platform      |
| `trans_date`     | Transaction date in the Payment Platform    |
| `decline_reason` | The reason why the transaction was declined |
| `amount`         | Order amount                                |
| `currency`       | Currency                                    |

**Redirect transaction response**

| **Parameter**     | **Description**                                      |
| ----------------- | ---------------------------------------------------- |
| `action`          | SALE                                                 |
| `result`          | REDIRECT                                             |
| `status`          | REDIRECT                                             |
| `order_id`        | Transaction ID in the Client's system                |
| `trans_id`        | Transaction ID in the Payment Platform               |
| `trans_date`      | Transaction date in the Payment Platform             |
| `redirect_url`    | URL to which the Client should redirect the Customer |
| `redirect_params` | Array of specific parameters                         |
| `redirect_method` | The method of transferring parameters (POST or GET)  |
| `amount`          | Order                                                |
| `currency`        | Currency                                             |

**Undefined sale response**

| **Parameter** | **Description**                                                                                                                                    |
| ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| `action`      | SALE                                                                                                                                               |
| `result`      | UNDEFINED                                                                                                                                          |
| `status`      | REDIRECT / PREPARE                                                                                                                                 |
| `order_id`    | Transaction ID in the Merchant's system                                                                                                            |
| `trans_id`    | Transaction ID in the Payment Platform                                                                                                             |
| `trans_date`  | Transaction date in the Payment Platform                                                                                                           |
| `descriptor`  | This is a string which the owner of the credit card will see in the statement from the bank. In most cases, this is the Customers support web-site |
| `amount`      | Order amount                                                                                                                                       |
| `currency`    | Currency                                                                                                                                           |

#### Callback parameters

**INIT transaction response (for crypto flow)**

| Parameter    | Description                                                                                                                                         |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| `action`     | SALE                                                                                                                                                |
| `result`     | INIT                                                                                                                                                |
| `status`     | PENDING                                                                                                                                             |
| `order_id`   | Order ID in the Client's system                                                                                                                     |
| `trans_id`   | Transaction ID in the Payment Platform                                                                                                              |
| `trans_date` | Transaction date in the Payment Platform                                                                                                            |
| `descriptor` | This is a string which the owner of the credit card will see in the statement from the bank. In most cases, this is the Customer's support web-site |
| `amount`     | Order amount                                                                                                                                        |
| `currency`   | Currency                                                                                                                                            |

**Successful sale response**

| **Parameter**                 | **Description**                                                                                                                                   |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| `action`                      | SALE                                                                                                                                              |
| `result`                      | SUCCESS                                                                                                                                           |
| `status`                      | SETTLED                                                                                                                                           |
| `order_id`                    | Transaction ID in the Client's system                                                                                                             |
| `trans_id`                    | Transaction ID in the Client's system                                                                                                             |
| `trans_date`                  | Transaction date in the Payment Platform                                                                                                          |
| `descriptor`                  | This is a string which the owner of the credit card will see in the statement from the bank.In most cases, this is the Customers support web-site |
| `exchange_rate`               | Rate used to make exchange                                                                                                                        |
| `exchange_rate_base`          | The rate used in the double conversion to convert the original currency to the base currency                                                      |
| `exchange_currency`           | Original currency                                                                                                                                 |
| `exchange_amount`             | Original amount                                                                                                                                   |
| `hash`                        | Special signature to validate callback. See Appendix A, Callback signature.                                                                       |
| `amount`                      | Order amount                                                                                                                                      |
| `currency`                    | Currency                                                                                                                                          |
| `custom_data`                 | <p>Object with the custom data.<br>This block duplicates the arbitrary parameters that were passed in the payment request</p>                     |
| `connector_name` \*\* \* \*\* | Connector's name (Payment Gateway)                                                                                                                |
| `rrn` \*\* \* \*\*            | Retrieval Reference Number value from the acquirer system                                                                                         |
| `approval_code` \*\* \* \*\*  | Approval code value from the acquirer system                                                                                                      |
| `gateway_id` **\***           | Gateway ID – transaction identifier provided by payment gateway                                                                                   |
| `extra_gateway_id`**\***      | Extra Gateway ID – additional transaction identifier provided by payment gateway.                                                                 |
| `merchant_name`\*\* \* \*\*   | Merchant Name                                                                                                                                     |
| `mid_name` \*\* \* \*\*       | MID Name                                                                                                                                          |
| `issuer_country` \*\* \* \*\* | Issuer Country                                                                                                                                    |
| `issuer_bank` \*\* \* \*\*    | Issuer Bank                                                                                                                                       |

\*\* \* \*\* The parameters are included if the appropriate setup is configured in the admin panel (see “Add Extended Data to Callback” block in the Configurations -> Protocol Mappings section).

**Unsuccessful sale response**

| **Parameter**    | **Description**                                                                                                               |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| `action`         | SALE                                                                                                                          |
| `result`         | DECLINED                                                                                                                      |
| `status`         | DECLINED                                                                                                                      |
| `order_id`       | Transaction ID in the Client's system                                                                                         |
| `trans_id`       | Transaction ID in the Client's system                                                                                         |
| `trans_date`     | Transaction date in the Payment Platform                                                                                      |
| `descriptor`     | Transaction date in the Payment Platform                                                                                      |
| `decline_reason` | Description of the cancellation of the transaction                                                                            |
| `hash`           | Special signature to validate callback. See Appendix A, Callback signature.                                                   |
| `amount`         | Order amount                                                                                                                  |
| `currency`       | Currency                                                                                                                      |
| `custom_data`    | <p>Object with the custom data.<br>This block duplicates the arbitrary parameters that were passed in the payment request</p> |

**Redirect transaction response**

| **Parameter**     | **Description**                                                                                                               |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| `action`          | SALE                                                                                                                          |
| `result`          | REDIRECT                                                                                                                      |
| `status`          | REDIRECT                                                                                                                      |
| `order_id`        | Transaction ID in the Client's system                                                                                         |
| `trans_id`        | Transaction ID in the Client's system                                                                                         |
| `trans_date`      | Transaction date in the Payment Platform                                                                                      |
| `redirect_url`    | URL to which the Client should redirect the Customer                                                                          |
| `redirect_params` | Array parameters                                                                                                              |
| `redirect_method` | The method of transferring parameters (POST or GET)                                                                           |
| `hash`            | Special signature to validate callback. See Appendix A, Callback signature.                                                   |
| `amount`          | Order amount                                                                                                                  |
| `currency`        | Currency                                                                                                                      |
| `custom_data`     | <p>Object with the custom data.<br>This block duplicates the arbitrary parameters that were passed in the payment request</p> |

**Undefined sale response**

| **Parameter** | **Description**                                                                                                               |
| ------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| `action`      | SALE                                                                                                                          |
| `result`      | UNDEFINED                                                                                                                     |
| `status`      | REDIRECT / PREPARE                                                                                                            |
| `order_id`    | Transaction ID in the Merchant's system system                                                                                |
| `trans_id`    | Transaction ID in the Payment Platform                                                                                        |
| `trans_date`  | Transaction date in the Payment Platform                                                                                      |
| `descriptor`  | TDescriptor from the bank, the same as cardholder will see in the bank statement                                              |
| `amount`      | Order amount                                                                                                                  |
| `currency`    | Order currency                                                                                                                |
| `custom_data` | <p>Object with the custom data.<br>This block duplicates the arbitrary parameters that were passed in the payment request</p> |
| `hash`        | Special signature, used to validate callback, see Appendix A, Formula 2                                                       |

### CREDIT2VIRTUAL request

***

CREDIT2VIRTUAL request is used to create CREDIT2VIRTUAL transaction in Payment Platform.

This operation transfers money from merchants account to customer account.

If you want to send a payout for the specific sub-account (channel), you need to use `channel_id` that specified in your Payment Platform account settings.

This request is sent by POST in the background (e.g., through PHP CURL).

#### Request Parameters

| **Parameter**       | **Description**                                                | **Values**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | **Required** |
| ------------------- | -------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------: |
| `action`            | CREDIT2VIRTUAL                                                 | CREDIT2VIRTUAL                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |       +      |
| `client_key`        | Unique client key (CLIENT\_KEY)                                |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |       +      |
| `channel_id`        | Payment channel (Sub-account)                                  | String up to 16 characters                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |       -      |
| `brand`             | Brand through which the transaction is performed               | String up to 36 characters                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |       +      |
| `order_id`          | Order ID in the Clients system                                 | String up to 255 characters                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |       +      |
| `order_amount`      | The amount of the transaction                                  | <p>Format depends on currency.<br>Send Integer type value for currencies with zero-exponent.<br><strong>Example:</strong> 1000<br>Send Float type value for currencies with exponents 2, 3, 4.<br>Format for 2-exponent currencies: XX.XX<br><strong>Example:</strong> 100.99<br><strong>Pay attention</strong> that currencies 'UGX', 'JPY', 'KRW', 'CLP' must be send in the format XX.XX, with the zeros after comma.<br><strong>Example:</strong> 100.00<br>Format for 3-exponent currencies: XXX.XXX<br><strong>Example:</strong> 100.999.<br>Format for 4-exponent currencies: XXX.XXXX<br><strong>Example:</strong> 100.9999<br></p> |       +      |
| `order_currency`    | Currency                                                       | 3-letter code                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |       +      |
| `order_description` | Description of the transaction (product name)                  | String up to 1024 characters                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |       +      |
| `parameters`        | Array of the parameters for specific brand                     | (Appendix C)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |       +      |
| `hash`              | Special signature to validate your request to Payment Platform | See Appendix A, Credit2Virtual signature.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |       +      |

#### Response Parameters

You will get JSON encoded string with transaction result.

**Successful response**

| **Parameter** | **Description**                          |
| ------------- | ---------------------------------------- |
| `action`      | CREDIT2VIRTUAL                           |
| `result`      | SUCCESS                                  |
| `status`      | SETTLED                                  |
| `order_id`    | Order ID in the Client’s system          |
| `trans_id`    | Transaction ID in the Payment Platform   |
| `trans_date`  | Transaction date in the Payment Platform |
| `amount`      | Order amount                             |
| `currency`    | Currency                                 |

<details>

<summary>Response Example (Successful result)</summary>

```
{
"action": "CREDIT2VIRTUAL",
"result": "SUCCESS",
"status": "SETTLED",
"order_id": "1613117050",
"trans_id": "e5098d62-6d08-11eb-9da3-0242ac120013",
"trans_date": "2021-02-12 08:04:15",
"amount": "0.19",
"currency": "USD"
}
```

</details>

**Unsuccessful response**

| **Parameter**    | **Description**                             |
| ---------------- | ------------------------------------------- |
| `action`         | CREDIT2VIRTUAL                              |
| `result`         | DECLINED                                    |
| `status`         | DECLINED                                    |
| `order_id`       | Order ID in the Client’s system             |
| `trans_id`       | Transaction ID in the Payment Platform      |
| `trans_date`     | Transaction date in the Payment Platform    |
| `decline_reason` | The reason why the transaction was declined |
| `amount`         | Order amount                                |
| `currency`       | Currency                                    |

<details>

<summary>Response Example (Unsuccessful result)</summary>

```
{
"action": "CREDIT2VIRTUAL",
"result": "DECLINED",
"status": "DECLINED",
"order_id": "1613117050",
"trans_id": "e5098d62-6d08-11eb-9da3-0242ac120013",
"trans_date": "2021-02-12 08:04:15",
"decline_reason": "Declined by procesing",
"amount": "0.19",
"currency": "USD"
}
```

</details>

**Undefined response**

| **Parameter** | **Description**                          |
| ------------- | ---------------------------------------- |
| `action`      | CREDIT2VIRTUAL                           |
| `result`      | UNDEFINED                                |
| `status`      | PREPARE                                  |
| `order_id`    | Order ID in the Client’s system          |
| `trans_id`    | Transaction ID in the Payment Platform   |
| `trans_date`  | Transaction date in the Payment Platform |
| `amount`      | Order amount                             |
| `currency`    | Currency                                 |

<details>

<summary>Response Example (Undefined result)</summary>

```
{
"action": "CREDIT2VIRTUAL",
"result": "UNDEFINED",
"status": "PREPARE",
"order_id": "1613117050",
"trans_id": "e5098d62-6d08-11eb-9da3-0242ac120013",
"trans_date": "2021-02-12 08:04:15",
"amount": "0.19",
"currency": "USD"
}
```

</details>

#### Callback parameters

**Successful response**

| **Parameter**                 | **Description**                                                                           |
| ----------------------------- | ----------------------------------------------------------------------------------------- |
| `action`                      | CREDIT2VIRTUAL                                                                            |
| `result`                      | SUCCESS                                                                                   |
| `status`                      | SETTLED                                                                                   |
| `order_id`                    | Order ID in the Client’s system                                                           |
| `trans_id`                    | Transaction ID in the Payment Platform                                                    |
| `trans_date`                  | Transaction date in the Payment Platform                                                  |
| `amount`                      | Order amount                                                                              |
| `currency`                    | Currency                                                                                  |
| `connector_name` \*\* \* \*\* | Connector's name (Payment Gateway)                                                        |
| `rrn` \*\* \* \*\*            | Retrieval Reference Number value from the acquirer system                                 |
| `approval_code` \*\* \* \*\*  | Approval code value from the acquirer system                                              |
| `gateway_id` **\***           | Gateway ID – transaction identifier provided by payment gateway                           |
| `extra_gateway_id`**\***      | Extra Gateway ID – additional transaction identifier provided by payment gateway.         |
| `merchant_name`\*\* \* \*\*   | Merchant Name                                                                             |
| `mid_name` \*\* \* \*\*       | MID Name                                                                                  |
| `issuer_country` \*\* \* \*\* | Issuer Country                                                                            |
| `issuer_bank` \*\* \* \*\*    | Issuer Bank                                                                               |
| `hash`                        | Special signature to validate callback. See Appendix A, Credit2Virtual callback signature |

\*\* \* \*\* The parameters are included if the appropriate setup is configured in the admin panel (see “Add Extended Data to Callback” block in the Configurations -> Protocol Mappings section).

<details>

<summary>Response Example (Successful result)</summary>

```
action=CREDIT2VIRTUAL&result=SUCCESS&status=SETTLED&order_id=64401210&trans_id=2732359c-3b21-11ef-8a36-ca7f64825086&hash=73a796d264052625286faf12245b8489&trans_date=2024-07-05+22%3A51%3A46&amount=0.19&currency=USD

```

</details>

**Unsuccessful response**

| **Parameter**    | **Description**                                                                           |
| ---------------- | ----------------------------------------------------------------------------------------- |
| `action`         | CREDIT2VIRTUAL                                                                            |
| `result`         | DECLINED                                                                                  |
| `status`         | DECLINED                                                                                  |
| `order_id`       | Order ID in the Client’s system                                                           |
| `trans_id`       | Transaction ID in the Payment Platform                                                    |
| `trans_date`     | Transaction ID in the Payment Platform                                                    |
| `decline_reason` | Description of the cancellation of the transaction                                        |
| `hash`           | Special signature to validate callback. See Appendix A, Credit2Virtual callback signature |
| `amount`         | Order amount                                                                              |
| `currency`       | Currency                                                                                  |

<details>

<summary>Response Example (Unsuccessful result)</summary>

```
action=CREDIT2VIRTUAL&result=DECLINED&status=DECLINED&order_id=64401210&trans_id=2732359c-3b21-11ef-8a36-ca7f64825086&hash=73a796d264052625286faf12245b8489&trans_date=2024-07-05+22%3A51%3A46&amount=0.19&currency=USD&decline_reason=reason
```

</details>

**Undefined response**

| **Parameter** | **Description**                                                   |
| ------------- | ----------------------------------------------------------------- |
| `action`      | CREDIT2VIRTUAL                                                    |
| `result`      | UNDEFINED                                                         |
| `status`      | PREPARE                                                           |
| `order_id`    | Order ID in the Client’s system                                   |
| `trans_id`    | Transaction ID in the Payment Platform                            |
| `trans_date`  | Date of CREDIT2CARD action                                        |
| `hash`        | Special signature to validate callback. See Appendix A, Formula 6 |
| `amount`      | Order amount                                                      |
| `currency`    | Currency                                                          |

<details>

<summary>Response Example (Undefined result)</summary>

```
action=CREDIT2VIRTUAL&result=UNDEFINED&status=PREPARE&order_id=64401210&trans_id=2732359c-3b21-11ef-8a36-ca7f64825086&hash=73a796d264052625286faf12245b8489&trans_date=2024-07-05+22%3A51%3A46&amount=0.19&currency=USD
```

</details>

### CREDIT2CRYPTO request

***

CREDIT2CRYPTO request is used to create CREDIT transaction in Payment Platform.

This operation transfers funds from merchant\`s crypto wallet to customer.

This request is sent by POST in the background (e.g., through PHP CURL).

#### Request parameters

| **Parameter**       | **Description**                                                | **Values**                                                                                                                                                                            | **Required** |
| ------------------- | -------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------: |
| `action`            | CREDIT2CRYPTO                                                  | CREDIT2CRYPTO                                                                                                                                                                         |       +      |
| `client_key`        | Unique client key (CLIENT\_KEY)                                |                                                                                                                                                                                       |       +      |
| `channel_id`        | Payment channel (Sub-account)                                  | String up to 16 characters                                                                                                                                                            |       -      |
| `brand`             | Brand through which the transaction is performed               | String up to 36 characters                                                                                                                                                            |       +      |
| `order_id`          | Order ID in the Clients system                                 | String up to 255 characters                                                                                                                                                           |       +      |
| `order_amount`      | The amount of the transaction                                  | Format depends on currency                                                                                                                                                            |       +      |
| `order_currency`    | Crypto currency                                                | alpha code, from 3 to 6 characters                                                                                                                                                    |       +      |
| `order_description` | Description of the transaction (product name)                  | String up to 1024 characters                                                                                                                                                          |       +      |
| `crypto_network`    | Blockchain network                                             | <p>String up to 50 characters<br><br>You can use an arbitrary value or choose one of the following:<br>• ERC20<br>• TRC20<br>• BEP20<br>• BEP2<br>• OMNI<br>• solana<br>• polygon</p> |       -      |
| `parameters`        | Array of the parameters for specific brand                     | (Appendix C)                                                                                                                                                                          |       -      |
| `hash`              | Special signature to validate your request to Payment Platform | See Appendix A, Credit2Virtual signature.                                                                                                                                             |       +      |

#### Response Parameters

You will get JSON encoded string with transaction result.

**Synchronous - INIT transaction response (for crypto flow)**

| **Parameter** | **Description**                                                              |
| ------------- | ---------------------------------------------------------------------------- |
| `action`      | CREDIT2CRYPTO                                                                |
| `result`      | INIT / UNDEFINED / DECLINED                                                  |
| `status`      | <p>PENDING if success<br>PREPARE if undefined status<br>DECLINED if fail</p> |
| `order_id`    | Order ID in the Client’s system                                              |
| `trans_id`    | Transaction ID in the Payment Platform                                       |
| `trans_date`  | Transaction date in the Payment Platform                                     |
| `descriptor`  | Additional payment description                                               |
| `amount`      | Order amount                                                                 |
| `currency`    | Currency                                                                     |

#### Callback Parameters

**Callback - INIT transaction response**

| **Parameter** | **Description**                               |
| ------------- | --------------------------------------------- |
| `action`      | CREDIT2CRYPTO                                 |
| `result`      | INIT                                          |
| `status`      | <p>PENDING if success<br>DECLINED if fail</p> |
| `order_id`    | Order ID in the Client’s system               |
| `trans_id`    | Transaction ID in the Payment Platform        |
| `trans_date`  | Transaction ID in the Client's system         |
| `descriptor`  | Additional payment description                |
| `amount`      | Order amount                                  |
| `currency`    | Currency                                      |

**Callback - Successful response**

| **Parameter** | **Description**                          |
| ------------- | ---------------------------------------- |
| `action`      | CREDIT2CRYPTO                            |
| `result`      | SUCCESS                                  |
| `status`      | SETTLED                                  |
| `order_id`    | Order ID in the Client’s system          |
| `trans_id`    | Transaction ID in the Payment Platform   |
| `amount`      | Order amount                             |
| `currency`    | Currency                                 |
| `trans_date`  | Transaction date in the Payment Platform |

**Callback - Unsuccessful response**

| **Parameter**    | **Description**                             |
| ---------------- | ------------------------------------------- |
| `action`         | CREDIT2CRYPTO                               |
| `result`         | DECLINED                                    |
| `status`         | DECLINED                                    |
| `order_id`       | Order ID in the Client’s system             |
| `trans_id`       | Transaction ID in the Payment Platform      |
| `amount`         | Order amount                                |
| `currency`       | Currency                                    |
| `trans_date`     | Transaction date in the Payment Platform    |
| `decline_reason` | The reason why the transaction was declined |

**Callback - Undefined response**

| **Parameter** | **Description**                          |
| ------------- | ---------------------------------------- |
| `action`      | CREDIT2CRYPTO                            |
| `result`      | UNDEFINED                                |
| `status`      | PREPARE / PENDING                        |
| `order_id`    | Order ID in the Client’s system          |
| `trans_id`    | Transaction ID in the Payment Platform   |
| `trans_date`  | Transaction date in the Payment Platform |
| `amount`      | Order amount                             |
| `currency`    | Currency                                 |

### CREDITVOID request

***

CREDITVOID request is used to complete REFUND transactions.

REFUND transaction is used to return funds to account, previously submitted by SALE transactions.

This request is sent by POST in the background (e.g. through PHP CURL).

#### Request parameters

| **Parameter** | **Description**                                                 | **Limitations**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | **Required** |
| ------------- | --------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------: |
| `action`      | Action to perform                                               | = CREDITVOID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |       +      |
| `client_key`  | Unique client key                                               | CLIENT\_KEY                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |              |
| `trans_id`    | Transaction ID in the Payment Platform                          | String up to 255 characters                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |       +      |
| `amount`      | The amount for partial refund. Several partial refunds allowed. | <p>Format depends on currency.<br>Send Integer type value for currencies with zero-exponent.<br><strong>Example:</strong> 1000<br>Send Float type value for currencies with exponents 2, 3, 4.<br>Format for 2-exponent currencies: XX.XX<br><strong>Example:</strong> 100.99<br><strong>Pay attention</strong> that currencies 'UGX', 'JPY', 'KRW', 'CLP' must be send in the format XX.XX, with the zeros after comma.<br><strong>Example:</strong> 100.00<br>Format for 3-exponent currencies: XXX.XXX<br><strong>Example:</strong> 100.999.<br>Format for 4-exponent currencies: XXX.XXXX<br><strong>Example:</strong> 100.9999<br></p> |       -      |
| `hash`        | Special signature to validate your request to Payment Platform  | See Appendix A, Creditvoid signature.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |       +      |

#### Response parameters

| **Parameter** | **Description**                         |
| ------------- | --------------------------------------- |
| `action`      | CREDITVOID                              |
| `result`      | ACCEPTED                                |
| `order_id`    | Transaction ID in the Merchant's system |
| `trans_id`    | Transaction ID in the Payment Platform  |

#### Callback parameters

**Successful refund response**

| **Parameter**                 | **Description**                                                                     |
| ----------------------------- | ----------------------------------------------------------------------------------- |
| `action`                      | CREDITVOID                                                                          |
| `result`                      | SUCCESS                                                                             |
| `status`                      | REFUND (full refund) / SETTLED (partial refund)                                     |
| `order_id`                    | Transaction ID in the Merchant's system                                             |
| `trans_id`                    | Transaction ID in the Payment Platform                                              |
| `creditvoid_date`             | Date of the refund/reversal                                                         |
| `amount`                      | Amount of refund                                                                    |
| `connector_name` \*\* \* \*\* | Connector's name (Payment Gateway)                                                  |
| `rrn` \*\* \* \*\*            | Retrieval Reference Number value from the acquirer system                           |
| `approval_code` \*\* \* \*\*  | Approval code value from the acquirer system                                        |
| `gateway_id` **\***           | Gateway ID – transaction identifier provided by payment gateway                     |
| `extra_gateway_id`**\***      | Extra Gateway ID – additional transaction identifier provided by payment gateway.   |
| `merchant_name`\*\* \* \*\*   | Merchant Name                                                                       |
| `mid_name` \*\* \* \*\*       | MID Name                                                                            |
| `issuer_country` \*\* \* \*\* | Issuer Country                                                                      |
| `issuer_bank` \*\* \* \*\*    | Issuer Bank                                                                         |
| `hash`                        | Special signature, used to validate callback. See Appendix A, Creditvoid signature. |

\*\* \* \*\* The parameters are included if the appropriate setup is configured in the admin panel (see “Add Extended Data to Callback” block in the Configurations -> Protocol Mappings section).

**Unsuccessful refund response**

| **Parameter**    | **Description**                                                                     |
| ---------------- | ----------------------------------------------------------------------------------- |
| `action`         | CREDITVOID                                                                          |
| `result`         | DECLINED                                                                            |
| `status`         | SETTLED                                                                             |
| `order_id`       | Transaction ID in the Merchant's system                                             |
| `trans_id`       | Transaction ID in the Payment Platform                                              |
| `decline_reason` | Description of the cancellation of the transaction                                  |
| `hash`           | Special signature, used to validate callback. See Appendix A, Creditvoid signature. |

**Undefined refund response**

| **Parameter**     | **Description**                                                         |
| ----------------- | ----------------------------------------------------------------------- |
| `action`          | CREDITVOID                                                              |
| `result`          | UNDEFINED                                                               |
| `status`          | SETTLED                                                                 |
| `order_id`        | Transaction ID in the Transaction ID in the Merchant's system           |
| `trans_id`        | Transaction ID in the Payment Platform                                  |
| `creditvoid_date` | Transaction date in the Payment Platform                                |
| `amount`          | Amount of refund                                                        |
| `hash`            | Special signature, used to validate callback, see Appendix A, Formula 2 |

### VOID request

***

The VOID request is used to cancel the operation which was performed the same financial day.

The cancellation is possible for the SALE operation.

The VOID request is allowed for the payments in SETTLED status only.

This request is sent by POST in the background (e.g. through PHP CURL).

#### Request parameters

| **Parameter** | **Description**                                                | **Limitations**                 | **Required** |
| ------------- | -------------------------------------------------------------- | ------------------------------- | :----------: |
| `action`      | Action to perform                                              | = VOID                          |       +      |
| `client_key`  | Unique client key                                              | CLIENT\_KEY                     |       +      |
| `trans_id`    | Transaction ID in the Payment Platform                         | String up to 255 characters     |       +      |
| `hash`        | Special signature to validate your request to Payment Platform | See Appendix A, Void signature. |       +      |

#### Response parameters

You will get JSON encoded string with transaction result.

**Successful void response**

| **Parameter** | **Description**                          |
| ------------- | ---------------------------------------- |
| `action`      | VOID                                     |
| `result`      | SUCCESS                                  |
| `status`      | VOID                                     |
| `order_id`    | Transaction ID in the Merchant's system  |
| `trans_id`    | Transaction ID in the Payment Platform   |
| `trans_date`  | Transaction date in the Payment Platform |

**Unsuccessful void response**

| **Parameter**    | **Description**                             |
| ---------------- | ------------------------------------------- |
| `action`         | VOID                                        |
| `result`         | DECLINED                                    |
| `status`         | SETTLED                                     |
| `order_id`       | Transaction ID in the Merchant's system     |
| `trans_id`       | Transaction ID in the Payment Platform      |
| `trans_date`     | Transaction date in the Payment Platform    |
| `decline_reason` | The reason why the transaction was declined |

**Undefined void response**

| **Parameter** | **Description**                          |
| ------------- | ---------------------------------------- |
| `action`      | VOID                                     |
| `result`      | UNDEFINED                                |
| `status`      | SETTLED                                  |
| `order_id`    | Transaction ID in the Merchant's system  |
| `trans_id`    | Transaction ID in the Payment Platform   |
| `trans_date`  | Transaction date in the Payment Platform |

#### Callback parameters

**Successful void response**

| **Parameter**                 | **Description**                                                                   |
| ----------------------------- | --------------------------------------------------------------------------------- |
| `action`                      | VOID                                                                              |
| `result`                      | SUCCESS                                                                           |
| `status`                      | SETTLED                                                                           |
| `order_id`                    | Transaction ID in the Merchant's system                                           |
| `trans_id`                    | Transaction ID in the Payment Platform                                            |
| `trans_date`                  | Transaction date in the Payment Platform                                          |
| `connector_name` \*\* \* \*\* | Connector's name (Payment Gateway)                                                |
| `rrn` \*\* \* \*\*            | Retrieval Reference Number value from the acquirer system                         |
| `approval_code` \*\* \* \*\*  | Approval code value from the acquirer system                                      |
| `gateway_id` **\***           | Gateway ID – transaction identifier provided by payment gateway                   |
| `extra_gateway_id`**\***      | Extra Gateway ID – additional transaction identifier provided by payment gateway. |
| `merchant_name`\*\* \* \*\*   | Merchant Name                                                                     |
| `mid_name` \*\* \* \*\*       | MID Name                                                                          |
| `issuer_country` \*\* \* \*\* | Issuer Country                                                                    |
| `issuer_bank` \*\* \* \*\*    | Issuer Bank                                                                       |
| `hash`                        | Special signature, used to validate callback, see Appendix A, Formula 2           |

\*\* \* \*\* The parameters are included if the appropriate setup is configured in the admin panel (see “Add Extended Data to Callback” block in the Configurations -> Protocol Mappings section).

**Unsuccessful void response**

| **Parameter**    | **Description**                                                         |
| ---------------- | ----------------------------------------------------------------------- |
| `action`         | VOID                                                                    |
| `result`         | DECLINED                                                                |
| `status`         | SETTLED                                                                 |
| `order_id`       | Transaction ID in the Merchant's system                                 |
| `trans_id`       | Transaction ID in the Payment Platform                                  |
| `trans_date`     | Transaction date in the Payment Platform                                |
| `decline_reason` | The reason why the transaction was declined                             |
| `hash`           | Special signature, used to validate callback, see Appendix A, Formula 2 |

**Undefined void response**

| **Parameter** | **Description**                                                         |
| ------------- | ----------------------------------------------------------------------- |
| `action`      | VOID                                                                    |
| `result`      | UNDEFINED                                                               |
| `status`      | SETTLED                                                                 |
| `order_id`    | Transaction ID in the Merchant's system                                 |
| `trans_id`    | Transaction ID in the Payment Platform                                  |
| `trans_date`  | Transaction date in the Payment Platform                                |
| `hash`        | Special signature, used to validate callback, see Appendix A, Formula 2 |

### DEBIT request

Use debit actions to create debit transactions.\
When conducting a debit transaction, merchants can utilize commissions for payments by configuring the appropriate settings in the admin panel.

Debit transactions can be executed in two ways:

**One-step debit**

This option enables payment processing with a single request - DEBIT2VIRTUAL.\
If commissions are set for debits, they will be applied immediately upon payment. The commission amount can be viewed in the callback (`commission` parameter).

**Two-steps debit**

This option involves initiating the debit using the DEBIT2VIRTUAL\_CALC request first, followed by confirming the operation through the DEBIT2VIRTUAL\_COMPLETE request.\
In this scenario, if commissions are configured for debits, the commission amount will be returned in the response (`commission` parameter) after executing the DEBIT2VIRTUAL\_CALC request.

#### DEBIT2VIRTUAL request parameters

Use DEBIT2VIRTUAL action to create debit transaction in one step.

| **Parameter**       | **Description**                                                                                               | **Values**                         | **Required** |
| ------------------- | ------------------------------------------------------------------------------------------------------------- | ---------------------------------- | ------------ |
| `action`            | Action that you want to perform. Fixed value.                                                                 | DEBIT2VIRTUAL                      | +            |
| `client_key`        | Unique key (CLIENT\_KEY)                                                                                      | UUID format value                  | +            |
| `order_id`          | Transaction ID in the Merchants system                                                                        | String up to 255 characters        | +            |
| `order_amount`      | The amount of the transaction                                                                                 | Numbers in the form XXXX.XX        | +            |
| `order_currency`    | Currency                                                                                                      | 3-letter code                      | +            |
| `order_description` | Description of the transaction (product name)                                                                 | String up to 1024 characters       | +            |
| `identifier`        | <p>Extra parameter for transaction.<br>It could be token, account information, additional descriptor etc.</p> | String up to 255 characters        | +            |
| `brand`             | Brand through which the transaction is performed                                                              | String up to 36 characters         | +            |
| `payer_first_name`  | Customer’s name                                                                                               | String up to 32 characters         | -            |
| `payer_last_name`   | Customer’s surname                                                                                            | String up to 32 characters         | -            |
| `payer_middle_name` | Customer’s middle name                                                                                        | String up to 32 characters         | -            |
| `payer_birth_date`  | Customer’s birthday                                                                                           | format yyyy-MM-dd, e.g. 1970-02-17 | -            |
| `payer_address`     | Customer’s address                                                                                            | String up to 255 characters        | -            |
| `payer_address2`    | The adjoining road or locality (if required) of the customer’s address                                        | String up to 255 characters        | -            |
| `payer_country`     | Customer’s country                                                                                            | 2-letter code                      | -            |
| `payer_state`       | Customer’s state                                                                                              | String up to 32 characters         | -            |
| `payer_city`        | Customer’s city                                                                                               | String up to 32 characters         | -            |
| `payer_zip`         | ZIP-code of the Customer                                                                                      | String up to 10 characters         | -            |
| `payer_email`       | Customer’s email                                                                                              | String up to 256 characters        | -            |
| `payer_phone`       | Customer’s phone                                                                                              | String up to 32 characters         | -            |
| `payer_ip`          | IP-address of the Customer                                                                                    | XXX.XXX.XXX.XXX                    | +            |
| `payer_return_url`  | Customer return URL                                                                                           | String up to 256 characters        | +            |
| `hash`              | Special signature to validate your request to Payment Platform                                                | See Appendix A, Debit signature    | +            |

#### Response parameters

You will get JSON encoded string with transaction result. If your account supports 3D-Secure, transaction result will be sent to your Notification URL.

**Successful response**

| Parameter      | Description                                                                |
| -------------- | -------------------------------------------------------------------------- |
| `action`       | DEVIT2VIRTUAL                                                              |
| `result`       | SUCCESS                                                                    |
| `status`       | 3DS / REDIRECT / SETTLED                                                   |
| `order_id`     | Transaction ID in the Merchant’s system                                    |
| `trans_id`     | Transaction ID in the Payment Platform                                     |
| `trans_date`   | Transaction date in the Payment Platform                                   |
| `descriptor`   | Descriptor from the bank, the same as payer will see in the bank statement |
| `amount`       | Order amount                                                               |
| `currency`     | Currency                                                                   |
| `commission`   | Commission for transaction                                                 |
| `total_amount` | Total amount for transaction: total\_amount = amount + commission          |

**Unsuccessful response**

| Parameter        | Description                                                                |
| ---------------- | -------------------------------------------------------------------------- |
| `action`         | DEBIT2VIRTUAL                                                              |
| `result`         | DECLINED                                                                   |
| `status`         | DECLINED                                                                   |
| `order_id`       | Transaction ID in the Merchant’s system                                    |
| `trans_id`       | Transaction ID in the Payment Platform                                     |
| `trans_date`     | Transaction date in the Payment Platform                                   |
| `descriptor`     | Descriptor from the bank, the same as payer will see in the bank statement |
| `amount`         | Order amount                                                               |
| `currency`       | Currency                                                                   |
| `commission`     | Commission for transaction                                                 |
| `total_amount`   | Total amount for transaction                                               |
| `decline_reason` | The reason why the transaction was declined                                |

**Undefined response**

| Parameter      | Description                                                                |
| -------------- | -------------------------------------------------------------------------- |
| `action`       | DEVIT2VIRTUAL                                                              |
| `result`       | UNDEFINED                                                                  |
| `status`       | REDIRECT / PREPARE                                                         |
| `order_id`     | Transaction ID in the Merchant’s system                                    |
| `trans_id`     | Transaction ID in the Payment Platform                                     |
| `trans_date`   | Transaction date in the Payment Platform                                   |
| `descriptor`   | Descriptor from the bank, the same as payer will see in the bank statement |
| `amount`       | Order amount                                                               |
| `currency`     | Order currency                                                             |
| `commission`   | Commission for transaction                                                 |
| `total_amount` | Total amount for transaction: total\_amount = amount + commission          |

#### Callback parameters

**Successful response**

| Parameter                     | Description                                                                       |
| ----------------------------- | --------------------------------------------------------------------------------- |
| `action`                      | DEBIT2VIRTUAL                                                                     |
| `result`                      | SUCCESS                                                                           |
| `status`                      | REDRIECT / 3DS / SETTLED                                                          |
| `order_id`                    | Transaction ID in the Merchant’s system                                           |
| `trans_id`                    | Transaction ID in the Payment Platform                                            |
| `trans_date`                  | Transaction date in the Payment Platform                                          |
| `descriptor`                  | Descriptor from the bank, the same as payer will see in the bank statement        |
| `amount`                      | Order amount                                                                      |
| `currency`                    | Currency                                                                          |
| `commission`                  | Commission for transaction                                                        |
| `total_amount`                | Total amount for transaction: total\_amount = amount + commission                 |
| `connector_name` \*\* \* \*\* | Connector's name (Payment Gateway)                                                |
| `rrn` \*\* \* \*\*            | Retrieval Reference Number value from the acquirer system                         |
| `approval_code` \*\* \* \*\*  | Approval code value from the acquirer system                                      |
| `gateway_id` **\***           | Gateway ID – transaction identifier provided by payment gateway                   |
| `extra_gateway_id`**\***      | Extra Gateway ID – additional transaction identifier provided by payment gateway. |
| `merchant_name`\*\* \* \*\*   | Merchant Name                                                                     |
| `mid_name` \*\* \* \*\*       | MID Name                                                                          |
| `issuer_country` \*\* \* \*\* | Issuer Country                                                                    |
| `issuer_bank` \*\* \* \*\*    | Issuer Bank                                                                       |
| `hash`                        | Special signature, used to validate callback, see Appendix A, Formula 2           |

\*\* \* \*\* The parameters are included if the appropriate setup is configured in the admin panel (see “Add Extended Data to Callback” block in the Configurations -> Protocol Mappings section).

**Unsuccessful response**

| Parameter        | Description                                                                |
| ---------------- | -------------------------------------------------------------------------- |
| `action`         | DEBIT2VIRTUAL                                                              |
| `result`         | DECLINED                                                                   |
| `status`         | DECLINED                                                                   |
| `order_id`       | Transaction ID in the Merchant’s system                                    |
| `trans_id`       | Transaction ID in the Payment Platform                                     |
| `trans_date`     | Transaction date in the Payment Platform                                   |
| `descriptor`     | Descriptor from the bank, the same as payer will see in the bank statement |
| `amount`         | Order amount                                                               |
| `currency`       | Currency                                                                   |
| `commission`     | Commission for transaction                                                 |
| `total_amount`   | Total amount for transaction: total\_amount = amount + commission          |
| `decline_reason` | Description of the cancellation of the transaction                         |
| `hash`           | Special signature, used to validate callback, see Appendix A, Formula 2    |

**Undefined response**

| Parameter      | Description                                                                |
| -------------- | -------------------------------------------------------------------------- |
| `action`       | DEBIT2VIRTUAL                                                              |
| `result`       | UNDEFINED                                                                  |
| `status`       | REDIRECT / PREPARE                                                         |
| `order_id`     | Transaction ID in the Merchant’s system                                    |
| `trans_id`     | Transaction ID in the Payment Platform                                     |
| `trans_date`   | Transaction date in the Payment Platform                                   |
| `descriptor`   | Descriptor from the bank, the same as payer will see in the bank statement |
| `amount`       | Order amount                                                               |
| `currency`     | Order currency                                                             |
| `commission`   | Commission for transaction                                                 |
| `total_amount` | Total amount for transaction: total\_amount = amount + commission          |
| `hash`         | Special signature, used to validate callback, see Appendix A, Formula 2    |

#### DEBIT2VIRTUAL\_CALC request parameters

Use DEBIT2VIRTUAL\_CALC action to initiate DEBIT transaction and to get commission value.

| Parameter           | Description                                                            | Values                                                             | Required |
| ------------------- | ---------------------------------------------------------------------- | ------------------------------------------------------------------ | -------- |
| `action`            | Action that you want to perform. Fixed value.                          | DEBIT2VIRTUAL\_CALC                                                | +        |
| `client_key`        | Unique key (CLIENT\_KEY)                                               | UUID format value                                                  | +        |
| `order_id`          | Transaction ID in the Merchants system                                 | String up to 255 characters                                        | +        |
| `order_amount`      | The amount of the transaction                                          | Numbers in the form XXXX.XX                                        | +        |
| `order_currency`    | Currency                                                               | 3-letter code                                                      | +        |
| `order_description` | Description of the transaction (product name)                          | String up to 1024 characters                                       | +        |
| `identifier`        | Extra parameter for transaction                                        | It could be token, account information, additional descriptor etc. | +        |
| `brand`             | Brand through which the transaction is performed                       | String up to 36 characters                                         | +        |
| `payer_first_name`  | Customer’s name                                                        | String up to 32 characters                                         | -        |
| `payer_last_name`   | Customer’s surname                                                     | String up to 32 characters                                         | -        |
| `payer_middle_name` | Customer’s middle name                                                 | String up to 32 characters                                         | -        |
| `payer_birth_date`  | Customer’s birthday                                                    | <p>format yyyy-MM-dd,<br>e.g. 1970-02-17</p>                       | -        |
| `payer_address`     | Customer’s address                                                     | String up to 255 characters                                        | +        |
| `payer_address2`    | The adjoining road or locality (if required) of the customer’s address | String up to 255 character                                         | -        |
| `payer_country`     | Customer’s country                                                     | 2-letter code                                                      | -        |
| `payer_state`       | Customer’s state                                                       | String up to 32 characters                                         | -        |
| `payer_city`        | Customer’s city                                                        | String up to 32 characters                                         | -        |
| `payer_zip`         | ZIP-code of the Customer                                               | String up to 10 characters                                         | -        |
| `payer_email`       | Customer’s email                                                       | String up to 256 characters                                        | -        |
| `payer_phone`       | Customer’s phone                                                       | String up to 32 characters                                         | -        |
| `payer_ip`          | IP-address of the Customer                                             | XXX.XXX.XXX.XXX                                                    | +        |
| `payer_return_url`  | Customer return URL                                                    | String up to 256 characters                                        | +        |
| `hash`              | Special signature to validate your request to Payment Platform         | See Appendix A, Debit signature                                    | +        |

#### Response parameters

You will get JSON encoded string with transaction result.

**Successful response**

| **Parameter**  | **Description**                                                            |
| -------------- | -------------------------------------------------------------------------- |
| `action`       | DEBIT2VIRTUAL\_CALC                                                        |
| `result`       | SUCCESS                                                                    |
| `status`       | PREPARE                                                                    |
| `order_id`     | Transaction ID in the Merchant’s system                                    |
| `trans_id`     | Transaction ID in the Payment Platform                                     |
| `trans_date`   | Transaction date in the Payment Platform                                   |
| `descriptor`   | Descriptor from the bank, the same as payer will see in the bank statement |
| `amount`       | Order amount                                                               |
| `currency`     | Currency                                                                   |
| `commission`   | Commission for transaction                                                 |
| `total_amount` | Total amount for transaction: total\_amount = amount + commission          |

**Unsuccessful response**

| **Parameter**    | **Description**                                                            |
| ---------------- | -------------------------------------------------------------------------- |
| `action`         | DEBIT2VIRTUAL\_CALC                                                        |
| `result`         | DECLINED                                                                   |
| `status`         | DECLINED                                                                   |
| `order_id`       | Transaction ID in the Merchant’s system                                    |
| `trans_id`       | Transaction ID in the Payment Platform                                     |
| `trans_date`     | Transaction date in the Payment Platform                                   |
| `descriptor`     | Descriptor from the bank, the same as payer will see in the bank statement |
| `amount`         | Order amount                                                               |
| `currency`       | Currency                                                                   |
| `commission`     | Commission for transaction                                                 |
| `total_amount`   | Total amount for transaction                                               |
| `decline_reason` | The reason why the transaction was declined                                |

#### Callback parameters

**Successful response**

| **Parameter**                 | **Description**                                                                   |
| ----------------------------- | --------------------------------------------------------------------------------- |
| `action`                      | DEBIT2VIRTUAL\_CALC                                                               |
| `result`                      | SUCCESS                                                                           |
| `status`                      | PREPARE                                                                           |
| `order_id`                    | Transaction ID in the Merchant’s system                                           |
| `trans_id`                    | Transaction ID in the Payment Platform                                            |
| `trans_date`                  | Transaction date in the Payment Platform                                          |
| `descriptor`                  | Descriptor from the bank, the same as payer will see in the bank statement        |
| `amount`                      | Order amount                                                                      |
| `currency`                    | Currency                                                                          |
| `commission`                  | Commission for transaction                                                        |
| `total_amount`                | Total amount for transaction                                                      |
| `connector_name` \*\* \* \*\* | Connector's name (Payment Gateway)                                                |
| `rrn` \*\* \* \*\*            | Retrieval Reference Number value from the acquirer system                         |
| `approval_code` \*\* \* \*\*  | Approval code value from the acquirer system                                      |
| `gateway_id` **\***           | Gateway ID – transaction identifier provided by payment gateway                   |
| `extra_gateway_id`**\***      | Extra Gateway ID – additional transaction identifier provided by payment gateway. |
| `merchant_name`\*\* \* \*\*   | Merchant Name                                                                     |
| `mid_name` \*\* \* \*\*       | MID Name                                                                          |
| `issuer_country` \*\* \* \*\* | Issuer Country                                                                    |
| `issuer_bank` \*\* \* \*\*    | Issuer Bank                                                                       |
| `hash`                        | Special signature, used to validate callback, see Appendix A, Formula 2           |

\*\* \* \*\* The parameters are included if the appropriate setup is configured in the admin panel (see “Add Extended Data to Callback” block in the Configurations -> Protocol Mappings section).

**Unsuccessful response**

| **Parameter**    | **Description**                                                            |
| ---------------- | -------------------------------------------------------------------------- |
| `action`         | DEBIT2VIRTUAL\_CALC                                                        |
| `result`         | DECLINED                                                                   |
| `status`         | DECLINED                                                                   |
| `order_id`       | Transaction ID in the Merchant’s system                                    |
| `trans_id`       | Transaction ID in the Payment Platform                                     |
| `trans_date`     | Transaction date in the Payment Platform                                   |
| `descriptor`     | Descriptor from the bank, the same as payer will see in the bank statement |
| `amount`         | Order amount                                                               |
| `currency`       | Currency                                                                   |
| `commission`     | Commission for transaction                                                 |
| `total_amount`   | Total amount for transaction                                               |
| `decline_reason` | Description of the cancellation of the transaction                         |
| `hash`           | Special signature, used to validate callback, see Appendix A, Formula 2    |

#### DEBIT2VIRTUAL\_COMPLETE request parameters

Use DEBIT2VIRTUAL\_COMPLETE action to confirm initiated debit transaction.

| **Parameter** | **Description**                                                | **Values**                               | **Required field** |
| ------------- | -------------------------------------------------------------- | ---------------------------------------- | ------------------ |
| `action`      | Action that you want to perform. Fixed value.                  | DEBIT2VIRTUAL\_COMPLETE                  | +                  |
| `client_key`  | Unique key (CLIENT\_KEY)                                       | UUID format value                        | +                  |
| `trans_id`    | Initiated transfer transaction ID in the Payment Platform      | UUID format value                        | +                  |
| `hash`        | Special signature to validate your request to Payment Platform | See Appendix A, Complete Debit signature | +                  |

#### Response parameters

You will get JSON encoded string (see an example on Appendix B) with transaction result. If your account supports 3D-Secure, transaction result will be sent to your Notification URL.

**Successful response**

| **Parameter**  | **Description**                                                            |
| -------------- | -------------------------------------------------------------------------- |
| `action`       | DEBIT2VIRTUAL\_COMPLETE                                                    |
| `result`       | SUCCESS                                                                    |
| `status`       | 3DS / REDIRECT / SETTLED                                                   |
| `order_id`     | Transaction ID in the Merchant’s system                                    |
| `trans_id`     | Transaction ID in the Payment Platform                                     |
| `trans_date`   | Transaction date in the Payment Platform                                   |
| `descriptor`   | Descriptor from the bank, the same as payer will see in the bank statement |
| `amount`       | Order amount                                                               |
| `currency`     | Currency                                                                   |
| `commission`   | Commission for transaction                                                 |
| `total_amount` | Total amount for transaction                                               |

**Unsuccessful response**

| **Parameter**    | **Description**                                                            |
| ---------------- | -------------------------------------------------------------------------- |
| `action`         | DEBIT2VIRTUAL\_COMPLETE                                                    |
| `result`         | DECLINED                                                                   |
| `status`         | DECLINED                                                                   |
| `order_id`       | Transaction ID in the Merchant’s system                                    |
| `trans_id`       | Transaction ID in the Payment Platform                                     |
| `trans_date`     | Transaction date in the Payment Platform                                   |
| `descriptor`     | Descriptor from the bank, the same as payer will see in the bank statement |
| `amount`         | Order amount                                                               |
| `currency`       | Currency                                                                   |
| `commission`     | Commission for transaction                                                 |
| `total_amount`   | Total amount for transaction                                               |
| `decline_reason` | The reason why the transaction was declined                                |

**Undefined response**

| Parameter      | Description                                                                |
| -------------- | -------------------------------------------------------------------------- |
| `action`       | DEBIT2VIRTUAL\_COMPLETE                                                    |
| `result`       | UNDEFINED                                                                  |
| `status`       | REDIRECT / PREPARE                                                         |
| `order_id`     | Transaction ID in the Merchant’s system                                    |
| `trans_id`     | Transaction ID in the Payment Platform                                     |
| `trans_date`   | Transaction date in the Payment Platform                                   |
| `descriptor`   | Descriptor from the bank, the same as payer will see in the bank statement |
| `amount`       | Order amount                                                               |
| `currency`     | Order currency                                                             |
| `commission`   | Commission for transaction                                                 |
| `total_amount` | Total amount for transaction: total\_amount = amount + commission          |

#### Callback parameters

**Successful response**

| **Parameter**                 | **Description**                                                                   |
| ----------------------------- | --------------------------------------------------------------------------------- |
| `action`                      | DEBIT2VIRTUAL                                                                     |
| `result`                      | SUCCESS                                                                           |
| `status`                      | 3DS / REDIRECT / SETTLED                                                          |
| `order_id`                    | Transaction ID in the Merchant’s system                                           |
| `trans_id`                    | Transaction ID in the Payment Platform                                            |
| `trans_date`                  | Transaction date in the Payment Platform                                          |
| `descriptor`                  | Descriptor from the bank, the same as payer will see in the bank statement        |
| `amount`                      | Order amount                                                                      |
| `currency`                    | Currency                                                                          |
| `commission`                  | Commission for transaction                                                        |
| `total_amount`                | Total amount for transaction                                                      |
| `connector_name` \*\* \* \*\* | Connector's name (Payment Gateway)                                                |
| `rrn` \*\* \* \*\*            | Retrieval Reference Number value from the acquirer system                         |
| `approval_code` \*\* \* \*\*  | Approval code value from the acquirer system                                      |
| `gateway_id` **\***           | Gateway ID – transaction identifier provided by payment gateway                   |
| `extra_gateway_id`**\***      | Extra Gateway ID – additional transaction identifier provided by payment gateway. |
| `merchant_name`\*\* \* \*\*   | Merchant Name                                                                     |
| `mid_name` \*\* \* \*\*       | MID Name                                                                          |
| `issuer_country` \*\* \* \*\* | Issuer Country                                                                    |
| `issuer_bank` \*\* \* \*\*    | Issuer Bank                                                                       |
| `hash`                        | Special signature, used to validate callback, see Appendix A, Formula 2           |

\*\* \* \*\* The parameters are included if the appropriate setup is configured in the admin panel (see “Add Extended Data to Callback” block in the Configurations -> Protocol Mappings section).

**Unsuccessful response**

| **Parameter**    | **Description**                                                            |
| ---------------- | -------------------------------------------------------------------------- |
| `action`         | DEBIT2VIRTUAL                                                              |
| `result`         | DECLINED                                                                   |
| `status`         | DECLINED                                                                   |
| `order_id`       | Transaction ID in the Merchant’s system                                    |
| `trans_id`       | Transaction ID in the Payment Platform                                     |
| `trans_date`     | Transaction date in the Payment Platform                                   |
| `descriptor`     | Descriptor from the bank, the same as payer will see in the bank statement |
| `amount`         | Order amount                                                               |
| `currency`       | Currency                                                                   |
| `commission`     | Commission for transaction                                                 |
| `total_amount`   | Total amount for transaction                                               |
| `decline_reason` | Description of the cancellation of the transaction                         |
| `hash`           | Special signature, used to validate callback, see Appendix A, Formula 2    |

**Undefined response**

| Parameter      | Description                                                                |
| -------------- | -------------------------------------------------------------------------- |
| `action`       | DEBIT2VIRTUAL                                                              |
| `result`       | UNDEFINED                                                                  |
| `status`       | REDIRECT / PREPARE                                                         |
| `order_id`     | Transaction ID in the Merchant’s system                                    |
| `trans_id`     | Transaction ID in the Payment Platform                                     |
| `trans_date`   | Transaction date in the Payment Platform                                   |
| `descriptor`   | Descriptor from the bank, the same as payer will see in the bank statement |
| `amount`       | Order amount                                                               |
| `currency`     | Order currency                                                             |
| `commission`   | Commission for transaction                                                 |
| `total_amount` | Total amount for transaction: total\_amount = amount + commission          |
| `hash`         | Special signature, used to validate callback, see Appendix A, Formula 2    |

### GET\_TRANS\_STATUS request

***

Gets order status from Payment Platform. This request is sent by POST in the background (e.g., through PHP CURL).

#### Request parameters

| **Parameter** | **Description**                                                | **Values**                                   | **Required** |
| ------------- | -------------------------------------------------------------- | -------------------------------------------- | ------------ |
| `action`      | GET\_TRANS\_STATUS                                             | GET\_TRANS\_STATUS                           | +            |
| `client_key`  | Unique client key (CLIENT\_KEY)                                |                                              | +            |
| `trans_id`    | Transaction ID in the Payment Platform                         | String up to 255 characters                  | +            |
| `hash`        | Special signature to validate your request to Payment Platform | See Appendix A, GET\_TRANS\_STATUS signature | +            |

#### Response parameters

| **Parameter**    | **Description**                                                                         |
| ---------------- | --------------------------------------------------------------------------------------- |
| `action`         | GET\_TRANS\_STATUS                                                                      |
| `result`         | SUCCESS                                                                                 |
| `status`         | REDIRECT / PREPARE / DECLINED / SETTLED / REFUND / VOID                                 |
| `order_id`       | Order ID in the Client’s system                                                         |
| `trans_id`       | Transaction ID in the Payment Platform                                                  |
| `decline_reason` | Reason of transaction decline. It shows for the transactions with the “DECLINED” status |

### Errors

***

In case error you get synchronous response from Payment Platform:

| **Parameter**   | **Description** |
| --------------- | --------------- |
| `result`        | ERROR           |
| `error_message` | Error message   |

### Testing

***

You can make test requests using data below. Please note, that all transactions will be processed using Test engine.

| **Customer's email** |                                                                          **Testing / Result**                                                                          |
| :------------------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|   success@gmail.com  |  <p>This email must be used for testing successful sale.<br>Response on successful SALE request:<br><code>{action: SALE, result: SUCCESS, status: SETTLED}</code></p>  |
|    fail@gmail.com    | <p>This email date must be used for testing unsuccessful sale<br>Response on unsuccessful SALE request:<br>```{action: SALE, result: DECLINED, status: DECLINED}``</p> |

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


### Appendix A

***

Hash is signature rule used either to validate your requests to payment platform or to validate callback from payment platform to your system. It must be md5 encoded string calculated by rules below:

#### Sale signature

Hash is calculated by the formula:

```
_md5(strtoupper(strrev(identifier + order_id + order_amount + order_currency + PASSWORD)));_
```

#### Creditvoid signature

Hash is calculated by the formula:

```
_md5(strtoupper(strrev(trans_id + PASSWORD)));_
```

#### Void signature

Hash is calculated by the formula:

```
$hash = md5(strtoupper(strrev($trans_id)) . $ PASSWORD);
```

#### Credit2Virtual signature

Hash is calculated by the formula:

```
$hash = md5(strtoupper(strrev($order_id . $order_amount . $order_currency)) . $PASSWORD);
```

#### Debit signature

Hash is calculated by the formula:

```
_md5(strtoupper(strrev(identifier + order_id + order_amount + order_currency + PASSWORD)));_
```

#### Complete Debit signature

Hash is calculated by the formula:

```
$hash = md5(strtoupper(strrev($trans_id)) . $ PASSWORD);
```

#### GET\_TRANS\_STATUS signature

Hash is calculated by the formula:

```
$hash = md5(strtoupper(strrev($trans_id)) . $ PASSWORD);
```

#### Callback signature

Hash is calculated by the formula:

```
array_walk_recursive($params, static function (&$value) {
    $value = strrev($value);
});
$params['hash'] = md5(strtoupper(convert($params) . PASSWORD));
function convert($params)
{
    foreach ($params as &$value) {
        if (is_array($value)) {
            $value = $this->convert($value);
        }
    }
    ksort($params);
    return implode($params);
}
```

#### Sale callback signature

Hash calculation for notification in S2S APM based on the next formula:\
\
• All parameter values from the callback are used, except for hash;\
\
• Sort values alphabetically by their parameter names;\
\
• Reverse each value individually;\
\
• Concatenate all reversed values together into a single string;\
\
• Convert this string to uppercase;\
\
• Append the password in uppercase to the end of this string;\
\
• Generate the MD5 hash of the resulting string\


For example, for callback like this:

```action=sale
result=SUCCESS
amount=9.22
transactions=[ctrans1 = 123, atrans2 =32, itrans2 =325]
```

formula will be looking like this:

```
hash = reverse(action) + reverse(amount) + reverse(result) + reverse(transactions.atrans2) + reverse(transactions.ctrans1) + reverse(transactions.itrans2) + PASSWORD
```

and string\_result:

```
string_result = ELASSSECCUS22.923321523PASSWORD
```

and hash:

```
hash = md5(string_result)
```

#### Credit2Virtual callback signature

Hash is calculated by the formula:

```
$hash = md5(strtoupper(strrev($trans_id . $order_id . $status)) . $ PASSWORD);
```



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
