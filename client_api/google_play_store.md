---
title: Client API - Google Play Store
---

# Client API
---
## Google Play Store Client API

`POST` Google Play Store purchase data to the Mbaasy using this API.

### Endpoint URL

`https://api.mbaasy.com/client/google_play/purchase_orders`

### Parameters

| Name | Required | Type | Description |
| ---- | -------- | ---- | ----------- |
| `purchase_data` | Yes | JSON String | The original JSON string from the the `INAPP_PURCHASE_DATA` provided by the [getBuyIntent()](https://developer.android.com/google/play/billing/billing_reference.html#getBuyIntent) method.<br />**Important:** The `INAPP_PURCHASE_DATA` must be intact and as provided by the [getBuyIntent()](https://developer.android.com/google/play/billing/billing_reference.html#getBuyIntent) method. For this reason, the API will only accept the original JSON string. |
| `purchase_signature` | Yes | Base64 encoded string | A Base64 encoded string containing the `INAPP_DATA_SIGNATURE` provided by the [getBuyIntent()](https://developer.android.com/google/play/billing/billing_reference.html#getBuyIntent) method.<br />**Important:** The `INAPP_DATA_SIGNATURE` is already Base64 encoded by the [getBuyIntent()](https://developer.android.com/google/play/billing/billing_reference.html#getBuyIntent) method. There is no need to re-encode the string when sending to the API. |
| `user_identifier` | No | String | User ID corresponding to your user database. |
| `ip_address` | No | V4 or V6 IP Addresses | IP address of the end user. Defaults to the network request IP if omitted, send `NULL` to prevent the IP address from being stored. |
| `metadata` | No | JSON object | Store any arbitrary data to be recorded against the purchase order. |

---

## Legacy Google Play Store Client API

For existing code bases that do not retain the original `INAPP_PURCHASE_DATA` and `INAPP_DATA_SIGNATURE` you can use this endpoint instead.

### Legacy Endpoint URL
`https://api.mbaasy.com/client/google_play/purchase_orders/legacy`

### Parameters

| Name | Required | Type | Description |
| ---- | -------- | ---- | ----------- |
| `package_name` | Yes | String | The packageName extracted from the `INAPP_PURCHASE_DATA` provided by the [getBuyIntent()](https://developer.android.com/google/play/billing/billing_reference.html#getBuyIntent) method. |
| `purchase_token` | Yes | String | The purchaseToken extracted from the `INAPP_PURCHASE_DATA` provided by the [getBuyIntent()](https://developer.android.com/google/play/billing/billing_reference.html#getBuyIntent) method. |
| `product_id` | Yes | String | The productId extracted from the `INAPP_PURCHASE_DATA` provided by the [getBuyIntent()](https://developer.android.com/google/play/billing/billing_reference.html#getBuyIntent) method. |
| `order_id` | No | String | The orderId extracted from the `INAPP_PURCHASE_DATA` provided by the getBuyIntent() method. |
| `user_identifier` | No | String | User ID corresponding to your user database. |
| `ip_address` | No | V4 or V6 IP Addresses | IP address of the end user. Defaults to the network request IP if omitted, send `NULL` to prevent the IP address from being stored. |
| `metadata` | No | JSON object | Store any arbitrary data to be recorded against the purchase order. |
