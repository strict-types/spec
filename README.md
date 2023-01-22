---
description: Portable & deterministic formalism for algebraic data types
---

# Strict types

_Strict types_ is a formal semantic and notation system for encoding algebraic data types in a portable & deterministic way.

Why do we need another algebraic data types if we have one in Haskell, Idris etc? Well, since these systems focus on mathematical correctness and a quite far from the real existing computing platforms. **Strict types**, on the other hand, **apply algebraic data types to the existing computing platforms, networking and programming languages, without sacrificing formal analytical properties**. For instance, in strict types any collection must have a defined bounds for the minimum and maximum number of elements the collection may hold, which becomes a part of a type itself. This allows to precisely predict resource requirements at a compile time and avoid runtime exceptions, as well as apply formal analysis in a platform-specific manner.

Key strict encoding features include:

* portability, or platform-independence: it can be used with different computing architectures, instruction set architectures and in networking systems without modification;
* determinism: any two strict encoded data which differ in their byte sequence will represent semantically-distinct information pieces;
* resource limits: any strict-encoded data can be accessed and computed on a limited-resource systems (embedded systems), since none of atomic encoding data pieces can exceed 64kb in size;
* formally verification: with algebraic data types and the strict encoding API it is possible to formally verify that the data serialization and deserialization is performed according to a schema, as well as prove statements about type semantic and memory layout equivalence and convertibility;
* extensibility: strict encoding typing may be extended with new types using provided notation system; this extensions include TLV-like extensions used for agile networking RPC contracts.

Strict types is a functional and close to the bare metal at the same time, which puts it into a segment of serialization languages which was not populated before:&#x20;

<figure><img src=".gitbook/assets/strict-encoding-box-black (1).png" alt=""><figcaption><p>Strict encoding on landscape of other languages and encoding notations</p></figcaption></figure>

Strict types were developed by Dr Maxim Orlovsky and is a part of a Swiss non-profit UBIDECO Institute (UBIDECO stands for "Ubiquitous Deterministic Computing"), launched by LNP/BP Standards Association, CypherNet DAO and Pandora Prime Inc at the end of 2022. Its roots go back to 2019 when client-side-validation paradigm was development by LNP/BP Standards Association, targeting distributed computing and originating from Peter Todd ideas about proofmarshal systems. Strict types development was supportedby iFinex Inc (Bitfinex, Tether), Fulgur Ventures, Pandora Prime Inc, DIBA Inc through of LNP/BP Association funding, as well as personal Dr Maxim Orlovsky funds.

Today, strict types are used in such systems as commit-verify cryptographic schemes, client-side-validation (including RGB protocol), distributed computing (including PRiSM computing), network encodings (especially in Internet2-related protocols), [AluVM](https://www.aluvm.org).

This document describes the main design goals behind strict types, its formal semantics and notation syntax, covers the main implementations on different platforms and languages, and targets technical people, engineers and "power users" interested in technology details.
