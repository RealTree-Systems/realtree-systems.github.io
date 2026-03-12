---
layout: default
title: User-Defined Functions
parent: Jyro Language Reference
has_children: false
has_toc: true
permalink: /jyro/user-functions/
nav_order: 96
---

# User-Defined Functions

User-defined functions let you extract reusable, named logic with explicit parameters. They are **pure** - they cannot access `Data` or capture variables from the enclosing script.

```jyro
func CalculateTax(amount: number, rate: number)
    return amount * rate
end

Data.tax = CalculateTax(Data.subtotal, Data.taxRate)
Data.total = Data.subtotal + Data.tax
```

## Declaration

A function declaration starts with `func`, followed by a name, a parameter list in parentheses, a body of statements, and `end`:

```jyro
func Name(param1, param2: type)
    # body (any statements: var, if, while, for, return, etc.)
end
```

Functions must be declared at the **top level** of the script - not inside `if` blocks, loops, or other functions:

```jyro
# INCORRECT - function declarations must be at the top level
if Data.mode == "advanced" then
    func Helper(x)
        return x * 2
    end
end

# INCORRECT - functions cannot be nested inside other functions
func Outer()
    func Inner()
        return 42
    end
    return Inner()
end
```

Function names must be unique. A function cannot share a name with a built-in or host-provided function:

```jyro
func Round(x)       # Error: 'Round' is a built-in function
    return x
end
```

## Parameters

Parameters are listed by name with optional type hints. When a type hint is present, the argument is coerced to that type when the function is called - the same behaviour as a typed `var` declaration.

```jyro
func Add(a: number, b: number)
    return a + b
end

func Label(value)
    return "Value: " + ToString(value)
end
```

By default, parameters are **required**. Every call must supply the expected number of arguments:

```jyro
func Multiply(a, b)
    return a * b
end

Data.result = Multiply(3, 4)       # 12
Data.result = Multiply(3)          # Error: expected 2 arguments, got 1
Data.result = Multiply(3, 4, 5)    # Error: expected 2 arguments, got 3
```

Parameter names must be unique within a function, and `Data` cannot be used as a parameter name.

### Default parameter values

A parameter can be made optional by assigning a default value with `=`. When the caller omits an optional argument (or passes `null`), the default value is used instead:

```jyro
func Greet(name, greeting = "Hello", excited = false)
    var msg = greeting + ", " + name
    if excited then
        msg = msg + "!"
    end
    return msg
end

Data.a = Greet("Alice", "Hi", true)   # "Hi, Alice!"
Data.b = Greet("Bob", "Hey")          # "Hey, Bob"
Data.c = Greet("Charlie")             # "Hello, Charlie"
Data.d = Greet("Dave", null)          # "Hello, Dave" - null triggers default
```

Default values and type hints can be combined:

```jyro
func TypedGreet(name: string, greeting: string = "Hello", excited: boolean = false)
    var msg = greeting + ", " + name
    if excited then
        msg = msg + "!"
    end
    return msg
end
```

Rules for default parameters:

- Default values must be **literals** (numbers, strings, booleans, or `null`) - not expressions or variable references.
- **Required parameters must come before optional parameters.** Placing a required parameter after an optional one is a compiler error.
- When calling with positional arguments, optional arguments can only be omitted from the right - you cannot skip a middle optional argument.

## Return values

Inside a function, `return` sends a value back to the caller:

```jyro
func Double(x: number)
    return x * 2
end

var result = Double(5)    # 10
```

A bare `return` without a value returns `null`:

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

If the function body reaches `end` without hitting a `return`, the function returns `null`.

`return` is exclusively for functions. At the script's top level, use `exit` for clean termination and `fail` for error termination - using `return` outside a function is a compiler error.

## Calling functions

Call user-defined functions the same way as built-in functions - by name with arguments in parentheses. Function calls are expressions, so they work anywhere an expression is valid:

```jyro
Data.tax = CalculateTax(Data.subtotal, Data.taxRate)

if IsValid(Data.amount) then
    Data.status = "ok"
end

var valid = Where(Data.items, item => IsValid(item.price))

var total = Reduce(Data.values, (sum, v) => sum + Double(v), 0)
```

### Named arguments

Instead of passing arguments by position, you can pass them by name using `paramName: value` syntax. Named arguments can appear in any order:

```jyro
func Greet(name, greeting = "Hello", excited = false)
    var msg = greeting + ", " + name
    if excited then
        msg = msg + "!"
    end
    return msg
end

# Arguments in any order
Data.a = Greet(excited: true, name: "Alice", greeting: "Hi")   # "Hi, Alice!"

# Omit optional arguments freely
Data.b = Greet(name: "Bob")                                     # "Hello, Bob"

# Skip middle optional arguments
Data.c = Greet(name: "Charlie", excited: true)                  # "Hello, Charlie!"

# Provide all arguments
Data.d = Greet(name: "Eve", greeting: "Hey", excited: false)    # "Hey, Eve"
```

Named arguments work with standard library functions too:

