# Instafriends

## Terminology

* **Client** – the user who scans the QR code.
* **Host** – the user who provides the QR code.
* **Code**, **Secure Code** – see below.

## Supported Services

* telegram
* vk
* facebook
* instagram
* phone
* email
* ...

## Status Codes

Every response is a JSON blob and contains a status code under key "status".

* success
* ...

## Methods and Data

* type **ServiceData**
    * **service** – service id
    * service-specific
        * vk, instagram, etc...
            * **suid** – service user id
        * phone
        * email

* type **PrivateServiceData**
    * ServiceData
    * vk, instagram, etc...
        * token
    * phone, email – not applicable

* GET **login**
    * input
        * PrivateServiceData
    * output
        * token – Instafriends token
    * desc
    
      **login** returns a new token, the server must recognize the user by one of his services.

* POST **enroll\_service**
    * input
        * PrivateServiceData
    * desc
    
      **enroll\_service** adds services to the database.

* POST **redeem\_code**
    * input
        * code

    * desc
    
      See below for details.

## Code

The code is a JSON blob, transmitted via one of the supported channels. The first one to be implemented is transmission through QR codes. Other attractive possibilities are NFC, Bluetooth and Wi-Fi.

* sec_code
    * services – array of services
    * hmac – proof of authority, see below for crypto params
* services – unsigned service data

## Crypto

* hmac: sha256, 32-byte keys
* password hashing: argon2 (time=3, memory=32*1024, threads=4, keylen=32)
