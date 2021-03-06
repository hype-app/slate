### CallUpdateTokenS2S 

Merchants can use this method to update the expiry date of a token.

Merchants belonging to a group are allowed to update also tokens created by other merchants of the same group. If a merchant belongs to a group, can update also token created by the other group's merchants.


#### Request details 

> Request example: 

```xml
<CallUpdateTokenS2S>
  <shopLogin>90000001</shopLogin>
  <token>1234WJFXABCD5678</token>
  <expiryMonth>12</expiryMonth>
  <expiryYear>21</expiryYear>
  <withAut>Y</withAut>
</CallUpdateTokenS2S>
```

Mandatory parameters are in **bold**. 

| parameter name | description | type | length | 
| -------------- | ----------- | -----|--------| 
| **`shopLogin`** | the merchant's code | string | 30 |  
| **`token`** | Token value to update | string | 25 
| **`expiryMonth`** | Assigns card expiration month | string | 2
| **`expiryYear`** | Assigns card expiration year | string | 2 
| **`withAut`** | tries also to authorize the card. <br> `Y` on `N` | string | 1
| `apikey` | If you have selected the _apiKey_ authentication method, add the `apikey` field to the call. [More details about the apiKey here](#authorizing-calls-against-gestpay). |  |  | 

#### Response details 

> Response example: 

```xml
<CallUpdateTokenS2SResult>
  <GestPayS2S xmlns="">
    <TransactionType>UpdateToken</TransactionType>
    <TransactionResult>OK</TransactionResult>
    <ErrorCode>0</ErrorCode>
    <ErrorDescription />
  </GestPayS2S>
</CallUpdateTokenS2SResult>
```

| parameter name | description |  
| -------------- | ----------- |  
| `TransactionType` | `UpdateToken`
| `TransactionResult` | `OK` or `KO`
| `ErrorCode` | transaction error code | 
| `ErrorDescription` | transaction error description