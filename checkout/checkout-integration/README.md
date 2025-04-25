---
icon: credit-card-front
---

# Checkout Integration

Version: 6.6.2\
\
Released: 2025/02/26\


## Introduction

***

This document describes the Checkout integration procedures between Checkout service and the website for e-commerce merchants.\
\
To make your integration easier, please download collection and try it out:\
[Postman Collection](https://identity.getpostman.com/accounts?continue=https%3A%2F%2Fgo.postman.co%2Fnetwork%2Fimport%3Fcollection%3D7445020-069ec96c-fc25-4f5f-b255-26dcecfe6d8e-TVKFyFni%26referrer%3Dhttps%253A%252F%252Fdocumenter.getpostman.com%252Fview%252F7445020%252FTVKFyFni%2523intro%26versionTag%3Dlatest%26traceId%3Dundefined)\


## General Information

***

Checkout service is a fast and easy way to create a secure payment page. It allows collecting and submitting payments and sending them for processing.

To use the Checkout service on the site you have to perform integration. Checkout integration provides a set of APIs that allow customizing payment processing for the business. These protocols implement acquiring payments (purchases) using specific API interaction with the merchant websites.

The API requires request data as json string data and responds also with json string data.

⚠️ **Pay attention**\


> Checkout protocol requires requests to be sent using JSON formatting with the following content type:\
>> \
> Header:> \
> Content-Type: application/json

### Checkout process

***

Checkout payment flow is shown below.\
When a Customer wants to make a purchase on your site the following happens:

1. Customer places an order and initiates payment on the site.
2. Site confirms the order and sends the payment processing request to the Checkout system with information about the order, payment and hash.
3. Checkout system validates the request and sends to the site the response with the redirect link.
4. The site redirects the Customer on the Checkout page by redirect link.
5. Customer selects the payment method, enters the payment data and confirms the payment. The payment method will be specifying automatically If only one method is available.
6. The payment processes at Payment Gateway.
7. Payment Gateway sends a callback to the site with the payment result.
8. The payment result is shown to the Customer.

The payment could be declined in case of invalid data detection.

### Protocol Mapping

It is necessary to check the existence of the protocol mapping before using the Checkout integration.\
Merchants can’t make payments if the CHECKOUT protocol is not mapped.

### Customer return after payment

After completing the payment, the payer will be returned to the URL specified in the Authentication request in the "success\_url" and "cancel\_url" parameters.\
If you need to get additional parameters in the return URLs, you should configure it in the Protocol Mappings modules by enabling the "Return parameters" attribute.\
In that case, "success\_url" and "cancel\_url" will contain the following data:

| **Parameter** | **Description**                                                                                                                                                                                                                          |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `payment_id`  | <p>Payment ID<br><strong>Example:</strong> dc66cdd8-d702-11ea-9a2f-0242c0a87002</p>                                                                                                                                                      |
| `trans_id`    | <p>Transaction ID<br><strong>Example:</strong> dc66cdd8-d702-11ea-9a2f-0242c0a87002</p>                                                                                                                                                  |
| `order_id`    | <p>Order ID<br><strong>Example:</strong> order-1234</p>                                                                                                                                                                                  |
| `hash`        | <p>Special signature, used to validate data. Addition in Signature section.<br>Must be SHA1 of MD5 encoded string (uppercased): payment_public_id + order.number + order.amount + order.currency + order.description + merchant.pass</p> |

⚠️ **Pay attention**\


> _that for "cancel\_url", the payer's return to the merchant with parameters is possible only after the decline has happened and the payer has closed the payment form (not the browser tab). If the payer closes the payment form without initiating the payment (pressing the PAY button), the redirection to "cancel\_url" occurs without passing additional parameters._

### DMS mode

If you need to use Checkout integration to work with payments in DMS-mode (two-step payments), you should set the corresponding MID configuration in the admin panel. Specify DMS-mode for the MID attribute.\
In this case, the payment request will be processed as an authorization stage for DMS-mode. The capture requests will be generated and gathered in the queue. You could monitor and manage the capture request queue via the admin panel as well.\
For detailed information, please contact your administrator.

### Checkout page description

***

Checkout page is shown to the Customer after a payment initiation. There are the fields to enter the payment data.

### Fields and Validation

The list of the fields on the Checkout page depends on the request parameters and the specified payment method.

Your customers will not see the fields if the acquirer does not need the information that is transmitted in them. For example, if an alternative payment method is specified, the card data is not displayed on the Checkout page.

As well, pay attention to the conditional fields such as e-mail or billing address. If the e-mail and billing address (data object) parameters are specified in the request, the Checkout page will not contain them.

Additional fields can also be displayed if a payment method is selected that requests additional data from the Customer.

The Checkout page has the fields validation. In case of the invalid data the error message will be shown and the field will be highlighted.

The list of the general fields and possible errors on the Checkout page is below:

| **Fields**      |                                         **Type**                                         | **Limitations**                                                 | **Error**                                                                                                                                              |
| --------------- | :--------------------------------------------------------------------------------------: | --------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `Card number`   |                                        _Integral_                                        | Lun algorithm, length 14-19 numbers                             | Invalid card number                                                                                                                                    |
| `Expirу Date`   |                                          _Date_                                          | <p>2-2 numbers (in the format mm-yy),<br>after today's date</p> | <p>The expiration date of card is expired<br>and not valid.</p>                                                                                        |
| `Security code` |                                        _Integral_                                        | Up to 4 characters                                              | Invalid security code                                                                                                                                  |
| `Name on card`  |                                         _String_                                         | Up to 32 characters                                             | <p>The name on card field<br>Must contain at least 2 words: first name and last name. Allowed special characters: hyphens, apostrophes, diacritics</p> |
| `Country`       |                                          _List_                                          | 2-letters code                                                  | <p>Country is required.<br>Please enter a valid Country</p>                                                                                            |
| `State/Region`  | <p><em>String</em><br><em>List</em> - for USA,<br>Canada, Australia,<br>Japan, India</p> |                                                                 |                                                                                                                                                        |
| `City`          |                                         _String_                                         | Up to 32 characters                                             | <p>City is required.<br>Please enter a valid City</p>                                                                                                  |
| `Address line`  |                                         _String_                                         | Up to 32 characters                                             | <p>Address line is required.<br>Please enter a valid Address line</p>                                                                                  |
| `Zip code`      |                                         _String_                                         | Up to 32 characters                                             | <p>Zip code is required.<br>Please enter a valid Zip code</p>                                                                                          |
| `Phone number`  |                                         _String_                                         | Up to 32 characters                                             | <p>Phone number is required.<br>Please enter a valid Phone number</p>                                                                                  |

### Pre-routing

To make payment method choice easier for the Customer, pre-routing is provided on the Checkout.

The functionality allows you to set up matches in the admin panel via the Custom routing module, which will determine the list of payment methods suitable for the current payment.

This way, you can restrict your Customers from randomly choosing a payment method that is not available in their region, for example. This will help you to increase the number of successful payments and reduce the risk of declined transactions.

Note that if the Authentication request contains a list of the payment methods in the `methods` array, then the pre-routing configurations will be ignored.

### Card data tokenization

For regular customers, we have made the payment page even more convenient and simple.

You can save the customer's card data so that they can reuse it for future payments.

To do this, you need to send the `req_token = true` parameter in the Authentication request. And then, in the callback, you will receive a card token.

Use the token when sending the next Authentication request and your client will see anonymized card details on the payment form, which will greatly simplify the payment process.

### Web information

Checkout service gathers information about browser, which the Customer uses.

When the Customer is on the Checkout page, the service gets the data about the Customer's OS, browser, and browser language. That information is sent to the acquirer in some cases.

### iFrame option

You can use iFrame option to show Checkout page on your domain. All you need is just to past in the code redirect url received in the response to the Authentication request.

Please follow the example:

```
<iframe src=redirect_url height="600" width="300"></iframe>
```

Note that screen size can be adjusted according to your requirements.

_Important:_ cross-domain requests are prohibited by security policy of our service.

⚠️ **Pay attention**\


> _By adding our Checkout Payment Page to the iframe on your site, ensure you ALLOW ACCESS TO COOKIE STORAGE._\
>> _This is required to avoid issues when redirecting the payer back to your site, such as after 3DS authentication. If you prohibit the storage of third-party data in your cookies, you will prevent the identification of the payer after returning from 3DS. This can cause interruptions in the payment session._\
>

### Page Customization

The administration service provides the ability to stylize the Checkout page in accordance with the preferences of the merchant. The action is available for the authorized users only. To customize the payment page you need:

1. Click the “Configuration → Branding” item on the menu bar. The settings are displayed in the work area.
2. Select a page template to make changes.
3. Change the settings:
   1. login icon selection;
   2. color selection for the following items:
      1. heading;
      2. background;
      3. buttons;
4. Click the “Submit” button to apply the settings. The selected settings will be displayed to the Customers on the Checkout page.

## Authentication request parameters

***

The merchant submits an authorization request and as a result of successful response receives the `redirect_url` - link on the Checkout page.\


```
/api/v1/session
```

The authentication performs when the payment is initiated.

You need to send an authentication POST request in JSON format on Checkout form URL (CHECKOUT\_URL) to start checkout process for the Customers. As a result of the authentication request you will receive the following:

* An authentication session is created with a unique identifier (Session ID). The session expires after one hour.
* A link is generated to redirect to the Checkout page: one link corresponds to one payment. The link becomes invalid after a successful payment. The authentication request parameters are below.\


If DMS-mode (two-stages payment) is available, after successful authentication it is necessary to capture. The capture would be performed automatically according to the settings or you can do it manually in the admin portal.

### Request Parameters

| **Parameter**                                              |      **Type**      | **Mandatory, Limitations**                                                                                                                                                                                                                                                       | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| ---------------------------------------------------------- | :----------------: | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `merchant_key`                                             |      _String_      | Required                                                                                                                                                                                                                                                                         | <p>Key for Merchant identification<br><strong>Example:</strong> xxxxx-xxxxx-xxxxx</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `operation`                                                |      _String_      | Required                                                                                                                                                                                                                                                                         | <p>Defines a payment transaction<br><strong>Possible values:</strong> purchase, debit, transfer<br>Send the “purchase” value so that the payment page for sale initiation will be shown to the customer.<br>Send the “debit” value so that the payment page for debit initiation will be shown to the customer.<br>Send the “transfer” value so that the payment page for transfer initiation will be shown to the customer.<br><strong>Example:</strong> purchase</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| `methods`                                                  |       _Array_      | Optional                                                                                                                                                                                                                                                                         | <p>An array of payment methods. Limits the available methods on the Checkout page (the list of the possible values in the Payment methods section). In the case of parameter absence, the pre-routing rules are applied. If pre-routing rules are not configured, all available payment methods are displayed.<br><strong>Condition:</strong> for purchase operation only<br>For purchase and debit operations.<br><strong>Example:</strong> card, paypal, googlepay</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| `channel_id`                                               |      _String_      | <p>Optional<br>max: 16</p>                                                                                                                                                                                                                                                       | <p>This parameter is used to direct payments to a specific sub-account (channel).<br>If the channel is configured for Merchant Mapping, the system matches the value with the corresponding <code>channel_id</code> value in the request to route the payment.<br><br>Note: The channel must correspond to one of the payment <code>methods</code> (brands) listed in the <code>methods</code> array. If the methods array is empty, only the <code>channel_id</code> will affect the selection of the payment method (Merchant Mapping).</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `session_expiry`                                           |      _Integer_     | <p>Optional<br>Values from 1 to 720 min<br></p>                                                                                                                                                                                                                                  | <p>Session expiration time in minutes.<br>Default value = 60.<br>Could not be zero.<br><strong>Example:</strong> 60</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| `success_url`                                              |      _String_      | <p>Required<br>Valid URL<br>max: 1024</p>                                                                                                                                                                                                                                        | <p>URL to redirect the Customer in case of the successful payment<br><strong>Example:</strong> https://example.domain.com/success<br>The return of additional parameters can be configured for success_url. See the “Customer return after payment” section for details.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| `cancel_url`                                               |      _String_      | <p>Optional<br>Valid URL<br>min: 0 max: 1024</p>                                                                                                                                                                                                                                 | <p>URL to return Customer in case of a payment cancellation (“Close” button on the Checkout page). The logic of redirection on “cancel_url” could be configured in the admin panel (Configuration -> Protocol Mappings section): use the “Maximum count declines” field to set the payment failed attempts quantity before redirection. For example, if the field value is set to "1", then after the first declined attempt, a payer will be redirected to "cancel_url."<br><strong>Example:</strong> https://example.domain.com/cancel<br>The return of additional parameters can be configured for cancel_url. See the “Customer return after payment” section for details.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| `expiry_url`                                               |      _String_      | <p>Optional<br>Valid URL<br>min: 0 max: 1024</p>                                                                                                                                                                                                                                 | <p>URL where the payer will be redirected in case of session expiration<br>**Example: **https://example.domain.com/expiry</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `error_url`                                                |      _String_      | <p>Optional<br>Valid URL<br>min: 0 max: 1024</p>                                                                                                                                                                                                                                 | <p>URL to return Customer in case of undefined transaction status.<br>If the URL is not specified, the cancel_url is used for redirection.<br>**Example: **https://example.domain.com/errorurl</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| `url_target`                                               |      _String_      | <p>Optional<br>Possible values: <code>_self</code>, <code>_parent</code>, <code>_top</code>.<br><br>Find the result of applying the values in the HTML standard description (<a href="https://html.spec.whatwg.org/#valid-browsing-context-name">Browsing context names</a>)</p> | <p>Name of, or keyword for a browsing context where Customer should be returned according to HTML specification.<br><strong>Example:</strong> _parent</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `req_token`                                                |      _Boolean_     | <p>Optional<br>default - false</p>                                                                                                                                                                                                                                               | <p>Special attribute pointing for further tokenization<br>If the <code>card_token</code> is specified, <code>req_token</code> will be ignored.<br>For purchase and debit operations.<br><strong>Example:</strong> false</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| `card_token`                                               | _Array of Strings_ | <p>Optional<br>String 64 characters</p>                                                                                                                                                                                                                                          | <p>Credit card token value<br>For purchase and debit operations.<br><strong>Example:</strong> f5d6a0ab6fcfb6487a39e2256e50fff3c95aaa97</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| `recurring_init`                                           |      _Boolean_     | <p>Optional<br>default - false</p>                                                                                                                                                                                                                                               | <p>Initialization of the transaction with possible following recurring<br>Only for purchase operation<br><strong>Example:</strong> true</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| `schedule_id`                                              |      _String_      | <p>Optional<br>It s available when recurring_init = true</p>                                                                                                                                                                                                                     | <p>Schedule ID for recurring payments<br>Only for purchase operation<br><strong>Example:</strong> 57fddecf-17b9-4d38-9320-a670f0c29ec0</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| `vat_calc`                                                 |      _Boolean_     | <p>Conditional<br>true or false<br>default - false</p>                                                                                                                                                                                                                           | <p>Indicates the need of calculation for the VAT amount<br>• 'true' - if VAT calculation needed<br>• 'false' - if VAT should not be calculated for current payment.<br>Only for purchase operation<br><strong>Example:</strong> false</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `hash`                                                     |      _String_      | Required                                                                                                                                                                                                                                                                         | <p>Special signature to validate your request to Payment Platform Addition in Signature section.<br><strong>Example:</strong> Must be SHA1 of MD5 encoded string (uppercased): order_number + order_amount + order_currency + order_description + password</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| **order**                                                  |      _Object_      | Required                                                                                                                                                                                                                                                                         | Information about an order                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| `number`                                                   |      _String_      | <p>Required<br>max: 255<br>[a-z A-Z 0-9 -!"#$%&#x26;'()*+,./:;&#x26;@]</p>                                                                                                                                                                                                       | <p>Order ID<br><strong>Example:</strong> order-1234</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| `amount`                                                   |      _String_      | <p>Required<br>Greater then 0<br>[0-9]<br>max: 255</p>                                                                                                                                                                                                                           | <p>Format depends on currency.<br>Send Integer type value for currencies with zero-exponent. <strong>Example:</strong> 1000<br>Send Float type value for currencies with exponents 2, 3, 4.<br>Format for 2-exponent currencies: XX.XX <strong>Example:</strong> 100.99<br><strong>Pay attention</strong> that currencies 'UGX', 'JPY', 'KRW', 'CLP' must be send in the format XX.XX, with the zeros after comma. <strong>Example:</strong> 100.00<br>Format for 3-exponent currencies: XXX.XXX <strong>Example:</strong> 100.999.<br>Format for 4-exponent currencies: XXX.XXXX <strong>Example:</strong> 100.9999<br>⚠️ <strong>Note!</strong> For crypto currencies use the exponent as appropriate for the specific currency.<br><strong>For purchase operation with crypto:</strong><br>Do not send parameter at all if you want to create a transaction with an unknown amount - for cases when the amount is set by the payer outside the merchant site (for example, in an app). As well, you have to omit "amount" parameter for hash calculation for such cases.<br><strong>For transfer operation:</strong><br>Send amount if you want to show default value in the amount field on the Checkout. The customers can change the value manually on the payment form.Send "-1" value if you want to show empty amount field on the Checkout so that the Customers could enter the value manually.</p> |
| `currency`                                                 |      _String_      | <p>Required<br>3 characters for fiat currencies and from 3 to 6 characters for crypto currencies<br>[A-Z]<br><a href="https://www.xe.com/iso4217.php">ISO 4217</a></p>                                                                                                           | <p>Currency<br><strong>Example (fiat):</strong> USD<br><strong>Example (crypto):</strong> USDT</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| `description`                                              |      _String_      | <p>Required<br>min: 2 max: 1024<br>[a-z A-Z 0-9 !"#$%&#x26;'()*+,./:;&#x26;@]</p>                                                                                                                                                                                                | <p>Product name<br><strong>Example:</strong> Very important gift - # 9</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| **customer**                                               |      _Object_      | Conditional                                                                                                                                                                                                                                                                      | Customer's information. Send an object if a payment method needs                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| `name`                                                     |      _String_      | <p>Conditional<br>min: 2 max: 32<br>Latin basic<br>[a-z A-Z]</p>                                                                                                                                                                                                                 | <p>Customer's name<br>Condition: If the parameter is NOT specified in the request, then it will be displayed on the Checkout page (if a payment method needs) - the "Cardholder" field<br><strong>Example:</strong> John Doe</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| `email`                                                    |      _String_      | <p>Conditional<br>min: 2 max: 255<br>email format</p>                                                                                                                                                                                                                            | <p>Customer's email address<br>Condition: If the parameter is NOT specified in the request, then it will be displayed on the Checkout page (if a payment method needs) - the "E-mail" field<br><strong>Example:</strong> example@mail.com</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `birth_date`                                               |      _String_      | <p>Conditional<br>format: yyyy-mm-dd<br><a href="https://en.wikipedia.org/wiki/ISO_8601">ISO 8601</a></p>                                                                                                                                                                        | <p>Payer's birth date<br><strong>Example:</strong> 1970-02-17</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| **billing\_address**                                       |      _Object_      | Conditional                                                                                                                                                                                                                                                                      | <p>Billing address information.<br><strong>Condition:</strong> If the object or some object's parameters are NOT specified in the request, then it will be displayed on the Checkout page (if a payment method needs)</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `country`                                                  |      _String_      | <p>Conditional 2 characters<br>(alpha-2 code)<br><a href="https://www.iban.com/country-codes">ISO 3166-1</a></p>                                                                                                                                                                 | <p>Billing country<br><strong>Example:</strong> US</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| `state`                                                    |      _String_      | <p>Optional<br>min: 2 max: 32<br>[a-z A-Z]<br>It is 2-letters code for USA, Canada, Australia, Japan, India</p>                                                                                                                                                                  | <p>Billing state address<br><strong>Example:</strong> CA</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| `city`                                                     |      _String_      | <p>Conditional<br>min: 2 max: 40<br>[a-z A-Z]</p>                                                                                                                                                                                                                                | <p>Billing city<br><strong>Example:</strong> Los Angeles</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| `district`                                                 |      _String_      | <p>Optional<br>min: 2 max: 32<br>[a-z A-Z 0-9 - space]</p>                                                                                                                                                                                                                       | <p>City district<br><strong>Example:</strong> Beverlywood</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `address`                                                  |      _String_      | <p>Conditional<br>min: 2 max: 32<br>[a-z A-Z 0-9]</p>                                                                                                                                                                                                                            | <p>Billing address<br><strong>Example:</strong> Moor Building</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| `house_number`                                             |      _String_      | <p>Optional<br>min: 1 max: 9<br>[a-z A-Z 0-9/ - space]</p>                                                                                                                                                                                                                       | <p>House number<br><strong>Example:</strong> 17/2</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `zip`                                                      |      _String_      | <p>Conditional<br>min: 2 max: 10<br>[a-z A-Z 0-9]</p>                                                                                                                                                                                                                            | <p>Billing zip code<br><strong>Example:</strong> 123456, MK77</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| `phone`                                                    |      _String_      | <p>Conditional<br>min: 1 max: 32<br>[0-9 + () -]</p>                                                                                                                                                                                                                             | <p>Customer phone number<br><strong>Example:</strong> 347771112233</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| **payee**                                                  |      _Object_      | Conditional                                                                                                                                                                                                                                                                      | <p>Payee's information.<br>Specify additional information about Payee for transfer operation if it is required by payment provider.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| `name`                                                     |      _String_      | <p>Required<br>min: 2 max: 32<br>Latin basic<br>[a-z A-Z]</p>                                                                                                                                                                                                                    | <p>Customer's name.<br><strong>Example:</strong> John Doe</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `email`                                                    |      _String_      | <p>Conditional<br>email format</p>                                                                                                                                                                                                                                               | <p>Customer's email address.<br><strong>Example:</strong> example@mail.com</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| **payee\_billing\_address**                                |      _Object_      | Conditional                                                                                                                                                                                                                                                                      | Billing address information for Payee.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| `country`                                                  |      _String_      | <p>Conditional 2 characters<br>(alpha-2 code)<br><a href="https://www.iban.com/country-codes">ISO 3166-1</a></p>                                                                                                                                                                 | <p>Billing country<br><strong>Example:</strong> US</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| `state`                                                    |      _String_      | <p>Optional<br>min: 2 max: 32<br>[a-z A-Z]<br>It is 2-letters code for USA, Canada, Australia, Japan, India</p>                                                                                                                                                                  | <p>Billing state address<br><strong>Example:</strong> CA</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| `city`                                                     |      _String_      | <p>Conditional<br>min: 2 max: 40<br>[a-z A-Z]</p>                                                                                                                                                                                                                                | <p>Billing city<br><strong>Example:</strong> Los Angeles</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| `district`                                                 |      _String_      | <p>Optional<br>min: 2 max: 32<br>[a-z A-Z 0-9 - space]</p>                                                                                                                                                                                                                       | <p>City district<br><strong>Example:</strong> Beverlywood</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `address`                                                  |      _String_      | <p>Conditional<br>min: 2 max: 32<br>[a-z A-Z 0-9]</p>                                                                                                                                                                                                                            | <p>Billing address<br><strong>Example:</strong> Moor Building</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| `house_number`                                             |      _String_      | <p>Optional<br>min: 1 max: 9<br>[a-z A-Z 0-9/ - space]</p>                                                                                                                                                                                                                       | <p>House number<br><strong>Example:</strong> 17/2</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `zip`                                                      |      _String_      | <p>Conditional<br>min: 2 max: 10<br>[a-z A-Z 0-9]</p>                                                                                                                                                                                                                            | <p>Billing zip code<br><strong>Example:</strong> 123456, MK77</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| `phone`                                                    |      _String_      | <p>Conditional<br>min: 1 max: 32<br>[0-9 + () -]</p>                                                                                                                                                                                                                             | <p>Customer phone number<br><strong>Example:</strong> 347771112233</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| **crypto**                                                 |      _Object_      | Optional                                                                                                                                                                                                                                                                         | Additional information regarding crypto transactions                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| `network` \*\* \* \*\*                                     |      _String_      | <p>Optional<br>max: 50</p>                                                                                                                                                                                                                                                       | <p>You can use an arbitrary value or select one from the following.<br><strong>Example:</strong><br>• ERC20<br>• TRC20<br>• BEP20<br>• BEP2<br>• OMNI<br>• solana<br>• polygon</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| **parameters**                                             |      _Object_      | Optional                                                                                                                                                                                                                                                                         | <p>Extra-parameters required for specific payment method<br><strong>Example:</strong><br><code>"parameters": { "payment_method": { "param1":"val1", "param2":"val2" } }</code></p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| **custom\_data**                                           |      _Object_      | Optional                                                                                                                                                                                                                                                                         | <p>Custom data<br>This block can contain arbitrary data, which will be returned in the callback.<br><strong>Example:</strong><br><code>“custom_data”: {“param1”:”value1”, “param2”:”value2”, “param3”:”value3”}</code></p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| \*\* \* \*\* Specified value will be shown on the Checkout |                    |                                                                                                                                                                                                                                                                                  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |

<details>

<summary>Authentication (OK)</summary>

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/session' \
--header 'Content-Type: application/json' \
--data-raw '{
  "merchant_key": "xxxxx-xxxxx-xxxxx",
  "operation": "purchase",
  "methods": [
    "card",
    "method1"
  ],
  "parameters": {
    "card": {
      "param1": "val-1",
      "param2": "val-2"
    },
    "method1": {
      "param1": "val-3",
      "param2": "val-4"
    }
  },
  "session_expiry": 60,
  "order": {
    "number": "order-1234",
    "amount": "0.19",
    "currency": "USD",
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
    "country": "US",
    "state": "CA",
    "city": "Los Angeles",
    "district": "Beverlywood",
    "address": "Moor Building 35274",
    "house_number": "17/2",
    "zip": "123456",
    "phone": "347771112233"
  },
  "card_token": [
    "f5d6a0ab6fcfb6487a39e2256e50fff3c95aaa97075ee5e539bb662fceff4dc1"
  ],
  "req_token": true,
  "recurring_init": true,
  "schedule_id": "9d0f5cc4-f07b-11ec-abf4-0242ac120006",
  "hash": "{{session_hash}}"
}
'
```

</details>

<details>

<summary>Debit Operation (OK)</summary>

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/session' \
--header 'Content-Type: application/json' \
--data-raw '{
  "merchant_key": "xxxxx-xxxxx-xxxxx",
  "operation": "debit",
  "methods": [
    "card"
  ],
  "parameters": {
    "card": {
      "param1": "val-1",
      "param2": "val-2"
    },
    "session_expiry": 60,
    "order": {
      "number": "order-1234",
      "amount": "0.19",
      "currency": "USD",
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
      "country": "US",
      "state": "CA",
      "city": "Los Angeles",
      "disctrict": "Beverlywood",
      "address": "Moor Building 35274",
      "house_number": "17/2",
      "zip": "123456",
      "phone": "347771112233"
    },
    "card_token": [
      "f5d6a0ab6fcfb6487a39e2256e50fff3c95aaa97075ee5e539bb662fceff4dc1"
    ],
    "req_token": true,
    "hash": "{{session_hash}}"
  }
}
'
```

</details>

<details>

<summary>Transfer Operation (OK)</summary>

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/session' \
--header 'Content-Type: application/json' \
--data-raw '{  
   "merchant_key":"xxxxx-xxxxx-xxxxx",
   "operation":"transfer",
   "order":{
      "number":"order-1234",
      "amount": "100.55",
      "currency":"USD",
      "description":"Important gift"
   },
   "cancel_url":"https://example.com/cancel",
   "success_url":"https://example.com/success",
   "customer":{
        "name":"John Doe",
        "email":"success@gmail.com"
    },
    "payee": {
        "name":"John Doe",
        "email":"success@gmail.com"
        },
    "billing_address":{
        "country":"US",
        "state":"California",
        "city":"Los Angeles",
        "address":"Moor Building 35274 State ST Fremont. U.S.A",
        "zip":"94538",
        "phone":"0987654321",
        "district":"Brentwood",
        "house_number":"123"
    },
    "payee_billing_address":{
        "country":"US",
        "state":"New York",
        "city":"New York",
        "address":"Moor Building 35274 State ST Fremont. U.S.A",
        "zip":"94538",
        "phone":"0987654321",
        "district":"Brentwood",
        "house_number":"123"
    },
   "hash":"{{session_hash}}"
}
'
```

</details>

<details>

<summary>Example Response (OK)</summary>

```
{
  "redirect_url": "{{CHECKOUT_HOST}/auth/ZXlKMGVYQWlPaUpLVjFRaUxDSmhiR2NpT2lKU1V6STFOaUo5LmV5SnBZWFFpT2pFMU9UWXhPRFl6T0Rnc0ltcDBhU0k2SWpGbE5qTTNObVZoTFdRek1HUXRNVEZsWVMxaE16QXlMVEF5TkRKak1HRTRNekF4TWlJc0ltVjRjQ0k2TVRVNU5qRTRPVGs0T0gwLm9CMmVhdlRtTU5DMXFTajlDVFlqQ0dOMDlHdUs1NXRkQTVpWFR3d2F2cWR0cEpEU2NRWWFaT3Z5dmJSVjJUSFNUVlFlS0NUX3pRdFNycDlKS1M4X0pqUzRMclM5MnUyNXRfSHNGa1FUQ0VOdGtadHQtaGxONERYdVhkLTU5cEhKLUN1RXBqSmZ4UDZEQXhFaVAxWEpRZDlyQldNa1RQVDdGZm1ac0g4LTM5YnV6LTI3MWxKMndkekdvSGJYa0NKVnNTNFJldGxrbno2U3dGd3ZFMW5KNDhwYTBGMDNLWjBpNnhpRFVPR3p2U0ZKdGZfMndDTTdzTTdsemc1TlBmSDl0Q0RKQmZEaG1hUmJCRmR6RlZMZlJncG5tMzB3VWpTMGMxbmt6SkkxOGJTd2Z6Z0hfZFpnc1cyUFhCM2ZLdG9pWDJXeFRsQzlxR204QTRYVm9EQy1mOWxvRHlMd0F5eV9xY3JrWmNuQTJVSjk5Zl91c0cwODZKUlBTT0I4VHVRZndSTzUxSEN2bEU2TXdFYzVYRmtnYjBleEZRcXdpNGE4S2RlWV9HX3ZQam42bnpZODdtVzFINlpQMjJ0dzVzazYtUENMeHdvNXctUmFBWC1mYVVhcEVHTzFLZkVHbndaQWZBZVNyc3U4MV9XQUFJMlN5RUxGWi1IU1lXMUZLWFgybzNNeF93Ty1DS3FLTWZsUTV1cGc2eDAybzhsbFhoeGJlVmVIOWlkMHgzYldRWE9vWk5hWm1MeVpJMmJsT2dtVDV0cHR4NHNQNDNqT0NtYW1sdkxyUkZvQmxCNTJ4V0RUQTBZQnhBLW5meUxCRHRJN0dPaVRWQjJ5cWd1Z1lBdGRfbWFQN2x2YTJpbVJWaHhxT0R5SlRiZThxcDdhWkw4bkJvTHZocnZDOHlv"
}
```

</details>

## Authentication request possible errors

***

### Parameter Values Validation

Checkout service validates the request parameters and sends the error responses in case of an invalid data detection.

<details>

<summary>Example Request - Validation (bad)</summary>

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/session' \
--header 'Content-Type: application/json' \
--data-raw '{  
   "merchant_key":"xxxxx-xxxxx-xxxxx",
   "operation":"credit",
   "methods":[ "" ],
   "order":{
      "number":"",
      "amount": "1.2",
      "currency":"Dollar",
      "description":""
   },
   "cancel_url":"1.com",
   "success_url":"",
   "customer":{
      "name":"John Doe",
      "email":"example@mail.com"
   },
   "recurring_init": "true",
   "hash":"728d13b95cf2b6b3ee04b20dc2fc9889ffff1cf4"
}
'
```

</details>

<details>

<summary>Example Response - 400 Bad Request</summary>

```
{
  "error_code": 0,
  "error_message": "Request data is invalid.",
  "errors": [
    {
      "error_code": 100000,
      "error_message": "operation: The value you selected is not a valid choice."
    },
    {
      "error_code": 100000,
      "error_message": "methods: This value should not be blank."
    },
    {
      "error_code": 100000,
      "error_message": "order.number: This value should not be blank."
    },
    {
      "error_code": 100000,
      "error_message": "order.amount: This value should be greater than 0."
    },
    {
      "error_code": 100000,
      "error_message": "order.currency: This value is not valid."
    },
    {
      "error_code": 100000,
      "error_message": "order.description: This value should not be blank."
    },
    {
      "error_code": 100000,
      "error_message": "cancel_url: This value is not a valid URL."
    },
    {
      "error_code": 100000,
      "error_message": "success_url: This value should not be blank."
    }
  ]
}
```

</details>

### Hash Validation

The Checkout service always performs `hash` validation.\
The request will be rejected if the `hash` value is invalid.

<details>

<summary>Example Request - Hash Validation</summary>

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/session' \
--header 'Content-Type: application/json' \
--data-raw '{  
   "merchant_key":"xxxxx-xxxxx-xxxxx",
   "operation":"purchase",
   "methods":[
      "card"
   ],
   "order":{
      "number":"order-1234",
      "amount": "0.19",
      "currency":"USD",
      "description":"Important gift"
   },
   "cancel_url":"https://example.com/cancel",
   "success_url":"https://example.com/success",
   "customer":{
      "name":"John Doe"
   },
   "hash":"wrong hash"
}
'
```

</details>

<details>

<summary>Example Response - Hash error</summary>

```
{
  "error_code": 0,
  "error_message": "Request data is invalid.",
  "errors": [
    {
      "error_code": 100000,
      "error_message": "hash: Hash is not valid."
    }
  ]
}
```

</details>

## Callback Notification

***

Checkout service sends the callback on the merchant `notification_URL` as a result of an operation.

You can receive the callback for the next operation types:

* SALE
* 3DS
* REDIRECT
* REFUND
* VOID
* RECURRING
* CHARGEBACK
* DEBIT
* TRANSFER

⚠️ **Pay attention**\


> _Note that the notification URL may be temporarily blocked due to consistently receiving timeouts in response to the callback._> _If five timeouts accumulate within five minutes for a merchant’s notification URL, it will be blocked for 15 minutes. During this block,_ _all merchants associated with the URL will not receive notifications._> _The block automatically lifts after 15 minutes._> _Additionally, it is possible to manually unblock the URL through the admin panel by navigating to Configuration → Merchants → Edit_ _Merchant. In this case, the block will be removed immediately._> _The timeout counter resets if a callback response is successfully processed. For instance, if there are four timeouts within five minutes_> _but a successful response on the sixth minute, the counter resets._

⚠️ **Pay attention**\


> _In the case of cascading, the logic for sending callbacks differs._\
> &#xNAN;_&#x49;f cascading is triggered for the order, in general case you will receive only callback for the last payment attempt, where the final status of the order (settled or declined) is determined._> \
> _In the particular cases, you will receive callback for the first payment attempt with the data for customer’s redirection if it is required by payment provider. Callbacks for intermediate attempts (between the first decline and the last payment attempt) are not sent._\
>

### List of Possible Transaction Statuses

The possible statuses are listed below:

| **Transaction Status** | **Operation type**                                                                 | **Description**                                                                |
| ---------------------- | ---------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| SUCCESS                | sale, 3ds, redirect, capture, refund, void, recurring, chargeback, debit, transfer | Transaction is successfully completed in Payment Platform                      |
| FAIL                   | sale, capture, refund, void, recurring, debit, transfer                            | Transaction has the errors and is not validated by Payment Platform            |
| WAITING                | sale, capture, refund, void, recurring, debit, transfer                            | Transaction is being processed by Payment Platform                             |
| UNDEFINED              | sale, 3ds, redirect, capture, refund, void, recurring, debit, transfer             | Uncertain payment status due to problems interacting with the payment provider |

⚠️ **Pay attention**

> _**that successful transaction does not mean successful final status for the payment.**_\
>
>
> > _**Example for SMS mode (1-step paments) only:**_\
> >> > _Payment is successfully completed if transaction has `status` = success and `type` = sale. Payment is not completed if transaction has `status` = success and `type` = redirect._

### Callback parameters

Callback is HTTP POST request whcih includes the following data:

| **Parameter**                   |   **Type**   | **Mandatory, Limitations**                               | **Description**                                                                                                                                                                                                                                                                                                                                                            |
| ------------------------------- | :----------: | -------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `id`                            |    String    | Required                                                 | <p>Transaction ID<br><strong>Example:</strong> dc66cdd8-d702-11ea-9a2f-0242c0a87002</p>                                                                                                                                                                                                                                                                                    |
| `order_number`                  |   _String_   | Required Up to 255 characters                            | <p>Order ID<br><strong>Example:</strong> order-1234</p>                                                                                                                                                                                                                                                                                                                    |
| `order_amount`                  |    _Float_   | Required Format: XX.XX, without leading zeroes           | <p>Product price<br><strong>Example:</strong> 0.19</p>                                                                                                                                                                                                                                                                                                                     |
| `order_currency`                |   _String_   | Required Up to 6 characters                              | <p>Currency code.<br>3-characters for fiat currencies and from 3 to 6 characters for crypto currencies<br><strong>Example (fiat):</strong> USD<br><strong>Example (crypto):</strong> USDT</p>                                                                                                                                                                              |
| `order_description`             |   _String_   | Required Up to 1024 characters                           | <p>Product description<br><strong>Example:</strong> Important gift</p>                                                                                                                                                                                                                                                                                                     |
| `order_status`                  |   _String_   | Required Up to 20 characters                             | <p>Payment status: prepare, settled, pending, 3ds, redirect, decline, refund, reversal, chargeback<br><strong>Example:</strong> 3ds</p>                                                                                                                                                                                                                                    |
| `type`                          |   _String_   | Required Up to 36 characters                             | <p>Operation type: sale, 3ds, redirect, capture, refund, void, chargeback, debit, transfer<br><strong>Example:</strong> sale</p>                                                                                                                                                                                                                                           |
| `status`                        |   _String_   | Required Up to 20 characters                             | <p>Transaction status: success, fail, waiting, undefined<br><strong>Example:</strong> success</p>                                                                                                                                                                                                                                                                          |
| `reason`                        |   _String_   | Optional Up to 1024 characters                           | <p>Decline or error reason. It displays only if the transaction has FAIL status.<br><strong>Example:</strong> The operation was rejected. Please contact the site support</p>                                                                                                                                                                                              |
| `payee_name`                    |   _String_   | Optional                                                 | <p>Payee's first and last name<br>Returns only for transfer operation<br><strong>Example:</strong> John Rickher</p>                                                                                                                                                                                                                                                        |
| `payee_email`                   |   _String_   | Optional                                                 | <p>Payee's e-mail<br>Returns only for transfer operation<br><strong>Example:</strong> example@mail.com</p>                                                                                                                                                                                                                                                                 |
| `payee_country`                 |   _String_   | <p>Optional<br>Up to 3 characters</p>                    | <p>Payee's country<br>Returns only for transfer operation<br><strong>Example:</strong> US</p>                                                                                                                                                                                                                                                                              |
| `payee_state`                   |   _String_   | <p>Optional<br>Up to 32 characters</p>                   | <p>Payee's state<br>Returns only for transfer operation<br><strong>Example:</strong> California</p>                                                                                                                                                                                                                                                                        |
| `payee_city`                    |   _String_   | <p>Optional<br>Up to 32 characters</p>                   | <p>Payee's city<br>Returns only for transfer operation<br><strong>Example:</strong> Los Angeles</p>                                                                                                                                                                                                                                                                        |
| `payee_address`                 |   _String_   | <p>Optional<br>Up to 32 characters</p>                   | <p>Payee's city<br>Returns only for transfer operation<br><strong>Example:</strong> 123 Sample Street</p>                                                                                                                                                                                                                                                                  |
| `connector_name` \*\* \* \*\*   |   _String_   | Optional                                                 | <p>Connector's name (Payment Gateway).<br><strong>Example:</strong> MyPaymentConnector</p>                                                                                                                                                                                                                                                                                 |
| `rrn` \*\* \* \*\*              |   _String_   | Optional                                                 | <p>Retrieval Reference Number value from the acquirer system.<br><strong>Example:</strong> 123456789012</p>                                                                                                                                                                                                                                                                |
| `approval_code` \*\* \* \*\*    |   _String_   | Optional                                                 | <p>Approval code value from the acquirer system.<br><strong>Example:</strong> 123456</p>                                                                                                                                                                                                                                                                                   |
| `gateway_id` \*\* \* \*\*       |   _String_   | Optional                                                 | <p>Gateway ID – transaction identifier provided by payment gateway.<br><strong>Example:</strong> 123-456-789</p>                                                                                                                                                                                                                                                           |
| `extra_gateway_id` \*\* \* \*\* |   _String_   | Optional                                                 | <p>Extra Gateway ID – additional transaction identifier provided by payment gateway.<br><strong>Example:</strong> abc123</p>                                                                                                                                                                                                                                               |
| `merchant_name`\*\* \* \*\*     |   _String_   | Optional                                                 | <p>Merchant Name<br><strong>Example:</strong> TESTMerchantName</p>                                                                                                                                                                                                                                                                                                         |
| `mid_name` \*\* \* \*\*         |   _String_   | Optional                                                 | <p>MID Name<br><strong>Example:</strong> TESTMIDName</p>                                                                                                                                                                                                                                                                                                                   |
| `issuer_country` \*\* \* \*\*   |   _String_   | Optional                                                 | <p>Issuer Country<br><strong>Example:</strong> DE</p>                                                                                                                                                                                                                                                                                                                      |
| `issuer_bank` \*\* \* \*\*      |   _String_   | Optional                                                 | <p>Issuer Bank<br><strong>Example:</strong> Deutsche Bank</p>                                                                                                                                                                                                                                                                                                              |
| `card`                          |   _String_   | <p>Optional<br>Format: ХХХХХХ******ХХХХ</p>              | <p>Card number mask<br><strong>Example:</strong> 411111******1111</p>                                                                                                                                                                                                                                                                                                      |
| `card_expiration_date`          |   _String_   | Optional                                                 | <p>Card expiration date<br><strong>Example:</strong> 12/2022</p>                                                                                                                                                                                                                                                                                                           |
| `payee_card`                    |   _String_   | Optional                                                 | <p>Payee card number mask<br>Returns only for transfer operation<br><strong>Example:</strong> 411111******1111</p>                                                                                                                                                                                                                                                         |
| `card_token`                    |   _String_   | Optional                                                 | <p>Card token. It is available if the parameter <code>req_token</code> was enabled<br>It is available only for purchase operation<br><strong>Example:</strong> VjFRaUxDSmhiR2NpT2lKU1V6STFO</p>                                                                                                                                                                            |
| `crypto_network`                |   _String_   | Optional                                                 | <p>Blockchain network where the transaction is taking place<br><strong>Example:</strong> ERC20</p>                                                                                                                                                                                                                                                                         |
| `crypto_address`                |   _String_   | Optional                                                 | <p>Public address of a cryptocurrency wallet<br><strong>Example:</strong> bc1QWXYZ8901ghijkLMNOPqrstv2345wxyzABCDE</p>                                                                                                                                                                                                                                                     |
| `customer_name`                 |   _String_   | Optional                                                 | <p>Customer's first and last name<br><strong>Example:</strong> John Rickher</p>                                                                                                                                                                                                                                                                                            |
| `customer_email`                |   _String_   | <p>Optional<br>Format: example@mail.com</p>              | <p>Customer's email address<br><strong>Example:</strong> fdfd@dfsds.ds</p>                                                                                                                                                                                                                                                                                                 |
| `customer_country`              |   _String_   | <p>Optional<br>Up to 3 characters</p>                    | <p>Customer's country<br><strong>Example:</strong> US</p>                                                                                                                                                                                                                                                                                                                  |
| `customer_state`                |   _String_   | <p>Optional<br>Up to 32 characters</p>                   | <p>Customer's state<br><strong>Example:</strong> California</p>                                                                                                                                                                                                                                                                                                            |
| `customer_city`                 |   _String_   | Optional                                                 | <p>Customer's city<br><strong>Example:</strong> Los Angeles</p>                                                                                                                                                                                                                                                                                                            |
| `customer_address`              |   _String_   | <p>Optional<br>Up to 32 characters</p>                   | <p>Customer's address<br><strong>Example:</strong> 123 Sample Street</p>                                                                                                                                                                                                                                                                                                   |
| `customer_ip`                   |   _String_   | Required                                                 | <p>Customer's IP<br><strong>Example:</strong> 255.41.45.57</p>                                                                                                                                                                                                                                                                                                             |
| `date`                          |    _Date_    | <p>Optional<br>Format: YYYY-MM-DD hh:mm:ss</p>           | <p>Transaction date<br><strong>Example:</strong> 2020-08-05 07:41:10</p>                                                                                                                                                                                                                                                                                                   |
| `recurring_init_trans_id`       |   _String_   | Optional                                                 | <p>Reference to the first transaction that initializes the recurring (provided if recurring was initialized)<br>It is available only for purchase operation<br><strong>Example:</strong> dc66cdd8-d702-11ea-9a2f-0242c0a87099</p>                                                                                                                                          |
| `recurring_token`               |   _String_   | Optional                                                 | <p>Recurring token (provided if recurring was initialized)<br><strong>Example:</strong> e5f60b35485e</p>                                                                                                                                                                                                                                                                   |
| `schedule_id`                   |   _String_   | Optional                                                 | <p>It is available if schedule is used for recurring sale<br>It is available only for purchase operation</p>                                                                                                                                                                                                                                                               |
| `exchange_rate`                 |   _Decimal_  | Optional                                                 | <p>Rate used to make exchange.<br>It returns if the currency exchange has been applied for the payment.</p>                                                                                                                                                                                                                                                                |
| `exchange_rate_base`            |   _Decimal_  | Optional                                                 | <p>The rate used in the double conversion to convert the original currency to the base currency.<br>It returns if the currency exchange has been applied for the payment.</p>                                                                                                                                                                                              |
| `exchange_currency`             | _Dictionary_ | Optional                                                 | <p>Original currency.<br>It returns if the currency exchange has been applied for the payment.</p>                                                                                                                                                                                                                                                                         |
| `exchange_amount`               |   _Integer_  | Optional                                                 | <p>Original amount.<br>It returns if the currency exchange has been applied for the payment.</p>                                                                                                                                                                                                                                                                           |
| `vat_amount`                    |    _Float_   | <p>Optional<br>Format: XX.XX, without leading zeroes</p> | <p>Product VAT<br>Returns if VAT has been calculated for the payment.<br>It is available only for purchase operation<br><strong>Example:</strong> 0.09</p>                                                                                                                                                                                                                 |
| `digital_wallet`                |   _String_   | Optional                                                 | <p>Wallet provider: googlepay, applepay.<br>It is returned if digital wallets were used for card payment creation.<br><strong>Example:</strong> googlepay</p>                                                                                                                                                                                                              |
| `pan_type`                      |   _String_   | Optional                                                 | <p>It refers to digital payments, such as Apple Pay and Google Pay, and the card numbers returned as a result of payment token decryption: DPAN (Digital Primary Account Number) and FPAN (Funding Primary Account Number).<br>Possible values: dpan, fpan.<br>It is returned if digital wallets were used in card payment creation.<br><strong>Example:</strong> dpan</p> |
| **custom\_data**                |   _Object_   | Optional                                                 | <p>Custom data<br>This block duplicates the arbitrary parameters that were passed in the payment request<br><strong>Example:</strong><br><code>“custom_data”: {“param1”:”value1”, “param2”:”value2”, “param3”:”value3”}</code></p>                                                                                                                                         |
| `hash`                          |   _String_   | Required                                                 | <p>Special signature, used to validate callback Addition in Signature section.<br><strong>Example:</strong> Must be SHA1 of MD5 encoded string (uppercased): payment_public_id + order.number + order.amount + order.currency + order.description + merchant.pass</p>                                                                                                      |

\*\* \* \*\* The parameters are included if the appropriate setup is configured in the admin panel (see “Add Extended Data to Callback” block in the Configurations -> Protocol Mappings section).

### Examples

<details>

<summary>Callback examples</summary>



</details>

## Recurring

***

Recurring payments are commonly used to create new transactions based on already stored cardholder information from previous operations.\
This request is sent by POST in JSON format.

**RECURRING request**

```
/api/v1/payment/recurring
```

### Request Parameters

| **Parameter**             | **Type** | **Mandatory, Limitations**                                                     | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| ------------------------- | :------: | ------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `merchant_key`            | _String_ | Required                                                                       | <p>Key for Merchant identification<br><strong>Example:</strong> xxxxx-xxxxx-xxxxx</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| `recurring_init_trans_id` | _String_ | Required                                                                       | <p>Transaction ID of the primary transaction in the Payment Platform<br><strong>Example:</strong> dc66cdd8-d702-11ea-9a2f-0242c0a87002</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| `recurring_token`         | _String_ | Required                                                                       | <p>Recurring token<br><strong>Example:</strong> 9a2f-0242c0a87002</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| `schedule_id`             | _String_ | Optional                                                                       | Schedule ID for recurring payments                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| `hash`                    | _String_ | Required                                                                       | <p>Special signature to validate your request to Payment Platform Addition in Signature section<br>Must be SHA1 of MD5 encoded string (uppercased): recurring_init_trans_id + recurring_token + order.number + order.amount + order.description + merchant_pass</p>                                                                                                                                                                                                                                                                                                                                                          |
| **order**                 | _Object_ |                                                                                |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| `number`                  | _String_ | <p>Required<br>Not blank<br>max: 255<br>[a-zA-Z0-9-]</p>                       | <p>Order ID<br><strong>Example:</strong> order-1234</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `amount`                  |  _Float_ | <p>Required<br>Not blank<br>Greater then 0<br>0-9<br>max: 255</p>              | <p>Format depends on currency.<br>Send Integer type value for currencies with zero-exponent. <strong>Example:</strong> 1000<br>Send Float type value for currencies with exponents 2, 3, 4.<br>Format for 2-exponent currencies: XX.XX <strong>Example:</strong> 100.99<br><strong>Pay attention</strong> that currencies 'UGX', 'JPY', 'KRW', 'CLP' must be send in the format XX.XX, with the zeros after comma. <strong>Example:</strong> 100.00<br>Format for 3-exponent currencies: XXX.XXX <strong>Example:</strong> 100.999.<br>Format for 4-exponent currencies: XXX.XXXX <strong>Example:</strong> 100.9999<br></p> |
| `description`             | _String_ | <p>Required<br>min: 2 max: 1024<br>[a-zA-Z0-9!"#$%&#x26;'()*+,./:;&#x26;@]</p> | <p>Product name<br><strong>Example:</strong> Very important gift - # 9</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |

### Response Parameters

| **Parameter** | **Type** | **Mandatory, Limitations**                     | **Description**                                                                                                                                                                                             |
| ------------- | :------: | ---------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `status`      | _String_ | Required                                       | <p>Payment status<br><strong>Example:</strong> SETTLED, PENDING, DECLINE</p>                                                                                                                                |
| `reason`      | _String_ | Optional                                       | <p>Decline reason translation for unsuccessful payment.<br>It displays only if the transaction is unsuccessful<br><strong>Example:</strong> The operation was rejected. Please contact the site support</p> |
| `payment_id`  | _String_ | <p>Required<br>Up to 255 characters</p>        | <p>Transaction ID (public)<br><strong>Example:</strong> dc66cdd8-d702-11ea-9a2f-0242c0a87002</p>                                                                                                            |
| `date`        | _String_ | <p>Required<br>Format: YYYY-MM-DD hh:mm:ss</p> | <p>Transaction date<br><strong>Example:</strong> 2020-08-05 07:41:10</p>                                                                                                                                    |
| `schedule_id` | _String_ | Optional                                       | Schedule ID for recurring payments                                                                                                                                                                          |
| **order**     | _Object_ |                                                |                                                                                                                                                                                                             |
| `number`      | _String_ | <p>Required<br>max: 255</p>                    | <p>Order ID<br><strong>Example:</strong> order-1234</p>                                                                                                                                                     |
| `amount`      | _String_ | <p>Required<br>Format: XX.XX</p>               | <p>Product price (currency will be defined by the first payment)<br><strong>Example:</strong> 0.19</p>                                                                                                      |
| `currency`    | _String_ | <p>Required<br>3 characters</p>                | <p>Currency<br><strong>Example:</strong> USD</p>                                                                                                                                                            |
| `description` | _String_ | <p>Required<br>max: 1024</p>                   | <p>Product name<br><strong>Example:</strong> Very important gift - # 9</p>                                                                                                                                  |

<details>

<summary>Example Request</summary>

```
<details>
<summary markdown="span">Recurring (settled)</summary>
```

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/payment/recurring' \
--header 'Content-Type: application/json' \
--data-raw '{ 
   "merchant_key":"xxxxx-xxxxx-xxxxx",
   "order":{
      "number":"order-1234",
      "amount": "0.19",
      "description":"very important gift"
},
    "customer": {
        "name": "John Doe",
        "email": "success@gmail.com"
    },     
       "recurring_init_trans_id":"dc66cdd8-d702-11ea-9a2f-0242c0a87002", 
       "recurring_token":"9a2f-0242c0a87002",
       "schedule_id":"57fddecf-17b9-4d38-9320-a670f0c29ec0", 
       "hash":"{{session_hash}}"
}'
```

</details>

<details>

<summary>Recurring (declined)</summary>

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/payment/recurring' \
--header 'Content-Type: application/json' \
--data-raw '{ 
    "payment_id": "1f34f446-fc45-11ec-a50f-0242ac120004",
    "date": "2022-07-05 09:30:34",
    "status": "decline",
    "reason": "Declined by processing.",
    "order": {
        "number": "order-1234",
        "amount": "3.01",
        "currency": "USD",
        "description": "bloodline"
    },
    "customer": {
        "name": "John Doe",
        "email": "success@gmail.com"
    }
}'
```

</details>

<details>

<summary>Example Response</summary>



</details>

## Retry

***

RETRY request is used to retry funds charging for secondary recurring payments in case of soft decline.

This action creates RETRY transaction and can cause payment final status changing.

This request is sent by POST in JSON format.

**RETRY request**

```
/api/v1/payment/retry
```

### Request parameters

| **Parameter**  | **Type** | **Mandatory, Limitations**              | **Description**                                                                                                                                                                     |
| -------------- | :------: | --------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `merchant_key` | _String_ | Required                                | <p>Key for Merchant identification<br><strong>Example:</strong> xxxxx-xxxxx-xxxxx</p>                                                                                               |
| `payment_id`   | _String_ | <p>Required<br>Up to 255 characters</p> | <p>Payment ID (public)<br><strong>Example:</strong> dc66cdd8-d702-11ea-9a2f-0242c0a87002</p>                                                                                        |
| `hash`         | _String_ | Required                                | <p>Special signature to validate your request to Payment Platform Addition in Signature section.<br>Must be SHA1 of MD5 encoded string (uppercased): payment_id + merchant_pass</p> |

<details>

<summary>Request example</summary>

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/payment/retry' \
--header 'Content-Type: application/json' \
--data-raw '{
   "merchant_key":"xxxxx-xxxxx-xxxxx",
   "payment_id":"63c781cc-de3d-11eb-a1f1-0242ac130006",
   "hash":"{{operation_hash}}"
}'
```

</details>

### Response Parameters

| **Parameter** | **Type** | **Mandatory, Limitations**              | **Description**                                                                              |
| ------------- | :------: | --------------------------------------- | -------------------------------------------------------------------------------------------- |
| `payment_id`  | _String_ | <p>Required<br>Up to 255 characters</p> | <p>Payment ID (public)<br><strong>Example:</strong> dc66cdd8-d702-11ea-9a2f-0242c0a87002</p> |
| `result`      | _String_ | Required                                | <p>Retry request result <br><strong>Example:</strong> accepted</p>                           |

<details>

<summary>Response example</summary>

```
{
  "payment_id": "63c781cc-de3d-11eb-a1f1-0242ac130006",
  "result": "accepted"
}
```

</details>

## Get transaction status

***

This request is sent by POST in JSON format.

To get order status you can use one the identifiers:

• `payment_id` – transaction ID in the Payment Platform\
\
• `order_id` – merchant’s transaction ID

```
/api/v1/payment/status
```

## GET\_TRANS\_STATUS by `payment_id`

> Note: The response logic for a status request depends on the Cascading Context for Get Status setting in Cofiguration --> Protocol Mappings section. If enabled, the system returns the status of the most recently created payment within the cascade (i.e., the payment with the latest creation date), rather than the payment specified in the request.

### Request Parameters by `payment_id`

| **Parameter**  | **Type** | **Mandatory, Limitations**              | **Description**                                                                                                                                                                     |
| -------------- | :------: | --------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `merchant_key` | _String_ | Required                                | <p>Key for Merchant identification<br><strong>Example:</strong> xxxxx-xxxxx-xxxxx</p>                                                                                               |
| `payment_id`   | _String_ | <p>Required<br>Up to 255 characters</p> | <p>Payment ID (public)<br><strong>Example:</strong> dc66cdd8-d702-11ea-9a2f-0242c0a87002</p>                                                                                        |
| `hash`         | _String_ | Required                                | <p>Special signature to validate your request to Payment Platform Addition in Signature section.<br>Must be SHA1 of MD5 encoded string (uppercased): payment_id + merchant_pass</p> |

### Response Parameters by `payment_id`

| **Parameter**     | **Type** | **Mandatory, Limitations**                     | **Description**                                                                                                                                                                                          |
| ----------------- | :------: | ---------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `payment_id`      | _String_ | <p>Required<br>Up to 255 characters</p>        | <p>Payment ID (public)<br><strong>Example:</strong> dc66cdd8-d702-11ea-9a2f-0242c0a87002</p>                                                                                                             |
| `date`            | _String_ | <p>Required<br>Format: YYYY-MM-DD hh:mm:ss</p> | <p>Transaction date<br><strong>Example:</strong> 2020-08-05 07:41:10</p>                                                                                                                                 |
| `status`          | _String_ | Required                                       | <p>Payment status: prepare, settled, pending, 3ds, redirect, decline, refund, reversal, void, chargeback<br><strong>Example:</strong> settled</p>                                                        |
| `reason`          | _String_ | Optional                                       | <p>Decline reason translation for unsuccessful payment. It displays only if the transaction is unsuccessful<br><strong>Example:</strong> The operation was rejected. Please contact the site support</p> |
| `recurring_token` | _String_ | Optional                                       | <p>Recurring token (provided if recurring was initialized)<br><strong>Example:</strong> e5f60b35485e</p>                                                                                                 |
| `shedule_id`      | _String_ | Optional                                       | <p>Schedule ID for recurring payments. Only for purchase operation<br><strong>Example:</strong> 57fddecf-17b9-4d38-9320-a670f0c29ec0</p>                                                                 |
| **order**         | _Object_ |                                                |                                                                                                                                                                                                          |
| `number`          | _String_ | <p>Required<br>Up to 255 characters</p>        | <p>Order ID<br><strong>Example:</strong> order-1234</p>                                                                                                                                                  |
| `amount`          | _String_ | <p>Required<br>Format: XX.XX</p>               | <p>Product price<br><strong>Example:</strong> 0.19</p>                                                                                                                                                   |
| `currency`        | _String_ | <p>Required<br>Up to 3 characters</p>          | <p>Currency<br><strong>Example:</strong> USD</p>                                                                                                                                                         |
| `description`     | _String_ | <p>Required<br>Up to 1024 characters</p>       | <p>Product name<br><strong>Example:</strong> Very important gift</p>                                                                                                                                     |
| **customer**      | _Object_ |                                                |                                                                                                                                                                                                          |
| `name`            | _String_ | Required                                       | <p>Customer's name<br><strong>Example:</strong> John Doe</p>                                                                                                                                             |
| `email`           | _String_ | Required                                       | <p>Customer's email address<br><strong>Example:</strong> example@mail.com</p>                                                                                                                            |
| `digital_wallet`  | _String_ | Optional                                       | <p>Wallet provider: googlepay, applepay. It returns if digital wallets were used for card payment creation.<br><strong>Example:</strong> googlepay</p>                                                   |

<details>

<summary>Example Request</summary>

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/payment/status' \
--header 'Content-Type: application/json' \
--data-raw '{  
   "merchant_key":"xxxxx-xxxxx-xxxxx",
   "payment_id":"63c781cc-de3d-11eb-a1f1-0242ac130006",
   "hash":"{{operation_hash}}"
}
'
```

</details>

<details>

<summary>Example Response</summary>



</details>

## GET\_TRANS\_STATUS by `order_id`

Use this request to get the status of the most recent transaction in the order's transaction subsequence.

It is recommended to send an unique order number (the `order.number` parameter) in the Authorization request. That way, it will be easier to uniquely identify the payment by `order_id`. This is especially important if cascading is configured. In this case, several intermediate transactions could be created within one payment.

### Request Parameters by `order_id`

| **Parameter**  | **Type** | **Mandatory, Limitations**              | **Description**                                                                                                                                                                   |
| -------------- | :------: | --------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `merchant_key` | _String_ | Required                                | <p>Key for Merchant identification<br><strong>Example:</strong> xxxxx-xxxxx-xxxxx</p>                                                                                             |
| `order_id`     | _String_ | <p>Required<br>Up to 255 characters</p> | <p>Merchant’s Order Number<br><strong>Example:</strong> dc66cdd8-d702-11ea-9a2f-0242c0a87002</p>                                                                                  |
| `hash`         | _String_ | Required                                | <p>Special signature to validate your request to Payment Platform Addition in Signature section.<br>Must be SHA1 of MD5 encoded string (uppercased): order_id + merchant_pass</p> |

### Response Parameters by `order_id`

| **Parameter**     | **Type** | **Mandatory, Limitations**                     | **Description**                                                                                                                                                                                          |
| ----------------- | :------: | ---------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `payment_id`      | _String_ | <p>Required<br>Up to 255 characters</p>        | <p>Transaction ID in Payment Platform<br><strong>Example:</strong> dc66cdd8-d702-11ea-9a2f-0242c0a87002</p>                                                                                              |
| `date`            | _String_ | <p>Required<br>Format: YYYY-MM-DD hh:mm:ss</p> | <p>Transaction date<br><strong>Example:</strong> 2020-08-05 07:41:10</p>                                                                                                                                 |
| `status`          | _String_ | <p>Required<br>Up to 20 characters</p>         | <p>Payment status: prepare, settled, pending, 3ds, redirect, decline, refund, reversal, void, chargeback<br><strong>Example:</strong> settled</p>                                                        |
| `reason`          | _String_ | <p>Optional<br>Up to 1024 characters</p>       | <p>Decline reason translation for unsuccessful payment. It displays only if the transaction is unsuccessful<br><strong>Example:</strong> The operation was rejected. Please contact the site support</p> |
| `recurring_token` | _String_ | Optional                                       | <p>Recurring token (provided if recurring was initialized)<br><strong>Example:</strong> e5f60b35485e</p>                                                                                                 |
| `shedule_id`      | _String_ | Optional                                       | <p>Schedule ID for recurring payments. Only for purchase operation<br><strong>Example:</strong> 57fddecf-17b9-4d38-9320-a670f0c29ec0</p>                                                                 |
| **order**         | _Object_ |                                                |                                                                                                                                                                                                          |
| `number`          | _String_ | <p>Required<br>Up to 255 characters</p>        | <p>Merchant’s Order ID<br><strong>Example:</strong> order-1234</p>                                                                                                                                       |
| `amount`          | _String_ | <p>Required<br>Format: XX.XX</p>               | <p>Product price (currency will be defined by the first payment)<br><strong>Example:</strong> 0.19</p>                                                                                                   |
| `currency`        | _String_ | <p>Required<br>Up to 3 characters</p>          | <p>Currency<br><strong>Example:</strong> USD</p>                                                                                                                                                         |
| `description`     | _String_ | <p>Required<br>Up to 1024 characters</p>       | <p>Product name or other additional information<br><strong>Example:</strong> Very important gift</p>                                                                                                     |
| **customer**      | _Object_ |                                                |                                                                                                                                                                                                          |
| `name`            | _String_ | Required                                       | <p>Customer's name<br><strong>Example:</strong> John Doe</p>                                                                                                                                             |
| `email`           | _String_ | Required                                       | <p>Customer's email address<br><strong>Example:</strong> example@mail.com</p>                                                                                                                            |
| `digital_wallet`  | _String_ | Optional                                       | <p>Wallet provider: googlepay, applepay.<br>It is returned if digital wallets were used for card payment<br><strong>Example:</strong> googlepay</p>                                                      |

<details>

<summary>Example Request</summary>

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/payment/status' \
--header 'Content-Type: application/json' \
--data-raw '{  
   "merchant_key":"xxxxx-xxxxx-xxxxx",
   "order_id":"63c781cc-de3d-11eb-a1f1-0242ac130006",
   "hash":"{{operation_hash}}"
}
'
```

</details>

<details>

<summary>Example Response</summary>



</details>

## Refund

***

To make refund you can use Refund request. Use payment public ID from Payment Platform in the request.

This request is sent by POST in JSON format.

**Refund request**

```
/api/v1/payment/refund
```

### Request Parameters

| **Parameter**  | **Type** | **Mandatory, Limitations**                               | **Description**                                                                                                                                                                              |
| -------------- | :------: | -------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `merchant_key` | _String_ | Required                                                 | <p>Key for Merchant identification<br><strong>Example:</strong> xxxxx-xxxxx-xxxxx</p>                                                                                                        |
| `payment_id`   | _String_ | <p>Required<br>Up to 255 characters</p>                  | <p>Payment ID (public)<br><strong>Example:</strong> dc66cdd8-d702-11ea-9a2f-0242c0a87002</p>                                                                                                 |
| `amount`       | _String_ | <p>Required<br>Format: XX.XX, without leading zeroes</p> | <p>Amount to refund. It is required for particular refund.<br><strong>Example:</strong> 0.19</p>                                                                                             |
| `hash`         | _String_ | Required                                                 | <p>Special signature to validate your request to Payment Platform Addition in Signature section.<br>Must be SHA1 of MD5 encoded string (uppercased): payment.id + amount + merchant.pass</p> |

### Response Parameters

| **Parameter** | **Type** | **Mandatory, Limitations**    | **Description**                                                                                  |
| ------------- | :------: | ----------------------------- | ------------------------------------------------------------------------------------------------ |
| `payment_id`  | _String_ | Required Up to 255 characters | <p>Transaction ID (public)<br><strong>Example:</strong> dc66cdd8-d702-11ea-9a2f-0242c0a87002</p> |
| `result`      | _String_ | Required                      | Refund request result **Example:** accepted                                                      |

<details>

<summary>Example Request</summary>

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/payment/refund' \
--header 'Content-Type: application/json' \
--data-raw '{  
   "merchant_key":"xxxxx-xxxxx-xxxxx",
   "payment_id":"63c781cc-de3d-11eb-a1f1-0242ac130006",
   "amount":"0.10",
   "hash":"{{operation_hash}}"
}
'
```

</details>

<details>

<summary>Example Response: Refund request accepted</summary>

```
{
  "payment_id": "63c781cc-de3d-11eb-a1f1-0242ac130006",
  "result": "accepted"
}
```

</details>

## Void

***

To make a void for an operation which was performed the same financial day you can use Void request.

The Void request is allowed for the payments in SETTLED status only.

Use payment public ID from Payment Platform in the request.

**Void request**

```
/api/v1/payment/void
```

### Request Parameters

| **Parameter**  | **Type** | **Mandatory, Limitations**    | **Description**                                                                                                                                                                     |
| -------------- | :------: | ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `merchant_key` | _String_ | Required                      | <p>Key for Merchant identification<br><strong>Example:</strong> xxxxx-xxxxx-xxxxx</p>                                                                                               |
| `payment_id`   | _String_ | Required Up to 255 characters | <p>Payment ID (public)<br><strong>Example:</strong> dc66cdd8-d702-11ea-9a2f-0242c0a87002</p>                                                                                        |
| `hash`         | _String_ | Required                      | <p>Special signature to validate your request to Payment Platform Addition in Signature section.<br>Must be SHA1 of MD5 encoded string (uppercased): payment.id + merchant.pass</p> |

### Response Parameters

| **Parameter** | **Type** | **Mandatory, Limitations**                     | **Description**                                                                                                                                                                                                                |
| ------------- | :------: | ---------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `status`      | _String_ | Required                                       | <p>Payment status<br><strong>Example:</strong> VOID / SETTLED</p>                                                                                                                                                              |
| `payment_id`  | _String_ | Required Up to 255 characters                  | <p>Transaction ID (public)<br><strong>Example:</strong> dc66cdd8-d702-11ea-9a2f-0242c0a87002</p>                                                                                                                               |
| `date`        | _String_ | <p>Required<br>Format: YYYY-MM-DD hh:mm:ss</p> | <p>Transaction date<br><strong>Example:</strong> 2020-08-05 07:41:10</p>                                                                                                                                                       |
| `reason`      | _String_ | Optional                                       | <p>Decline or error reason (for "sale", "void "and "refund" operation types). It displays only if the transaction has FAIL status<br><strong>Example:</strong> The operation was rejected. Please contact the site support</p> |
| **order**     | _Object_ |                                                |                                                                                                                                                                                                                                |
| `number`      | _String_ | Required                                       | <p>Order ID<br><strong>Example:</strong> order-1234</p>                                                                                                                                                                        |
| `amount`      | _String_ | Required                                       | <p>Product price<br><strong>Example:</strong> 0.19</p>                                                                                                                                                                         |
| `currency`    | _String_ | Required Up to 3 characters                    | <p>Currency<br><strong>Example:</strong> USD</p>                                                                                                                                                                               |
| `description` | _String_ | Required Up to 1024 characters                 | <p>Product name<br><strong>Example:</strong> Very important gift</p>                                                                                                                                                           |

<details>

<summary>Example Request</summary>

```
curl --location -g --request POST '{{CHECKOUT_HOST}}/api/v1/payment/void' \
--header 'Content-Type: application/json' \
--data-raw '{  
   "merchant_key":"xxxxx-xxxxx-xxxxx",
   "payment_id":"63c781cc-de3d-11eb-a1f1-0242ac130006",
   "hash":"{{operation_hash}}"
}
'
```

</details>

<details>

<summary>Example Response: Void successful</summary>

```
{
  "status": "SUCCESS", 
  "payment_status": "void",
  "payment_id": "dc66cdd8-d702-11ea-9a2f-0242c0a87002",
  "date": "2020-08-05 07:41:10",
  "order": {
    "number": "order-1234",
    "amount": "0.19",
    "currency": "USD",
    "description": "very important gift"
  }
}
```

</details>

<details>

<summary>Example Response: Void failed</summary>

```
{
  "status": "FAIL", 
  "payment_status": "settled",
  "payment_id": "dc66cdd8-d702-11ea-9a2f-0242c0a87002",
  "date": "2020-08-05 07:41:10",
  "reason": "Declined by processing",
   "order": {
    "number": "order-1234",
    "amount": "0.19",
    "currency": "USD",
    "description": "very important gift"
  }
}
```

</details>

## Signature

***

Sign is signature rule used either to validate your requests to payment platform or to validate callback from payment platform to your system.

### Authentication Signature

It must be SHA1 of MD5 encoded string and calculated by the formula below:

\*sha1(md5(strtoupper(id.order.amount.currency.description.PASSWORD)))

// Use the CryptoJSvar

#### Example for JS

```javascript
var to_md5 = order.number + order.amount + order.currency + order.description + merchant.pass;
```

// Use the CryptoJS

_var hash = CryptoJS.SHA1(CryptoJS.MD5(to\_md5.toUpperCase()).toString());_

_var result = CryptoJS.enc.Hex.stringify(hash);_

### Get Transaction Status Signature (by `payment_id`)

It must be SHA1 of MD5 encoded string and calculated by the formula below:

_hash = CryptoJS.SHA1(CryptoJS.MD5(to\_md5.toUpperCase()).toString());_

_\*sha1(md5(strtoupper))_

// Use the CryptoJSvar

#### Example for JS

```javascript
var to_md5 = payment.id + merchant.pass;
```

// Use the CryptoJS

_var hash = CryptoJS.SHA1(CryptoJS.MD5(to\_md5.toUpperCase()).toString());_

_var result = CryptoJS.enc.Hex.stringify(hash);_

### Get Transaction Status Signature (by `order_id`)

It must be SHA1 of MD5 encoded string and calculated by the formula below:

_hash = CryptoJS.SHA1(CryptoJS.MD5(to\_md5.toUpperCase()).toString());_

_\*sha1(md5(strtoupper))_

// Use the CryptoJSvar

#### Example for JS

```javascript
var to_md5 = order.id + merchant.pass;
```

// Use the CryptoJS

_var hash = CryptoJS.SHA1(CryptoJS.MD5(to\_md5.toUpperCase()).toString());_

_var result = CryptoJS.enc.Hex.stringify(hash);_

### Refund Signature

It must be SHA1 of MD5 encoded string and calculated by the formula below:

_hash = CryptoJS.SHA1(CryptoJS.MD5(to\_md5.toUpperCase()).toString()); \*sha1(md5(strtoupper))_

// Use the CryptoJSvar

#### Example for JS

```javascript
var to_md5 = payment.id + amount + merchant.pass;
```

// Use the CryptoJS

_var hash = CryptoJS.SHA1(CryptoJS.MD5(to\_md5.toUpperCase()).toString());_

_var result = CryptoJS.enc.Hex.stringify(hash);_

### Void Signature

It must be SHA1 of MD5 encoded string and calculated by the formula below:

_hash = CryptoJS.SHA1(CryptoJS.MD5(to\_md5.toUpperCase()).toString()); \*sha1(md5(strtoupper))_

// Use the CryptoJSvar

#### Example for JS

```javascript
var to_md5 = payment.id + merchant.pass;
```

// Use the CryptoJS

_var hash = CryptoJS.SHA1(CryptoJS.MD5(to\_md5.toUpperCase()).toString());_

_var result = CryptoJS.enc.Hex.stringify(hash);_

### Recurring Signature

It must be SHA1 of MD5 encoded string and calculated by the formula below:

\*sha1(md5(strtoupper(recurring\_init\_trans\_id.recurring\_token.order\_id.amount.description.merchant.pass)))

// Use the CryptoJS

#### Example for JS

```javascript
var to_md5 = recurring_init_trans_id + recurring_token + order.number + order.amount + order.description + merchant.pass;
```

// Use the CryptoJS

var hash = CryptoJS.SHA1(CryptoJS.MD5(to\_md5.toUpperCase()).toString());

var result = CryptoJS.enc.Hex.stringify(hash);

### Signature for return

It must be SHA1 of MD5 encoded string and calculated by the formula below:

_hash = CryptoJS.SHA1(CryptoJS.MD5(to\_md5.toUpperCase()).toString());_\
\*sha1(md5(strtoupper(payment\_public\_id.order\_id.amount.currency.description.PASSWORD)))

Example for JS

```javascript
var to_md5 = payment_public_id + order.number + order.amount + order.currency + order.description + merchant.pass;
```

// Use the CryptoJS

_var hash = CryptoJS.SHA1(CryptoJS.MD5(to\_md5.toUpperCase()).toString());_

_var result = CryptoJS.enc.Hex.stringify(hash);_

### Callback Signature

It must be SHA1 of MD5 encoded string and calculated by the formula below:

_hash = CryptoJS.SHA1(CryptoJS.MD5(to\_md5.toUpperCase()).toString()); \*sha1(md5(strtoupper(payment\_public\_id.order\_id.amount.currency.description.PASSWORD)))_

#### Example for JS

```javascript
var to_md5 = payment_public_id + order.number + order.amount + order.currency + order.description + merchant.pass;
```

// Use the CryptoJS

_var hash = CryptoJS.SHA1(CryptoJS.MD5(to\_md5.toUpperCase()).toString());_

_var result = CryptoJS.enc.Hex.stringify(hash);_

## Testing

***

You can test your integration. To do the test you need to perform the actions with the API using (in test mode). For instance, send the request and receive the response with a link on the Checkout page.\
To mark your transactions as test in the system, you should use Merchant Test Key as value of the `merchant_key` parameter. You can contact your administrator to make sure that you have the necessary settings to work with test transactions.

Use the following test data to emulate the different scenarios.

**Scenario: SUCCESS payment**

**Card data:**

`Card number` 4111 1111 1111 1111\
`Expiry Date` 01/38\
`CVV2` any 3 digits

⚠️ Use that card data to create recurring payments and get a recurring token for the initial recurring. Testing recurring payments does not work with other test cards.

**Result:** customer is redirected to the "success\_URL"

**Scenario: FAILED payment (decline)**

**Card data:**

`Card number` 4111 1111 1111 1111\
`Expiry Date` 02/38\
`CVV2` any 3 digits

**Result:** customer gets an error message: Declined by processing.

**Scenario: SUCCESS 3DS payment**

**Card data:**

`Card number` 4111 1111 1111 1111\
`Expiry Date` 05/38\
`CVV2` any 3 digits

**Result:** customer is redirected to the "success\_URL" after 3DS verification

**Scenario: FAILED 3DS payment**

**Card data:**

`Card number` 4111 1111 1111 1111\
`Expiry Date` 06/38\
`CVV2` any 3 digits

**Result:** customer gets unsuccessful sale after 3DS verification

**Scenario: FAILED payment (Luhn algorithm)**

**Card data:**

`Card number` 1111 2222 3333 4444\
`Expiry Date` 01/38\
`CVV2` any 3 digits

**Result:** customer gets an error message: Bad Request. Brand of card does not support.

##
