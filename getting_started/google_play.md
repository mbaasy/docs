# Getting Started - Configuring an Android (Google Play) app

1. [Package name](#package-name)
1. [License Key](#license-key)
1. [Service account credentials](#service-account-credentials)
1. [Real-time developer notifications](#real-time-developer-notifications)


![Play Store Settings](/assets/images/play_store/play_store_settings.jpg)

---

## Package name

The *Package name* uniquely identifies your app in the *Play Store*.

To find your *Package name*, first navigate and login to the [Google Play Developer Publisher Console](https://play.google.com/apps/publish), then navigate to *All applications*.

You will find your *Package name* in the list of applications beneath the app's name.

---

## License key

The *license key* is used to verify the *purchase signature* when a new purchase is made.

![Step 1](/assets/images/play_store/license-key.jpg)

To find your *License key*, first navigate and login to the [Google Play Developer Publisher Console](https://play.google.com/apps/publish), then navigate to All applications and select your app.

Click *Development tools* on the left, then click *Services & APIs*.

You will find your *License key* in the *Licensing & in-app billing section*.

---

## Service account credentials

The *service account credentials**is a JSON file containing a private key used for authenticating with the Google Play Developer API.

To create a service account, first navigate and login to the [Google Play Developer Publisher Console - API access](https://play.google.com/apps/publish/#ApiAccessPlace) page.

You will need a linked Google Play Android Developer project, if you don't already have a linked project, click the Create new project button.

![Step 1](/assets/images/play_store/service-account-1.jpg)

Next step is to create a *service account*, click the *Create service account* button and read the instructions carfully.

It will guide to you to navigate to the *Google API Console* to create your *service account* there.

![Step 2](/assets/images/play_store/service-account-2.jpg)

Don't click *Done* until you have created your *service account* in the *Google API Console*.

![Step 3](/assets/images/play_store/service-account-3.jpg)

Once in the *Google API Console* click on *Create service account*.

![Step 4](/assets/images/play_store/service-account-4.jpg)

Give the service account a descriptive name, we recommend naming it *Mbaasy*.

Select *Owner* from *Project role > Project > Owner*.

Select *Furnish a new private key* and ensure *JSON* is selected, then click *Save*.

A .json file will automatically download, keep this file safe as it contains your private key.

Once downloaded, return to your Google Play Console tab, now you can click Done.

![Step 5](/assets/images/play_store/service-account-5.jpg)

You will now see your new *service account* in the *Service accounts* section.

![Step 6](/assets/images/play_store/service-account-6.jpg)

Click on the *Grant access* button and a new page will open. Select *Finance* from *Role* and ensure both *View app information* and *View financial data* are checked.

![Step 7](/assets/images/play_store/service-account-7.jpg)

Click *Add user* and you're done.

---

## Real-time developer notifications

Google Play can send event notifications when subscription entitlements change. Every Google Play App registered with Mbaasy come furnished with a unique Google Cloud Pub/Sub Topic, making the process as simple as copying the Pub/Sub topic from the *[Mbaasy App Publisher Console](https://console.mbaasy.com) > Apps > [App] > Settings > Play Store settings* page and pasting it on the *[Google Play Developer Publisher Console](https://play.google.com/apps/publish) > [App] > Development tools > Services & APIs* page in the *Real-time developer notifications* section.

![Real-time developer notifications](/assets/images/play_store/real-time-developer-notifications.jpg)

This will ensure Mbaasy stays up-to-date with any changes made to your subscriptions.
