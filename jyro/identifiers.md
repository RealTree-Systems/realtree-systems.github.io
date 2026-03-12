---
layout: default
title: Identifiers
parent: Jyro Language Reference
has_children: false
has_toc: false
permalink: /jyro/identifiers/
nav_order: 50
---

# Identifiers

Identifiers name variables and are used in property access expressions. They are case-sensitive.

## Naming rules

Identifiers

- Must begin with a letter or underscore (`_`)
- May contain any Unicode letters, digits (0–9), and underscores
- Cannot be a [reserved keyword](/jyro/keywords/) (except when used as object property names)

## Naming conventions

| Usage | Convention | Example |
|-------|-----------|---------|
| Local variables | lowerCamelCase | `orderTotal`, `itemCount` |
| Private/internal variables | underscore prefix | `_payloadData`, `_metadata` |
| Iterator variables | match the collection name | `foreach item in Data.items` |

Private/internal variables are not special in any way - Jyro treats all identifiers equally. The underscore option is provided for semantic differentiation.

Function names are host-provided and always PascalCase (e.g., `ToUpper`, `WhereByField`). The `Data` identifier is reserved and always PascalCase.
