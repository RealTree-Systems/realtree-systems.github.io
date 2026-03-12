---
layout: default
title: Error Codes
parent: Jyro Language Reference
has_children: false
has_toc: true
permalink: /jyro/error-codes/
nav_order: 170
---

# Error Codes

Every error produced by the Jyro pipeline has a unique code in the format **JMXXXX** (e.g. `JM5200` for division by zero). These codes are stable, searchable identifiers that tell you exactly which subsystem produced the error and what went wrong.

## Code layout

Codes follow the pattern **XYZZ**:

- **X** - Pipeline stage (1–5)
- **Y** - Category within that stage
- **ZZ** - Specific error

| Range | Stage | Description |
|-------|-------|-------------|
| 1xxx | Lexer | Tokenization errors (unexpected characters, unterminated strings) |
| 2xxx | Parser | Syntax errors (unexpected tokens, malformed literals) |
| 3xxx | Validator | Semantic checks before execution (undeclared variables, type mismatches, unreachable code) |
| 4xxx | Linker | Function resolution (undefined functions, argument count mismatches) |
| 5xxx | Runtime | Errors during script execution |

## Runtime categories (5xxx)

Runtime errors are further grouped by the hundreds digit:

| Range | Category | Examples |
|-------|----------|----------|
| 50xx | General / System | Script return, cancellation, catch-all errors |
| 51xx | Type & Casting | Invalid type assignment, argument type mismatch, missing arguments |
| 52xx | Arithmetic & Operators | Division by zero, incompatible operand types, unknown operators |
| 53xx | Index & Property Access | Index out of range, property access on null |
| 54xx | Type Checking & Iteration | Unknown type name, not iterable |
| 55xx | Function & Script Resolution | Undefined function, script not found |
| 56xx | Control Flow & Expression | Invalid for-loop step, number parse failure |
| 57xx | String & Pattern Operations | Regex timeout, invalid regex pattern, pad length exceeded |
| 58xx | Date & Time Operations | Date parse error, invalid date unit |
| 59xx | Resource Limits & Encoding | Statement/time/depth limits exceeded, Base64 decode error |

## Error format

Errors are formatted as:

```
[JMXXXX] Ln N, Col N: message
```

For example:

```
[JM5200] Ln 12, Col 5: Division by zero
[JM3100] Ln 3, Col 1: Undeclared variable 'foo'
[JM4200] Ln 7, Col 10: Function 'Process' requires at least 2 arguments, but 1 were provided
```

This format is deterministically parseable via regex (e.g. `\[JM(\d{4})\] Ln (\d+), Col (\d+): (.+)`) when outputted to stderr.

All errors include source location (line and column).

## Structured diagnostics

For embedded hosts, every diagnostic carries structured data that can be used for programmatic handling, custom formatting, or localization:

- **Code** - the JMXXXX string
- **NumericCode** - the integer value (e.g. 5200)
- **Severity** - `error`, `warning`, or `info`
- **Message** - the pre-formatted English message
- **Args** - the raw arguments used to build the message (e.g. the variable name, the expected type)
- **Line / Column / Length** - source location and span of the offending token
- **Subsystem** - `lexer`, `parser`, `validator`, `linker`, or `runtime`

The `Args` array allows hosts to re-format messages using their own templates. For example, a host providing French localization could map `JM5100` to `"Impossible d'assigner {0} à la variable '{1}' de type {2}"` and format the structured args into that template, while the default English message is always available as a fallback.

## Full code reference

### 1xxx - Lexer

| Code | Name | Message |
|------|------|---------|
| 1000 | UnknownLexerError | *(varies)* |
| 1100 | UnexpectedCharacter | Unexpected character '{0}' |
| 1200 | UnterminatedString | Unterminated string literal |

### 2xxx - Parser

| Code | Name | Message |
|------|------|---------|
| 2000 | UnknownParserError | *(varies)* |
| 2100 | UnexpectedToken | Unexpected token '{0}' |
| 2101 | MissingToken | Expected '{0}' |
| 2200 | InvalidNumberFormat | Invalid number format: '{0}' |

