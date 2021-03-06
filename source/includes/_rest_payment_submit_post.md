### POST payment/submit


> Sandbox URL:

```
POST https://sandbox.gestpay.net/api/v1/payment/submit
```


> Production URL: 

```
POST https://ecomms2s.sella.it/api/v1/payment/submit
```


> Request Body: 

```json
{  
  "shopLogin":"",
  "paymentType":"",
  "buyer":{  
    "name":"",
    "email":""
  },
  "paymentTypeDetails":{  
    "creditcard":{
      "number":"",
      "token":"",
      "expMonth":"",
      "expYear":"",
      "CVV":"",
      "requestToken":"",
      "DCC": ""
    },
    "amazonPay":{  
      "amazonReferenceOrderId":"",
      "requestToken":"",
      "token":""
    },
    "applePay":{  
      "PKPaymentToken":"",
      "onlinePaymentCryptogram":"",
      "eciIndicator":"",
      "requestToken":"",
      "token":""
    },
    "bancomatPay": {
      "phoneNumber": ""
    },
    "googlePay": {
      "tokenizationData": {
        "token": {}
      }
    },
    "myBank":{  
      "bankCode":""
    },
    "iDeal":{  
      "bankCode":""
    },
    "payPal":{  
      "token":""
    }
  },
  "responseURLs":{  
    "buyerOK":"",
    "buyerKO":"",
    "serverNotificationURL":""
  },
  // SEPA
  "slimPay":{  
    "token":"SP7NYQTXYKG5CQFM",
    "executionDate":"20201212"
  },
}
```

<aside class="warning" style="font-size: 18px; font-weight: bold;">Payments with Google Pay may currently be declined.<br>
We are working to restore normal operation of the service.</aside>

Enables merchants to perform authorization requests for all the payment methods enabled for the merchant.

