---
title: Apple App Store Client API
---

# Client API
---
## Apple App Store Client API

`POST` purchase receipts to the Mbaasy using this API.

### Endpoint URL

`https://api.mbaasy.com/client/itunes_connect/receipts`

### Parameters

| Name | Required | Type | Description |
| ---- | -------- | ---- | ----------- |
| `receipt` | Yes | Base64 encoded string | A base64 encoded string containing the receipt. |
| `country_code` | No | String | [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) country code of the end user's App Store region. [Please refer to Retrieving the iTunes Store Region](https://mbaasy.com/docs/resources/itunes_connect_store_region/)
| `identifier_for_vendor` | No | String | The [[UIDevice identifierForVendor](https://developer.apple.com/reference/uikit/uidevice#//apple_ref/occ/instp/UIDevice/identifierForVendor)]. |
| `user_identifier` | No | String | User ID corresponding to your user database |
| `ip_address` | No | V4 or V6 IP Addresses | IP address of the end user. Defaults to the network request IP if omitted, send `NULL` to prevent the IP address from being stored. |
| `metadata` | No | JSON object | Store any arbitrary data to be recorded against the purchase order. |
