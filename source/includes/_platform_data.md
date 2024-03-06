# Platform Data

Some Platforms will require custom attributes and schemas that are not available in an API product's default attribute set.

To address this requirement, Platforms are able to configure custom app-level schemas and attributes to be provided by Publisher partners; as well as to define the regional availability of a Platform Client by region or specific countries.

For example, a Platform Client may require a custom app identifier, `custom_app_id`, is returned in UpNext API responses. In order to ensure such custom app identifier is provided by a Publisher Client, the Platform will configure this custom attribute from within the Client in the Paket Developer Portal.

> Example `platform_data` attribute:

```
{
    ...,
    platform_data: {
        custom_app_id: 12345,
        deep_link: "app://12345/cid12345"
    }
}
```

When preparing to integrate with the Platform, the Publisher will configure the requested custom attribute, in this case the `custom_app_id`. This data will then be returned in subsequent Platform API responses under the `platform_data` attribute.

Taking this example further, perhaps the Platform Client also requires a `deep_link` url attribute comprised of the custom attribute `custom_app_id` and the default attribute `content_id`. In this example, the data must be returned using the following schema: `app://custom_app_id/content_id`.

![Custom Schema Definition](custom_schema.png)

The Platform will also define this schema under the custom attribute `deep_link` and by using handlebars to define the schema: `app://{{custom_app_id}}/{{content_id}}`.

If the value of an Publisher's `custom_app_id` is `12345` and an item's `content_id` is `cid12345`, the resulting `platform_data` attribute returned in an API response to the requesting Platform will look something like the example to the right.

