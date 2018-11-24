# Getting Started - Importing historical data

Historical data can be uploaded via the *[Mbaasy App Publisher Console](https://console.mbaasy.com) > Apps > [App] > Settings > Imports* page.

## How to create an import file

Import files must be [Gzipped](https://en.wikipedia.org/wiki/Gzip) and [New-line deliminated JSON](http://ndjson.org/) with each line containing an individual JSON object that includes the properties from the *JSON request body* from either the [Apple App Store](/client_api/apple_app_store) or [Google Play](/client_api/google_play) Client API.

One additional property named `marketplace` must be included and be set to either `itunes` or `google` depending on the payload.

## Import file example

```json
{"marketplace": "itunes", "receipt": "QXBwbGUgcmVjZWlwdA==", "user_identifer": "42", "ip_address": "156.33.241.5", "metadata": {"campaign_id": "99"}}
{"marketplace": "google", "purchase_data": "{\"autoRenewing\":true,\"developerPayload\":{\"uid\":\"42\"},\"packageName\":\"com.example.app\",\"productId\":\"premium.1.month\",\"purchaseState\":0,\"purchaseStateEnum\":\"PURCHASED\",\"purchaseTime\":1491983056073,\"purchaseToken\":\"Lh27.AO-sER_DvQ_fEChr-XA6sK\", \"orderId\": \"GPA.1234-5678-9123-45678\"}", "purchase_signature": "UHVyY2hhc2Ugc2lnbmF0dXJl", "user_identifer": "23", "ip_address": "99.88.77.00", "metadata": {"campaign_id": "99"}}
{"marketplace": "google", "package_name": "com.example.app", "product_id": "premium.1.month", "order_id": "GPA.1234-5678-9123-45678", "user_identifer": "120", "ip_address": "88.77.66.55", "metadata": {"campaign_id": "99"}}
```

In this example, `Line 1` contains an App Store purchase, identified by the marketplace value `itunes` and properties from the [Apple App Store Client API](/client_api/apple_app_store) is used.

`Line 2` and `Line 3` both contain Google Play purchases identified by the marketplace value `google`.

`Line 2` contains the original `INAPP_PURCHASE_DATA` JSON string and `INAPP_DATA_SIGNATURE` so the properties from the [Google Play Client API](/client_api/google_play) are used.

For `Line 3` however, the `INAPP_PURCHASE_DATA` JSON string and `INAPP_DATA_SIGNATURE` was not available, so we used the `purchase_token`, `package_name` and `product_id` properties from the [Google Play Legacy Client API](/client_api/legacy_google_play) instead.
