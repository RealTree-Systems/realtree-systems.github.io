---
layout: default
title: Gotchas & Edge Cases
parent: Jyro Language Reference
has_children: false
has_toc: true
permalink: /jyro/gotchas/
nav_order: 130
---

# Gotchas & Edge Cases

This page documents important Jyro behaviours and the correct way to handle them.

## 1. Empty arrays and objects are truthy

Jyro treats all arrays and objects as truthy - even empty ones. This means an `if` check on a collection always passes, regardless of whether it contains any elements.

Check the length explicitly instead:

```jyro
# INCORRECT - always enters the block, even when Data.items is []
if Data.items then ... end

# CORRECT - tests whether the array actually has elements
if Length(Data.items) > 0 then ... end
```

## 2. ToNumber() returns null for invalid input

When `ToNumber` cannot parse a string as a number it returns `null`, not `0`. Code that assumes a numeric result without checking will propagate `null` through subsequent calculations.

Always test the return value before using it:

```jyro
var num = ToNumber(Data.input)
if num is null then
    fail "Invalid number: " & Data.input
end
```

## 3. Division by zero terminates the script

Division or modulo by zero throws an immediate runtime error that terminates the script.

Guard every division with a zero check:

```jyro
if count != 0 then
    Data.average = total / count
else
    Data.average = 0
end
```

## 4. All array functions return new arrays

Every standard library function that operates on an array returns a **new** array. The original is never mutated. Assign the return value to capture the result.

Assign the result back to the variable:

```jyro
var items = [3, 1, 2]
Sort(items)                # items is STILL [3, 1, 2] - result discarded
var sorted = Sort(items)   # sorted is [1, 2, 3]

var items = [1, 2]
Append(items, 3)           # items is STILL [1, 2] - result discarded
items = Append(items, 3)   # NOW items is [1, 2, 3]
```

## 5. Cannot modify a collection during iteration

Reassigning the array that a `foreach` loop is iterating over causes a runtime error. The iterator holds a reference to the original collection and detects the mutation.

Collect changes into a separate array, then apply them after the loop:

```jyro
# INCORRECT - runtime error: modifying the iterated collection
foreach item in Data.items do
    Data.items = Append(Data.items, item)
end

# CORRECT - accumulate into a separate array, merge afterwards
var additions = []
foreach item in Data.items do
    if item.shouldDuplicate then
        additions = Append(additions, item)
    end
end
foreach addition in additions do
    Data.items = Append(Data.items, addition)
end
```

## 6. Nested properties need intermediate objects

Assigning to a deeply nested path like `Data.a.b.c` throws a runtime error if the intermediate objects (`Data.a`, `Data.a.b`) do not already exist. Property access on a missing key returns `null`, and attempting to set a property on `null` (or any non-object type) is a runtime error. Jyro does not auto-create parent objects on assignment (i.e. no autovivification).

Create each level explicitly, or assign a nested literal:

```jyro
# INCORRECT - runtime error: cannot set property on null
Data.deep.nested.value = "x"

# CORRECT - build the path one level at a time
Data.deep = {}
Data.deep.nested = {}
Data.deep.nested.value = "x"

# ALSO CORRECT - assign the whole structure as a literal
Data.deep = {"nested": {"value": "x"}}
```

## 7. Missing `then` after `if`/`elseif`/`case`/`default`

The `then` keyword is required after every `if` condition, `elseif` condition, `case` value list, and `default` keyword. Omitting it causes a parse error.

```jyro
# INCORRECT - parse error
if x > 0
    ...
end

# CORRECT
if x > 0 then
    ...
end
```

## 8. Missing `do` after `while`/`for`/`foreach`

Loop statements require the `do` keyword after the condition or iteration clause. Omitting it causes a parse error.

```jyro
# INCORRECT - parse error
while x > 0
    ...
end

# CORRECT
while x > 0 do
    ...
end
```

## 9. Missing `end` for blocks

Jyro is explicit about block delimiters. Every `if`, `while`, `for`, `foreach`, and `switch` block must be closed with `end`. Inside a `switch`, each `case` and `default` block is implicitly terminated by the next `case`, `default`, or the closing `end`. A missing `end` causes a parse error at the end of the script or at the start of the next block.

## 10. No string interpolation — use `&` not `+`

Jyro does not support template syntax like `${name}` or `{name}` inside strings. These are treated as literal characters, not variable references. Use the `&` operator to concatenate values into strings:

```jyro
# INCORRECT - produces the literal text "${name}", not the variable's value
"Hello ${name}"
"Hello {name}"

# ALSO INCORRECT - runtime or compile-time error: + requires numeric operands
"Hello " + name

# CORRECT - & converts the variable to a string
"Hello " & name
```

## 10a. `+` is numeric-only

The `+` operator is restricted to numeric addition. Using `+` with a string operand is a **compile-time error** when the string is statically known (string literal or type-hinted variable), or a **runtime error** otherwise. Always use `&` for string concatenation:

```jyro
# INCORRECT - compile-time error (string literal operand)
var greeting = "Hello, " + name

# INCORRECT - runtime error (dynamic string value)
var s = Data.someString
var result = s + " suffix"

# CORRECT
var greeting = "Hello, " & name
var result = s & " suffix"
```

## 11. Min()/Max()/Sum()/Average() take arrays, not multiple arguments

The aggregate math functions each accept a **single array argument**, not variadic arguments.

Wrap the values in an array literal:

```jyro
# INCORRECT - too many arguments
Sum(1, 2, 3)
Min(a, b)

# CORRECT - pass a single array
Sum([1, 2, 3])
Min([a, b])
```

## 12. Merge() takes an array of objects

Like the aggregate math functions, `Merge` accepts a single array of objects to merge together. It does not accept multiple object arguments.

```jyro
# INCORRECT - too many arguments
Merge(obj1, obj2)

# CORRECT - pass an array of objects (later entries override earlier ones)
Merge([obj1, obj2])
```

## 13. GroupBy() takes a field name string, not a lambda

`GroupBy` groups objects by a named field and expects a plain string, not a lambda expression. This is consistent with other field-based query functions like `WhereByField` and `SortByField`.

```jyro
# INCORRECT - GroupBy does not accept lambdas
GroupBy(arr, x => x.category)

# CORRECT - pass the field name as a string
GroupBy(arr, "category")
```

## 14. Regex patterns - no `\d`, `\w`, `\s`

Jyro's string lexer only recognises the escape sequences `\n`, `\r`, `\t`, `\\`, `\"`, `\'`, `\0`, and `\uXXXX`. Any other backslash sequence - including `\d`, `\w`, and `\s` - causes a **parse error**. Use equivalent character classes instead:

```jyro
# INCORRECT - parse error: \d is not a recognised escape sequence
RegexMatch(text, "\d+")

# CORRECT - character class reaches the regex engine intact
RegexMatch(text, "[0-9]+")
```

