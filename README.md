# Toss Payments (toss-payments)

Toss Payments is a South Korean payment gateway (PG) operated by Viva Republica, the company behind the Toss super-app. Its REST Core API lets merchants accept and manage online payments across cards, Korean easy-pay wallets (Toss Pay, KakaoPay, Naver Pay), virtual accounts, bank transfer, and mobile-phone billing. The API covers payment confirmation and cancellation, recurring billing keys, virtual account issuance, cash receipts, transaction and settlement queries, and marketplace seller payouts, with asynchronous results delivered by webhooks.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/toss-payments/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/toss-payments/refs/heads/main/apis.yml)

## Access Model

Toss Payments uses a key-based access model with distinct test and live environments, selected by the prefix of the key you send:

- **Secret keys (server-side).** Test keys are prefixed `test_sk_` (and `test_gsk_`); live keys are prefixed `live_sk_` (and `live_gsk_`). The secret key authenticates every Core API call. Authentication is **HTTP Basic**: the secret key is used as the username with an **empty password**, then Base64 encoded — `Authorization: Basic base64(secretKey:)`. Note the trailing colon.
- **Client keys (browser-side).** Test keys are prefixed `test_ck_` and live keys `live_ck_`. The client key is used by the JavaScript SDK / payment widget to open the checkout window in the browser; it is not used for Core API authentication.

The public base host for the Core API is `https://api.tosspayments.com`. Payment and billing resources live under `/v1`; the newer marketplace payouts product lives under `/v2`. The same host serves both test and live traffic — the environment is determined by whether you present a test or live secret key. A bare request to `https://api.tosspayments.com/v1/payments` returns HTTP 401 (auth required), confirming the live, authenticated surface.

Some capabilities — key-in (manual card) payments, manual settlement requests, and marketplace payouts — require an additional contract with Toss Payments before the endpoints will approve.

## Tags

- Payments
- Payment Gateway
- South Korea
- Cards
- Easy Pay
- Virtual Account
- Billing
- Checkout
- Fintech

## Timestamps

- **Created:** 2026-07-12
- **Modified:** 2026-07-12

## APIs

### Toss Payments Payments API

Confirm (authorize) a payment created in the checkout window, look up a payment by `paymentKey` or `orderId`, and cancel a payment in full or in part. Also supports key-in (manual card entry) payments. Payments move through READY, IN_PROGRESS, WAITING_FOR_DEPOSIT, DONE, CANCELED, PARTIAL_CANCELED, ABORTED, and EXPIRED states.

