---
layout: default
title: Standard Library
parent: Jyro Language Reference
has_children: true
has_toc: false
permalink: /jyro/stdlib/
nav_order: 120
---

# Jyro Standard Library Reference

A complete reference of all built-in functions available in the Jyro scripting language.

**Immutability:** Jyro follows an immutable-by-default design. Functions return new values rather than modifying their inputs in place. Arguments to standard library functions are never altered.

**Chaining:** Functions that return arrays, strings, or objects can be chained by nesting calls or using the result as input to another function. Functions that return scalars (numbers, booleans) or null are terminal and cannot be meaningfully chained.

---

## Array functions

| Function | Description | Mutates | Chainable |
|----------|-------------|---------|-----------|
| [Append](/jyro/stdlib/array/append/) | Adds an element to the end of an array | No, returns new array | Yes |
| [Concatenate](/jyro/stdlib/array/concatenate/) | Joins two arrays into a single new array | No, returns new array | Yes |
| [Distinct](/jyro/stdlib/array/distinct/) | Removes duplicate elements, keeping first occurrence | No, returns new array | Yes |
| [First](/jyro/stdlib/array/first/) | Returns the first element, or null if empty | No | No |
| [Flatten](/jyro/stdlib/array/flatten/) | Recursively flattens nested arrays into a single level | No, returns new array | Yes |
| [FlattenOnce](/jyro/stdlib/array/flattenonce/) | Flattens nested arrays by one level only | No, returns new array | Yes |
| [GroupBy](/jyro/stdlib/array/groupby/) | Groups array elements by a field value into an object | No, returns new object | Yes (object) |
| [IndexOf](/jyro/stdlib/array/indexof/) | Returns the position of a value in an array or string | No | No |
| [Insert](/jyro/stdlib/array/insert/) | Inserts an element at a specified index | No, returns new array | Yes |
| [Last](/jyro/stdlib/array/last/) | Returns the last element, or null if empty | No | No |
| [Length](/jyro/stdlib/array/length/) | Returns the length of a string, array, or object | No | No |
| [Prepend](/jyro/stdlib/array/prepend/) | Adds an element to the beginning of an array | No, returns new array | Yes |
| [RandomChoice](/jyro/stdlib/array/randomchoice/) | Selects a random element from an array | No | No |
| [Range](/jyro/stdlib/array/range/) | Generates an array of numbers in a range with optional step | No, returns new array | Yes |
| [RemoveAt](/jyro/stdlib/array/removeat/) | Removes the element at a specified index | No, returns new array | Yes |
| [RemoveFirst](/jyro/stdlib/array/removefirst/) | Removes the first element from an array | No, returns new array | Yes |
| [RemoveLast](/jyro/stdlib/array/removelast/) | Removes the last element from an array | No, returns new array | Yes |
| [Reverse](/jyro/stdlib/array/reverse/) | Reverses the order of elements | No, returns new array | Yes |
| [SelectMany](/jyro/stdlib/array/selectmany/) | Flattens a nested array field from each object | No, returns new array | Yes |
| [Skip](/jyro/stdlib/array/skip/) | Skips the first N elements | No, returns new array | Yes |
| [Slice](/jyro/stdlib/array/slice/) | Extracts a portion of an array by index range | No, returns new array | Yes |
| [Sort](/jyro/stdlib/array/sort/) | Sorts elements by type group then value | No, returns new array | Yes |
| [SortByField](/jyro/stdlib/array/sortbyfield/) | Sorts objects by a field in ascending or descending order | No, returns new array | Yes |

## DateTime functions

| Function | Description | Mutates | Chainable |
|----------|-------------|---------|-----------|
| [DateAdd](/jyro/stdlib/datetime/dateadd/) | Adds a time amount to a date | No, returns new string | No |
| [DateDiff](/jyro/stdlib/datetime/datediff/) | Calculates the difference between two dates in a given unit | No | No |
| [DatePart](/jyro/stdlib/datetime/datepart/) | Extracts a component from a date (year, month, day, etc.) | No | No |
| [FormatDate](/jyro/stdlib/datetime/formatdate/) | Formats a date using a .NET format pattern | No, returns new string | No |
| [Now](/jyro/stdlib/datetime/now/) | Returns the current UTC date and time in ISO 8601 | No | No |
| [ParseDate](/jyro/stdlib/datetime/parsedate/) | Parses a date string and normalizes to ISO 8601 | No, returns new string | No |
| [Today](/jyro/stdlib/datetime/today/) | Returns the current UTC date without time | No | No |