```jyro
# Substring(text, start, length?) - reorder freely
Data.sub = Substring(start: 2, text: "Hello World")

# PadLeft(text, length, padChar?) - skip optional padChar
Data.pad = PadLeft(text: "hi", length: 8)

# Replace(source, oldValue, newValue) - fully reordered
Data.replaced = Replace(newValue: "World", oldValue: "X", source: "Hello X")
```

Rules for named arguments:

- A call must use **either** all positional arguments **or** all named arguments - you cannot mix the two styles in a single call.
- Every **required** parameter must be provided. Optional parameters with defaults can be omitted.
- Each parameter name can only appear once per call.
- The parameter name must match an actual parameter of the function being called.

## Function hoisting

Functions can be called before they are declared. All function declarations are visible throughout the entire script, regardless of where they appear in the source:

```jyro
Data.result = Process(Data.input)

func Process(x)
    return x * 2
end
```

Functions can call each other freely (mutual recursion):

```jyro
func IsEven(n: number)
    if n == 0 then return true end
    return IsOdd(n - 1)
end

func IsOdd(n: number)
    if n == 0 then return false end
    return IsEven(n - 1)
end

Data.result = IsEven(4)    # true
```

Hoisting applies only to function declarations. Variables declared with `var` are not hoisted - they exist from the point of declaration forward.

## No Data inside functions

Functions cannot access `Data`. This is the most important rule about user-defined functions, and the compiler enforces it:

```jyro
func Bad()
    return Data.value    # Error: cannot access Data inside a function
end
```

If a function needs information from `Data`, pass it as an argument:

```jyro
func Good(value)
    return value * 2
end

Data.result = Good(Data.value)
```

The rule keeps functions pure. A function's behaviour depends only on its arguments. The top level of the script is where `Data` is read and written; functions are helpers that the top level calls.

## No closures

Functions cannot see variables from the surrounding script. Each function has its own isolated scope containing only its parameters and any variables it declares locally:

```jyro
var multiplier = 3

func Scale(x)
    return x * multiplier   # Error: 'multiplier' is not declared
end
```

Pass the value as a parameter instead:

```jyro
func Scale(x, multiplier)
    return x * multiplier
end

var m = 3
Data.result = Scale(Data.value, m)
```

This is where functions and lambdas differ. Lambdas capture variables from the surrounding scope:

```jyro
var multiplier = 3
var scaled = Map(Data.values, x => x * multiplier)   # this works
```

Functions enforce an explicit interface - every dependency comes through the parameter list. Lambdas are lightweight inline expressions that close over their environment. See [Lambda Expressions](/jyro/lambda-expressions/) for more detail.

## exit and fail inside functions

`exit` terminates the entire script, even from inside a function. It does not just return from the function - it stops everything:

```jyro
func Validate(x)
    if x is null then
        exit "missing required value"    # terminates the script
    end
    return x * 2
end
```

`fail` works the same way - it terminates the script with an error, regardless of how deep in the call stack it appears:

```jyro
func RequirePositive(x, name)
    if x is not number then
        fail name + " must be a number"
    end
    if x <= 0 then
        fail name + " must be positive"
    end
    return x
end

var amount = RequirePositive(Data.amount, "amount")
var rate = RequirePositive(Data.rate, "rate")
Data.result = amount * rate
```

The three keywords have clear, non-overlapping meanings:

| Keyword | Effect | Valid where |
|---------|--------|-------------|
| `return` | Exit the function, resume at the call site | Inside functions only |
| `exit` | Terminate the script cleanly (success) | Anywhere |
| `fail` | Terminate the script with an error (failure) | Anywhere |

## Recursion

A function can call itself. Jyro enforces a maximum call depth to prevent infinite recursion from exhausting memory:

```jyro
func Factorial(n: number)
    if n <= 1 then
        return 1
    end
    return n * Factorial(n - 1)
end

Data.result = Factorial(5)    # 120
```

## Examples

### Validation helpers

```jyro
func RequireString(value, fieldName)
    if value is not string or value == "" then
        fail fieldName + " is required"
    end
    return value
end

func RequireNumber(value, fieldName, min, max)
    if value is not number then
        fail fieldName + " must be a number"
    end
    if value < min or value > max then
        fail fieldName + " must be between " + ToString(min) + " and " + ToString(max)
    end
    return value
end

var name = RequireString(Data.name, "name")
var age = RequireNumber(Data.age, "age", 0, 150)
Data.record = { "name": name, "age": age }
```

### Data transformation

```jyro
func FullName(first, last)
    return Trim(first) + " " + Trim(last)
end

func TotalWithTax(amount: number, taxRate: number)
    return Round(amount + amount * taxRate, 2)
end

foreach order in Data.orders do
    order.customerName = FullName(order.firstName, order.lastName)
    order.total = TotalWithTax(order.subtotal, Data.taxRate)
end
```

### Composing with lambdas

```jyro
func IsEligible(customer)
    if customer.age < 18 then return false end
    if customer.status != "active" then return false end
    if customer.balance < 0 then return false end
    return true
end

var eligible = Where(Data.customers, c => IsEligible(c))
Data.eligibleCount = Length(eligible)
```
