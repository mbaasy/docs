# Client API - Uploading Google Play Receipts

Notify Mbaasy of, and add metadata to Android purchases and subscription receipts using this API.

**Note:** This API endpoint expects the `INAPP_PURCHASE_DATA` and `INAPP_DATA_SIGNATURE` properties provided by the [getBuyIntent()](https://developer.android.com/google/play/billing/billing_reference.html#getBuyIntent) method to be intact. If this is not available you may use the [Legacy API endpoint](/client_api/legacy_google_play/) to send individual properties.

**⚠ Requires [Authentication](/client_api/authentication/).**

## HTTP endpoint

**POST** `https://api.mbaasy.com/client/google_play/purchase_orders`

## JSON request body

| Name | Type | Description |
| ---- | ---- | ----------- |
| `purchase_data`[^man] | String | The original JSON string from the the `INAPP_PURCHASE_DATA` provided by the [getBuyIntent()](https://developer.android.com/google/play/billing/billing_reference.html#getBuyIntent) method.<br />**Important:** The `INAPP_PURCHASE_DATA` must be intact and as provided by the [getBuyIntent()](https://developer.android.com/google/play/billing/billing_reference.html#getBuyIntent) method. For this reason, the API will only accept the original JSON string. |
| `purchase_signature`[^man] | String | A Base64 encoded string containing the `INAPP_DATA_SIGNATURE` provided by the [getBuyIntent()](https://developer.android.com/google/play/billing/billing_reference.html#getBuyIntent) method.<br />**Important:** The `INAPP_DATA_SIGNATURE` is already Base64 encoded by the [getBuyIntent()](https://developer.android.com/google/play/billing/billing_reference.html#getBuyIntent) method. There is no need to re-encode the string when sending to the API. |
| `user_identifier`[^opt] | String | User ID corresponding to your user database. |
| `ip_address`[^opt] | String | V4 or V6 IP address of the end user. Defaults to the network request IP if omitted, send `NULL` to prevent the IP address from being stored. |
| `metadata`[^opt] | Object | Store any arbitrary data to be recorded against the purchase order. e.g. Campaign ID or prices. |

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

## JSON response body

| Name | Type | Description |
| ---- | ---- | ----------- |
| `id` | UUID | Mbaasy database ID of the purchase order. |
| `app_family_id` | UUID | Mbaasy database ID of the app. |
| `package_name` | String | Package name referenced in the `INAPP_PURCHASE_DATA`. |
| `is_legacy` | Boolean | Weather the legacy endpoint was used to upload the purchase order |
| `is_fraud` | Boolean | Whether the purchase order is fraudulent. |
| `fraud_reason` | String | The reason the purchsae order was marked as fraudulent. |
| `environment` | String | The environment of the receipt, either "sandbox" or "production". |
| `purchase_token` | String | The purchase token referenced in the `INAPP_PURCHASE_DATA`. |
| `order_id` | String | The order ID referneced in the `INAPP_PURCHASE_DATA`. |
| `user_identifier` | String | The `user_identifier` as provided from the [request payload](#json-request-body). |
| `metadata` | Object | The `metadata`  as provided from the [request payload](#json-request-body). |
| `ip_address` | String | The `ip_address` as provided from the [request payload](#json-request-body). |
| `in_app_purchase` | Object | The [In-App Purchase resources](/glossary/in_app_purchase_resource/). |
| `created_at` | Timestamp[^ts] | Date and time when the purchase order was created on Mbaasy. |
| `updated_at` | Timestamp[^ts] | Date and time when the purchase order was last updated. |

[^ts]: Timestamps are objects that contain an `ms` (Unix Timestamp in milliseconds) and `utc` property. e.g.
    ```json
    {
      "utc": "2018-02-26 10:35:47 UTC",
      "ms": 1519641347834
    }
    ```
