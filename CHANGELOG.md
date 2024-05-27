# Changelog

## master (2024-05-27)

add complete mietzinsreserve to TenancyAgreement.Create.Accepted message

## master (2024-05-06)

Add reason to Invoicing.Invoice.Deleted and Invoicing.Invoice.Cancelled messages

## master (2023-04-02)

Add uniqueness validations to Invoicing.Invoice.Created

- creditorReference + externalReference must be unique
- dmsReference

## master (2023-03-21)

Add booking details to Invoicing.Invoice.Booked

## master (2022-12-08)

Invoicing.Order.Created / Updated Messages allow empty orderItems again

## master (2022-12-02)

- Message property 'delivery_mode' replaces 'persistent' to match the AMQP specification.
- Invoicing.Order.Created#externalReference must be unique

## master (2022-11-03)

- new message property `correlation_id` documented

## master (2022-10-17)

- `Invoicing.Order.Updated` message added
- `backlinkUrl` attribute added to `Invoicing.Order.Created` message

## master (2022-10-14)

- `Invoicing.Order.Created` message changed:
  - `description` must contain plain text, html ist not supported anymore
  - `amount` was removed since we have orderItems
  - `orderItems` is mandatory now and requires at least one item
  - `orderItem.accountNumber` must NOT be prefixed with an accounting reference
  - `orderItem.bookingAmount` added (use it instead of amount)
  - `orderItem.amount` now has the same meaning as `Invoicing.Invoice.Created.invoiceItems.amount`
  - `orderItem.bookingText` is now documented (it was already supported before)

## master (2022-10-12)

- Notification.Case.Closed message added

## 1.16 (2022-08-17)

  Corrected API - compliant with SIX SwissQRCode Standard v2.2 - now `addInfo` fields are correctly nested within `rmtInf`

  ```
    rmtInf: {
      tp: ,
      ref: ,
      addInf: {
        ustrd: ,
        trailer: ,
        strdBkgInf:
      }
    }
  ```

  Until v1.16 GREM was non-compliant with SIX SwissQRCode Standard v2.2 it had defined `addInf` attributes at the TopLevel of a SwissQRCode invoice

  ```
    rmtInf: {
      tp: ,
      ref:
    },
    addInf: {
      ustrd: ,
      trailer: ,
      strdBkgInf:
    }
  ```
