# Accounting Context

## Events

| Type                                                      | GARAIO REM         | REM | Description                                                        |
| --------------------------------------------------------- | ------------------ | --- | ------------------------------------------------------------------ |
| [Accounting.Account.Create](#accountingaccountcreate)     | :white_check_mark: | :x: | Create an account on an accounting                                 |

### Accounting.Account.Create

Create an account on an accountig

| Field                                 | Type      | Content / Remarks                                                                                                                                                                      |
| ------------------------------------- | --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `eventType`                           | `string`  | `Accounting.Account.Create`                                                                                                                                                            |
| `data`                                | `hash`    |                                                                                                                                                                                        |
| &nbsp;&nbsp;`accountingReference`     | `string`  | Reference of the accounting; **required**                                                                                                                                              |
| &nbsp;&nbsp;`number`                  | `string`  | Number of the account to create; must not be already taken; **required**                                                                                                               |
| &nbsp;&nbsp;`type`                    | `string`  | optional account type; the following types are accepted: `Konto`, `KontoNebenkosten`, `KontoScharnierung`, `KontoSteuer`, `KontoVerrechnung`; leave empty to create a standard account |
| &nbsp;&nbsp;`accountGroupNumber`      | `string`  | Existing account group number where the account should be added; **required**                                                                                                          |
| &nbsp;&nbsp;`titleDe`                 | `string`  | German title of the account; **required**; see (1)                                                                                                                                     |
| &nbsp;&nbsp;`titleFr`                 | `string`  | French title of the account; see (1)                                                                                                                                                   |
| &nbsp;&nbsp;`titleIt`                 | `string`  | Italian title of the account; see (1)                                                                                                                                                  |
| &nbsp;&nbsp;`titleEn`                 | `string`  | English title of the account; see (1)                                                                                                                                                  |
| &nbsp;&nbsp;`bookingTextMandatory`    | `boolean` | Is a booking text mandatory when posting to this account?                                                                                                                              |
| &nbsp;&nbsp;`defaultBookingTextDe`    | `string`  | German default booking text; see (1)                                                                                                                                                   |
| &nbsp;&nbsp;`defaultBookingTextEn`    | `string`  | French default booking text; see (1)                                                                                                                                                   |
| &nbsp;&nbsp;`defaultBookingTextFr`    | `string`  | Italian default booking text; see (1)                                                                                                                                                  |
| &nbsp;&nbsp;`defaultBookingTextIt`    | `string`  | English default booking text; see (1)                                                                                                                                                  |
| &nbsp;&nbsp;`defaultCostCenterNumber` | `string`  | Default cost center number; see (2)                                                                                                                                                    |
| &nbsp;&nbsp;`individualItemBalancing` | `boolean` | Should posts to this account be individual item balanced?                                                                                                                              |
| &nbsp;&nbsp;`validFrom`               | `string`  | When does the account become active?; ISO 8601 date; Leave empty for default                                                                                                           |
| &nbsp;&nbsp;`validTo`                 | `string`  | When does the account become inactive? ; ISO 8601 date                                                                                                                                 |
| &nbsp;&nbsp;`feeEligible`             | `boolean` | Is a fee eligible for posts on this account?                                                                                                                                           |
| &nbsp;&nbsp;`isLocked`                | `boolean` | Should the account be locked after creation?                                                                                                                                           |
| &nbsp;&nbsp;`isInheritable`           | `boolean` | Should accountings linked to this accounting inherit the account?                                                                                                                      |
| &nbsp;&nbsp;`saveLifeCycle`           | `boolean` | Should posts to this account be visible in the masterdata life cycle?                                                                                                                  |
| &nbsp;&nbsp;`quantityMandatory`       | `boolean` | Is a quantity mandatory for posts to this account?                                                                                                                                     |
| &nbsp;&nbsp;`unitMandatory`           | `boolean` | Is a unit reference mandatory for posts to this account?                                                                                                                               |
| &nbsp;&nbsp;`vatCodes`                | `string`  | Array of VAT Codes that are accepted for this account                                                                                                                                  |
| &nbsp;&nbsp;`vatNumber_1`             | `string`  | VAT number 1 (used to fill the VAT form)                                                                                                                                               |
| &nbsp;&nbsp;`vatNumber_2`             | `string`  | VAT number 2 (used to fill the VAT form)                                                                                                                                               |
| &nbsp;&nbsp;`taxType`                 | `string`  | Tax type; pass INPUT or SALE when creating a KontoSteuer                                                                                                                               |
| &nbsp;&nbsp;`taxAccountNumber`        | `string`  | Account number of the tax account to use when posting with a VAT Code                                                                                                                  |
| &nbsp;&nbsp;`taxQuota`                | `string`  | Optional tax quota percentage                                                                                                                                                          |
| &nbsp;&nbsp;`revenueAccountNumber`    | `string`  | Revenue account number                                                                                                                                                                 |

Notes

* (1) multi language attributes fall back to german if the corresponding language attribute is not set
* (2) set this attribute only if you create a KontoNebenkosten account

#### Minimal example

```json
{
  "eventType": "Accounting.Account.Create",
  "data": {
    "accountingReference": "1569",
    "number": "10210",
    "accountGroupNumber": "1020",
    "titleDe": "Testkonto 10210"
  }
}
```

#### Tax account example

```json
{
  "eventType": "Accounting.Account.Create",
  "data": {
    "accountingReference": "1569",
    "type": "KontoSteuer",
    "taxType": "INPUT",
    "number": "10210",
    "accountGroupNumber": "1020",
    "titleDe": "Steuerkonto 10210",
  }
}
```

#### Accounting.Account.Accepted

The [Accept](./result_messages.md#accepted-message) message.

Additional `data` fields:

| Field                     | Type     | Content / Remarks                            |
| ------------------------- | -------- | -------------------------------------------- |
| `accountingReference`     | `string`  | Reference of the accounting                 |
| `number`                  | `string`  | Number of the created account               |

#### Accounting.Account.Rejected

The [Reject](./result_messages.md#rejected-message) message.

No additional `data` fields.
