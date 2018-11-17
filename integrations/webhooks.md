# Integrations - Webhooks

Webooks are a way to deliver [Event payloads](/integrations/event_payloads) to your API.

### Signature Validation

Every event that is sent to your webhooks are signed using unique a `SHA256 HMAC Key` in the `X-SHA256-Digest` header. You can verify the authenticity of the payload by generating a comparison signature using the HMAC Key, a `SHA256 digest` and the `JSON` body.

You will find your HMAC Key in the options menu beside each integration in the App General Settings page on the Mbaasy App Publisher Developer Console.
