# Getting Started - Configuring an iOS (Apple App Store) app

1. [Bundle ID](#bundle-id)
1. [Shared secret](#shared-secret)
1. [Subscription status URL](#subscription-status-url)

![App Store Settings](/assets/images/app_store/app_store_settings.jpg)

---

### Bundle ID

To find your *Bundle ID*, navigate to the ***[App Store Connect](https://appstoreconnect.apple.com) > My Apps > [App] > App Information*** page and copy the *Bundle ID*.

![Bundle ID](/assets/images/app_store/bundle_id.jpg)

---

### App Specific Shared Secret

To find your *App Specific Shared Secret*, navigate to the ***[App Store Connect](https://appstoreconnect.apple.com) > My Apps > Apps > [App] > Features > In-App Purchases*** page and click on *App-Specific Shared Secret*. Click *Generate Shared Secret* and copy the *Shared Secret*.

![Shared secret](/assets/images/app_store/shared_secret.jpg)

---

### Subscription Status URL

The App Store can send event notifications when subscription entitlements change. Your Mbaasy connected iOS apps come furnished with a unique *Subscription Status URL*. Copy the *Subscription Status URL* from the ***[Mbaasy App Publisher Console](https://console.mbaasy.com) > Apps > [App] > Settings > App Store settings*** page and paste it into the *Subscription Status URL* field on the ***[App Store Connect](https://appstoreconnect.apple.com) > My Apps > [App] > App Information*** page.

![Subscription Status URL](/assets/images/app_store/subscription_status_url.jpg)
