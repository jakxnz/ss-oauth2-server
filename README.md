# OAuth2 Server

## Introduction

This allows your Silverstripe site to be in OAuth 2.0 provider.

Please note that this is under development. It should work just fine, but has not been extensively tested, and is poorly documented.

It supports the following grants:

 * Authorization code grant
 * Refresh grant

## Requirements

 * SilverStripe 3.x

## Installation

Install the add-on with Composer:

```
composer require iansimpson/ss-oauth2-server
```

Next, generate a private/public key pair:

```
openssl genrsa -out private.key 1024
openssl rsa -in private.key -pubout -out public.key
chmod 600 private.key
chmod 600 public.key
```

And put these on your web server, somewhere outside the web root. Add the following lines in your `mysite/_config/config.yml`, updating the privateKey and publicKey to point to the key file (relative to the Silverstripe root), and adding an encryption key (which you might generate with `php -r 'echo base64_encode(random_bytes(32)), PHP_EOL;'`).

```
IanSimpson\OAuth2\OauthServerController:
  privateKey: '../private.key'
  publicKey: '../public.key'
  encryptionKey: ''
```

Finally, after doing a `/dev/build/` go into your site settings and on the OAuth Configuration and add a new Client.

## Usage

Generate a key at `/oauth/authorize`, per the OAuth 2.0 spec (https://tools.ietf.org/html/rfc6749).

Find more information about the server implementation (https://github.com/thephpleague/oauth2-server)

### grant_type

If you are wondering which `grant_type` you should use, read [this helpful guide](https://oauth2.thephpleague.com/authorization-server/which-grant/)

### Permissions within SilverStripe

Once the request is authorised, you can use the below to confirm the member authorised by the request.

```
$member = IanSimpson\OAuth2\OauthServerController::getMember($this); // returns Member or false
```
