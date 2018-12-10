# Entitlements

Entitlements offer a solution to organizing your app's product IDs in simple, nested collections of ***entitlements*** and ***offerings***.

An ***entitlement*** describes the content or feature which your app makes available or 'unlocks' during an active subscription or after a one-time or consumable purchase. Give your ***entitlements*** a name that describes the content to unlock, for example: `premium` or `pro`.

An ***offering*** describes the purchase options available to an ***entitlement***, such as the duration. Give your ***offerings*** a descriptive name, for example: `weekly`, `monthly` or `annually`.

Product IDs are auto-discovered, either in real-time when a purchase is made, or in the case of Google Play apps, via the [Google Play Developer API - Inappproducts](https://developers.google.com/android-publisher/api-ref/inappproducts) endpoint synchronized every 24 hours. On first discovery they are immediately associated to ***offerings*** that match to your defined [regular expression](https://en.wikipedia.org/wiki/Regular_expression) patterns.

## Defining Product ID Regular Expression Patterns

Regular expression patterns are matched at **at any position in the product ID**, therefor, it is important to define patterns as explicitly as possible to avoid unwanted or unexpected matches. For instance, a pattern of only `premium` will match any product ID containing the string `premium`.

Adding the `^` and `$` anchors at the beginning and end of the pattern will ensure an exact match, e.g. the pattern `^premium.1month$` will perform an exact match on the product ID `premium.1month` but will not match the product ID `premium.1month.extra`. However a pattern of `^premium.1month` (note the absence of `$` at the end) will match any product ID that begins with `premium.1month`.

Similarly, a pattern of `extra$` will match any product ID with the word `extra` at the end.

## Example

The "Example App" offers two plans: `premium` and `pro`. The `premium` plan have some paid features but the `pro` plan has everything.

Over the years the app has been in production, there have been a number of product IDs created, in both the App Store and Google Play to account for pricing changes, holiday specials and A/B tests.

Product IDs mostly follow a convention of `[plan].[platform].[duration].[version]`. e.g. `premium.ios.1month.v4`. But this convention took some time to figure out so there are some older Product IDs that don't follow this convention, e.g. `premium_monthly` and `pro_monthly`, and `premium_yearly` and `pro_yearly`, which relate to the `premium` and `pro` - `monthly` and `yearly` plans respectively.

In this case the entitlements are organized as follows:

* `premium`
  * `monthly`
    * `^premium.\w+.1month.v\d+$`
    * `^premium_monthly$`
  * `3monthly`
    * `^premium.\w+.3month.v\d+$`
  * `6monthly`
    * `/^premium.\w+.6month.v\d+$`
  * `yearly`
    * `^premium.\w+.12month.v\d+$`
    * `^premium_yearly$`
* `pro`
  * `monthly`
    * `^premium.\w+.1month.v\d+$`
    * `^pro_monthly$`
  * `3monthly`
    * `^premium.\w+.3month.v\d+$`
  * `6monthly`
    * `^premium.\w+.6month.v\d+$`
  * `yearly`
    * `^premium.\w+.12month.v\d+$`
    * `^pro_yearly$`

This means that product IDs such as `premium.ios.1month.v2` and `premium.android.1month.v5` will be associated to `premium/monthly` and `pro.ios.3month.v6` will be associated to `pro/3monthly`.

## Resources

* [Regular Expressions Wikipedia Article](https://en.wikipedia.org/wiki/Regular_expression)
* [Regex tutorial — A quick cheatsheet by examples](https://medium.com/factory-mind/regex-tutorial-a-simple-cheatsheet-by-examples-649dc1c3f285)
* [RegExr - Expression Tester](https://regexr.com)
