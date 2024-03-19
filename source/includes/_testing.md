# Testing

API requests may be tested in a live environment simply by using the API Test Key provided in the [Paket Developer Portal](https://developer.paket.tv). Test keys begin with the prefix `test_sk` and inherit the same behaviors as live API Secret Keys, save that they call test responses. 

Test API requests are validated identically to live requests and although the response schema are identical to live requests, the responses themselves are dynamically generated. 

The `Paket-Response-Type: Test` header indicates whether or not the response is via the API Test Key.