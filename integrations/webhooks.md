# Integrations - Webhooks

Webooks are a way to deliver [Event payloads](/integrations/event_payloads/) to your API.

To enable a Webhook integration, visit the ***[Mbaasy App Publisher Console](https://console.mbaasy.com) > Apps [App] > Settings > Integrations*** page and add a new Webhook integration by selecting `Webhook` as the *target*, enter a unique `name` and your `Endpoint URL`.

Webhooks will be delivered using the **POST** HTTP method.

### Signature Validation

Every event that is sent to your webhooks are signed using a unique `SHA256 HMAC Key` in the `X-SHA256-Digest` header. You can verify the authenticity of the payload by generating a comparison signature using the HMAC Key, a `SHA256 digest` and the `JSON` body.

You will find your HMAC Key in the options menu beside each integration in the App General Settings page on the Mbaasy App Publisher Developer Console.
