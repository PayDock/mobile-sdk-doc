# Generating your Wallet Token

Generating your wallet token and embedding the wallet button are mandatory steps to use the GooglePay, ApplePay, FlyPay and PayPal widgets. 

## Overview

When the customer taps the Payment button for your Digital Wallet, the SDK triggers a callback to the Merchant app requesting a `wallet_token`. You must perform a wallet initialization request to receive your `wallet_token`. For the request you must:

1. Ensure that you have your `x-user-secret-key`. You can generate this by following the instructions in our [integration guide](https://docs.paydock.com/#getting-started). 

2. Add the customer details to your request body. The example uses a demo customer, *Wanda Mertz*. 

3. Add the relevant `"<gateway_id>"` to the `"payment_source"` section.

```json
{ 
    "customer": {
        "first_name": "Wanda",
        "last_name": "Mertz",
        "email": "wanda.mertz@example.com",
        "phone": "+1234567890",
        "payment_source": {
            "gateway_id": "<gateway_id>"
        }
    },
    "amount": 10,
    "currency": "AUD",
    "reference": "reference123",
    "description": "description123",
    "meta": {
        "store_name": "123",
        "store_id": "123"
    }
}
```

```json
{
    "status": 201,
    "error": null,
    "resource": {
        "type": "charge",
        "data": {
            "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjYwZjU0NGZlOGVmZDZiNWViMTU4MjczOSIsIm1ldGEiOiJleUp0WlhSaElqcDdJbU5vWVhKblpTSTZleUpwWkNJNklqWXdaalUwTkdabE5tTTBOREl5TldWaFpXUXdaVEprWkNJc0ltRnRiM1Z1ZENJNk1UQXNJbU4xY25KbGJtTjVJam9pUVZWRUlpd2lZMkZ3ZEhWeVpTSTZkSEoxWlgwc0ltZGhkR1YzWVhraU9uc2lkSGx3WlNJNklrWnNlWEJoZVNJc0ltMXZaR1VpT2lKMFpYTjBJbjE5ZlE9PSIsImlhdCI6MTYyNjY4NjcxOCwiZXhwIjoxNjI2NzczMTE4fQ.-hi56NjusbZVZwickfFrcYHvWvbLIcWz6Wc5zsMtZko",
            "charge": {
                "_id": "60f544fe6c44225eaed0e2dd",
                "amount": 10,
                "currency": "AUD",
                "reference": "reference123",
                "description": "description123",
                "status": "wallet_initialized",
                "capture": true,
                "one_off": true,
                "amount_surcharge": null,
                "amount_original": null,
                "created_at": "2021-07-19T09:25:18.782Z",
                "updated_at": "2021-07-19T09:25:18.782Z",
                "transactions": [],
                "customer": {
                    "last_name": "Mertz",
                    "first_name": "Wanda",
                    "email": "wanda.mertz@example.com",
                    "phone": "+1234567890",
                    "payment_source": {
                        "type": "wallet",
                        "wallet_type": "Flypay",
                        "gateway_id": "60e84f19a7cc2c2e523b6c99",
                        "gateway_name": "Flypay",
                        "gateway_type": "Flypay"
                    }
                },
                "meta": {
                    "store_id": "123",
                    "store_name": "123"
                },
                "shipping": {
                    "options": []
                }
            }
        }
    }
}
```

