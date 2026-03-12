---
layout: default
title: Introduction
parent: Jyro Language Reference
has_children: false
has_toc: true
permalink: /jyro/introduction/
nav_order: 20
---

# Introduction

A Jyro script is a sequence of statements executed top-to-bottom. There are no modules or imports. Scripts may define their own [user-defined functions](/jyro/user-functions/) and [discriminated unions](/jyro/discriminated-unions/), and call host-provided library functions (including the standard library). Function and union declarations are hoisted - they can be called before they appear in the source. Jyro's security model is set up so that the host tightly controls what is available to scripts.

## The `Data` context

The `Data` object is the sole input/output mechanism for every Jyro script. The host environment populates `Data` before execution begins, and all modifications made to `Data` during execution are returned to the host when the script completes.

```jyro
# Data is always available - never declare it
var input = Data.userInput       # Read from Data (provided by host)
Data.processed = true            # Write to Data (returned to host)
Data.timestamp = Now()

# Local variables are discarded after execution
var temp = 100                   # NOT returned to host
Data.output = temp               # This IS returned
```

### Some notes about `Data`

- `Data` is always an object - never null, never reassignable; freely modifiable by adding and removing properties
- Only modifications to `Data` persist after execution
- Local variables (`var`) exist only during script execution
- `Data` is always returned to the host in whatever state the script left it in, even on failure
- Jyro keywords are valid as member names on `Data` - `Data.for`, `Data.number`, `Data.string` all work. This only applies to property access on objects; `var for = "x"` is still a syntax error.

### Creating nested structures

Intermediate objects must be created explicitly. Assigning to a nested path where intermediate objects do not exist causes a runtime error.

```jyro
# INCORRECT - error if Data.level1 doesn't exist
Data.level1.level2.value = "x"

# CORRECT - create each level explicitly
Data.level1 = {}
Data.level1.level2 = {}
Data.level1.level2.value = "x"

# Or use a literal
Data.level1 = {"level2": {"value": "x"}}

# Or build the structure in a variable first
var result = {
    "level2": {
        "value": "x"
    }
}
Data.level1 = result
```

## Execution model

Scripts execute statements sequentially from top to bottom. Execution ends when the last statement completes, or when an `exit` or `fail` statement is encountered. Inside [user-defined functions](/jyro/user-functions/), `return` exits the function and resumes at the call site. The host environment enforces resource limits on execution time, statement count, loop iterations, and stack depth.

The language is fully sandboxed - there is no file I/O, network access, or system calls. All external interaction happens through the `Data` context and the host-provided standard library.

Hosts can provide their own functions to Jyro scripts, which operate in exactly the same way as the standard library.

## Case conventions

Jyro is case-sensitive. The conventions are:

| Element | Convention | Example |
|---------|-----------|---------|
| Keywords | lowercase | `foreach`, `while`, `var` |
| Local variables | lowerCamelCase | `var myFoo = 42` |
| Private/internal variables | underscore + lowerCamelCase | `var _payloadData = Data.formattedPayload` |
| Iterator variables | follow the array name | `foreach item in Data.items` |
| Host function calls | PascalCase | `Today()`, `WhereByField(...)` |
| The `Data` object | PascalCase | `Data` (upper-case D) |

## File formats

Jyro scripts come in two file formats.

### Plain text

Plain text source code scripts have the extension `.jyro` and are parsed and validated each time they are run.

### Binaries

Jyro also has a binary format with the extension `.jyrx`. These files contain a pre-parsed format and are not validated before execution, which improves execution time.

Because .jyrx files are not validated at runtime, never execute an untrusted jyrx script.