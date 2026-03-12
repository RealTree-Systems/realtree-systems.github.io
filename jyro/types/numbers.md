---
layout: default
title: Numbers
parent: Types
has_children: false
has_toc: false
permalink: /jyro/literals/numbers/
nav_order: 100
---

# Numbers

Jyro supports integer and floating-point number literals in decimal, hexadecimal, and binary notation.

## Decimal integers

```jyro
42
-5
0
```

## Floating-point

```jyro
3.14
-0.5
```

## Hexadecimal

Prefixed with `0x` (lowercase only). Digits `a`–`f` are case-insensitive.

```jyro
0xFF
0xff
0x1A3F
```

## Binary

Prefixed with `0b` (lowercase only).

```jyro
0b1010        # 10 in decimal
0b11111111    # 255 in decimal
```

## Type

All number literals produce values of type `number`. There is no separate integer type at runtime - integers and floats are both `number`.
