# Security

MFAStack is a solution intended to enhance security, therefore it also implements certain security best practices.

- MFAStack doesn't allow file uploading.
- MFAStack comes with a self-signed certificate and all requests handled over HTTPS. This should be changed to a CA signed certificate when possible.
- Maximum request size is 2MB. Larger requests are intended for Yubikey mass import facility. If a request is larger than 2MB, the client will be disconnected and request dropped.
- MFAStack doesn't use cookies, unless an administrator needs to login to the administrative control panel. 
- Cookies used are encrypted using a 128-bit secret key which is generated upon installation.
- Every administrative action requires a confirmation using an administrative Identity. This means that if you log on to admin panel and leave your desk, someone else can't walk in and perform destructive actions using your account (assuming you don't leave your 2 factor device lying around!)
- Admin session lasts for 10 minutes, unless extended by performing an action. We have an indicator for session duration in the top right of the interface.
- Server and PHP signature headers are hidden.
- No errors are ever returned to the user (user being the client talking to MFAStack over HTTP), they are logged to server log. This is done so that malicious requests don't force the server to reveal sensitive information.
- For Identities based on RFC 4226 and RFC 6238 algorithms, MFAStack provides a one-time-link facility where the QR code is loaded and shown so that the end user can scan it with their (mobile) device. Information transferred is encrypted using CryptoJS so that using browser developer tools doesn't reveal this information, and as the name indicates - these links are valid only once.

