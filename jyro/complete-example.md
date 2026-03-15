---
layout: default
title: Complete Example
parent: Jyro Language Reference
has_children: false
has_toc: false
permalink: /jyro/complete-example/
nav_order: 9999
---

# Complete Example

An order processing script demonstrating input validation, iteration, aggregation, and output shaping.

**Input**: `Data.orders` - an array of order objects.

**Output**: `Data.summary`, `Data.processed`, `Data.errors`, `Data.export`.

```jyro
# Order processing script
# Input: Data.orders (array of order objects)
# Output: Data.summary, Data.processed, Data.errors

# 1. Validate input
if Data.orders is null or Data.orders is not array then
    fail "Orders must be an array"
end

if Length(Data.orders) == 0 then
    Data.summary = {count: 0, total: 0}
    Data.processed = []
    exit "No orders to process"
end

# 2. Quick validation
if AnyByField(Data.orders, "amount", "<", 0) then
    fail "Negative order amounts are not allowed"
end

# 3. Process orders
Data.errors = []
var processed = []
var total = 0

foreach order in Data.orders do
    if order is not object then
        Data.errors = Append(Data.errors, "Skipped non-object item")
        continue
    end

    if order.amount is not number then
        Data.errors = Append(Data.errors, "Missing amount for order: " & order.id)
        continue
    end

    total += order.amount
    order.processedAt = Now()
    order.tax = order.amount * 0.1
    order.total = order.amount + order.tax
    processed = Append(processed, order)
end

# 4. Generate summary
var count = Length(processed)
Data.summary = {
    "count": count,
    "total": total,
    "average": count > 0 ? total / count : 0,
    "errorCount": Length(Data.errors)
}

# 5. Sort and store results
Data.processed = SortByField(processed, "total", "desc")

# 6. Create export view
Data.export = Project(Data.processed, ["id", "amount", "total", "processedAt"])

exit "Processed " & count & " orders"
```
