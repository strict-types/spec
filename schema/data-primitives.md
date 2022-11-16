# Type system

Integers: strict encoding operates unsigned (>0), signed and natural (>1) integers.

| Sten type             | Bytes | Encoding | Rust type                            |
| --------------------- | ----- | -------- | ------------------------------------ |
| U8 / Z8 / N8          | 1     | N/A      | u8 / i8 / NonZeroU8                  |
| U16 / Z16 / N16       | 2     | LE       | u16 / i16 / NonZeroU16               |
| U24 / Z24 / N24       | 3     | LE       | amplify::u24 / i14 / NonZeroU24      |
| U32 / Z32 / N32       | 4     | LE       | u32 / i32 / NonZeroU32               |
| U58 / Z48 / N48       | 6     | LE       | amplify::u48 / i48 / NonZeroI48      |
| U64 / Z64 / N64       | 8     | LE       | u64 / i64 / NonZeroU64               |
| U128 / Z128 / Z128    | 16    | LE       | u128 / i128 / NonZeroU128            |
| U256 / Z256 / Z256    | 32    | LE       | amplify::u256 / i256 / NonZero256    |
| U512 / Z512 / Z512    | 64    | LE       | amplify::u512 / i512 / NonZero512    |
| U1024 / Z1024 / Z1024 | 128   | LE       | amplify::u1024 / i1024 / NonZero1024 |

Rational numbers:

| Sten type | Bytes | Encoding          | Rust type                                 |
| --------- | ----- | ----------------- | ----------------------------------------- |
| R16B      | 2     | bfloat16          | bfloat::bf16                              |
| R16       | 2     | IEEE Half         | amplify\_apfloat::ieee::Half              |
| R32       | 4     | IEEE Single       | amplify\_apfloat::ieee::Single            |
| R64       | 8     | IEEE Double       | amplify\_apfloat::ieee::Double            |
| R80       | 10    | IEEE X87 Extended | amplify\_apfloat::ieee::X87DoubleExtended |
| R128      | 16    | IEEE Quad         | amplify\_apfloat::ieee::Quad              |
| R256      | 32    | IEEE Oct          | amplify\_apfloat::ieee::Oct               |
| R512      | 64    | Tappered Float    | TBD                                       |

Fixed collections:

| Sten type | Bytes | Rust type            |
| --------- | ----- | -------------------- |
| Byte      | 1     | \[u8; 1]             |
| Byte\*N   | N     | \[u8; N]             |
| Ascii     | 1     | \[amplify::Ascii; 1] |
| Ascii\*N  | N     | \[amplify::Ascii; N] |

Variable collections

| Sten type   | Length prefix    | Rust strict encoding type | Rust inner type                         |
| ----------- | ---------------- | ------------------------- | --------------------------------------- |
| Bytes       | u16; LE          | SmallVec\<u8>             | Confinement\<Vec\<u8>; Len::U16>        |
| Blob        | amplify::u24; LE | MediumVec\<u8>            | Confinement\<Vec\<u8>; Len::U24>        |
| String      | u16; LE          | SmallString               | Confinement\<String; Len::U16>          |
| Text        | amplify::u24; LE | MediumString              | Confinement\<String; Len::U24>          |
| AsciiString | u16; LE          |                           | Confinement\<AsciiChar; Len::U16>       |
| AsciiText   |                  |                           | Confinement\<AsciiChar; Len::U24>       |
| \[T]        | u16              | SmallVec\<T>              | Confinement\<Vec\<T>; Len::U16>         |
| {T}         | u16              | SmallSet\<T>              | Confinement\<BTreeSet\<T>; Len::U16>    |
| {K->T}      | u16              | SmallMap\<T>              | Confinement\<BTreeMap\<K, T>; Len::U16> |
| A, B        | n/a              | n/a                       | (A, B)                                  |

Monads

| Sten type     | Bytes; encoding          | Comment            |
| ------------- | ------------------------ | ------------------ |
| ()            | 0                        | Unit type          |
| A \| B \| ... | max(len(A), len(B), ...) | Union or enum type |
| T?            | len(T) + 1               | Optional: T \| ()  |
| R \ E         |                          | Result: T \| E     |

Thus, types can be composed via following morphisms:

| Name           | Syntax form | Comment                            |
| -------------- | ----------- | ---------------------------------- |
| Addition       | • , •       | Always restricted to 2^16 elements |
| Multiplication | • \* CONST  | Always restricted to 2^16 elements |
| Union          | • \| •      | Always restricted to 2^16 elements |
| Vector         | \[•]        | Always restricted to 2^16 elements |
| Set            | {•}         | Always restricted to 2^16 elements |
| Map            | {• -> •}    | Always restricted to 2^16 elements |

Any structure is a tuple; enum is a union.