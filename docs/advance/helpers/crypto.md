# Crypto

Cryptographic functions using the Crypto npm library.

## Hash

One way hashing of data.

```ts
import { hash } from 'lesgo/utils/crypto';

const hashedString = hash('some data to hash');
// 6a2da20943931e9834fc12cfe5bb47bbd9ae43489a30726962b576f4e3993e50
```

You may also define the algorithm to be used by setting the 2nd parameter. SHA256 will be used if none is set.

```ts
const md5String = hash('some data to hash', { 
  algorithm: 'md5'
});
```

Available algorithms:

- `md5`
- `sha256`
- `sha512`

!!! warning "Warning"

    While MD5 hashing algorithm is fast, it is not at all recommended as it is considered a weak 1-iteration hash with no salt.

You may set the algorithm to be used globally by defining the following env variable:

```bash
LESGO_CRYPTO_HASH_ALG=sha512
```

## Encrypt

2-way encryption with iv and cipher.

```ts
import { encrypt } from 'lesgo/utils/crypto';

const encryptedStr = encrypt('some string to be encrypted that can be decrypted');
//
```

You may also define the algorithm and secret key used by setting the 2nd parameter.

```ts
const encryptedStr = encrypt('some string to be encrypted that can be decrypted', {
  algorithm: 'aes-256-cbc',
  secretKey: 'someSecretKeyForAes256CbcIs32Len',
});
```

!!! warning "Ensure correct key size"

    It is important that the `secretKey` size defined matches the algorithm used. I.e.; for `aes-256-cbc`, a key size of 32 is required.

You may set the algorithm and key to be used globally by defining the following env variable:

```bash
LESGO_CRYPTO_ENCRYPTION_ALG=aes-256-cbc
LESGO_CRYPTO_ENCRYPTION_SEC=someSecretKeyForAes256CbcIs32Len
```

## Decrypt

Decrypting a previously encrypted data.

```ts
import { decrypt } from 'lesgo/utils/crypto';

const decryptedStr = decrypt('some-encrypted-string');
// some decrypted string
```