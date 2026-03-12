---
layout: default
title: Lambda Expressions
parent: Jyro Language Reference
has_children: false
has_toc: true
permalink: /jyro/lambda-expressions/
nav_order: 95
---

# Lambda Expressions

A lambda is a short inline rule that says "given this input, produce this output". The `=>` arrow separates the input (left) from the output (right):

```jyro
x => x * 2
```

This is read as: "given `x`, return `x * 2`". The name `x` is a placeholder - it takes on the value of each element as the function processes the array. For example, `Map([10, 20, 30], x => x * 2)` produces `[20, 40, 60]`.

Jyro supports lambdas as anonymous functions used as arguments to higher-order standard library functions such as `Map`, `Where`, `Reduce`, `Find`, `All`, `Any`, `SortBy`, and `Each`.

## Syntax

```jyro
x => x * 2                          # Single parameter
(x, y) => x + y                     # Multiple parameters
(acc, item) => acc + item.price      # Used with Reduce
```

Parentheses around the parameter list are optional for single-parameter lambdas. They are required for multiple parameters.

## Single-expression body

The body of a lambda is a single expression. The value of the expression is implicitly returned.

```jyro
# Valid - single expression
var doubled = Map(Data.numbers, x => x * 2)

# NOT valid - blocks are not supported in lambdas
# var doubled = Map(Data.numbers, x => { var result = x * 2; return result })
```

## Closures

Lambdas capture variables from the enclosing scope:

```jyro
var threshold = 100
var big = Where(Data.items, x => x > threshold)    # Captures 'threshold'
```

## Common patterns

```jyro
# Transform
var doubled = Map(Data.numbers, x => x * 2)
var upper = Map(Data.names, n => ToUpper(n))

# Filter
var adults = Where(Data.users, u => u.age >= 18)
var evens = Where(Data.numbers, x => x % 2 == 0)

# Aggregate
var total = Reduce(Data.items, (sum, item) => sum + item.price, 0)

# Search
var first = Find(Data.items, x => x.status == "pending")

# Check
var anyExpired = Any(Data.items, x => x.expired)
var allValid = All(Data.items, x => x.price > 0)

# Sort by computed key
var byPrice = SortBy(Data.items, x => x.price)
```

## Lambda vs query functions

For simple single-field comparisons, the query functions (`WhereByField`, `FindByField`, etc.) are suitable. Lambdas are normally used to pass complex conditions involving multiple fields or computed values.

```jyro
# Simple field comparison - query is more concise
var active = WhereByField(Data.users, "active", "==", true)

# The above as a lambda
var active = Where(Data.users, u => u.active == true)

# Complex condition - lambda is the only option
var eligible = Where(Data.users, u => u.age >= 18 and u.active and u.balance > 0)
```

Lambdas can express any condition that query functions can, plus multi-field logic, computed values, and function calls. Query functions are a concise shorthand for the common "filter by one field" pattern.

The one capability unique to query functions is dot-path field traversal with a dynamic field name. Because the field name is a string, it can be stored in a variable:

```jyro
var field = "address.city"
var result = WhereByField(Data.users, field, "==", "Boston")
```

A lambda has no equivalent for nested paths:

```jyro
var field = "address.city"

# Query function - traverses the dot-path "address.city"
var result = WhereByField(Data.users, field, "==", "Boston")    # Works

# Lambda - treats "address.city" as a single top-level key, not a path
var result = Where(Data.users, u => u[field] == "Boston") # Looks for a property literally named "address.city"
```

## Lambdas vs user-defined functions

Jyro has two ways to define reusable logic: lambdas and [user-defined functions](/jyro/user-functions/). They serve different purposes:

| | Lambda | User-defined function |
|---|---|---|
| **Syntax** | `x => x * 2` | `func Double(x) ... end` |
| **Scope** | Captures variables from enclosing scope | Isolated - only sees parameters and local `var`s |
| **Data access** | Can read `Data` | Cannot access `Data` (compiler error) |
| **Body** | Single expression | Block of statements (`if`, `while`, `var`, `return`, etc.) |
| **Naming** | Anonymous - always inline | Named - callable from anywhere in the script |
| **Hoisting** | Not hoisted | Hoisted - can be called before declaration |
| **Use case** | Inline callbacks for `Map`, `Where`, `Reduce`, etc. | Reusable named logic with explicit parameters |

Lambdas are lightweight inline expressions that close over their environment. Functions enforce an explicit interface - every dependency comes through the parameter list.

```jyro
# Lambda - captures 'multiplier' from the enclosing scope
var multiplier = 3
var scaled = Map(Data.values, x => x * multiplier)

# Function - 'multiplier' must be passed as an argument
func Scale(x, multiplier)
    return x * multiplier
end

var scaled = Map(Data.values, x => Scale(x, 3))
```

Functions and lambdas compose naturally. Define complex logic in a function, then reference it from a lambda:

```jyro
func IsEligible(customer)
    if customer.age < 18 then return false end
    if customer.status != "active" then return false end
    if customer.balance < 0 then return false end
    return true
end

var eligible = Where(Data.customers, c => IsEligible(c))
```

