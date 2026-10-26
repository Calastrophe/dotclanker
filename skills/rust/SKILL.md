---
name: rust
description: Use when writing, reviewing, or refactoring Rust code.
---

# Rust

## Scope and conventions

Apply the repository's conventions first. Where they leave a choice, favor readable, maintainable code and useful automation. Do not add complexity just for visual consistency.

Keep application code direct and design reusable library APIs for flexibility and compatibility. Treat public API rules as requirements for libraries and strong recommendations for applications.

Run the configured formatter and follow public Rust conventions unless project guidance conflicts. Read only applicable rules and respect their scope and exceptions. Do not apply public API rules to local implementation details.

Rule prefixes identify the categories below. For public library signatures, read the relevant `api-` rules.

## Modules and imports

- [org-leaf-module](rules/org-leaf-module.md) - Name a leaf module after its main user-defined type when it has one.
- [org-non-leaf](rules/org-non-leaf.md) - Keep non-leaf modules focused on declarations and re-exports, with a small amount of glue code when useful.
- [org-glob-imports](rules/org-glob-imports.md) - Prefer explicit imports; use glob imports only when their scope and origin remain clear.
- [org-imports-per-module](rules/org-imports-per-module.md) - Keep imports from different modules in separate use statements.
- [org-local-trait-import](rules/org-local-trait-import.md) - Import a trait inside a function when its use is local.
- [org-qualify-functions](rules/org-qualify-functions.md) - Qualify free function calls with their module when that improves clarity.
- [org-import-types](rules/org-import-types.md) - Import types and traits for unqualified use unless qualification resolves ambiguity or improves clarity.

## Formatting

- [`fmt-automatic`](rules/fmt-automatic.md) - Use the project's formatter and formatting settings.
- [fmt-skip](rules/fmt-skip.md) - Skip formatting only for generated code or deliberate alignment that aids reading.

## Ownership and function APIs

- [own-avoid-rc-arc](rules/own-avoid-rc-arc.md) - Prefer borrowing and unique ownership; use Rc or Arc when they avoid an unreasonably complex ownership design.
- [own-borrow-deep-copy](rules/own-borrow-deep-copy.md) - Borrow instead of deep-cloning when ownership is not needed.
- [api-flexible-borrows](rules/api-flexible-borrows.md) - Accept references to data rather than references to its container or wrapper.
- [api-iterator-return](rules/api-iterator-return.md) - Prefer iterator return types when callers benefit from choosing how to consume results.
- [api-iterator-params](rules/api-iterator-params.md) - Reusable library functions can accept iterators when they only need to traverse inputs.
- [api-into-params](rules/api-into-params.md) - Consider impl Into parameters for ergonomic library APIs, but account for complexity and code size.
- [api-enum-bool](rules/api-enum-bool.md) - Use a named enum when a boolean parameter would be unclear at the call site.
- [api-error-reporting](rules/api-error-reporting.md) - Choose Option, Result, or panic according to what the caller can recover from.

## Types and traits

- [type-newtype](rules/type-newtype.md) - Use a newtype when a value needs distinct meaning, validation, or room for its own API.
- [type-named-fields](rules/type-named-fields.md) - Prefer named fields when tuple positions are not obvious.
- [trait-add-impls](rules/trait-add-impls.md) - Add trait implementations in step with expected use, with more anticipation in foundational libraries.
- [trait-standard](rules/trait-standard.md) - Implement a standard trait when its contract matches the behavior.
- [trait-derive](rules/trait-derive.md) - Use a derive when it expresses the intended implementation.
- [trait-generics-vs-dyn](rules/trait-generics-vs-dyn.md) - Prefer generics in reusable library inputs unless code size or runtime selection favors dyn.
- [trait-or-inherent](rules/trait-or-inherent.md) - Define a trait for polymorphism or extension, not merely to group methods.
- [trait-extension](rules/trait-extension.md) - Use Ext for method-only extension traits and keep them focused.
- [trait-common-impls](rules/trait-common-impls.md) - Implement common traits when their contracts fit the type and likely use.

## Concurrency and macros

- [conc-poison](rules/conc-poison.md) - Do not inspect or use protected data after mutex poisoning.
- [macro-prefer-functions](rules/macro-prefer-functions.md) - Prefer ordinary Rust constructs, then declarative macros, before heavier code generation.

## Naming and documentation

- [name-methods](rules/name-methods.md) - Follow standard Rust naming conventions for methods and conversions.
- [name-lifetimes](rules/name-lifetimes.md) - Use descriptive lifetime names when they clarify nontrivial relationships.
- [name-generic-params](rules/name-generic-params.md) - Keep generic parameter names short and related to their role.
- [name-constructors](rules/name-constructors.md) - Use conventional names for constructors and conversions.
- [name-abbreviations](rules/name-abbreviations.md) - Avoid abbreviations that readers outside the project may not know.
- [doc-public](rules/doc-public.md) - Document public APIs, panic cases, unsafe invariants, and the module or crate where useful.

## Other language features

- [`feature-unsafe`](rules/feature-unsafe.md) - Prefer safe code; isolate and justify `unsafe` code when it is necessary.
- [feature-toolchain](rules/feature-toolchain.md) - Follow the repository's stable or nightly toolchain policy.
- [feature-accessors](rules/feature-accessors.md) - Name getters, setters, and scoped closure accessors consistently.
- [feature-api-pairs](rules/feature-api-pairs.md) - Provide companion APIs such as is_empty when the main method implies them.
- [feature-doc-hidden](rules/feature-doc-hidden.md) - Treat doc-hidden items you do not own as private.
- [feature-const](rules/feature-const.md) - Add const to public functions only when it is a durable part of the API.
- [feature-operator-overload](rules/feature-operator-overload.md) - Overload operators only for their expected mathematical or logical meaning.
- [feature-non-exhaustive-enums](rules/feature-non-exhaustive-enums.md) - Use non_exhaustive for public enums that may gain variants.
- [`feature-non-exhaustive-structs`](rules/feature-non-exhaustive-structs.md) - Use `#[non_exhaustive]` for public structs with public fields when compatibility matters.
- [`feature-must-use-functions`](rules/feature-must-use-functions.md) - Mark function results `#[must_use]` when ignoring them can be a bug.
- [`feature-must-use-types`](rules/feature-must-use-types.md) - Mark result types `#[must_use]` when callers need to consume them.
- [feature-pre-main](rules/feature-pre-main.md) - Avoid implicit code execution before main.

## Implementation

- [`impl-inference`](rules/impl-inference.md) - Add type annotations where inference makes important values hard to understand.
- [impl-match-before-unwrap](rules/impl-match-before-unwrap.md) - Use control flow that proves an Option or Result case before accessing its value.
- [`impl-expect`](rules/impl-expect.md) - Use `expect` with a reason when an invariant makes a panic necessary.
- [impl-match-non-exhaustive](rules/impl-match-non-exhaustive.md) - Make handling of known and future enum variants clear.
- [impl-assert-context](rules/impl-assert-context.md) - Add useful context to assertions when it helps explain failures.
- [`impl-iterator-readability`](rules/impl-iterator-readability.md) - Use direct loops when they make complex iterator processing easier to follow.
- [`impl-catch-unwind`](rules/impl-catch-unwind.md) - Do not use `catch_unwind` as ordinary error handling.
- [impl-format-strings](rules/impl-format-strings.md) - Use format strings for multi-part string construction and literal substitution.
