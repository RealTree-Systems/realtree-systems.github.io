---
layout: default
title: while Loop
parent: Iteration
has_children: false
has_toc: false
permalink: /jyro/iteration/while-loop/
nav_order: 140
---

# `while` Loop

The `while` loop executes its body repeatedly as long as the condition evaluates to a truthy value.

```jyro
while condition do
    # body
end
```

The `do` keyword is required after the condition. The block is closed with `end`.

## Example

```jyro
var i = 0
while i < 10 do
    Data.output = Append(Data.output, i)
    i += 1
end
```

## Condition evaluation

The condition is checked before each iteration. If the condition is falsy on the first check, the body never executes. See [Truthiness](/jyro/truthiness/) for the rules on which values are truthy.

## Resource limits

The host environment enforces a maximum iteration count. An infinite loop or excessively long-running loop will be terminated by the runtime.
