# MFAStack Introduction

MFAStack is a REST-ful server that validates one-time passwords based on the following algorithms: 

- Yubico OTP
- Yubikey-OATH
- FIDO U2F
- RFC 4226 (time-based one time password algorithm)
- RFC 6238 (counter-based one time password algorithm)

Along with validating the one-time passwords, MFAStack also acts as key storage which holds information required by the mentioned algorithms to work and optionally provides a bundled IdP (SimpleSAMLphp) for existing apps and websites which support SAML.

# MFA Stack is available in two variations

- MFA Stack - 2FA Server
- MFA Stack - 2FA Server + SSO

# Purpose 

The purpose of MFAStack is to provide **unified interface** for existing applications to integrate with in order to implement 2 factor authentication quickly and painlessly.

Since applications are written in various languages, we decided to create a server which uses a known and simple protocol (HTTP) to allow integration. Using the REST interface provided, it's easy to integrate MFAStack with various applications regardless of their purpose or even the stack they're running on. We don't expose an overly complex REST interface, so the learning curve is very small.

For organisations looking for 2 factor support across multiple applications simultaneously then we offer the SSO edition with a bundled IdP (SimpleSAMLphp).  SimpleSAMLphp requires a service provider (SP) module or plugin to be installed on each application, then the 2 factor process can be abstracted to the IdP only, which then creates secure tokens which each SP consumes.

# Benefits of using MFAStack 
 
MFAStack is intended to be hosted by companies or organisations within their own network or virtual private cloud and does not act as a public cloud or multi-tenancy service.  

Running MFAStack in this way shifts responsibility of OTP validation from 3rd party vendors to your own organisation.  This ensures sensitive data about devices (private keys) is kept private, a requirement for compliance in many industries.

MFAStack provides two simple ways to integrate 2 factor authentication (REST + SAML) and provides easy to use, modern interfaces for administration and configuration.   There is support for both physical devices (Such as Yubikeys) and software (Google authenticator, etc).
 
# Components and software used 

MFAStack consists of two parts - an HTTP frontend and database storage that we call Key Storage Module which stores sensitive information for creating and validating OTPs. 
MFAStack server is written in PHP, using [Lumen](http://lumen.laravel.com) framework.
It supports four different database vendors:

- MySQL
- Postgresql
- SQLite
- MS SQL

# MFALib

MFAStack uses the library written by us, called `MFALib`, which works by providing a pluggable interface and various helper classes, which in turn allows the use of the same PHP API for implementing various OTP-related algorithms.

We're publicly showing the source of this library for auditing purposes, but the library itself operates the same commercial licence as MFAStack.

Both mentioned components are scalable with documentation available on how to scale both vertically and horizontally, along with recommendations for best security practices.

# Supported devices 
 
MFAStack supports Yubikey, Yubikey NEO and Yubikey FIDO U2F Security key. 
Along with Yubikey devices, MFAStack allows you to turn your mobile device or tablet into a second-factor device by means of using [Google Authenticator](https://play.google.com/store/apps/details?id=com.google.android.apps.authenticator2) or [FreeOTP Authenticator](https://play.google.com/store/apps/details?id=org.fedorahosted.freeotp&hl=hr) apps.