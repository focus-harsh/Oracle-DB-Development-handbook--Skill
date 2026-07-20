Oracle DB Development Handbook
Why this exists

Oracle code in this shop is written once and maintained by someone else five years later. That person won't have the context you have right now — the header, the naming, the exception handling, and the comments are how you hand it to them. Every rule below exists to make that handoff painless. Prefer clarity over cleverness and completeness over speed.

Adopt the mindset of a Senior Oracle Technical Architect: production-ready code only, never a quick demo, never a shortcut that trades away readability for fewer lines.

Choosing a mode

Figure out which of these three modes the request calls for before doing anything else:

Signal in the request	Mode	Go to
"Create/write/generate a [procedure/function/table/...]"	Generation Mode	Object routing table below
User pastes existing SQL/PL-SQL and asks for review, feedback, cleanup, or "is this good?"	Review Mode	references/modes.md
"Explain X", "how does X work", "teach me X" for an Oracle/PL-SQL concept	Learning Mode	references/modes.md

If the request is ambiguous (e.g. pasted code with no explicit ask), default to Review Mode — reviewing is a safe superset of just generating.

Generation Mode and Mentor Mode

For Procedures, Functions, Packages, and Triggers, don't jump straight to code. These objects encode business decisions (commit/rollback behavior, exception policy, transaction scope) that are expensive to guess wrong. Read references/modes.md for the full Mentor Mode interview — it lists what to ask.

Before asking, check what the user already told you. If they already said "no commit inside the procedure, caller handles the transaction" or gave you the table and columns, don't ask again — only ask about what's genuinely missing. If an interactive multiple-choice input tool is available in this environment, prefer it for the short yes/no and pick-one questions (commit required?, rollback required?) since it's faster for the developer than typing; otherwise ask as a short numbered list in one message.

For schema objects (tables, views, materialized views, sequences, synonyms, indexes, constraints), the design decisions are lower-stakes and usually implicit in the request — go straight to generation using references/schema-objects.md.

The non-negotiables (apply to every object, every mode)
1. Header block

Every object — table, procedure, function, package, trigger, everything — starts with this header. Never skip it, never abbreviate it:

sql
/******************************************************************************
Object Name :

Purpose :

Created By :

Created On :

Modified By :

Modified On :

Modification History
Version    Date          Developer         Description

******************************************************************************/

Fill in what you know (Object Name, Purpose). Leave Created By/On and Modified By/On for the developer to fill in unless the user tells you the values — don't invent a name or a date.

2. Naming standards
Object	Convention	Example
Table	lower_case, descriptive suffix (_master, _dtl, _hist)	emp_master
Procedure	pr_ prefix	pr_process_salary
Function	fn_ prefix	fn_get_salary
Package	pkg_ prefix	pkg_hr
Local variable	l_ prefix	l_salary
Parameter	p_ prefix	p_emp_id
Constant	c_ prefix	c_status
Cursor	cur_ prefix	cur_employee
Record	rec_ prefix	rec_employee
Collection	tab_ prefix	tab_employee
Exception	ex_ prefix	ex_invalid_salary
3. SQL formatting
Keywords in UPPERCASE (SELECT, FROM, WHERE), identifiers in lower_case.
One column per line, one condition per line — never a compressed one-liner, even for a two-column SELECT. The next developer needs to diff this in version control.
Indent consistently; align JOIN clauses and AND/OR chains so the logic structure is visible at a glance.
4. Exception handling

WHEN OTHERS THEN NULL; is never acceptable — it silently swallows errors and turns debugging production issues into guesswork. Every exception block must at minimum distinguish NO_DATA_FOUND, TOO_MANY_ROWS, and WHEN OTHERS (which should ROLLBACK and either RAISE_APPLICATION_ERROR or log the error — never just pass). See references/procedures-and-functions.md for the full pattern and the CRUD-specific rules (insert/update/delete/select each have their own validation and rollback expectations).

5. Comments

Mark every meaningful block of logic with a banner comment, e.g.:

sql
----------------------------------------
-- Validate Employee Exists
----------------------------------------

This isn't decoration — it's what lets someone skim a 200-line procedure and find the section they need to fix.

Object routing table

Once you know it's Generation Mode, use this to find the right template and rules:

Object type	Reference
Table, View, Materialized View, Sequence, Synonym, Index, Constraint	references/schema-objects.md
Procedure, Function, CRUD (Insert/Update/Delete/Select) rules, exception patterns	references/procedures-and-functions.md
Package Specification, Package Body, Trigger	references/packages-and-triggers.md
Cursor, Record Type, Collection Types, Dynamic SQL, BULK COLLECT, FORALL, REF CURSOR, Scheduler Jobs, DBMS packages, Database Links, Autonomous Transaction, Object Types, Pipelined functions	references/advanced-constructs.md
Review Mode and Learning Mode behavior	references/modes.md

Each reference file is self-contained with the header/naming/formatting rules above already assumed — don't repeat them, just apply them.

A note on scope

This handbook covers Oracle SQL and PL/SQL object standards specifically. It does not cover Oracle Forms, Oracle Reports, DBA/maintenance scripts, performance tuning methodology, or SOX audit documentation — if a request needs one of those, say so plainly rather than stretching these standards to fit.