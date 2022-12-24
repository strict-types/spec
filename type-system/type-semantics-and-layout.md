# Type semantics and layout

These chapter introduces and explains three main concepts:

* Type semantics
* Type memory layout
* Type serialization

Each data type in strict encoding is a **semantic** type, which has some **memory layout**.

Two different types may have the same memory layout - but they are still different types. For instance, both date of month and age of a person can be represented by a single byte in memory - but semantically these are two different types, which instances can't be directly compared or ordered (27 aged person is neither can be classified to be "before" or "after" 27 day of any month).

## Type semantics and ids

_Strict encoding types commit to their semantic_, and that commitment represents a **strict type semantic id** (`SemId`). The semantic of a type includes the following information:

1. Memory layout: number apples on a tree representable as `U16` and in the whole world, representable as `U64` are all different types;
2. Type composition, which includes the information about ordering and naming of all fields or variants of the composed types, which is required to distinguish types like `Result<A,B>` from `ControlFlow<A,B>`
3. Type name: two types with the same memory layout and composition still are distinct types. This is required to distinguish number of apples from a number of oranges: both has the same memory layout and composition, but different semantic.

To achieve this, semantic type id is computed as a Black3 hash of:

1. Type name
2. Names of all fields/variants
3. Semantic identifiers for each of the fields/variants

{% hint style="info" %}
Two strict types are equal only and only if their semantic ids are equal - and otherwise, if two semantic type ids are equal, the corresponding types are equal.
{% endhint %}

This approach allows to track any breaking changes in types, and formally verify that new releases of a library do not contain breaking changes in data types, which is important for consensus and networking protocols, data persistence and many other applications.

