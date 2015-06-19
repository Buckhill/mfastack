# U2F Support

MFAStack supports U2F and allows administrative users to access the admin control panel using a U2F Yubikey.
However, we can't document U2F capabilities using interactive Swagger documentation.

# U2F enrollment challenge

Requesting the enrollment challenge is done by issuing a `POST` request to resource `/v1/u2f/enroll/chalenge`, providing the following information:

- Display Title
- Yubikey Serial Number (optional)
- FIDO AppId (mandatory)

### HTTP Request
The entire HTTP request in its raw format looks like this:

```
POST /ui/u2f/enroll/register HTTP/1.1
Host: your.mfastack.domain
Connection: keep-alive
Content-Length: 1705
Accept: application/json, text/javascript, */*; q=0.01
Origin: https://your.mfastack.domain
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Windows NT 6.3; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/43.0.2357.124 Safari/537.36
Content-Type: application/json
Accept-Encoding: gzip, deflate
Accept-Language: en-GB,en;q=0.8,en-US;q=0.6,hr;q=0.4,sr;q=0.2

title=My+Yubikey&serial_number=1234+5678&facet_id=https%3A%2F%2Fserver.mfa.web
```

Method: `POST`
Resource: `/v1/u2f/enroll/challenge`

HTTP Body fields:
- `title` - the title of the U2F Yubikey, it appears under this title in the administrative control panel
- `serial_number` - optional serial number
- `facet_id` - FIDO AppId
 
### HTTP Response

In case of success, HTTP status code is 200 and the response is JSON-encoded message:
 
```
{
    "requestId":49,
    "challenge":"Vk9TbWJ4YklMOVlBa01RRmJqYzV4MmpiUWQ0YXREUVk",
    "version":"U2F_V2",
    "appId":"https://server.mfa.web",
    "title":"My U2F Yubikey"
}
```

Once the device starts flashing and the challenge is signed correctly, the integrating application must also provide `requestId` value in the upcoming authentication step. 
 

# U2F enrollment authentication

After obtaining the challenge and using U2F Yubikey to sign it, you should perform another `POST` request to `/v1/u2f/enroll/register`.

HTTP body fields: 

- `registration` - contains JSON-encoded output of Yubikey (`var response = JSON.stringify(what_yubikey_returns);`).

### HTTP Request (after signing)

```
POST /ui/u2f/enroll/register HTTP/1.1
Host: your.mfastack.domain
Connection: keep-alive
Content-Length: 1705
Accept: application/json, text/javascript, */*; q=0.01
Origin: https://your.mfastack.domain
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Windows NT 6.3; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/43.0.2357.124 Safari/537.36
Content-Type: application/json
Accept-Encoding: gzip, deflate
Accept-Language: en-GB,en;q=0.8,en-US;q=0.6,hr;q=0.4,sr;q=0.2

{"registration":"{\"type\":\"u2f_register_response\",\"requestId\":1,\"responseData\":{\"registrationData\":\"BQQEGeZXsuxqD_Wckla-cvMbQ6KX8r0Kwg43_u34Uhxpp1J56cIQo9ehcv4uYp0dXRauhF0gGuQXt6_I2XZdQKkNQLWhRjFx4lj3aGf3xAL4s_dJcqjganv7cwOoXYuqhjkc3rnjtlNEuJUdfHFzVpwVfLcCrdwhJnaPxkSDngQT62owggIcMIIBBqADAgECAgQk26tAMAsGCSqGSIb3DQEBCzAuMSwwKgYDVQQDEyNZdWJpY28gVTJGIFJvb3QgQ0EgU2VyaWFsIDQ1NzIwMDYzMTAgFw0xNDA4MDEwMDAwMDBaGA8yMDUwMDkwNDAwMDAwMFowKzEpMCcGA1UEAwwgWXViaWNvIFUyRiBFRSBTZXJpYWwgMTM1MDMyNzc4ODgwWTATBgcqhkjOPQIBBggqhkjOPQMBBwNCAAQCsJS-NH1HeUHEd46-xcpN7SpHn6oeb-w5r-veDCBwy1vUvWnJanjjv4dR_rV5G436ysKUAXUcsVe5fAnkORo2oxIwEDAOBgorBgEEAYLECgEBBAAwCwYJKoZIhvcNAQELA4IBAQCjY64OmDrzC7rxLIst81pZvxy7ShsPy2jEhFWEkPaHNFhluNsCacNG5VOITCxWB68OonuQrIzx70MfcqwYnbIcgkkUvxeIpVEaM9B7TI40ZHzp9h4VFqmps26QCkAgYfaapG4SxTK5k_lCPvqqTPmjtlS03d7ykkpUj9WZlVEN1Pf02aTVIZOHPHHJuH6GhT6eLadejwxtKDBTdNTv3V4UlvjDOQYQe9aL1jUNqtLDeBHso8pDvJMLc0CX3vadaI2UVQxM-xip4kuGouXYj0mYmaCbzluBDFNsrzkNyL3elg3zMMrKvAUhoYMjlX_-vKWcqQsgsQ0JtSMcWMJ-umeDMEUCIQC4ftin6wROYMxwNq4yzDqxKyfad_MQcGFsHQtwPdJ1ugIgGitkw-v9Fp22rusUv2tD49Jua4eL2wO39BYCz4E1hBI\",\"requestId\":49,\"challenge\":\"Vk9TbWJ4YklMOVlBa01RRmJqYzV4MmpiUWQ0YXREUVk\",\"version\":\"U2F_V2\",\"appId\":\"https://your.mfastack.domain\",\"title\":\"My U2F Yubikey\",\"clientData\":\"eyJ0eXAiOiJuYXZpZ2F0b3IuaWQuZmluaXNoRW5yb2xsbWVudCIsImNoYWxsZW5nZSI6IlZrOVRiV0o0WWtsTU9WbEJhMDFSUm1KcVl6VjRNbXBpVVdRMFlYUkVVVmsiLCJvcmlnaW4iOiJodHRwczovL3NlcnZlci5tZmEud2ViIiwiY2lkX3B1YmtleSI6IiJ9\"}}"}

```

### HTTP Response

In case of success, HTTP status code is 200 and the response is JSON-encoded message carrying the newly created `Identity` information:

```
{
    "identity_id":198,
    "title":"My U2F Yubikey"
}
```


# U2F auth challenge

Once the device is enrolled, you can perform U2F authentication challenge by specifying the Identity ID in the resource path.

The method is `POST` and the resource is `/v1/u2f/auth/challenge/{identity_id}`. Required resource path parameter is numeric ID of the `Identity`. If the Identity is a valid U2F Identity, you will receive correct challenge back.


 
### HTTP Request



# U2F authentication

