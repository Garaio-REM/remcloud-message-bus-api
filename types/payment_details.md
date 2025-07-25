# PaymentDetails

Payment details of a person. This is a Array of hash structured as follows.

| Field                                          | Type      | Content / Remarks                                                               |
| ---------------------------------------------- | --------- | ------------------------------------------------------------------------------- |
| &nbsp;&nbsp;`paymentDetails`                   | `array`   | list of new payment details                                                     |
| &nbsp;&nbsp;&nbsp;&nbsp;`iban`                 | `string`  | iban; **required**                                                              |
| &nbsp;&nbsp;&nbsp;&nbsp;`ibanName`             | `string`  | iban name, e.g. name of the bank                                                |
| &nbsp;&nbsp;&nbsp;&nbsp;`bic`                  | `string`  | bic                                                                             |
| &nbsp;&nbsp;&nbsp;&nbsp;`locked`               | `boolean` | should the payment detail be locked                                             |
| &nbsp;&nbsp;&nbsp;&nbsp;`lockReason`           | `string`  | why should the payment detail be locked? required if you set `locked` to `true` |
| &nbsp;&nbsp;&nbsp;&nbsp;`defaultPaymentDetail` | `boolean` | should this payment detail be the default?; see (1)                             |

Notes

* (1) Set it to `true` to make this the default payment detail. If more than one payment detail have this set to `true`, the last one takes precedence. Setting the flag to `false` has no effect.

## Merging strategy

When updating `PaymentDetails`, fields are merged as follows:

* do not send the attribute if you do not want to create current data of this type
* send an empty array (`[]`) to delete all current data of this type
* send an array of valid data to fully replace the current data of this type
* payment details that exist but are not contained in the paymentDetails will be locked