- **Human URL:** [https://docs.tosspayments.com/reference](https://docs.tosspayments.com/reference)
- **Base URL:** `https://api.tosspayments.com`

#### Tags

- Payments
- Checkout
- Cards
- Cancellation

#### Properties

- [Documentation](https://docs.tosspayments.com/en/api-guide)
- [API Reference](https://docs.tosspayments.com/reference)
- [OpenAPI](openapi/toss-payments-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/toss-payments.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/toss-payments.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Toss Payments Billing API

Issue a billing key for recurring (automatic) payments — from an `authKey` returned by the billing auth window, or directly from card credentials — and then charge that billing key on demand for subscription and installment billing.

- **Human URL:** [https://docs.tosspayments.com/guides/v2/billing/integration-api](https://docs.tosspayments.com/guides/v2/billing/integration-api)
- **Base URL:** `https://api.tosspayments.com`

#### Tags

- Billing
- Recurring
- Subscriptions
- Cards

#### Properties

- [Documentation](https://docs.tosspayments.com/guides/v2/billing/integration-api)
- [API Reference](https://docs.tosspayments.com/reference)
- [OpenAPI](openapi/toss-payments-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)

### Toss Payments Virtual Accounts API

Issue a per-order virtual bank account that a customer deposits into. Deposit and cancellation events are delivered asynchronously through the DEPOSIT_CALLBACK webhook, and the resulting payment is retrievable via the Payments API.

- **Human URL:** [https://docs.tosspayments.com/reference](https://docs.tosspayments.com/reference)
- **Base URL:** `https://api.tosspayments.com`

#### Tags

- Virtual Account
- Bank Transfer
- Deposit

#### Properties

- [API Reference](https://docs.tosspayments.com/reference)
- [OpenAPI](openapi/toss-payments-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)

### Toss Payments Cash Receipts API

Issue, cancel, and query Korean cash receipts (현금영수증) for income deduction or expense evidence, including the customer identity number and receipt key needed for National Tax Service reporting.

- **Human URL:** [https://docs.tosspayments.com/reference](https://docs.tosspayments.com/reference)
- **Base URL:** `https://api.tosspayments.com`

#### Tags

- Cash Receipt
- Tax
- South Korea

#### Properties

- [API Reference](https://docs.tosspayments.com/reference)
- [OpenAPI](openapi/toss-payments-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)

### Toss Payments Transactions API

Query the ledger of approved and canceled transactions across a date range, with cursor pagination (`startingAfter`, `limit`), for reconciliation and reporting.

- **Human URL:** [https://docs.tosspayments.com/reference](https://docs.tosspayments.com/reference)
- **Base URL:** `https://api.tosspayments.com`

#### Tags

- Transactions
- Reporting
- Reconciliation

#### Properties

- [API Reference](https://docs.tosspayments.com/reference)
- [OpenAPI](openapi/toss-payments-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)

### Toss Payments Settlements API

Query settlement records by sold date or paid-out date with the fees and payout amounts per transaction, and (under an additional contract) request manual settlement for a given `paymentKey`.

- **Human URL:** [https://docs.tosspayments.com/reference](https://docs.tosspayments.com/reference)
- **Base URL:** `https://api.tosspayments.com`

#### Tags

- Settlements
- Payouts
- Reconciliation

#### Properties

- [API Reference](https://docs.tosspayments.com/reference)
- [OpenAPI](openapi/toss-payments-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)

### Toss Payments Payouts API

Marketplace payouts (v2) — check the available balance and send batched payout requests to sellers, immediately or on a schedule. Seller and payout status changes are delivered via the `seller.changed` and `payout.changed` webhooks.

- **Human URL:** [https://docs.tosspayments.com/en/integration-payouts](https://docs.tosspayments.com/en/integration-payouts)
- **Base URL:** `https://api.tosspayments.com`

#### Tags

- Payouts
- Marketplace
- Sellers

#### Properties

- [Documentation](https://docs.tosspayments.com/en/integration-payouts)
- [OpenAPI](openapi/toss-payments-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)

### Toss Payments Brandpay API

Brandpay is Toss Payments' embeddable in-merchant easy-pay wallet, letting customers register payment methods once and reuse them. Method and customer status changes are pushed via the METHOD_UPDATED and CUSTOMER_STATUS_CHANGED webhooks. This is a documented product; individual Brandpay endpoints are not modeled in the OpenAPI in this entry.

- **Human URL:** [https://docs.tosspayments.com/reference/brandpay](https://docs.tosspayments.com/reference/brandpay)
- **Base URL:** `https://api.tosspayments.com`

#### Tags

- Brandpay
- Easy Pay
- Wallet

#### Properties

- [Documentation](https://docs.tosspayments.com/reference/brandpay)

## Common Properties

- [Domain Security](security/toss-payments-domain-security.yml)
- [Authentication](authentication/toss-payments-authentication.yml)
- [LinkedIn](https://www.linkedin.com/company/tosspayments)
- [Website](https://www.tosspayments.com)
- [Documentation](https://docs.tosspayments.com/en)
- [Plans](plans/toss-payments-plans-pricing.yml)
- [Rate Limits](rate-limits/toss-payments-rate-limits.yml)
- [Fin Ops](finops/toss-payments-finops.yml)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
