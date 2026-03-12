---
layout: default
title: Discriminated Unions
parent: Jyro Language Reference
has_children: false
has_toc: true
permalink: /jyro/discriminated-unions/
nav_order: 97
---

# Discriminated Unions

Discriminated unions declare a closed set of named shapes (called variants) that a value can take. The `match` statement destructures those variants with compile-time exhaustiveness checking - if you forget to handle a case, the compiler rejects the script before it runs.

```jyro
union Shape
    Circle(radius: number)
    Rect(width: number, height: number)
end

func Area(shape)
    return match shape do
        case Circle(r) then 3.14159 * r * r
        case Rect(w, h) then w * h
    end
end

Data.area = Area(Circle(10))    # 314.159
```

## Declaring a union

A union declaration names a type and lists its variants. Each variant has a name and optional fields:

```jyro
union PaymentMethod
    CreditCard(number: string, expiry: string, cvv: string)
    BankTransfer(routingNumber: string, accountNumber: string)
    Cash()
end
```

Rules:

- Unions must be declared at the **top level** of the script - not inside `if` blocks, loops, or functions
- Each variant name must be **globally unique** across all unions in the script. Variant names are registered as constructor functions
- Variant names cannot collide with built-in functions or user-defined functions
- Field type hints are optional. When present, they coerce arguments at construction time, just like typed function parameters

## Variants without fields

A variant with no fields acts like an enum value - a tag with no associated data:

```jyro
union Status
    Active()
    Suspended()
    Terminated()
end

var userStatus = Active()
```

The empty parentheses are required - `Active()` is a function call. The result is a tagged object with a `_variant` field and nothing else.

## Creating values

Each variant in a union becomes a constructor function. Call it like any other function:

```jyro
union Shape
    Circle(radius: number)
    Rect(width: number, height: number)
    Triangle(base: number, height: number)
end

var c = Circle(10)
var r = Rect(4, 5)
var t = Triangle(6, 3)
```

All arguments are required. `Circle()` with no arguments is an error.

Under the surface, calling `Circle(10)` creates a plain object with a `_variant` tag and the declared fields:

```json
{
    "_variant": "Circle",
    "radius": 10
}
```

The host sees ordinary JSON. There is nothing exotic about the runtime representation.

## Destructuring with match

`match` examines a union value and branches based on its variant. Each `case` names a variant and binds its fields to local variables:

```jyro
match shape do
    case Circle(r) then
        Data.description = "Circle with radius " + ToString(r)
    case Rect(w, h) then
        Data.description = ToString(w) + " x " + ToString(h) + " rectangle"
    case Triangle(b, h) then
        Data.description = "Triangle with base " + ToString(b)
end
```

The binding names (`r`, `w`, `h`, `b`) do not have to match the field names in the union declaration. They are positional - the first binding gets the first field, the second gets the second, and so on. `case Circle(r)` maps `r` to the `radius` field because `radius` is the first (and only) field of `Circle`.

### Zero-field variant bindings

When matching a variant that has no fields, omit the parentheses:

```jyro
union Toggle
    On()
    Off()
end

match state do
    case On then
        Data.label = "Enabled"
    case Off then
        Data.label = "Disabled"
end
```

## Exhaustiveness

The compiler checks that every `match` covers every variant of the union. If you forget one, it is a compile-time error:

```jyro
union Direction
    North()
    South()
    East()
    West()
end

# Error: non-exhaustive match - missing variants: South, West
match heading do
    case North() then
        Data.y = Data.y + 1
    case East() then
        Data.x = Data.x + 1
end
```

There is **no `default`** in `match`. The whole point of declaring a union is opting into this exhaustiveness guarantee. If you want open-ended dispatch with a catch-all, use `switch` with `default` instead.

## Match expressions

`match` can appear as an expression, where each arm is a single expression that produces a value:

```jyro
# Statement match: branch and do work
match order do
    case Pending then
        Data.status = "waiting"
        Data.priority = 0
    case Shipped(tracking, carrier) then
        Data.status = "in transit"
        Data.trackingUrl = carrier + "/" + tracking
end

# Match expression: compute a value
var label = match order do
    case Pending then "Awaiting processing"
    case Shipped(tracking, carrier) then carrier + ": " + tracking
end
```

In the statement form, `then` is followed by a block of statements that ends at the next `case` or `end`. In the expression form, `then` is followed by a single expression on the same line.

Match expressions work anywhere an expression is expected - variable initialisation, function arguments, `return` values:

```jyro
func Area(shape)
    return match shape do
        case Circle(r) then 3.14159 * r * r
        case Rect(w, h) then w * h
    end
end
```

Both forms require exhaustive coverage.

## Combining unions with functions

Functions cannot access `Data`, so they need union values passed as arguments. This makes them natural homes for logic that operates on unions:

```jyro
union Expr
    Literal(value: number)
    Add(left, right)
    Mul(left, right)
end

func Eval(expr)
    return match expr do
        case Literal(v) then v
        case Add(a, b) then Eval(a) + Eval(b)
        case Mul(a, b) then Eval(a) * Eval(b)
    end
end

# (2 + 3) * 4 = 20
var tree = Mul(Add(Literal(2), Literal(3)), Literal(4))
Data.result = Eval(tree)
```

Recursive unions like `Expr` are powerful. The `Add` and `Mul` variants contain other `Expr` values as fields, creating a tree structure. `Eval` walks the tree recursively, matching each node and computing the result.

## Examples

### Result types

```jyro
union Result
    Ok(value)
    Error(message: string)
end

func Divide(a: number, b: number)
    if b == 0 then
        return Error("Division by zero")
    end
    return Ok(a / b)
end

var result = Divide(Data.numerator, Data.denominator)
match result do
    case Ok(v) then
        Data.quotient = v
    case Error(msg) then
        Data.error = msg
end
```

### State machines

```jyro
union OrderStatus
    Pending()
    Processing(startedAt: string)
    Shipped(trackingNumber: string, carrier: string)
    Delivered(deliveredAt: string)
    Cancelled(reason: string)
end

func StatusLabel(status)
    return match status do
        case Pending then "Awaiting processing"
        case Processing(started) then "Processing since " + started
        case Shipped(tracking, carrier) then carrier + " - " + tracking
        case Delivered(when) then "Delivered " + when
        case Cancelled(reason) then "Cancelled: " + reason
    end
end
```

### Typed messages

```jyro
union Notification
    Email(to: string, subject: string, body: string)
    Push(userId: string, title: string, message: string)
    Webhook(url: string, payload)
end

func BuildNotification(user, event)
    if user.prefersPush then
        return Push(user.id, event.title, event.summary)
    end
    if HasProperty(user, "webhookUrl") and user.webhookUrl is string then
        return Webhook(user.webhookUrl, { "event": event.type, "data": event })
    end
    return Email(user.email, event.title, event.detail)
end

Data.notifications = Map(Data.users, (u) => BuildNotification(u, Data.event))
```

## When to use unions vs ad-hoc objects

| Situation | Approach |
|-----------|----------|
| Shapes are known at script-write time | Union |
| Shapes come from external data (API, user input) | Ad-hoc objects with `is` checks |
| You need exhaustiveness checking | Union |
| You have 2-3 simple cases in one place | `if`/`elseif` with `is` checks |
| Multiple functions operate on the same set of shapes | Union |
| The set of shapes might grow and you want the compiler to catch missing cases | Union |