## Lambda functions

Higher-order functions that accept inline lambda expressions as arguments.

| Function | Description | Mutates | Chainable |
|----------|-------------|---------|-----------|
| [All](/jyro/stdlib/lambda/all/) | Tests whether all elements satisfy a predicate | No | No |
| [Find](/jyro/stdlib/lambda/find/) | Returns the first element matching a predicate | No | No |
| [Each](/jyro/stdlib/lambda/each/) | Executes a lambda for each element (side effects only) | No, returns null | No |
| [Map](/jyro/stdlib/lambda/map/) | Transforms each element using a lambda | No, returns new array | Yes |
| [Reduce](/jyro/stdlib/lambda/reduce/) | Accumulates an array into a single value | No | No |
| [Any](/jyro/stdlib/lambda/any/) | Tests whether any element satisfies a predicate | No | No |
| [SortBy](/jyro/stdlib/lambda/sortby/) | Sorts an array using a lambda key extractor | No, returns new array | Yes |
| [Where](/jyro/stdlib/lambda/where/) | Filters an array using a lambda predicate | No, returns new array | Yes |

## Math functions

| Function | Description | Mutates | Chainable |
|----------|-------------|---------|-----------|
| [Absolute](/jyro/stdlib/math/absolute/) | Returns the absolute value of a number | No | No |
| [Average](/jyro/stdlib/math/average/) | Returns the arithmetic mean of numeric values | No | No |
| [Ceiling](/jyro/stdlib/math/ceiling/) | Rounds up to the nearest integer | No | No |
| [Clamp](/jyro/stdlib/math/clamp/) | Constrains a number to a min/max range | No | No |
| [Floor](/jyro/stdlib/math/floor/) | Rounds down to the nearest integer | No | No |
| [Log](/jyro/stdlib/math/log/) | Returns the logarithm with an optional base | No | No |
| [Max](/jyro/stdlib/math/max/) | Returns the largest numeric value in an array | No | No |
| [Median](/jyro/stdlib/math/median/) | Returns the median value of a numeric array | No | No |
| [Min](/jyro/stdlib/math/min/) | Returns the smallest numeric value in an array | No | No |
| [Mode](/jyro/stdlib/math/mode/) | Returns the most frequently occurring value | No | No |
| [Power](/jyro/stdlib/math/power/) | Raises a number to a specified power | No | No |
| [RandomInt](/jyro/stdlib/math/randomint/) | Generates a cryptographically secure random integer | No | No |
| [Round](/jyro/stdlib/math/round/) | Rounds a number to a specified precision with configurable tie-breaking | No | No |
| [SquareRoot](/jyro/stdlib/math/squareroot/) | Returns the square root of a number | No | No |
| [Sum](/jyro/stdlib/math/sum/) | Returns the sum of all numeric values in an array | No | No |

## Query functions

Field-based query functions for filtering and projecting arrays of objects.

| Function | Description | Mutates | Chainable |
|----------|-------------|---------|-----------|
| [AllByField](/jyro/stdlib/query/allbyfield/) | Tests whether all objects match a field condition | No | No |
| [AnyByField](/jyro/stdlib/query/anybyfield/) | Tests whether any object matches a field condition | No | No |
| [CountIf](/jyro/stdlib/query/countif/) | Counts objects matching a field condition | No | No |
| [WhereByField](/jyro/stdlib/query/wherebyfield/) | Returns objects matching a field condition | No, returns new array | Yes |
| [FindByField](/jyro/stdlib/query/findbyfield/) | Returns the first object matching a field condition | No | No |
| [Omit](/jyro/stdlib/query/omit/) | Removes specified fields from each object | No, returns new array | Yes |
| [Project](/jyro/stdlib/query/project/) | Extracts specified fields from each object | No, returns new array | Yes |
| [Select](/jyro/stdlib/query/select/) | Extracts a single field value from each object | No, returns new array | Yes |

## Schema functions

| Function | Description | Mutates | Chainable |
|----------|-------------|---------|-----------|
| [ValidateRequired](/jyro/stdlib/schema/validaterequired/) | Checks that an object contains required non-null fields | No | No |
| [ValidateSchema](/jyro/stdlib/schema/validateschema/) | Validates data against a JSON Schema, returning errors | No, returns new array | Yes |

## String functions

