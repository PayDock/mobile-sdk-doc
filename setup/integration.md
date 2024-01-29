# Integrate the Widgets into your application

You have downloaded and initialized the SDK dependency to your app or project during the installation stage of this setup. This guide demonstrates some example flows that integrate the SDK's widgets into your project.   

## Card payments

You can use a combination of widgets to orchestrate full card payments. To help you understand the process, the entire payment flow is implemented in the "Example App" that is packaged with the MobileSDK.

For this flow you need:

- The [3DS widget](/widgets/threeds)

- The [Card tokenisation widget](/widgets/card).

### Step 1

Use the SDK's [Card tokenisation widget](/widgets/card) to tokenise your customer's card details and receive the card one time token (OTT).

### Step 2

Convert the card OTT into a session vault token by calling ``/vault/payment_sources ``:

```
POST /v1/vault/payment_sources
Host: api-sandbox.paydock.com
x-user-secret-key: <Your secret key>
Content-Type: application/json;charset=utf-8

{"token":"<Your card OTT>","vault_type":"session"}
``` 

### Step 3

Create a 3DS token using the ``charges/3ds`` endpoint.

```
POST /v1/charges/3ds? HTTP/1.1
Host: api-sandbox.paydock.com
x-user-public-key: <Your public key>
Content-Type: application/json;charset=utf-8

{
  "currency": <ISO 4217 Currency (for example USD, AUD)>,
  "_3ds":{
     "browser_details":{
        "screen_height":"640",
        "screen_width":"480",
        "time_zone":"273",
        "language":"en-US",
        "name":"chrome",
        "color_depth":"24",
        "java_enabled":"true"
     }
  },
  "customer":{
     "payment_source":{
        "gateway_id": <Your gateway ID>,
        "vault_token": <Vault token you received from previous step>
     }
  },
  "amount": <Amount to charge>
}
``` 

### Step 4

If the ``authentication_not_supported`` is returned, move to step 5. If ``pre_authentication_pending`` is returned, use the SDK's integrated [3DS widget](/widgets/threeds) with the vault token to perform a 3DS challenge.

### Step 5

Capture the charge:

``` 
POST /v1/charges? HTTP/1.1
Host: api-sandbox.paydock.com
Content-Type: application/json;charset=utf-8
x-user-secret-key: <Your secret key>

{
  "currency": <ISO 4217 Currency (for example USD, AUD)>,
  "amount": <Amount to charge (String)>,
  "customer":{
     "payment_source":{
        "vault_token": <Received vault token>
     }
  }
}
``` 

## PayPal

The steps for using your PayPal widget in your application are as follows:

### Step 1

After your customer taps on the PayPal Widget button, you can initialize the wallet charge using the ``/charges/wallet`` endpoint, and pass the received token back into the widget using the provided callback.

``` 
POST /v1/charges/wallet?capture=true HTTP/1.1
Host: api-sandbox.paydock.com
x-user-secret-key: <Your secret key>
Content-Type: application/json;charset=utf-8

{
  "description":"Test transaction for PayPal",
  "customer":{
     "last_name":"Smith",
     "first_name":"John",
     "phone":"+11234567890",
     "email":"john.smith@example.com",
     "payment_source":{
        "address_postcode":"Some postcode",
        "gateway_id": <Your gateway ID>,
        "address_line1":"Some address"
     }
  },
  "currency": <ISO 4217 Currency (for example USD, AUD)>,
  "amount": <Amount to charge (Decimal)>,
  "meta":{
     "store_name": <Ypur store name>,
     "merchant_name": <Your merchant name>,
     "store_id": <Your store ID (String)>
  },
  "reference": <Transaction reference ID (String)>
}
```

### Step 2

After the PayPal widget flow has been completed, handle the success/failure result that you receive from the PayPal Widget.

## ApplePay

Steps for using your ApplePay widget in your application are as follows:

### Step 1

After your customer taps on the ApplePay Widget button, initialize the wallet charge using the ``/charges/wallet`` endpoint.

``` 
POST /v1/charges/wallet?capture=true HTTP/1.1
Host: api-sandbox.paydock.com
x-user-secret-key: <Your secret key>
Content-Type: application/json;charset=utf-8

{
  "description":"Test transaction for ApplePay",
  "customer":{
     "last_name":"Smith",
     "first_name":"John",
     "phone":"+11234567890",
     "email":"john.smith@example.com",
     "payment_source":{
        "address_postcode":"Some postcode",
        "gateway_id": <Your gateway ID>,
        "address_line1":"Some address"
     }
  },
  "currency": <ISO 4217 Currency (for example USD, AUD)>,
  "amount": <Amount to charge (Decimal)>,
  "meta":{
     "store_name": <Ypur store name>,
     "merchant_name": <Your merchant name>,
     "store_id": <Your store ID (String)>
  },
  "reference": <Transaction reference ID (String)>
}
```

### Step 2

Create the ``ApplePayRequest`` that contains the received token and ``PKPaymentRequest`` that you can generate using the helper function that the SDK provides. [refer to the ApplePay Widget documentation](/digital-wallet-widgets/applepay) for more information.

### Step 3

Pass the generated ``ApplePayRequest`` back into the widget using the provided widget callback. When the widget is finished handle the success/failure result.

## Google Pay

Steps to use your GooglePay Widget in your application are as follows:

### Step 1

Create the ``IsReadyToPayRequest`` JSONObject to specify additional filtering criteria for determining if your customer is considered as ready to pay. You can generate this using the helper util that the SDK provides [refer to the Google Pay Widget documentation](/digital-wallet-widgets/googlepay).
Reference: [IsReadyToPay](https://developers.google.com/android/reference/com/google/android/gms/wallet/IsReadyToPayRequest)

### Step 2

Create the ``PaymentDataRequest`` JSONObject. This provides the necessary information to support a payment. You can generate this using the helper util that the SDK provides [refer to the Google Pay Widget documentation](/digital-wallet-widgets/googlepay).
Reference: [PaymentDataRequest](https://developers.google.com/android/reference/com/google/android/gms/wallet/PaymentDataRequest)

### Step 3

After your customer taps on a Google Pay Widget button, initialize the wallet charge using the ``/charges/wallet`` endpoint.

``` 
POST /v1/charges/wallet?capture=true HTTP/1.1
Host: api-sandbox.paydock.com
x-user-secret-key: <Your secret key>
Content-Type: application/json;charset=utf-8

{
  "description":"Test transaction for Google Pay",
  "customer":{
     "last_name":"Smith",
     "first_name":"John",
     "phone":"+11234567890",
     "email":"john.smith@example.com",
     "payment_source":{
        "address_postcode":"Some postcode",
        "gateway_id": <Your gateway ID>,
        "address_line1":"Some address"
     }
  },
  "currency": <ISO 4217 Currency (for example USD, AUD)>,
  "amount": <Amount to charge (Decimal)>,
  "meta":{
     "store_name": <Ypur store name>,
     "merchant_name": <Your merchant name>,
     "store_id": <Your store ID (String)>
  },
  "reference": <Transaction reference ID (String)>
}
```

### Step 4

After the Google Pay widget is finished, handle the success/failure result that you receive from the Google Pay Widget.