# Documents Context

## Events

| Type                                            | GARAIO REM         | REM | Description               |
| ----------------------------------------------- | ------------------ | --- | ------------------------- |
| [Documents.Document.Create](#documentsdocumentadd) | :white_check_mark: | :x: | Stores a document in GREM |
| [Documents.Document.Viewed](#documentsdocumentviewed) | :white_check_mark: | :x: | Marks a document as viewed in Garaio REM when accessed on a remote System (Portal) |

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
| &nbsp;&nbsp;`replaceRemoteDocumentWithUrl` | `string` | provide the URL of the RemoteDokument url you want this Document to replace |

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

### Documents.Document.Viewed

This event is sent to mark a document as viewed in GARAIO REM when it has been accessed on a remote system (e.g., Portal).

| Field                            | Type     | Content / Remarks                                                              |
| -------------------------------- | -------- | ------------------------------------------------------------------------------ |
| `eventType`                      | `string` | `Documents.Document.Viewed`                                                    |
| `data`                           | `hash`   |                                                                                |
| &nbsp;&nbsp;`documentId`         | `string` | The ID of the document that was viewed **required**                            |
| &nbsp;&nbsp;`documentType`       | `string` | The type of the document (e.g., `BriefDokument`) **required** (1)              |
| &nbsp;&nbsp;`personReference`    | `string` | The reference of the person who viewed the document **required**               |
| &nbsp;&nbsp;`viewedOn`           | `string` | ISO 8601 timestamp when the document was viewed **required**                   |

Notes

* (1) While `Dokument` is valid in most cases as a fallback, it is strongly recommended to send the specific document type when known. For documents sent for reading, the document type should be known and sending the specific type provides better data quality. 

**Note:** _**documentType** within `Documents.Document.Viewed` is different from **`docType`** used by `Documents.Document.Create`.  In the `Create` message GARAIO REM chooses the actual `documentType` to create based on the values given by `docType` and `relatedToType`._

#### Example

```json
{
  "app_id": "imofix",
  "eventType": "Documents.Document.Viewed",
  "data": {
    "documentId": "16254",
    "documentType": "BriefDokument",
    "personReference": "100088",
    "viewedOn": "2025-09-28T12:30:35Z"
  }
}
```

**Note:** _**`app_id`** needs to be included while manually testing (or include a header within the message - as shown in the example).  Actual messages from a remote session will include an `app_id` from the message header. The `app_id`, is critical information in order to identify where the document was viewed._


#### Documents.Document.Viewed.Accepted

The [Accept](./result_messages.md#accepted-message) message.

No additional `data` fields.

#### Documents.Document.Viewed.Rejected

The [Reject](./result_messages.md#rejected-message) message.

No additional `data` fields.
