# Masterdata Context

## Events

Type | GARAIO REM | REM | Description
---|---|---|---
[Masterdata.Property.Created](#masterdatapropertycreated) | :white_check_mark: | :white_check_mark: | A new property has been created
[Masterdata.Property.Updated](#masterdatapropertyupdated) | :white_check_mark: | :white_check_mark: | Data associated to a property has changed; you get changed attributes only
[Masterdata.Property.TagAdded](#masterdatapropertytagadded) | :white_check_mark: | :x: | A tag was added to a property; please read the specs for this event carefully
[Masterdata.Property.TagRemoved](#masterdatapropertytagremoved) | :white_check_mark: | :x: | A tag was removed from a property; please read the specs for this event carefully
[Masterdata.Building.Created](#masterdatabuildingcreated) | :white_check_mark: | :white_check_mark: | A building has been created
[Masterdata.Building.Updated](#masterdatabuildingupdated) | :white_check_mark: | :white_check_mark: | Data associated to a building has changed; you get the reference plus all changed attributes
[Masterdata.Building.Deleted](#masterdatabuildingdeleted) | :white_check_mark: | :white_check_mark:| The building was deleted
[Masterdata.Building.ReferenceChanged](#masterdatabuildingreferencechanged) | :white_check_mark: | :x: | The building reference has changed
[Masterdata.Unit.Created](#masterdataunitcreated) | :white_check_mark: | :white_check_mark: | A rentable unit has been created
[Masterdata.Unit.Updated](#masterdataunitupdated) | :white_check_mark: | :white_check_mark: | Data associated to a rentable unit has changed; you get the reference plus all changed attributes
[Masterdata.Unit.Deleted](#masterdataunitdeleted) | :white_check_mark: | :white_check_mark: | The unit was deleted
[Masterdata.Unit.ReferenceChanged](#masterdataunitreferencechanged) | :white_check_mark: | :white_check_mark: | The unit reference has changed
[Masterdata.Condominium.Updated](#masterdatacondominiumupdated) | :white_check_mark: | :x: | Data associated to a condominium has changed; you get the reference plus all changed attributes
[Masterdata.ManagementTeam.Updated](#masterdatamanagementteamupdated) | :white_check_mark: | :x: | A change to a property management team was applied; only changed roles are published
[Masterdata.Configuration.SedexIdChanged](#masterdataconfigurationsedexidchanged) | :x: | :white_check_mark: | A new SedexID has been configured |
[Masterdata.PersonContactData.Update](#masterdatapersoncontactdataupdate) | :white_check_mark: | :x: | Update the contact data of a person with this message
[Masterdata.PersonPaymentDetails.Update](#masterdatapersonpaymentdetailsupdate) | :white_check_mark: | :x: | Update the payment details of a person with this message
[Masterdata.Person.Create](#masterdatapersoncreate) | :white_check_mark: | :x: | Create a new person record with this message
[Masterdata.Person.Update](#masterdatapersonupdate) | :white_check_mark: | :x: | Update the masterdata of a person with this message

### Masterdata.Property.Created

Field | Type | Content / Remarks
---|---|---
eventType | string | Masterdata.Property.Created
data | hash |
&nbsp;&nbsp;reference | string | unique identifier for the property
&nbsp;&nbsp;description | string |
&nbsp;&nbsp;zipCode | string |
&nbsp;&nbsp;city | string |
&nbsp;&nbsp;countryCode | string | ISO country code, eg 'CH'
&nbsp;&nbsp;startOfAdministration | string | ISO 8601 encoded date, eg '2019-03-01'
&nbsp;&nbsp;endOfAdministration | string | ISO 8601 encoded date, eg '2019-03-01'
&nbsp;&nbsp;administrationKind| string | One of the following values: <ul><li>MEG</li><li>VOLL</li><li>TECHNISCH</li><li>ADMINISTRATIV</li><li>BACKMANAGEMENT</li><li>CENTERMANAGEMENT</li><li>EXTERNAL_SYSTEM</li><li>STEWE</li><li>INKASSO</li><li>UNBEKANNT</li></ul>

#### Example

```json
{"eventType":"Masterdata.Property.Created",
  "data":{
    "reference":"1234",
    "description":"my property",
    "zipCode":"3000",
    "city":"Bern",
    "countryCode":"CH",
    "endOfAdministration": "2018-12-31",
    "administrationKind": "MEG"
  }
}
```

### Masterdata.Property.Updated

Field | Type | Content / Remarks
---|---|---
eventType | string | Masterdata.Property.Updated
data | hash |
&nbsp;&nbsp;reference | string | unique identifier for the property
&nbsp;&nbsp;description | string |
&nbsp;&nbsp;zipCode | string |
&nbsp;&nbsp;city | string |
&nbsp;&nbsp;countryCode | string | ISO country code, eg 'CH'
&nbsp;&nbsp;mandateTerminatedBy | string | ISO date, eg '2018-12-31'
&nbsp;&nbsp;endOfAdministration | string | ISO 8601 encoded date, eg '2019-03-01'
&nbsp;&nbsp;administrationKind| string | (REM1 only) One of the following values: <ul><li>MEG</li><li>VOLL</li><li>TECHNISCH</li><li>ADMINISTRATIV</li><li>BACKMANAGEMENT</li><li>CENTERMANAGEMENT</li><li>EXTERNAL_SYSTEM</li><li>STEWE</li><li>INKASSO</li><li>UNBEKANNT</li></ul>

#### Example

```json
{"eventType":"Masterdata.Property.Updated",
  "data":{
    "reference":"1234",
    "description":"my property renamed",
    "endOfAdministration": "2018-12-31"
  }
}
```

### Masterdata.Property.TagAdded

If you receive this event and your service has tag constraints (you only 'see' properties tagged with certain tags), this means than an existing property has been tagged with a tag visible to you.

To get the complete data set for this property, use the GraphQL query ```property(reference: String!): Property```

Field | Type | Content / Remarks
---|---|---
eventType | string | Masterdata.Property.TagAdded
data | hash |
&nbsp;&nbsp;reference | string | unique identifier for the property
&nbsp;&nbsp;tag | string | The tag that was added

#### Example

```json
{"eventType":"Masterdata.Property.TagAdded",
  "data":{
    "reference":"1234",
    "tag":"t001"
  }
}
```

### Masterdata.Property.TagRemoved

If you receive this event and your service has tag constraints (you only 'see' properties tagged with certain tags), this means that a tag visible to you has been removed from this property.

This is the very last event that you get for this property and you must remove it from your local domain model (if you store property data).

Field | Type | Content / Remarks
---|---|---
eventType | string | Masterdata.Property.TagRemoved
data | hash |
&nbsp;&nbsp;reference | string | unique identifier for the property
&nbsp;&nbsp;tag | string | The tag that was removed

#### Example

```json
{"eventType":"Masterdata.Property.TagRemoved",
  "data":{
    "reference":"1234",
    "tag":"t001"
  }
}
```

### Masterdata.Building.Created

Field | Type | Content / Remarks
---|---|---
eventType | string | Masterdata.Building.Created
data | hash |
&nbsp;&nbsp;reference | string | unique identifier for the building; the first segment of the key is the property reference, eg '1234.01'
&nbsp;&nbsp;description | string | might be null
&nbsp;&nbsp;numberOfElevators | integer | might be null
&nbsp;&nbsp;numberOfFloorsAboveGround | integer | might be null
&nbsp;&nbsp;numberOfFloorsBelowGround | integer | might be null
&nbsp;&nbsp;egid | integer | [read about it](https://www.bfs.admin.ch/bfs/de/home/register/personenregister/registerharmonisierung/minimaler-inhalt-einwohnerregister/egid-ewid.html), might be null
&nbsp;&nbsp;mandateTerminatedBy | string | ISO date, eg '2018-12-31', might be null
&nbsp;&nbsp;street | string | street name including the building number where appropriate
&nbsp;&nbsp;zipCode | string |
&nbsp;&nbsp;city | string |
&nbsp;&nbsp;countryCode | string | ISO country code, eg 'CH'
&nbsp;&nbsp;wgs84Position | hash | Geo Coordinates, might be null
&nbsp;&nbsp;&nbsp;latitude | decimal | Latitude
&nbsp;&nbsp;&nbsp;longitude | decimal | Longitude
&nbsp;&nbsp;startOfAdministration | string | ISO 8601 encoded date, eg '2019-03-01'
&nbsp;&nbsp;endOfAdministration | string | ISO 8601 encoded date, eg '2019-03-01'

#### Example

```json
{"eventType": "Masterdata.Building.Created",
  "data":{
    "reference":"1234.01",
    "description":"a building",
    "numberOfElevators":null,
    "numberOfFloorsAboveGround":3,
    "numberOfFloorsBelowGround":null,
    "egid":123456,
    "mandateTerminatedBy": "2025-05-31",
    "street":"Bahnhofstrasse 23",
    "zipCode":"3000",
    "city":"Bern",
    "countryCode":"CH",
    "wgs84Position":{
      "latitude":44.640672,
      "longitude":4.756836
    }
  }
}
```

### Masterdata.Building.Updated

Field | Type | Content / Remarks
---|---|---
eventType | string | Masterdata.Building.Updated
data | hash |
&nbsp;&nbsp;reference | string | unique identifier for the building; the first segment of the key is the property reference, eg '1234.01'
&nbsp;&nbsp;description | string | might be null
&nbsp;&nbsp;numberOfElevators | integer | might be null
&nbsp;&nbsp;numberOfFloorsAboveGround | integer | might be null
&nbsp;&nbsp;numberOfFloorsBelowGround | integer | might be null
&nbsp;&nbsp;egid | integer | [read about it](https://www.bfs.admin.ch/bfs/de/home/register/personenregister/registerharmonisierung/minimaler-inhalt-einwohnerregister/egid-ewid.html), might be null
&nbsp;&nbsp;mandateTerminatedBy | string | ISO date, eg '2018-12-31', might be null
&nbsp;&nbsp;street | string | street name including the building number where appropriate
&nbsp;&nbsp;zipCode | string |
&nbsp;&nbsp;city | string |
&nbsp;&nbsp;countryCode | string | ISO country code, eg 'CH'
&nbsp;&nbsp;wgs84Position | hash | Geo Coordinates; only present if the geo coordinates have changed, might be null
&nbsp;&nbsp;&nbsp;latitude | decimal | Latitude
&nbsp;&nbsp;&nbsp;longitude | decimal | Longitude
&nbsp;&nbsp;startOfAdministration | string | ISO 8601 encoded date, eg '2019-03-01'
&nbsp;&nbsp;endOfAdministration | string | ISO 8601 encoded date, eg '2019-03-01'

#### Example

```json
{"eventType":"Masterdata.Building.Updated",
  "data":{
    "reference":"1234.01",
    "street":"Bahnhofstrasse 23a",
    "wgs84Position":{
      "latitude":44.640672,
      "longitude":4.756836
    }
  }
}
```

### Masterdata.Building.Deleted

Field | Type | Content / Remarks
---|---|---
eventType | string | Masterdata.Building.Deleted
data | hash |
&nbsp;&nbsp;reference | string | unique identifier for the building; the first segment of the key is the property reference, eg '1234.01'

#### Example

```json
{"eventType":"Masterdata.Building.Deleted",
  "data":{
    "reference":"1234.01"
  }
}
```

### Masterdata.Building.ReferenceChanged

A user might change the reference of a building in GARAIO REM. This event reflects such a change. If you store building data you must apply this change to your data.

Note that a change of building reference also leads to a change of object references and tenancy agreement references in that building.
These changes will be published by separate messages, namely Masterdata.Unit.ReferenceChanged and Letting.Tenancy.TenancyAgreementReferenceChanged.

Field | Type | Content / Remarks
---|---|---
eventType | string | Masterdata.Building.ReferenceChanged
data | hash |
&nbsp;&nbsp;reference | string | unique identifier for the building; the first segment of the key is the property reference, the second is the building reference eg '1234.01'
&nbsp;&nbsp;newReference | string | new identifier for the building

#### Example

```json
{"eventType":"Masterdata.Building.ReferenceChanged",
  "data":{
    "reference":"1234.02",
    "newReference":"1234.03"
  }
}
```

### Masterdata.Unit.Created

Field | Type | Content / Remarks
---|---|---
eventType | string | Masterdata.Unit.Created
data | hash |
&nbsp;&nbsp;reference | string | unique identifier for the unit; the first segment of the key is the property reference, the second is the building reference eg '1234.01.0001'
&nbsp;&nbsp;unitCategoryCode | string | code to identify the unit category; the list of unit categories is part of the Graphql API
&nbsp;&nbsp;unitTypeCode | string | code to identify the unit type; the list of unit types is part of the Graphql API
&nbsp;&nbsp;idxCategory | string | code to identify the unit category according to the IDX standard; the list of unit categories is described in the IDX definition
&nbsp;&nbsp;idxType | integer | code to identify the unit type according to the IDX standard; the idxType depends on the idxCategory and is described in the IDX definition
&nbsp;&nbsp;storeyCode | string | code to identify the unit storey; the list of storeys is part of the Graphql API; might be null
&nbsp;&nbsp;location | string | location of the unit, where appropriate, eg left, middle, right; this is free text and might be null
&nbsp;&nbsp;numberOfRooms | string | number of rooms as a decimal, eg "3.5"; might be null
&nbsp;&nbsp;ewid | integer | [read about it](https://www.bfs.admin.ch/bfs/de/home/register/personenregister/registerharmonisierung/minimaler-inhalt-einwohnerregister/egid-ewid.html), might be null
&nbsp;&nbsp;bfsId | string | [read about it](https://www.bfs.admin.ch/bfs/de/home/register/gebaeude-wohnungsregister/gebaeudeadressen.html), might be null
&nbsp;&nbsp;validFrom | string | ISO 8601 encoded date, eg '2019-03-01'
&nbsp;&nbsp;validUntil| string | ISO 8601 encoded date, eg '2019-03-01'

#### Example

```json
{"eventType":"Masterdata.Unit.Created",
  "data":{
    "reference":"1234.01.0001",
    "unitCategoryCode":"1",
    "unitTypeCode":"01",
    "idxCategory":"APPT",
    "idxType":1,
    "storeyCode":"1",
    "location":"links",
    "numberOfRooms":"3.5",
    "ewid":123456,
    "bfsId":"A654321"
  }
}
```

### Masterdata.Unit.Updated

Field | Type | Content / Remarks
---|---|---
eventType | string | Masterdata.Building.Updated
data | hash |
&nbsp;&nbsp;reference | string | unique identifier for the unit; the first segment of the key is the property reference, the second is the building reference eg '1234.01.0001'
&nbsp;&nbsp;unitCategoryCode | string | code to identify the unit category; the list of unit categories is part of the Graphql API
&nbsp;&nbsp;unitTypeCode | string | code to identify the unit type; the list of unit types is part of the Graphql API
&nbsp;&nbsp;idxCategory | string | code to identify the unit category according to the IDX standard; the list of unit categories is described in the IDX definition
&nbsp;&nbsp;idxType | integer | code to identify the unit type according to the IDX standard; the idxType depends on the idxCategory and is described in the IDX definition
&nbsp;&nbsp;storeyCode | string | code to identify the unit storey; the list of storeys is part of the Graphql API; might be null
&nbsp;&nbsp;location | string | location of the unit, where appropriate, eg left, middle, right; this is free text and might be null
&nbsp;&nbsp;numberOfRooms | string | number of rooms as a decimal, eg "3.5"; might be null
&nbsp;&nbsp;ewid | integer | [read about it](https://www.bfs.admin.ch/bfs/de/home/register/personenregister/registerharmonisierung/minimaler-inhalt-einwohnerregister/egid-ewid.html), might be null
&nbsp;&nbsp;bfsId | string | [read about it](https://www.bfs.admin.ch/bfs/de/home/register/gebaeude-wohnungsregister/gebaeudeadressen.html), might be null
&nbsp;&nbsp;validFrom | string | ISO 8601 encoded date, eg '2019-03-01'
&nbsp;&nbsp;validUntil| string | ISO 8601 encoded date, eg '2019-03-01'

#### Example

```json
{"eventType":"Masterdata.Unit.Updated",
  "data":{
    "reference":"1234.01.0001",
    "numberOfRooms":"3.0"
  }
}
```

### Masterdata.Unit.Deleted

Field | Type | Content / Remarks
---|---|---
eventType | string | Masterdata.Building.Deleted
data | hash |
&nbsp;&nbsp;reference | string | unique identifier for the unit; the first segment of the key is the property reference, the second is the building reference eg '1234.01.0001'

#### Example

```json
{"eventType":"Masterdata.Unit.Deleted",
  "data":{
    "reference":"1234.01.0001"
  }
}
```

### Masterdata.Unit.ReferenceChanged

A user might change the reference of a unit in GARAIO REM. This event reflects such a change. If you store unit data in a local domain model you must apply this change to your data.

Note that a change of unit reference also leads to a change of tenancy agreement references in that unit.
These changes will be published by separate messages, namely `Letting.Tenancy.TenancyAgreementReferenceChanged`.

Field | Type | Content / Remarks
---|---|---
eventType | string | Masterdata.Unit.ReferenceChanged
data | hash |
&nbsp;&nbsp;reference | string | unique identifier for the unit; the first segment of the key is the property reference, the second is the building reference eg '1234.01.0001'
&nbsp;&nbsp;newReference | string | new identifier for the unit

#### Example

```json
{"eventType":"Masterdata.Unit.ReferenceChanged",
  "data":{
    "reference":"1234.01.0001",
    "newReference":"1234.01.0002"
  }
}
```

### Masterdata.Condominium.Updated

Field | Type | Content / Remarks
---|---|---
`eventType` | string | `Masterdata.Condominium.Updated`
`data` | hash |
&nbsp;&nbsp;`reference` | string | Unique identifier for the condominium unit
&nbsp;&nbsp;`unitTypeCode` | string | Code to identify the unit type
&nbsp;&nbsp;`storeyCode` | string | Code to identify the unit storey
&nbsp;&nbsp;`location` | string | Location of the unit, where appropriate, e.g., left, middle, right; this is free text and might be null
&nbsp;&nbsp;`numberOfRooms` | string | Number of rooms as a decimal, e.g., "3.0"; might be null
&nbsp;&nbsp;`ewid` | integer | [Read about it](https://www.bfs.admin.ch/bfs/de/home/register/personenregister/registerharmonisierung/minimaler-inhalt-einwohnerregister/egid-ewid.html), might be null
&nbsp;&nbsp;`bfsId` | string | [Read about it](https://www.bfs.admin.ch/bfs/de/home/register/gebaeude-wohnungsregister/gebaeudeadressen.html), might be null
&nbsp;&nbsp;`unitCategoryCode` | string | Code to identify the unit category
&nbsp;&nbsp;`headVotes` | integer | Number of head votes for the condominium unit
&nbsp;&nbsp;`m2` | decimal | Area in square meters, might be null
&nbsp;&nbsp;`m3` | decimal | Volume in cubic meters, might be null

#### Example

```json
{
  "eventType": "Masterdata.Condominium.Updated",
  "data": {
    "reference": "10001.561.339",
    "unitTypeCode": "10",
    "storeyCode": "3",
    "location": null,
    "numberOfRooms": "3.0",
    "ewid": null,
    "bfsId": null,
    "unitCategoryCode": "1",
    "headVotes": 1,
    "m2": null,
    "m3": null
  }
}
```

### Masterdata.ManagementTeam.Updated

Field | Type | Content / Remarks
---|---|---
eventType | string | Masterdata.ManagementTeam.Updated
data | hash |
&nbsp;&nbsp;propertyReference | string | unique identifier for the property
&nbsp;&nbsp;managementTeamChanges | array |
&nbsp;&nbsp;&nbsp;&nbsp;userRoleCode | string | user role code, eg R001
&nbsp;&nbsp;&nbsp;&nbsp;surname | string |
&nbsp;&nbsp;&nbsp;&nbsp;firstName | string |
&nbsp;&nbsp;&nbsp;&nbsp;languageCode | string | de, fr, it or en; **must be lower case**
&nbsp;&nbsp;&nbsp;&nbsp;phoneNumber | string |
&nbsp;&nbsp;&nbsp;&nbsp;email | string |

#### Example

```json
{"eventType":"Masterdata.ManagementTeam.Updated",
  "data":{
    "propertyReference":"1234",
    "managementTeamChanges":[
      {"userRoleCode":"R001",
       "surname":"Muster",
       "firstName":"Max",
       "languageCode":"de",
       "phoneNumber":"555 123 456",
       "email":"max.muster@test-mail.com"
      },
      {"userRoleCode":"R002",
       "surname":"Muster",
       "firstName":"Maxine",
       "languageCode":"fr",
       "phoneNumber":"555 654 321",
       "email":"maxine.muster@test-mail.com"
      }
    ]
  }
}
```

### Masterdata.Configuration.SedexIdChanged

Field | Type | Content / Remarks
---|---|---
eventType | string | Masterdata.Configuration.SedexIdChanged
data | hash |
&nbsp;&nbsp;sedexId | string | the new SedexID (e.g. T4-123456-2); A value null means the previously used SedexID is not valid anymore.

#### Example

```json
{"eventType":"Masterdata.Configuration.SedexIdChanged",
  "data":{
    "sedexId":"T-123456-2"
  }
}
```

### Masterdata.PersonContactData.Update

This message is sent from an external message publisher to a GARAIO REM instance and allows to update contact data of a person.
Set the recipient property in the headers, eg `"grem_wincasa"`. All attributes are optional unless noted otherwise in the remarks.
Rules for all array attributes:

- do not send the attribute if you do not want to create current data of this type
- send an empty array (`[]`) to delete all current data of this type
- send an array of valid date to fully replace the current data of this type

GARAIO REM replies with a standard [Accepted](./result_messages.md#accepted-message) / [Rejected](./result_messages.md#rejected-message) message containing the personReference and reject reasons, where appropriate

Field | Type | Content / Remarks
---|---|---
`eventType` | `string` | Masterdata.PersonContactData.Update
`data` | `hash` |
&nbsp;&nbsp;`personReference` | `string` | reference of the person that should receive the communication updates; **required**
&nbsp;&nbsp;`privateEmails` | `array` | list of new private emails
&nbsp;&nbsp;`businessEmails` | `array` | list of new business emails
&nbsp;&nbsp;`otherEmails` | `array` | list of new other emails
&nbsp;&nbsp;`privatePhoneNumbers` | `array` | list of new private phone numbers
&nbsp;&nbsp;`businessPhoneNumbers` | `array` | list of new business phone numbers
&nbsp;&nbsp;`mobilePhoneNumbers` | `array` | list of new mobile phone numbers
&nbsp;&nbsp;`otherPhoneNumbers` | `array` | list of new other phone numbers
&nbsp;&nbsp;`privateFaxNumbers` | `array` | list of new private fax numbers
&nbsp;&nbsp;`businessFaxNumbers` | `array` | list of new business fax numbers
&nbsp;&nbsp;`otherFaxNumbers` | `array` | list of new other fax numbers
&nbsp;&nbsp;`privateUrls` | `array` | list of new private urls
&nbsp;&nbsp;`businessUrls` | `array` | list of new business urls
&nbsp;&nbsp;`otherUrls` | `array` | list of new other urls
&nbsp;&nbsp;`contacts` | `array` |
&nbsp;&nbsp;&nbsp;&nbsp;`name` | `string` | name of the contact
&nbsp;&nbsp;&nbsp;&nbsp;`contactAddress` | `string` | address / email / phone number of the contact; **required**

#### Examples

##### add email, phone number and contact data

```json
{"eventType":"Masterdata.PersonContactData.Update",
  "data":{
    "personReference":"123456",
    "privateEmails":["me@outlook.com"],
    "privatePhoneNumbers":["+41 79 123 45 67"],
    "contacts":[
      {
        "name":"Max Muster",
        "contactAddress":"him@outlook.com"
      }
    ]
  }
}
```

##### accepted response message

```json
{"eventType":"Masterdata.PersonContactData.UpdateAccepted",
  "data":{
    "personReference":"123456"
  }
}
```

##### rejected response message

```json
{"eventType":"Masterdata.PersonContactData.UpdateRejected",
  "data":{
    "personReference":"123456",
    "reasons":[
      {
        "attribute":"privateEmails",
        "reason":"enthält ungültige Email-Adressen"
      }
    ]
  }
}
```

### Masterdata.PersonPaymentDetails.Update

This message is sent from an external message publisher to a GARAIO REM instance and allows to update payment of a person.
Set the recipient property in the headers, eg `"grem_wincasa"`. All attributes are optional unless noted otherwise in the remarks.
Rules for all array attributes:

- do not send the attribute if you do not want to create current data of this type
- send an empty array (`[]`) to delete all current data of this type
- send an array of valid date to fully replace the current data of this type
- payment details that exist but are not contained in the paymentDetails will be locked

GARAIO REM replies with a standard [Accepted](./result_messages.md#accepted-message) / [Rejected](./result_messages.md#rejected-message) message containing the personReference and reject reasons, where appropriate

Field | Type | Content / Remarks
---|---|---
`eventType` | `string` | Masterdata.PersonPaymentDetails.Update
`data` | `hash` |
&nbsp;&nbsp;`personReference` | `string` | reference of the person that should receive the communication updates; **required**
&nbsp;&nbsp;`paymentDetails` | `array` | list of new payment details
&nbsp;&nbsp;&nbsp;&nbsp;`iban` | `string` | iban; **required**
&nbsp;&nbsp;&nbsp;&nbsp;`ibanName` | `string` | iban name, e.g. name of the bank
&nbsp;&nbsp;&nbsp;&nbsp;`bic` | `string` | bic
&nbsp;&nbsp;&nbsp;&nbsp;`locked` | `boolean` | should the payment detail be locked
&nbsp;&nbsp;&nbsp;&nbsp;`lockReason` | `string` | why should the payment detail be locked? required if you set `locked` to `true`
&nbsp;&nbsp;&nbsp;&nbsp;`defaultPaymentDetail` | `boolean` | should this payment detail be the default? Set it to `true` if the payment detail should become the default. Do not send `false` since that makes no sense, we will ignore it. If you send `true` for more than one payment detail, the last one wins
&nbsp;&nbsp;`deactivationReason` | `string` | Reason why payment details that are not transmitted will be locked; **required**

#### Examples

##### add iban and bic

```json
{"eventType":"Masterdata.PersonPaymentDetails.Update",
  "data":{
    "personReference":"123456",
    "paymentDetails":[
      {
        "iban":"DE19500105176829385733",
        "bic":"WELADEDLLEV",
        "defaultPaymentDetail":true
      }
    ],
    "deactivationReason":"deactivated by API"
  }
}
```

##### accepted response message

```json
{"eventType":"Masterdata.PersonPaymentDetails.UpdateAccepted",
  "data":{
    "personReference":"123456"
  }
}
```

##### rejected response message

```json
{"eventType":"Masterdata.PersonPaymentDetails.UpdateRejected",
  "data":{
    "personReference":"123456",
    "reasons":[
      {
        "attribute": "IBAN DE19500105176829385733",
        "reason": "Beim Sperren muss ein Sperrgrund angegeben werden"
      }
    ]
  }
}
```

### Masterdata.Person.Create

This message is sent from an external message publisher to a GARAIO REM instance and allows to update contact data of a person.
Set the recipient property in the headers, eg `"grem_wincasa"`. All attributes are optional unless noted otherwise in the remarks.

GARAIO REM replies with a standard [Accepted](./result_messages.md#accepted-message) / [Rejected](./result_messages.md#rejected-message) message containing the personReference and reject reasons, where appropriate

| Field                                    | Type                         | Content / Remarks                                                                                                                                                                                                  |
| ---------------------------------------- | ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `eventType`                              | `string`                     | `Masterdata.Person.Create`                                                                                                                                                                                         |
| `data`                                   | `hash`                       |                                                                                                                                                                                                                    |
| &nbsp;&nbsp;`createdAt`                  | `string`                     | ISO 8601 encoded timestamp, eg `'2024-02-06T14:10:11+01:00'`. Defaults to current time.                                                                                                                            |
| &nbsp;&nbsp;`updatedAt`                  | `string`                     | ISO 8601 encoded timestamp, eg `'2024-02-06T14:10:11+01:00'`. Defaults to current time.                                                                                                                            |
| &nbsp;&nbsp;`allowDuplicates`            | `boolean`                    | Whether to allow creation of duplicates (i.e. multiple people with the same first name, surname, city, zipCode and street). Defaults to false.                                                                     |
| &nbsp;&nbsp;`firstName`                  | `string`                     | first name; **required**                                                                                                                                                                                           |
| &nbsp;&nbsp;`surname`                    | `string`                     | surname for natural persons or company name for legal persons; **required**                                                                                                                                        |
| &nbsp;&nbsp;`reference`                  | `string`                     | an optional reference that must be unique. If none is provided, GARAIO REM will generate one.                                                                                                                      |
| &nbsp;&nbsp;`nameSuffix1`                | `string`                     | additional name suffix (i.e. `'c/o Garaio REM AG'`)                                                                                                                                                                |
| &nbsp;&nbsp;`nameAddition2`              | `string`                     | additional field to store name information on company records                                                                                                                                                      |
| &nbsp;&nbsp;`salutation`                 | `string`                     | one of the following values will be accepted: `none`, `sir`, `madam`. Send _either_ `salutation` _or_ `salutationCode` but not both.                                                                               |
| &nbsp;&nbsp;`salutationCode`             | `string`                     | a value of the salutation code table (see code table entries "Anreden" for valid codes). Send _either_ `salutation` _or_ `salutationCode` but not both.                                                            |
| &nbsp;&nbsp;`additionalSalutation`       | `string` (max 50 characters) | additional salutation (i.e. `Dr. med.`)                                                                                                                                                                            |
| &nbsp;&nbsp;`maritalStatus`              | `string`                     | one of the following values will be accepted: `unmarried`, `married`, `widowed`, `divorced`, `separated`, `civil_union`. Send _either_ `maritalStatus` _or_ `maritalStatusCode` but not both.                      |
| &nbsp;&nbsp;`maritalStatusCode`          | `string`                     | a value of the marital status code table (see code table entries "Zivilstände" for valid codes). Send _either_ `maritalStatus` _or_ `maritalStatusCode` but not both.                                              |
| &nbsp;&nbsp;`jobTitle`                   | `string`                     | job title                                                                                                                                                                                                          |
| &nbsp;&nbsp;`personType`                 | `string`                     | person code for this person. Can be one of the following values: `'natural_person'` for natural persons and `'legal_person'` for legal persons and `'condominium'` for condominiums. Defaults to `natural_person`. |
| &nbsp;&nbsp;`dateOfBirth`                | `string`                     | ISO 8601 encoded date, eg `'2019-03-01'`; only **required** if person is a natural person                                                                                                                          |
| &nbsp;&nbsp;`dateOfDeath`                | `string`                     | ISO 8601 encoded date, eg `'2019-03-01'`                                                                                                                                                                           |
| &nbsp;&nbsp;`homeTown`                   | `string`                     | home town                                                                                                                                                                                                          |
| &nbsp;&nbsp;`nationalityCode`            | `string`                     | ISO country code, eg `'CH'`                                                                                                                                                                                        |
| &nbsp;&nbsp;`sensitive`                  | `boolean`                    | sensitive flag. `true` if the person is sensitive and only people with the "Personen Admin" role can mutate that record afterwards.                                                                                |
| &nbsp;&nbsp;`rating`                     | `string`                     | defines the creditor rating of this person.                                                                                                                                                                        |
| &nbsp;&nbsp;`modeOfDispatch`             | `string`                     | defines the mode of dispatch for this person. One of the following values will be accepted: `email`, `post`                                                                                                        |
| &nbsp;&nbsp;`sendEMail`                  | `string`                     | email address where the person wants to receive documents if `modeOfDispatch` is set to `email`                                                                                                                    |
| &nbsp;&nbsp;`isCreditor`                 | `boolean`                    | declares whether this person has a creditor profile.                                                                                                                                                               |
| &nbsp;&nbsp;`creditorProfileIsBlocked`   | `boolean`                    | declares whether this person's creditor profile is blocked.                                                                                                                                                        |
| &nbsp;&nbsp;`discount`                   | `decimal`                    | discount for this person                                                                                                                                                                                           |
| &nbsp;&nbsp;`skonto`                     | `decimal`                    | skonto for this person in percent.                                                                                                                                                                                 |
| &nbsp;&nbsp;`skontoInfo`                 | `string`                     | skonto information for this person.                                                                                                                                                                                |
| &nbsp;&nbsp;`skontoDays`                 | `integer`                    | skonto information for this person. Must be empty, 0 or a positive integer                                                                                                                                         |
| &nbsp;&nbsp;`creditorIndustryCodes`      | `array`                      | Creditor industry codes (given as strings) for this person. Must be a valid creditor industry codes (see code table entries "Kreditorbranchen" for valid codes).                                                   |
| &nbsp;&nbsp;`tenantIndustryCode`         | `string`                     | tenant industry code for this person. Must be a valid tenant industry code (see code table entries "Mieterbranchen" for valid codes).                                                                              |
| &nbsp;&nbsp;`companyGroupReference`      | `string`                     | company group reference for this person. Must be a valid reference of an existing company record in GARAIO REM.                                                                                                    |
| &nbsp;&nbsp;`brandName`                  | `string`                     | brand name for this record                                                                                                                                                                                         |
| &nbsp;&nbsp;`hasDunningBlock`            | `boolean`                    | declares whether this person has a dunning block. `true` if they should have a dunning block and `false` if they shouldn't.                                                                                        |
| &nbsp;&nbsp;`isVATexempt`                | `boolean`                    | declares whether this record is VAT exempt. `true` if they should be VAT exempt and `false` if they shouldn't.                                                                                                     |
| &nbsp;&nbsp;`UIDnumber`                  | `string`                     | UID number for this person. Must be a valid UID number (i.e. `461.079.435`).                                                                                                                                       |
| &nbsp;&nbsp;`VATnr`                      | `string`                     | VAT number for this person. Must be a valid VAT number (i.e. `461.079.435`).                                                                                                                                       |
| &nbsp;&nbsp;`hasPaymentBlock`            | `boolean`                    | declares whether this person has a payment block. `true` if they should have a payment block and `false` if they shouldn't.                                                                                        |
| &nbsp;&nbsp;`paymentBlockReason`         | `string`                     | reason for the payment block for this person.                                                                                                                                                                      |
| &nbsp;&nbsp;`correspondenceLanguageCode` | `string`                     | `'de'`, `'fr'`, `'it'` or `'en'`; must be lower case, defaults to `de`.                                                                                                                                            |
| &nbsp;&nbsp;`iban`                       | `string`                     | IBAN for this person.                                                                                                                                                                                              |
| &nbsp;&nbsp;`bic`                        | `string`                     | BIC for this person.                                                                                                                                                                                               |
| &nbsp;&nbsp;`address`                    | `hash`                       | current address                                                                                                                                                                                                    |
| &nbsp;&nbsp;&nbsp;&nbsp;`city`           | `string`                     | city; **required**                                                                                                                                                                                                 |
| &nbsp;&nbsp;&nbsp;&nbsp;`zipCode`        | `string`                     | zipCode; **required**                                                                                                                                                                                              |
| &nbsp;&nbsp;&nbsp;&nbsp;`street`         | `string`                     | street incl. number; **required**                                                                                                                                                                                  |
| &nbsp;&nbsp;&nbsp;&nbsp;`postbox`        | `string`                     | postbox; **optional**                                                                                                                                                                                              |
| &nbsp;&nbsp;&nbsp;&nbsp;`mailboxNumber`  | `string`                     | mailbox number; **optional**                                                                                                                                                                                       |
| &nbsp;&nbsp;&nbsp;&nbsp;`supplement`     | `string`                     | address supplement, e.g. Elektro-Fachgeschäft; **optional**                                                                                                                                                        |
| &nbsp;&nbsp;&nbsp;&nbsp;`countryCode`    | `string`                     | ISO country code, eg `'CH'`. Defaults to 'CH'.                                                                                                                                                                     |
| &nbsp;&nbsp;`contactData`                | `hash`                       | [ContactData](types/contact_data.md) of this person.                                                                                                                                                               |

#### example

```json
{"eventType":"Masterdata.Person.Create",
  "data":{
    "firstName":"Max",
    "surname":"Muster",
    "dateOfBirth":"1980-01-01",
    "correspondenceLanguageCode":"de",
    "jobTitle":"CEO",
    "homeTown":"Bern",
    "nationalityCode":"ch",
    "maritalStatus":"married",
    "salutation":"SIR",
    "address":{
      "city":"Bern",
      "zipCode":"3007",
      "street":"Gartenstrasse 1/3",
      "countryCode":"CH"
    }
  }
}
```

##### accepted response message

```json
{"eventType":"Masterdata.Person.CreateAccepted",
  "data":{
    "personReference":"123456"
  }
}
```

##### rejected response message

```json
{"eventType":"Masterdata.Person.CreateRejected",
  "data":{
    "reasons":[
      {
        "attribute":"surname",
        "reason":"darf nicht leer sein"
      }
    ]
  }
}
```

### Masterdata.Person.Update

This message is sent from an external message publisher to a GARAIO REM instance and allows to update contact data of a person.
Set the recipient property in the headers, eg `"grem_wincasa"`. All attributes are optional unless noted otherwise in the remarks.

GARAIO REM replies with a standard [Accepted](./result_messages.md#accepted-message) / [Rejected](./result_messages.md#rejected-message) message containing the personReference and reject reasons, where appropriate

Field | Type | Content / Remarks
---|---|---
`eventType` | `string` | `Masterdata.Person.Update`
`data` | `hash` |
&nbsp;&nbsp;`createdAt` | `string` | ISO 8601 encoded timestamp, eg `'2024-02-06T14:10:11+01:00'`. Left unchanged if not given. |
&nbsp;&nbsp;`updatedAt` | `string` | ISO 8601 encoded timestamp, eg `'2024-02-06T14:10:11+01:00'`. Defaults to current time. |
&nbsp;&nbsp;`personReference` | `string` | reference of the person that should receive the communication updates; **required**
&nbsp;&nbsp;`firstName` | `string` | first name
&nbsp;&nbsp;`surname` | `string` | surname for natural persons or company name for legal persons
&nbsp;&nbsp;`nameSuffix1` | `string` | additional name suffix (i.e. `'c/o Garaio REM AG'`)
&nbsp;&nbsp;`nameAddition2` | `string` | additional field to store name information on company records
&nbsp;&nbsp;`salutation` | `string` | one of the following values will be accepted: `none`, `sir`, `madam`. Send _either_ `salutation` _or_ `salutationCode` but not both.
&nbsp;&nbsp;`salutationCode` | `string` | a value of the salutation code table (see code table entries "Anreden" for valid codes). Send _either_ `salutation` _or_ `salutationCode` but not both.
&nbsp;&nbsp;`additionalSalutation` | `string` (max 50 characters) | additional salutation (i.e. `Dr. med.`)
&nbsp;&nbsp;`maritalStatus` | `string` | one of the following values will be accepted: `unmarried`, `married`, `widowed`, `divorced`, `separated`, `civil_union`. Send _either_ `maritalStatus` _or_ `maritalStatusCode` but not both.
&nbsp;&nbsp;`maritalStatusCode` | `string` | a value of the marital status code table (see code table entries "Zivilstände" for valid codes). Send _either_ `maritalStatus` _or_ `maritalStatusCode` but not both.
&nbsp;&nbsp;`jobTitle` | `string` | job title
&nbsp;&nbsp;`dateOfBirth` | `string` | ISO 8601 encoded date, eg `'2019-03-01'`
&nbsp;&nbsp;`dateOfDeath` | `string` | ISO 8601 encoded date, eg `'2019-03-01'`
&nbsp;&nbsp;`homeTown` | `string` | home town
&nbsp;&nbsp;`nationalityCode` | `string` | ISO country code, eg `'CH'`
&nbsp;&nbsp;`sensitive` | `boolean` | sensitive flag. `true` if the person is sensitive and only people with the "Personen Admin" role can mutate that record afterwards.
&nbsp;&nbsp;`rating` | `string` | defines the creditor rating of this person.
&nbsp;&nbsp;`modeOfDispatch` | `string` | defines the mode of dispatch for this person. One of the following values will be accepted: `email`, `post`
&nbsp;&nbsp;`sendEMail` | `string` |  email address where the person wants to receive documents if `modeOfDispatch` is set to `email`
&nbsp;&nbsp;`isCreditor` | `boolean` | declares whether this person has a creditor profile.
&nbsp;&nbsp;`creditorProfileIsBlocked` | `boolean` | declares whether this person's creditor profile is blocked.
&nbsp;&nbsp;`discount` | `decimal` | discount for this person
&nbsp;&nbsp;`skonto` | `decimal` | skonto for this person in percent.
&nbsp;&nbsp;`skontoInfo` | `string` | skonto information for this person.
&nbsp;&nbsp;`skontoDays` | `integer` | skonto information for this person. Must be empty, 0 or a positive integer
&nbsp;&nbsp;`creditorIndustry` | `string` | creditor industry code for this person. Must be a valid creditor industry code (see code table entries "Kreditorbranchen" for valid codes).
&nbsp;&nbsp;`tenantIndustryCode` | `string` | tenant industry code for this person. Must be a valid tenant industry code (see code table entries "Mieterbranchen" for valid codes).
&nbsp;&nbsp;`companyGroupReference` | `string` | company group reference for this person. Must be a valid reference of an existing company record in GARAIO REM.
&nbsp;&nbsp;`brandName` | `string` | brand name for this record
&nbsp;&nbsp;`hasDunningBlock` | `boolean` | declares whether this person has a dunning block. `true` if they should have a dunning block and `false` if they shouldn't.
&nbsp;&nbsp;`isVATexempt` | `boolean` | declares whether this record is VAT exempt. `true` if they should be VAT exempt and `false` if they shouldn't.
&nbsp;&nbsp;`UIDnumber` | `string` | UID number for this person. Must be a valid UID number (i.e. `461.079.435`).
&nbsp;&nbsp;`VATnr` | `string` | VAT number for this person. Must be a valid VAT number (i.e. `461.079.435`).
&nbsp;&nbsp;`hasPaymentBlock` | `boolean` | declares whether this person has a payment block. `true` if they should have a payment block and `false` if they shouldn't.
&nbsp;&nbsp;`paymentBlockReason` | `string` | reason for the payment block for this person.
&nbsp;&nbsp;`correspondenceLanguageCode` | `string` | `'de'`, `'fr'`, `'it'` or `'en'`; **must be lower case**
&nbsp;&nbsp;`address` | `hash` | current address; do pass a complete address or leave it empty, partial updates are not supported
&nbsp;&nbsp;&nbsp;&nbsp;`city` | `string` | city; **required**
&nbsp;&nbsp;&nbsp;&nbsp;`zipCode` | `string` | zipCode; **required**
&nbsp;&nbsp;&nbsp;&nbsp;`street` | `string` | street incl. number; **required**
&nbsp;&nbsp;&nbsp;&nbsp;`postbox` | `string` | postbox; **optional**
&nbsp;&nbsp;&nbsp;&nbsp;`mailboxNumber` | `string` | mailbox number; **optional**
&nbsp;&nbsp;&nbsp;&nbsp;`supplement`    | `string` | address supplement, e.g. Elektro-Fachgeschäft; **optional**
&nbsp;&nbsp;&nbsp;&nbsp;`countryCode` | `string` | ISO country code, eg `'CH'`; **required**
&nbsp;&nbsp;&nbsp;&nbsp;`validFrom` | `string` | optional ISO 8601 encoded date; pass a future date to create or update an address that becomes valid in the future
&nbsp;&nbsp;`contactData`| `hash` | [ContactData](types/contact_data.md) of this person.

#### examples

```json
{"eventType":"Masterdata.Person.Update",
  "data":{
    "personReference":"123456",
    "firstName":"Max",
    "surname":"Muster",
    "dateOfBirth":"1980-01-01",
    "correspondenceLanguageCode":"de",
    "jobTitle":"CEO",
    "homeTown":"Bern",
    "nationalityCode":"ch",
    "maritalStatus":"married",
    "salutation":"SIR",
    "address":{
      "city":"Bern",
      "zipCode":"3007",
      "street":"Gartenstrasse 1/3",
      "countryCode":"CH",
      "postbox":"123456"
    }
  }
}
```

##### create a new address in the future (assuming today is the 4.3.2024)

```json
{"eventType":"Masterdata.Person.Update",
  "data":{
    "personReference":"123456",
    "address":{
      "city":"Aarau Rohr",
      "zipCode":"5032",
      "street":"Salamattweg 15",
      "countryCode":"CH",
      "validFrom":"2024-04-01"
    }
  }
}
```

##### accepted response message

```json
{"eventType":"Masterdata.Person.UpdateAccepted",
  "data":{
    "personReference":"123456"
  }
}
```

##### rejected response message

```json
{"eventType":"Masterdata.Person.UpdateRejected",
  "data":{
    "personReference":"123456",
    "reasons":[
      {
        "attribute":"surname",
        "reason":"darf nicht leer sein"
      }
    ]
  }
}
```
