---
layout: default
title: exit Statement
parent: Control Flow
has_children: false
has_toc: false
permalink: /jyro/control-flow/exit-statement/
nav_order: 125
---

# `exit` Statement

The `exit` statement terminates the script immediately with a success status. It accepts an optional string message that must appear on the **same line**.

```jyro
exit                                # Clean exit, no message
exit "Processing complete"          # Clean exit with message
```

## Behaviour

- The script stops executing at the `exit` statement
- The `Data` context is returned to the host with all modifications made up to that point
- If a message is provided, it is reported to the host as an informational message
- `exit` terminates the entire script even when called from inside a [user-defined function](/jyro/user-functions/)

```jyro
func Validate(x)
    if x is null then
        exit "missing required value"    # terminates the entire script
    end
    return x * 2
end

Data.result = Validate(Data.input)
```

## Message must be on the same line

The message is part of the `exit` statement syntax. If placed on a separate line, it becomes an independent string expression statement - not an exit message.

```jyro
# CORRECT
exit "Done"

# INCORRECT - "Done" is a separate statement that never executes
exit
"Done"
```

## Comparison with return and fail

| Keyword | Effect | Valid where |
|---------|--------|-------------|
| `exit` | Terminate the script cleanly (success) | Anywhere |
| `fail` | Terminate the script with an error (failure) | Anywhere |
| `return` | Return a value from a function | Inside functions only |

`exit` and `fail` both terminate the entire script - even from inside functions. `return` exits the current function and resumes at the call site. See [fail](/jyro/control-flow/fail-statement/) and [return](/jyro/control-flow/return-statement/) for details.
