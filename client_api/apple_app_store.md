# Client API - Uploading Apple App Store Receipts

Notify Mbaasy of, and add metadata to iOS purchases and subscription receipts using this API.

## HTTP endpoint

**POST** `https://api.mbaasy.com/client/itunes_connect/receipts`

## JSON request body

| Name | Required | Type | Description |
| ---- | -------- | ---- | ----------- |
| `receipt` | Yes | Base64 encoded string | A base64 encoded string containing the receipt. |
| `country_code` | No | String | [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) country code of the end user's App Store region. [Please refer to Retrieving the App Store Region](#retrieving-the-app-store-region)
| `identifier_for_vendor` | No | String | The [[UIDevice identifierForVendor](https://developer.apple.com/reference/uikit/uidevice#//apple_ref/occ/instp/UIDevice/identifierForVendor)]. |
| `user_identifier` | No | String | User ID corresponding to your user database |
| `ip_address` | No | V4 or V6 IP Addresses | IP address of the end user. Defaults to the network request IP if omitted, send `NULL` to prevent the IP address from being stored. |
| `metadata` | No | JSON object | Store any arbitrary data to be recorded against the purchase order. e.g. Campaign ID or prices. |

## HTTP response codes

| Code | Name | Reason |
| ---- | ---- | ------ |
| `200` | `Okay` | The receipt signature is valid and has been synchronously validated with the App Store API. |
| `206` | `Partial content` | The receipt and signature is valid however the App Store API was unreachable. The receipt has been enqueued for asynchronous validation with the App Store API. |
| `401` | `Unauthorized` | The access token is **invalid** or the `Authorization` header is malformed or missing, see [Authorization](/client_api/authorization). |
| `402` | `Payment required` | The receipt signature is **invalid** (possible fraud attempt). |
| `422` | `Unprocessable Entity` | The request payload is invalid, check the errors in the response payload. |

## Retrieving the App Store Region

The `country_code` property is dependent on the user's App Store region (which is not necessarily their current, physical location). It isn't trivial to find because it is only accessible from the [priceLocale](https://developer.apple.com/documentation/storekit/skproduct/1506145-pricelocale) instance property on the [SKProduct](https://developer.apple.com/documentation/storekit/skproduct) object.

[priceLocale](https://developer.apple.com/documentation/storekit/skproduct/1506145-pricelocale) extends from the [Locale](https://developer.apple.com/documentation/foundation/locale) structure and can be cast as a [NSLocale](https://developer.apple.com/documentation/foundation/nslocale) instance.

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
