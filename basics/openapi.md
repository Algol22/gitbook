---
icon: network-wired
---

# OpenAPI

You can sync GitBook pages with an OpenAPI or Swagger file or a URL to include auto-generated API methods in your documentation.

```csharp
```

{% tabs fullWidth="true" %}
{% tab title="Java OkHttp" %}
```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("text/plain");
RequestBody body = new MultipartBody.Builder().setType(MultipartBody.FORM)
  .addFormDataPart("action","SALE")
  .addFormDataPart("client_key","09f6e0a8-ff62-11ef-8b15-0242ac120006")
  .addFormDataPart("order_id","1745608325withdrawal")
  .addFormDataPart("order_amount","1.01")
  .addFormDataPart("order_currency","USD")
  .addFormDataPart("order_description","wine")
  .addFormDataPart("card_number","4601541833776519")
  .addFormDataPart("card_exp_month","01")
  .addFormDataPart("card_exp_year","2025")
  .addFormDataPart("card_cvv2","123")
  .addFormDataPart("payer_first_name","John")
  .addFormDataPart("payer_last_name","Rickher")
  .addFormDataPart("payer_middle_name","Patronimicffffffffff")
  .addFormDataPart("payer_birth_date","1970-10-10")
  .addFormDataPart("payer_address","Shevchenko 1")
  .addFormDataPart("payer_address2","address")
  .addFormDataPart("payer_country","UA")
  .addFormDataPart("payer_state","Kyiv")
  .addFormDataPart("payer_city","Kyiv")
  .addFormDataPart("payer_zip","1234")
  .addFormDataPart("payer_email","test@gmail.com")
  .addFormDataPart("payer_phone","+38981234567")
  .addFormDataPart("payer_ip","23.129.64.182")
  .addFormDataPart("term_url_3ds","https://asdf.com")
  .addFormDataPart("hash","0efabb55d7faa9ed0ca7a0ff9267617e")
  .build();
Request request = new Request.Builder()
  .url("https://dev2-api-dev.rafinita.com/post")
  .method("POST", body)
  .build();
Response response = client.newCall(request).execute();
```
{% endtab %}

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
