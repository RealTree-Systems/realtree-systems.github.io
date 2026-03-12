---
layout: default
title: foreach Loop
parent: Iteration
has_children: false
has_toc: true
permalink: /jyro/iteration/foreach-loop/
nav_order: 130
---

# `foreach` Loop

The `foreach` loop iterates over the elements of an array, the characters of a string, or the values of an object.

```jyro
foreach variable in expression do
    # body
end
```

The `do` keyword is required. The block is closed with `end`.

## Iterating over arrays

```jyro
foreach item in Data.items do
    item.processed = true
end
```

## Iterating over strings

When the source is a string, each iteration yields a single-character string:

```jyro
foreach ch in "hello" do
    # ch is "h", "e", "l", "l", "o"
end
```

## Iterating over objects

When the source is an object, `foreach` iterates over the object's **values**:

```jyro
foreach val in Data.scores do
    total += val
end
```

To iterate over an object's keys, use `Keys()`:

```jyro
foreach key in Keys(Data.scores) do
    var value = Data.scores[key]
end
```

## Non-iterable values

Only arrays, strings, and objects are iterable. Attempting to iterate over a number, boolean, or null is an error:

- **Compile-time** - if the collection expression is a literal of a non-iterable type (e.g. `foreach x in 42 do`), the compiler reports error [JM3402](/jyro/error-codes/).
- **Runtime** - if a variable evaluates to a non-iterable type at runtime, error [JM5402](/jyro/error-codes/) is raised.

## Loop variable scoping

The iteration variable is scoped to the loop body and does not exist outside the loop.

## Collection modification

Modifying the collection being iterated causes a runtime error. Use a separate collection for additions:

```jyro
# INCORRECT - runtime error
foreach item in Data.items do
    Data.items = Append(Data.items, item)
end

# CORRECT - collect into a separate array
var additions = []
foreach item in Data.items do
    if item.shouldDuplicate then
        additions = Append(additions, item)
    end
end
foreach addition in additions do
    Data.items = Append(Data.items, addition)
end
```
