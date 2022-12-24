---
description: >-
  Portable & deterministic notation and formal system for algebraic data type
  serialization
---

# Strict encoding

Strict encoding is a formal semantic and notation system for encoding algebraic data types in a portable & deterministic way. Key strict encoding features include:

* portability, or platform-independence: it can be used with different computing architectures, instruction set architectures and in networking systems without modification;
* determinism: any two strict encoded data which differ in their byte sequence will represent semantically-distinct information pieces;
* resource limits: any strict-encoded data can be accessed and computed on a limited-resource systems (embedded systems), since none of atomic encoding data pieces can exceed 64kb in size;
* formally verifiabe: with algebraic data types and the strict encoding API it is possible to formally verify that the data serialization and deserialization is performed according to a schema, as well as prove statements about type semantic and memory layout equivalence and convertability;
* extensibility: strict encoding typing may be extended with new types using provided notation system; this extensions include TLV extensions used for agile networking RPC contracts (like in Lightning network-based protocols, for instance Bifrost).

Strict encoding was developed by Dr Maxim Orlovsky and is a part of a non-profit Ubideco Project ("UBIquitous DEterministic COmputing"), jointly hosted by LNP/BP Standards Association and Cyphernet Association. It had started in 2019 as a part of a client-side-validation paradigm development, targeting distributed computing systems. Its orignats come down to Peter Todd ideas about proofmarshal syste. It was contributed to Ubideco Project first in 2019 (initial MVP) and later in 2021 (release) in form of reference Rust implementation with the aim of standardization, further peer review & future maintenance. Strict encoding development was also supported at early stage (2019 H2-2020 H1) by iFinex Inc (Bitfinex, Tether) and Fulgur Ventures as a part of LNP/BP Association funding.

Today, strict encoding is used in such systems as commit-verify cryptographic schemes, client-side-validation (including RGB protocol), distributed computing (including PRiSM computing), network encodings (especially in Internet2-related protocols), [AluVM](https://www.aluvm.org). Strict encoding formal semantics and its notation syntax (SE/1) were standardized as LNPBP-7 standard in 2021.

This document describes the main design goals behind strict encoding, its formal semantics and notation syntax, covers the main implementations on different platforms and languages, and targets technical people, engineers and "power users" interested in technology details.

## Related materials

* LNPBP-7 standard, defining strict encoding formal semantics and notation syntax
* Reference implementation in Rust: strict\_encoding crate
* API reference for rust strict\_encoding implementation
* Strict TLV extensions used in distributed computing (including Bifrist protocol, Alu computing)
* Language-specific libraries and bindings for strict encoding
* Strict encoding presentation from LNP/BP Association development calls
* Strict encoding notation for RGB data
* Strict encoding notation for Bitcoin data
