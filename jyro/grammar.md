---
layout: default
title: Language Grammar
parent: Jyro Language Reference
has_children: false
has_toc: true
permalink: /jyro/grammar/
nav_order: 180
---

# Jyro Language Grammar

Formal grammar specification for the Jyro scripting language, expressed in Extended Backus-Naur Form (EBNF).

## Notation

| Symbol     | Meaning                                |
|------------|----------------------------------------|
| `=`        | Definition                             |
| `;`        | End of rule                            |
| `\|`       | Alternative                            |
| `,`        | Concatenation                          |
| `{ ... }`  | Repetition (zero or more)              |
| `[ ... ]`  | Optional (zero or one)                 |
| `( ... )`  | Grouping                               |
| `" ... "`  | Terminal string                        |
| `' ... '`  | Terminal string (alternate quoting)    |
| `(* ... *)` | Comment                               |

---

## 1. Lexical grammar

### 1.1 Whitespace and comments

```ebnf
whitespace = ? any Unicode whitespace character ? ;
line_comment = "#" , { ? any character except newline ? } ;
ws = { whitespace | line_comment } ;
```

### 1.2 Identifiers

```ebnf
letter = "A" | "B" | ... | "Z" | "a" | "b" | ... | "z" ;
digit = "0" | "1" | ... | "9" ;
identifier_start = letter | "_" ;
identifier_char = letter | digit | "_" ;

identifier = identifier_start , { identifier_char } ;  (* must not be a keyword *)
```

### 1.3 Keywords

The following words are reserved and cannot be used as identifiers:

```
var       if        then      elseif    else      end
switch    do        case      default   while     for
foreach   in        return    fail      break     continue
and       or        not       is        true      false
null      number    string    boolean   object    array
to        downto    by        func      exit      union
match     delete
```

### 1.4 Literals

#### Number literals

```ebnf
hex_digit = digit | "A" | "B" | "C" | "D" | "E" | "F"
                   | "a" | "b" | "c" | "d" | "e" | "f" ;
binary_digit = "0" | "1" ;

decimal_literal = [ "-" ] , digit , { digit } , [ "." , digit , { digit } ] ;
hex_literal = [ "-" ] , "0x" , hex_digit , { hex_digit } ;
binary_literal = [ "-" ] , "0b" , binary_digit , { binary_digit } ;

number_literal = hex_literal | binary_literal | decimal_literal ;
```

#### String literals

```ebnf
escape_sequence = "\" , ( "n" | "r" | "t" | "\" | '"' | "'" | "0"
                        | "u" , hex_digit , hex_digit , hex_digit , hex_digit ) ;

double_string = '"' , { escape_sequence | ? any character except '"' and '\' ? } , '"' ;
single_string = "'" , { escape_sequence | ? any character except "'" and '\' ? } , "'" ;

string_literal = double_string | single_string ;
```

#### Boolean and null literals

```ebnf
boolean_literal = "true" | "false" ;
null_literal = "null" ;

literal = number_literal | string_literal | boolean_literal | null_literal ;
```

### 1.5 Operators and punctuation

#### Arithmetic operators

| Operator | Description    |
|----------|----------------|
| `+`      | Addition       |
| `-`      | Subtraction    |
| `*`      | Multiplication |
| `/`      | Division       |
| `%`      | Modulo         |

#### Comparison operators

| Operator | Description           |
|----------|-----------------------|
| `==`     | Equal                 |
| `!=`     | Not equal             |
| `<`      | Less than             |
| `<=`     | Less than or equal    |
| `>`      | Greater than          |
| `>=`     | Greater than or equal |

#### Logical operators

| Operator | Description      |
|----------|------------------|
| `and`    | Logical AND      |
| `or`     | Logical OR       |
| `not`    | Logical NOT      |

#### Assignment operators

| Operator | Description          |
|----------|----------------------|
| `=`      | Assign               |
| `+=`     | Add and assign       |
| `-=`     | Subtract and assign  |
| `*=`     | Multiply and assign  |
| `/=`     | Divide and assign    |
| `%=`     | Modulo and assign    |

