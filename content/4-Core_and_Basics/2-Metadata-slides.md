---
title: FRAME Metadata
description: FRAME Metadata
---

# FRAME Metadata

---

# FRAME Metadata

## What you will learn:

- What is it? What's on it?<!-- .element: class="fragment" -->
- What is it used for?<!-- .element: class="fragment" -->
- How does it work?<!-- .element: class="fragment" -->
- What's in it?<!-- .element: class="fragment" -->
- What are its shortcomings?<!-- .element: class="fragment" -->
- Evolution of Metadata (V14, V15, V16)<!-- .element: class="fragment" -->
- Hands on.<!-- .element: class="fragment" -->

---

# FRAME Metadata: Introduction

## FRAME Metadata: What is it?

It's an opaque blob of bytes, that when decoded provides:

- The necessary context to encode/decode different chain interaction.<!-- .element: class="fragment" -->
- Documentation about certain chain interactions.<!-- .element: class="fragment" -->
- The value of all Pallet constants.<!-- .element: class="fragment" -->

---

## FRAME Metadata: What is it used for?

- Creating types for other languages (eg: PAPI TS types).<!-- .element: class="fragment" -->
- Creating codecs on the fly.<!-- .element: class="fragment" -->
- Displaying rich information about what the user is about to sign. I.e: prevent blind-signing.<!-- .element: class="fragment" -->
- Creating docs for the chain.<!-- .element: class="fragment" -->
- Detecting compatibility changes.<!-- .element: class="fragment" -->

---

## FRAME Metadata: How does it work?

- Autogenerated by FRAME<!-- .element: class="fragment" -->
- Available via a Runtime-API.<!-- .element: class="fragment" -->

---

## FRAME Metadata: What's in it?

### Type Registry

- A normalized data-structure containing all the type information used for a given chain.<!-- .element: class="fragment" -->
- Its codec looks like this:<!-- .element: class="fragment" -->

Notes:

- Use dev.papi.how to navigate a decoded version of the metadata.

---

## FRAME Metadata: What's in it?

```js
Vector(
  Struct({
    id: compact,
    path: Vector(str),
    params: Vector(Struct({ name: str, type: Option(compact) })),
    def: Enum({
      composite: Vector(field),
      variant: Vector(
        Struct({
          name: str,
          fields: Vector(field),
          index: u8,
          docs,
        }),
      ),
      sequence: compact,
      array: Struct({
        len: u32,
        type: compact,
      }),
      tuple: Vector(compact),
      primitive,
      compact: compact,
      bitSequence,
    }),
    docs: Vector(str),
  }),
);

const field = Struct({
  name: str,
  type: compact,
  typeName: str,
  docs: Vector(str),
});
```

Notes:

- Use dev.papi.how to navigate a decoded version of the metadata.

---

## FRAME Metadata: What's in it?

- Pallets: Storage, Calls, Events, Constants, Errors<!-- .element: class="fragment" -->
- Extrinsic<!-- .element: class="fragment" -->
- Runtime-APIs<!-- .element: class="fragment" -->

Notes:

- Use dev.papi.how to highliht this

---

## FRAME Metadata: What are its shortcomings?

- Autogenerated => leaks implementation details. Examples:<!-- .element: class="fragment" -->
  - Referenda vote.
  - Identity types.
- Native token is not a first class citizen. (Neither the symbol, nor the decimals are part of the metadata).<!-- .element: class="fragment" -->
- It requires a certain level of heuristics to use it properly. Eg: AccountId, AccountId20, etc.<!-- .element: class="fragment" -->

---

## FRAME Metadata: How it evolved?

- Prior to v14: It was not self-describing and incomplete.<!-- .element: class="fragment" -->
- v14 was the first self-describing version. However its still missing things: runtime APIs, outters, certain extrinsic<!-- .element: class="fragment" -->
  information. `metadata_metadata` runtime API will always return metadata v14.
- Metadata v15: exposes runtime-apis, outters, and better types for extrinsic.<!-- .element: class="fragment" -->
- Metadata v16: is able to highlight deprecated methods, supports extrinsic v5 and versioned transaction-extensions.<!-- .element: class="fragment" -->