### 3xxx - Validator

| Code | Name | Message |
|------|------|---------|
| 3000 | UnknownValidatorError | *(varies)* |
| 3100 | UndeclaredVariable | Undeclared variable '{0}' |
| 3101 | VariableAlreadyDeclared | Variable '{0}' is already declared |
| 3102 | ReservedIdentifier | '{0}' is a reserved identifier |
| 3200 | InvalidAssignmentTarget | Invalid assignment target |
| 3300 | TypeMismatch | *(varies)* |
| 3400 | LoopStatementOutsideOfLoop | {0} statement outside of loop |
| 3401 | ExcessiveLoopNesting | Loop nesting exceeds maximum depth of {0} |
| 3402 | NotIterableLiteral | Value of type {0} is not iterable |
| 3403 | UnreachableCode | Unreachable code detected |
| 3500 | ReturnOutsideFunction | 'return' can only be used inside a function |
| 3501 | DataAccessInsideFunction | Cannot access 'Data' inside a function |
| 3502 | UndeclaredVariableInFunction | Undeclared variable '{0}' in function '{1}' |
| 3503 | FunctionNotAtTopLevel | Function declarations must be at the top level |
| 3504 | NestedFunction | Functions cannot be nested inside other functions |
| 3505 | DuplicateParameter | Duplicate parameter name '{0}' |
| 3506 | RequiredParamAfterOptional | Required parameter '{0}' cannot follow optional parameter '{1}' in function '{2}' |
| 3507 | DefaultValueNotLiteral | Default value for parameter '{0}' in function '{1}' must be a literal value |
| 3600 | UnionNotAtTopLevel | Union declarations must be at the top level |
| 3601 | DuplicateVariant | Variant '{0}' is already defined |
| 3602 | DuplicateVariantField | Duplicate field name '{0}' in variant '{1}' |
| 3603 | NonExhaustiveMatch | Non-exhaustive match - missing variants: {0} |
| 3604 | MatchBindingCountMismatch | Variant '{0}' has {1} fields but {2} bindings were provided |
| 3605 | UnknownVariant | Unknown variant '{0}' in match |
| 3606 | DuplicateMatchCase | Duplicate case for variant '{0}' |

### 4xxx - Linker

| Code | Name | Message |
|------|------|---------|
| 4000 | UnknownLinkerError | *(varies)* |
| 4100 | UndefinedFunction | Undefined function '{0}' |
| 4101 | DuplicateFunction | Function '{0}' is already defined |
| 4102 | FunctionOverride | Function '{0}' overrides a built-in function |
| 4200 | TooFewArguments | Function '{0}' requires at least {1} arguments, but {2} were provided |
| 4201 | TooManyArguments | Function '{0}' accepts at most {1} arguments, but {2} were provided |
| 4202 | DuplicateNamedArgument | Duplicate named argument '{0}' in call to '{1}' |
| 4203 | UnknownNamedArgument | Unknown parameter '{0}' in call to '{1}'. Available parameters: {2} |
| 4204 | MissingRequiredNamedArgument | Required parameter '{0}' not provided in call to '{1}' |
| 4300 | VariantConstructorConflict | Variant constructor '{0}' conflicts with existing function '{0}' |
| 4301 | VariantUndefined | Variant constructor '{0}' is not defined |

### 5xxx - Runtime