#### Other operators

| Operator  | Description                              |
|-----------|------------------------------------------|
| `??`      | Null coalescing                          |
| `? :`     | Ternary conditional                      |
| `is`      | Type check                               |
| `is not`  | Negated type check                       |
| `++`      | Increment (prefix or postfix)            |
| `--`      | Decrement (prefix or postfix)            |
| `.`       | Property access                          |
| `[ ]`     | Index access                             |
| `( )`     | Function call / grouping                 |
| `=>`      | Lambda arrow                             |

#### Punctuation

| Symbol | Usage                           |
|--------|---------------------------------|
| `(`    | Open parenthesis                |
| `)`    | Close parenthesis               |
| `[`    | Open bracket                    |
| `]`    | Close bracket                   |
| `{`    | Open brace                      |
| `}`    | Close brace                     |
| `,`    | Separator                       |
| `:`    | Type annotation / object pair / named argument |

---

## 2. Syntactic grammar

### 2.1 Program structure

```ebnf
program = ws , { top_level_item } , EOF ;
top_level_item = func_def | union_def | statement ;
block = { statement } ;
```

### 2.2 Statements

```ebnf
statement = var_decl
          | if_stmt
          | while_stmt
          | for_stmt
          | foreach_stmt
          | switch_stmt
          | match_stmt
          | return_stmt
          | exit_stmt
          | fail_stmt
          | break_stmt
          | continue_stmt
          | delete_stmt
          | assignment_stmt
          | expr_stmt ;
```

#### Function definition

```ebnf
param = identifier , [ ":" , type_keyword ] , [ "=" , literal ] ;
param_list = param , { "," , param } ;

func_def = "func" , identifier , "(" , [ param_list ] , ")" , block , "end" ;
```

The optional `= literal` assigns a default value, making the parameter optional. Required parameters must precede optional parameters.

#### Union definition

```ebnf
variant_field = identifier , [ ":" , type_keyword ] ;
variant_fields = variant_field , { "," , variant_field } ;
variant = identifier , "(" , [ variant_fields ] , ")" ;

union_def = "union" , identifier , { variant } , "end" ;
```

#### Variable declaration

```ebnf
type_keyword = "number" | "string" | "boolean" | "object" | "array" ;

var_decl = "var" , identifier , [ ":" , type_keyword ] , [ "=" , expression ] ;
```

#### Assignment

```ebnf
assign_op = "=" | "+=" | "-=" | "*=" | "/=" | "%=" ;
assign_target = identifier | property_access | index_access ;

assignment_stmt = assign_target , [ ":" , type_keyword ] , assign_op , expression ;
```

The optional type hint (`: type_keyword`) is only valid with the `=` operator and must appear on the first assignment to that name.

#### If statement

```ebnf
if_stmt = "if" , expression , "then" , block ,
          { "elseif" , expression , "then" , block } ,
          [ "else" , block ] ,
          "end" ;
```

#### While loop

```ebnf
while_stmt = "while" , expression , "do" , block , "end" ;
```

#### For loop

```ebnf
for_direction = "to" | "downto" ;

for_stmt = "for" , identifier , "in" , expression ,
           for_direction , expression ,
           [ "by" , expression ] ,
           "do" , block , "end" ;
```

#### ForEach loop

```ebnf
foreach_stmt = "foreach" , identifier , "in" , expression ,
               "do" , block , "end" ;
```

#### Switch statement

```ebnf
case_values = expression , { "," , expression } ;

switch_case = "case" , case_values , "then" , block ;
default_case = "default" , "then" , block ;

switch_stmt = "switch" , expression , "do" ,
              { switch_case } ,
              [ default_case ] ,
              "end" ;
```

#### Match statement

```ebnf
match_binding = identifier ;
match_bindings = match_binding , { "," , match_binding } ;

match_case_stmt = "case" , identifier , [ "(" , match_bindings , ")" ] , "then" , block ;
match_case_expr = "case" , identifier , [ "(" , match_bindings , ")" ] , "then" , expression ;

match_stmt = "match" , expression , "do" ,
             { match_case_stmt } ,
             "end" ;

match_expr = "match" , expression , "do" ,
             { match_case_expr } ,
             "end" ;
```

