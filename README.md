# REM-Cloud Message Bus API

[REM](https://www.garaio-rem.ch/) (Real Estate Management) is a suite of products and systems developed by GARAIO REM AG. The REM-Cloud Message Bus connects
these systems and enables other organisations to integrate with REM. The REM-Cloud Message Bus is a [RabbitMQ Message Bus](https://www.rabbitmq.com/).
RabbitMQ itself implements [AMQP](https://www.amqp.org/).

## Versioning

The master branch in this repository reflects current development. If you need the specs for a specific release, have a look at the corresponding branch.

## Getting Started

See [Getting Started](./getting_started.md).

## About this specification

Messages in this specification have a specification status. These status are:

| Status             | Meaning                                                                  |
| ------------------ | ------------------------------------------------------------------------ |
| :white_check_mark: | The information is available and will not change without further notice. |
| :x:                | The information is not available.                                        |
| :warning:          | The information is deprecated and may become invalid in future releases. |

Please remember that only the master branch of this documentation reflects the current state of our productive REM systems.
Other branches are used to describe new or amended information applicable to future releases of our REM systems.

## Messages

### Message Properties

All messages must specify at least the following AMQP message properties:

| Property           | Value                         | Example                             | Description                                                |
| ------------------ | ----------------------------- | ----------------------------------- | ---------------------------------------------------------- |
| `app_id`           | `<app_id>`                    | `'YourService'` or `'REMCustomerA'` | Uniquely identifies the sender of a message                |
| `user_id`          | `<user_id>`                   | `'YourService'` or `'REMCustomerA'` | Uniquely identifies the authenticated RabbitMQ user        |
| `content_type`     | `application/json`            |                                     | The content type must always be JSON                       |
| `content_encoding` | `UTF-8`                       |                                     | The message encoding must always be UTF-8                  |
| `message_id`       | `<app_id>-<app_specific_uid>` | `'YourService-7712897'`             | Uniquely identifies an event that caused this message. (1) |
| `correlation_id`   | String                        | `'Origin-Message-ID'`               | For responses, contains the request `message_id` (3)       |
| `timestamp`        | Unix timestamp                | `1553245964`                        | A timestamp to indicate when the message was created       |
| `routing_key`      | `<message type>`              | `Notification.Message.Created`      | Pass the event type as the routing key                     |
| `delivery_mode`    | Byte                          | `2`                                 | Always pass `2` (2)                                        |

Notes

* (1) The app specific uid is an alphanumeric, app wide unique key
* (2) `2` ensures the message persists even if the broker restarts. In some RabbitMQ-Client libraries, `delivery_mode` might be mapped to a boolean property called `persistent` which shall be set to `true`
* (3) Messages that respond to a message (`*.Accepted` / `*.Rejected`) send back the `message_id` of the received message as the correlation_id so that the sender can map the response. The property is only present in [result messages](./result_messages.md).

### Headers

The headers are part of the message properties and must specify at least the app id.

| Property | Type     | Value      | Example                              | Description                                                                      |
| -------- | -------- | ---------- | ------------------------------------ | -------------------------------------------------------------------------------- |
| `app_id` | `string` | `<app_id>` | `headers: { app_id: 'YourService' }` | In order to be able to route the messages we need the app id in the headers, too |

Some messages must contain additional properties. Please refer to [Header Properties](/header_properties.md) for more information.

### Events

Events are messages that can be received by multiple subscribers. The message body contains a json data structure.

### Error handling

If your message does not adhere to the basic rules given above, it will be discarded and you'll receive an [Errors.Message.Invalid](./errors.md#errorsmessageinvalid) with detailed error information.

## Naming Conventions

Messages that end with a verb in the simple past (eg `Created`) describe events that happened in the GARAIO REM domain.
Messages that end with an imperative (eg `Create`) describe commands that trigger an effect in the GARAIO REM domain.

## Contexts

Events are grouped by contexts. A context relates to a certain subdomain within the domain of property management,
or to a domain outside of property management. **IMPORTANT: Message attribute names are case sensitive**.

| Context                                               | Description                                                                    |
| ----------------------------------------------------- | ------------------------------------------------------------------------------ |
| [Archiving](archiving_context.md)                     | Events that are related to archiving Documents                                 |
| [Damage Case](damage_case_context.md)                 | Events that are related to a Damage Case                                       |
| [Condominium](condominium_context.md)                 | Events related to condominiums                                                 |
| [Documents](documents_context.md)                     | Events related to documents                                                    |
| [Invoicing](invoicing_context.md)                     | Events related to orders and invoicing                                         |
| [Letting](letting_context.md)                         | Events related to the letting process                                          |
| [Marketplace](marketplace_context.md)                 | Events related to marketplace platforms                                        |
| [Masterdata](masterdata_context.md)                   | Informs about changed data in the context of Property, Buildings and Units     |
| [Notification](notification_context.md)               | Events that represent notifications; always addressed to a GARAIO REM instance |
| [Owner Portal](owner_portal_context.md)               | Events related to owner portals                                                |
| [Pending Issue](pending_issue.md)                     | Events related to pending issues ("Pendenzen")                                 |
| [Planning Application](planning_application.md)       | Events related to planning applications                                        |
| [Property Check](property_check_context.md)           | Events that are related to a Property Check                                    |
| [Sedex](sedex.md)                                     | Allows clients to send and receive Sedex-Messages                              |
| [Tenancy Application](tenancy_application_context.md) | Events related to tenancy applications                                         |
| [Tenant Portal](tenant_portal.md)                     | Events that occur on a Tenant Portal                                           |
| [Thirdparty Notification](thirdparty_notification.md) | Events related to a Thirdparty Notification. (1)                               |
| [Errors](errors.md)                                   | Errors due to invalid messages                                                 |

Notes:

* (1) These events are issued by the Thirdparty Notification Service (Drittmeldung Service)

## Examples

[Ruby script to publish a notification message](examples/ruby/publish_notification.rb)
