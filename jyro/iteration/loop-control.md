---
layout: default
title: Loop Control
parent: Iteration
has_children: false
has_toc: false
permalink: /jyro/iteration/loop-control/
nav_order: 150
---

# Loop Control

Jyro has two keywords for controlling loop behaviour: `break` and `continue`.

## `break`

The `break` statement exits the innermost enclosing loop immediately. Execution continues with the first statement after the loop's `end`.

```jyro
foreach item in Data.items do
    if item.status == "stop" then
        break
    end
    Data.processed = Append(Data.processed, item)
end
```

## `continue`

The `continue` statement skips the remainder of the current iteration and proceeds to the next iteration of the innermost enclosing loop.

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

## Nested loops

Both `break` and `continue` affect only the innermost loop. There is no labelled break or continue for targeting outer loops.
