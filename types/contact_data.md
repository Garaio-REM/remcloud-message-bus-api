# ContactData

Contact data of a person. This is a JSON hash structured as follows.

| Field                        | Type     | Content / Remarks                             |
| ---------------------------- | -------- | --------------------------------------------- |
| `privateEmails`              | `array`  | list of new private emails                    |
| `businessEmails`             | `array`  | list of new business emails                   |
| `otherEmails`                | `array`  | list of new other emails                      |
| `privatePhoneNumbers`        | `array`  | list of new private phone numbers             |
| `businessPhoneNumbers`       | `array`  | list of new business phone numbers            |
| `mobilePhoneNumbers`         | `array`  | list of new mobile phone numbers              |
| `otherPhoneNumbers`          | `array`  | list of new other phone numbers               |
| `privateFaxNumbers`          | `array`  | list of new private fax numbers               |
| `businessFaxNumbers`         | `array`  | list of new business fax numbers              |
| `otherFaxNumbers`            | `array`  | list of new other fax numbers                 |
| `privateUrls`                | `array`  | list of new private urls                      |
| `businessUrls`               | `array`  | list of new business urls                     |
| `otherUrls`                  | `array`  | list of new other urls                        |
| `contacts`                   | `array`  |                                               |
| &nbsp;&nbsp;`name`           | `string` | name of the contact                           |
| &nbsp;&nbsp;`contactAddress` | `string` | address / email / phone number of the contact |

## Merging strategy

When updating `ContactData`, fields are merged as follows:

* If you set a field, the existing entries in the given field will be replaced.
* If you set a field to `[]`, the existing entries in the given field are deleted.
* If you omit a field, the existing entries in the given field are left in place.
