# Documents Context

## Events

| Type                                            | GARAIO REM         | REM | Description               |
| ----------------------------------------------- | ------------------ | --- | ------------------------- |
| [Documents.Document.Create](#documentsdocumentadd) | :white_check_mark: | :x: | Stores a document in GREM |

### Documents.Document.Create

This is a work in progress, [contact us](https://github.com/Garaio-REM/grem-dev-guide) if you have questions.

| Field                            | Type     | Content / Remarks                                                              |
| -------------------------------- | -------- | ------------------------------------------------------------------------------ |
| `eventType`                      | `string` | `Documents.Document.Create`                                                       |
| `data`                           | `hash`   |                                                                                |
| &nbsp;&nbsp;`fetchUrl`           | `string` | an URL where the document can be downloaded from (3) **required**              |
| &nbsp;&nbsp;`docType`            | `string` | the document type, see (1) **required**                                        |
| &nbsp;&nbsp;`relatedToType`      | `string` | the type of the business object this document relates to, see (2) **required** |
| &nbsp;&nbsp;`relatedToReference` | `string` | the reference of the business object this document relates to **required**     |
| &nbsp;&nbsp;`fileName`           | `string` | the file name **required**                                                     |
| &nbsp;&nbsp;`description`        | `string` | description of the document                                                    |
| &nbsp;&nbsp;`replaceRemoteDocumentWithUrl` | `string` | provide the URL of the RemoteDokument url you want this Document to replace        |

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
