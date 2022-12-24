# Data type algebra

## Fundamental types

Fundamental types are types built-in in strict encoding and not derived from any other types. These types include:

1. Unit type `()`
2. Byte type `Byte`
3. Integer numbers (signed `I_`, unsigned `U_` and natural `N_`)
4. Floating-point numbers `F_`
5. UTF-8 character `Utf8`

Byte type is introduced due to the fact that it semantically different from a 8-bit signed or unsigned integer: it does not contain information about sign and may not be representative with an integer at all.

While Unicode character type can be expressed expressed as a composite type, it will be very verbose expression (union with 256 variants), so for the practical purposes (to reduce the complexity of types which use Unicode strings) it was decided to built it in.

Strict encoding has reserved place for 55 more types, which may be introduced in a future to represent more floating-point integer encodings, Unicode variants etc. At the present moment use of that type identifiers would result in encoding/decoding failure.

### Integers

Integer types are named using a single upper case latter specifying set of integers used in type (`U` for unsigned, `I` for signed and `N` for natural non-zero integers) followed by a decimal number of bits in the type encoding (like `U8` or `I1024`).

Strict encoding covers integer types of different size in two ranges:

| Bit size    | Step                     |                    |
| ----------- | ------------------------ | ------------------ |
| 8 to 256    | 8 bits (i.e. 1 byte)     | from U8 up to U256 |
| 272 to 4352 | 128 bits (i.e. 16 bytes) | from U272 to U4352 |

In total, there are 64 different types for unsigned integers, 64 types for signed and 64 types for non-zero integers, giving 194 possible integer types in total. Not all of these types have a representation in all of the supported languages, so below we give a list of integer types which can be represented in Rust:

| Sten type                   | Bytes | Encoding | Rust type                                      |
| --------------------------- | ----- | -------- | ---------------------------------------------- |
| `U8` / `I8` / `N8`          | 1     | N/A      | `u8` / `i8` / `NonZeroU8`                      |
| `U16` / `I16` / `N16`       | 2     | LE       | `u16` / `i16` / `NonZeroU16`                   |
| `U24` / `I24` / `N24`       | 3     | LE       | `amplify_num::u24` / `i14` / `NonZeroU24`      |
| `U32` / `I32` / `N32`       | 4     | LE       | `u32` / `i32` / `NonZeroU32`                   |
| `U48` / `I48` / `N48`       | 6     | LE       | `amplify_num::u48` / `i48` / `NonZeroI48`      |
| `U64` / `I64` / `N64`       | 8     | LE       | `u64` / `i64` / `NonZeroU64`                   |
| `U128` / `I128` / `N128`    | 16    | LE       | `u128` / `i128` / `NonZeroU128`                |
| `U256` / `I256` / `N256`    | 32    | LE       | `amplify_num::u256` / `i256` / `NonZero256`    |
| `U512` / `I512` / `N512`    | 64    | LE       | `amplify_num::u512` / `i512` / `NonZero512`    |
| `U1024` / `I1024` / `N1024` | 128   | LE       | `amplify_num::u1024` / `i1024` / `NonZero1024` |

### Floating-point numbers

Strict encoding supports the following floating number encodings:

| Sten type | Bytes | Encoding          | Rust type                                  |
| --------- | ----- | ----------------- | ------------------------------------------ |
| `R16B`    | 2     | bfloat16          | `bfloat::bf16`                             |
| `R16`     | 2     | IEEE Half         | `amplify_apfloat::ieee::Half`              |
| `R32`     | 4     | IEEE Single       | `amplify_apfloat::ieee::Single`            |
| `R64`     | 8     | IEEE Double       | `amplify_apfloat::ieee::Double`            |
| `R80`     | 10    | IEEE X87 Extended | `amplify_apfloat::ieee::X87DoubleExtended` |
| `R128`    | 16    | IEEE Quad         | `amplify_apfloat::ieee::Quad`              |
| `R256`    | 32    | IEEE Oct          | `amplify_apfloat::ieee::Oct`               |

