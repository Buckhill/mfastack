# About

Our SSO solution uses:

 - [SimpleSAMLphp](https://simplesamlphp.org/) powered IDP which communicates with MFAStack for OTP verification 
 - [SimpleSAMLphp](https://simplesamlphp.org/) powered SP, an identity management app which communicates with MFAStack for identity creation 

We have created a custom SimpleSAMLphp module which adds a step of verifying an OTP to the authentication process. The identity management app is a small app whose job is to pair users with MFAStack identities. 

The IDP and the identity management app are meant to be used together, since they share a small database - the identity manager writes the (user <-> MFAStack identity) pairs, and the IDP reads them during the authentication process.
