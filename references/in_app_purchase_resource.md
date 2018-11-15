---
title: In App Purchase Resource
---

# In App Purchase Resource

| Name | Type | Description |
| ---- | ---- | ----------- |
| `id` | UUID | Mbaasy database ID for the In App Purchase. |
| `app_family_id` | UUID | Mbaasy database ID for the App Family. |
| `fact_id` | UUID | Mbaasy database ID for the In App Purchase Fact. |
| `marketplace` | String | The marketplace the purchase was made. |
| `app_identifier` | String | Unique identifier for the app. |
| `product_id` | String | Marketplace Product ID / SKU for the purchase. |
| `quantity` | Integer | Number of items purchased. |
| `type` | String | Type of purchase. |
| `environment` | String | Environment of the purchase. |
| `country_code` | String | ISO 3166-1 alpha-2 country code / region code of the app store user. |
| `currency_code` | String | ISO 4217 currency code associated to the country_code. |
| `user_identifier` | String | The user_identifier received via an uploaded receipt / purchase order. |
| `is_auto_renewing` | Boolean | Indicates the auto-renewing status of the subscription. |
| `in_trial_period` | Boolean | Indicates if the subscription is in the trial period. |
| `in_grace_period` | Boolean | Indicates if the marketplace is still trying to renew the subscription. |
| `metadata` | Object | The metadata received via an uploaded receipt / purchase order. |
| `purchased_at` | Timestamp | Timestamp when the subscription first started / was originally purchased. |
| `current_period_start_at` | Timestamp | The date and time when the current subscription period started (Only available on subscription purchases) |
| `current_period_end_at` | Timestamp | The date and time when the current subscription period will/has ended. (Only available on subscription purchases) |
| `created_at` | Timestamp | The date and time when the record was created. |
| `updated_at` | Timestamp | The date and time when the record was most recently updated. |