Before calling this method, you MUST have already called [POST payment/create](#post-payment-create). 

In case of a payment method that requires a widget (e.g. AmazonPay) it will provide the URL and the dimensions of the iFrame. 

In case the payment method requires a selected redirect it will provide the URL to redirect the customer.

Note: Redirected url will require merchant return url and transkey.

This method will handle payment triangulations (e.g. 3DSecure, PayPal, S2Pay, etc.) through transkey and method related tokens in the 2nd calls.

In order to display transaction results to the user without requesting the server to retrieve it through paymentResult it should return it in the response payload.


#### Request 

Headers: 

| Header          | Value                         | Description                                                        |
| --------------- | ----------------------------- | ------------------------------------------------------------------ |
| [`paymentToken`](#payment-token) | `{PaymentToken}` | The payment token, created via [payment/create](#post-payment-create) | 


Request Body: 

(Fields in **bold** are mandatory)

| Field | Description 
| -------------- | -----------
| **`shopLogin`** | the merchant's code 
| `paymentType` | the payment method chosen by the user 
| `buyer.name` | The buyer's name 
| `buyer.email` | The buyer's email
| `paymentTypeDetails` |  based on the chosen payment method, it will contain informations to complete the payment. See next sections. 
| `responseURLs` | where to redirect the user after the payment. 
| `executionDate` | Date of charge on the buyer’s banking account. (SEPA)

<br>

`executionDate` allows the merchant to define the date of charge on the buyer’s banking account. To follow SEPA rules, if the date it’s not on a working day, the charge will be postponed to the next working day. It must be in `yyyymmdd` format

<br>

<aside class="warning">You should fill out at least one payment method.</aside>
 

##### `paymentTypeDetails`: Credit card 

If the customer has chosen the credit card payment type, fill out these fields:

| Field | Description | 
| ----- | ----------- | 
| `number` | The credit card number
| `token` | The credit card token
| `expMonth` | The expiry expMonth
| `expYear` | The expiry year 
| `CVV` | The CVV code 
| `requestToken` | `MASKEDPAN` for a Standard Token; any other value for Custom Token
| `DCC` | if the payment uses currency conversion. `True` or `False`


##### `paymentTypeDetails`: Amazon Pay

To pay with Amazon Pay, fill out the fields below: 

| Field | Description | 
| ----- | ----------- | 
| `amazonReferenceOrderId` | The order reference identifier retrieved from the Amazon Button widget. 
| `requestToken` | `MASKEDPAN` for a Standard Token; any other value for Custom Token
| `token` | A token representing the AmazonPay token. 

##### `paymentTypeDetails`: Apple Pay

If your customers choose Apple Pay, these are the parameters to pass: 

| Field | Description | 
| ----- | ----------- | 
| `PKPaymentToken` | An object that contains the user's payment credentials. 
| `onlinePaymentCryptogram` |  a unique, one time use cryptogram used by Axerve E-commerce Solutions to decrypt the payment data
| `eciIndicator` | optional data used for 3D secure transactions
| `requestToken` | `MASKEDPAN` for a Standard Token; any other value for Custom Token
| `token` | a token representing the ApplePay token 

##### `paymentTypeDetails`: Bancomat Pay

If your customers choose Bancomat Pay, `bancomatPay` must be added 


<!-- ##### `paymentTypeDetails`: Google Pay -->
<!--  -->
<!-- If your customers choose Google Pay, these are the parameters to pass:  -->
<!--  -->
<!-- | Field | Description |  -->
<!-- | ----- | ----------- |  -->
<!-- | `token` | a token representing the Google Pay token  -->
<!--  -->
<!-- You can find more information in the [documentation](https://docs.gestpay.it/soap/alternative-payments/google-pay/) -->


##### `paymentTypeDetails`: My Bank

| Field | Description | 
| ----- | ----------- | 
| `bankCode` | A MyBank bank code 

##### `paymentTypeDetails`: iDeal

| Field | Description | 
| ----- | ----------- | 
| `bankCode` | An iDeal bank code 

##### `paymentTypeDetails`: Paypal

| Field | Description | 
| ----- | ----------- | 
| `token` |  A token representing the Paypal token. 

##### Response URLs 

With the REST Api you can set the redirect url for the success or failure case, along with the Server to server notification URL. 

| Field | Description | 
| ----- | ----------- | 
| `buyerOK` | The URL where the user will be redirected in case of success.
| `buyerKO` | The URL where the user will be redirected in case of failure. 
| `serverNotificationURL` | Axerve E-commerce Solutions will try to reach this URL to notify the merchant of the payment result.


#### Response 

> Success response for a non 3D credit card:<br>
> `200 OK`

```json
{
  "error":{  
    "code":"0",
    "description":"request correctly processed"
  },
  "payload": {
    "transactionType" : "submit",
    "transactionResult" :"OK",
    "transactionErrorCode" :"",   
    "transactionErrorDescription" :"",
    "bankTransactionID" : "11111111",
    "shopTransactionID":"myshoptransactionID",
    "authorizationCode":"1223321",
    "paymentID":"000000001",
    "currency":"EUR",
    "country":"ITALY",
    "company":"VISA",
    "tdLevel":"NULL",
    "buyer":{
      "name":"buyerName Submitted",
      "email":"buyer@SubmittedEmail.com"
    },
    "risk":{
      "riskResponseCode":"",
      "riskResponseDescription":""
    }
    "customInfo":{
      "myCustomInfo1":"myCustomInfoValue1",
      "myCustomInfo2":"myCustomInfoValue2"
    },
    "alertCode":"",
    "alertDescription":"",
    "cvvPresent":"",
    "maskedPAN":"",
    "paymentMethod":"",
    "productType":"",
    "token":"444444xxxxxx4448",
    "tokenExpiryMonth":"01",
    "tokenExpiryYear":"25",
    "userRedirect":{
      "href": "https://ecomm.sella.it/pagam/pagam3d.aspx?a={shopLogin}&b={paymentToken}"
    }
  }
}
```

> Success response for a 3D credit card:<br>
> `200 OK`

```json
{
  "error":{  
    "code":"0",
    "description":"request correctly processed"
  },
  "payload": {
    "transactionType":"submit",
    "transactionResult":"",
    "transactionErrorCode" :"8006",   
    "transactionErrorDescription" :"Verify By Visa",
    "bankTransactionID":"",
    "shopTransactionID":"myshoptransactionID",
    "authorizationCode":"",
    "paymentID":"00000000001",
    "currency":"",
    "country":"",
    "company":"",
    "tdLevel":"",
    "buyer":{
      "name":"",
      "email":""
    },
    "risk":{
      "riskResponseCode":"",
      "riskResponseDescription":""
    },
    "customInfo":{

    },
    "alertCode":"",
    "alertDescription":"",
    "cvvPresent":"",
    "maskedPAN":"",
    "paymentMethod":"",
    "productType":"",
    "token":"",
    "tokenExpiryMonth":"",
    "tokenExpiryYear":"",
    "userRedirect":{
      "href":"https://ecomm.sella.it/pagam/pagam3d.aspx?a={shopLogin}&b={paymentToken}"
    }
  }
}
```

> Success response for a redirect-based payment method (i.e. Paypal):<br>
> `200 OK`

```json
{
  "error":{  
    "code":"0",
    "description":"request correctly processed"
  },
  "payload": {
    "transactionType":"submit",
    "transactionResult":"",
    "transactionErrorCode" :"",   
    "transactionErrorDescription" :"",
    "bankTransactionID":"",
    "shopTransactionID":"myshoptransactionID",
    "authorizationCode":"",
    "currency":"",
    "country":"",
    "company":"",
    "tdLevel":"",
    "buyer":{
      "name":"",
      "email":""
    },
    "risk":{
      "riskResponseCode":"",
      "riskResponseDescription":""
    },
    "customInfo":{

    },
    "alertCode":"",
    "alertDescription":"",
    "cvvPresent":"",
    "maskedPAN":"",
    "paymentMethod":"PayPal",
    "productType":"",
    "token":"",
    "tokenExpiryMonth":"",
    "tokenExpiryYear":"",
    "userRedirect":{
      "href":"https://ecomm.sella.it/pagam/pagam3d.aspx?a={shopLogin}&b={paymentToken}&paymentType=S2PX"
    }
  }
}
```

See the section [Handling responses & errors](#handling-responses-amp-errors) to learn how Axerve E-commerce Solutions reports errors.

Response `payload` details:


| Field          | Description 
| -------------- | -----------
| `transactionType` | Always `submit` for this method.
| `transactionResult` | `OK` or `KO`
| `transactionErrorCode` | The [error code](#error-code), in case of errors 
| `transactionErrorDescription` | A description in common language of the occurred error. 
| `bankTransactionID` | code assigned by Axerve E-commerce Solutions this transaction.
| `shopTransactionID` | shop transaction ID value
| `authorizationCode` | authorisation code |
| `paymentID` | The payment ID 
| `currency` | The [currency ISO code](#currency-codes)
| `country` | nationality of the card issuer | 
| `company` | Card issuer company | 
| `tdLevel` | The level of 3D-Secure authentication: `FULL` or `HALF`. 
| `buyer` | Contains informations about the buyer. See the table below. 
| `risk` | A risk code, assigned by [Axerve Guaranteed Payment](#gestpay-guaranteed-payment). See the table below for more informations.  
| `customInfo` | An object containing optional customised parameters, created by the merchant in the form of key-value. 
| `alertCode` | Alert code. See [Better Risk Management](<%= config[:doc_url] %>/soap/security/better-risk-management-reacting-to-suspicious-activity/) for an accurate description. 
| `alertDescription` | A textual description of the `alertCode`. 
| `cvvPresent` | Credit Card security code flag  
| `maskedPAN` | Masked Pan string
| `paymentMethod` | Indicates the used Payment Method
| `productType` |  String containing Card Type
| `token` | String containing the token value
| `tokenExpiryMonth` | String containing the token expiry month
| `TokenExpiryYear` | String containing the token expiry year
| `userRedirect.href` | a URL to redirect the user for 3D-Secure authentication, or to pay on alternative payment systems. 


##### `buyer` details

| Field          | Description 
| -------------- | -----------
| `name` | The buyer's name 
| `email` | The buyer's email 

##### `risk` details 

| Field | Description
| ----- | -----------
| `riskResponseCode` | One of Axerve Guaranteed Payment [response codes](#risk-response-codes).
| `riskResponseDescription` | A textual description of the risk analisys. 

