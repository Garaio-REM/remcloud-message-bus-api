# Custom tables

## What are custom tables?

"Custom tables" are database tables that contain additional information regarding a business entity.

The contents of these tables can be managed via the GARAIO REM UI. The tables are custom in the sense that they can be set up per instance of GARAIO REM as needed. Custom tables are defined in the schema `custom`.

## Definitions

| Term            | Meaning                                                                                               |
| --------------- | ----------------------------------------------------------------------------------------------------- |
| business entity | A business entity of GREM (e.g. a person, property, tenancy agreement, ...)                           |
| 1:1 custom table       | A custom table with one entry per business entity, e.g.  `personen <-[1:1]-> person_additional_infos` |
| 1:n custom table       | A custom table with many entries per business entity, e.g. `personen <-[1:n]-> person_hobbies`        |

## Custom tables on the API

Data for custom tables can be sent by third party systems to GARAIO REM alongside many messages. Custom table data is sent via the field `data.customTables`.

An example:

Given 1:1 custom table `person_additional_infos`

| Column             | Descripton                                              |
| ------------------ | ------------------------------------------------------- |
| `id`               | Integer, PK                                             |
| `person_id`        | Integer, FK on table `personen`                         |
| `nickname`         | String                                                  |
| `lucky_number`     | Integer                                                 |
| `favourite_ide_cd` | String, represents a code in code table `favourite_ide` |

and 1:n custom table `person_hobbies_n`

| Column      | Descripton                      |
| ----------- | ------------------------------- |
| `id`        | Integer, PK                     |
| `person_id` | Integer, FK on table `personen` |
| `name`      |                                 |

the following `Masterdata.Person.Update` message

```json
{
    "eventType":"Masterdata.Person.Update",
    "data": {
        "personReference":"123456",
        "firstName":"Fred",
        "customTables": {
            "person_additional_infos": {
                "nickname": "Fredu",
                "lucky_number": 13,
                "favourite_ide_cd": "VIM"
            },
            "person_hobbies_n": [
                {
                    "id": 1,
                    "name": "Knitting"
                },
                {
                    "name": "Running"
                },
                {
                    "id": 3,
                    "_delete": true
                }
            ]
        }
    }
}
```

* will change the first name of the person with reference `123456` to "Fred" (standard behaviour of `Masterdata.Person.Update`)
* will manage the contents of the custom tables as specified (additional custom table data behaviour)
  * the FK `person_id` of the rows custom tables will automatically be set to the id of the person with reference `123456`

### Supported messages

Custom data can be sent alongside any message/event type that is clearly related to a business entity. For now we save us the trouble to maintain an exhaustive list of supported messages. The system actively rejects messages and tells you via error if custom table data cannnot be sent alongside your message type.

### Update behaviour

For 1:1 custom tables

* If no record is present it will be created.
* If there already is a record, it will be updated with the fields you provide.
* To delete an existing record, pass `{ "_delete": true }`
  * This operation is idempotent, it will not fail if no record is present.
  * `_delete` is not supported as a table column name.

For 1:n custom tables, each array entry is handled as follows

* If no id is present the record will be created.
* If an id is present, the record will be updated with the fields you provide.
* If only the keys `id` and `_delete` are present and `_delete` is `true`, the record with the given id will be deleted.
  * `_delete` is not supported as a table column name.

## Mapping of references

Custom tables belong to a business entity. Furthermore, they might have FKs pointing to the ids of other business entities.
On our APIs, we use the reference of business entities as their identifier. To allow you to fill FKs in custom tables if you only know the reference and not the id, the API will map given references to ids.

So, given this 1:1 custom table `person_objekt_rating`

| Column    | Descripton                      |
| --------- | ------------------------------- |
| id        | Integer, PK                     |
| person_id | Integer, FK on table `personen` |
| objekt_id | Integer, FK on table `objekte`  |
| rating    | Integer                         |

and an entry in `objekte` with reference `"000123.45.000678"` and id `23`,
you can manage the table via the API as follows (adapting the example from above):

```json
{
    "eventType":"Masterdata.Person.Update",
    "data": {
        "personReference":"123456",
        "firstName":"Fred",
        "customTables": {
            "person_objekt_rating": {
                "rating": 5,
                "objekt_reference": "000123.45.000678"
            }
        }
    }
}
```

and the `objekt_id` written to `person_objekt_rating` will be `23`.

### Using ids instead of references

You can also use IDs directly. So, in the example above, you could've also specified `{ "objekt_id": 23 }` to achieve the same result.

Prefer using references. This feature is intended to make life easier for customers who integrate via direct read access on the database instead of the GraphQL API.
If you are not doing so currently: We advise against using direct read database access. It burdens you with the challenge and responsability of keeping your integrations stable on new releases, which bring schema and data changes.

## Limitations

The current implementation of the API does not support nested / hierarchical custom tables. Hierarchical custom tables are a construct like `personen <-[1:n]-> person_checklists  <-[1:n]-> person_checklists_items`.
