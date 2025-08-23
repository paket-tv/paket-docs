# Testing

API requests may be tested in a live environment simply by using the API Test Key provided in the [Paket Developer Portal](https://developer.paket.tv).

## Test Keys

Test keys begin with the prefix **`test_sk`** and inherit the same behaviors as live API Secret Keys, save that they call test responses. 

### Test Request Behavior

- Test API requests are validated identically to live requests
- Response schemas are identical to live requests
- Response data is dynamically generated for testing purposes

### Identifying Test Responses

The **`Paket-Response-Type: Test`** header indicates whether or not the response is via the API Test Key.

<aside class="notice">
Test keys allow you to safely test your integration without affecting live data or incurring charges.
</aside>