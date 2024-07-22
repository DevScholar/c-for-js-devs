# Scalar Types

The following table lists the primitive types in C and their equivalent in JavaScript:

| C                          | JavaScript                | Note        |
| -------------------------- | ------------------------- | ----------- |
| `bool (stdbool.h)`         | `boolean`                 |             |
| `char`                     | `string`                  | See note 1. |
| `char / signed char`       | `number`                  | See note 2. |
| `short int`                | `number`                  |             |
| `int / signed int`         | `number`                  |             |
| `long long int`            | `number`/`bigint`         |             |
| `unsigned long long int`   | `number`/`bigint`         |             |
| `LONG_LONG_MAX (limits.h)` | `Number.MAX_SAFE_INTEGER` |             |
| `unsigned char`            | `number`                  |             |
| `unsigned short int`       | `number`                  |             |
| `unsigned int`             | `number`                  |             |
| `unsigned long long int`   | `number`/`bigint`         |             |
| `unsigned long long int`   | `number`/`bigint`         |             |
| `LONG_LONG_MAX (limits.h)` | `Number.MAX_SAFE_INTEGER` |             |
| `float`                    | `number`/`bigdecimal`     |             |
| `double`                   | `number`/`bigdecimal`     |             |
|                            | `number`                  |             |
| `null`                     | `null`                    |             |
|                            | `undefined`               |             |
|                            |                           | See note 3. |

Notes:

1. [`char`][char.c] in C and [`string`][string.js] in JavaScript have different definitions. In C, a `char` is 1 bytes wide, but in JavaScript, a character is 2 bytes wide and stores the character using the UTF-16 encoding. There is no `char` type equivalent in JavaScript, only `string`. For more information, see the [C `char` documentation][char.c].  
2. There are only three number data type in JavaScript, `number`, which is essentially a floating point number. And the `bigint` type for storing numbers that exceed the range -(2<sup>53</sup> - 1) (`Number.MIN_SAFE_INTEGER`) to 2<sup>53</sup> - 1 (`Number.MAX_SAFE_INTEGER`). and the `bigdecimal` type for storing high-precision decimals.  
3. For historical reasons, JavaScript has two empty data types: `null` and `undefined`. `undefined` denotes a value that was never created, and null denotes a value that was created but intentionally left empty.
See also:

- [Keywords (C Reference)][keywords.c]

[string.js]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#string_type
[char.c]: https://en.cppreference.com/w/c/keyword/char
[Unicode scalar value]: https://www.unicode.org/glossary/#unicode_scalar_value
[keywords.c]: https://en.cppreference.com/w/c/keyword
