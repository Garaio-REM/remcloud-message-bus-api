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
| [Letting.Tenant.Updated](#lettingtenantupdated)                                                     | :white_check_mark: | :x:                | Notification when tenant information has been updated/changed                  |

Notes

* (1) A tenancy is uniquely identified by tenancy agreement reference, tenant reference and unit reference
* (2) This event is only raised for tenants that live or trade in a given unit. For example the event is not raised for tenants that act as guarantors or for tenants that have had their tenancy agreement changed while staying in the same unit.
* (3) Like Letting.Tenancy.MoveInConfirmed the event is also only raised when a tenant has lived or traded in person at the given unit.

### Letting.Tenancy.Created

NOTE: We have discoved minor differences in the logic for sending the _preferred_ `email` and `phoneNumber` in mbus messages, the priority is always the same for all persons, while GraphQL queries send different values Legal and Physical persons.  At the moment, we are not planning to change this behaviour without consulting our partners, in order to prevent unexpected side effects.

| Field                                           | Type              | Content / Remarks                                                                           |
| ----------------------------------------------- | ----------------- | ------------------------------------------------------------------------------------------- |
| eventType                                       | `string`          | Letting.Tenancy.Created                                                                     |
| data                                            | `hash`            |                                                                                             |
| &nbsp;&nbsp;startDate                           | `string`          | ISO 8601 encoded date, eg '2019-05-25'                                                      |
| &nbsp;&nbsp;endDate                             | `string`          | ISO 8601 encoded date, eg '2019-05-25'; might be null                                       |
| &nbsp;&nbsp;tenancyAgreementReference           | `string`          | unique tenancy agreement identifier, eg '1234.01.0001.01'                                   |
| &nbsp;&nbsp;unitReference                       | `string`          | unique unit identifier, eg '234.01.0001'                                                    |
| &nbsp;&nbsp;tenant                              | `hash`            |                                                                                             |
| &nbsp;&nbsp;&nbsp;&nbsp;reference               | `string`          | tenant reference; uniquely identifies a person                                              |
| &nbsp;&nbsp;&nbsp;&nbsp;firstName               | `string`          |                                                                                             |
| &nbsp;&nbsp;&nbsp;&nbsp;surname                 | `string`          |                                                                                             |
| &nbsp;&nbsp;&nbsp;&nbsp;languageCode            | `string`          | de, fr, it or en; **must be lower case**                                                    |
| &nbsp;&nbsp;&nbsp;&nbsp;nationalityCode         | `string`          | ISO country code (ISO 3166-1 alpha-2), eg 'CH'                                              |
| &nbsp;&nbsp;&nbsp;&nbsp;phoneNumber             | `string`          | might be null                                                                               |
| &nbsp;&nbsp;&nbsp;&nbsp;email                   | `string`          | might be null                                                                               |
| &nbsp;&nbsp;&nbsp;&nbsp;fullName                | `string`          | built from the individual name parts, respecting the type of tenant (corporate or physical) |
| &nbsp;&nbsp;&nbsp;&nbsp;type                    | `string`          | LEGAL (a company) or PHYSICAL (Physical person)                                             |
| &nbsp;&nbsp;&nbsp;&nbsp;dateOfBirth             | `string`          | ISO 8601 encoded date, eg '2019-05-30'                                                      |
| &nbsp;&nbsp;&nbsp;&nbsp;allphoneNumbers         | `array of hashes` | a list of all available email addresses and type                                            |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;phoneNumber       | `string`          |                                                                                             |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;type              | `string`          | one of: PRIVATE, PROFESSIONAL, MOBILE or OTHER                                              |
| &nbsp;&nbsp;&nbsp;&nbsp;allEmails               | `array of hashes` | a list of all available email addresses and type                                            |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;emailAddress      | `string`          |                                                                                             |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;type              | `string`          | one of: PRIVATE, PROFESSIONAL or OTHER                                                      |
| &nbsp;&nbsp;&nbsp;&nbsp;postalAddress           | `hash`            | current address fields conformant to the eCH-0010 specs                                     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;addressLine1      | `string`          | See eCH-0010 specs                                                                          |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;countryCode       | `string`          | ISO 3166-1 alpha-2 country code, eg CH                                                      |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;city              | `string`          |                                                                                             |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;zipCode           | `string`          |                                                                                             |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;postOfficeBoxText | `string`          | See eCH-0010 specs                                                                          |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;street            | `string`          | Street name including number where appropriate                                              |

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
      "email":"name@home-mail.xy",
      "fullName":"Haupt Mieter",
      "type":"PHYSICAL",
      "dateOfBirth":"1980-01-01",
      "allphoneNumbers":[
        {"phoneNumber":"+41 31 331 21 11","type":"PRIVATE"},
        {"phoneNumber":"+41 31 331 21 12","type":"PROFESSIONAL"},
        {"phoneNumber":"+41 31 331 21 13","type":"MOBILE"},
        {"phoneNumber":"+41 31 331 21 14","type":"OTHER"}
      ],
      "allEmails":[
        {"emailAddress":"name@home-mail.xy","type":"PRIVATE"},
        {"emailAddress":"username@work-mail.yz","type":"PROFESSIONAL"}
      ],
      "postalAddress":{
        "addressLine1":"Haupt Mieter",
        "countryCode":"CH",
        "city":"Bern",
        "zipCode":"3000",
        "postOfficeBoxText":"Postfach 1234",
        "street":"Hauptstrasse 1"
      }
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

GARAIO REM replies with a standard [Accepted](./result_messages.md#accepted-message) / [Rejected](./result_messages.md#rejected-message) message containing the `tenancyAgreementReference`, VAT state infos and rental costs of the created tenancy agreement or reject reasons

| Field                                                             | Type      | Content / Remarks                                                                                                                                                                              |
| ----------------------------------------------------------------- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `eventType`                                                       | `string`  | Letting.TenancyAgreement.Create                                                                                                                                                                |
| `data`                                                            | `hash`    |                                                                                                                                                                                                |
| &nbsp;&nbsp;`primaryUnitReference`                                | `string`  | reference of an available unit; **required**                                                                                                                                                   |
| &nbsp;&nbsp;`tenancyAgreementTypeCode`                            | `string`  | a valid tenancy agreement type code (see code table entries (`mietvertrag_typ`) for valid codes);  **required**                                                                                |
| &nbsp;&nbsp;`rentStartDate`                                       | `string`  | ISO 8601 encoded date, eg '2019-03-01'; **required**                                                                                                                                           |
| &nbsp;&nbsp;`contractDate`                                        | `string`  | ISO 8601 encoded date, eg '2019-02-18'; **required**                                                                                                                                           |
| &nbsp;&nbsp;`paymentModeCode`                                     | `string`  | a valid code from the zahlmodus code table; defaults to '01' (monthly in advance)                                                                                                              |
| &nbsp;&nbsp;`primaryTenant`                                       | `hash`    | data describing the primary tenant; a new tenant will be created if no tenant with the same name and dateOfBirth exists; **required**                                                          |
| &nbsp;&nbsp;&nbsp;&nbsp;`firstName`                               | `string`  | first name; **required**                                                                                                                                                                       |
| &nbsp;&nbsp;&nbsp;&nbsp;`surname`                                 | `string`  | surname; **required**                                                                                                                                                                          |
| &nbsp;&nbsp;&nbsp;&nbsp;`address`                                 | `hash`    | current address                                                                                                                                                                                |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`city`                        | `string`  | city; **required**                                                                                                                                                                             |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`zipCode`                     | `string`  | zipCode; **required**                                                                                                                                                                          |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`street`                      | `string`  | street incl. number; **required**                                                                                                                                                              |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`countryCode`                 | `string`  | ISO country code, eg 'CH'; **required**                                                                                                                                                        |
| &nbsp;&nbsp;&nbsp;&nbsp;`correspondenceLanguageCode`              | `string`  | de, fr, it or en; **must be lower case, required**                                                                                                                                             |
| &nbsp;&nbsp;&nbsp;&nbsp;`tenantIndustryCode`                      | `string`  | a valid rental type code (see code table entries (`branche_mieter`) for valid codes)                                                                                                           |
| &nbsp;&nbsp;&nbsp;&nbsp;`email`                                   | `string`  | email address                                                                                                                                                                                  |
| &nbsp;&nbsp;&nbsp;&nbsp;`phoneNumber`                             | `string`  | phone number (international format)                                                                                                                                                            |
| &nbsp;&nbsp;&nbsp;&nbsp;`maritalStatus`                           | `string`  | one of the following values will be accepted: `unmarried`, `married`, `widowed`, `divorced`, `separated`, `civil_union`. Send _either_ `maritalStatus` _or_ `maritalStatusCode` but not both.  |
| &nbsp;&nbsp;&nbsp;&nbsp;`maritalStatusCode`                       | `string`  | a value of the marital status code table (see code table entries "Zivilstände" for valid codes). Send _either_ `maritalStatus` _or_ `maritalStatusCode` but not both.                          |
| &nbsp;&nbsp;&nbsp;&nbsp;`dateOfBirth`                             | `string`  | ISO 8601 encoded date, eg '2019-03-01'; **required**                                                                                                                                           |
| &nbsp;&nbsp;&nbsp;&nbsp;`homeTown`                                | `string`  | home town                                                                                                                                                                                      |
| &nbsp;&nbsp;&nbsp;&nbsp;`nationalityCode`                         | `string`  | ISO country code, eg 'CH'                                                                                                                                                                      |
| &nbsp;&nbsp;&nbsp;&nbsp;`jobTitle`                                | `string`  | job title                                                                                                                                                                                      |
| &nbsp;&nbsp;&nbsp;&nbsp;`salutation`                              | `string`  | one of the following values will be accepted: `none`, `sir`, `madam`. Send _either_ `salutation` _or_ `salutationCode` but not both.                                                           |
| &nbsp;&nbsp;&nbsp;&nbsp;`salutationCode`                          | `string`  | a value of the salutation code table (see code table entries "Anreden" for valid codes). Send _either_ `salutation` _or_ `salutationCode` but not both.                                        |
| &nbsp;&nbsp;&nbsp;&nbsp;`iban`                                    | `string`  | a valid IBAN for payouts                                                                                                                                                                       |
| &nbsp;&nbsp;&nbsp;&nbsp;`bic`                                     | `string`  | a valid BIC (Bank Identification Code) for payouts. For CH and LI IBANs, this field must be omitted or set to `null`. For other country IBANs, this field is required.                         |
| &nbsp;&nbsp;&nbsp;&nbsp;`assignBuildingAddress`                   | `boolean` | should the building address be assigned per rent start date?                                                                                                                                   |
| &nbsp;&nbsp;`jointTenants`                                        | `array`   | data describing optional joint tenants; new tenants will be created if no tenant with the same name and dateOfBirth exists                                                                     |
| &nbsp;&nbsp;&nbsp;&nbsp;`firstName`                               | `string`  | first name; **required**                                                                                                                                                                       |
| &nbsp;&nbsp;&nbsp;&nbsp;`surname`                                 | `string`  | surname; **required**                                                                                                                                                                          |
| &nbsp;&nbsp;&nbsp;&nbsp;`address`                                 | `hash`    | current address                                                                                                                                                                                |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`city`                        | `string`  | city; **required**                                                                                                                                                                             |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`zipCode`                     | `string`  | zipCode; **required**                                                                                                                                                                          |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`street`                      | `string`  | street incl. number; **required**                                                                                                                                                              |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`countryCode`                 | `string`  | ISO country code, eg 'CH';  **required**                                                                                                                                                       |
| &nbsp;&nbsp;&nbsp;&nbsp;`correspondenceLanguageCode`              | `string`  | de, fr, it or en; **must be lower case, required**                                                                                                                                             |
| &nbsp;&nbsp;&nbsp;&nbsp;`email`                                   | `string`  | email address                                                                                                                                                                                  |
| &nbsp;&nbsp;&nbsp;&nbsp;`phoneNumber`                             | `string`  | phone number (international format)                                                                                                                                                            |
| &nbsp;&nbsp;&nbsp;&nbsp;`maritalStatus`                           | `string`  | one of the following values will be accepted: `unmarried`, `married`, `widowed`, `divorced`, `separated`, `civil_union`.  Send _either_ `maritalStatus` _or_ `maritalStatusCode` but not both. |
| &nbsp;&nbsp;&nbsp;&nbsp;`maritalStatusCode`                       | `string`  | a value of the marital status code table (see code table entries "Zivilstände" for valid codes). Send _either_ `maritalStatus` _or_ `maritalStatusCode` but not both.                          |
| &nbsp;&nbsp;&nbsp;&nbsp;`dateOfBirth`                             | `string`  | ISO 8601 encoded date, eg '2019-03-01'; **required**                                                                                                                                           |
| &nbsp;&nbsp;&nbsp;&nbsp;`homeTown`                                | `string`  | home town                                                                                                                                                                                      |
| &nbsp;&nbsp;&nbsp;&nbsp;`nationalityCode`                         | `string`  | ISO country code, eg 'CH'                                                                                                                                                                      |
| &nbsp;&nbsp;&nbsp;&nbsp;`jobTitle`                                | `string`  | job title                                                                                                                                                                                      |
| &nbsp;&nbsp;&nbsp;&nbsp;`salutation`                              | `string`  | one of the following values will be accepted: `none`, `sir`, `madam`. Send _either_ `salutation` _or_ `salutationCode` but not both.                                                           |
| &nbsp;&nbsp;&nbsp;&nbsp;`salutationCode`                          | `string`  | a value of the salutation code table (see code table entries "Anreden" for valid codes). Send _either_ `salutation` _or_ `salutationCode` but not both.                                        |
| &nbsp;&nbsp;&nbsp;&nbsp;`assignBuildingAddress`                   | `boolean` | should the building address be assigned per rent start date?                                                                                                                                   |
| &nbsp;&nbsp;`rentRegulations`                                     | `hash`    | optional rent regulations                                                                                                                                                                      |
| &nbsp;&nbsp;&nbsp;&nbsp;`rentalTypeCode`                          | `string`  | a valid rental type code (see code table entries (`mieter_art`) for valid codes); **required**                                                                                                 |
| &nbsp;&nbsp;&nbsp;&nbsp;`rentLockedUntil`                         | `string`  | ISO 8601 encoded date, eg '2019-03-01'                                                                                                                                                         |
| &nbsp;&nbsp;&nbsp;&nbsp;`rentLockedReason`                        | `string`  | reason why the rent is locked                                                                                                                                                                  |
| &nbsp;&nbsp;`limitedUntil`                                        | `string`  | optional ISO 8601 encoded date, eg '2019-03-31'; if applied, valid cancellationRegulations must be applied, too                                                                                |
| &nbsp;&nbsp;`cancellationRegulations`                             | `hash`    | cancellation regulations                                                                                                                                                                       |
| &nbsp;&nbsp;&nbsp;&nbsp;`cancellationModeCode`                    | `string`  | a valid cancellation mode code (see code table entries (`kuendigungs_termin`) for valid codes)                                                                                                 |
| &nbsp;&nbsp;&nbsp;&nbsp;`cancellationPeriodInMonths`              | `integer` | minimum number of months a cancellation must be announced in advance by the landlord                                                                                                           |
| &nbsp;&nbsp;&nbsp;&nbsp;`cancellationPeriodTenantInMonths`        | `integer` | minimum number of months a cancellation must be announced in advance by the tenant                                                                                                             |
| &nbsp;&nbsp;&nbsp;&nbsp;`earliestPossibleTerminationDateLandlord` | `string`  | ISO 8601 encoded date, eg '2019-03-01'                                                                                                                                                         |
| &nbsp;&nbsp;&nbsp;&nbsp;`earliestPossibleTerminationDatesTenant`  | `string`  | ISO 8601 encoded date, eg '2019-03-01'                                                                                                                                                         |
| &nbsp;&nbsp;`securityDeposit`                                     | `hash`    | security deposit                                                                                                                                                                               |
| &nbsp;&nbsp;&nbsp;&nbsp;`depositTypeCode`                         | `string`  | a valid deposit type code (see code table entries (depot_art) for valid codes)                                                                                                                 |
| &nbsp;&nbsp;&nbsp;&nbsp;`depositAmount`                           | `decimal` | amount to pay in CHF                                                                                                                                                                           |
| &nbsp;&nbsp;&nbsp;&nbsp;`paidAmount`                              | `decimal` | paid amount in CHF                                                                                                                                                                             |
| &nbsp;&nbsp;&nbsp;&nbsp;`bankGuaranteeExpiration`                 | `string`  | ISO 8601 encoded date, eg '2023-12-31'                                                                                                                                                         |
| &nbsp;&nbsp;&nbsp;&nbsp;`refundedAt`                              | `string`  | ISO 8601 encoded date, eg '2024-12-31'                                                                                                                                                         |
| &nbsp;&nbsp;&nbsp;&nbsp;`depositAccountNumber`                    | `string`  | payment information. freetext, use e.g. for IBAN                                                                                                                                               |
| &nbsp;&nbsp;&nbsp;&nbsp;`custodianReference`                      | `string`  | person reference of the custodian                                                                                                                                                              |
| &nbsp;&nbsp;&nbsp;&nbsp;`payerReference`                          | `string`  | person reference of the payer                                                                                                                                                                  |
| &nbsp;&nbsp;`intendedUse`                                         | `hash`    | intended use                                                                                                                                                                                   |
| &nbsp;&nbsp;&nbsp;&nbsp;`numberOfPeople`                          | `integer` | number of people                                                                                                                                                                               |
| &nbsp;&nbsp;&nbsp;&nbsp;`installationsForCommonUsage`             | `string`  | freetext describing installations for common usage                                                                                                                                             |
| &nbsp;&nbsp;&nbsp;&nbsp;`roomsForSoleUsage`                       | `array`   | rooms for sole usage                                                                                                                                                                           |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`unitTypeCode`                | `string`  | a valid unit type code (see code table entries (`objekt_art`) for valid codes)                                                                                                                 |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`number`                      | `string`  | unit number                                                                                                                                                                                    |
| &nbsp;&nbsp;`initialRentForm`                                     | `hash`    | initial rent form data                                                                                                                                                                         |
| &nbsp;&nbsp;&nbsp;&nbsp;`reason`                                  | `string`  | Reason for the rent                                                                                                                                                                            |
| &nbsp;&nbsp;&nbsp;&nbsp;`withSupportContribution`                 | `boolean` | has the tenancy agreement support contribution?                                                                                                                                                |

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
      "iban":"CH9531999000000001234",
      "assignBuildingAddress": true
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
        "salutation":"madam",
        "assignBuildingAddress": false
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

| Field                                               | Type      | Content / Remarks                                                               |
| --------------------------------------------------- | --------- | ------------------------------------------------------------------------------- |
| `rentReserves`                                      | `hash`    | Rent reserves ("Mietzinsreserven")                                              |
| &nbsp;&nbsp;`fromRentBasePercent`                   | `decimal` | "Aufgrund Mietzinsbasis", in Percent                                            |
| &nbsp;&nbsp;`fromRentBaseAmount`                    | `decimal` | "Aufgrund Mietzinsbasis", in CHF                                                |
| &nbsp;&nbsp;`mortgageRateAdjustment`                | `decimal` | "Hypozinsanpassung", in Percent                                                 |
| &nbsp;&nbsp;`countryIndexAdjustment`                | `decimal` | "Landesindexanpassung", in Percent                                              |
| &nbsp;&nbsp;`costIncreaseAdjustment`                | `decimal` | "Kostensteigerungsanpassung", in Percent                                        |
| &nbsp;&nbsp;`fromInsufficientReturnPercent`          | `decimal` | "aufgrund ungenügender Bruttorendite", in Percent                               |
| &nbsp;&nbsp;`fromInsufficientReturnAmount`           | `decimal` | "aufgrund ungenügender Bruttorendite", in CHF                                   |
| &nbsp;&nbsp;`fromLocalStandardsPercent`             | `decimal` | "aufgrund Orts- und Quartierüblichkeit", in Percent                             |
| &nbsp;&nbsp;`fromLocalStandardsAmount`              | `decimal` | "aufgrund Orts- und Quartierüblichkeit", in CHF                                 |
| &nbsp;&nbsp;`fromValueAddingInvestmentsPercent`     | `decimal` | "aufgrund wertvermehrender Investitionen", in Percent                           |
| &nbsp;&nbsp;`fromValueAddingInvestmentsAmount`      | `decimal` | "aufgrund wertvermehrender Investitionen", in CHF                               |
| &nbsp;&nbsp;`fromValueAddingInvestmentsPotentials`  | `Array`   | Array of hashes (typeCode, amount, notes)                                       |
| &nbsp;&nbsp;`fromValueAddingInvestmentsTable`       | `Array`   | Array of hashes (title, percent, amount)                                        |

```json
{"eventType":"Letting.TenancyAgreement.CreateAccepted",
  "data":{
    "tenancyAgreementReference":"1234.01.0001.02",
    "vat":{
      "validFrom":"2023-06-01",
      "liable":false,
      "code":"NO",
      "rate":7.7,
      "primaryTenancyAgreementReference":null
    },
    "rentalCosts":{
      "netRent":2000.0,
      "grossRentExcludingVat": 2010.0,
      "additionalCosts":10.0,
      "additionalCostsComponents":[
        {"code":"10",
        "amount":10.0}
      ]
    },
    "rentalBasis":{
      "mortageRate":2.75,
      "mortageRateDate":"2005-01-01",
      "costIncreaseDate":"",
      "countryIndexDate":"",
      "countryIndexBaseYear":null,
      "countryIndexPoints":null
    },
    "paymentModeCode":"01",
    "rentReserves": {
      "fromRentBasePercent":8.24,
      "fromRentBaseAmount":62.698,
      "mortgageRateAdjustment":6.0,
      "countryIndexAdjustment":2.42,
      "costIncreaseAdjustment":0.0,
      "fromInsufficientReturnPercent":1.1,
      "fromInsufficientReturnAmount":1000.0,
      "fromLocalStandardsPercent":1.2,
      "fromLocalStandardsAmount":2000.0,
      "fromValueAddingInvestmentsPercent":1.3,
      "fromValueAddingInvestmentsAmount":3000.0,
      "fromValueAddingInvestmentsPotentials":[
        { "typeCode":"30",
          "amount":300.0
          "notes":null}
      ],
      "fromValueAddingInvestmentsTable":[
        { "title":"Mietzins-Reserve aufgrund wertvermehrender Investitionen",
          "percent": "5.9%",
          "amount": "44.00"}
      ]
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
  "eventType":"Letting.TenancyAgreementSecurityDepot.Update",
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

#### Letting.TenancyAgreementSecurityDepot.UpdateRejected

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

### Letting.Tenant.Updated

Letting.Tenant messages are sent when a tenant is updated or merged with another person.

**These messages are sent for EACH Tenancy Agreement that the tenant is linked to and only sent to relevant parties.** _This is accomplished (on the backend), by including the `propery` and `unit` so that we can check the associated Tags to filter for allowed message recipients.)_

Because we send a message for each Tenancy, we also send the following fields: `tenancyAgreementReference`, `unitReference`, `tenantReference` to ensure a successful lookup on the remote system. `tenant` fields are optional and will generally only be sent if they have changed. The possible Tenant fields to be sent should be the same as those in the [Letting.Tenant.Create](./#lettingtenantcreate) message. _(Please notify us if you find any discrepancies.)_

* When an address is added the full address data indluding a `validFrom` (in case it is an address for the future).
* When an address is deleted the full current address data will be sent (it is possible that an older address was deleted and the adress hasn't actually changed).
* when an address is changed (updated) we will only send the address fields that have changed (and the `validFrom` date - incase it is a future address)
* In the case of a removed **email** or **phoneNumber**, the corresponding preferred value will always be sent, as it is difficult to know if the preferred value has changed.
* In the case of a **tenant merge**, the full tenant data will be sent, as it is difficult to know what has changed, since the old record has already been removed at the time of building this message.  It is also important to note, that in the case of a merge the `reference` field will be changed and MUST be updated in order to match all future tenant references in ANY subsequent messages.

NOTE:

| Field                                           | Type              | Content / Remarks                                                                                                                                           |
| ----------------------------------------------- | ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| eventType                                       | `string`          | Letting.Tenant.Updated                                                                                                                                      |
| data                                            | `hash`            |                                                                                                                                                             |
| &nbsp;&nbsp;tenancyAgreementReference           | `string`          | unique tenancy agreement identifier, eg '1234.01.0001.01'                                                                                                   |
| &nbsp;&nbsp;unitReference                       | `string`          | unique unit identifier, eg '234.01.0001'                                                                                                                    |
| &nbsp;&nbsp;tenantReference                     | `string`          | unique tenant identifier, eg '100004' - this is ALWAYS the reference previously published                                                                   |
| &nbsp;&nbsp;tenant                              | `hash`            | ALL TENANT FIELDS except REFERENCE are optional and will only be send when changed                                                                          |
| &nbsp;&nbsp;&nbsp;&nbsp;reference               | `string`          | unique tenant identifier, eg '100004' - this is ALWAYS the reference to be persisted and the reference that will be used in future `tenantReference` values |
| &nbsp;&nbsp;&nbsp;&nbsp;firstName               | `string`          |                                                                                                                                                             |
| &nbsp;&nbsp;&nbsp;&nbsp;surname                 | `string`          |                                                                                                                                                             |
| &nbsp;&nbsp;&nbsp;&nbsp;languageCode            | `string`          | de, fr, it or en; **must be lower case**                                                                                                                    |
| &nbsp;&nbsp;&nbsp;&nbsp;nationalityCode         | `string`          | ISO country code (ISO 3166-1 alpha-2), eg 'CH'                                                                                                              |
| &nbsp;&nbsp;&nbsp;&nbsp;phoneNumber             | `string`          |                                                                                                                                                             |
| &nbsp;&nbsp;&nbsp;&nbsp;email                   | `string`          |                                                                                                                                                             |
| &nbsp;&nbsp;&nbsp;&nbsp;fullName                | `string`          | built from the individual name parts, respecting the type of tenant (corporate or physical)                                                                 |
| &nbsp;&nbsp;&nbsp;&nbsp;type                    | `string`          | LEGAL (a company) or PHYSICAL (Physical person)                                                                                                             |
| &nbsp;&nbsp;&nbsp;&nbsp;dateOfBirth             | `string`          | ISO 8601 encoded date, eg '2019-05-30'                                                                                                                      |
| &nbsp;&nbsp;&nbsp;&nbsp;allphoneNumbers         | `array of hashes` | a list of all available email addresses and type                                                                                                            |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;phoneNumber       | `string`          |                                                                                                                                                             |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;type              | `string`          | one of: PRIVATE, PROFESSIONAL, MOBILE or OTHER                                                                                                              |
| &nbsp;&nbsp;&nbsp;&nbsp;allEmails               | `array of hashes` | a list of all available email addresses and type                                                                                                            |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;emailAddress      | `string`          |                                                                                                                                                             |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;type              | `string`          | one of: PRIVATE, PROFESSIONAL or OTHER                                                                                                                      |
| &nbsp;&nbsp;&nbsp;&nbsp;postalAddress           | `hash`            | current address fields conformant to the eCH-0010 specs                                                                                                     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;addressLine1      | `string`          | See eCH-0010 specs                                                                                                                                          |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;countryCode       | `string`          | ISO 3166-1 alpha-2 country code, eg CH                                                                                                                      |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;city              | `string`          |                                                                                                                                                             |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;zipCode           | `string`          |                                                                                                                                                             |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;postOfficeBoxText | `string`          | See eCH-0010 specs                                                                                                                                          |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;street            | `string`          | Street name including number where appropriate                                                                                                              |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;validFrom         | `string`          | Some Addresses will be created for the future                                                                                                               |

#### Example

Typical tenant updated message example:

**NOTE:** in most cases the `tenantReference` _(prior reference)_ and `tenant.reference` _(new reference)_ will be the same.

```json
{"eventType":"Letting.Tenant.Updated",
  "data":{
    "tenancyAgreementReference":"10001.786.29.01",
    "unitReference":"10001.786.29",
    "tenantReference":"100004",
    "tenant":{
      "reference":"100004",
      "email":"name@home-mail.xy",
      "postalAddress":{
        "addressLine1":"Haupt Mieter",
        "countryCode":"CH",
        "city":"Bern",
        "zipCode":"3000",
        "postOfficeBoxText":"Postfach 1234",
        "street":"Hauptstrasse 1",
        "validFrom":"2024-01-01"
      }
    }
  }
}
```

Typical tenant merge message example:

**NOTE:** in the case of a merge the `tenantReference` _(prior reference)_ is different from `tenant.reference` _(new reference)_

```json
{"eventType":"Letting.Tenant.Updated",
  "data":{
    "tenancyAgreementReference":"10001.786.29.01",
    "unitReference":"10001.786.29",
    "tenantReference":"100004",
    "tenant":{
      "reference":"987654",
      "firstName":"Haupt",
      "surname":"Mieter",
      "languageCode":"de",
      "nationalityCode":"AT",
      "phoneNumber":"+41 31 331 21 11",
      "email":"name@home-mail.xy",
      "fullName":"Haupt Mieter",
      "type":"PHYSICAL",
      "dateOfBirth":"1980-01-01",
      "allphoneNumbers":[
        {"phoneNumber":"+41 31 331 21 11","type":"PRIVATE"},
        {"phoneNumber":"+41 31 331 21 12","type":"PROFESSIONAL"},
        {"phoneNumber":"+41 31 331 21 13","type":"MOBILE"},
        {"phoneNumber":"+41 31 331 21 14","type":"OTHER"}
      ],
      "allEmails":[
        {"emailAddress":"name@home-mail.xy","type":"PRIVATE"},
        {"emailAddress":"username@work-mail.yz","type":"PROFESSIONAL"}
      ],
      "postalAddress":{
        "addressLine1":"Haupt Mieter",
        "countryCode":"CH",
        "city":"Bern",
        "zipCode":"3000",
        "postOfficeBoxText":"Postfach 1234",
        "street":"Hauptstrasse 1"
      }
    }
  }
}
```
