# Rotate an expired ECC subkey with curve Ed25519

Rotate an expired ECC subkey with curve Ed25519.

## Sections

- [Rotate an expired ECC subkey with curve Ed25519](#rotate-an-expired-ecc-subkey-with-curve-ed25519)
  - [Sections](#sections)
  - [List secret keys](#list-secret-keys)
  - [Generate new subkeys](#generate-new-subkeys)

## List secret keys

- See listed keys with `gpg --list-secret-keys --keyid-format=long`.
- Expired keys will not showup.

```powershell
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


O:\ on ☁️  (us-east-1)
❯
```

## Generate new subkeys

- Backup all files, just in case.
- Move old subkeys into another folder.
- Create a new directory with `mkdir temp_directory`, where `temp_directory` is the name of the directory.
- Import the primary private key with `gpg --homedir temp_directory --import filename.extension`.
- Import owner trust with `gpg --homedir temp_directory --import-ownertrust otrust.txt` where `otrust.txt` is the name of the text file where ownertrust is stored..
- Generate new subkeys by starting the key editing process with `gpg --homedir temp_directory --edit-key keyid`, where `temp_directory` is the name of the directory and where `keyid` is the ID of the key. A prompt should start with `gpg>`. Input `addkey` to generate new subkeys.
- Export a primary public key and all subkeys in GPG binary format with `gpg --output key_filename.gpg --export identifier`, where identifier is used to select a set of keys. It can be username, email, keyid, etc. It can be convenient to set the filename as `public-keyid.GPG`. Where `keyid` is the ID of the key.
- Export a primary private key and all subkeys in GPG binary format with `gpg --output key_filename.gpg --export-secret-key identifier`, where identifier is used to select a set of keys. It can be username, email, keyid, etc. It can be convenient to set the filename as `private-keyid.gpg`. Where `keyid` is the ID of the key.
- Export newly generated individual private subkeys in GPG binary format with `gpg --output key_filename.gpg --export-secret-subkeys keyid!`. It can be convenient to set the filename as `private-subkey-keyid.gpg`. Where `keyid` is the ID of the key.
- Export ownertrust.

```powershell
ls
mkdir temp_directory
gpg --homedir temp_directory --list-secret-keys --keyid-format=long
gpg --homedir temp_directory --import filename.extension
gpg --homedir temp_directory --import-ownertrust otrust.txt
gpg --homedir temp_directory --list-secret-keys --keyid-format=long
gpg --homedir temp_directory --expert --edit-key keyid
gpg --output public-keyid.gpg --export identifier
gpg --homedir temp_directory --output private-keyid.gpg --export-secret-key identifier
gpg --homedir temp_directory --output private-subkey-keyid.gpg --export-secret-subkeys keyid!
gpg --homedir temp_directory --export-ownertrust
```

- Change the password of subkeys to use a shorter passphrase.
- Test the new keys.

```powershell
ls
mkdir temp_directory
gpg --homedir temp_directory --list-secret-keys --keyid-format=long
gpg --homedir temp_directory --import filename.extension
gpg --homedir temp_directory --list-secret-keys --keyid-format=long
gpg --homedir temp_directory --expert --edit-key keyid
passwd
save
mkdir subkeys_modified
gpg --homedir temp_directory --output subkeys_modified/private-subkey-keyid.gpg --export-secret-subkeys keyid!

ls
cd subkeys_modified
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

```powershell
O:\ on ☁️  (us-east-1)
❯ ls


    Directory: O:\


Mode                 LastWriteTime         Length Name
d-----         24-Dec-22     09:54                old_subkeys_A3F4629FD281BA40
d-----         24-Dec-22     10:24                backup
-a----         26-Oct-22     12:24            296 GPG-A3F4629FD281BA40.asc
-a----         26-Oct-22     12:26           1241 private-A3F4629FD281BA40.gpg
-a----         26-Oct-22     12:30            178 otrust.txt
-a----         25-Nov-22     05:17              5 encr.txt
-a----         25-Nov-22     05:18            180 doc.gpg
-a----         25-Nov-22     05:19              5 doc.txt
-a----         25-Nov-22     05:20            148 doc.sig
-a----         25-Nov-22     05:20              5 doc2.txt
-a----         24-Dec-22     10:23           1576 public-A3F4629FD281BA40.gpg



O:\ on ☁️  (us-east-1)
❯ mkdir gnupg_temp


    Directory: O:\


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         24-Dec-22     10:32                gnupg_temp



O:\ on ☁️  (us-east-1)
❯ gpg --homedir gnupg_temp --list-secret-keys --keyid-format=long
gpg: keybox 'O://gnupg_temp/pubring.kbx' created
gpg: O://gnupg_temp/trustdb.gpg: trustdb created

O:\ on ☁️  (us-east-1)
❯ gpg --homedir gnupg_temp --import private-A3F4629FD281BA40.gpg
gpg: key A3F4629FD281BA40: public key "Myusename <myemail@email.com>" imported
gpg: warning: lower 3 bits of the secret key are not cleared
gpg: key A3F4629FD281BA40: secret key imported
gpg: Total number processed: 1
gpg:               imported: 1
gpg:       secret keys read: 1
gpg:   secret keys imported: 1

O:\ on ☁️  (us-east-1) took 47s
❯ gpg --homedir gnupg_temp --import-ownertrust otrust.txt
gpg: inserting ownertrust of 6

O:\ on ☁️  (us-east-1)
❯
  gpg --homedir gnupg_temp --import public-A3F4629FD281BA40.gpg
gpg: key A3F4629FD281BA40: "Myusename <myemail@email.com>" 3 new signatures
gpg: key A3F4629FD281BA40: "Myusename <myemail@email.com>" 3 new subkeys
gpg: Total number processed: 1
gpg:            new subkeys: 3
gpg:         new signatures: 3

O:\ on ☁️  (us-east-1)
❯
  gpg --homedir gnupg_temp --list-secret-keys --keyid-format=long
gpg: checking the trustdb
gpg: marginals needed: 3  completes needed: 1  trust model: pgp
gpg: depth: 0  valid:   1  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 1u
gpg: next trustdb check due at 2023-01-24
O://gnupg_temp/pubring.kbx
--------------------------
sec   ed25519/A3F4629FD281BA40 2022-10-26 [C] [expires: 2023-01-24]
      B918606497361959CEBA4CF2A3F4629FD281BA40
uid                 [ultimate] Myusename <myemail@email.com>


O:\ on ☁️  (us-east-1)
❯
  gpg --homedir gnupg_temp --expert --edit-key A3F4629FD281BA40
gpg (GnuPG) 2.3.8; Copyright (C) 2021 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Secret key is available.

sec  ed25519/A3F4629FD281BA40
     created: 2022-10-26  expires: 2023-01-24  usage: C
     trust: ultimate      validity: ultimate
ssb  ed25519/57EF7B1CFC3857E7
     created: 2022-10-26  expired: 2022-11-23  usage: S
ssb  cv25519/2BE955F8622E955F
     created: 2022-10-26  expired: 2022-11-23  usage: E
ssb  ed25519/31BB23812DA62DA6
     created: 2022-10-26  expired: 2022-11-23  usage: A
sub  ed25519/60A152A3F0F5F0F5
     created: 2022-11-25  expired: 2022-12-23  usage: S
sub  cv25519/120ADCB9BC0FBC0F
     created: 2022-11-25  expired: 2022-12-23  usage: E
sub  ed25519/BECBF4FCB62AB62A
     created: 2022-11-25  expired: 2022-12-23  usage: A
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
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>y = key expires in n years
Key is valid for? (0) 4w
Key expires at 21-Jan-23 10:37:30 Eastern Standard Time
Is this correct? (y/N) y
Really create? (y/N) y
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.

sec  ed25519/A3F4629FD281BA40
     created: 2022-10-26  expires: 2023-01-24  usage: C
     trust: ultimate      validity: ultimate
ssb  ed25519/57EF7B1CFC3857E7
     created: 2022-10-26  expired: 2022-11-23  usage: S
ssb  cv25519/2BE955F8622E955F
     created: 2022-10-26  expired: 2022-11-23  usage: E
ssb  ed25519/31BB23812DA62DA6
     created: 2022-10-26  expired: 2022-11-23  usage: A
sub  ed25519/60A152A3F0F5F0F5
     created: 2022-11-25  expired: 2022-12-23  usage: S
sub  cv25519/120ADCB9BC0FBC0F
     created: 2022-11-25  expired: 2022-12-23  usage: E
sub  ed25519/BECBF4FCB62AB62A
     created: 2022-11-25  expired: 2022-12-23  usage: A
ssb  ed25519/6D321D3644AE21D3
     created: 2022-12-24  expires: 2023-01-21  usage: S
[ultimate] (1). Myusename <myemail@email.com>

gpg> save

O:\ on ☁️  (us-east-1) took 2m38s
❯ gpg --homedir gnupg_temp --expert --edit-key A3F4629FD281BA40
gpg (GnuPG) 2.3.8; Copyright (C) 2021 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Secret key is available.

sec  ed25519/A3F4629FD281BA40
     created: 2022-10-26  expires: 2023-01-24  usage: C
     trust: ultimate      validity: ultimate
ssb  ed25519/57EF7B1CFC3857E7
     created: 2022-10-26  expired: 2022-11-23  usage: S
ssb  cv25519/2BE955F8622E955F
     created: 2022-10-26  expired: 2022-11-23  usage: E
ssb  ed25519/31BB23812DA62DA6
     created: 2022-10-26  expired: 2022-11-23  usage: A
sub  ed25519/60A152A3F0F5F0F5
     created: 2022-11-25  expired: 2022-12-23  usage: S
sub  cv25519/120ADCB9BC0FBC0F
     created: 2022-11-25  expired: 2022-12-23  usage: E
sub  ed25519/BECBF4FCB62AB62A
     created: 2022-11-25  expired: 2022-12-23  usage: A
ssb  ed25519/6D321D3644AE21D3
     created: 2022-12-24  expires: 2023-01-21  usage: S
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
   (9) secp256k1
Your selection? 1
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key expires at 21-Jan-23 10:38:59 Eastern Standard Time
Is this correct? (y/N) y
Really create? (y/N) y
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.

sec  ed25519/A3F4629FD281BA40
     created: 2022-10-26  expires: 2023-01-24  usage: C
     trust: ultimate      validity: ultimate
ssb  ed25519/57EF7B1CFC3857E7
     created: 2022-10-26  expired: 2022-11-23  usage: S
ssb  cv25519/2BE955F8622E955F
     created: 2022-10-26  expired: 2022-11-23  usage: E
ssb  ed25519/31BB23812DA62DA6
     created: 2022-10-26  expired: 2022-11-23  usage: A
sub  ed25519/60A152A3F0F5F0F5
     created: 2022-11-25  expired: 2022-12-23  usage: S
sub  cv25519/120ADCB9BC0FBC0F
     created: 2022-11-25  expired: 2022-12-23  usage: E
sub  ed25519/BECBF4FCB62AB62A
     created: 2022-11-25  expired: 2022-12-23  usage: A
ssb  ed25519/6D321D3644AE21D3
     created: 2022-12-24  expires: 2023-01-21  usage: S
ssb  cv25519/028588DF293988DF
     created: 2022-12-24  expires: 2023-01-21  usage: E
[ultimate] (1). Myusename <myemail@email.com>

gpg> save

O:\ on ☁️  (us-east-1) took 53s
❯ gpg --homedir gnupg_temp --expert --edit-key A3F4629FD281BA40
gpg (GnuPG) 2.3.8; Copyright (C) 2021 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Secret key is available.

sec  ed25519/A3F4629FD281BA40
     created: 2022-10-26  expires: 2023-01-24  usage: C
     trust: ultimate      validity: ultimate
ssb  ed25519/57EF7B1CFC3857E7
     created: 2022-10-26  expired: 2022-11-23  usage: S
ssb  cv25519/2BE955F8622E955F
     created: 2022-10-26  expired: 2022-11-23  usage: E
ssb  ed25519/31BB23812DA62DA6
     created: 2022-10-26  expired: 2022-11-23  usage: A
sub  ed25519/60A152A3F0F5F0F5
     created: 2022-11-25  expired: 2022-12-23  usage: S
sub  cv25519/120ADCB9BC0FBC0F
     created: 2022-11-25  expired: 2022-12-23  usage: E
sub  ed25519/BECBF4FCB62AB62A
     created: 2022-11-25  expired: 2022-12-23  usage: A
ssb  ed25519/6D321D3644AE21D3
     created: 2022-12-24  expires: 2023-01-21  usage: S
ssb  cv25519/028588DF293988DF
     created: 2022-12-24  expires: 2023-01-21  usage: E
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
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 4w
Key expires at 21-Jan-23 10:39:56 Eastern Standard Time
Really create? (y/N) y
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
generator a better chance to gain enough entropy.

sec  ed25519/A3F4629FD281BA40
     created: 2022-10-26  expires: 2023-01-24  usage: C
     trust: ultimate      validity: ultimate
ssb  ed25519/57EF7B1CFC3857E7
     created: 2022-10-26  expired: 2022-11-23  usage: S
ssb  cv25519/2BE955F8622E955F
     created: 2022-10-26  expired: 2022-11-23  usage: E
ssb  ed25519/31BB23812DA62DA6
     created: 2022-10-26  expired: 2022-11-23  usage: A
sub  ed25519/60A152A3F0F5F0F5
     created: 2022-11-25  expired: 2022-12-23  usage: S
sub  cv25519/120ADCB9BC0FBC0F
     created: 2022-11-25  expired: 2022-12-23  usage: E
sub  ed25519/BECBF4FCB62AB62A
     created: 2022-11-25  expired: 2022-12-23  usage: A
ssb  ed25519/6D321D3644AE21D3
     created: 2022-12-24  expires: 2023-01-21  usage: S
ssb  cv25519/028588DF293988DF
     created: 2022-12-24  expires: 2023-01-21  usage: E
ssb  ed25519/F3D12E475E0FE475
     created: 2022-12-24  expires: 2023-01-21  usage: A
[ultimate] (1). Myusename <myemail@email.com>

gpg> save

O:\ on ☁️  (us-east-1) took 1m37s
❯ gpg --homedir gnupg_temp --output public-A3F4629FD281BA40.gpg --export A3F4629FD281BA40
File 'public-A3F4629FD281BA40.gpg' exists. Overwrite? (y/N) y

O:\ on ☁️  (us-east-1)
❯ gpg --homedir gnupg_temp --list-secret-keys --keyid-format=long
O://gnupg_temp/pubring.kbx
--------------------------
sec   ed25519/A3F4629FD281BA40 2022-10-26 [C] [expires: 2023-01-24]
      B918606497361959CEBA4CF2A3F4629FD281BA40
uid                 [ultimate] Myusename <myemail@email.com>
ssb   ed25519/6D321D3644AE21D3 2022-12-24 [S] [expires: 2023-01-21]
ssb   cv25519/028588DF293988DF 2022-12-24 [E] [expires: 2023-01-21]
ssb   ed25519/F3D12E475E0FE475 2022-12-24 [A] [expires: 2023-01-21]


O:\ on ☁️  (us-east-1)
❯
  gpg --homedir gnupg_temp --output private-A3F4629FD281BA40.gpg --export-secret-key A3F4629FD281BA40
File 'private-A3F4629FD281BA40.gpg' exists. Overwrite? (y/N) y

O:\ on ☁️  (us-east-1) took 36s
❯ gpg --homedir gnupg_temp --output private-subkey-6D321D3644AE21D3.gpg --export-secret-subkeys 6D321D3644AE21D3!

O:\ on ☁️  (us-east-1) took 35s
❯ gpg --homedir gnupg_temp --output private-subkey-028588DF293988DF.gpg --export-secret-subkeys 028588DF293988DF!

O:\ on ☁️  (us-east-1) took 26s
❯ gpg --homedir gnupg_temp --output private-subkey-F3D12E475E0FE475.gpg --export-secret-subkeys F3D12E475E0FE475!

O:\ on ☁️  (us-east-1) took 30s
❯ gpg --homedir gnupg_temp --export-ownertrust otrust.txt
usage: gpg [options] --export-ownertrust

O:\ on ☁️  (us-east-1)
❯ gpg --homedir gnupg_temp --export-ownertrust
# List of assigned trustvalues, created 24-Dec-22 10:48:39 Eastern Standard Time
# (Use "gpg --import-ownertrust" to restore them)
B918606497361959CEBA4CF2A3F4629FD281BA40:6:

O:\ on ☁️  (us-east-1)
❯ ls


    Directory: O:\


----                 -------------         ------ ----
d-----         24-Dec-22     09:54                old_subkeys_A3F4629FD281BA40
d-----         24-Dec-22     10:24                backup
-a----         26-Oct-22     12:24            296 GPG-A3F4629FD281BA40.asc
-a----         24-Dec-22     10:49            178 otrust.txt
-a----         25-Nov-22     05:17              5 encr.txt
-a----         25-Nov-22     05:18            180 doc.gpg
-a----         25-Nov-22     05:19              5 doc.txt
-a----         25-Nov-22     05:20            148 doc.sig
-a----         25-Nov-22     05:20              5 doc2.txt
-a----         24-Dec-22     10:42           2243 public-A3F4629FD281BA40.gpg
-a----         24-Dec-22     10:47            633 private-subkey-6D321D3644AE21D3.gpg
-a----         24-Dec-22     10:47            519 private-subkey-028588DF293988DF.gpg



O:\ on ☁️  (us-east-1)
❯ mkdir gnupg_temp


    Directory: O:\


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         24-Dec-22     10:50                gnupg_temp



❯ gpg --homedir gnupg_temp --list-secret-keys --keyid-format=long
gpg: keybox 'O://gnupg_temp/pubring.kbx' created
gpg: O://gnupg_temp/trustdb.gpg: trustdb created

O:\ on ☁️  (us-east-1)
❯ gpg --homedir gnupg_temp --import private-subkey-6D321D3644AE21D3.gpg
gpg: key A3F4629FD281BA40: public key "Myusename <myemail@email.com>" imported
gpg: To migrate 'secring.gpg', with each smartcard, run: gpg --card-status
gpg: key A3F4629FD281BA40: secret key imported
gpg: Total number processed: 1
gpg:               imported: 1
gpg:   secret keys imported: 1

O:\ on ☁️  (us-east-1) took 31s
❯ gpg --homedir gnupg_temp --import private-subkey-028588DF293988DF.gpg private-subkey-F3D12E475E0FE475.gpg
gpg: key A3F4629FD281BA40: "Myusename <myemail@email.com>" 1 new signature
gpg: key A3F4629FD281BA40: "Myusename <myemail@email.com>" 1 new subkey
gpg: warning: lower 3 bits of the secret key are not cleared
gpg: To migrate 'secring.gpg', with each smartcard, run: gpg --card-status
gpg: key A3F4629FD281BA40: secret key imported
gpg: key A3F4629FD281BA40: "Myusename <myemail@email.com>" 1 new signature
gpg: key A3F4629FD281BA40: "Myusename <myemail@email.com>" 1 new subkey
gpg: To migrate 'secring.gpg', with each smartcard, run: gpg --card-status
gpg: key A3F4629FD281BA40: secret key imported
gpg: Total number processed: 2
gpg:            new subkeys: 2
gpg:         new signatures: 2
gpg:       secret keys read: 2
gpg:   secret keys imported: 2

O:\ on ☁️  (us-east-1) took 1m1s
❯ gpg --homedir gnupg_temp --list-secret-keys --keyid-format=long
O://gnupg_temp/pubring.kbx
--------------------------
sec#  ed25519/A3F4629FD281BA40 2022-10-26 [C] [expires: 2023-01-24]
uid                 [ unknown] Myusename <myemail@email.com>
ssb   ed25519/6D321D3644AE21D3 2022-12-24 [S] [expires: 2023-01-21]
ssb   cv25519/028588DF293988DF 2022-12-24 [E] [expires: 2023-01-21]
ssb   ed25519/F3D12E475E0FE475 2022-12-24 [A] [expires: 2023-01-21]


O:\ on ☁️  (us-east-1)
❯ gpg --homedir gnupg_temp --expert --edit-key 6D321D3644AE21D3
gpg (GnuPG) 2.3.8; Copyright (C) 2021 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.


pub  ed25519/A3F4629FD281BA40
     trust: unknown       validity: unknown
ssb  ed25519/6D321D3644AE21D3
ssb  cv25519/028588DF293988DF
     created: 2022-12-24  expires: 2023-01-21  usage: E
     created: 2022-12-24  expires: 2023-01-21  usage: A
[ unknown] (1). Myusename <myemail@email.com>

gpg> passwd
gpg: key A3F4629FD281BA40/A3F4629FD281BA40: error changing passphrase: Bad secret key

gpg> save
Key not changed so no update needed.

O:\ on ☁️  (us-east-1) took 2m6s
❯ mkdir subkeys_modified


    Directory: O:\


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         24-Dec-22     10:56                subkeys_modified



O:\ on ☁️  (us-east-1)
❯ gpg --homedir gnupg_temp --output subkeys_modified/private-subkey-6D321D3644AE21D3.gpg --export-secret-subkeys 6D321D3644AE21D3!

O:\ on ☁️  (us-east-1) took 8s

O:\ on ☁️  (us-east-1) took 12s
❯ gpg --homedir gnupg_temp --output subkeys_modified/private-subkey-F3D12E475E0FE475.gpg --export-secret-subkeys F3D12E475E0FE475!

O:\ on ☁️  (us-east-1) took 9s
❯ ls


    Directory: O:\


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         24-Dec-22     09:54                old_subkeys_A3F4629FD281BA40
d-----         24-Dec-22     10:24                backup
d-----         24-Dec-22     10:56                subkeys_modified
-a----         26-Oct-22     12:24            296 GPG-A3F4629FD281BA40.asc
-a----         24-Dec-22     10:45           2848 private-A3F4629FD281BA40.gpg
-a----         24-Dec-22     10:49            178 otrust.txt
-a----         25-Nov-22     05:17              5 encr.txt
-a----         25-Nov-22     05:18            180 doc.gpg
-a----         25-Nov-22     05:19              5 doc.txt
-a----         25-Nov-22     05:20            148 doc.sig
-a----         25-Nov-22     05:20              5 doc2.txt
-a----         24-Dec-22     10:42           2243 public-A3F4629FD281BA40.gpg
-a----         24-Dec-22     10:47            633 private-subkey-6D321D3644AE21D3.gpg
-a----         24-Dec-22     10:47            519 private-subkey-028588DF293988DF.gpg
-a----         24-Dec-22     10:48            514 private-subkey-F3D12E475E0FE475.gpg



O:\ on ☁️  (us-east-1)
❯ ls subkeys_modified


    Directory: O:\subkeys_modified


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         24-Dec-22     10:57            633 private-subkey-6D321D3644AE21D3.gpg
-a----         24-Dec-22     10:57            519 private-subkey-028588DF293988DF.gpg
-a----         24-Dec-22     10:57            514 private-subkey-F3D12E475E0FE475.gpg



O:\ on ☁️  (us-east-1)
❯
  ls


    Directory: O:\


Mode                 LastWriteTime         Length Name
d-----         24-Dec-22     09:54                old_subkeys_A3F4629FD281BA40
d-----         24-Dec-22     10:24                backup
-a----         26-Oct-22     12:24            296 GPG-A3F4629FD281BA40.asc
-a----         24-Dec-22     10:45           2848 private-A3F4629FD281BA40.gpg
-a----         24-Dec-22     10:49            178 otrust.txt
-a----         25-Nov-22     05:17              5 encr.txt
-a----         25-Nov-22     05:18            180 doc.gpg
-a----         25-Nov-22     05:19              5 doc.txt
-a----         25-Nov-22     05:20            148 doc.sig
-a----         25-Nov-22     05:20              5 doc2.txt
-a----         24-Dec-22     10:42           2243 public-A3F4629FD281BA40.gpg
-a----         24-Dec-22     10:47            633 private-subkey-6D321D3644AE21D3.gpg
-a----         24-Dec-22     10:47            519 private-subkey-028588DF293988DF.gpg
-a----         24-Dec-22     10:48            514 private-subkey-F3D12E475E0FE475.gpg


O:\ on ☁️  (us-east-1)
❯ gpg --list-secret-keys --keyid-format=long
C:\Users\user\AppData\Roaming\gnupg\pubring.kbx
---------------------------------------------
sec   rsa1120/65D9AD8DB8063134 2022-10-22 [SC] [expires: 2023-01-20]
      D34ED6F56025EFB6B88C5A8065D9AD8DB8063134
uid                 [ultimate] Myusename <myemail@email.com>

sec#  ed25519/A3F4629FD281BA40 2022-10-26 [C] [expires: 2023-01-24]
      B918606497361959CEBA4CF2A3F4629FD281BA40
uid                 [ultimate] Myusename <myemail@email.com>


O:\ on ☁️  (us-east-1)
❯ cd .\subkeys_modified\

O:\subkeys_modified on ☁️  (us-east-1)
❯ ls




Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         24-Dec-22     10:57            633 private-subkey-6D321D3644AE21D3.gpg
-a----         24-Dec-22     10:57            519 private-subkey-028588DF293988DF.gpg
-a----         24-Dec-22     10:57            514 private-subkey-F3D12E475E0FE475.gpg



O:\subkeys_modified on ☁️  (us-east-1)
❯ gpg --import private-subkey-6D321D3644AE21D3.gpg private-subkey-028588DF293988DF.gpg private-subkey-F3D12E475E0FE475.gpg
gpg: key A3F4629FD281BA40: "Myusename <myemail@email.com>" 1 new signature
gpg: key A3F4629FD281BA40: "Myusename <myemail@email.com>" 1 new subkey
gpg: To migrate 'secring.gpg', with each smartcard, run: gpg --card-status
gpg: key A3F4629FD281BA40: "Myusename <myemail@email.com>" 1 new signature
gpg: key A3F4629FD281BA40: "Myusename <myemail@email.com>" 1 new subkey
gpg: To migrate 'secring.gpg', with each smartcard, run: gpg --card-status
gpg: key A3F4629FD281BA40: secret key imported
gpg: key A3F4629FD281BA40: "Myusename <myemail@email.com>" 1 new subkey
gpg: To migrate 'secring.gpg', with each smartcard, run: gpg --card-status
gpg: key A3F4629FD281BA40: secret key imported
gpg: Total number processed: 3
gpg:            new subkeys: 3
gpg:         new signatures: 3
gpg:       secret keys read: 3
gpg:   secret keys imported: 3

O:\subkeys_modified on ☁️  (us-east-1) took 29s
❯ gpg --list-secret-keys --keyid-format=long
C:\Users\user\AppData\Roaming\gnupg\pubring.kbx
---------------------------------------------
sec   rsa1120/65D9AD8DB8063134 2022-10-22 [SC] [expires: 2023-01-20]
      D34ED6F56025EFB6B88C5A8065D9AD8DB8063134

sec#  ed25519/A3F4629FD281BA40 2022-10-26 [C] [expires: 2023-01-24]
      B918606497361959CEBA4CF2A3F4629FD281BA40
uid                 [ultimate] Myusename <myemail@email.com>
ssb   cv25519/028588DF293988DF 2022-12-24 [E] [expires: 2023-01-21]
ssb   ed25519/F3D12E475E0FE475 2022-12-24 [A] [expires: 2023-01-21]

O:\subkeys_modified on ☁️  (us-east-1)
❯ cd ..

O:\ on ☁️  (us-east-1)
❯ gpg --import-ownertrust otrust.txt
O:\ on ☁️  (us-east-1)
❯ gpg --list-secret-keys --keyid-format=long
C:\Users\user\AppData\Roaming\gnupg\pubring.kbx
sec   rsa1120/65D9AD8DB8063134 2022-10-22 [SC] [expires: 2023-01-20]
      D34ED6F56025EFB6B88C5A8065D9AD8DB8063134
uid                 [ultimate] Myusename <myemail@email.com>
sec#  ed25519/A3F4629FD281BA40 2022-10-26 [C] [expires: 2023-01-24]
      B918606497361959CEBA4CF2A3F4629FD281BA40
uid                 [ultimate] Myusename <myemail@email.com>
ssb   ed25519/6D321D3644AE21D3 2022-12-24 [S] [expires: 2023-01-21]
ssb   cv25519/028588DF293988DF 2022-12-24 [E] [expires: 2023-01-21]
ssb   ed25519/F3D12E475E0FE475 2022-12-24 [A] [expires: 2023-01-21]

O:\ on ☁️  (us-east-1)
❯ gpg --output doc.gpg --encrypt --recipient keyid encr.txt
gpg: keyid: skipped: No public key
gpg: encr.txt: encryption failed: No public key

O:\ on ☁️  (us-east-1)
❯ gpg --output doc.gpg --encrypt --recipient 028588DF293988DF encr.txt
File 'doc.gpg' exists. Overwrite? (y/N) y

O:\ on ☁️  (us-east-1) took 3s
❯ gpg --output doc.txt --decrypt doc.gpg
gpg: encrypted with cv25519 key, ID 028588DF293988DF, created 2022-12-24
      "Myusename <myemail@email.com>"
File 'doc.txt' exists. Overwrite? (y/N) y

O:\ on ☁️  (us-east-1) took 17s
❯ cat doc.txt
hello

O:\ on ☁️  (us-east-1)
❯ gpg --local-user 6D321D3644AE21D3 --output doc.sig --sign doc.txt
File 'doc.sig' exists. Overwrite? (y/N) y

O:\ on ☁️  (us-east-1)
❯ gpg --output doc2.txt --decrypt doc.sig
File 'doc2.txt' exists. Overwrite? (y/N) y
gpg: Signature made 24-Dec-22 11:03:36 Eastern Standard Time
gpg:                using EDDSA key 2361D0CE15A094FF95F052306D321D3644AE21D3
gpg: Good signature from "Myusename <myemail@email.com>" [ultimate]

O:\ on ☁️  (us-east-1)
❯ cat doc2.txt
hello

O:\ on ☁️  (us-east-1)
❯
```
