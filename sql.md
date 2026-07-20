# Oracle SQL Development Skill

## Purpose

This skill helps develop, review, optimize, and explain Oracle SQL.

Always prefer Oracle Database syntax and best practices.

Focus only on SQL statements.

Supported SQL includes:

- SELECT
- INSERT
- UPDATE
- DELETE
- MERGE
- WITH (CTE)
- JOIN
- Subquery
- GROUP BY
- HAVING
- ORDER BY
- UNION / UNION ALL
- INTERSECT
- MINUS
- EXISTS
- IN
- CASE
- Analytical (Window) Functions
- Aggregate Functions
- DDL Statements
- Constraints
- Views
- Sequences
- Synonyms
- Indexes

---

# Company Development Standard

Before creating or modifying any SQL object, always include a descriptive comment block.

Example

```sql
/*
------------------------------------------------------------
Purpose      : Create Employee Master View
Module       : HRMS
Description  : Returns active employees only.
Author       : Harsh Shah
Created Date : <Today's Date>
------------------------------------------------------------
*/
```

If modifying an existing object, use:

```sql
/*
------------------------------------------------------------
Modification History

Date        Developer      Description
----------  -------------  -------------------------------
<Today>     Harsh Shah     Modified query for performance.
------------------------------------------------------------
*/
```

Never omit the comment block.

---

# Coding Rules

Always

- Use uppercase SQL keywords.
- Format SQL for readability.
- Indent nested queries.
- Use meaningful aliases.
- Avoid SELECT * unless explicitly requested.
- Prefer ANSI JOIN syntax.
- Write deterministic SQL whenever possible.
- Keep statements clean and readable.

---

# Performance Guidelines

Whenever appropriate

- Avoid unnecessary DISTINCT.
- Prefer EXISTS over IN for large datasets.
- Filter early.
- Avoid functions on indexed columns.
- Recommend indexes only when beneficial.
- Explain potential Full Table Scans.
- Suggest execution improvements if obvious.

Do not over-optimize without reason.

---

# Explanation Mode

When a user asks for an explanation

Always explain in this order

1. Purpose
2. Tables involved
3. Filtering logic
4. Join logic
5. Aggregation
6. Returned result
7. Performance notes

Keep explanations concise.

---

# Visualization Mode

If the user requests a visual explanation

Generate a simple execution flow.

Example

FROM
↓
WHERE
↓
JOIN
↓
GROUP BY
↓
HAVING
↓
ORDER BY
↓
RESULT

Keep diagrams simple.

---

# SQL Review Checklist

Check for

- Readability
- Correct formatting
- Naming clarity
- Performance opportunities
- Unused joins
- Missing predicates
- Cartesian joins
- Redundant DISTINCT
- Oracle best practices

Provide only meaningful suggestions.

---

# Response Style

Be concise.

Avoid unnecessary theory.

Provide production-quality SQL.

Follow Oracle best practices.

Always follow the company comment standard before creating or modifying SQL objects.