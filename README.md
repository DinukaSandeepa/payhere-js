# payhere-js

This repository contains resources related to integrating the PayHere JavaScript SDK (`payhere.js`) for Onsite Checkout experiences.

Files of interest:

- `LLMs.txt` — A complete copy of the PayHere JavaScript SDK (payhere.js) documentation used as reference for integrations and examples.

Usage Notes:
- The `LLMs.txt` file contains full documentation excerpts including examples for generating secure hashes, event handling callbacks, and server-side verification steps.
- Do not store your Merchant Secret on the client-side (browser). The examples show how to generate `hash` and `md5sig` using your server-side Merchant Secret.

Short Summary of Integration Steps:
1. Add the `payhere.js` script to your page.
2. Create the `payment` object with required parameters and generate `hash` on the server.
3. Initiate the payment popup via `payhere.startPayment(payment)` and handle the callbacks.
4. Accept POST notifications at your `notify_url` server endpoint and verify notifications by calculating `md5sig`.

Last updated on 21st Jan 2023
