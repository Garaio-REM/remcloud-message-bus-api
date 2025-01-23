# Documents Context

## Events

| Type                                            | GARAIO REM         | REM | Description               |
| ----------------------------------------------- | ------------------ | --- | ------------------------- |
| [Documents.Document.Create](#documentsdocumentadd) | :white_check_mark: | :x: | Stores a document in GREM |

### Documents.Document.Create

This is a work in progress, [contact us](https://github.com/Garaio-REM/grem-dev-guide) if you have questions.

| Field                                      | Type     | Required | Content / Remarks                                                   |
| ------------------------------------------ | -------- | ------------------------------------------------------------------------------ |
| `eventType`                                | `string` | **true** | `Documents.Document.Create`                                         |
| `data`                                     | `hash`   | **true** |                                                                     |
| &nbsp;&nbsp;`fetchUrl`                     | `string` | **true** | an URL where the document can be downloaded from (3)                |
| &nbsp;&nbsp;`docType`                      | `string` | **true** | the document type, see (1)                                          |
| &nbsp;&nbsp;`relatedToType`                | `string` | **true** | the type of the business object this document relates to, see (2)   |
| &nbsp;&nbsp;`relatedToReference`           | `string` | **true** | the reference of the business object this document relates to       |
| &nbsp;&nbsp;`fileName`                     | `string` | **true** | the file name                                                       |
| &nbsp;&nbsp;`description`                  | `string` |  false   | description of the document                                         |
| &nbsp;&nbsp;`replaceRemoteDocumentWithUrl` | `string` |  false   | the RemoteDokument with the given URL (and maches relatedTo fields) |

Notes

* (1) Allowed values by `relatedToType`:
  * `accounting`: `Dokument`,
  * `property`: `Dokument`,
  * `building`: `Dokument`,
  * `unit`: `Dokument`, `BewerbungsFormular`,
  * `invoicing-invoice`: `KreditorRechnung`
  * `letting-tenancy-agreement`: `Dokument`
* (2) Possible values
  * accounting
  * property
  * building
  * unit
  * invoicing-invoice
  * letting-tenancy-agreement
* (3) If the download from `fetchUrl` fails, GREM will send back a `Documents.Document.Rejected` message with details.

#### Example

```json
{"eventType":"Documents.Document.Create",
  "data":{
    "fetchUrl": "http://www.example.com/document",
    "docType": "Dokument",
    "relatedToType": "accounting",
    "relatedToReference": "123805",
    "fileName": "document.pdf"
  }
}
```

#### Documents.Document.Accepted

The [Accept](./result_messages.md#accepted-message) message.

Additional `data` fields:

| Field       | Type     | Content / Remarks                            |
| ----------- | -------- | -------------------------------------------- |
| `reference` | `string` | The reference of the newly created document. |

#### Documents.Document.Rejected

The [Reject](./result_messages.md#rejected-message) message.

No additional `data` fields.
