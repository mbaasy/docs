# Getting Started - Subscription Update Fetching

Subscription update fetching ensures subscriptions are fetched after the current period has ended and are fetched at least once every 24 hours while in grace period. It works in addition to subscription status updates from the App Store and Play Store where changes to properties such as the auto renewing preference don't trigger a status update notification, and also as a fail safe in-case status updates are not being received.

You'll find the subscription fetching controls on the *[Mbaasy App Publisher Console](https://console.mbaasy.com) > [App] > Settings > General Settings* Page.

<div class="alert alert-warning">
  <p>⚠ Please be aware that subscription update fetching counts toward your allocated events regardless of any detected changes to the subscription properties.</p>
</div>

![Subscription update fetching](/assets/images/subscription_update_fetching.jpg)

The following levels enable more precise tracking of changes to properties such as the auto renewing preference that don't trigger a status update notification from the App Store and Play Store.

## Level 1

Enabling Level 1 ensures active subscriptions are fetched at least once every 28 days.

## Level 2

Enabling Level 2 ensures active subscriptions are fetched at least once every 7 days when the current period will end within 28 days.

## Level 3

Enabling Level 3 ensures active subscriptions are fetched at least once every 24 hours when the current period will end within 7 days.
