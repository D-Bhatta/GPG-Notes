# Create a new RSA Key for signing

Create a new RSA key for signing.

## Sections

- [Create a new RSA Key for signing](#create-a-new-rsa-key-for-signing)
  - [Sections](#sections)
  - [Generate a key](#generate-a-key)

## Generate a key

- Check GPG version with `gpg --version`.

```powershell
GPG-Notes on  create-new-rsa-key-ChiCh7V [⇡] on ☁️  (us-east-1) 
❯ gpg --version
gpg (GnuPG) 2.3.7
libgcrypt 1.10.1
Copyright (C) 2021 g10 Code GmbH
License GNU GPL-3.0-or-later <https://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Home: C:\Users\username\AppData\Roaming\gnupg
Supported algorithms:
Pubkey: RSA, ELG, DSA, ECDH, ECDSA, EDDSA
Cipher: IDEA, 3DES, CAST5, BLOWFISH, AES, AES192, AES256, TWOFISH,
        CAMELLIA128, CAMELLIA192, CAMELLIA256
AEAD: EAX, OCB
Hash: SHA1, RIPEMD160, SHA256, SHA384, SHA512, SHA224
Compression: Uncompressed, ZIP, ZLIB, BZIP2
```

- To start generating a key, type: `gpg --full-generate-key`.
- Select `RSA (sign only)` when asked for type of key.
- Input key size as `4096`, a secure option.
- Keeping keys around for a long time itself may not lead to security incidents. However, making keys expire will help develop practices that do not just depend on key integrity and confidentiality remaining intact forever. Also, key expiry dates can invite periodic security adjustments on the organization. Moreover, regular key rotation ensures leaked keys have a small shelf life before they are rendered unusable.
- Input validity time as `4w`. Review time and confirm when asked.
- Add identity information. The information will be available to everyone who can access the public key, so be careful about that. Review information and confirm when asked.
- Add a passphrase. Store it securely.
- Once this is over, there should be a key printed in the terminal.

```powershell
GPG-Notes on  create-new-rsa-key-ChiCh7V [!⇡] on ☁️  (us-east-1)
❯ gpg --full-generate-key
gpg (GnuPG) 2.3.7; Copyright (C) 2021 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Please select what kind of key you want:
   (1) RSA and RSA
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
   (9) ECC (sign and encrypt) *default*
  (10) ECC (sign only)
  (14) Existing key from card
Your selection? 4
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (3072) 4096
Requested keysize is 4096 bits
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 4w
Key expires at 24-Oct-22 12:41:16 Eastern Daylight Time
Is this correct? (y/N) y

GnuPG needs to construct a user ID to identify your key.

Real name: My-name
Email address: myname@gmail.com
Comment: RSA key for signing
You selected this USER-ID:
    "My-name (RSA key for signing) <myname@gmail.com>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
gpg: directory 'C:\\Users\\user\\AppData\\Roaming\\gnupg\\openpgp-revocs.d' created
gpg: revocation certificate stored as 'C:\\Users\\user\\AppData\\Roaming\\gnupg\\openpgp-revocs.d\\D34ED6F56025EFB6B88C5A8065D9AD8DB8063134.rev'
public and secret key created and signed.

Note that this key cannot be used for encryption.  You may want to use
the command "--edit-key" to generate a subkey for this purpose.
pub   rsa4096 2022-09-26 [SC] [expires: 2022-10-24]
      D34ED6F56025EFB6B88C5A8065D9AD8DB8063134
uid                      My-name (RSA key for signing) <myname@gmail.com>


GPG-Notes on  create-new-rsa-key-ChiCh7V [!⇡] on ☁️  (us-east-1) took 1m33s
❯
```

- To view the keys, use `gpg --list-secret-keys --keyid-format=long`.

```powershell
GPG-Notes on  create-new-rsa-key-ChiCh7V [!⇡] on ☁️  (us-east-1) 
❯ gpg --list-secret-keys --keyid-format=long
gpg: checking the trustdb
gpg: marginals needed: 3  completes needed: 1  trust model: pgp
gpg: depth: 0  valid:   1  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 1u
gpg: next trustdb check due at 2022-10-24
C:\Users\username\AppData\Roaming\gnupg\pubring.kbx
---------------------------------------------
sec   rsa4096/65D9AD8DB8063134 2022-09-26 [SC] [expires: 2022-10-24]
      D34ED6F56025EFB6B88C5A8065D9AD8DB8063134
uid                 [ultimate] My-name (RSA key for signing) <myname@gmail.com>


GPG-Notes on  create-new-rsa-key-ChiCh7V [!⇡] on ☁️  (us-east-1) 
❯
```

- To export a key, use `gpg --armor --export key_id`, where `key_id` is the ID of the key to be exported. This will export the public key pair in ASCII armor format.

```powershell
GPG-Notes on  create-new-rsa-key-ChiCh7V [!⇡] on ☁️  (us-east-1) 
❯ gpg --armor --export  65D9AD8DB8063134
-----BEGIN PGP PUBLIC KEY BLOCK-----

...==
=yUFb
-----END PGP PUBLIC KEY BLOCK-----

GPG-Notes on  create-new-rsa-key-ChiCh7V [!⇡] on ☁️  (us-east-1) 
❯
```