See [Strings - Regex Pattern Escaping](/jyro/literals/strings/#regex-pattern-escaping) for the full substitution table.

## 15. `exit`/`fail` message must be on the same line

The `exit` and `fail` keywords accept an optional message, but it must appear on the **same line**. A string on the next line is parsed as a separate expression statement, not as the message. The same rule applies to `return` inside functions.

```jyro
# INCORRECT - "message" is a standalone expression, not the exit message
exit
"message"

# CORRECT - message on the same line
exit "message"
```

## 16. `for` loop bounds are exclusive

The upper bound of `to` and the lower bound of `downto` are exclusive, similar to `<` and `>` respectively. The loop variable never reaches the boundary value. To include the boundary, adjust it by one:

```jyro
for i in 0 to 5 do     # iterates: 0, 1, 2, 3, 4 (NOT 5)
    ...
end

for i in 5 downto 0 do # iterates: 5, 4, 3, 2, 1 (NOT 0)
    ...
end

# To include 5:
for i in 0 to 5 + 1 do # iterates: 0, 1, 2, 3, 4, 5
    ...
end
```

## 17. Function names are PascalCase and case-sensitive

All standard library functions use PascalCase naming (e.g. `ToUpper`, `WhereByField`). Function lookup is case-sensitive, so `toupper` or `toUpper` will fail with an "unknown function" error.

```jyro
# INCORRECT - case mismatch
toupper("hello")
toUpper("hello")

# CORRECT
ToUpper("hello")
```

## Return values on empty or missing data

| Function | Condition | Returns |
|----------|-----------|---------|
| `First(arr)` | Empty array | `null` |
| `Last(arr)` | Empty array | `null` |
| `IndexOf(arr, val)` | Not found | `-1` |
| `WhereByField(...)` | No matches | `[]` (empty array) |
| `FindByField(...)` | No match | `null` |
| `Find(arr, fn)` | No match | `null` |
| `AnyByField(...)` | Empty array | `false` |
| `AllByField(...)` | Empty array | `true` (vacuous truth) |
| `Any(arr, fn)` | Empty array | `false` |
| `All(arr, fn)` | Empty array | `true` (vacuous truth) |
| `Select(...)` | Empty array | `[]` |
| `SelectMany(...)` | Empty array | `[]` |
| `Project(...)` | Empty array | `[]` |
| `Omit(...)` | Empty array | `[]` |
| `Merge(arr)` | Empty array | `{}` |
| `Min(arr)` | No numbers | `null` |
| `Max(arr)` | No numbers | `null` |
| `Average(arr)` | No numbers | `null` |
| `Median(arr)` | No numbers | `null` |
| `Mode(arr)` | No numbers | `null` |
| `Sum(arr)` | No numbers | `0` |
| `ToNumber(s)` | Invalid string | `null` |
| `RegexMatch(...)` | No match | `null` |
| `RegexMatchAll(...)` | No matches | `[]` |
| `RegexMatchDetail(...)` | No match | `null` |
| `RandomChoice(arr)` | Empty array | `null` |

## Case sensitivity

- String comparisons (`==`, `!=`, `<`, `>`) are case-sensitive
- `Contains`, `StartsWith`, `EndsWith` are case-sensitive
- Query function comparisons are case-sensitive
- Use `ToLower()` or `ToUpper()` for case-insensitive matching

## Sort order for mixed types

`Sort` groups elements by type, then sorts within each group:

1. `null` values (first)
2. Numbers (ascending)
3. Strings (lexicographic)
4. Booleans (`false` before `true`)

### Split() preserves empty strings

The `Split` function preserves empty strings produced by adjacent or leading/trailing delimiters:

```jyro
Split("a,,b", ",")        # ["a", "", "b"]
Split(",a,b,", ",")       # ["", "a", "b", ""]
```

## 18. `return` is only valid inside functions

`return` exits a user-defined function. Using `return` at the script's top level is a compiler error. Use `exit` for clean script termination and `fail` for error termination:

```jyro
# INCORRECT - compiler error: return outside a function
return "done"

# CORRECT - use exit at the top level
exit "done"
```

## 19. Functions cannot access `Data`

User-defined functions are pure - they cannot read or write `Data`. This is enforced by the compiler. Pass the values you need as arguments:

```jyro
# INCORRECT - compiler error: cannot access Data inside a function
func Bad()
    return Data.value
end

# CORRECT - pass the value as a parameter
func Good(value)
    return value * 2
end
Data.result = Good(Data.value)
```

## 20. Functions cannot capture variables (no closures)

Functions only see their own parameters and locally declared variables. Variables from the enclosing script are not visible:

```jyro
var multiplier = 3

# INCORRECT - compiler error: 'multiplier' is not declared
func Scale(x)
    return x * multiplier
end

# CORRECT - pass it as a parameter
func Scale(x, multiplier)
    return x * multiplier
end
```

Lambdas _do_ capture variables from the surrounding scope. See [Lambda Expressions](/jyro/lambda-expressions/#lambdas-vs-user-defined-functions) for the comparison.

## 21. Functions and unions must be declared at the top level

`func` and `union` declarations must appear at the script's top level - not inside `if` blocks, loops, or other functions:

```jyro
# INCORRECT - compiler error
if Data.mode == "advanced" then
    func Helper(x)
        return x * 2
    end
end

# CORRECT - declare at the top level
func Helper(x)
    return x * 2
end
```

## 22. Variant names are globally unique

Each variant name in a `union` must be unique across all unions in the script. Variant names also cannot collide with built-in or user-defined function names, because constructors are registered as functions:

```jyro
# INCORRECT - 'Round' collides with built-in function
union Shape
    Round(radius: number)    # OK - no collision
end

# INCORRECT - 'Circle' in two unions
union Shape2D
    Circle(r: number)
end
union Shape3D
    Circle(r: number)       # Error: duplicate variant name
end
```

## 23. `match` has no `default` - exhaustiveness is required

Unlike `switch`, `match` has no `default` case. Every variant of the union must be handled. If you forget a variant, the compiler reports an error:

```jyro
union Direction
    North()
    South()
    East()
    West()
end

# INCORRECT - compiler error: missing variants South, West
match heading do
    case North() then ...
    case East() then ...
end
```

## 24. `match` bindings are positional, not by name

In a `match` case, bindings map to fields by position, not by name. The names you use in the `case` can differ from the field names in the union declaration:

```jyro
union Rect
    Rect(width: number, height: number)
end

# 'w' binds to the first field (width), 'h' to the second (height)
match shape do
    case Rect(w, h) then w * h
end
```
