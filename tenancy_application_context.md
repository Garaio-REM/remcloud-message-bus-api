# Tenancy Application Context

## Events

Events in this context are always fired for a single grem instance. The [recipient](/header_properties.md/#AdditionalHeaderProperties) header property must be set to the grem instance name in order to route the event to the customer, e.g. recipient: grem_demo1

Type | GARAIO REM | REM | Description
---|---|---|---
[`TenancyApplication.Dossier.Selected`](#tenancyapplicationdossierselected) | :white_check_mark: | :x: | A tenancy application has been selected on a tenancy application platform
[`TenancyApplication.Dossier.Received`](#tenancyapplicationdossierreceived) | :white_check_mark: | :x: | A tenancy application has been received on a tenancy application platform
[`TenancyApplication.AllDossiersProcessed`](#tenancyapplicationalldossiersprocessed) | :white_check_mark: | :x: | All tenancy applications have been processed for the specified unit, on a tenancy application platform
[`TenancyApplication.RegistrationForm.Received`](#tenancyapplicationregistrationformreceived) | :white_check_mark: | :x: | A new registration form is available that can be handed out for applications

### TenancyApplication.Dossier.Selected

Field | Type | Content / Remarks
---|---|---
`eventType` | `string` | `TenancyApplication.Dossier.Selected`
`data` | `hash` |
&nbsp;&nbsp;`dossierId` | `string` | Unique identifier within the tenancy application platform |
&nbsp;&nbsp;`unitReference` | `string` | `String` referencing an existing unit in the target GARAIO REM |
&nbsp;&nbsp;`requestedMovingDate` | `string` | ISO 8601 encoded date, eg `'1980-02-17'`; might be null |
&nbsp;&nbsp;`applicants` | `array` | list of applicants; must contain at least one applicant |
&nbsp;&nbsp;&nbsp;&nbsp;`address` | `hash` | address data for this applicant; might be null |
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`street` | `string` | name of street, including the number, where appropriate |
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`zipCode` | `string` | |
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`city` | `string` | |
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`countryCode` | `string` | ISO country code, eg `'CH'` |
&nbsp;&nbsp;&nbsp;&nbsp;`firstName` | `string` | might be null |
&nbsp;&nbsp;&nbsp;&nbsp;`surname` | `string` | might be null |
&nbsp;&nbsp;&nbsp;&nbsp;`dateOfBirth` | `string` | ISO 8601 encoded date, eg `'1980-02-17'`; might be null |
&nbsp;&nbsp;&nbsp;&nbsp;`homeTown` | `string` | might be null |
&nbsp;&nbsp;&nbsp;&nbsp;`jobTitle` | `string` | might be null |
&nbsp;&nbsp;&nbsp;&nbsp;`phoneNumber` | `string` | might be null |
&nbsp;&nbsp;&nbsp;&nbsp;`email` | `string` | might be null |
&nbsp;&nbsp;&nbsp;&nbsp;`nationalityCode` | `string` | ISO country code, eg `'CH'`; might be null |
&nbsp;&nbsp;&nbsp;&nbsp;`language` | `string` | language code eg `'DE'`, `'FR'`, `'IT'` or `'EN'` |
&nbsp;&nbsp;&nbsp;&nbsp;`marriageStatus` | `string` | Accepted statuses: `unmarried`, `married`, `civil_union`, `divorced`, `widowed`, `separated`; might be null |
&nbsp;&nbsp;&nbsp;&nbsp;`salutation` | `string` | Accepted salutations: none, Sir, Madam; not case sensitive |

#### Example

```json
{
  "eventType":"TenancyApplication.Dossier.Selected",
  "data":{
    "applicants":[
      {"address":
        {"city":"Bern",
         "countryCode":"CH",
         "street":"Gartenstr. 3",
         "zipCode":"3007"
        },
        "dateOfBirth":"1980-02-17",
        "email":"max.muster@gmail.com",
        "firstName":"Max",
        "hometown":"Bern",
        "jobTitle":"Software Engineer",
        "language":"DE",
        "marriageStatus":"married",
        "nationalityCode":"CH",
        "phoneNumber":"555 123 456",
        "surname":"Muster",
        "salutation":"Sir"
      }
    ],
    "dossierId":"1111",
    "requestedMovingDate":"2019-08-01",
    "unitReference":"1234.01.0001"
  }
}
```

### TenancyApplication.Dossier.Received

Field | Type | Content / Remarks
---|---|---
`eventType` | `string` | `TenancyApplication.Dossier.Received`
`data` | `hash` |
&nbsp;&nbsp;`unitReference` | `string` | String referencing an existing unit in the target GARAIO REM |
&nbsp;&nbsp;`dossierId` | `string` | Unique identifier within the tenancy application platform |
&nbsp;&nbsp;`title` | `hash` | containing the title of the application in 4 languages
&nbsp;&nbsp;&nbsp;&nbsp;`de` | `string` | String containing the title of the application in german
&nbsp;&nbsp;&nbsp;&nbsp;`fr` | `string` | String containing the title of the application in french
&nbsp;&nbsp;&nbsp;&nbsp;`it` | `string` | String containing the title of the application in italian
&nbsp;&nbsp;&nbsp;&nbsp;`en` | `string` | String containing the title of the application in english
&nbsp;&nbsp;`text` | `hash` | containing the text of the application in 4 languages
&nbsp;&nbsp;&nbsp;&nbsp;`de` | `string` | Html string containing the text of the application in german
&nbsp;&nbsp;&nbsp;&nbsp;`fr` | `string` | Html string containing the text of the application in french
&nbsp;&nbsp;&nbsp;&nbsp;`it` | `string` | Html string containing the text of the application in italian
&nbsp;&nbsp;&nbsp;&nbsp;`en` | `string` | Html string containing the text of the application in english
&nbsp;&nbsp;`url` | `hash` | containing the url of the application for 4 languages
&nbsp;&nbsp;&nbsp;&nbsp;`de` | `string` | String containing the url of the application in german
&nbsp;&nbsp;&nbsp;&nbsp;`fr` | `string` | String containing the url of the application in french
&nbsp;&nbsp;&nbsp;&nbsp;`en` | `string` | String containing the url of the application in english
&nbsp;&nbsp;&nbsp;&nbsp;`it` | `string` | String containing the url of the application in italian

#### Example

```json
{
  "eventType":"TenancyApplication.Dossier.Received",
  "data":{
    "title":
      {
        "de":"Neue Bewerbung für Gartenstrasse 1",
        "fr":"Nouveau dossier pour la Gartenstrasse 1",
        "it":"Neue Bewerbung für Gartenstrasse 1",
        "en":"New dossier for Gartenstrasse 1"
      },
      "text":
      {
        "de":"Bewerbung A<br>22.02.2020",
        "fr":"Dossier A<br>22.02.2020",
        "it":"Bewerbung A<br>22.02.2020",
        "en":"Dossier A<br>22.02.2020"
      },
      "url":
      {
        "de":"http://mieterplatform.ch/de/bewerbung/201",
        "fr":"http://mieterplatform.ch/fr/bewerbung/201",
        "it":"http://mieterplatform.ch/it/bewerbung/201",
        "en":"http://mieterplatform.ch/en/bewerbung/201"
      },
    "unitReference":"1234.01.0001",
    "dossierId":"1111"
  }
}
```

### TenancyApplication.AllDossiersProcessed

Field | Type | Content / Remarks
---|---|---
`eventType` | `string` | `TenancyApplication.AllDossiersProcessed`
`data` | `hash` |
&nbsp;&nbsp;`unitReference` | `string` | String referencing an existing unit in the target GARAIO REM |

#### Example

```json
{
  "eventType":"TenancyApplication.AllDossiersProcessed",
  "data":{
    "unitReference":"1234.01.0001"
  }
}
```

### TenancyApplication.RegistrationForm.Received

Field | Type | Content / Remarks
---|---|---
`eventType` | `string` | TenancyApplication.RegistrationForm.Received
`data` | `hash` |
&nbsp;&nbsp;`unitReference` | `string` | String referencing an existing unit in the target GARAIO REM |
&nbsp;&nbsp;`externalObjectId` | `string` | Unique identifier within the tenancy application platform |
&nbsp;&nbsp;`title` | `hash` | containing the title of the pending issue in 4 languages
&nbsp;&nbsp;&nbsp;&nbsp;`de` | `string` | String containing the title of the pending issue in german
&nbsp;&nbsp;&nbsp;&nbsp;`fr` | `string` | String containing the title of the pending issue in french
&nbsp;&nbsp;&nbsp;&nbsp;`it` | `string` | String containing the title of the pending issue in italian
&nbsp;&nbsp;&nbsp;&nbsp;`en` | `string` | String containing the title of the pending issue in english
&nbsp;&nbsp;`text` | `hash` | containing the text of the pending issue in 4 languages
&nbsp;&nbsp;&nbsp;&nbsp;`de` | `string` | Html string containing the text of the pending issue in german
&nbsp;&nbsp;&nbsp;&nbsp;`fr` | `string` | Html string containing the text of the pending issue in french
&nbsp;&nbsp;&nbsp;&nbsp;`it` | `string` | Html string containing the text of the pending issue in italian
&nbsp;&nbsp;&nbsp;&nbsp;`en` | `string` | Html string containing the text of the pending issue in english
&nbsp;&nbsp;`url` | `hash` | containing the url of the registration form for 4 languages
&nbsp;&nbsp;&nbsp;&nbsp;`de` | `string` | String containing the url of the registration form in german
&nbsp;&nbsp;&nbsp;&nbsp;`fr` | `string` | String containing the url of the registration form in french
&nbsp;&nbsp;&nbsp;&nbsp;`en` | `string` | String containing the url of the registration form in english
&nbsp;&nbsp;&nbsp;&nbsp;`it` | `string` | String containing the url of the registration form in italian

#### Example

```json
{
  "eventType":"TenancyApplication.RegistrationForm.Received",
  "data":{
    "title":
      {
        "de":"Neues Anmeldeformular für Gartenstrasse 1",
        "fr":"Nouveau formulaire de registration pour Gartenstrasse 1",
        "it":"Neues Anmeldeformular für Gartenstrasse 1",
        "en":"New registration form for Gartenstrasse 1"
      },
      "text":
      {
        "de":"Anmeldeformular<br>22.02.2020",
        "fr":"Formulare de registration<br>22.02.2020",
        "it":"Anmeldeformular<br>22.02.2020",
        "en":"Registration form<br>22.02.2020"
      },
      "url":
      {
        "de":"http://mieterplattform.ch/de/registration-form/201",
        "fr":"http://mieterplattform.ch/fr/registration-form/201",
        "it":"http://mieterplattform.ch/it/registration-form/201",
        "en":"http://mieterplattform.ch/en/registration-form/201"
      },
    "unitReference":"1234.01.0001",
    "externalObjectId":"201"
  }
}
```
