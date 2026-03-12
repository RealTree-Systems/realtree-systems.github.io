---
layout: default
title: Type Checking
parent: Types
has_children: false
has_toc: false
permalink: /jyro/types/type-checking/
nav_order: 9999
---

# Type Checking

The `is` operator tests whether a value is of a given type at runtime. The `is not` form negates the test.

```jyro
value is number               # true if value is a number
value is not string           # true if value is NOT a string
```

## Valid type keywords

The valid type keywords are simply the names of Jyro's types:

`number`, `string`, `boolean`, `array`, `object`, `null`

## Examples

In this example, we can take different paths if input is a number as opposed to a string:

```jyro
if Data.input is number then
    Data.result = Data.input * 2
elseif Data.input is string then
    Data.result = ToUpper(Data.input)
end
```

Type checking is commonly used for input validation and type-safe processing:

```jyro
if Data.items is null or Data.items is not array then
    fail "Items must be an array"
end

foreach item in Data.items do
    if item is not object then
        continue
    end
    if item.price is not number then
        continue
    end
    # Safe to process
end
```

## Runtime function

The `TypeOf()` standard library function returns the type name as a string:

```jyro
TypeOf(42)        # "number"
TypeOf("hello")   # "string"
TypeOf(true)      # "boolean"
TypeOf([1, 2])    # "array"
TypeOf({a: 1})    # "object"
TypeOf(null)      # "null"
```
