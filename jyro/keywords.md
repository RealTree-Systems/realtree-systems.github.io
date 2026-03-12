---
layout: default
title: Reserved Keywords
parent: Jyro Language Reference
has_children: false
has_toc: false
permalink: /jyro/keywords/
nav_order: 40
---

# Keywords

The following identifiers are reserved and cannot be used as variable names.

## Types

`number`, `string`, `boolean`, `object`, `array`

## Declaration

`var`, `func`

## Control flow

`if`, `then`, `elseif`, `else`, `end`, `switch`, `case`, `default`, `return`, `exit`, `fail`

## Mutation

`delete`

## Unions and pattern matching

`union`, `match`

## Iteration

`for`, `foreach`, `in`, `to`, `downto`, `by`, `do`, `while`, `break`, `continue`

## Logical operators

`and`, `or`, `not`

## Type checking

`is`

## Literals

`true`, `false`, `null`

## Special identifier

`Data` - the built-in data context object. Cannot be declared with `var` or reassigned.

## Data context property names

Keywords are valid as property names on objects. `Data.for`, `Data.number`, and `Data.string` are all legal property access expressions. This does not extend to local variable declarations - `var for = "x"` is a syntax error.
