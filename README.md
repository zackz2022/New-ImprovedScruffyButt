# Scruffy Butt Digital Design
A selling platform for digital media and promoting sites

## PayPal sandbox checkout

The bookstore uses `payment-config.js` to call the Cloudflare Worker at
`https://scruffybuttpaypal-sandbox.zackswifey1202.workers.dev`. It creates
orders at `/api/orders` and captures approved orders at
`/api/orders/{orderId}/capture`. The checkout is for sandbox testing only;
it does not deliver books.

To deploy the Worker, authenticate Wrangler to the Cloudflare account that
owns the Worker and configured D1 database, then from the repository root:

1. Make sure `wrangler.toml` has the correct `STORE_ORIGIN` and `AFFILIATES_DB`
   binding. Follow `AFFILIATE-SETUP.md` to initialize the database if needed.
2. Set `PAYPAL_CLIENT_ID` and `PAYPAL_CLIENT_SECRET` as Worker secrets with
   `npx wrangler secret put PAYPAL_CLIENT_ID` and
   `npx wrangler secret put PAYPAL_CLIENT_SECRET`. Use the REST credentials
   from the same PayPal sandbox app as the public client ID in
   `payment-config.js`. Do not put the client secret in the repository; NVP/SOAP
   credentials are not used by this Worker.
3. Run `npx wrangler deploy`, then test checkout from the configured site origin
   using a PayPal sandbox buyer account.

Keep the frontend client ID, Worker credentials, `PAYPAL_ENVIRONMENT`, and
Worker URL in the same environment. Do not switch to live payments without
configuring and verifying the live integration and delivery process.
