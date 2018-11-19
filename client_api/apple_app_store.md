# Client API - Uploading Apple App Store Receipts

Notify Mbaasy of, and add metadata to iOS purchases and subscription receipts using this API.

## HTTP endpoint

**POST** `https://api.mbaasy.com/client/itunes_connect/receipts`

## JSON request body

| Name | Required | Type | Description |
| ---- | -------- | ---- | ----------- |
| `receipt` | Yes | Base64 encoded string | A base64 encoded string containing the receipt. |
| `country_code` | No | String | [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) country code of the end user's App Store region. [Please refer to Retrieving the iTunes Store Region](https://mbaasy.com/docs/resources/itunes_connect_store_region/)
| `identifier_for_vendor` | No | String | The [[UIDevice identifierForVendor](https://developer.apple.com/reference/uikit/uidevice#//apple_ref/occ/instp/UIDevice/identifierForVendor)]. |
| `user_identifier` | No | String | User ID corresponding to your user database |
| `ip_address` | No | V4 or V6 IP Addresses | IP address of the end user. Defaults to the network request IP if omitted, send `NULL` to prevent the IP address from being stored. |
| `metadata` | No | JSON object | Store any arbitrary data to be recorded against the purchase order. |

## HTTP response codes

| Code | Name | Reason |
| ---- | ---- | ------ |
| `200` | `Okay` | The receipt signature is valid and has been synchronously validated with the App Store API. |
| `206` | `Partial content` | The receipt and signature is valid however the App Store API was unreachable. The receipt has been enqueued for asynchronous validation with the App Store API. |
| `401` | `Unauthorized` | The access token is **invalid** or the `Authorization` header is malformed or missing, see [Authorization](/client_api/authorization). |
| `402` | `Payment required` | The receipt signature is **invalid** (possible fraud attempt). |
| `422` | `Unprocessable Entity` | The request payload is invalid, check the errors in the response payload. |
