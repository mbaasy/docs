# Client API - Uploading Apple App Store Receipts

Notify Mbaasy of, and add metadata to iOS purchases and subscription receipts using this API.

**⚠ Requires [Authentication](/client_api/authentication/).**

## HTTP endpoint

**POST** `https://api.mbaasy.com/client/itunes_connect/receipts`

## JSON request body

| Name | Type | Description |
| ---- | ---- | ----------- |
| `receipt`[^man] | String | A Base64 encoded string containing the receipt. |
| `country_code`[^opt] [^country-code] | String | [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) country code of the end user's App Store region. [See country_code footnote](#fn:country-code). |
| `identifier_for_vendor`[^opt] | String | The [[UIDevice identifierForVendor](https://developer.apple.com/reference/uikit/uidevice#//apple_ref/occ/instp/UIDevice/identifierForVendor)]. |
| `user_identifier`[^opt] | String | User ID corresponding to your user database |
| `ip_address`[^opt] | String | V4 or V6 IP address of the end user. Defaults to the network request IP if omitted, send `NULL` to prevent the IP address from being stored. |
| `metadata`[^opt] | Object | Store any arbitrary data to be recorded against the purchase order. e.g. Campaign ID or prices. |

[^man]: This property is **mandatory**.

[^opt]: This property is **optional**.

[^country-code]: The `country_code` property is dependent on the user's App Store region (which is not necessarily their current, physical location). It is accessible from the [priceLocale](https://developer.apple.com/documentation/storekit/skproduct/1506145-pricelocale) instance property on the [SKProduct](https://developer.apple.com/documentation/storekit/skproduct) object of any of your available products.

    The [priceLocale](https://developer.apple.com/documentation/storekit/skproduct/1506145-pricelocale) instance extends from the [Locale](https://developer.apple.com/documentation/foundation/locale) structure and can be cast as a [NSLocale](https://developer.apple.com/documentation/foundation/nslocale) instance.

    Using the [CFLocaleGetValue](https://developer.apple.com/documentation/corefoundation/1543547-cflocalegetvalue?language=objc) function, you can pass the [NSLocale](https://developer.apple.com/documentation/foundation/nslocale) instance and the [kCFLocaleCountryCode](https://developer.apple.com/documentation/corefoundation/kcflocalecountrycode?language=objc) key property to return the country code associated to the user's App store account.

    Here is a quick example in Objective-C:

    ```objc
    - (void)productsRequest:(SKProductsRequest *)request didReceiveResponse:(SKProductsResponse *)response
    {
        SKProduct *product = [response.products objectAtIndex:0]; // Get the first product
        NSLocale* priceLocale = product.priceLocale; // Get the priceLocale
        NSString *countryCode = (NSString*)CFLocaleGetValue((CFLocaleRef)priceLocale, kCFLocaleCountryCode); // Get the country code
    }
    ```

    Credit to [this Stack Overflow answer](https://stackoverflow.com/questions/14453910/how-to-get-locale-currency-price-for-in-app-purchases-in-ios/14602248#answer-14621894).

## HTTP response codes

| Code | Reason |
| ---- | ------ |
| `200 Okay` | The receipt signature is valid and has been synchronously validated with the App Store API. |
| `206 Partial content` | The receipt and signature is valid however the App Store API was unreachable. The receipt has been enqueued for asynchronous validation with the App Store API. |
| `401 Unauthorized` | The access token is **invalid** or the `Authorization` header is malformed or missing, see [Authorization](/client_api/authorization). |
| `402 Payment required` | The receipt signature is **invalid** (possible fraud attempt). |
| `422 Unprocessable Entity` | The request payload is invalid, check the errors in the response payload. |

## JSON response body

| Name | Type | Description |
| ---- | ---- | ----------- |
| `id` | UUID | Mbaasy database ID of the receipt. |
| `style` | String | Style of the receipt, either "transaction" or "unified". |
| `app_family_id` | UUID | Mbaasy database ID of the app. |
| `bundle_id` | String | Bundle ID referenced in the receipt. |
| `is_fraud` | Boolean | Whether the receipt is fraudulent. |
| `fraud_reason` | String | The reason the receipt was marked as fraudulent. |
| `environment` | String | The environment of the receipt, either "sandbox" or "production". |
| `user_identifier` | String | The `user_identifier` as provided from the [request payload](#json-request-body). |
| `metadata` | Object | The `metadata`  as provided from the [request payload](#json-request-body). |
| `ip_address` | String | The `ip_address` as provided from the [request payload](#json-request-body). |
| `in_app_purchases` | Array | An array (list) of [In-App Purchase resources](/glossary/in_app_purchase_resource/). (Returns an array because "unified" receipts may contain multiple purchases. |
| `created_at` | Timestamp[^ts] | Date and time when the receipt was created on Mbaasy. |
| `updated_at` | Timestamp[^ts] | Date and time when the receipt was last updated. |

[^ts]: Timestamps are objects that contain an `ms` (Unix Timestamp in milliseconds) and `utc` property. e.g.
    ```json
    {
      "utc": "2018-02-26 10:35:47 UTC",
      "ms": 1519641347834
    }
    ```
