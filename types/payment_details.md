# PaymentDetails

Payment details of a person. This is a Array of hash structured as follows.

| Field                                          | Type      | Content / Remarks                                                               |
| ---------------------------------------------- | --------- | ------------------------------------------------------------------------------- |
| &nbsp;&nbsp;`paymentDetails`                   | `array`   | list of new payment details                                                     |
| &nbsp;&nbsp;&nbsp;&nbsp;`iban`                 | `string`  | iban; **required**                                                              |
| &nbsp;&nbsp;&nbsp;&nbsp;`ibanName`             | `string`  | iban name, e.g. name of the bank                                                |
| &nbsp;&nbsp;&nbsp;&nbsp;`bic`                  | `string`  | bic                                                                             |
| &nbsp;&nbsp;&nbsp;&nbsp;`locked`               | `boolean` | should the payment detail be locked/disabled (blocked from usage)               |
| &nbsp;&nbsp;&nbsp;&nbsp;`lockReason`           | `string`  | why should the payment detail be locked? required if you set `locked` to `true` |
| &nbsp;&nbsp;&nbsp;&nbsp;`defaultPaymentDetail` | `boolean` | should this payment detail be the default?; see (1)                             |

**Notes:**

* (1) Set it to `true` to make this the default payment detail. If more than one payment detail have this set to `true`, the last one takes precedence. Setting the flag to `false` has no effect.

## Merging strategy

When updating `paymentDetails`, fields are merged as follows:

* **Omit field** -> existing payment details remain unchanged
* **Send `[]`** -> locks/disables usage of all existing payment details (requires `deactivationReason`)
* **Send array with entries** -> updates/creates specified entries, locks all others not in the array (requires `deactivationReason` for locked ones)
* **send multiple entries** -> to completely replace all existing payment data of this type.

**NOTE:** When using Masterdata.Person.Update, the deactivation reason field is paymentDeactivationReason instead of deactivationReason.

## Type Example
```json
"paymentDetails": [
  {
    "iban": "CH9300762011623852957",
    "bic": "UBSWCHZH80A",
    "ibanName": "UBS Zürich",
    "defaultPaymentDetail": true
  },
  {
    "iban": "DE19500105176829385733",
    "bic": "WELADEDLLEV",
    "ibanName": "Sparkasse Berlin",
    "locked": false
  }
]
```

## Full Message Examples

**Example 1:** Add new payment detail and set as default

```json
{
  "eventType": "Masterdata.PersonPaymentDetails.Update",
  "data": {
    "personReference": "100150",
    "paymentDetails": [
      {
        "iban": "CH9300762011623852957",
        "bic": "UBSWCHZH80A",
        "ibanName": "UBS Zürich",
        "defaultPaymentDetail": true
      }
    ],
    "deactivationReason": "Old account closed"
  }
}
```

**Example 2:** Lock all existing payment details
```json
{
  "eventType": "Masterdata.PersonPaymentDetails.Update",
  "data": {
    "personReference": "100150",
    "paymentDetails": [],
    "deactivationReason": "Account verification required"
  }
}
```

**Example 3:** Unlock a previously locked payment detail
```json
{
  "eventType": "Masterdata.PersonPaymentDetails.Update",
  "data": {
    "personReference": "100150",
    "paymentDetails": [
      {
        "iban": "CH9300762011623852957",
        "locked": false,
        "lockReason": ""
      }
    ]
  }
}
```

**Example 4:** Update via Masterdata.Person.Update
```json
{
  "eventType": "Masterdata.Person.Update",
  "data": {
    "personReference": "100150",
    "paymentDetails": [
      {
        "iban": "DE19500105176829385733",
        "bic": "WELADEDLLEV",
        "defaultPaymentDetail": true
      }
    ],
    "paymentDeactivationReason": "Migrating to new bank"
  }
}
```
