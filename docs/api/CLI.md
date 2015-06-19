# CLI API

Command Line Interface scripts are written in PHP and should be used for recovery purposes.
 
At this point, we've enabled 8 scripts which are related to CRUD operations on `Identities` and handling administrative accounts.

To run the scripts, you must navigate to the directory where MFAStack is installed. 

To see the whole list of supported CLI commands, type `php artisan`.


# Creating an Identity

Currently MFA Stack supports creating only 4 types of identities (out of 5) via CLI interface. 


### Interactive Yubico OTP / Yubikey OATH Identity

To start the interactive interface for registering `Yubico OTP` or `Yubikey-OATH` based identity, use the following:
```php
$ php artisan mfa:register
```

Follow the instructions on-screen in order to complete the registration

### RFC 4226 and RFC 6238 identities

To create any of the above, use:

```php
$ php artisan mfa:rfc4226 "Title" "Title to be shown in Google Authenticator"
```

or, for counter-based algorithm: 

```php
$ php artisan mfa:rfc6238 "Title" "Title to be shown in Google Authenticator"
```

# Creating an administrative account

To create an administrative account use the `php artisan mfadmin:create` command.

Example: 

```php
$ php artisan mfadmin:create "First Name" "email@example.com" "desired password"
```

You can associate identities with the account you're creating by specifying their IDs after the required arguments. 

Example:

```php
$ php artisan mfadmin:create "First Name" "email@example.com" "desired password" 1 2 3 4 5 55 120 550
```

# Associating identities with existing admin account

To associate an identity with existing account, use the `php artisan mfadmin:associate` command.

Example of adding a single identity to the administrative account:

```php
$ php artisan mfadmin:associate 1 1
```

The above will look up the user with ID 1 and associate identity ID 1 with it. 

You can specify multiple identities to be associated with the admin by specifying them in a list. 

Example:

```php
$ php artisan mfadmin:associate 1 1 2 3 4 5 6
```

The above will associate identities 1 - 6 with user ID 1.
