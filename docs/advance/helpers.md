# Helpers

Lesgo! comes in-built with various helper functions available within the `Utils/` namespace.

## Crypto

> `Utils/crypto`

Cryptographic functions using the Crypto npm library.

### Hash

One way hashing of data using SHA-256.

```js
import { hash } from "Utils/crypto";

const hashedString = hash("some data to hash");

// 6a2da20943931e9834fc12cfe5bb47bbd9ae43489a30726962b576f4e3993e50
```

### MD5 Hash

One way hashing of data using the faster MD5.

```js
import { hashMD5 } from "Utils/crypto";

const hashedMD5String = hashMD5("some data to hash");

// 527d738baba016840c3a33f2790845dd
```

!!! warning "Warning"

    While hashMD5 is fast, it is not at all recommended as it is considered a weak 1-iteration hash with no salt.

### Encrypt

2-way encryption with auto-generated iv and cipher.

```js
import { encrypt } from "Utils/crypto";

const encryptedStr = encrypt(
  "some string to be encrypted that can be decrypted"
);

//
```

### Decrypt

Decrypting a previously encrypted data.

```js
import { decrypt } from "Utils/crypto";

const decryptedStr = decrypt("some-encrypted-string");

//
```

## Generate Uid

> `Utils/generateUid`

Generates random string with an optional prefix and suffix using the Nanoid npm library. Useful for generating unique id.

```js
import generateUid from "Utils/generateUid";

const uid = generateUid());

// 098f6bcd4621d373cade4

const prefixUid = generateUid({ prefix: "somePrefix" }));

// somePrefix-621d4bc3cade7d4098f63

const uidSuffix = generateUid({ suffix: "someSuffix" }));

// 7d6af6c3cde214093d4b8-someSuffix

const uidLimited = generateUid({ length: 36 }));

// ce214094b8daf683d4b7d63ccde21403dd93
```

## Email Format Validator

> `Utils/isEmail`

Validates for a valid email format.

```js
import isEmail from "Utils/isEmail";

isEmail("john@doe.com"));

// true

isEmail("some string"));

// false
```

## Decimal Number Validator

> `Utils/isDecimal`

Validates for a valid decimal number in any format such as `##.##`.

```js
import isDecimal from "Utils/isDecimal";

isDecimal(5.4);

// true

isDecimal(5);

// false
```

## Empty Value Validator

> `Utils/isEmpty`

Validates if a value is either an `undefined`, or `null`, or empty string, or an empty array, or an object without keys.

```js
import isEmpty from "Utils/isEmpty";

isEmpty(undefined);
isEmpty(null);
isEmpty({});
isEmpty([]);
isEmpty("");

// true

isEmpty("test");
isEmpty(0);

// false
```

## Field Validator

> `Utils/validateFields`

Validates parameters received to ensure the data is of the right type and exists.

```js
import validateFields from "Utils/validateFields";

const validFields = [
  { key: "someString", type: "string", required: true },
  { key: "someObject", type: "object", required: true },
  { key: "someNumber", type: "number", required: true },
  { key: "someDecimal", type: "decimal", required: true },
  { key: "someEmail", type: "email", required: true },
  { key: "someArray", type: "array", required: true },
  {
    key: "someEnum",
    type: "enum",
    enumValues: ["abc", "def", "xyz"],
    required: true,
  },
  { key: "someFunc", type: "function", required: true },
  { key: "someJson", type: "json", required: true },
  { key: "someObjectCollection", type: "object", required: true, isCollection: true },
  {
    key: "someEnumCollection",
    type: "enum",
    enumValues: ["abc", "def", "xyz"],
    required: true,
    isCollection: true
  },
];

const toValidate1 = {
  someString: "The cat",
  someObject: {
    objKey1: "one",
    objKey2: "two",
  },
  someNumber: 123,
  someDecimal: 0.123,
  someEmail: "email@mail.com",
  someArray: ["aaa", "bbb", "ccc"],
  someEnum: "def",
  someFunc: () => 'bar',
  someJson: "{\"foo\":\"bar\",\"isTest\":1}",
  someObjectCollection: [{ "foo": "bar", "isTest": 1 }, [{ "another": "object" }],
  someEnumCollection: ["def", "abc", "abc"],
};

const validated = validateFields(toValidate1, validFields));

/**
{
  someString: "The cat",
  someObject: {
    objKey1: "one",
    objKey2: "two",
  },
  someNumber: 123,
  someDecimal: 0.123,
  someEmail: "email@mail.com",
  someArray: ["aaa", "bbb", "ccc"],
  someEnum: "def",
  someFunc: () => 'bar',
  someJson: "{\"foo\":\"bar\",\"isTest\":1}",
  someObjectCollection: [{ "foo": "bar", "isTest": 1 }, [{ "another": "object" }],
  someEnumCollection: ["def", "abc", "abc"],
}
*/
```

The example above will pass and return with the exact validated fields in `toValidate1`.

The next example will throw an Error.

```js
const toValidate2 = {
  ...toValidate1,
  someString: 123,
}

const validated = validateFields(toValidate2, validFields));

/**
new LesgoException(
  `Invalid type for 'someString', expecting 'string'`,
  `${FILE}::INVALID_TYPE_NAME`
)
*/
```

## Prep Insert SQL Parameter

> `Utils/prepSQLInsertParams`

Takes in data and returns a nicely formatted response to be used for `Utils/db.insert()`.

```js
prepSQLInsertParams(
  params: Object, // data to insert to db (matching key to db column)
  columns: Object // valid db column names
): {
  insertColumns,  // comma-separated db column field names
  insertValues,   // comma-separated field values in prepared statement form i.e; :username, :id, etc
  insertFields    // data object to insert
};
```

**Usage**

```js
import prepSQLInsertParams from "Utils/prepSQLInsertParams";

const params = {
  username: "John",
  email: "john.doe@gmail.com",
};

const validFields = [
  { key: "username", type: "string", required: true },
  { key: "email", type: "string", required: true },
];

const { insertColumns, insertValues, insertFields } = prepSQLInsertParams(
  params,
  validFields
);
```

See usage with [`Utils/db.insert()`](../database/rds-aurora.md#inserting-a-single-record).

## Prep Update SQL Parameter

> `Utils/prepSQLUpdateParams`

Takes in data and returns a nicely formatted response to be used for `Utils/db.update()`.

```js
prepSQLUpdateParams(
  params: Object, // data to update to db (matching key to db column)
  columns: Object // valid db column names
): {
  updateColumnValues, // comma-separated column=:value pair
  wherePrimaryKey,    // primary key (based on default table.id)
  updateFields        // data object to update
};
```

**Usage**

```js
import prepSQLUpdateParams from "Utils/prepSQLUpdateParams";

const params = {
  id: 1,
  username: "John",
  email: "john.doe@gmail.com",
};

const validFields = [
  { key: "id", type: "number", required: true },
  { key: "username", type: "string", required: true },
  { key: "email", type: "string", required: true },
];

const { updateColumnValues, wherePrimaryKey, updateFields } =
  prepSQLUpdateParams(params, validFields);
```

See usage with [`Utils/db.update()`](../database/rds-aurora.md#updating-an-existing-record).
