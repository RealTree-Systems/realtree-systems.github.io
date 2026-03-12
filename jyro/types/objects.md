---
layout: default
title: Objects
parent: Types
has_children: false
has_toc: false
permalink: /jyro/literals/objects/
nav_order: 150
---

# Objects

Objects are unordered collections of key-value pairs. They are written as comma-separated `key: value` pairs enclosed in curly braces:

```jyro
{name: "Alice", age: 30}
{"status": "active", "count": 5}
{}                                    # Empty object
```

## Object keys

Keys can be either unquoted identifiers or quoted strings:

```jyro
var obj = {name: "Alice", age: 30}           # Unquoted keys
var obj2 = {"first-name": "Alice"}           # Quoted keys (required for special characters)
```

Use quoted keys when the key contains characters that are not valid in identifiers (such as hyphens or spaces).

## Nested objects

Objects may be nested to any depth:

```jyro
var user = {
    name: "Alice",
    address: {
        city: "Boston",
        zip: "02101"
    }
}
```

## Creating nested structures on `Data`

When assigning nested properties on the `Data` context, intermediate objects must be created explicitly:

```jyro
# INCORRECT - error if Data.level1 doesn't exist
Data.level1.level2.value = "x"

# CORRECT - create each level
Data.level1 = {}
Data.level1.level2 = {}
Data.level1.level2.value = "x"

# Or assign a nested literal directly
Data.level1 = {"level2": {"value": "x"}}
```

## Truthiness

Empty objects `{}` are **truthy**. See [Truthiness](/jyro/truthiness/) for details.
