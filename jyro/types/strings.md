---
layout: default
title: Strings
parent: Types
has_children: false
has_toc: false
permalink: /jyro/literals/strings/
nav_order: 110
---

# Strings

String literals are delimited by either single quotes (`'`) or double quotes (`"`). Both forms are identical in behaviour.

```jyro
"hello"
'world'
"It's a string"
'She said "hi"'
```

## Escape sequences

The following escape sequences are recognised within string literals:

| Sequence | Meaning |
|----------|---------|
| `\n` | Newline |
| `\r` | Carriage return |
| `\t` | Tab |
| `\\` | Backslash |
| `\"` | Double quote |
| `\'` | Single quote |
| `\0` | Null character |
| `\uXXXX` | Unicode code point (4 hex digits) |

## No string interpolation

Jyro does not support string interpolation. Use the `+` operator for concatenation:

```jyro
var greeting = "Hello, " + name     # Correct
# var greeting = "Hello, ${name}"   # INCORRECT - not supported
```

When either operand of `+` is a string, the other operand is automatically converted to its string representation.

## Regex pattern escaping

Jyro's string lexer only recognises the escape sequences listed above. Any other backslash sequence - including regex metacharacters like `\d`, `\w`, and `\s` - causes a **parse error**. Use character classes instead:

| Instead of | Use |
|------------|-----|
| `\d` | `[0-9]` |
| `\w` | `[a-zA-Z0-9_]` |
| `\s` | `[ \t\n\r]` |
| `\.` | `[.]` |

```jyro
# INCORRECT - parse error: \d is not a recognised escape sequence
var num = RegexMatch(text, "\d+")

# CORRECT - character class reaches the regex engine intact
var num = RegexMatch(text, "[0-9]+")
```
