# Archiving Context

## Events

| Type                                                      | GARAIO REM         | REM | Description                                                        |
| --------------------------------------------------------- | ------------------ | --- | ------------------------------------------------------------------ |
| [Archiving.Document.Archived](#archivingdocumentarchived) | :white_check_mark: | :x: | A document has been sent to an external document management system |

### Archiving.Document.Archived

A document has been sent to an external document management system (DMS).

| Field                    | Type     | Content / Remarks                                  |
| ------------------------ | -------- | -------------------------------------------------- |
| `eventType`              | `string` | `Archiving.Document.Archived`                      |
| `data`                   | `hash`   |                                                    |
| &nbsp;&nbsp;`dmsId`      | `string` | ID of the corresponding document in the DMS        |
| &nbsp;&nbsp;`internalId` | `string` | Internal ID (table `dokumente`). Caution, see (1). |

Notes

* (1) This information is only useful if you are doing things directly in the database, which is discouraged. We encourage you to talk to us about your usecase first, maybe we can provide a stable and supported API.

#### Example

```json
{"eventType":"Archiving.Document.Archived",
  "data":{
    "dmsId":"1234",
    "internalId":"5555"
  }
}
```
