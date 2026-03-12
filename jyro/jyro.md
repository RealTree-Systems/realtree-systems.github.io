---
layout: default
title: Jyro Language Reference
has_children: true
has_toc: true
permalink: /jyro/
nav_order: 10
---

# Jyro Language Reference

Jyro is a scripting language for secure, sandboxed data transformation. Scripts read and write a shared `Data` object using JSON-compatible types (numbers, strings, booleans, objects, arrays, and null) and a standard library of built-in functions.

It is imperative, optionally-typed, with a small keyword set, lambda expressions for higher-order function arguments, and an extensible host function interface that lets the host application expose domain-specific capabilities to scripts.

The runtime is fully isolated. Scripts have no access to files, networks, databases, or host state - only the `Data` object and whatever host functions the host explicitly provides. This makes Jyro safe for executing untrusted scripts, including those supplied by end users in multi-tenant systems.

The language omits features that potentially introduce complexity, ambiguity and security issues: no `eval`, no metaprogramming, no classes or inheritance, no async/await. Standard library functions never mutate their inputs. Evaluation is deterministic and top-to-bottom.

Key characteristics:

- **Data-centric** - all input and output flows through the implicit `Data` context object
- **JSON-native** - works directly with numbers, strings, booleans, objects, arrays, and null
- **Sandboxed** - no file I/O, network access, or system calls
- **Resource-limited** - enforced limits on execution time, statements, loops, and stack depth
- **Immutable standard library** - all library functions return new values and never mutate their inputs
- **User-defined functions** - named, reusable, pure functions with explicit parameters and no access to `Data`
- **Discriminated unions** - declare closed sets of tagged variants with compile-time exhaustive `match`
