# Letting Context

## Events

Type | GARAIO REM | REM | Description
---|---|---|---
[Letting.Tenancy.Created](#lettingtenancycreated) | :white_check_mark: | :x: | A tenancy has been created; does not reliably signal a tenant move in. A tenancy is uniquely identified by tenancy agreement reference, tenant reference and unit reference
[Letting.Tenancy.Updated](#lettingtenancyupdated) | :white_check_mark: | :x: | Start and / or end date of a tenancy have been changed
[Letting.Tenancy.Deleted](#lettingtenancydeleted) | :white_check_mark: | :x: | A tenancy has been deleted; this means that the tenancy never became effective
[Letting.Tenancy.TenancyAgreementReferenceChanged](#lettingtenancytenancyagreementreferencechanged) | :white_check_mark: | :x: | The reference of a tenancy agreement has changed
[Letting.Tenancy.SecurityDeposit.Change](#lettingtenancysecuritydepositchange)  | :white_check_mark: | :x: |  A system other than GARAIO REM changed security deposit details.
[Letting.Tenancy.SecurityDeposit.Accepted](#lettingtenancysecuritydepositaccepted)  | :white_check_mark: | :x: | TODO
[Letting.Tenancy.SecurityDeposit.Rejected](#lettingtenancysecuritydepositrejected)  | :white_check_mark: | :x: | TODO
[Letting.Tenancy.MoveInConfirmed](#lettingtenancymoveinconfirmed) | :x: | :white_check_mark: | Confirms a tenant will move or has moved into a unit. This event is only raised for tenants that live or trade in a given unit. For example the event is not raised for tenants that act as guarantors or for tenants that have had their tenancy agreement changed while staying in the same unit.  |
[Letting.Tenancy.MoveOutConfirmed](#lettingtenancymoveoutconfirmed) | :x: | :white_check_mark: | Confirms a tenant will move out or has moved out of a unit. Like Letting.Tenancy.MoveInConfirmed the event is also only raised when a tenant has lived or traded in person at the given unit. |
[Letting.Tenancy.Create](#lettingtenancycreate) | :x: | :white_check_mark: | A new tenancy has been created by a system other than GARAIO REM.
[Letting.Tenancy.Accepted](#lettingtenancyaccepted) | :x: | :white_check_mark: | TODO
[Letting.Tenancy.Rejected](#lettingtenancyrejected) | :x: | :white_check_mark: | TODO
[Letting.Reservation.Change](#lettingreservationchange) | :x: | :white_check_mark: | A system other than GARAIO REM reserved a unit.
[Letting.Reservation.Accepted](#lettingreservationaccepted) | :x: | :white_check_mark: | TODO
[Letting.Reservation.Rejected](#lettingreservationrejected) | :x: | :white_check_mark: | TODO

### Letting.Tenancy.Created

Field | Type | Content / Remarks
---|---|---
eventType | string | Letting.Tenancy.Created
data | hash |
&nbsp;&nbsp;startDate | string | ISO 8601 encoded date, eg '2019-05-25'
&nbsp;&nbsp;endDate | string | ISO 8601 encoded date, eg '2019-05-25'; might be null
&nbsp;&nbsp;tenancyAgreementReference | string | unique tenancy agreement identifier, eg '1234.01.0001.01'
&nbsp;&nbsp;unitReference | string | unique unit identifier, eg '234.01.0001'
&nbsp;&nbsp;tenant | hash |
&nbsp;&nbsp;&nbsp;&nbsp;reference | string | tenant reference; uniquely identifies a person
&nbsp;&nbsp;&nbsp;&nbsp;firstName | string |
&nbsp;&nbsp;&nbsp;&nbsp;surname | string |
&nbsp;&nbsp;&nbsp;&nbsp;languageCode | string | de, fr, it or en; **must be lower case**
&nbsp;&nbsp;&nbsp;&nbsp;nationalityCode | string | ISO country code, eg 'CH'
&nbsp;&nbsp;&nbsp;&nbsp;phoneNumber | string | might be null
&nbsp;&nbsp;&nbsp;&nbsp;email | string | might be null

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

Field | Type | Content / Remarks
---|---|---
eventType | string | Letting.Tenancy.Updated
data | hash |
&nbsp;&nbsp;startDate | string | ISO 8601 encoded date, eg '2019-05-25'
&nbsp;&nbsp;endDate | string | ISO 8601 encoded date, eg '2019-05-25'; might be null
&nbsp;&nbsp;tenancyAgreementReference | string | unique tenancy agreement identifier, eg '1234.01.0001.01'
&nbsp;&nbsp;unitReference | string | unique unit identifier, eg '234.01.0001'
&nbsp;&nbsp;tenantReference | string | unique person identifier, eg '1234'

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

Field | Type | Content / Remarks
---|---|---
eventType | string | Letting.Tenancy.Deleted
data | hash |
&nbsp;&nbsp;tenancyAgreementReference | string | unique tenancy agreement identifier, eg '1234.01.0001.01'
&nbsp;&nbsp;unitReference | string | unique unit identifier, eg '234.01.0001'
&nbsp;&nbsp;tenantReference | string | unique person identifier, eg '1234'

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

Field | Type | Content / Remarks
---|---|---
eventType | string | Letting.Tenancy.TenancyAgreementReferenceChanged
data | hash |
&nbsp;&nbsp;tenancyAgreementReference | string | unique tenancy agreement identifier, eg '1234.01.0001.01'
&nbsp;&nbsp;newTenancyAgreementReference | string | new identifier for the tenancy agreement, eg '1234.01.0011.01'

#### Example

```json
{"eventType":"Letting.Tenancy.TenancyAgreementReferenceChanged",
  "data":{
    "tenancyAgreementReference":"1234.01.0001.01",
    "newTenancyAgreementReference":"1234.01.0011.01",
  }
}
```

### Letting.Tenancy.SecurityDeposit.Change

#### Example

```json
{"eventType":"Letting.Tenancy.TenancyAgreementSetSecurityDeposit",
  "data":{
    "tenancyAgreementReference":"1234.01.0001.01",
    ""
  }
}
```

### Letting.Tenancy.SecurityDeposit.Accepted

TODO

### Letting.Tenancy.SecurityDeposit.Rejected

TODO

### Letting.Tenancy.MoveInConfirmed

Field | Type | Content / Remarks
---|---|---
eventType | string | Letting.Tenancy.MoveInConfirmed
data | hash |
&nbsp;&nbsp;tenancyStartDate | string | ISO 8601 encoded date, eg '2019-03-01'
&nbsp;&nbsp;tenancyAgreementReference | string | unique tenancy agreement identifier, eg '1234.01.0001.01'
&nbsp;&nbsp;unitReference | string | unique unit identifier, eg '234.01.0001'
&nbsp;&nbsp;tenant | hash |
&nbsp;&nbsp;&nbsp;&nbsp;reference | string | tenant reference; uniquely identifies a person
&nbsp;&nbsp;&nbsp;&nbsp;previousAddress | hash | the address where the tenant lived or traded before. Can be nil if unknown.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;street | string | street name including the building number where appropriate
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;zipCode | string |
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;city | string |
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;countryCode | string | ISO country code, eg 'CH'
&nbsp;&nbsp;lettingContactReference | string | uniquely identifies the management team member who represents the property owner regarding this tenancy.

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

Field | Type | Content / Remarks
---|---|---
eventType | string | Letting.Tenancy.MoveOutConfirmed
data | hash |
&nbsp;&nbsp;tenancyEndDate | string | ISO 8601 encoded date, eg '2019-05-30'
&nbsp;&nbsp;tenancyAgreementReference | string | unique tenancy agreement identifier, eg '1234.01.0001.01'
&nbsp;&nbsp;unitReference | string | unique unit identifier, eg '234.01.0001'
&nbsp;&nbsp;tenant | hash |
&nbsp;&nbsp;&nbsp;&nbsp;reference | string | tenant reference; uniquely identifies a person
&nbsp;&nbsp;&nbsp;&nbsp;nextAddress | hash | the address where the tenant will live or trade next. Can be nil if unknown.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;street | string | street name including the building number where appropriate
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;zipCode | string |
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;city | string |
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;countryCode | string | ISO country code, eg 'CH'
&nbsp;&nbsp;lettingContactReference | string | uniquely identifies the management team member who represents the property owner regarding this tenancy.

#### Additional Header Properties

[local_authority](/header_properties.md/#AdditionalHeaderProperties)

#### Example

```json
TODO
```

### Letting.Tenancy.Create

Field | Type | Content / Remarks
---|---|---
eventType | string | Letting.Tenancy.Create
data | hash |
&nbsp;&nbsp;unitReference | string | unique unit identifier, eg '234.01.0001'
&nbsp;&nbsp;tenancyAgreemenTypeCode | string | TODO
&nbsp;&nbsp;createdAt | string | ISO 8601 encoded date
&nbsp;&nbsp;startDate | string | Rental start date. ISO 8601 encoded date, eg '2019-05-25'
&nbsp;&nbsp;endDate | string | Rental end date. ISO 8601 encoded date, eg '2019-05-25'; might be null
&nbsp;&nbsp;tenants[] | array |
&nbsp;&nbsp;&nbsp;&nbsp;firstName | string |
&nbsp;&nbsp;&nbsp;&nbsp;surname | string |
&nbsp;&nbsp;&nbsp;&nbsp;salutationTypeCode | string | TODO
&nbsp;&nbsp;&nbsp;&nbsp;address.city | string |
&nbsp;&nbsp;&nbsp;&nbsp;address.countryCode | string | e.g. 'CH'
&nbsp;&nbsp;&nbsp;&nbsp;address.street | string |
&nbsp;&nbsp;&nbsp;&nbsp;address.zipCode | string |
&nbsp;&nbsp;&nbsp;&nbsp;email | string |
&nbsp;&nbsp;&nbsp;&nbsp;phoneNumber | string |
&nbsp;&nbsp;&nbsp;&nbsp;marriageStatus | string | TODO use typeCode instead?
&nbsp;&nbsp;&nbsp;&nbsp;dateOfBirth | string | ISO 8601 encoded date, eg '2019-05-25'
&nbsp;&nbsp;&nbsp;&nbsp;placeOfOrigin | string |
&nbsp;&nbsp;&nbsp;&nbsp;nationalityCode | string | ISO country code, eg 'CH' TODO use typeCode?
&nbsp;&nbsp;&nbsp;&nbsp;jobTitle | string |
&nbsp;&nbsp;&nbsp;&nbsp;tenantIndustryTypeCode | string |
&nbsp;&nbsp;paymentModeTypeCode | string | TODO
&nbsp;&nbsp;numberOfPeople | integer | TODO
&nbsp;&nbsp;terminationModeTypeCode | integer | TODO
&nbsp;&nbsp;noticePeriodMonths | integer | Notice period in months
&nbsp;&nbsp;securityDepositAmount | integer | Security deposit amount in CHF
&nbsp;&nbsp;paymentMethod | integer | TODO use well known string-Codes on API?
&nbsp;&nbsp;paymentIBAN | string |
&nbsp;&nbsp;installationsForCommonUsage | string | Installations for common usage
&nbsp;&nbsp;roomsForSoleUsage[] | array | Rooms for sole usage
&nbsp;&nbsp;&nbsp;&nbsp;count | integer |
&nbsp;&nbsp;&nbsp;&nbsp;kindTypeCode | integer |

#### Example

TODO

### Letting.Tenancy.Accepted

TODO

### Letting.Tenancy.Rejected

TODO

### Letting.Reservation.Change

#### Example

```json
{"eventType":"Letting.Reserve",
  "data":{
    "unitReference":"10001.786.29",
    "reserved": true,
    "reservationTypeCode": "01",
    "reservationReason": "Offenes Mietangebot"
  }
}
```

### Letting.Reservation.Accepted

TODO

### Letting.Reservation.Rejected

TODO
