# Letting Context

## Events

| Type                                                                                                | GARAIO REM         | REM                | Description                                                                    |
| --------------------------------------------------------------------------------------------------- | ------------------ | ------------------ | ------------------------------------------------------------------------------ |
| [Letting.Tenancy.Created](#lettingtenancycreated)                                                   | :white_check_mark: | :x:                | A tenancy has been created; does not reliably signal a tenant move in. (1)     |
| [Letting.Tenancy.Updated](#lettingtenancyupdated)                                                   | :white_check_mark: | :x:                | Start and / or end date of a tenancy have been changed                         |
| [Letting.Tenancy.Deleted](#lettingtenancydeleted)                                                   | :white_check_mark: | :x:                | A tenancy has been deleted; this means that the tenancy never became effective |
| [Letting.Tenancy.TenancyAgreementReferenceChanged](#lettingtenancytenancyagreementreferencechanged) | :white_check_mark: | :x:                | The reference of a tenancy agreement has changed                               |
| [Letting.Reservation.Update](#lettingreservationupdate)                                             | :white_check_mark: | :x:                | Updates the reservation status of a unit.                                      |
| [Letting.TenancyAgreement.Create](#lettingtenancyagreementcreate)                                   | :white_check_mark: | :x:                | A tenancy agreement should be created in GARAIO REM                            |
| [Letting.TenancyAgreement.Activate](#lettingtenancyagreementactivate)                               | :white_check_mark: | :x:                | A tenancy agreement should be activated in GARAIO REM                          |
| [Letting.TenancyAgreement.Delete](#lettingtenancyagreementdelete)                                   | :white_check_mark: | :x:                | A tenancy agreement should be deleted in GARAIO REM                            |
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
| &nbsp;&nbsp;unitReference              | `string`  | unique unit identifier, eg '234.01.0001'; **required**                        |
| &nbsp;&nbsp;reserve                    | `boolean` | reserve the unit if `true`, free the unit if `false`; **required**            |
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

Additional `data` fields:

| Field           | Type     | Content / Remarks                               |
| --------------- | -------- | ----------------------------------------------- |
| `unitReference` | `string` | The unit reference given in the request message |

#### Letting.Reservation.UpdateRejected

The [Reject](./result_messages.md#rejected-message) message.

Additional `data` fields:

| Field           | Type     | Content / Remarks                               |
| --------------- | -------- | ----------------------------------------------- |
| `unitReference` | `string` | The unit reference given in the request message |

### Letting.TenancyAgreement.Create

This message is sent from an external message publisher to a GARAIO REM instance and allows to create a tenancy agreement. Set the recipient property in the headers, eg "grem_wincasa". Always required attributes are noted in the remarks. Depending on the tenancy agreement template configuration, additional attributes may be required.

GARAIO REM replies with a standard [Accepted](./result_messages.md#accepted-message) / [Rejected](./result_messages.md#rejected-message) message containing the `tenancyAgreementReference` and VAT state infos of the created tenancy agreement or reject reasons

Field | Type | Content / Remarks
---|---|---
eventType | string | Letting.TenancyAgreement.Create
data | hash |
&nbsp;&nbsp;primaryUnitReference | string | reference of an available unit; **required**
&nbsp;&nbsp;tenancyAgreementTypeCode | string | a valid tenancy agreement type code (see code table entries (mietvertrag_typ)for valid codes);  **required**
&nbsp;&nbsp;rentStartDate | string | ISO 8601 encoded date, eg '2019-03-01'; **required**
&nbsp;&nbsp;contractDate | string | ISO 8601 encoded date, eg '2019-02-18'; **required**
&nbsp;&nbsp;paymentModeCode | string | a valid code from the zahlmodus code table; defaults to '01' (monthly in advance)
&nbsp;&nbsp;primaryTenant | hash | data describing the primary tenant; a new tenant will be created if no tenant with the same name and dateOfBirth exists; **required**
&nbsp;&nbsp;&nbsp;&nbsp;firstName | string | first name; **required**
&nbsp;&nbsp;&nbsp;&nbsp;surname | string | surname; **required**
&nbsp;&nbsp;&nbsp;&nbsp;address | hash | current address
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;city | string | city; **required**
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;zipCode | string | zipCode; **required**
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;street | string | street incl. number; **required**
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;countryCode | string | ISO country code, eg 'CH'; defaults to 'CH'
&nbsp;&nbsp;&nbsp;&nbsp;correspondenceLanguageCode | string | de, fr, it or en; **must be lower case, required**
&nbsp;&nbsp;&nbsp;&nbsp;tenantIndustryCode | string | a valid rental type code (see code table entries (branche_mieter) for valid codes)
&nbsp;&nbsp;&nbsp;&nbsp;email | string | email address
&nbsp;&nbsp;&nbsp;&nbsp;phoneNumber | string | phone number (international format)
&nbsp;&nbsp;&nbsp;&nbsp;maritalStatus | string | one of the following values will be accepted: `unmarried`, `marrided`, `widowed`, `divorced`, `separated`, `civil_union`
&nbsp;&nbsp;&nbsp;&nbsp;dateOfBirth | string | ISO 8601 encoded date, eg '2019-03-01'; **required**
&nbsp;&nbsp;&nbsp;&nbsp;homeTown | string | home town
&nbsp;&nbsp;&nbsp;&nbsp;nationalityCode | string | ISO country code, eg 'CH'
&nbsp;&nbsp;&nbsp;&nbsp;jobTitle | string | job title
&nbsp;&nbsp;&nbsp;&nbsp;salutation | string | one of the following values will be accepted: `none`, `sir`, `madam`
&nbsp;&nbsp;&nbsp;&nbsp;iban | string | a valid IBAN for payouts
&nbsp;&nbsp;jointTenants | array | data describing optional joint tenants; new tenants will be created if no tenant with the same name and dateOfBirth exists
&nbsp;&nbsp;&nbsp;&nbsp;firstName | string | first name; **required**
&nbsp;&nbsp;&nbsp;&nbsp;surname | string | surname; **required**
&nbsp;&nbsp;&nbsp;&nbsp;address | hash | current address
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;city | string | city; **required**
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;zipCode | string | zipCode; **required**
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;street | string | street incl. number; **required**
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;countryCode | string | ISO country code, eg 'CH';  **required**
&nbsp;&nbsp;&nbsp;&nbsp;correspondenceLanguageCode | string | de, fr, it or en; **must be lower case, required**
&nbsp;&nbsp;&nbsp;&nbsp;email | string | email address
&nbsp;&nbsp;&nbsp;&nbsp;phoneNumber | string | phone number (international format)
&nbsp;&nbsp;&nbsp;&nbsp;maritalStatus | string | one of the following values will be accepted: `unmarried`, `marrided`, `widowed`, `divorced`, `separated`, `civil_union`
&nbsp;&nbsp;&nbsp;&nbsp;dateOfBirth | string | ISO 8601 encoded date, eg '2019-03-01'; **required**
&nbsp;&nbsp;&nbsp;&nbsp;homeTown | string | home town
&nbsp;&nbsp;&nbsp;&nbsp;nationalityCode | string | ISO country code, eg 'CH'
&nbsp;&nbsp;&nbsp;&nbsp;jobTitle | string | job title
&nbsp;&nbsp;&nbsp;&nbsp;salutation | string | one of the following values will be accepted: `none`, `sir`, `madam`
&nbsp;&nbsp;rentRegulations | hash | optional rent regulations
&nbsp;&nbsp;&nbsp;&nbsp;rentalTypeCode | string | a valid rental type code (see code table entries (mieter_art) for valid codes)
&nbsp;&nbsp;&nbsp;&nbsp;rentLockedUntil | string | ISO 8601 encoded date, eg '2019-03-01'
&nbsp;&nbsp;&nbsp;&nbsp;rentLockedReason | string | reason why the rent is locked
&nbsp;&nbsp;limitedUntil | string | optional ISO 8601 encoded date, eg '2019-03-31'; if applied, valid cancellationRegulations must be applied, too
&nbsp;&nbsp;cancellationRegulations | hash | cancellation regulations
&nbsp;&nbsp;&nbsp;&nbsp;cancellationModeCode | string | a valid cancellation mode code (see code table entries (kuendigungs_termin) for valid codes)
&nbsp;&nbsp;&nbsp;&nbsp;cancellationPeriodInMonths | integer | minimum number of months a cancellation must be announced in advance by the landlord
&nbsp;&nbsp;&nbsp;&nbsp;cancellationPeriodTenantInMonths | integer | minimum number of months a cancellation must be announced in advance by the tenant
&nbsp;&nbsp;&nbsp;&nbsp;earliestPossibleTerminationDateLandlord | string | ISO 8601 encoded date, eg '2019-03-01'
&nbsp;&nbsp;&nbsp;&nbsp;earliestPossibleTerminationDatesTenant | string | ISO 8601 encoded date, eg '2019-03-01'
&nbsp;&nbsp;securityDeposit | hash | security deposit
&nbsp;&nbsp;&nbsp;&nbsp;depositTypeCode | string | a valid deposit type code (see code table entries (depot_art) for valid codes)
&nbsp;&nbsp;&nbsp;&nbsp;depositAmount | decimal | amount to pay in CHF
&nbsp;&nbsp;&nbsp;&nbsp;paidAmount | decimal | paid amount in CHF
&nbsp;&nbsp;&nbsp;&nbsp;bankGuaranteeExpiration | string | ISO 8601 encoded date, eg '2023-12-31'
&nbsp;&nbsp;&nbsp;&nbsp;refundedAt | string | ISO 8601 encoded date, eg '2024-12-31'
&nbsp;&nbsp;&nbsp;&nbsp;depositAccountNumber | string | payment information. freetext, use e.g. for IBAN
&nbsp;&nbsp;&nbsp;&nbsp;custodianReference | string | person reference of the custodian
&nbsp;&nbsp;&nbsp;&nbsp;payerReference | string | person reference of the payer
&nbsp;&nbsp;intendedUse | hash | intended use
&nbsp;&nbsp;&nbsp;&nbsp;numberOfPeople | integer | number of people
&nbsp;&nbsp;&nbsp;&nbsp;installationsForCommonUsage | string | freetext describing installations for common usage
&nbsp;&nbsp;&nbsp;&nbsp;roomsForSoleUsage | array | rooms for sole usage
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;unitTypeCode | string | a valid unit type code (see code table entries (objekt_art) for valid codes)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;number | string | unit number
&nbsp;&nbsp;initialRentForm | hash | initial rent form data
&nbsp;&nbsp;&nbsp;&nbsp;reason | string | Reason for the rent
&nbsp;&nbsp;&nbsp;&nbsp;withSupportContribution | boolean | has the tenancy agreement support contribution?

#### Examples

##### minimal required attributes

```json
{"eventType":"Letting.TenancyAgreement.Create",
  "data":{
    "primaryUnitReference":"1234.01.0001",
    "tenancyAgreementTypeCode":"1",
    "rentStartDate":"2023-06-01",
    "contractDate":"2023-05-15",
    "paymentModeCode":"03",
    "primaryTenant":{
      "firstName":"Max",
      "surname":"Muster",
      "address":{
        "street":"Bahnhofstrasse 23",
        "zipCode":"3000",
        "city":"Bern",
        "countryCode":"CH",
      },
      "correspondenceLanguageCode":"de",
      "dateOfBirth":"1972-11-23"
    }
  }
}
```

##### all available attributes

```json
{"eventType":"Letting.TenancyAgreement.Create",
  "data":{
    "primaryUnitReference":"1234.01.0001",
    "tenancyAgreementTypeCode":"1",
    "rentStartDate":"2023-06-01",
    "primaryTenant":{
      "firstName":"Max",
      "surname":"Muster",
      "address":{
        "street":"Bahnhofstrasse 23",
        "zipCode":"3000",
        "city":"Bern",
        "countryCode":"CH",
      },
      "tenantIndustryCode":"01",
      "correspondenceLanguageCode":"de",
      "dateOfBirth":"1972-11-23",
      "email":"max.muster@gmail.com",
      "phoneNumber":"+41 123 45 67",
      "maritalStatus":"civil_union",
      "homeTown":"Bern",
      "nationalityCode":"AT",
      "jobTitle":"software engineer",
      "salutation":"sir",
      "iban":"CH9531999000000001234"
    },
    "jointTenants":[
      {
        "firstName":"Jeanine",
        "surname":"Gibtsnicht",
        "address":{
          "street":"Freiestrasse 2a",
          "zipCode":"5000",
          "city":"Aarau",
          "countryCode":"CH",
        },
        "correspondenceLanguageCode":"de",
        "dateOfBirth":"1981-03-12",
        "email":"jeanine.gibtsnicht@gmail.com",
        "phoneNumber":"+41 765 43 21",
        "maritalStatus":"civil_union",
        "homeTown":"Aarau",
        "nationalityCode":"DE",
        "jobTitle":"Medizinische Praxisassistentin",
        "salutation":"madam"
      }
    ],
    "cancellationRegulations":{
      "cancellationModeCode":"01",
      "cancellationPeriodInMonths":3,
      "cancellationPeriodTenantInMonths":3,
      "earliestPossibleTerminationDateLandlord":"2023-12-31",
      "earliestPossibleTerminationDatesTenant":"2023-12-31"
    },
    "securityDeposit":{
      "depositTypeCode":"01",
      "depositAmount":5000.00,
      "paidAmount":5000.00,
      "depositAccountNumber":"DE21700500000003600282",
      "custodianReference":"123456",
      "payerReference":"654321"
    },
    "intendedUse":{
      "numberOfPeople":2,
      "installationsForCommonUsage":"Veloraum",
      "roomsForSoleUsage":[
        {
          "unitTypeCode":"26",
          "number":"1234"
        }
      ]
    },
    "initialRentForm": {
      "reason":"Erstvermietung",
      "withSupportContribution":false,
    }
  }
}
```

##### accepted response message

```json
{"eventType":"Letting.TenancyAgreement.CreateAccepted",
  "data":{
    "tenancyAgreementReference":"1234.01.0001.02",
    "vat":{
      "validFrom":"2023-06-01",
      "liable":false,
      "code":"NO",
      "primaryTenancyAgreementReference":null
    }
  }
}
```

##### rejected response message

```json
{"eventType":"Letting.TenancyAgreement.CreateRejected",
  "data":{
    "reasons":[
      {
        "attribute":"primaryTenant.correspondenceLanguageCode",
        "reason":"darf nicht leer sein"
      },
      {
        "attribute":"primaryTenant.surname",
        "reason":"darf nicht leer sein"
      }
    ]
  }
}
```

### Letting.TenancyAgreement.Activate

Activate a tenancy agreement in GREM.

| Field                 | Type     | Content / Remarks                         |
| --------------------- | -------- | ----------------------------------------- |
| eventType             | `string` | Letting.TenancyAgreement.Activate         |
| data                  | `hash`   |                                           |
| &nbsp;&nbsp;reference | `string` | tenancy agreement reference; **required** |

#### Letting.TenancyAgreement.ActivateAccepted

The [Accept](./result_messages.md#accepted-message) message.

Additional `data` fields:

| Field       | Type     | Content / Remarks               |
| ----------- | -------- | ------------------------------- |
| `reference` | `string` | The tenancy agreement reference |

#### Letting.TenancyAgreement.ActivateRejected

The [Reject](./result_messages.md#rejected-message) message.

Additional `data` fields:

| Field       | Type     | Content / Remarks               |
| ----------- | -------- | ------------------------------- |
| `reference` | `string` | The tenancy agreement reference |

### Letting.TenancyAgreement.Delete

Delete requests for activated tenancy agreements will be rejected (only agreements in states `in_erfassung` and `validiert` are deletable).

| Field                 | Type     | Content / Remarks                         |
| --------------------- | -------- | ----------------------------------------- |
| eventType             | `string` | Letting.TenancyAgreement.Delete           |
| data                  | `hash`   |                                           |
| &nbsp;&nbsp;reference | `string` | tenancy agreement reference; **required** |

#### Letting.TenancyAgreement.DeleteAccepted

The [Accept](./result_messages.md#accepted-message) message.

Additional `data` fields:

| Field       | Type     | Content / Remarks               |
| ----------- | -------- | ------------------------------- |
| `reference` | `string` | The tenancy agreement reference |

#### Letting.TenancyAgreement.DeleteRejected

The [Reject](./result_messages.md#rejected-message) message.

Additional `data` fields:

| Field       | Type     | Content / Remarks               |
| ----------- | -------- | ------------------------------- |
| `reference` | `string` | The tenancy agreement reference |

### Letting.TenancyAgreementSecurityDepot.Update

| Field                            | Type      | Content / Remarks                                         |
| -------------------------------- | --------- | --------------------------------------------------------- |
| eventType                        | `string`  | Letting.TenancyAgreementSecurityDepot.Update              |
| data                             | `hash`    |                                                           |
| &nbsp;&nbsp;reference            | `string`  | tenancy agreement reference; **required**                 |
| &nbsp;&nbsp;depositTypeCode      | `string`  | deposit type code ("Depot-Art"); **required**             |
| &nbsp;&nbsp;custodianReference   | `string`  | person that is the custodian for the depot; reference (1) |
| &nbsp;&nbsp;payerReference       | `string`  | person that pays the depot; reference (1)                 |
| &nbsp;&nbsp;depositAmount        | `decimal` | amount to pay in CHF (1)                                  |
| &nbsp;&nbsp;paidAmount           | `decimal` | paid amount in CHF (1)                                    |
| &nbsp;&nbsp;depositAccountNumber | `string`  | payment information. freetext, use e.g. for IBAN (1)      |

Notes:

* (1) Whether the field is required depends on the configuration of your depositTypeCode.

#### Example

```json
{
  "eventType":"Letting.Reservation.Update",
  "data": {
    "reference": "10001.869.474.01",
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

Additional `data` fields:

| Field       | Type     | Content / Remarks               |
| ----------- | -------- | ------------------------------- |
| `reference` | `string` | The tenancy agreement reference |

#### Letting.TenaancyAgreementSecurityDepot.UpdateRejected

The [Reject](./result_messages.md#rejected-message) message.

Additional `data` fields:

| Field       | Type     | Content / Remarks               |
| ----------- | -------- | ------------------------------- |
| `reference` | `string` | The tenancy agreement reference |

### Letting.TenancyAgreementDetails.Update

| Field                                 | Type      | Content / Remarks                                            |
| ------------------------------------- | --------- | ------------------------------------------------------------ |
| eventType                             | `string`  | Letting.TenancyAgreementDetails.Update                       |
| data                                  | `hash`    |                                                              |
| &nbsp;&nbsp;reference                 | `string`  | tenancy agreement identifier, eg '234.01.0001'; **required** |
| &nbsp;&nbsp;liabilityInsuranceChecked | `boolean` | whether the necessary liability insurance checks were done.  |

#### Example

```json
{
  "eventType":"Letting.TenancyAgreementDetails.Update",
  "data": {
    "reference": "10001.349.769.01",
    "liabilityInsuranceChecked": true
  }
}
```

#### Letting.TenancyAgreementDetails.UpdateAccepted

The [Accept](./result_messages.md#accepted-message) message.

Additional `data` fields:

| Field       | Type     | Content / Remarks               |
| ----------- | -------- | ------------------------------- |
| `reference` | `string` | The tenancy agreement reference |

#### Letting.TenancyAgreementDetails.UpdateRejected

The [Reject](./result_messages.md#rejected-message) message.

Additional `data` fields:

| Field       | Type     | Content / Remarks               |
| ----------- | -------- | ------------------------------- |
| `reference` | `string` | The tenancy agreement reference |
