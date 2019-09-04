## 3DS Redirect URL Helper For Midtrans Card Transaction

On Card transaction, 3DS `redirect_url` from Midtrans may be tricky to open in iframe/popup mode, or the platform you use to integrate does not support that method.

> Midtrans JS library actually support `redirect` method. But the transaction result after on the final redirect, is wrapped inside POST http request, which can be tricky to receive the result.

This HTML file is intended to be a "bridge", so you can open the 3DS `redirect_url` by redirecting to this HTML, and then finally receive the transaction status as final redirect with GET query string.

## Usage

Host this `index.html` file on your website, no backend needed, just serve it as static html page (All logic will run on client side javascript). For example the url is `https://myweb.com/open3ds/`

> Or use the online hosted version at https://mid-open-three-ds.netlify.com

Then redirect user to `https://<url-where-this-html-hosted>/?finish_url=<your-desired-finish-url>&otp_url=<redirect_url-from-midtrans>`

example:
```
https://mid-open-three-ds.netlify.com/?finish_url=https://example.com/finish/&otp_url=https://api.sandbox.veritrans.co.id/v2/token/rba/redirect/481111-1114-bb53fd68-8a9e-47ed-a27c-2b0815ec0d8a
```

After transaction complete/fail your customer will be redirected to `finish_url` with all the transaction data as GET query param (URL encoded).

example:
```
http://example.com/finish/
?status_code=200
&status_message=Success%2C+Credit+Card+transaction+is+successful
&transaction_id=f933e8a5-f30f-48b6-b1b2-57052e6a5fdc
&order_id=order-php-d-1567586053
&gross_amount=100000.00
&currency=IDR
&payment_type=credit_card
&transaction_time=2019-09-04+15%3A34%3A14
&transaction_status=capture
&fraud_status=accept
&approval_code=1567586094009
&eci=05
&masked_card=481111-1114
&bank=bni
&card_type=credit
&installment_term=12
&channel_response_code=00
&channel_response_message=Approved
```

Then for example on your finish url, you can get the value of `transaction_status` to show success/fail message to user.

### Available Parameter

- `finish_url`: finish url implemented on your website, this is where your customer will be redirected after 3DS
- `otp_url`: 3DS redirect_url from Midtrans API

> Tips: you should encodeUrl those value, to be safe.

#### Notes

- This is not official tools supported or created by Midtrans
- This is just a helper sample, which you can further implement to suit your needs