#### Return statement

```ebnf
return_stmt = "return" , [ expression ] ;  (* only valid inside func_def; expression must begin on same line *)
```

#### Exit statement

```ebnf
exit_stmt = "exit" , [ expression ] ;  (* expression must begin on same line *)
```

#### Fail statement

```ebnf
fail_stmt = "fail" , [ expression ] ;  (* expression must begin on same line *)
```

#### Break and continue

```ebnf
break_stmt = "break" ;
continue_stmt = "continue" ;
```

#### Delete statement

```ebnf
delete_target = property_access | index_access ;

delete_stmt = "delete" , delete_target ;  (* target must begin on same line *)
```

#### Expression statement

```ebnf
expr_stmt = expression ;
```

### 2.3 Expressions

Expressions are listed below in order of increasing precedence. Each level binds tighter than the one above it.

```ebnf
expression = ternary_expr ;
```

#### Precedence 1: ternary conditional (lowest)

```ebnf
ternary_expr = coalesce_expr , [ "?" , expression , ":" , expression ] ;
```

#### Precedence 2: null coalescing (right-associative)

```ebnf
coalesce_expr = or_expr , { "??" , or_expr } ;  (* right-associative *)
```

#### Precedence 3: logical OR

```ebnf
or_expr = and_expr , { "or" , and_expr } ;
```

#### Precedence 4: logical AND

```ebnf
and_expr = not_expr , { "and" , not_expr } ;
```

#### Precedence 5: logical NOT (prefix unary)

```ebnf
not_expr = "not" , not_expr
         | type_check_expr ;
```

#### Precedence 6: type check

```ebnf
type_check_expr = equality_expr , [ "is" , [ "not" ] , type_keyword ] ;
```

#### Precedence 7: equality

```ebnf
equality_op = "==" | "!=" ;
equality_expr = relational_expr , { equality_op , relational_expr } ;
```

#### Precedence 8: relational

```ebnf
relational_op = "<" | "<=" | ">" | ">=" ;
relational_expr = additive_expr , { relational_op , additive_expr } ;
```

#### Precedence 9: additive

```ebnf
additive_op = "+" | "-" ;
additive_expr = multiplicative_expr , { additive_op , multiplicative_expr } ;
```

#### Precedence 10: multiplicative

```ebnf
multiplicative_op = "*" | "/" | "%" ;
multiplicative_expr = unary_expr , { multiplicative_op , unary_expr } ;
```

#### Precedence 11: unary prefix

```ebnf
unary_expr = "++" , postfix_expr       (* prefix increment *)
           | "--" , postfix_expr       (* prefix decrement *)
           | "-" , unary_expr          (* numeric negation *)
           | postfix_expr ;
```

#### Precedence 12: postfix

```ebnf
postfix_expr = primary_expr , { postfix_op } ;

named_arg = identifier , ":" , expression ;

postfix_op = "(" , [ arg_list ] , ")"                               (* function call *)
           | "." , identifier                                       (* property access *)
           | "[" , expression , "]"                                 (* index access *)
           | "++"                                                   (* postfix increment *)
           | "--" ;                                                 (* postfix decrement *)

arg_list = named_arg , { "," , named_arg }                          (* all named *)
         | expression , { "," , expression } ;                      (* all positional *)
```

Note: Function calls are only valid when the base expression is an identifier. The call `Name(args)` invokes the function `Name`. A call must use either all positional or all named arguments - the two styles cannot be mixed.

#### Precedence 13: primary (highest)

```ebnf
primary_expr = lambda_expr
             | match_expr
             | null_literal
             | boolean_literal
             | number_literal
             | string_literal
             | array_literal
             | object_literal
             | grouped_expr
             | identifier ;
```

#### Composite literals