| Code | Name | Message |
|------|------|---------|
| 5000 | ScriptReturn | *(control flow)* |
| 5001 | UnknownExecutorError | *(varies)* |
| 5002 | RuntimeError | *(varies)* |
| 5003 | CancelledByHost | Script execution was cancelled by the host |
| 5004 | ScriptFailure | *(varies)* |
| 5100 | InvalidType | Cannot assign {0} to variable '{1}' of type {2} |
| 5101 | InvalidArgumentType | *(varies)* |
| 5102 | InvalidCast | Cannot cast {0} to {1} |
| 5103 | ArgumentNotProvided | Argument {0} not provided |
| 5104 | ArgumentTypeMismatch | Expected {0} but got {1} |
| 5105 | CallbackExpected | {0} expected a lambda callback but got {1} |
| 5200 | DivisionByZero | Division by zero |
| 5201 | ModuloByZero | Modulo by zero |
| 5202 | NegateNonNumber | Cannot negate non-number value of type {0} |
| 5203 | IncrementDecrementNonNumber | Cannot increment/decrement non-number value of type {0} |
| 5204 | IncompatibleOperandTypes | Incompatible operand types: {0} and {1} |
| 5205 | IncompatibleComparison | Cannot compare {0} and {1} |
| 5206 | UnsupportedBinaryOperation | Unsupported binary operation {0} for {1} |
| 5207 | UnsupportedUnaryOperation | Unsupported unary operation {0} for {1} |
| 5208 | UnknownOperator | Unknown operator '{0}' |
| 5300 | IndexOutOfRange | *(varies)* |
| 5301 | NegativeIndex | Negative index: {0} |
| 5302 | IndexAccessOnNull | Cannot access index on null value |
| 5303 | InvalidIndexTarget | Cannot index into value of type {0} |
| 5304 | PropertyAccessOnNull | Cannot access property '{0}' on null value |
| 5305 | PropertyAccessInvalidType | Cannot access property '{0}' on value of type {1} |
| 5306 | SetPropertyOnNonObject | Cannot set property '{0}' on value of type {1} |
| 5307 | SetIndexOnNonContainer | Cannot set index on value of type {0} |
| 5400 | InvalidTypeCheck | *(varies)* |
| 5401 | UnknownTypeName | Unknown type name '{0}' |
| 5402 | NotIterable | Value of type {0} is not iterable |
| 5500 | UndefinedFunctionRuntime | Undefined function '{0}' |
| 5501 | InvalidFunctionTarget | Cannot call value of type {0} as a function |
| 5502 | ScriptResolverNotConfigured | Script resolver is not configured |
| 5503 | ScriptNotFound | Script '{0}' not found |
| 5600 | ForLoopInvalidStep | For loop step must be a positive number |
| 5601 | InvalidExpressionSyntax | *(varies)* |
| 5602 | InvalidNumberParse | Cannot parse '{0}' as a number |
| 5700 | RegexTimeout | {0}: Pattern matching timed out |
| 5701 | RegexInvalidPattern | {0}: Invalid regex pattern - {1} |
| 5702 | PadLengthExceeded | {0} length {1} exceeds maximum allowed value of {2} |
| 5703 | NegativeLength | {0} requires a non-negative integer length. Received: {1} |
| 5704 | EmptyCharacterSet | {0} character set cannot be empty |
| 5705 | StringOrArrayRequired | {0} requires a string or array as first argument |
| 5800 | DateParseError | Unable to parse date: '{0}' |
| 5801 | DateFormatStringInvalid | Invalid date format string: '{0}' |
| 5802 | DateFormatInvalid | Invalid date format: '{0}' |
| 5803 | DateAddAmountNotInteger | DateAdd() amount must be an integer |
| 5804 | DateUnitInvalid | Invalid date unit: '{0}'. Valid units: {1} |
| 5805 | DatePartInvalid | Invalid date part: '{0}'. Valid parts: {1} |
| 5900 | StatementLimitExceeded | Script execution exceeded maximum statement limit of {0} |
| 5901 | LoopIterationLimitExceeded | Script execution exceeded maximum loop iteration limit of {0} |
| 5902 | CallDepthLimitExceeded | Script execution exceeded maximum call depth limit of {0} |
| 5903 | ExecutionTimeLimitExceeded | Script execution exceeded maximum time limit of {0}ms |
| 5950 | IntegerRequired | {0} requires an integer {1}. Received: {2} |
| 5951 | IncomparableTypes | Cannot compare values of incompatible types: {0} and {1}. Relational operators (<, <=, >, >=) require both values to be numbers, strings, or booleans of the same type. |
| 5952 | UnsupportedComparisonOperator | Unsupported comparison operator: '{0}'. Supported operators are: ==, !=, <, <=, >, >= |
| 5960 | Base64DecodeError | Base64Decode() requires a valid Base64-encoded string. Error: {0} |
