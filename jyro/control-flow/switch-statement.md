---
layout: default
title: switch Statement
parent: Control Flow
has_children: false
has_toc: false
permalink: /jyro/control-flow/switch-statement/
nav_order: 120
---

# `switch` Statement

The `switch` statement selects a block of code to execute based on the value of an expression.

```jyro
switch expression do
    case value1 then
        # body
    case value2, value3 then
        # body
    default then
        # body
end
```

## Syntax rules

- The `do` keyword follows the switch expression
- Each `case` block uses `then` to open and is implicitly terminated by the next `case`, `default`, or the closing `end`
- Multiple values per case are comma-separated
- The `default` clause is optional and handles unmatched values
- The outer `switch` block is closed with `end`

## No fall-through

Only the matched case executes. There is no fall-through between cases, and no `break` is needed.

```jyro
switch Data.action do
    case "create" then
        Data.status = "created"
    case "update", "patch" then
        Data.status = "updated"
    default then
        Data.status = "unknown"
end
```

In this example, if `Data.action` is `"update"` or `"patch"`, only the second case body executes. The `default` case is skipped.

## No match without default

If no case matches and there is no `default` clause, execution silently continues past the `switch` statement:

```jyro
switch Data.status do
    case "active" then
        Data.label = "Active"
    case "inactive" then
        Data.label = "Inactive"
end
# If Data.status is "pending", execution continues here with no case executed
```
