---
layout: default
title: Variable Scoping
parent: Variables
has_children: false
has_toc: false
permalink: /jyro/variables/scoping/
nav_order: 130
---

# Variable Scoping

## Block visibility

Variables declared before a block (`if`, `while`, `for`, `foreach`, `switch`) are visible inside the block. Modifications to those variables inside the block affect the outer scope.

```jyro
var total = 0
foreach item in Data.items do
    total += item.price    # Modifies the outer 'total'
end
# total reflects the accumulated sum
```

## Loop variable scoping

The iteration variable of a `for` or `foreach` loop is scoped to the loop body. It does not exist before the loop and is not accessible after it.

```jyro
for i in 0 to 5 do
    # i is available here
end
# i is NOT available here

foreach item in Data.items do
    # item is available here
end
# item is NOT available here
```

## Shadowing

An inner `var` declaration can shadow an outer variable of the same name. The inner variable takes precedence within its scope, and the outer variable is restored when the block exits.

```jyro
var x = 10
if true then
    var x = 20     # Shadows the outer x
    # x is 20 here
end
# x is 10 here - outer variable is unchanged
```
