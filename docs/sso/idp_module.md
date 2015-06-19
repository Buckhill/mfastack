# SimpleSAMLphp IDP MFAStack module

The module we have created essentially adds an [Authentication filter](https://simplesamlphp.org/docs/stable/simplesamlphp-authproc) to the IDP. The authentication filter adds an additional step to the authentication process on the IDP and that step is OTP verification. 

To achieve that the MFAStack REST API is called remotely from the IDP during the authentication process.

The good thing about this approach is that any SP (service provider - any app that want's to use SSO and delegate user authentication to the IDP) that connects to the IDP for SSO purposes gets the multi-factor authentication feature out of the box - there is no need to change the SP additionally - all that is required is to integrate with a MFAStack + SSO IDP, which usually boils down to exchanging metadata between the SP and the IDP, and upgrading the SP slightly to delegate user authentication to the IDP instead of the SP doing it itself.
