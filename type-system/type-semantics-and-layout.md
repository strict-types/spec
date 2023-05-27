# Type semantics and layout

This chapter introduces and explains three main concepts:

* Type semantics
* Type memory layout
* Type serialization

Each data type in strict types is a **semantic** type, which has some **memory layout**.

Two different types may have the same memory layout - but they are still different types. For instance, both date of month and age of a person can be represented by a single byte in memory - but semantically these are two different types, which instances can't be directly compared or ordered (a person aged 27 is neither can be classified to be "before" or "after" 27 day of any month).

## Type semantics and ids

_Strict types commit to data semantics_, and that commitment represents a **strict type semantic id** (`SemId`). The semantics of a type includes the following information:

1. Memory layout: number of apples on a tree representable as `U16` and in the whole world, representable as `U64` are all different types;
2. Type composition, which includes the information about ordering and naming of all fields or variants of the composed types, is required to distinguish types like `Result<A,B>` from `ControlFlow<A,B>`
3. Type name: two types with the same memory layout and composition still are distinct types. This is required to distinguish a number of apples from a number of oranges: both have the same memory layout and composition, but different semantics.

To achieve this, semantic type id is computed as a SHA2-256 hash of:

1. Type name
2. Names of all fields/variants
3. Semantic identifiers for each of the fields/variants

{% hint style="info" %}
Two strict types are equal only and only if their semantic ids are equal - and otherwise, if two semantic type ids are equal, the corresponding types are equal.
{% endhint %}

This approach allows tracking any breaking changes in types, and formally verifies that new releases of a library do not contain breaking changes in data types, which is important for consensus and networking protocols, data persistence and many other applications.

## Type libraries and type system

Types are structured into type libraries. The type library is a collection of data types, which may depend on other libraries. The library commits to the semantic type ids of its types. Types which use references to types from other libraries commit to both the library id and semantic id of the external types.

Circular dependencies between types from different are not allowed, thus any set of strict types (type library or type system) always represents a DAG.
