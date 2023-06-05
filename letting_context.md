# Letting Context

## Events

| Type                                                                                                | GARAIO REM         | REM                | Description                                                                    |
| --------------------------------------------------------------------------------------------------- | ------------------ | ------------------ | ------------------------------------------------------------------------------ |
| [Letting.Tenancy.Created](#lettingtenancycreated)                                                   | :white_check_mark: | :x:                | A tenancy has been created; does not reliably signal a tenant move in. (1)     |
| [Letting.Tenancy.Updated](#lettingtenancyupdated)                                                   | :white_check_mark: | :x:                | Start and / or end date of a tenancy have been changed                         |
| [Letting.Tenancy.Deleted](#lettingtenancydeleted)                                                   | :white_check_mark: | :x:                | A tenancy has been deleted; this means that the tenancy never became effective |
| [Letting.Tenancy.TenancyAgreementReferenceChanged](#lettingtenancytenancyagreementreferencechanged) | :white_check_mark: | :x:                | The reference of a tenancy agreement has changed                               |
| [Letting.Reservation.Update](#lettingreservationupdate)                                             | :white_check_mark: | :x:                | Updates the reservation status of a unit.                                      |
| [Letting.TenancyAgreementSecurityDepot.Update](#lettingtenancyagreementsecuritydepotupdate)         | :white_check_mark: | :x:                | Updates the reservation status of a unit.                                      |
| [Letting.TenancyAgreementDetails.Update](#lettingtenancyagreementdetailsupdate)                     | :white_check_mark: | :x:                | Updates some details of a tenancy agreement.                                   |
| [Letting.Tenancy.MoveInConfirmed](#lettingtenancymoveinconfirmed)                                   | :x:                | :white_check_mark: | Confirms a tenant will move or has moved into a unit. (2)                      |
| [Letting.Tenancy.MoveOutConfirmed](#lettingtenancymoveoutconfirmed)                                 | :x:                | :white_check_mark: | Confirms a tenant will move out or has moved out of a unit. (3)                |

Notes

* (1) A tenancy is uniquely identified by tenancy agreement reference, tenant reference and unit reference
* (2) This event is only raised for tenants that live or trade in a given unit. For example the event is not raised for tenants that act as guarantors or for tenants that have had their tenancy agreement changed while staying in the same unit.
* (3) Like Letting.Tenancy.MoveInConfirmed the event is also only raised when a tenant has lived or traded in person at the given unit.

### Letting.Tenancy.Created

| Field                                   | Type     | Content / Remarks                                         |
| --------------------------------------- | -------- | --------------------------------------------------------- |
| eventType                               | `string` | Letting.Tenancy.Created                                   |
| data                                    | `hash`   |                                                           |
| &nbsp;&nbsp;startDate                   | `string` | ISO 8601 encoded date, eg '2019-05-25'                    |
| &nbsp;&nbsp;endDate                     | `string` | ISO 8601 encoded date, eg '2019-05-25'; might be null     |
| &nbsp;&nbsp;tenancyAgreementReference   | `string` | unique tenancy agreement identifier, eg '1234.01.0001.01' |
| &nbsp;&nbsp;unitReference               | `string` | unique unit identifier, eg '234.01.0001'                  |
| &nbsp;&nbsp;tenant                      | `hash`   |                                                           |
| &nbsp;&nbsp;&nbsp;&nbsp;reference       | `string` | tenant reference; uniquely identifies a person            |
| &nbsp;&nbsp;&nbsp;&nbsp;firstName       | `string` |                                                           |
| &nbsp;&nbsp;&nbsp;&nbsp;surname         | `string` |                                                           |
| &nbsp;&nbsp;&nbsp;&nbsp;languageCode    | `string` | de, fr, it or en; **must be lower case**                  |
| &nbsp;&nbsp;&nbsp;&nbsp;nationalityCode | `string` | ISO country code, eg 'CH'                                 |
| &nbsp;&nbsp;&nbsp;&nbsp;phoneNumber     | `string` | might be null                                             |
| &nbsp;&nbsp;&nbsp;&nbsp;email           | `string` | might be null                                             |

#### Example

```json
{"eventType":"Letting.Tenancy.Created",
  "data":{
    "startDate":"2019-05-01",
    "endDate":null,
    "tenancyAgreementReference":"10001.786.29.01",
    "unitReference":"10001.786.29",
    "tenant":{
      "reference":"100004",
      "firstName":"Haupt",
      "surname":"Mieter",
      "languageCode":"de",
      "nationalityCode":"AT",
      "phoneNumber":"+41 31 331 21 11",
      "email":"email@test-mail.xy"
    }
  }
}
```

### Letting.Tenancy.Updated

| Field                                 | Type     | Content / Remarks                                         |
| ------------------------------------- | -------- | --------------------------------------------------------- |
| eventType                             | `string` | Letting.Tenancy.Updated                                   |
| data                                  | `hash`   |                                                           |
| &nbsp;&nbsp;startDate                 | `string` | ISO 8601 encoded date, eg '2019-05-25'                    |
| &nbsp;&nbsp;endDate                   | `string` | ISO 8601 encoded date, eg '2019-05-25'; might be null     |
| &nbsp;&nbsp;tenancyAgreementReference | `string` | unique tenancy agreement identifier, eg '1234.01.0001.01' |
| &nbsp;&nbsp;unitReference             | `string` | unique unit identifier, eg '234.01.0001'                  |
| &nbsp;&nbsp;tenantReference           | `string` | unique person identifier, eg '1234'                       |

#### Example

```json
{"eventType":"Letting.Tenancy.Updated",
  "data":{
    "startDate":"2019-05-01",
    "endDate":null,
    "tenancyAgreementReference":"10001.786.29.01",
    "unitReference":"10001.786.29",
    "tenantReference":"100004"
    }
  }
}
```

### Letting.Tenancy.Deleted

| Field                                 | Type     | Content / Remarks                                         |
| ------------------------------------- | -------- | --------------------------------------------------------- |
| eventType                             | `string` | Letting.Tenancy.Deleted                                   |
| data                                  | `hash`   |                                                           |
| &nbsp;&nbsp;tenancyAgreementReference | `string` | unique tenancy agreement identifier, eg '1234.01.0001.01' |
| &nbsp;&nbsp;unitReference             | `string` | unique unit identifier, eg '234.01.0001'                  |
| &nbsp;&nbsp;tenantReference           | `string` | unique person identifier, eg '1234'                       |

#### Example

```json
{"eventType":"Letting.Tenancy.Deleted",
  "data":{
    "tenancyAgreementReference":"10001.786.29.01",
    "unitReference":"10001.786.29",
    "tenantReference":"100004"
  }
}
```

### Letting.Tenancy.TenancyAgreementReferenceChanged

A user might change the reference of a unit in GARAIO REM which affects tenancy agreement references as well. This event reflects such a change. If you store tenancy data in a local domain model you must apply this change to your data.

| Field                                    | Type     | Content / Remarks                                              |
| ---------------------------------------- | -------- | -------------------------------------------------------------- |
| eventType                                | `string` | Letting.Tenancy.TenancyAgreementReferenceChanged               |
| data                                     | `hash`   |                                                                |
| &nbsp;&nbsp;tenancyAgreementReference    | `string` | unique tenancy agreement identifier, eg '1234.01.0001.01'      |
| &nbsp;&nbsp;newTenancyAgreementReference | `string` | new identifier for the tenancy agreement, eg '1234.01.0011.01' |

#### Example

```json
{"eventType":"Letting.Tenancy.TenancyAgreementReferenceChanged",
  "data":{
    "tenancyAgreementReference":"1234.01.0001.01",
    "newTenancyAgreementReference":"1234.01.0011.01",
  }
}
```

### Letting.Tenancy.MoveInConfirmed

| Field                                           | Type     | Content / Remarks                                                           |
| ----------------------------------------------- | -------- | --------------------------------------------------------------------------- |
| eventType                                       | `string` | Letting.Tenancy.MoveInConfirmed                                             |
| data                                            | `hash`   |                                                                             |
| &nbsp;&nbsp;tenancyStartDate                    | `string` | ISO 8601 encoded date, eg '2019-03-01'                                      |
| &nbsp;&nbsp;tenancyAgreementReference           | `string` | unique tenancy agreement identifier, eg '1234.01.0001.01'                   |
| &nbsp;&nbsp;unitReference                       | `string` | unique unit identifier, eg '234.01.0001'                                    |
| &nbsp;&nbsp;tenant                              | `hash`   |                                                                             |
| &nbsp;&nbsp;&nbsp;&nbsp;reference               | `string` | tenant reference; uniquely identifies a person                              |
| &nbsp;&nbsp;&nbsp;&nbsp;previousAddress         | `hash`   | the address where the tenant lived or traded before. Can be nil if unknown. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;street      | `string` | street name including the building number where appropriate                 |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;zipCode     | `string` |                                                                             |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;city        | `string` |                                                                             |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;countryCode | `string` | ISO country code, eg 'CH'                                                   |
| &nbsp;&nbsp;lettingContactReference             | `string` | person reference. (1)                                                       |

Notes

* (1) uniquely identifies the management team member who represents the property owner regarding this tenancy.

#### Additional Header Properties

[local_authority](/header_properties.md/#AdditionalHeaderProperties)

#### Example

```json
{
  "eventType": "Letting.Tenancy.MoveInConfirmed",
  "data": {
    "tenancyStartDate": "2019-01-01",
    "tenancyAgreementReference": "6020.03.0001.07",
    "unitReference": "6020.02.0101",
    "tenant": {
      "reference": "9913",
      "previousAddress": {
        "street": "Huobmatt 7a",
        "zipCode": "4005",
        "city": "Olten",
        "countryCode": "CH"
      }
    },
    "lettingContactReference": "ruf134",
    "isTest": null
  }
}
```

### Letting.Tenancy.MoveOutConfirmed

| Field                                           | Type     | Content / Remarks                                                            |
| ----------------------------------------------- | -------- | ---------------------------------------------------------------------------- |
| eventType                                       | `string` | Letting.Tenancy.MoveOutConfirmed                                             |
| data                                            | `hash`   |                                                                              |
| &nbsp;&nbsp;tenancyEndDate                      | `string` | ISO 8601 encoded date, eg '2019-05-30'                                       |
| &nbsp;&nbsp;tenancyAgreementReference           | `string` | unique tenancy agreement identifier, eg '1234.01.0001.01'                    |
| &nbsp;&nbsp;unitReference                       | `string` | unique unit identifier, eg '234.01.0001'                                     |
| &nbsp;&nbsp;tenant                              | `hash`   |                                                                              |
| &nbsp;&nbsp;&nbsp;&nbsp;reference               | `string` | tenant reference; uniquely identifies a person                               |
| &nbsp;&nbsp;&nbsp;&nbsp;nextAddress             | `hash`   | the address where the tenant will live or trade next. Can be nil if unknown. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;street      | `string` | street name including the building number where appropriate                  |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;zipCode     | `string` |                                                                              |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;city        | `string` |                                                                              |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;countryCode | `string` | ISO country code, eg 'CH'                                                    |
| &nbsp;&nbsp;lettingContactReference             | `string` | person reference. (1)                                                        |

Notes

* (1) uniquely identifies the management team member who represents the property owner regarding this tenancy.

#### Additional Header Properties

[local_authority](/header_properties.md/#AdditionalHeaderProperties)

#### Example

```json
TODO
```

### Letting.Reservation.Update

| Field                                  | Type      | Content / Remarks                                                             |
| -------------------------------------- | --------- | ----------------------------------------------------------------------------- |
| eventType                              | `string`  | Letting.Reservation.Update                                                    |
| data                                   | `hash`    |                                                                               |
| &nbsp;&nbsp;unitReference              | `string`  | unique unit identifier, eg '234.01.0001'                                      |
| &nbsp;&nbsp;reserve                    | `boolean` | reserve the unit if true, free the unit if false                              |
| &nbsp;&nbsp;reservedForPersonReference | `string`  | person the unit is reserved for. reference; uniquely identifies a person. (1) |
| &nbsp;&nbsp;reservationTypeCode        | `string`  | reservation type code ("Objektreservierungsart"). (1)                         |
| &nbsp;&nbsp;reservationReason          | `string`  | reservation reason. (1)                                                       |

Notes:

* (1) If `reserve` is set to `false`, you must not use this field.

#### Example

```json
{
  "eventType":"Letting.Reservation.Update",
  "data": {
    "reserve": true,
    "unitReference": "10001.353.423",
    "reservationTypeCode": "01",
    "reservedForPersonReference": "100004",
    "reservationReason": "reason"
  }
}
```

#### Letting.Reservation.UpdateAccepted

The [Accept](./result_messages.md#accepted-message) message.

#### Letting.Reservation.UpdateRejected

The [Reject](./result_messages.md#rejected-message) message.

### Letting.TenancyAgreementSecurityDepot.Update

| Field                                 | Type      | Content / Remarks                                         |
| ------------------------------------- | --------- | --------------------------------------------------------- |
| eventType                             | `string`  | Letting.TenancyAgreementSecurityDepot.Update              |
| data                                  | `hash`    |                                                           |
| &nbsp;&nbsp;tenancyAgreementReference | `string`  | tenancy agreement reference; **required**                 |
| &nbsp;&nbsp;depositTypeCode           | `string`  | deposit type code ("Depot-Art"); **required**             |
| &nbsp;&nbsp;custodianReference        | `string`  | person that is the custodian for the depot; reference (1) |
| &nbsp;&nbsp;payerReference            | `string`  | person that pays the depot; reference (1)                 |
| &nbsp;&nbsp;depositAmount             | `decimal` | amount to pay in CHF (1)                                  |
| &nbsp;&nbsp;paidAmount                | `decimal` | paid amount in CHF (1)                                    |
| &nbsp;&nbsp;depositAccountNumber      | `string`  | payment information. freetext, use e.g. for IBAN (1)      |

Notes:

* (1) Whether the field is required depends on the configuration of your depositTypeCode.

#### Example

```json
{
  "eventType":"Letting.Reservation.Update",
  "data": {
    "tenancyAgreementReference": "10001.869.474.01",
    "depositTypeCode": "1",
    "custodianReference": "100006",
    "payerReference": "100007",
    "depositAmount": "10",
    "paidAmount": "0",
    "depositAccountNumber": "CH00 0000 0000 0000 0000 0"
  }
}
```

#### Letting.TenaancyAgreementSecurityDepot.UpdateAccepted

The [Accept](./result_messages.md#accepted-message) message.

#### Letting.TenaancyAgreementSecurityDepot.UpdateRejected

The [Reject](./result_messages.md#rejected-message) message.

### Letting.TenancyAgreementDetails.Update

| Field                                 | Type      | Content / Remarks                                            |
| ------------------------------------- | --------- | ------------------------------------------------------------ |
| eventType                             | `string`  | Letting.TenancyAgreementDetails.Update                       |
| data                                  | `hash`    |                                                              |
| &nbsp;&nbsp;tenancyAgreementReference | `string`  | tenancy agreement identifier, eg '234.01.0001'; **required** |
| &nbsp;&nbsp;liabilityInsuranceChecked | `boolean` | whether the necessary liability insurance checks were done.  |

#### Example

```json
{
  "eventType":"Letting.TenancyAgreementDetails.Update",
  "data": {
    "tenancyAgreementReference": "10001.349.769.01",
    "liabilityInsuranceChecked": true
  }
}
```
