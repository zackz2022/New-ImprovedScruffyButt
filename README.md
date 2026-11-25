# Scruffy Butt Digital Design
A selling platform for digital media and promoting sites

## Bookstore payments

The storefront is hosted on GitHub Pages; `worker.js` is a separate Cloudflare
Worker that creates and captures PayPal orders. It does not deliver books.
For live orders, check the completed order in PayPal and email the matching
book files to the customer. Customers are asked to contact the store with their
order number to arrange delivery.

Before accepting live payments:

1. In Cloudflare, configure the Worker's `PAYPAL_CLIENT_ID` and
   `PAYPAL_CLIENT_SECRET` as secrets for the same PayPal application. Never
   put the client secret in the repository or browser configuration.
2. Set `PAYPAL_ENVIRONMENT` in `wrangler.toml` to `sandbox` for testing, and
   set `payment-config.js` to the matching sandbox client ID and deployed
   Worker URL. Keep `STORE_ORIGIN` set to the actual site's origin. The
   Worker also needs the `AFFILIATES_DB` D1 binding and schema described in
   `AFFILIATE-SETUP.md`.
3. Deploy the Worker with Wrangler using an authorized Cloudflare account.
   Deploying GitHub Pages does not deploy the Worker.
4. Complete a sandbox purchase and verify the PayPal order, cart amount,
   cancellation, error handling, and manual delivery process. Sandbox
   transactions are not real payments.
5. Only when ready to sell, switch the Worker environment to `live`, set
   its secrets to the matching live application credentials, update the
   public client ID in `payment-config.js` to the same live application,
   and deploy both the Worker and site. Confirm the recipient account and
   perform an authorized live transaction and refund if appropriate.
