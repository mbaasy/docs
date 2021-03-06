# Client API - Uploading Legacy Google Play Receipts

For existing Android code bases that **do not** retain the original `INAPP_PURCHASE_DATA` and `INAPP_DATA_SIGNATURE` you can use this endpoint in place of the [recommended endpoint](/client_api/google_play).

**⚠ Requires [Authentication](/client_api/authentication/).**

## Legacy Endpoint URL

**POST** `https://api.mbaasy.com/client/google_play/purchase_orders/legacy`

## JSON request body

| Name | Type | Description |
| ---- | ---- | ----------- |
| `package_name` [^man] | String | The packageName extracted from the `INAPP_PURCHASE_DATA` provided by the [getBuyIntent()](https://developer.android.com/google/play/billing/billing_reference.html#getBuyIntent) method. |
| `purchase_token` [^man] | String | The purchaseToken extracted from the `INAPP_PURCHASE_DATA` provided by the [getBuyIntent()](https://developer.android.com/google/play/billing/billing_reference.html#getBuyIntent) method. |
| `product_id` [^man] | String | The productId extracted from the `INAPP_PURCHASE_DATA` provided by the [getBuyIntent()](https://developer.android.com/google/play/billing/billing_reference.html#getBuyIntent) method. |
| `order_id` [^opt] | String | The orderId extracted from the `INAPP_PURCHASE_DATA` provided by the getBuyIntent() method. |
| `user_identifier` [^opt] | String | User ID corresponding to your user database. |
| `ip_address` [^opt] | String | V4 or V6 IP address of the end user. Defaults to the network request IP if omitted, send `NULL` to prevent the IP address from being stored. |
| `metadata` [^opt] | Object | Store any arbitrary data to be recorded against the purchase order. e.g. Campaign ID or prices. |

[^man]: This property is **mandatory**.
[^opt]: This property is **optional**.

## HTTP response codes

| Code | Reason |
| ---- | ------ |
| `200 Okay` | The receipt signature is valid and has been synchronously validated with the [Google Play Developer API](https://developers.google.com/android-publisher/api-ref/). |
| `206 Partial content` | The receipt and signature is valid however the Google Play Developer API was unreachable. The receipt has been enqueued for asynchronous validation with the Google Play Developer API. |
| `401 Unauthorized` | The access token is **invalid** or the `Authorization` header is malformed or missing, see [Authorization](/client_api/authorization). |
| `402 Payment required` | The receipt signature is **invalid** (possible fraud attempt). |
| `422 Unprocessable Entity` | The request payload is invalid, check the errors in the response payload. |
