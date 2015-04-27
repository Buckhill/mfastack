# MFA Stack

MFA Stack is a Key Storage Server and Validator, which enables 2-factor authentication, and optionally
Single Sign On, for existing apps and websites.

MFA Stack is available in three variations:-

* MFALib
* MFA Stack - 2FA Server
* MFA Stack - 2FA Server + SSO

### MFALib

This repository hosts the MFALib code base.   It is suitable for software vendors looking for a 2 factor authentication library which can be directly included in a software project via Composer. The library provides classes and methods for handling key storage and OTP validation but does not provide a REST API, SSO or any management facilities.

### MFA Stack - 2FA Server 

<a href="https://www.mfastack.co.uk/pricing">View on MFAStack.co.uk</a>

Suitable for software vendors looking for a turnkey 2 factor authentication server which communicates via REST API. The linking of users to 2-factor identities and devices will be handled by the software vendor in their own application. MFA Stack server will simply act as a KSS (Key Storage Server) and OTP (One time password) validator.

MFA Stack server comes bundled with a web GUI which consumes its own REST API. For ease of deployment, and for security best practice, MFA Stack server comes as a <a href="https://www.mfastack.co.uk/client/downloads" target="_blank">downloadable VM image</a> or via a <a href="https://www.mfastack.co.uk/client/downloads" target="_blank">1-click AWS installation</a>.

### MFA Stack - 2FA Server + SSO 

<a href="https://www.mfastack.co.uk/pricing">View on MFAStack.co.uk</a>

Suitable for companies looking to add 2-factor authentication and SSO (Single Sign On) to their existing applications. This iteration of MFA Stack comes bundled with SimpleSAMLphp IdP, the MFA Stack SimpleSAMLphp Module (enables OTP on the IdP) and the MFA Stack Identity Management App.

This is a complete 2-factor enabled, Federated Services system. Flexible enough to work with nearly any existing user database (MySQL, LDAP, AD, ADFS) and any SP, supporting both SAML 1 and 2.

For ease of deployment, and for security best practice, MFA Stack + SSO server comes as a <a href="https://www.mfastack.co.uk/client/downloads" target="_blank">downloadable VM image</a> or via a <a href="https://www.mfastack.co.uk/client/downloads" target="_blank">1-click AWS installation</a>.
