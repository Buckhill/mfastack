# Features

- REST interface for easy integration
- CLI tools for managing server-side
- OTP validation
- OTP device registration
- Supports 5 different OTP-related algorithms: `Yubico OTP`, `Yubico-OATH`, `FIDO U2F`, `RFC 4226` and `RFC 6238`
- For `RFC 4226` and `RFC 6238` based `Identities`, we provide a secure facility for sharing the scannable QR barcode called `One-time Link` which lets you turn your mobile device into a 2nd factor device
- Built upon tested code and [Lumen](http://lumen.laravel.com) framework
- Uses an ORM to communicate with four different database vendors: MySQL, Postgresql, SQLite, MS SQL
- Updates are performed using GIT and PHP package manager `composer` which turns updating easy via `composer update` CLI-command
- Critical libraries working with OTPs and database are source-open (not open-source and free) so that 3rd parties are able to audit and review the critical code
- Scalable, without hard-rules about how to scale (system administrators are able to apply their experience and knowledge to scale HTTP and DB components using their own approach)
- Two Javascript Single-page-application GUIs provided, for both MFA Stack administration, identities management (OTP/U2F protected) and for users to manage their own identities (SAML protected)
- SimpleSAMLphp IdP integration optional for deploying a company IdP to handle SSO and 2 factor
- Simple installation scripts
- Source available MFALib for auditing of OTP and U2F implementations