---
layout: default
title: return Statement
parent: Control Flow
has_children: false
has_toc: false
permalink: /jyro/control-flow/return-statement/
nav_order: 130
---

# `return` Statement

The `return` statement exits a [user-defined function](/jyro/user-functions/) and sends a value back to the caller. It is only valid inside a `func` body - using `return` at the script's top level is a compiler error.

```jyro
func Double(x: number)
    return x * 2
end

Data.result = Double(5)    # 10
```

## Behaviour

- Execution jumps back to the call site with the returned value
- A bare `return` (no expression) returns `null`
- If the function body reaches `end` without hitting a `return`, the function returns `null`

```jyro
func MaybeDouble(x)
    if x is not number then
        return              # returns null
    end
    return x * 2
end

var a = MaybeDouble(5)      # 10
var b = MaybeDouble("hi")   # null
```

## Not valid at the top level

`return` is exclusively for functions. At the script's top level, use `exit` for clean termination and `fail` for error termination:

```jyro
# INCORRECT - compiler error: return outside a function
return "Done"

# CORRECT - use exit at the top level
exit "Done"
```

## Comparison with exit and fail

| Keyword | Effect | Valid where |
|---------|--------|-------------|
| `return` | Return a value from a function | Inside functions only |
| `exit` | Terminate the script cleanly (success) | Anywhere |
| `fail` | Terminate the script with an error (failure) | Anywhere |

`return` exits the current function. `exit` and `fail` terminate the entire script - even from inside functions. See [exit](/jyro/control-flow/exit-statement/) and [fail](/jyro/control-flow/fail-statement/) for details.
