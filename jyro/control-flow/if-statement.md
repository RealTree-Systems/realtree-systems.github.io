---
layout: default
title: if Statement
parent: Control Flow
has_children: false
has_toc: true
permalink: /jyro/control-flow/if-statement/
nav_order: 110
---

# `if` Statement

## `if` / `elseif` / `else`

```jyro
if condition then
    # body
elseif otherCondition then
    # body
else
    # body
end
```

The `then` keyword is required after each condition. The block is closed with `end`. The `elseif` and `else` clauses are optional, and any number of `elseif` clauses may appear.

## `elseif` vs `else if`

The `elseif` and `else if` constructs are semantically different.

**`elseif` (one word)** is part of a single `if` statement and requires one `end`:

```jyro
if score >= 90 then
    grade = "A"
elseif score >= 80 then
    grade = "B"
else
    grade = "F"
end
```

**`else if` (two words)** creates a nested `if` inside an `else` block. Each `if` requires its own `end`:

```jyro
if score >= 90 then
    grade = "A"
else
    if score >= 80 then
        grade = "B"
    else
        grade = "F"
    end       # closes inner if
end           # closes outer if
```

With `elseif`, the next condition is evaluated immediately. With `else if`, statements can run between the `else` and the nested `if`:

```jyro
if Data.source == "api" then
    Data.result = handleApi()
else
    var fallback = Data.cache ?? loadDefaults()
    if fallback is not null then
        Data.result = fallback
    else
        fail "No data available"
    end
end
```

This would not be possible with `elseif`, because there is no place to put the `var fallback` line. Use `elseif` for straightforward conditional chains, and `else if` when you need to do work before the nested condition.