Strict encoding has 54 more type identifiers reserved for possible use by future floating-point number encodings (like Tappered float etc); at the present moment use of that type identifiers would result in encoding/decoding failure.

## Type composition

Strict encoding uses generalized algebraic data types (GADT). This means that new types can be composed out of primitive types via following fundamental morphisms:

| Name                               | Syntax form                                                                      | Max no of elements / fields / variants |
| ---------------------------------- | -------------------------------------------------------------------------------- | -------------------------------------- |
| Product types (structure, tuple)   | `• , •` _or_ `(• , •)`                                                           | 255                                    |
| <p>Sum types <br>(union, enum)</p> | `• \| •` _or_ `(• \| •)`                                                         | 255                                    |
| Mapping (function)                 | <p><code>• -> •</code> <em>or</em> <br><em></em><code>(•) ->^U..D (•)</code></p> | From `U` to `D`, up to 2^64            |
| Fixed array                        | `[•^N]`                                                                          | `N`, up to 2^16-1                      |
| Dynamic array                      | `[•]` _or_ `[• ^U..D]`                                                           | From `U` to `D`, up to 2^64            |
| Dynamic set                        | `{•}` _or_ `[• ^U..D]`                                                           | From `U` to `D`, up to 2^64            |

### Enums

Enums are a special case of unit type in which each variant is represented by a `Byte` value.

### Dynamic collections

Fundamental morphisms can be used to build more advanced types, like dynamic maps and dynamic arrays of tuples

|                         | Syntax form                                                                 | No of elements              |
| ----------------------- | --------------------------------------------------------------------------- | --------------------------- |
| Dynamic map             | `{• -> ^U..D •}`                                                            | From `U` to `D`, up to 2^64 |
| Dynamic array of tuples | <p><code>[•, • ^U..D]</code> <em>or</em><br><code>[(•, •) ^U..D]</code></p> | From `U` to `D`, up to 2^64 |

Construction `^U..D` used in type expression specifying minimum and maximum size of a dynamic collection is called _confinement bounds_. It can be seen as an upper and lower indexes on the possible number of elements, i.e. type definition `[Byte ^ 1..20]` can be read as $$\bigotimes^1_{20} byte$$ and means product type with dynamic number of fields, from 1 to 20 max, where each field is a byte - or, in more common terms, an byte array of dynamic size which can't have less than one byte - and can't grow larger than 20 bytes.

For simplifying syntax strict encoding provides comprehensions and defaults for specifying the confinement bounds:

| Comprehension                                      | Expands to        | Comment                                                                 |
| -------------------------------------------------- | ----------------- | ----------------------------------------------------------------------- |
| `[•]`                                              | `[• ^ 0..0xFFFF]` | Default number of elements in confined collections is from zero to 2^16 |
| <p><code>[•+]</code>or<br><code>[•^1..]</code></p> | `[• ^ 1..0xFFFF]` | Collection which must contain at least  one item                        |
| `[•^U]`                                            | `[• ^ U..0xFFFF]` | Collection with minimum of `U` items                                    |
| `[•^..D]`                                          | `[• ^ 0..D]`      | Collection having maximum of `D` items                                  |

Please note that type expression of `[•^N..N]`is not allowed, since it means "dynamic" collection with a fixed number of items, which is nonsense, so please use `[•^N]` instead.

### Optionals

A special case of union type of frequent use is an optional monad, which may contain some type or be `None`. In strict encoding there is a special comprehension for writing an optional: `T?`, which is an equivalend ot writing `(()|T)`.

### Types provided by the standard library

Most frequently used types are provided by a strict encoding standard library `StdLib`:

| Sten type | Type definition          | Rust type from amplify library |
| --------- | ------------------------ | ------------------------------ |
| `Bytes`   | `[Byte]`                 | `SmallVec<u8>`                 |
| `Blob`    | `[Byte^..0xFFFFFF]`      | `MediumVec<u8>`                |
| `String`  | `[Utf8]`                 | `SmallString`                  |
| `Text`    | `[Utf8^..0xFFFFFF]`      | `MediumString`                 |
| `Ascii`   | type definition too long | `AsciiChar`                    |



\
