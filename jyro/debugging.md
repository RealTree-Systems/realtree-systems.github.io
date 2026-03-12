---
layout: default
title: Debugging Strategies
parent: Jyro Language Reference
has_children: false
has_toc: true
permalink: /jyro/debugging/
nav_order: 160
---

# Debugging Strategies

The following provide tips for debugging Jyro code.

## Add intermediate variables

Break complex expressions into steps and write intermediate values to `Data` for inspection:

```jyro
# Hard to debug
Data.result = SortByField(WhereByField(Data.items, "active", "==", true), "price", "desc")

# Easy to debug
var active = WhereByField(Data.items, "active", "==", true)
Data._debugActive = active
var sorted = SortByField(active, "price", "desc")
Data._debugSorted = sorted
Data.result = sorted
```

## Inspect types and values

Write a debug object to `Data` to examine runtime state:

```jyro
Data._debug = {
    "inputType": TypeOf(Data.input),
    "inputValue": Data.input,
    "isNull": Data.input is null,
    "length": Data.input is array ? Length(Data.input) : "N/A"
}
```

## Use type guards liberally

Guard against mixed types with `is` checks and `continue`:

```jyro
foreach item in Data.items do
    if item is not object then
        continue
    end
    if item.price is not number then
        continue
    end
    # Safe to process
    total += item.price
end
```

## Test with minimal data first

Start with the simplest possible input:

```json
{"items": [{"id": 1, "price": 10}]}
```

Then add edge cases incrementally:

```json
{"items": [{"id": 1, "price": 10}, {"id": 2, "price": null}, {"id": 3}]}
```
