# Requirements

MFAStack requires a Linux-based server, preferred version is Ubuntu 14.04. 
Please refer to the <a href="/scaling">scaling</a> documentation to read further about how to scale.
Ideal server hardware would be a 2.4 GHZ CPU with 2GB of RAM. 

# Installation

MFAStack installation is performed by cloning the appropriate GIT repository. Currently, we provide several installation repositories:

- A shell script that installs all the software dependencies on to your Linux machine (MySQL, PHP, Nginx and performs database seeding) 
- A PHP script that utilises already existing MySQL, PHP and web server. This assumes you have experience in setting up the dependencies and wish to run MFAStack on your custom configured stack.

Preferred way for Yubiking competition is to install MFAStack by running the shell script on a clean Ubuntu 14.04 OS.

# Installing via shell script

Clone the repository found at our public GITLab and run the script as root: 

```
$ git clone http://yubigit.pub.1app.co.uk/mfastack/server-installation.git
$ cd server-installation
$ sudo su
$ ./install_demo.sh
```

The installation script will:

- install dependencies (PHP, MySQL, Nginx, LDAP) from the official Ubuntu repositories
- create a self-signed SSL certificate
- create a host in the Nginx configuration and add the self-signed SSL certificate to it
- seed the database with the required information
- create internally used random keys used to encrypt sensitive shared information
- provide you with administrative credentials in order to access MFAStack admin panel (a so-called `Identity`)

# Installing via PHP script

This procedure assumes you have configured all the required software and are able to provide additional details that MFAStack requires yourself.

In order for this procedure to work, you must install PHP package manager [Composer](https://getcomposer.org/).

Clone the repository found at our public GITLab:

```
$ cd /var/www
$ http://yubigit.pub.1app.co.uk/mfastack/install.git mfastack
$ cd mfastack
$ composer install
```

After the `composer` is finished, create a file called `.env` in the current directory and provide the following entries: 

```
APP_ENV=local
APP_DEBUG=false
APP_KEY=app_key_goes_here
APP_ONCE_BASE_URL=https://your.mfastack.domain/once/load/

DB_HOST=127.0.0.1
DB_DATABASE=mfastack_db
DB_USERNAME=mfastack_user
DB_PASSWORD=mfastack_password

CACHE_DRIVER=file
SESSION_DRIVER=cookie
SESSION_ENCRYPT=true
SESSION_COOKIE=mfa_session
SESSION_LIFETIME=60
SESSION_EXPIRE_ON_CLOSE=true
QUEUE_DRIVER=sync

MAIL_DRIVER=mailgun
MAIL_HOST=smtp.mailgun.org
MAIL_USERNAME=your_mailgun_username
MAIL_PASSWORD=your_mailgun_password
MAIL_FROM_ADDRESS=your_mail_from_address
MAIL_FROM_NAME="MFAStack Support"

MFA_MAIL_ADMIN_CREATED_SUBJECT=New administrative account created
MFA_MAIL_ADMIN_EDITED_SUBJECT=Administrative account updated

MFA_U2F_APP_ID=https://your.mfastack.domain
MFA_ADMIN_SESSION=mfastack_data
MFA_ADMIN_SESSION_DURATION_COOKIE=mfastack_admin_duration
MFA_LANGUAGE_COOKIE=mfastack_language
MFA_KEY=your_128_bit_key
MFA_SESSION_LIFETIME=10
MFA_PUBLIC_ID_MIN_LEN=1
MFA_PUBLIC_ID_MAX_LEN=16
MFA_PRIVATE_ID_LEN=6
MFA_AES_KEY_LEN=16

MFA_LOG_PATH=logs/mfalogs
MFA_LOG_FILE_NAME=mfa_app_log
MFA_LOG_FILE_EXTENSION=log

SSL_DIGEST_ALG=sha512
SSL_PRIVATE_KEY_BITS=2048
```

The values you have to edit are the following: 

- `APP_KEY` - this is a 32-character secret symmetric key used internally by Lumen framework for encryption purposes 
- `APP_ONCE_BASE_URL` - the domain name and URL path, used for one-time links. One-time links display QR codes that carry information about RFC 4226 and RFC 6238 keys that MFAStack supports. More info about this in feature section.
- `DB_HOST` - database host
- `DB_DATABASE` - the database that stores MFAStack tables
- `DB_USERNAME` - database username
- `DB_PASSWORD` - database password

Next section is about mail. We use mailgun by default, but you can configure this to use your own mail server. More information about configuration is found at [Laravel documentation page](http://laravel.com/docs/5.1/mail).

Final parts are :
- `MFA_U2F_APP_ID` - the AppId that will be used by your MFAStack admin page. This is used to verify U2F based keys when accessing adminstration page
- `MFA_KEY` - a 128 character symmetric key used for encryption purposes by MFAStack


Once you have the mentioned values specified, run the following to seed the database with initial information:

```
$ php artisan migrate
```