| Function | Description | Mutates | Chainable |
|----------|-------------|---------|-----------|
| [Contains](/jyro/stdlib/string/contains/) | Tests whether a string contains a substring (or array contains element) | No | No |
| [EndsWith](/jyro/stdlib/string/endswith/) | Tests whether a string ends with a suffix | No | No |
| [Join](/jyro/stdlib/string/join/) | Joins array elements into a string with a separator | No, returns new string | Yes (string) |
| [ToLower](/jyro/stdlib/string/tolower/) | Converts a string to lowercase | No, returns new string | Yes (string) |
| [PadLeft](/jyro/stdlib/string/padleft/) | Pads a string on the left to a specified width | No, returns new string | Yes (string) |
| [PadRight](/jyro/stdlib/string/padright/) | Pads a string on the right to a specified width | No, returns new string | Yes (string) |
| [RandomString](/jyro/stdlib/string/randomstring/) | Generates a cryptographically secure random string | No, returns new string | Yes (string) |
| [RegexMatch](/jyro/stdlib/string/regexmatch/) | Returns the first regex match | No | No |
| [RegexMatchAll](/jyro/stdlib/string/regexmatchall/) | Returns all regex matches as an array | No, returns new array | Yes |
| [RegexMatchDetail](/jyro/stdlib/string/regexmatchdetail/) | Returns a detailed match object with capture groups | No, returns new object | Yes (object) |
| [RegexTest](/jyro/stdlib/string/regextest/) | Tests whether a string matches a regex pattern | No | No |
| [Replace](/jyro/stdlib/string/replace/) | Replaces all occurrences of a substring | No, returns new string | Yes (string) |
| [Split](/jyro/stdlib/string/split/) | Splits a string into an array using a delimiter | No, returns new array | Yes (array) |
| [StartsWith](/jyro/stdlib/string/startswith/) | Tests whether a string begins with a prefix | No | No |
| [Substring](/jyro/stdlib/string/substring/) | Extracts a portion of a string by position and optional length | No, returns new string | Yes (string) |
| [ToNumber](/jyro/stdlib/string/tonumber/) | Converts a string to a number (returns null on failure) | No | No |
| [ToUpper](/jyro/stdlib/string/toupper/) | Converts a string to uppercase | No, returns new string | Yes (string) |
| [Trim](/jyro/stdlib/string/trim/) | Removes leading and trailing whitespace | No, returns new string | Yes (string) |

## Utility functions

| Function | Description | Mutates | Chainable |
|----------|-------------|---------|-----------|
| [Base64Decode](/jyro/stdlib/utility/base64decode/) | Decodes a Base64 string to UTF-8 | No, returns new string | Yes (string) |
| [Base64Encode](/jyro/stdlib/utility/base64encode/) | Encodes a string to Base64 | No, returns new string | Yes (string) |
| [Clone](/jyro/stdlib/utility/clone/) | Creates a deep copy of a value | No, returns new value | Yes |
| [Diff](/jyro/stdlib/utility/diff/) | Compares two objects and returns a structured summary of their differences | No, returns new object | Yes (object) |
| [Coalesce](/jyro/stdlib/utility/coalesce/) | Returns the first non-null value from an array | No | Depends on result type |
| [FromJson](/jyro/stdlib/utility/fromjson/) | Parses a JSON string into a Jyro value | No, returns new value | Depends on result type |
| [HasProperty](/jyro/stdlib/utility/hasproperty/) | Tests whether an object has a property (even if null) | No | No |
| [Keys](/jyro/stdlib/utility/keys/) | Returns all property names as an array | No, returns new array | Yes (array) |
| [Merge](/jyro/stdlib/utility/merge/) | Merges multiple objects into a new object | No, returns new object | Yes (object) |
| [NewGuid](/jyro/stdlib/utility/newguid/) | Generates a new UUID v4 string | No | No |
| [ToBoolean](/jyro/stdlib/utility/toboolean/) | Converts a value to boolean using Jyro truthiness rules | No | No |
| [ToJson](/jyro/stdlib/utility/tojson/) | Converts a Jyro value to a JSON string | No, returns new string | Yes (string) |
| [ToString](/jyro/stdlib/utility/tostring/) | Converts a value to its string representation | No, returns new string | Yes (string) |
| [TypeOf](/jyro/stdlib/utility/typeof/) | Returns the type name as a string | No | No |
| [Values](/jyro/stdlib/utility/values/) | Returns all property values as an array | No, returns new array | Yes (array) |
| [Sleep](/jyro/stdlib/utility/sleep/) | Pauses script execution for the specified number of milliseconds | No | No |