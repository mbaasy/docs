# Client API - Uploading Google Play Receipts

Notify Mbaasy of, and add metadata to Android purchases and subscription receipts using this API.

**Note:** This API endpoint expects the `INAPP_PURCHASE_DATA` and `INAPP_DATA_SIGNATURE` properties provided by the [getBuyIntent()](https://developer.android.com/google/play/billing/billing_reference.html#getBuyIntent) method to be intact. If this is not available you may use the [Legacy API endpoint](/client_api/legacy_google_play/) to send individual properties.

## HTTP endpoint

**POST** `https://api.mbaasy.com/client/google_play/purchase_orders`

## JSON request body

| Name | Required | Type | Description |
| ---- | -------- | ---- | ----------- |
| `purchase_data` | Yes | JSON String | The original JSON string from the the `INAPP_PURCHASE_DATA` provided by the [getBuyIntent()](https://developer.android.com/google/play/billing/billing_reference.html#getBuyIntent) method.<br />**Important:** The `INAPP_PURCHASE_DATA` must be intact and as provided by the [getBuyIntent()](https://developer.android.com/google/play/billing/billing_reference.html#getBuyIntent) method. For this reason, the API will only accept the original JSON string. |
| `purchase_signature` | Yes | Base64 encoded string | A Base64 encoded string containing the `INAPP_DATA_SIGNATURE` provided by the [getBuyIntent()](https://developer.android.com/google/play/billing/billing_reference.html#getBuyIntent) method.<br />**Important:** The `INAPP_DATA_SIGNATURE` is already Base64 encoded by the [getBuyIntent()](https://developer.android.com/google/play/billing/billing_reference.html#getBuyIntent) method. There is no need to re-encode the string when sending to the API. |
| `user_identifier` | No | String | User ID corresponding to your user database. |
| `ip_address` | No | V4 or V6 IP Addresses | IP address of the end user. Defaults to the network request IP if omitted, send `NULL` to prevent the IP address from being stored. |
| `metadata` | No | JSON object | Store any arbitrary data to be recorded against the purchase order. |

## HTTP response codes

| Code | Name | Reason |
| ---- | ---- | ------ |
| `200` | `Okay` | The receipt signature is valid and has been synchronously validated with the [Google Play Developer API](https://developers.google.com/android-publisher/api-ref/). |
| `206` | `Partial content` | The receipt and signature is valid however the Google Play Developer API was unreachable. The receipt has been enqueued for asynchronous validation with the Google Play Developer API. |
| `401` | `Unauthorized` | The access token is **invalid** or the `Authorization` header is malformed or missing, see [Authorization](/client_api/authorization). |
| `402` | `Payment required` | The receipt signature is **invalid** (possible fraud attempt). |
| `422` | `Unprocessable Entity` | The request payload is invalid, check the errors in the response payload. |
