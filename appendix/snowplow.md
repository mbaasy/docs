# Appendix - Webhook Integrations with Snowplow

Mbaasy's [webhook integrations](/integrations/webhooks/) work seamlessly with [Snowplow](https://snowplowanalytics.com), but you must ensure to set `additionalProperties` to `true` in your Iglu JSON Schema, because our [event payloads](/integrations/event_payloads/) may contain properties you are not expecting, such as those included in the `previous_attributes` and `new_attributes` objects as well as the introduction of new properties that may not have been announced yet.

See <https://discourse.snowplowanalytics.com/t/setting-additionalproperties-to-true-in-iglu-json-schemas/151> for for more information.
