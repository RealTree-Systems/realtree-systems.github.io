---
layout: default
title: Variables
parent: Jyro Language Reference
has_children: true
has_toc: true
permalink: /jyro/variables/
nav_order: 70
---

# Variables

Variables in Jyro are declared with the `var` keyword, support optional type hints with automatic coercion, and follow block-level scoping rules with shadowing. Local variables exist only during script execution - only modifications to the `Data` context persist after the script completes.
