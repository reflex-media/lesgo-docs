# Helpers

Lesgo! comes in-built with various helper functions available within the `Utils/` namespace.

## Crypto

> `Utils/crypto`

Cryptographic functions using the Crypto npm library.

### Hash

One way hashing of data using SHA-256.

```js
import { hash } from 'Utils/crypto';

return hash('some string to be hashed. This is a one-way hash');
```

### MD5 Hash

One way hashing of data using the faster MD5.

```js
import { hashMD5 } from 'Utils/crypto';

return hash('some string to be hashed by MD5. This is a one-way hash and is faster than hash');
```

### Encrypt

2-way encryption with auto-generated iv and cipher.

```js
import { encrypt } from 'Utils/crypto';

return encrypt('some string to be encrypted that can be decrypted');
```

### Decrypt

Decrypting a previously encrypted data.

```js
import { decrypt } from 'Utils/crypto';

return decrypt('some-encrypted-string');
```

## Generate Uid

> `Utils/generateUid`

Generates random string with an optional prefix and suffix using the Nanoid npm library. Useful for generating unique id.

```js
import generateUid from 'Utils/generateUid';

console.log(generateUid());
// 098f6bcd4621d373cade4

console.log(generateUid({ prefix: 'somePrefix' }));
// somePrefix-621d4bc3cade7d4098f63

console.log(generateUid({ suffix: 'someSuffix' }));
// 7d6af6c3cde214093d4b8-someSuffix

console.log(generateUid({ length: 36 }));
// ce214094b8daf683d4b7d63ccde21403dd93
```

## Email Format Validator

> `Utils/isEmail`

Validates for a valid email format.

```js
import isEmail from 'Utils/isEmail';

console.log(isEmail('john@doe.com'));
// true

console.log(isEmail('some string'));
// false
```