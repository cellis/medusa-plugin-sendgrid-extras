# SendGrid with EXTRA DATA.

Handle sending emails to customers related to orders, restock notifications, users, or custom events.

For sending `extra` data, see the configuration below.

[SendGrid Plugin Documentation](https://docs.medusajs.com/plugins/notifications/sendgrid) | [Medusa Website](https://medusajs.com) | [Medusa Repository](https://github.com/medusajs/medusa)

## Features

- Send emails when an event related to orders, restock notifications, or users is triggered.
- Use dynamic templates in SendGrid to build the emails to be sent.
- Send emails with SendGrid for custom events.

---

## Prerequisites

- [Medusa backend](https://docs.medusajs.com/development/backend/install)
- [SendGrid account](https://signup.sendgrid.com/)

---

## How to Install

1\. Run the following command in the directory of the Medusa backend:

  ```bash
  npm install medusa-plugin-sendgrid-extras
  ```

2\. Set the following environment variable in `.env`:

  ```bash
  SENDGRID_API_KEY=<API_KEY>
  SENDGRID_FROM=<SEND_FROM_EMAIL>
  # IDs for different email templates
  SENDGRID_ORDER_PLACED_ID=<ORDER_PLACED_TEMPLATE_ID> # example
  ```

3\. In `medusa-config.js` add the following at the end of the `plugins` array:

  ```js
  const plugins = [
    // ...,
    {
      resolve: `medusa-plugin-sendgrid-extras`,
      options: {
        api_key: process.env.SENDGRID_API_KEY,
        from: process.env.SENDGRID_FROM,
        extras: {
          // Each function takes an event, and the transformed data
          // returns whatever extra data you want to send to the 
          // dynamic template.
          // Note that any keys returned in this object will overwrite keys in `transformedEvent` data
          "order.placed": function orderPlacedExtra(event, transformedEvent) {
            return {
              foo: 'bar'
            }
          },
          "order.shipment_created": function orderShipmentCreatedExtra(event, transformedEvent) {
            // ship
            return {
              now: new Date()
            }
          }, 
        },
        order_placed_template: 
          process.env.SENDGRID_ORDER_PLACED_ID,
        localization: {
          "de-DE": { // locale key
            order_placed_template:
              process.env.SENDGRID_ORDER_PLACED_ID_LOCALIZED,
          },
        },
      },
    },
  ]
  ```

---

## Test the Plugin

1\. Run the following command in the directory of the Medusa backend to run the backend:

  ```bash
  npm run start
  ```

2\. Place an order using a storefront or the [Store APIs](https://docs.medusajs.com/api/store). You should receive a confirmation email.

---

## Additional Resources

- [SendGrid Plugin Documentation](https://docs.medusajs.com/plugins/notifications/sendgrid)
