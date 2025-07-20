# PaymentsDetails

Payment details of a person. This is a Array of hash structured as follows.

Field | Type | Content / Remarks
---|---|---
&nbsp;&nbsp;`paymentDetails` | `array` | list of new payment details
&nbsp;&nbsp;&nbsp;&nbsp;`iban` | `string` | iban; **required**
&nbsp;&nbsp;&nbsp;&nbsp;`ibanName` | `string` | iban name, e.g. name of the bank
&nbsp;&nbsp;&nbsp;&nbsp;`bic` | `string` | bic
&nbsp;&nbsp;&nbsp;&nbsp;`locked` | `boolean` | should the payment detail be locked
&nbsp;&nbsp;&nbsp;&nbsp;`lockReason` | `string` | why should the payment detail be locked? required if you set `locked` to `true`
&nbsp;&nbsp;&nbsp;&nbsp;`defaultPaymentDetail` | `boolean` | should this payment detail be the default? Set it to `true` if the payment detail should become the default. Do not send `false` since that makes no sense, we will ignore it. If you send `true` for more than one payment detail, the last one wins

## Merging strategy

When updating `PaymentsDetails`, fields are merged as follows:

* do not send the attribute if you do not want to create current data of this type
* send an empty array (`[]`) to delete all current data of this type
* send an array of valid data to fully replace the current data of this type
* payment details that exist but are not contained in the paymentDetails will be locked