```ebnf
array_literal = "[" , [ expression , { "," , expression } ] , "]" ;

object_key = identifier | string_literal ;
object_property = object_key , ":" , expression ;
object_literal = "{" , [ object_property , { "," , object_property } ] , "}" ;
```

#### Lambda expressions

```ebnf
lambda_params = "(" , [ identifier , { "," , identifier } ] , ")"
              | identifier ;

lambda_expr = lambda_params , "=>" , expression ;
```

#### Grouped expression

```ebnf
grouped_expr = "(" , expression , ")" ;
```

---

## 3. Operator precedence summary

From lowest to highest precedence:

| Prec. | Operator(s)                     | Associativity | Description           |
|------:|---------------------------------|---------------|-----------------------|
|     1 | `? :`                           | Right         | Ternary conditional   |
|     2 | `??`                            | Right         | Null coalescing       |
|     3 | `or`                            | Left          | Logical OR            |
|     4 | `and`                           | Left          | Logical AND           |
|     5 | `not`                           | Prefix        | Logical NOT           |
|     6 | `is` , `is not`                 | Postfix       | Type check            |
|     7 | `==` , `!=`                     | Left          | Equality              |
|     8 | `<` , `<=` , `>` , `>=`         | Left          | Relational            |
|     9 | `+` , `-`                       | Left          | Additive              |
|    10 | `*` , `/` , `%`                 | Left          | Multiplicative        |
|    11 | `-` , `++` , `--`               | Prefix        | Unary prefix          |
|    12 | `()` , `.` , `[]` , `++` , `--` | Left/Postfix  | Postfix / access      |

---

## 4. Type system

Jyro supports the following runtime types. Type keywords are used in variable declarations and `is` expressions.

| Type Keyword | Values                                     |
|--------------|--------------------------------------------|
| `number`     | IEEE 754 double-precision floating point    |
| `string`     | Unicode character sequence                  |
| `boolean`    | `true` or `false`                           |
| `object`     | Key-value map with string keys              |
| `array`      | Ordered, indexable collection of values     |
| `null`       | The singleton `null` value                  |

---

## 5. Grammar Notes

1. **Semicolons**: Jyro does not use semicolons. Statements are separated by whitespace.

2. **Block delimiters**: All compound statements use keyword pairs (`then`/`end`, `do`/`end`).

3. **Same-line constraint**: For `return`, `exit`, `fail`, and `delete` statements, the expression must begin on the same line as the keyword. For `return`, `exit`, and `fail` a newline after the keyword is interpreted as a bare statement. For `delete` the target is required.

4. **Object property keys**: Object literal keys may be unquoted identifiers (including keywords like `and` or `true`) or quoted strings.

5. **Assignment targets**: Only identifiers, property access expressions, and index access expressions are valid on the left-hand side of an assignment.

6. **For loop semantics**: The `to` keyword indicates ascending iteration; `downto` indicates descending. The optional `by` clause specifies the step value.

7. **Switch fallthrough**: There is no fallthrough between cases. Each `case` block is implicitly terminated by the next `case`, `default`, or the closing `end` of the `switch` statement.

8. **Comments**: Only single-line comments are supported, beginning with `#`.

9. **Function definitions**: `func` declarations must appear at the top level. `return` is only valid inside function bodies. Functions cannot be nested. Parameters may have default values (`= literal`), but required parameters must precede optional ones.

10. **Named arguments**: A function call may use named arguments (`paramName: value`) instead of positional arguments. All arguments in a single call must be either all named or all positional - the two styles cannot be mixed. When using named arguments, arguments may appear in any order, and optional parameters can be omitted.

11. **Union definitions**: `union` declarations must appear at the top level. Variant names must be globally unique across all unions and cannot collide with built-in or user-defined function names.

12. **Match exhaustiveness**: Every `match` must cover all variants of the union being matched. There is no `default` case in `match`.

13. **Delete targets**: The `delete` statement only accepts property access and index access expressions. Bare identifiers are not valid delete targets - variables cannot be deleted. The target expression must begin on the same line as `delete`.
