# Available Helpers

Lesgo! comes in-built with various helper functions available. These can be accesssed within the `lesgo/utils/` namespace.

## Generate uid

Generates random string with an optional prefix and suffix using the Nanoid npm library. Useful for generating unique id.

```Ts
import { generateUid } from 'lesgo/utils';

const uid = generateUid());
// 098f6bcd4621d373cade4

const prefixUid = generateUid({ prefix: "somePrefix" }));
// somePrefix-621d4bc3cade7d4098f63

const uidSuffix = generateUid({ suffix: "someSuffix" }));
// 7d6af6c3cde214093d4b8-someSuffix

const uidLimited = generateUid({ length: 36 }));
// ce214094b8daf683d4b7d63ccde21403dd93
```

## Email format validator

Validates for a valid email format.

```ts
import { isEmail } from 'lesgo/utils';

isEmail('john@doe.com'));
// true

isEmail('some string'));
// false
```

## Decimal number validator

Validates for a valid decimal number in any format such as `##.##`.

```ts
import isDecimal from 'lesgo/utils/isDecimal';

isDecimal(5.4);
// true

isDecimal(5);
// false
```

## Empty value validator

Validates if a value is either an `undefined`, or `null`, or empty string, or an empty array, or an object without keys.

```ts
import isEmpty from 'lesgo/utils/isEmpty';

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

## Field validator

Validates parameters received to ensure the data is of the right type and exists.

```ts
import { validateFields } from 'lesgo/utils';

const validFields = [
  { key: 'someString', type: 'string', required: true },
  { key: 'someObject', type: 'object', required: true },
  { key: 'someNumber', type: 'number', required: true },
  { key: 'someDecimal', type: 'decimal', required: true },
  { key: 'someEmail', type: 'email', required: true },
  { key: 'someArray', type: 'array', required: true },
  {
    key: 'someEnum',
    type: 'enum',
    enumValues: ['abc', 'def', 'xyz'],
    required: true,
  },
];

const toValidate1 = {
  someString: 'The cat',
  someObject: {
    objKey1: 'one',
    objKey2: 'two',
  },
  someNumber: 123,
  someDecimal: 0.123,
  someEmail: 'email@mail.com',
  someArray: ['abc', 'def', 'xyz'],
  someEnum: 'def',
};

const validated = validateFields(toValidate1, validFields);

/*
{
  someString: 'The cat',
  someObject: {
    objKey1: 'one',
    objKey2: 'two',
  },
  someNumber: 123,
  someDecimal: 0.123,
  someEmail: 'email@mail.com',
  someArray: ['abc', 'def', 'xyz'],
  someEnum: 'def',
}
*/
```

The example above will pass and return with the exact validated fields in `toValidate1`.

The next example will throw an Error.

```ts
const toValidate2 = {
  ...toValidate1,
  someString: 123,
}

const validated = validateFields(toValidate2, validFields));

/**
new LesgoException(
  `Invalid type for 'someString', expecting 'string'`,
  `lesgo.utils.validateFields::INVALID_TYPE_NAME`
)
*/
```
