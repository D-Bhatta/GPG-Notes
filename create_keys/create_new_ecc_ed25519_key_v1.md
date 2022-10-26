# Create a new ECC key with curve Ed25519

Create a new ECC key with curve Ed25519 and generate subkeys for common tasks.

## Sections

- [Create a new ECC key with curve Ed25519](#create-a-new-ecc-key-with-curve-ed25519)
  - [Sections](#sections)
  - [Generate a new key for certification](#generate-a-new-key-for-certification)
  - [Generate a subkey for signing](#generate-a-subkey-for-signing)
  - [Generate a subkey for encryption](#generate-a-subkey-for-encryption)
  - [Generate a subkey for authentication](#generate-a-subkey-for-authentication)
  - [Generate a revocation certificate](#generate-a-revocation-certificate)
  - [Export public and private keys](#export-public-and-private-keys)
    - [Test exported keys](#test-exported-keys)
  - [Delete primary key](#delete-primary-key)
  - [Create keys only in an encrypted volume](#create-keys-only-in-an-encrypted-volume)
  - [Change password of imported subkeys](#change-password-of-imported-subkeys)

## Generate a new key for certification

- Check the GnuPG version with `gpg --version`.
- Start the key generation process with `gpg --full-generate-key --expert`. The `expert` option will show us more options during the generation process.
- Choose the option for `ECC (set your own capabilities)`. As of version 2.3.8, it is on number 11.
- It should show a bunch of capabilities, most likely `Sign Certify`. These are the capabilities of the key, i.e. the actions it can perform. Toggle off the signing capability (and the authentication capability if present), since we only want a main/primary key to certify other keys. You want to reach the state of `Current allowed actions: Certify`. Press `q` when finished.
- Choose `Curve 25519` as the elliptic curve, a secure option. As of version 2.3.8, this is the default option.
- Keeping keys around for a long time may not, by itself, lead to security incidents. However, making keys expire will help develop practices that do not just depend on key integrity and confidentiality remaining intact forever. Also, key expiry dates can invite periodic security adjustments on the organization. Moreover, regular key rotation ensures leaked keys have a small shelf life before they are rendered unusable.
- Input validity time as `3m`. Review time and confirm when asked.
- Input user information such as name, email, and an optional comment.
- Input a passphrase. Include special characters and numbers to fulfill strong password criteria of GPG.
- See listed keys with `gpg --list-secret-keys --keyid-format=long`.

```powershell
~ via  v16.17.1 on ☁️  (us-east-1) took 16m46s
❯ gpg --version
gpg (GnuPG) 2.3.8
libgcrypt 1.10.1
Copyright (C) 2021 g10 Code GmbH
License GNU GPL-3.0-or-later <https://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Home: C:\Users\user\AppData\Roaming\gnupg
Supported algorithms:
Pubkey: RSA, ELG, DSA, ECDH, ECDSA, EDDSA
Cipher: IDEA, 3DES, CAST5, BLOWFISH, AES, AES192, AES256, TWOFISH,
        CAMELLIA128, CAMELLIA192, CAMELLIA256
AEAD: EAX, OCB
Hash: SHA1, RIPEMD160, SHA256, SHA384, SHA512, SHA224
Compression: Uncompressed, ZIP, ZLIB, BZIP2

~ via  v16.17.1 on ☁️  (us-east-1)
❯ gpg --full-generate-key --expert
gpg (GnuPG) 2.3.8; Copyright (C) 2021 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Please select what kind of key you want:
   (1) RSA and RSA
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
   (7) DSA (set your own capabilities)
   (8) RSA (set your own capabilities)
   (9) ECC (sign and encrypt) *default*
  (10) ECC (sign only)
  (11) ECC (set your own capabilities)
  (13) Existing key
  (14) Existing key from card
Your selection? 11

Possible actions for this ECC key: Sign Certify Authenticate
Current allowed actions: Sign Certify

   (S) Toggle the sign capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? s

Possible actions for this ECC key: Sign Certify Authenticate
Current allowed actions: Certify

   (S) Toggle the sign capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? q
Please select which elliptic curve you want:
   (1) Curve 25519 *default*
   (2) Curve 448
   (3) NIST P-256
   (4) NIST P-384
   (5) NIST P-521
   (6) Brainpool P-256
   (7) Brainpool P-384
   (8) Brainpool P-512
   (9) secp256k1
Your selection? 1
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>y = key expires in n years
Key is valid for? (0) 3m
Key expires at 21-Jan-23 10:06:37 Eastern Standard Time
Is this correct? (y/N) y

GnuPG needs to construct a user ID to identify your key.

Real name: Myusename
Email address: myemail@email.com
Comment:
You selected this USER-ID:
    "Myusename <myemail@email.com>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? o
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
generator a better chance to gain enough entropy.
gpg: revocation certificate stored as 'C:\\Users\\user\\AppData\\Roaming\\gnupg\\openpgp-revocs.d\\B918606497361959CEBA4CF2A3F4629FD281BA40.rev'
public and secret key created and signed.

pub   ed25519 2022-10-23 [C] [expires: 2023-01-21]
      B918606497361959CEBA4CF2A3F4629FD281BA40
uid                      Myusename <myemail@email.com>

~ via  v16.17.1 on ☁️  (us-east-1)
❯ gpg --list-secret-keys --keyid-format=long
C:\Users\user\AppData\Roaming\gnupg\pubring.kbx
---------------------------------------------
sec   rsa1120/65D9AD8DB8063134 2022-10-22 [SC] [expires: 2023-01-20]
      D34ED6F56025EFB6B88C5A8065D9AD8DB8063134
uid                 [ultimate] Myusename <myemail@email.com>

sec   ed25519/A3F4629FD281BA40 2022-10-23 [C] [expires: 2023-01-21]
      B918606497361959CEBA4CF2A3F4629FD281BA40
uid                 [ultimate] Myusename <myemail@email.com>


```

## Generate a subkey for signing

- See listed keys with `gpg --list-secret-keys --keyid-format=long`.
- Start generating a subkey with `gpg --expert --edit-key keyid`, where `keyid` is the ID of the main/primary key. A prompt should start with `gpg>`.
- Type in `addkey` to start.
- Choose the option for `ECC (set your own capabilities)`. As of version 2.3.8, it is on number 11.
- It should show a bunch of capabilities, most likely `Sign`. Toggle off capabilities other than signing, and when you reach the state of `Current allowed actions: Sign`, press `q` to finish.
- Choose `Curve 25519` as the elliptic curve, a secure option. As of version 2.3.8, this is the default option.
- Input validity time as `4w`. Enter main/primary key passphrase. Review time and confirm when asked.
- Input `quit` and save changes. Or input `save` and then quit.

```powershell
~ via  v16.17.1 on ☁️  (us-east-1)
❯ gpg --expert --edit-key A3F4629FD281BA40
gpg (GnuPG) 2.3.8; Copyright (C) 2021 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Secret key is available.

sec  ed25519/A3F4629FD281BA40
     created: 2022-10-23  expires: 2023-01-21  usage: C
     trust: ultimate      validity: ultimate
[ultimate] (1). Myusename <myemail@email.com>

gpg> addkey
Please select what kind of key you want:
   (3) DSA (sign only)
   (4) RSA (sign only)
   (5) Elgamal (encrypt only)
   (6) RSA (encrypt only)
   (7) DSA (set your own capabilities)
   (8) RSA (set your own capabilities)
  (10) ECC (sign only)
  (11) ECC (set your own capabilities)
  (12) ECC (encrypt only)
  (13) Existing key
  (14) Existing key from card
Your selection? 11

Possible actions for this ECC key: Sign Authenticate
Current allowed actions: Sign

   (S) Toggle the sign capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? q
Please select which elliptic curve you want:
   (1) Curve 25519 *default*
   (2) Curve 448
   (3) NIST P-256
   (4) NIST P-384
   (5) NIST P-521
   (6) Brainpool P-256
   (7) Brainpool P-384
   (8) Brainpool P-512
   (9) secp256k1
Your selection? 1
Please specify how long the key should be valid.
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 4w
Key expires at 20-Nov-22 10:30:01 Eastern Standard Time
Is this correct? (y/N) y
Really create? (y/N) y
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.

     created: 2022-10-23  expires: 2023-01-21  usage: C
     trust: ultimate      validity: ultimate
ssb  ed25519/38702E1FE87741CB
     created: 2022-10-23  expires: 2022-11-20  usage: S
[ultimate] (1). Myusename <myemail@email.com>

gpg> quit
Save changes? (y/N) y

~ via  v16.17.1 on ☁️  (us-east-1) took 8m6s
❯ gpg --list-secret-keys --keyid-format=long
C:\Users\user\AppData\Roaming\gnupg\pubring.kbx
---------------------------------------------
sec   rsa1120/65D9AD8DB8063134 2022-10-22 [SC] [expires: 2023-01-20]
      D34ED6F56025EFB6B88C5A8065D9AD8DB8063134
uid                 [ultimate] Myusename <myemail@email.com>

sec   ed25519/A3F4629FD281BA40 2022-10-23 [C] [expires: 2023-01-21]
      B918606497361959CEBA4CF2A3F4629FD281BA40
uid                 [ultimate] Myusename <myemail@email.com>
ssb   ed25519/38702E1FE87741CB 2022-10-23 [S] [expires: 2022-11-20]


```

## Generate a subkey for encryption

- See listed keys with `gpg --list-secret-keys --keyid-format=long`.
- Start generating a subkey with `gpg --expert --edit-key keyid`, where `keyid` is the ID of the main/primary key. A prompt should start with `gpg>`.
- Type in `addkey` to start.
- Choose the option for `ECC (encrypt only)`. As of version 2.3.8, it is on number 12.
- Choose `Curve 25519` as the elliptic curve, a secure option. As of version 2.3.8, this is the default option.
- Input validity time as `4w`. Enter main/primary key passphrase. Review time and confirm when asked.
- Input `quit` and save changes. Or input `save` and then quit.

```powershell
~ via  v16.17.1 on ☁️  (us-east-1)
❯ gpg --expert --edit-key A3F4629FD281BA40
gpg (GnuPG) 2.3.8; Copyright (C) 2021 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Secret key is available.

sec  ed25519/A3F4629FD281BA40
     created: 2022-10-23  expires: 2023-01-21  usage: C
     trust: ultimate      validity: ultimate
ssb  ed25519/38702E1FE87741CB
     created: 2022-10-23  expires: 2022-11-20  usage: S
[ultimate] (1). Myusename <myemail@email.com>

gpg> addkey
Please select what kind of key you want:
   (3) DSA (sign only)
   (4) RSA (sign only)
   (5) Elgamal (encrypt only)
   (6) RSA (encrypt only)
   (7) DSA (set your own capabilities)
   (8) RSA (set your own capabilities)
  (10) ECC (sign only)
  (11) ECC (set your own capabilities)
  (12) ECC (encrypt only)
  (13) Existing key
  (14) Existing key from card
Your selection? 12
Please select which elliptic curve you want:
   (1) Curve 25519 *default*
   (2) Curve 448
   (3) NIST P-256
   (4) NIST P-384
   (5) NIST P-521
   (6) Brainpool P-256
   (7) Brainpool P-384
   (8) Brainpool P-512
Your selection? 1
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 4w
Key expires at 20-Nov-22 10:45:35 Eastern Standard Time
Is this correct? (y/N) y
Really create? (y/N) y
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number

sec  ed25519/A3F4629FD281BA40
     created: 2022-10-23  expires: 2023-01-21  usage: C
     trust: ultimate      validity: ultimate
ssb  ed25519/38702E1FE87741CB
     created: 2022-10-23  expires: 2022-11-20  usage: S
ssb  cv25519/887FF28F2E8241B1
     created: 2022-10-23  expires: 2022-11-20  usage: E
[ultimate] (1). Myusename <myemail@email.com>

gpg> save

~ via  v16.17.1 on ☁️  (us-east-1) took 2m4s
❯ gpg --list-secret-keys --keyid-format=long
C:\Users\user\AppData\Roaming\gnupg\pubring.kbx
---------------------------------------------
sec   rsa1120/65D9AD8DB8063134 2022-10-22 [SC] [expires: 2023-01-20]
      D34ED6F56025EFB6B88C5A8065D9AD8DB8063134
uid                 [ultimate] Myusename <myemail@email.com>

sec   ed25519/A3F4629FD281BA40 2022-10-23 [C] [expires: 2023-01-21]
      B918606497361959CEBA4CF2A3F4629FD281BA40
uid                 [ultimate] Myusename <myemail@email.com>
ssb   ed25519/38702E1FE87741CB 2022-10-23 [S] [expires: 2022-11-20]
ssb   cv25519/887FF28F2E8241B1 2022-10-23 [E] [expires: 2022-11-20]


```

## Generate a subkey for authentication

- See listed keys with `gpg --list-secret-keys --keyid-format=long`.
- Start generating a subkey with `gpg --expert --edit-key keyid`, where `keyid` is the ID of the main/primary key. A prompt should start with `gpg>`.
- Type in `addkey` to start.
- Choose the option for `ECC (set your own capabilities)`. As of version 2.3.8, it is on number 11.
- It should show a bunch of capabilities, most likely `Sign`. Toggle off capabilities other than authentication, and when you reach the state of `Current allowed actions: Authenticate`, press `q` to finish.
- Choose `Curve 25519` as the elliptic curve, a secure option. As of version 2.3.8, this is the default option.
- Input validity time as `4w`. Enter main/primary key passphrase. Review time and confirm when asked.
- Input `quit` and save changes. Or input `save` and then quit.

```powershell
~ via  v16.17.1 on ☁️  (us-east-1)
❯ gpg --expert --edit-key A3F4629FD281BA40
gpg (GnuPG) 2.3.8; Copyright (C) 2021 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Secret key is available.

sec  ed25519/A3F4629FD281BA40
     created: 2022-10-23  expires: 2023-01-21  usage: C
     trust: ultimate      validity: ultimate
ssb  ed25519/38702E1FE87741CB
     created: 2022-10-23  expires: 2022-11-20  usage: S
ssb  cv25519/887FF28F2E8241B1
     created: 2022-10-23  expires: 2022-11-20  usage: E
[ultimate] (1). Myusename <myemail@email.com>

gpg> addkey
Please select what kind of key you want:
   (3) DSA (sign only)
   (4) RSA (sign only)
   (5) Elgamal (encrypt only)
   (6) RSA (encrypt only)
   (7) DSA (set your own capabilities)
   (8) RSA (set your own capabilities)
  (10) ECC (sign only)
  (11) ECC (set your own capabilities)
  (12) ECC (encrypt only)
  (13) Existing key
  (14) Existing key from card
Your selection? 11

Possible actions for this ECC key: Sign Authenticate
Current allowed actions: Sign

   (S) Toggle the sign capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? a

Possible actions for this ECC key: Sign Authenticate
Current allowed actions: Sign Authenticate

   (S) Toggle the sign capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? s

Possible actions for this ECC key: Sign Authenticate
Current allowed actions: Authenticate

   (S) Toggle the sign capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? q
Please select which elliptic curve you want:
   (1) Curve 25519 *default*
   (2) Curve 448
   (3) NIST P-256
   (4) NIST P-384
   (5) NIST P-521
   (6) Brainpool P-256
   (7) Brainpool P-384
   (8) Brainpool P-512
   (9) secp256k1
Your selection? 1
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 4w
Key expires at 20-Nov-22 10:50:47 Eastern Standard Time
Is this correct? (y/N) y
Really create? (y/N) y
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.

sec  ed25519/A3F4629FD281BA40
     trust: ultimate      validity: ultimate
ssb  ed25519/38702E1FE87741CB
     created: 2022-10-23  expires: 2022-11-20  usage: S
ssb  cv25519/887FF28F2E8241B1
     created: 2022-10-23  expires: 2022-11-20  usage: E
ssb  ed25519/E4EC51E72831E79E
     created: 2022-10-23  expires: 2022-11-20  usage: A
[ultimate] (1). Myusename <myemail@email.com>

gpg> save

~ via  v16.17.1 on ☁️  (us-east-1) took 3m7s
❯ gpg --list-secret-keys --keyid-format=long
C:\Users\user\AppData\Roaming\gnupg\pubring.kbx
---------------------------------------------
sec   rsa1120/65D9AD8DB8063134 2022-10-22 [SC] [expires: 2023-01-20]
      D34ED6F56025EFB6B88C5A8065D9AD8DB8063134
uid                 [ultimate] Myusename <myemail@email.com>

sec   ed25519/A3F4629FD281BA40 2022-10-23 [C] [expires: 2023-01-21]
      B918606497361959CEBA4CF2A3F4629FD281BA40
uid                 [ultimate] Myusename <myemail@email.com>
ssb   ed25519/38702E1FE87741CB 2022-10-23 [S] [expires: 2022-11-20]
ssb   cv25519/887FF28F2E8241B1 2022-10-23 [E] [expires: 2022-11-20]
ssb   ed25519/E4EC51E72831E79E 2022-10-23 [A] [expires: 2022-11-20]

```

## Generate a revocation certificate

- This certificate can be used to render your primary key and all subkeys unusable. It should be stored in a secure location. A good idea is to change directory into an encrypted volume, such as a VeraCrypt volume, and generate the certificate there.
- If the certificate is leaked, then the damage that can be done is minimized. The key can only be stopped, it cannot be misused.
- See keys with `gpg --list-secret-keys --keyid-format=long`.
- Generate a revocation certificate with `gpg --output filename.asc --gen-revoke keyid`. It can be convenient to set the filename as `GPG-keyid.asc`. Where `keyid` is the ID of the key.

```powershell
O:\ on ☁️  (us-east-1)
❯ gpg --list-secret-keys --keyid-format=long
C:\Users\user\AppData\Roaming\gnupg\pubring.kbx
---------------------------------------------
pub   rsa1120/65D9AD8DB8063134 2022-10-22 [SC] [expires: 2023-01-20]
      D34ED6F56025EFB6B88C5A8065D9AD8DB8063134
uid                 [ultimate] Myusename <myemail@email.com>

pub   ed25519/A3F4629FD281BA40 2022-10-23 [C] [expires: 2023-01-21]
      B918606497361959CEBA4CF2A3F4629FD281BA40
uid                 [ultimate] Myusename <myemail@email.com>
sub   ed25519/38702E1FE87741CB 2022-10-23 [S] [expires: 2022-11-20]
sub   cv25519/887FF28F2E8241B1 2022-10-23 [E] [expires: 2022-11-20]
sub   ed25519/E4EC51E72831E79E 2022-10-23 [A] [expires: 2022-11-20]


❯ gpg --output GPG-A3F4629FD281BA40.asc --gen-revoke A3F4629FD281BA40

sec  ed25519/A3F4629FD281BA40 2022-10-23 Myusename <myemail@email.com>

Create a revocation certificate for this key? (y/N) y
Please select the reason for the revocation:
  0 = No reason specified
  1 = Key has been compromised
  2 = Key is superseded
  3 = Key is no longer used
  Q = Cancel
(Probably you want to select 1 here)
Your decision? 0
Enter an optional description; end it with an empty line:
>
(No description given)
Is this okay? (y/N) y
Revocation certificate created.

access to this certificate he can use it to make your key unusable.
It is smart to print this certificate and store it away, just in case
your media become unreadable.  But have some caution:  The print system of
your machine might store the data and make it available to others!

```

## Export public and private keys

- If private keys are leaked, then it will be considered a critical compromise. It should be stored in a secure location. A good idea is to change directory into an encrypted volume, such as a VeraCrypt volume, and export the keys there.
- See keys with `gpg --list-secret-keys --keyid-format=long`.
- Export a primary public key and all subkeys in ASCII-armor format with `gpg --output key_filename.asc --export --armor identifier`, where identifier is used to select a set of keys. It can be username, email, keyid, etc. It can be convenient to set the filename as `public-keyid.asc`. Where `keyid` is the ID of the key.
- Export a primary private key and all subkeys in ASCII-armor format with `gpg --output key_filename.asc --export-secret-key --armor identifier`, where identifier is used to select a set of keys. It can be username, email, keyid, etc. It can be convenient to set the filename as `private-keyid.asc`. Where `keyid` is the ID of the key.
- Export a primary public key and all subkeys in GPG binary format with `gpg --output key_filename.gpg --export identifier`, where identifier is used to select a set of keys. It can be username, email, keyid, etc. It can be convenient to set the filename as `public-keyid.GPG`. Where `keyid` is the ID of the key.
- Export a primary private key and all subkeys in GPG binary format with `gpg --output key_filename.gpg --export-secret-key identifier`, where identifier is used to select a set of keys. It can be username, email, keyid, etc. It can be convenient to set the filename as `private-keyid.gpg`. Where `keyid` is the ID of the key.
- Export a public subkey in ASCII-armor format with `gpg --output key_filename.asc --export --armor keyid!`. It can be convenient to set the filename as `public-subkey-keyid.asc`. Where `keyid` is the ID of the key.
- Export a private subkey in ASCII-armor format with `gpg --output key_filename.asc --export-secret-subkeys --armor keyid!`. It can be convenient to set the filename as `private-subkey-keyid.asc`. Where `keyid` is the ID of the key.
- Export a public subkey in GPG binary format with `gpg --output key_filename.gpg --export keyid!`. It can be convenient to set the filename as `public-subkey-keyid.gpg`. Where `keyid` is the ID of the key.
- Export a private subkey in GPG binary format with `gpg --output key_filename.gpg --export-secret-subkeys keyid!`. It can be convenient to set the filename as `private-subkey-keyid.gpg`. Where `keyid` is the ID of the key.
- Output owner trust with `gpg --export-ownertrust`. Copy and paste the output in a file `otrust.txt`. Add a newline at the end of this file.

```powershell
O:\ on ☁️  (us-east-1) took 1m43s
❯ gpg --list-secret-keys --keyid-format=long
C:\Users\user\AppData\Roaming\gnupg\pubring.kbx
---------------------------------------------
pub   rsa1120/65D9AD8DB8063134 2022-10-22 [SC] [expires: 2023-01-20]
      D34ED6F56025EFB6B88C5A8065D9AD8DB8063134
uid                 [ultimate] Myusename <myemail@email.com>

pub   ed25519/A3F4629FD281BA40 2022-10-23 [C] [expires: 2023-01-21]
      B918606497361959CEBA4CF2A3F4629FD281BA40
uid                 [ultimate] Myusename <myemail@email.com>
sub   ed25519/38702E1FE87741CB 2022-10-23 [S] [expires: 2022-11-20]
sub   cv25519/887FF28F2E8241B1 2022-10-23 [E] [expires: 2022-11-20]
sub   ed25519/E4EC51E72831E79E 2022-10-23 [A] [expires: 2022-11-20]

O:\ on ☁️  (us-east-1)
❯ gpg --output public-A3F4629FD281BA40.asc --export --armor A3F4629FD281BA40

O:\ on ☁️  (us-east-1)
❯ gpg --output private-A3F4629FD281BA40.asc --export-secret-key --armor A3F4629FD281BA40

O:\ on ☁️  (us-east-1) took 6s
❯ gpg --output public-A3F4629FD281BA40.gpg --export A3F4629FD281BA40

O:\ on ☁️  (us-east-1)
❯ gpg --output private-A3F4629FD281BA40.gpg --export-secret-key A3F4629FD281BA40

O:\ on ☁️  (us-east-1) took 4s
❯ gpg --output public-subkey-38702E1FE87741CB.asc --export --armor 38702E1FE87741CB!

O:\ on ☁️  (us-east-1)
❯ gpg --output private-subkey-38702E1FE87741CB.asc --export-secret-subkeys --armor 38702E1FE87741CB!

O:\ on ☁️  (us-east-1) took 6s
❯ gpg --output public-subkey-38702E1FE87741CB.gpg --export 38702E1FE87741CB!

O:\ on ☁️  (us-east-1)
❯ gpg --output private-subkey-38702E1FE87741CB.gpg --export-secret-subkeys 38702E1FE87741CB!

O:\ on ☁️  (us-east-1) took 2s
❯ gpg --output public-subkey-887FF28F2E8241B1.asc --export --armor 887FF28F2E8241B1!

O:\ on ☁️  (us-east-1)
❯ gpg --output private-subkey-887FF28F2E8241B1.asc --export-secret-subkeys --armor 887FF28F2E8241B1!

O:\ on ☁️  (us-east-1) took 4s
❯ gpg --output public-subkey-887FF28F2E8241B1.gpg --export 887FF28F2E8241B1!

O:\ on ☁️  (us-east-1)
❯ gpg --output private-subkey-887FF28F2E8241B1.gpg --export-secret-subkeys 887FF28F2E8241B1!

O:\ on ☁️  (us-east-1)
❯ gpg --output public-subkey-E4EC51E72831E79E.asc --export --armor E4EC51E72831E79E!

O:\ on ☁️  (us-east-1)
❯ gpg --output private-subkey-E4EC51E72831E79E.asc --export-secret-subkeys --armor E4EC51E72831E79E!

O:\ on ☁️  (us-east-1) took 5s
❯ gpg --output public-subkey-E4EC51E72831E79E.gpg --export E4EC51E72831E79E!

O:\ on ☁️  (us-east-1)
❯ gpg --output private-subkey-E4EC51E72831E79E.gpg --export-secret-subkeys E4EC51E72831E79E!

O:\ on ☁️  (us-east-1) took 2s
❯ gpg --export-ownertrust
# List of assigned trustvalues, created 25-Oct-22 09:36:33 Eastern Daylight Time
# (Use "gpg --import-ownertrust" to restore them)
D34ED6F56025EFB6B88C5A8065D9AD8DB8063134:6:
B918606497361959CEBA4CF2A3F4629FD281BA40:6:

O:\ on ☁️  (us-east-1)
❯ ls

    Directory: O:\


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         24-Oct-22     08:00            296 GPG-A3F4629FD281BA40.asc
-a----         25-Oct-22     09:29           1333 public-A3F4629FD281BA40.asc
-a----         25-Oct-22     09:29           1793 private-A3F4629FD281BA40.asc
-a----         25-Oct-22     09:29            909 public-A3F4629FD281BA40.gpg
-a----         25-Oct-22     09:30           1241 private-A3F4629FD281BA40.gpg
-a----         25-Oct-22     09:32            831 public-subkey-38702E1FE87741CB.asc
-a----         25-Oct-22     09:32            542 public-subkey-38702E1FE87741CB.gpg
-a----         25-Oct-22     09:33            633 private-subkey-38702E1FE87741CB.gpg
-a----         25-Oct-22     09:34            673 public-subkey-887FF28F2E8241B1.asc
-a----         25-Oct-22     09:34            799 private-subkey-887FF28F2E8241B1.asc
-a----         25-Oct-22     09:34            428 public-subkey-887FF28F2E8241B1.gpg
-a----         25-Oct-22     09:35            665 public-subkey-E4EC51E72831E79E.asc
-a----         25-Oct-22     09:35            795 private-subkey-E4EC51E72831E79E.asc
-a----         25-Oct-22     09:35            423 public-subkey-E4EC51E72831E79E.gpg
-a----         25-Oct-22     09:35            514 private-subkey-E4EC51E72831E79E.gpg
-a----         25-Oct-22     09:37            223 otrust.txt



```

### Test exported keys

- It is good practice to check if a backup worked by restoring it. A backup isn't real until it has been tested.
- We do this by creating a new temporary home directory in another location. A good idea is to change directory into an encrypted volume, such as a VeraCrypt volume.
- Create a new directory with `mkdir temp_directory`, where `temp_directory` is the name of the directory.
- See keys with `gpg --homedir temp_directory --list-secret-keys --keyid-format=long`. This will also initialize a home directory.
- List available keyfiles. Import a keyfile with `gpg --homedir temp_directory --import filename.extension`.
- Import owner trust with `gpg --homedir temp_directory --import-ownertrust otrust.txt`.
- Create a file called `encr.txt` with some text in it. Encrypt with `gpg --homedir temp_directory --output doc.gpg --encrypt --recipient keyid encr.txt` and decrypt with `gpg --homedir temp_directory --output doc.txt --decrypt doc.gpg`.
- Sign a file with `gpg --homedir gnupg_temp --output doc.sig --sign doc.txt` and verify signature with `gpg --homedir gnupg_temp --output doc2.txt --decrypt doc.sig`.
- You can delete the temporary directory once you are done.

```powershell

O:\ on ☁️  (us-east-1)
❯ mkdir .gnupg_temp


    Directory: O:\


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         25-Oct-22     09:40                .gnupg_temp


O:\ on ☁️  (us-east-1)
❯ gpg --homedir .gnupg_temp --list-secret-keys --keyid-format=long
gpg: keybox 'O://.gnupg_temp/pubring.kbx' created
gpg: O://.gnupg_temp/trustdb.gpg: trustdb created

O:\ on ☁️  (us-east-1)
❯ gpg --homedir .gnupg_temp --import private-subkey-38702E1FE87741CB.gpg private-subkey-887FF28F2E8241B1.gpg private-subkey-E4EC51E72831E79E.gpg
gpg: key A3F4629FD281BA40: public key "Myusename <myemail@email.com>" imported
gpg: To migrate 'secring.gpg', with each smartcard, run: gpg --card-status
gpg: key A3F4629FD281BA40: secret key imported
gpg: key A3F4629FD281BA40: "Myusename <myemail@email.com>" 1 new signature
gpg: key A3F4629FD281BA40: "Myusename <myemail@email.com>" 1 new subkey
gpg: warning: lower 3 bits of the secret key are not cleared
gpg: To migrate 'secring.gpg', with each smartcard, run: gpg --card-status
gpg: key A3F4629FD281BA40: secret key imported
gpg: key A3F4629FD281BA40: "Myusename <myemail@email.com>" 1 new signature
gpg: key A3F4629FD281BA40: "Myusename <myemail@email.com>" 1 new subkey
gpg: To migrate 'secring.gpg', with each smartcard, run: gpg --card-status
gpg: key A3F4629FD281BA40: secret key imported
gpg: Total number processed: 3
gpg:               imported: 1
gpg:            new subkeys: 2
gpg:         new signatures: 2
gpg:       secret keys read: 3
gpg:   secret keys imported: 3

O:\ on ☁️  (us-east-1) took 7s
❯ gpg --homedir .gnupg_temp --list-secret-keys --keyid-format=long
O://.gnupg_temp/pubring.kbx
---------------------------
sec#  ed25519/A3F4629FD281BA40 2022-10-23 [C] [expires: 2023-01-21]
      B918606497361959CEBA4CF2A3F4629FD281BA40
uid                 [ unknown] Myusename <myemail@email.com>
ssb   cv25519/887FF28F2E8241B1 2022-10-23 [E] [expires: 2022-11-20]
ssb   ed25519/E4EC51E72831E79E 2022-10-23 [A] [expires: 2022-11-20]


❯ gpg --homedir .gnupg_temp --import private-A3F4629FD281BA40.gpg
gpg: key A3F4629FD281BA40: "Myusename <myemail@email.com>" not changed
gpg: warning: lower 3 bits of the secret key are not cleared
gpg: key A3F4629FD281BA40: secret key imported
gpg: Total number processed: 1
gpg:              unchanged: 1
gpg:       secret keys read: 1
gpg:   secret keys imported: 1
gpg:  secret keys unchanged: 1

O:\ on ☁️  (us-east-1) took 3s
❯ gpg --homedir .gnupg_temp --list-secret-keys --keyid-format=long
O://.gnupg_temp/pubring.kbx
---------------------------
sec   ed25519/A3F4629FD281BA40 2022-10-23 [C] [expires: 2023-01-21]
      B918606497361959CEBA4CF2A3F4629FD281BA40
uid                 [ unknown] Myusename <myemail@email.com>
ssb   ed25519/38702E1FE87741CB 2022-10-23 [S] [expires: 2022-11-20]
ssb   cv25519/887FF28F2E8241B1 2022-10-23 [E] [expires: 2022-11-20]
ssb   ed25519/E4EC51E72831E79E 2022-10-23 [A] [expires: 2022-11-20]


O:\ on ☁️  (us-east-1)
❯ gpg --homedir .gnupg_temp --import-ownertrust otrust.txt
gpg: inserting ownertrust of 6
gpg: inserting ownertrust of 6

O:\ on ☁️  (us-east-1)
❯ gpg --homedir .gnupg_temp --list-secret-keys --keyid-format=long
gpg: checking the trustdb
gpg: public key of ultimately trusted key 65D9AD8DB8063134 not found
gpg: marginals needed: 3  completes needed: 1  trust model: pgp
gpg: depth: 0  valid:   2  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 2u
gpg: next trustdb check due at 2023-01-21
O://.gnupg_temp/pubring.kbx
---------------------------
sec   ed25519/A3F4629FD281BA40 2022-10-23 [C] [expires: 2023-01-21]
      B918606497361959CEBA4CF2A3F4629FD281BA40
uid                 [ultimate] Myusename <myemail@email.com>
ssb   ed25519/38702E1FE87741CB 2022-10-23 [S] [expires: 2022-11-20]
ssb   cv25519/887FF28F2E8241B1 2022-10-23 [E] [expires: 2022-11-20]
ssb   ed25519/E4EC51E72831E79E 2022-10-23 [A] [expires: 2022-11-20]

O:\ on ☁️  (us-east-1) took 16s
❯ gpg --homedir gnupg_temp --list-secret-keys --keyid-format=long
O://gnupg_temp/pubring.kbx
--------------------------
sec#  ed25519/1370ECA23597756E 2022-10-25 [C] [expires: 2023-01-23]
      E74FA07E10EA215E57C0C64F1370ECA23597756E
uid                 [ unknown] Myusename <myemail@email.com>
ssb   ed25519/569B8979433D2BBC 2022-10-25 [S] [expires: 2022-11-22]
ssb   cv25519/AD92D392B67FA40E 2022-10-25 [E] [expires: 2022-11-22]
ssb   ed25519/53C60F309AE9ECCF 2022-10-25 [A] [expires: 2022-11-22]


O:\ on ☁️  (us-east-1)
❯ gpg --homedir gnupg_temp --output doc.gpg --encrypt --recipient AD92D392B67FA40E encr.txt
gpg: AD92D392B67FA40E: There is no assurance this key belongs to the named user

sub  cv25519/AD92D392B67FA40E 2022-10-25 Myusename <myemail@email.com>
 Primary key fingerprint: E74F A07E 10EA 215E 57C0  C64F 1370 ECA2 3597 756E
      Subkey fingerprint: 2FFA 1098 589B 17E1 2975  EC6F AD92 D392 B67F A40E
It is NOT certain that the key belongs to the person named
in the user ID.  If you *really* know what you are doing,
you may answer the next question with yes.

Use this key anyway? (y/N) y

O:\ on ☁️  (us-east-1) took 3s
❯ gpg --homedir gnupg_temp --output doc.txt --decrypt doc.gpg
gpg: encrypted with cv25519 key, ID AD92D392B67FA40E, created 2022-10-25
      "Myusename <myemail@email.com>"

O:\ on ☁️  (us-east-1) took 20s
❯ gpg --homedir gnupg_temp --output doc.sig --sign doc.txt

O:\ on ☁️  (us-east-1)
❯ gpg --homedir gnupg_temp --output doc2.txt --decrypt doc.sig
gpg: Signature made 26-Oct-22 03:54:51 Eastern Daylight Time
gpg:                using EDDSA key 8013D32F252DD5043B3B3297569B8979433D2BBC
gpg: Good signature from "Myusename <myemail@email.com>" [unknown]
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Primary key fingerprint: E74F A07E 10EA 215E 57C0  C64F 1370 ECA2 3597 756E
     Subkey fingerprint: 8013 D32F 252D D504 3B3B  3297 569B 8979 433D 2BBC
```

## Delete primary key

- Once the primary key has been backed up, it is a good practice to delete it.
- See keys with and keygrips with `gpg --list-secret-keys --with-keygrip`.
- Delete the key with `gpg-connect-agent "DELETE_KEY keygrip" /bye`, where `keygrip` is the keygrip of the primary key.

```powershell
O:\ on ☁️  (us-east-1)
❯ gpg --list-secret-keys --with-keygrip
C:\Users\user\AppData\Roaming\gnupg\pubring.kbx
---------------------------------------------
sec   rsa1120 2022-10-22 [SC] [expires: 2023-01-20]
      D34ED6F56025EFB6B88C5A8065D9AD8DB8063134
      Keygrip = 70B4B49BE50D13CDB712F982DE2A9D9F4AEC997E
uid           [ultimate] Myusename <myemail@email.com>

sec   ed25519 2022-10-23 [C] [expires: 2023-01-21]
      B918606497361959CEBA4CF2A3F4629FD281BA40
      Keygrip = ECA6434A8351C9EDCAD348236951594FB761FBEB
uid           [ultimate] Myusename <myemail@email.com>
ssb   ed25519 2022-10-23 [S] [expires: 2022-11-20]
      Keygrip = 2004A153D22E02B147C21037E0CA3C38B8B3822C
ssb   cv25519 2022-10-23 [E] [expires: 2022-11-20]
      Keygrip = 611FC48A6AD602239A801CEF15E88403D7BA5929
ssb   ed25519 2022-10-23 [A] [expires: 2022-11-20]
      Keygrip = 64999E320027D657E60284540D316D32B8871A61


O:\ on ☁️  (us-east-1)
❯ gpg-connect-agent "DELETE_KEY ECA6434A8351C9EDCAD348236951594FB761FBEB" /bye
OK

O:\ on ☁️  (us-east-1)
❯ gpg --list-secret-keys --keyid-format=long
C:\Users\user\AppData\Roaming\gnupg\pubring.kbx
---------------------------------------------
sec   rsa1120/65D9AD8DB8063134 2022-10-22 [SC] [expires: 2023-01-20]
      D34ED6F56025EFB6B88C5A8065D9AD8DB8063134
uid                 [ultimate] Myusename <myemail@email.com>

sec#  ed25519/A3F4629FD281BA40 2022-10-23 [C] [expires: 2023-01-21]
      B918606497361959CEBA4CF2A3F4629FD281BA40
uid                 [ultimate] Myusename <myemail@email.com>
ssb   ed25519/38702E1FE87741CB 2022-10-23 [S] [expires: 2022-11-20]
ssb   cv25519/887FF28F2E8241B1 2022-10-23 [E] [expires: 2022-11-20]
ssb   ed25519/E4EC51E72831E79E 2022-10-23 [A] [expires: 2022-11-20]

```

## Create keys only in an encrypted volume

- Using the above it is possible to create keys in an encrypted volume, and only import subkeys for general use, making sure the primary key never touches unencrypted disk.
- Create a temporary home directory.
- Use it generate a primary key, and then sub keys.
- Export the primary private key, the owner trust, and the private subkeys. Delete the temporary home directory.
- Test the backups. Also, make sure to cleanup any temporary home directories.
- Import subkeys only into default home directory.

```powershell
mkdir temp_directory
gpg --homedir temp_directory --list-secret-keys --keyid-format=long
gpg --homedir temp_directory --full-generate-key --expert
gpg --homedir temp_directory --expert --edit-key keyid
gpg --homedir temp_directory --output GPG-keyid.asc --gen-revoke keyid
gpg --homedir temp_directory --output private-keyid.gpg --export-secret-key identifier
gpg --homedir temp_directory --output private-subkey-keyid.gpg --export-secret-subkeys keyid!
gpg --homedir temp_directory --export-ownertrust
gpg --homedir temp_directory --import filename.extension
gpg --homedir temp_directory --import-ownertrust otrust.txt
gpg --homedir temp_directory --output doc.gpg --encrypt --recipient keyid encr.txt
gpg --homedir temp_directory --output doc.txt --decrypt doc.gpg
gpg --homedir temp_directory --output doc.sig --sign doc.txt
gpg --homedir temp_directory --output doc2.txt --decrypt doc.sig
gpg --import filename.extension
```

```powershell
O:\ on ☁️  (us-east-1)
❯ mkdir gnupg_temp


    Directory: O:\


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         25-Oct-22     12:57                gnupg_temp



O:\ on ☁️  (us-east-1)
❯ gpg --homedir gnupg_temp --list-secret-keys --keyid-format=long
gpg: keybox 'O://gnupg_temp/pubring.kbx' created
gpg: O://gnupg_temp/trustdb.gpg: trustdb created

O:\ on ☁️  (us-east-1)
❯ gpg --homedir gnupg_temp --full-generate-key --expert
gpg (GnuPG) 2.3.8; Copyright (C) 2021 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Please select what kind of key you want:
   (1) RSA and RSA
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
   (7) DSA (set your own capabilities)
   (8) RSA (set your own capabilities)
   (9) ECC (sign and encrypt) *default*
  (10) ECC (sign only)
  (11) ECC (set your own capabilities)
  (13) Existing key
  (14) Existing key from card
Your selection? 11

Possible actions for this ECC key: Sign Certify Authenticate
Current allowed actions: Sign Certify

   (S) Toggle the sign capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? s

Possible actions for this ECC key: Sign Certify Authenticate
Current allowed actions: Certify

   (S) Toggle the sign capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? q
Please select which elliptic curve you want:
   (1) Curve 25519 *default*
   (2) Curve 448
   (3) NIST P-256
   (4) NIST P-384
   (5) NIST P-521
   (6) Brainpool P-256
   (7) Brainpool P-384
   (8) Brainpool P-512
   (9) secp256k1
Your selection? 1
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>y = key expires in n years
Key is valid for? (0) 3m
Key expires at 23-Jan-23 11:58:00 Eastern Standard Time
Is this correct? (y/N) y

GnuPG needs to construct a user ID to identify your key.

Real name: Myusename
Email address: myemail@email.com
Comment:
You selected this USER-ID:
    "Myusename <myemail@email.com>"
Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? o
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
gpg: directory 'O://gnupg_temp/openpgp-revocs.d' created
gpg: revocation certificate stored as 'O://gnupg_temp/openpgp-revocs.d\\CC986EBC4A13718DE554B643BA89F2A837D441FC.rev'
public and secret key created and signed.

pub   ed25519 2022-10-25 [C] [expires: 2023-01-23]
      CC986EBC4A13718DE554B643BA89F2A837D441FC
uid                      Myusename <myemail@email.com>


O:\ on ☁️  (us-east-1) took 46s
❯ gpg --homedir gnupg_temp --list-secret-keys --keyid-format=long
gpg: checking the trustdb
gpg: marginals needed: 3  completes needed: 1  trust model: pgp
gpg: depth: 0  valid:   1  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 1u
gpg: next trustdb check due at 2023-01-23
O://gnupg_temp/pubring.kbx
--------------------------
sec   ed25519/BA89F2A837D441FC 2022-10-25 [C] [expires: 2023-01-23]
      CC986EBC4A13718DE554B643BA89F2A837D441FC
uid                 [ultimate] Myusename <myemail@email.com>


O:\ on ☁️  (us-east-1)
❯ gpg --homedir gnupg_temp --expert --edit-key BA89F2A837D441FC
gpg (GnuPG) 2.3.8; Copyright (C) 2021 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Secret key is available.

sec  ed25519/BA89F2A837D441FC
     created: 2022-10-25  expires: 2023-01-23  usage: C
     trust: ultimate      validity: ultimate
[ultimate] (1). Myusename <myemail@email.com>

gpg> addkey
Please select what kind of key you want:
   (3) DSA (sign only)
   (4) RSA (sign only)
   (5) Elgamal (encrypt only)
   (6) RSA (encrypt only)
   (7) DSA (set your own capabilities)
   (8) RSA (set your own capabilities)
  (10) ECC (sign only)
  (11) ECC (set your own capabilities)
  (12) ECC (encrypt only)
  (13) Existing key
  (14) Existing key from card
Your selection? 11

Possible actions for this ECC key: Sign Authenticate
Current allowed actions: Sign

   (S) Toggle the sign capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? q
Please select which elliptic curve you want:
   (1) Curve 25519 *default*
   (2) Curve 448
   (3) NIST P-256
   (4) NIST P-384
   (5) NIST P-521
   (6) Brainpool P-256
   (8) Brainpool P-512
   (9) secp256k1
Your selection? 1
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 4w
Key expires at 22-Nov-22 12:00:34 Eastern Standard Time
Is this correct? (y/N) y
Really create? (y/N) y
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.

sec  ed25519/BA89F2A837D441FC
     created: 2022-10-25  expires: 2023-01-23  usage: C
     trust: ultimate      validity: ultimate
ssb  ed25519/6C81C3296C26E7C7
     created: 2022-10-25  expires: 2022-11-22  usage: S
[ultimate] (1). Myusename <myemail@email.com>

gpg> save

O:\ on ☁️  (us-east-1) took 31s
❯ gpg --homedir gnupg_temp --expert --edit-key BA89F2A837D441FC
gpg (GnuPG) 2.3.8; Copyright (C) 2021 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Secret key is available.

sec  ed25519/BA89F2A837D441FC
     created: 2022-10-25  expires: 2023-01-23  usage: C
     trust: ultimate      validity: ultimate
ssb  ed25519/6C81C3296C26E7C7
     created: 2022-10-25  expires: 2022-11-22  usage: S
[ultimate] (1). Myusename <myemail@email.com>

gpg> addkey
Please select what kind of key you want:
   (3) DSA (sign only)
   (4) RSA (sign only)
   (5) Elgamal (encrypt only)
   (6) RSA (encrypt only)
   (7) DSA (set your own capabilities)
   (8) RSA (set your own capabilities)
  (10) ECC (sign only)
  (11) ECC (set your own capabilities)
  (12) ECC (encrypt only)
  (13) Existing key
  (14) Existing key from card
Your selection? 12
Please select which elliptic curve you want:
   (1) Curve 25519 *default*
   (2) Curve 448
   (3) NIST P-256
   (4) NIST P-384
   (5) NIST P-521
   (6) Brainpool P-256
   (7) Brainpool P-384
   (8) Brainpool P-512
Your selection? 1
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 4w
Key expires at 22-Nov-22 12:01:05 Eastern Standard Time
Is this correct? (y/N) y
Really create? (y/N) y
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.

sec  ed25519/BA89F2A837D441FC
     created: 2022-10-25  expires: 2023-01-23  usage: C
     trust: ultimate      validity: ultimate
ssb  ed25519/6C81C3296C26E7C7
     created: 2022-10-25  expires: 2022-11-22  usage: S
ssb  cv25519/6AB255B88F26A209
     created: 2022-10-25  expires: 2022-11-22  usage: E
[ultimate] (1). Myusename <myemail@email.com>

gpg> save

O:\ on ☁️  (us-east-1) took 25s
❯ gpg --homedir gnupg_temp --expert --edit-key BA89F2A837D441FC
gpg (GnuPG) 2.3.8; Copyright (C) 2021 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Secret key is available.

sec  ed25519/BA89F2A837D441FC
     created: 2022-10-25  expires: 2023-01-23  usage: C
     trust: ultimate      validity: ultimate
ssb  ed25519/6C81C3296C26E7C7
     created: 2022-10-25  expires: 2022-11-22  usage: S
ssb  cv25519/6AB255B88F26A209
     created: 2022-10-25  expires: 2022-11-22  usage: E
[ultimate] (1). Myusename <myemail@email.com>

gpg> addkey
Please select what kind of key you want:
   (3) DSA (sign only)
   (4) RSA (sign only)
   (5) Elgamal (encrypt only)
   (6) RSA (encrypt only)
   (7) DSA (set your own capabilities)
   (8) RSA (set your own capabilities)
  (10) ECC (sign only)
  (11) ECC (set your own capabilities)
  (12) ECC (encrypt only)
  (13) Existing key
  (14) Existing key from card
Your selection? 11

Possible actions for this ECC key: Sign Authenticate
Current allowed actions: Sign

   (S) Toggle the sign capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? a

Possible actions for this ECC key: Sign Authenticate
Current allowed actions: Sign Authenticate

   (S) Toggle the sign capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? s

Possible actions for this ECC key: Sign Authenticate
Current allowed actions: Authenticate

   (S) Toggle the sign capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? q
Please select which elliptic curve you want:
   (1) Curve 25519 *default*
   (2) Curve 448
   (3) NIST P-256
   (4) NIST P-384
   (5) NIST P-521
   (6) Brainpool P-256
   (7) Brainpool P-384
   (8) Brainpool P-512
   (9) secp256k1
Your selection? 1
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 4w
Key expires at 22-Nov-22 12:01:48 Eastern Standard Time
Is this correct? (y/N) y
Really create? (y/N) y
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
generator a better chance to gain enough entropy.

sec  ed25519/BA89F2A837D441FC
     created: 2022-10-25  expires: 2023-01-23  usage: C
     trust: ultimate      validity: ultimate
ssb  ed25519/6C81C3296C26E7C7
     created: 2022-10-25  expires: 2022-11-22  usage: S
ssb  cv25519/6AB255B88F26A209
     created: 2022-10-25  expires: 2022-11-22  usage: E
ssb  ed25519/F468DCA886F8750D
     created: 2022-10-25  expires: 2022-11-22  usage: A
[ultimate] (1). Myusename <myemail@email.com>

gpg> save

O:\ on ☁️  (us-east-1) took 35s
❯ gpg --homedir gnupg_temp --list-secret-keys --keyid-format=long
O://gnupg_temp/pubring.kbx
--------------------------
sec   ed25519/BA89F2A837D441FC 2022-10-25 [C] [expires: 2023-01-23]
      CC986EBC4A13718DE554B643BA89F2A837D441FC
uid                 [ultimate] Myusename <myemail@email.com>
ssb   ed25519/6C81C3296C26E7C7 2022-10-25 [S] [expires: 2022-11-22]
ssb   cv25519/6AB255B88F26A209 2022-10-25 [E] [expires: 2022-11-22]
ssb   ed25519/F468DCA886F8750D 2022-10-25 [A] [expires: 2022-11-22]


❯ gpg --homedir gnupg_temp --output GPG-BA89F2A837D441FC.asc --gen-revoke BA89F2A837D441FC

sec  ed25519/BA89F2A837D441FC 2022-10-25 Myusename <myemail@email.com>

Create a revocation certificate for this key? (y/N) y
Please select the reason for the revocation:
  0 = No reason specified
  1 = Key has been compromised
  2 = Key is superseded
  3 = Key is no longer used
  Q = Cancel
Your decision? 0
Enter an optional description; end it with an empty line:
Reason for revocation: No reason specified
(No description given)
ASCII armored output forced.
Revocation certificate created.
Please move it to a medium which you can hide away; if Mallory gets
access to this certificate he can use it to make your key unusable.
your media become unreadable.  But have some caution:  The print system of
your machine might store the data and make it available to others!

O:\ on ☁️  (us-east-1) took 13s
❯ gpg --homedir gnupg_temp --list-secret-keys --keyid-format=long
O://gnupg_temp/pubring.kbx
sec   ed25519/BA89F2A837D441FC 2022-10-25 [C] [expires: 2023-01-23]
      CC986EBC4A13718DE554B643BA89F2A837D441FC
uid                 [ultimate] Myusename <myemail@email.com>
ssb   ed25519/6C81C3296C26E7C7 2022-10-25 [S] [expires: 2022-11-22]
ssb   cv25519/6AB255B88F26A209 2022-10-25 [E] [expires: 2022-11-22]


O:\ on ☁️  (us-east-1)
❯ gpg --homedir gnupg_temp --output private-BA89F2A837D441FC.gpg --export-secret-key BA89F2A837D441FC

O:\ on ☁️  (us-east-1) took 5s
❯ gpg --homedir gnupg_temp --output private-subkey-6C81C3296C26E7C7.gpg --export-secret-subkeys 6C81C3296C26E7C7!

O:\ on ☁️  (us-east-1) took 4s
❯ gpg --homedir gnupg_temp --output private-subkey-6AB255B88F26A209.gpg --export-secret-subkeys 6AB255B88F26A209!

O:\ on ☁️  (us-east-1) took 4s
❯ gpg --homedir gnupg_temp --output private-subkey-F468DCA886F8750D.gpg --export-secret-subkeys F468DCA886F8750D!

O:\ on ☁️  (us-east-1) took 5s
❯ gpg --export-ownertrust
# List of assigned trustvalues, created 25-Oct-22 13:05:31 Eastern Daylight Time
# (Use "gpg --import-ownertrust" to restore them)
B918606497361959CEBA4CF2A3F4629FD281BA40:6:

O:\ on ☁️  (us-east-1)
❯ gpg --homedir gnupg_temp --export-ownertrust
# List of assigned trustvalues, created 25-Oct-22 13:05:50 Eastern Daylight Time
# (Use "gpg --import-ownertrust" to restore them)
CC986EBC4A13718DE554B643BA89F2A837D441FC:6:


O:\ on ☁️  (us-east-1)
❯ ls

    Directory: O:\


Mode                 LastWriteTime         Length Name
-a----         25-Oct-22     15:14            296 GPG-1370ECA23597756E.asc
-a----         25-Oct-22     15:15           1241 private-1370ECA23597756E.gpg
-a----         25-Oct-22     15:15            633 private-subkey-569B8979433D2BBC.gpg
-a----         25-Oct-22     15:15            519 private-subkey-AD92D392B67FA40E.gpg
-a----         25-Oct-22     15:16            514 private-subkey-53C60F309AE9ECCF.gpg
-a----         25-Oct-22     15:17            178 otrust.txt



O:\ on ☁️  (us-east-1)
❯ mkdir gnupg_temp


    Directory: O:\


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         26-Oct-22     03:48                gnupg_temp



O:\ on ☁️  (us-east-1)
❯ gpg --homedir gnupg_temp --list-secret-keys --keyid-format=long
gpg: keybox 'O://gnupg_temp/pubring.kbx' created
gpg: O://gnupg_temp/trustdb.gpg: trustdb created

O:\ on ☁️  (us-east-1)
❯ gpg --homedir gnupg_temp --import private-subkey-569B8979433D2BBC.gpg private-subkey-AD92D392B67FA40E.gpg private-subkey-53C60F309AE9ECCF.gpg
gpg: key 1370ECA23597756E: public key "Myusename <myemail@email.com>" imported
gpg: To migrate 'secring.gpg', with each smartcard, run: gpg --card-status
gpg: key 1370ECA23597756E: secret key imported
gpg: key 1370ECA23597756E: "Myusename <myemail@email.com>" 1 new signature
gpg: key 1370ECA23597756E: "Myusename <myemail@email.com>" 1 new subkey
gpg: warning: lower 3 bits of the secret key are not cleared
gpg: To migrate 'secring.gpg', with each smartcard, run: gpg --card-status
gpg: key 1370ECA23597756E: secret key imported
gpg: key 1370ECA23597756E: "Myusename <myemail@email.com>" 1 new signature
gpg: key 1370ECA23597756E: "Myusename <myemail@email.com>" 1 new subkey
gpg: To migrate 'secring.gpg', with each smartcard, run: gpg --card-status
gpg: key 1370ECA23597756E: secret key imported
gpg: Total number processed: 3
gpg:               imported: 1
gpg:            new subkeys: 2
gpg:         new signatures: 2
gpg:       secret keys read: 3
gpg:   secret keys imported: 3

O:\ on ☁️  (us-east-1) took 16s
❯ gpg --homedir gnupg_temp --list-secret-keys --keyid-format=long
O://gnupg_temp/pubring.kbx
--------------------------
sec#  ed25519/1370ECA23597756E 2022-10-25 [C] [expires: 2023-01-23]
      E74FA07E10EA215E57C0C64F1370ECA23597756E
uid                 [ unknown] Myusename <myemail@email.com>
ssb   ed25519/569B8979433D2BBC 2022-10-25 [S] [expires: 2022-11-22]
ssb   cv25519/AD92D392B67FA40E 2022-10-25 [E] [expires: 2022-11-22]
ssb   ed25519/53C60F309AE9ECCF 2022-10-25 [A] [expires: 2022-11-22]


O:\ on ☁️  (us-east-1)
❯


O:\ on ☁️  (us-east-1)
❯ gpg --homedir gnupg_temp --output doc.gpg --encrypt --recipient AD92D392B67FA40E encr.txt
gpg: AD92D392B67FA40E: There is no assurance this key belongs to the named user

sub  cv25519/AD92D392B67FA40E 2022-10-25 Myusename <myemail@email.com>
 Primary key fingerprint: E74F A07E 10EA 215E 57C0  C64F 1370 ECA2 3597 756E
      Subkey fingerprint: 2FFA 1098 589B 17E1 2975  EC6F AD92 D392 B67F A40E
It is NOT certain that the key belongs to the person named
in the user ID.  If you *really* know what you are doing,
you may answer the next question with yes.

Use this key anyway? (y/N) y

O:\ on ☁️  (us-east-1) took 3s
gpg: encrypted with cv25519 key, ID AD92D392B67FA40E, created 2022-10-25
      "Myusename <myemail@email.com>"

```

## Change password of imported subkeys

- You can change the password of an imported subkey, to use a shorter passphrase. We can do this by using a temporary home directory.
- Create a temporary home directory.
- Import a key.
- Start the key editing process with `gpg --homedir temp_directory --edit-key keyid`, where `temp_directory` is the name of the directory and where `keyid` is the ID of the key. A prompt should start with `gpg>`.
- Input `passwd` and change the passphrase.
- Input `quit` and save changes. Or input `save` and then quit.
- All imported subkeys will now have a different passphrase.
- Export keys into another directory.
- Import them into another keyring.
- Test the new keys.

```powershell
mkdir temp_directory
gpg --homedir temp_directory --list-secret-keys --keyid-format=long
gpg --homedir temp_directory --import filename.extension
gpg --homedir temp_directory --list-secret-keys --keyid-format=long
gpg --homedir temp_directory --edit-key keyid
passwd
save
gpg --homedir temp_directory --output doc.sig --sign doc.txt
mkdir subkeys_modified
gpg --homedir temp_directory --output subkeys_modified/private-subkey-keyid.gpg --export-secret-subkeys keyid!
ls
gpg --list-secret-keys --keyid-format=long
gpg --import filename.extension
gpg --list-secret-keys --keyid-format=long
cd ..
gpg --import-ownertrust otrust.txt
gpg --list-secret-keys --keyid-format=long
gpg --output doc.gpg --encrypt --recipient keyid encr.txt
gpg --output doc.txt --decrypt doc.gpg
gpg --local-user keyid --output doc.sig --sign doc.txt
gpg --output doc2.txt --decrypt doc.sig

```

- The below output has been truncated to only passphrase modification.

```powershell

O:\ on ☁️  (us-east-1) took 5s
❯ gpg --homedir temp_directory --list-secret-keys --keyid-format=long
gpg: keyblock resource 'O://temp_directory/pubring.kbx': No such file or directory
gpg: Fatal: O://temp_directory: directory does not exist!

O:\ on ☁️  (us-east-1)
❯ gpg --homedir gnupg_temp --list-secret-keys --keyid-format=long
O://gnupg_temp/pubring.kbx
--------------------------
sec#  ed25519/1370ECA23597756E 2022-10-25 [C] [expires: 2023-01-23]
      E74FA07E10EA215E57C0C64F1370ECA23597756E
uid                 [ unknown] Myusename <myemail@email.com>
ssb   ed25519/569B8979433D2BBC 2022-10-25 [S] [expires: 2022-11-22]
ssb   cv25519/AD92D392B67FA40E 2022-10-25 [E] [expires: 2022-11-22]
ssb   ed25519/53C60F309AE9ECCF 2022-10-25 [A] [expires: 2022-11-22]


O:\ on ☁️  (us-east-1)
❯ gpg --homedir gnupg_temp --edit-key AD92D392B67FA40E
gpg (GnuPG) 2.3.8; Copyright (C) 2021 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Secret subkeys are available.

pub  ed25519/1370ECA23597756E
     created: 2022-10-25  expires: 2023-01-23  usage: C
     trust: unknown       validity: unknown
ssb  ed25519/569B8979433D2BBC
     created: 2022-10-25  expires: 2022-11-22  usage: S
ssb  cv25519/AD92D392B67FA40E
     created: 2022-10-25  expires: 2022-11-22  usage: E
ssb  ed25519/53C60F309AE9ECCF
     created: 2022-10-25  expires: 2022-11-22  usage: A
[ unknown] (1). Myusename <myemail@email.com>

gpg> passwd
gpg: key 1370ECA23597756E/1370ECA23597756E: error changing passphrase: Bad secret key

gpg> save
Key not changed so no update needed.

O:\ on ☁️  (us-east-1) took 22s
❯ gpg --homedir gnupg_temp --output doc.gpg --encrypt --recipient AD92D392B67FA40E encr.txt
gpg: AD92D392B67FA40E: There is no assurance this key belongs to the named user

sub  cv25519/AD92D392B67FA40E 2022-10-25 Myusename <myemail@email.com>
 Primary key fingerprint: E74F A07E 10EA 215E 57C0  C64F 1370 ECA2 3597 756E
      Subkey fingerprint: 2FFA 1098 589B 17E1 2975  EC6F AD92 D392 B67F A40E

It is NOT certain that the key belongs to the person named
in the user ID.  If you *really* know what you are doing,
you may answer the next question with yes.

Use this key anyway? (y/N) y

O:\ on ☁️  (us-east-1) took 2s
❯ gpg --homedir gnupg_temp --output doc.txt --decrypt doc.gpg
gpg: encrypted with cv25519 key, ID AD92D392B67FA40E, created 2022-10-25
      "Myusename <myemail@email.com>"

O:\ on ☁️  (us-east-1) took 6s
❯ gpg --output doc.sig --sign doc.txt

O:\ on ☁️  (us-east-1) took 20s
❯ gpg --homedir gnupg_temp --output doc.sig --sign doc.txt

O:\ on ☁️  (us-east-1)
❯ gpg --homedir gnupg_temp --output doc2.txt --decrypt doc.sig
gpg: Signature made 26-Oct-22 03:54:51 Eastern Daylight Time
gpg:                using EDDSA key 8013D32F252DD5043B3B3297569B8979433D2BBC
gpg: Good signature from "Myusename <myemail@email.com>" [unknown]
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Primary key fingerprint: E74F A07E 10EA 215E 57C0  C64F 1370 ECA2 3597 756E
     Subkey fingerprint: 8013 D32F 252D D504 3B3B  3297 569B 8979 433D 2BBC

O:\ on ☁️  (us-east-1)
❯ ls


    Directory: O:\


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         26-Oct-22     03:48                gnupg_temp
-a----         25-Oct-22     15:14            296 GPG-1370ECA23597756E.asc
-a----         25-Oct-22     15:15           1241 private-1370ECA23597756E.gpg
-a----         25-Oct-22     15:15            633 private-subkey-569B8979433D2BBC.gpg
-a----         25-Oct-22     15:15            519 private-subkey-AD92D392B67FA40E.gpg
-a----         25-Oct-22     15:16            514 private-subkey-53C60F309AE9ECCF.gpg
-a----         25-Oct-22     15:17            178 otrust.txt
-a----         26-Oct-22     03:51              5 encr.txt
-a----         26-Oct-22     03:52            180 doc.gpg
-a----         26-Oct-22     03:52              5 doc.txt
-a----         26-Oct-22     03:54            149 doc.sig
-a----         26-Oct-22     03:55              5 doc2.txt



O:\ on ☁️  (us-east-1)
❯

```
