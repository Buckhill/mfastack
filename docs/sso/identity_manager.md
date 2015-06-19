# Identity Management SP

The identity management app is a small app that does the second part of the MFAStack SSO solution - it is responsible for creating and assigning MFAStack identities to users.

It does its job also by communicating with MFAStack REST API, but things are a bit more complex on the identity manager app, since the REST API routes it uses are protected with OTP verification. This means that there is a need for the identities manager app to 

- generate an valid OTP
- send the generated OTP on each request to MFAStack protected routes

and there is a need for the MFAStack to actually have an identity that will match the OTPs received from the identity manager app.

To achieve all this, a helper available in `MFALib` is used on the identity manager side, which generates RFC6238 (counter-based) OTPs, and the secret key for the identity is exchanged between the identity manager app and MFAStack, which enables the identity manager to communicate with the MFAStack protected REST API.

The identity manager is a simple app which enables users of the IDP to add MFAStack identities to their account. It writes to the small database that the IDP reads while going through the authentication process, and after a user adds an identity in the identity manager app, login to the IDP will not be possible without a valid OTP.
