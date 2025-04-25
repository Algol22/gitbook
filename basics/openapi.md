---
icon: network-wired
---

# OpenAPI

You can sync GitBook pages with an OpenAPI or Swagger file or a URL to include auto-generated API methods in your documentation.

```csharp
```

{% tabs %}
{% tab title="HTTP" %}
```http
POST /api/v1/session HTTP/1.1
Host: dev2-checkout.rafinita.com
Content-Type: application/json
Cookie: PHPSESSID=p49dvrcr0kdqp5hvoqevc12tur
Content-Length: 750

{  
   "merchant_key":"7879b6e4-f284-11ef-91ba-0242ac120005",
   "operation":"purchase",
   "methods":[
 "dl"
   ],
    "parameters": {
    "dl": {
      "document_number": "11934254096"
    }
  },
   "order":{
      "number":"order-1234",
      "amount": "1.19",
      "currency":"BRL",
      "description":"Important gift"
   },
   "cancel_url":"https://example.com/cancel",
   "success_url":"https://example.com/success",
   "customer":{
      "name":"John Doe",
      "email":"test@email.com"
   },
   "billing_address":{
      "country":"BR",
      "state": "CA",
      "city":"Los Angeles",
      "address":"Moor Building 35274",
      "zip":"123456",
      "phone":"347771112233"
     },
   "hash":"f9913870ba7336a777c1fb034dc578735e021407"
}

```
{% endtab %}
{% endtabs %}

###

{% openapi-operation spec="authentication" path="/api/v1/session" method="post" %}
[Broken link](broken-reference)
{% endopenapi-operation %}

### OpenAPI block

GitBook's OpenAPI block is powered by [Scalar](https://scalar.com/), so you can test your APIs directly from your docs.

{% openapi src="https://petstore3.swagger.io/api/v3/openapi.json" path="/pet" method="post" %}
[https://petstore3.swagger.io/api/v3/openapi.json](https://petstore3.swagger.io/api/v3/openapi.json)
{% endopenapi %}

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const message = "hello world";
console.log(message);
```
{% endtab %}

{% tab title="Python" %}
```python
message = "hello world"
print(message)
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
message = "hello world"
puts message
```
{% endtab %}
{% endtabs %}

{% openapi-operation spec="andriys-organization-api" path="/post" method="post" %}
[Broken link](broken-reference)
{% endopenapi-operation %}
