---
layout: default
title: Property and Index Access
parent: Jyro Language Reference
has_children: false
has_toc: true
permalink: /jyro/property-access/
nav_order: 90
---

# Property and Index Access

Jyro provides dot notation and bracket notation for accessing properties and elements on objects, arrays, and strings.

## Dot notation

Access object properties by name:

```jyro
obj.name                 # Property access
obj.address.city         # Nested property traversal
```

## Bracket notation

Access array elements by index (zero-based), object properties by key, or string characters by position:

```jyro
arr[0]                   # Array element (first)
obj["key"]               # Object property (for dynamic keys)
str[0]                   # String character (returns single-char string)
```

Bracket notation supports dynamic keys via expressions:

```jyro
var field = "name"
var value = obj[field]   # Equivalent to obj.name
```

## Null behaviour

- Property access on an object returns `null` if the property does not exist
- Index access returns `null` if the index is out of bounds or the key is missing
- Accessing a property on `null` returns `null` (null propagation)
- Assigning a property on `null` causes a runtime error ([JM5306](/jyro/error-codes/))

```jyro
# Null propagates through chained access
var result = Data.user.profile.name   # null if any part of the chain is null

# Use null coalescing for defaults
var name = Data.user.name ?? "Unknown"
```

## Assignment

Properties and indexes are assignable:

```jyro
obj.name = "Bob"
arr[0] = 99
obj["dynamicKey"] = "value"
```

## Deleting properties

The `delete` keyword removes a property from an object entirely - the key is gone, not just set to null.

```jyro
Data.user = {name: "alice", password: "secret", role: "admin"}

delete Data.user.password           # Property is completely removed
# Data.user is now {name: "alice", role: "admin"}

# Dynamic key via bracket access
var field = "role"
delete Data.user[field]             # Removes "role" from Data.user
```

**Rules:**
- `delete` only works on property access (`obj.key`) and bracket access (`obj["key"]`)
- `delete` on a non-existent property is a no-op (no error)
- `delete` on a bare variable (`delete myVar`) is a compile-time error - variables cannot be deleted
- `delete Data` is a compile-time error - `Data` itself cannot be removed
- The target expression must be on the same line as `delete`

This is the only way to completely remove a key from the `Data` context. Setting a property to `null` leaves the key present in the output; `delete` removes it entirely.

## Keywords as property names

Keywords are valid as property names when accessed via dot notation or bracket notation. This only applies to member access on objects - it does not allow keywords as local variable names.

```jyro
Data.for = "loop data"         # Valid
Data.number = 42               # Valid
Data.string = "text"           # Valid
var value = Data["default"]    # Valid

# var for = "x"                # INVALID - syntax error
```
