# Accounting Context

## Events

| Type                                                                                                          | GARAIO REM         | REM | Description                                      |
| ------------------------------------------------------------------------------------------------------------- | ------------------ | --- | ------------------------------------------------ |
| [Accounting.Accounting.Created](#accountingaccountingcreated)                                                 | :white_check_mark: | :x: | An accounting was created                        |
| [Accounting.Accounting.Updated](#accountingaccountingupdated)                                                 | :white_check_mark: | :x: | An accounting was updated                        |
| [Accounting.Accounting.Deleted](#accountingaccountingdeleted)                                                 | :white_check_mark: | :x: | An accounting was deleted                        |
| [Accounting.Account.Create](#accountingaccountcreate)                                                         | :white_check_mark: | :x: | Create an account on an accounting               |
| [Accounting.Account.Update](#accountingaccountupdate)                                                         | :white_check_mark: | :x: | Update an account on an accounting               |
| [Accounting.Account.Created](#accountingaccountcreated)                                                       | :white_check_mark: | :x: | An account was created                           |
| [Accounting.Account.Updated](#accountingaccountupdated)                                                       | :white_check_mark: | :x: | An account was updated                           |
| [Accounting.Account.Deleted](#accountingaccountdeleted)                                                       | :white_check_mark: | :x: | An account was deleted                           |
| [Accounting.BusinessYear.Updated](#accountingbusinessyearupdated)                                             | :white_check_mark: | :x: | Business year period status changed              |
| [Accounting.BusinessYearExportConfiguration.Updated](#accountingbusinessyearexportconfigurationupdated)       | :white_check_mark: | :x: | Business year export configuration changed       |
| [Accounting.CostCenter.Created](#accountingcostcentercreated)                                                 | :white_check_mark: | :x: | A cost center was created                        |
| [Accounting.CostCenter.Updated](#accountingcostcenterupdated)                                                 | :white_check_mark: | :x: | A cost center was updated                        |
| [Accounting.CostCenter.Deleted](#accountingcostcenterdeleted)                                                 | :white_check_mark: | :x: | A cost center was deleted                        |

### Accounting.Accounting.Created

An accounting was created

| Field                                 | Type      | Content / Remarks                                                                                                                                                                      |
| ------------------------------------- | --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `eventType`                           | `string`  | `Accounting.Accounting.Created`                                                                                                                                                        |
| `data`                                | `hash`    |                                                                                                                                                                                        |
| &nbsp;&nbsp;`reference`               | `string`  | Reference of the accounting                                                                                                                                                            |

#### Example

```json
{
  "eventType": "Accounting.Accounting.Created",
  "data": {
    "reference": "1569"
  }
}
```

### Accounting.Accounting.Updated

An accounting was updated

| Field                                 | Type      | Content / Remarks                                                                                                                                                                      |
| ------------------------------------- | --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `eventType`                           | `string`  | `Accounting.Accounting.Updated`                                                                                                                                                        |
| `data`                                | `hash`    |                                                                                                                                                                                        |
| &nbsp;&nbsp;`reference`               | `string`  | Reference of the accounting                                                                                                                                                            |

#### Example

```json
{
  "eventType": "Accounting.Accounting.Updated",
  "data": {
    "reference": "1569"
  }
}
```

### Accounting.Accounting.Deleted

An accounting was deleted

| Field                                 | Type      | Content / Remarks                                                                                                                                                                      |
| ------------------------------------- | --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `eventType`                           | `string`  | `Accounting.Accounting.Deleted`                                                                                                                                                        |
| `data`                                | `hash`    |                                                                                                                                                                                        |
| &nbsp;&nbsp;`reference`               | `string`  | Reference of the accounting                                                                                                                                                            |

#### Example

```json
{
  "eventType": "Accounting.Accounting.Deleted",
  "data": {
    "reference": "1569"
  }
}
```

### Accounting.Account.Create

Create an account on an accounting

| Field                                 | Type      | Content / Remarks                                                                                                                                                                      |
| ------------------------------------- | --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `eventType`                           | `string`  | `Accounting.Account.Create`                                                                                                                                                            |
| `data`                                | `hash`    |                                                                                                                                                                                        |
| &nbsp;&nbsp;`accountingReference`     | `string`  | Reference of the accounting; **required**                                                                                                                                              |
| &nbsp;&nbsp;`number`                  | `string`  | Number of the account to create; must not be already taken; **required**                                                                                                               |
| &nbsp;&nbsp;`type`                    | `string`  | account type; the following types are accepted: `Konto`, `KontoNebenkosten`, `KontoScharnierung`, `KontoSteuer`, `KontoVerrechnung`; **required**; if you do not pass a type atttribute, the default `Konto` is applied |
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
| &nbsp;&nbsp;`vatCodes`                | `string`  | Array of VAT Codes that are accepted when posting to this account, eg ["00","NO"]; only makes sense if you pass a taxAccountNumber, too                                                |
| &nbsp;&nbsp;`vatNumber_1`             | `string`  | VAT number 1 (used to fill the VAT form)                                                                                                                                               |
| &nbsp;&nbsp;`vatNumber_2`             | `string`  | VAT number 2 (used to fill the VAT form)                                                                                                                                               |
| &nbsp;&nbsp;`taxType`                 | `string`  | Tax type; pass INPUT or SALE when creating a KontoSteuer                                                                                                                               |
| &nbsp;&nbsp;`taxAccountNumber`        | `string`  | Account number of the tax account to use when posting with a VAT Code                                                                                                                  |
| &nbsp;&nbsp;`taxQuota`                | `decimal`  | Optional tax quota percentage                                                                                                                                                          |
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
    "taxQuota": 90.0,
    "number": "10210",
    "accountGroupNumber": "1020",
    "titleDe": "Steuerkonto 10210",
  }
}
```

### Accounting.Account.Update

Update an account on an accounting

| Field                                 | Type      | Content / Remarks                                                                                                                                                                      |
| ------------------------------------- | --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `eventType`                           | `string`  | `Accounting.Account.Update`                                                                                                                                                            |
| `data`                                | `hash`    |                                                                                                                                                                                        |
| &nbsp;&nbsp;`accountingReference`     | `string`  | Reference of the accounting; **required**                                                                                                                                              |
| &nbsp;&nbsp;`number`                  | `string`  | Number of the account to update; **required**                                                                                                                                           |
| &nbsp;&nbsp;`type`                    | `string`  | account type; the following types are accepted: `Konto`, `KontoNebenkosten`, `KontoScharnierung`, `KontoSteuer`, `KontoVerrechnung`; **required**; if you do not pass a type atttribute, the default `Konto` is applied |
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
| &nbsp;&nbsp;`isLocked`                | `boolean` | Should the account be locked after update?                                                                                                                                           |
| &nbsp;&nbsp;`isInheritable`           | `boolean` | Should accountings linked to this accounting inherit the account?                                                                                                                      |
| &nbsp;&nbsp;`saveLifeCycle`           | `boolean` | Should posts to this account be visible in the masterdata life cycle?                                                                                                                  |
| &nbsp;&nbsp;`quantityMandatory`       | `boolean` | Is a quantity mandatory for posts to this account?                                                                                                                                     |
| &nbsp;&nbsp;`unitMandatory`           | `boolean` | Is a unit reference mandatory for posts to this account?                                                                                                                               |
| &nbsp;&nbsp;`vatCodes`                | `string`  | Array of VAT Codes that are accepted when posting to this account, eg ["00","NO"]; only makes sense if you pass a taxAccountNumber, too                                                |
| &nbsp;&nbsp;`vatNumber_1`             | `string`  | VAT number 1 (used to fill the VAT form)                                                                                                                                               |
| &nbsp;&nbsp;`vatNumber_2`             | `string`  | VAT number 2 (used to fill the VAT form)                                                                                                                                               |
| &nbsp;&nbsp;`taxType`                 | `string`  | Tax type; pass INPUT or SALE when updating a KontoSteuer                                                                                                                               |
| &nbsp;&nbsp;`taxAccountNumber`        | `string`  | Account number of the tax account to use when posting with a VAT Code                                                                                                                  |
| &nbsp;&nbsp;`taxQuota`                | `decimal`  | Optional tax quota percentage                                                                                                                                                          |
| &nbsp;&nbsp;`revenueAccountNumber`    | `string`  | Revenue account number                                                                                                                                                                 |

Notes

* (1) multi language attributes fall back to german if the corresponding language attribute is not set
* (2) set this attribute only if you update a KontoNebenkosten account

#### Minimal example

```json
{
  "eventType": "Accounting.Account.Update",
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
  "eventType": "Accounting.Account.Update",
  "data": {
    "accountingReference": "1569",
    "type": "KontoSteuer",
    "taxType": "INPUT",
    "taxQuota": 90.0,
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
| `number`                  | `string`  | Number of the created / updated account     |

#### Accounting.Account.Rejected

The [Reject](./result_messages.md#rejected-message) message.

Additional `data` fields:

| Field                     | Type     | Content / Remarks                            |
| ------------------------- | -------- | -------------------------------------------- |
| `accountingReference`     | `string`  | Reference of the accounting                 |
| `number`                  | `string`  | Number of the created / updated account     |

### Accounting.Account.Created

An account was created

| Field                                 | Type      | Content / Remarks                                                                                                                                                                      |
| ------------------------------------- | --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `eventType`                           | `string`  | `Accounting.Account.Created`                                                                                                                                                           |
| `data`                                | `hash`    |                                                                                                                                                                                        |
| &nbsp;&nbsp;`accountingReference`     | `string`  | Reference of the accounting                                                                                                                                                            |
| &nbsp;&nbsp;`number`                  | `string`  | Number of the created account                                                                                                                                                          |

#### Example

```json
{
  "eventType": "Accounting.Account.Created",
  "data": {
    "accountingReference": "1569",
    "number": "10210"
  }
}
```

### Accounting.Account.Updated

An account was updated

| Field                                 | Type      | Content / Remarks                                                                                                                                                                      |
| ------------------------------------- | --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `eventType`                           | `string`  | `Accounting.Account.Updated`                                                                                                                                                           |
| `data`                                | `hash`    |                                                                                                                                                                                        |
| &nbsp;&nbsp;`accountingReference`     | `string`  | Reference of the accounting                                                                                                                                                            |
| &nbsp;&nbsp;`number`                  | `string`  | Number of the updated account                                                                                                                                                          |

#### Example

```json
{
  "eventType": "Accounting.Account.Updated",
  "data": {
    "accountingReference": "1569",
    "number": "10210"
  }
}
```

### Accounting.Account.Deleted

An account was deleted

| Field                                 | Type      | Content / Remarks                                                                                                                                                                      |
| ------------------------------------- | --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `eventType`                           | `string`  | `Accounting.Account.Deleted`                                                                                                                                                           |
| `data`                                | `hash`    |                                                                                                                                                                                        |
| &nbsp;&nbsp;`accountingReference`     | `string`  | Reference of the accounting                                                                                                                                                            |
| &nbsp;&nbsp;`number`                  | `string`  | Number of the deleted account                                                                                                                                                          |

#### Example

```json
{
  "eventType": "Accounting.Account.Deleted",
  "data": {
    "accountingReference": "1569",
    "number": "10210"
  }
}
```

### Accounting.BusinessYear.Updated

Business year period status changed. This message is published when:

* Period closing operations occur (period or business year closure)
* Period status changes in UpdateGeschaeftsjahrCommand (abgeschlossen_bis or in_abschluss_bis changes)
* Export is enabled for this business year (either closed or open periods)

| Field                                 | Type      | Content / Remarks                                                                                                                                                                      |
| ------------------------------------- | --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `eventType`                           | `string`  | `Accounting.BusinessYear.Updated`                                                                                                                                                      |
| `data`                                | `hash`    |                                                                                                                                                                                        |
| &nbsp;&nbsp;`accountingReference`     | `string`  | Reference of the accounting                                                                                                                                                            |
| &nbsp;&nbsp;`businessYear`            | `hash`    | Business year details                                                                                                                                                                  |
| &nbsp;&nbsp;&nbsp;&nbsp;`start`       | `string`  | Start date of the business year; ISO 8601 date                                                                                                                                         |
| &nbsp;&nbsp;&nbsp;&nbsp;`end`         | `string`  | End date of the business year; ISO 8601 date                                                                                                                                           |
| &nbsp;&nbsp;&nbsp;&nbsp;`periodicity` | `string`  | Periodicity of the business year; one of: `monthly`, `quarterly`, `semi-annual`, `yearly`                                                                                              |
| &nbsp;&nbsp;&nbsp;&nbsp;`periods`     | `array`   | Array of period objects                                                                                                                                                                |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`start` | `string` | Start date of the period; ISO 8601 date                                                                                                                                          |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`end`   | `string` | End date of the period; ISO 8601 date                                                                                                                                            |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`status` | `string` | Period status; one of: `open` (can be booked to), `blocked` (in closing process), `closed` (closed and locked)                                                             |

#### Example

```json
{
  "eventType": "Accounting.BusinessYear.Updated",
  "data": {
    "accountingReference": "1569",
    "businessYear": {
      "start": "2024-01-01",
      "end": "2024-12-31",
      "periodicity": "monthly",
      "periods": [
        {
          "start": "2024-01-01",
          "end": "2024-01-31",
          "status": "closed"
        },
        {
          "start": "2024-02-01",
          "end": "2024-02-29",
          "status": "blocked"
        },
        {
          "start": "2024-03-01",
          "end": "2024-03-31",
          "status": "open"
        }
      ]
    }
  }
}
```

### Accounting.BusinessYearExportConfiguration.Updated

Business year export configuration was updated. This message is published when the owner portal export settings change for a business year.

| Field                                 | Type      | Content / Remarks                                                                                                                                                                      |
| ------------------------------------- | --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `eventType`                           | `string`  | `Accounting.BusinessYearExportConfiguration.Updated`                                                                                                                                   |
| `data`                                | `hash`    |                                                                                                                                                                                        |
| &nbsp;&nbsp;`accountingReference`     | `string`  | Reference of the accounting                                                                                                                                                            |
| &nbsp;&nbsp;`businessYear`            | `hash`    | Business year identification                                                                                                                                                           |
| &nbsp;&nbsp;&nbsp;&nbsp;`start`       | `string`  | Start date of the business year; ISO 8601 date                                                                                                                                         |
| &nbsp;&nbsp;&nbsp;&nbsp;`end`         | `string`  | End date of the business year; ISO 8601 date                                                                                                                                           |
| &nbsp;&nbsp;`exportClosedPeriods`     | `boolean` | Whether closed periods should be exported to owner portal                                                                                                                              |
| &nbsp;&nbsp;`exportOpenPeriods`       | `boolean` | Whether open periods should be exported to owner portal                                                                                                                                |

#### Example

```json
{
  "eventType": "Accounting.BusinessYearExportConfiguration.Updated",
  "data": {
    "accountingReference": "1569",
    "businessYear": {
      "start": "2024-01-01",
      "end": "2024-12-31"
    },
    "exportClosedPeriods": true,
    "exportOpenPeriods": false
  }
}
```

### Accounting.CostCenter.Created

| Field                                 | Type      | Content / Remarks                                                                                                                                                                      |
| ------------------------------------- | --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `eventType`                           | `string`  | `Accounting.CostCenter.Created`                                                                                                                                                        |
| `data`                                | `hash`    |                                                                                                                                                                                        |
| &nbsp;&nbsp;`accountingReference`     | `string`  | Reference of the accounting                                                                                                                                                            |
| &nbsp;&nbsp;`number`                  | `string`  | Number of the created cost center                                                                                                                                                      |

#### Example

```json
{
  "eventType": "Accounting.CostCenter.Created",
  "data": {
    "accountingReference": "1569",
    "number": "10210"
  }
}
```

### Accounting.CostCenter.Updated

| Field                                 | Type      | Content / Remarks                                                                                                                                                                      |
| ------------------------------------- | --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `eventType`                           | `string`  | `Accounting.CostCenter.Updated`                                                                                                                                                        |
| `data`                                | `hash`    |                                                                                                                                                                                        |
| &nbsp;&nbsp;`accountingReference`     | `string`  | Reference of the accounting                                                                                                                                                            |
| &nbsp;&nbsp;`number`                  | `string`  | Number of the updated cost center                                                                                                                                                      |

#### Example

```json
{
  "eventType": "Accounting.CostCenter.Updated",
  "data": {
    "accountingReference": "1569",
    "number": "10210"
  }
}
```

### Accounting.CostCenter.Deleted

| Field                                 | Type      | Content / Remarks                                                                                                                                                                      |
| ------------------------------------- | --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `eventType`                           | `string`  | `Accounting.CostCenter.Deleted`                                                                                                                                                           |
| `data`                                | `hash`    |                                                                                                                                                                                        |
| &nbsp;&nbsp;`accountingReference`     | `string`  | Reference of the accounting                                                                                                                                                            |
| &nbsp;&nbsp;`number`                  | `string`  | Number of the deleted cost center                                                                                                                                                          |

#### Example

```json
{
  "eventType": "Accounting.CostCenter.Deleted",
  "data": {
    "accountingReference": "1569",
    "number": "10210"
  }
}
```
