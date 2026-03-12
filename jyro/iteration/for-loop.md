---
layout: default
title: for Loop
parent: Iteration
has_children: false
has_toc: true
permalink: /jyro/iteration/for-loop/
nav_order: 120
---

# `for` Loop

The `for` loop iterates over a numeric range. It supports ascending (`to`) and descending (`downto`) sequences with an optional step value.

## Ascending

```jyro
for i in 0 to 5 do              # 0, 1, 2, 3, 4
    # body
end

for i in 0 to 10 by 2 do        # 0, 2, 4, 6, 8
    # body
end
```

## Descending

```jyro
for i in 5 downto 0 do          # 5, 4, 3, 2, 1
    # body
end

for i in 10 downto 0 by 2 do    # 10, 8, 6, 4, 2
    # body
end
```

The step must always be a positive number. For descending (`downto`) loops, the step is subtracted from the loop variable automatically. Writing `by -2` or `by 0` causes a runtime error.

## Exclusive bounds

Bounds are always exclusive - the end value is never reached. For `to`, the bound behaves like `<`. For `downto`, it behaves like `>`.

To iterate over an inclusive range, adjust the bound by one:

```jyro
for i in 1 to 10 + 1 do         # 1 through 10 inclusive
    # body
end

for i in 10 downto 1 - 1 do     # 10 through 1 inclusive
    # body
end
```

## Non-iterable values

Only arrays, strings, and objects are iterable. Attempting to iterate over a number, boolean, or null is an error:

- **Compile-time** - if the collection expression is a literal of a non-iterable type (e.g. `foreach x in 42 do`), the compiler reports error [JM3402](/jyro/error-codes/).
- **Runtime** - if a variable evaluates to a non-iterable type at runtime, error [JM5402](/jyro/error-codes/) is raised.

## Evaluation

The start, end, and step expressions are evaluated **once** at loop entry. Modifying variables used in these expressions during the loop body has no effect on the iteration range.

## Loop variable scoping

The loop variable is scoped to the loop body. It is not accessible before or after the loop.

```jyro
for i in 0 to 5 do
    # i is available here
end
# i is NOT available here
```
