---
description: Portable & deterministic notation and formal system for data serialization
---

# Strict encoding overview

Strict encoding is a formal semantic and notation system for producing portable & deterministically encoded data, developed by Dr Maxim Orlovsky, Pandora Core AG, and maintained by LNP/BP Standards Association. Key strict encoding features include:

* portability, or platform-idependence: it can be used with different computing architecutres, instruction set architectures, networking systems without modificaiton;
* 
String encoding was created as a part of a client-side-validation paradigm omplementation, designed for for distributed computing and originating from the earlier works of Peter Todd. Today, strict encoding is used in such systems as commit-verify cryptographic schemes, client-side-validation, distributed computing \(including PRiSM computing\), network encodings \(especially in Internet2-related protocols\).

Strict encoding has being developed since 2019. It was contributed to LNP/BP Standards Association first in 2019 \(initial MVP\) and later in 2021 \(release\) in form of reference Rust implementation with the aim of standardization, further peer review & future maintenance. Strict encoding development was also supported at early stage \(2019 H2-2020 H1\) by iFinex Inc and Fulgur Ventures as a part of LNP/BP Association funding.

Strict encoding formal semantics and its notation syntax \(SE/1\) were standardized as LNPBP-7 standard in 2021.

This blue book paper describes the main design goals behind strict encoding, its formal semantics and notation syntax, covers the main implementations on different platforms and languages, and targets technical people, engineers and "power users" interested in technology details.

## Related materials

* LNPBP-7 standard, defining strict encoding formal semantics and notation syntax
* Reference implementation in Rust: strict\_encoding crate
* API reference for rust strict\_encoding implementation
* Strict TLV extensions used in distributed computing \(including Bifrist protocol, Alu computing\)
* Language-specific libraries and bindings for strict encoding
* Strict encoding presentation from LNP/BP Association development calls
* Strict encoding notation for RGB data
* Strict encoding notation for Bitcoin data



