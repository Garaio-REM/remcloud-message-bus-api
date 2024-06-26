# Archiving Context

## Events

| Type                                                      | GARAIO REM         | REM | Description                                                        |
| --------------------------------------------------------- | ------------------ | --- | ------------------------------------------------------------------ |
| [Archiving.Document.Archived](#archivingdocumentarchived) | :white_check_mark: | :x: | A document has been sent to an external document management system |

### Archiving.Document.Archived

A document has been sent to an external document management system (DMS).

| Field                       | Type      | Content / Remarks                                                           |
| --------------------------- | --------- | --------------------------------------------------------------------------- |
| `eventType`                 | `string`  | `Archiving.Document.Archived`                                               |
| `data`                      | `hash`    |                                                                             |
| &nbsp;&nbsp;`dmsId`         | `string`  | ID of the corresponding document in the DMS                                 |
| &nbsp;&nbsp;`internalId`    | `integer` | Internal ID (DB record id). Caution, see (1).                               |
| &nbsp;&nbsp;`documentClass` | `string`  | Internal document class (2). Caution, see (1). |

Notes

* (1) This information is only useful if you are doing things directly in the database, which is discouraged. We encourage you to talk to us about your usecase first, maybe we can provide a stable and supported API.
* (2) Corresponds to the DB table whose `internalId` is contained in the Message. One of `Dokument`, `PublikationDokument`, `RemoteDokument`, `VerarbeitungDokument` or `Stewe::AbrechnungDokument`.

#### Example

```json
{"eventType":"Archiving.Document.Archived",
  "data":{
    "dmsId":"1234",
    "internalId":5555,
    "documentClass": "Dokument"
  }
}
```
