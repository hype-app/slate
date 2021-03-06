### POST dashboard/export/template


> Sandbox URL:

```
POST https://sandbox.gestpay.net/api/v1/dashboard/export/template/
```


> Production URL: 

```
POST https://ecomms2s.sella.it/api/v1/dashboard/export/template/
```


> Example request body: 

```json
{
  "shopLogin":"myshoplogin",
  "templateId":"",
  "description":"New template 1",
  "default":"True",
  "columns":[
    "Email compratore",
    "ID Banca",
    "Stato",
    "Cardnumber",
    ...
  ]
}
```

Creates or modifies a template. If the `templateId` is present, the template is updated. 

#### Request 

Headers: 

| Header          | Value                         | Description                                                        |
| --------------- | ----------------------------- | ------------------------------------------------------------------ |
| `Authorization` | `apikey {merchant Api Key}` | The merchant API key can be found in Axerve Merchant Back-Office. |


Body parameters: 

(Fields in **bold** are mandatory)

| Field | Description | 
| --------- | ----------- | 
| **`shopLogin`** | The shop identifier. | 
| **`template.templateId`** | Leave empty if you're creating a new template. Otherwise, you must use an existing `templateId` to update one. 
| **`template.description`** | A description of the template in common language 
| `template.default` | `True` or `False`
| **`template.columns`** | an array of strings, representing the name of the properties that compose a transaction. 

#### Response 

> Success response example:<br>
> `200 OK`

```json
{
   "error":{  
      "code":"0",
      "description":"request correctly processed"
   },
   "payload":{
    "result":"OK",
    "template":[
      {
        "templateId":"121212212",
        "description":"New template 1",
        "default":"True",
        "columns":[
          "date",
          "shopTransactionID",
          "bankTransactionID",
          "transactionResult",
          "authorizedAmount",
          "capturedAmount",
          "paymentID",
          "currency"
        ]
      }
    ]
  }
}
```

See the section [Handling responses & errors](#handling-responses-amp-errors) to learn how Axerve E-commerce Solutions reports errors.

Response `payload` details: 

| Field          | Description 
| -------------- | -----------
| `result`     | `OK` or `KO`
| `template` | An array containing one or more templates.
| `template.templateID` | The template ID of the template created or updated
| `template.description` | A description of the template in common language 
| `template.default` | `True` or `False`
| `template.columns` | an array of strings, representing the name of the properties that compose a transaction. 
