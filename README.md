<div align="center">
  <table border="0" cellpadding="20" cellspacing="0">
    <tr>
      <td width="30%" align="center" valign="middle">
        <img src="SQL.png" alt="SQL Query Explainer Logo" width="160">
      </td>
      <td width="70%" valign="middle" align="left">
        <h1>🔮 SQL Query Explainer</h1>
        <h3><i>Translate complex SQL queries into plain-English summaries with step-by-step logic and actionable performance insights.</i></h3>
      </td>
    </tr>
  </table>
</div>

---


![Skill Format](https://img.shields.io/badge/format-SKILL.md-blue)
![Role](https://img.shields.io/badge/role-DBA%20%26%20SQL%20Architect-green)
![Compatibility](https://img.shields.io/badge/agents-Claude%20%7C%20Cursor%20%7C%20Codex-orange)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

**Tags:** `database` `sql` `performance-tuning` `query-optimization` `code-explanation` `data-engineering` `skill-md` `agent-skill` `claude-code` `cursor` `aider` `codex` `gemini-gems` `claude-projects` `prompt-engineering`

---

## Overview

Complex database queries are often difficult for cross-functional teams to parse and can hide costly performance inefficiencies like unindexed scans or suboptimal subqueries. **SQL Query Explainer** acts as a senior database administrator that translates raw SQL into plain English for non-technical stakeholders while identifying execution bottlenecks and suggesting concrete architectural optimizations.

| Output Section | Rules Enforced |
| :--- | :--- |
| **📝 Query Summary** | Exactly 1–2 sentences summarizing the query's goal in plain English, avoiding dense database jargon. |
| **🔍 Step-by-Step Breakdown** | Four structured bullet points: Data Retrieval, Relationships, Filtering & Grouping, and Sorting & Limits. |
| **⚡ Performance Notes & Bottlenecks** | Itemized bullet points highlighting risks (e.g., full table scans, implicit casts, cross joins). |
| **🛠️ Suggested Optimizations** | Actionable recommendations such as index proposals and query refactoring. |

Built-in guardrails enforce clean markdown formatting, bolded table and column references, and jargon-free high-level summaries suitable for non-technical stakeholders.

---

## Features

- ✅ **Plain-English Translation:** Distills complex multitable joins and subqueries into accessible language for business partners.
- ✅ **Chronological Deconstruction:** Breaks down queries by clause sequence (SELECT, JOIN, WHERE, GROUP BY, ORDER BY, LIMIT).
- ✅ **Proactive Bottleneck Detection:** Flags performance red flags such as missing indexes, full table scans, and cross joins.
- ✅ **Targeted Optimization Advice:** Delivers practical refactoring steps and indexing recommendations to cut query runtimes.
- ✅ **Consistent Formatting:** Enforces bolding for table and column names and structured lists for rapid scanning.

---

## Installation

### Claude Code

Install the skill at either the project level or globally across your environment:

```bash
# Project-level
mkdir -p .claude/skills/sql-query-explainer
cp SKILL.md .claude/skills/sql-query-explainer/

# User-level (available across all projects)
mkdir -p ~/.claude/skills/sql-query-explainer
cp SKILL.md ~/.claude/skills/sql-query-explainer/
```

Claude Code discovers skills via the `description` metadata in the YAML front matter and loads them automatically when query explanation requests are made.

### Other Agentic Tools

| Tool | Suggested Setup |
| :--- | :--- |
| **Cursor** | Copy `SKILL.md` into `.cursor/rules/sql-query-explainer.mdc`, or reference it directly in chat using `@SKILL.md`. |
| **Aider** | Pass the skill as read-only context: `aider --read SKILL.md`. |
| **Codex / AGENTS.md** | Paste the skill contents directly into `AGENTS.md`, or reference the file path in your prompt. |
| **Other Assistants** | Paste the full text of `SKILL.md` at the start of your session, then supply your query. |

### Chat-Based Setups (No File System)

| Interface | Suggested Setup |
| :--- | :--- |
| **Claude Projects** | Create a Project, add `SKILL.md` to Project Knowledge (or Project Instructions), and start a chat. |
| **Gemini Gems** | Create a Gem, paste the contents of `SKILL.md` into Instructions, or upload it as knowledge. |
| **Custom GPTs** | Paste the contents of `SKILL.md` into the custom instructions configuration box. |

*Exact file locations and menu labels vary by tool and version, so check each tool's documentation if something doesn't load.*

---

## Usage

| Variable | Description |
| :--- | :--- |
| `{{sql_query}}` | The raw SQL query string to be explained and analyzed. |

```text
Use the sql-query-explainer skill:

SQL Query:
SELECT c.customer_id, c.customer_name, COUNT(o.order_id) AS total_orders, SUM(o.order_amount) AS total_spend
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
WHERE o.order_date >= '2025-01-01'
GROUP BY c.customer_id, c.customer_name
ORDER BY total_spend DESC
LIMIT 10;
```

In tools without native skill support, replace the first line with: `Follow the instructions in SKILL.md.`

---

## Sample Input

```text
SQL Query:
SELECT u.account_id, u.full_name, AVG(t.transaction_amount) AS avg_ticket, COUNT(t.transaction_id) AS tx_count
FROM user_accounts u
JOIN transactions t ON u.account_id = t.account_id
WHERE YEAR(t.transaction_timestamp) = 2025
  AND t.status = 'COMPLETED'
GROUP BY u.account_id, u.full_name
HAVING COUNT(t.transaction_id) > 15
ORDER BY avg_ticket DESC
LIMIT 25;
```

---

## Sample Output

## 📝 Query Summary
This query finds the top 25 active customer accounts that completed more than 15 transactions in 2025, showing their total transaction count and average spend per transaction. It ranks these accounts starting with the highest average transaction amount.

## 🔍 Step-by-Step Breakdown
*   **Data Retrieval:** Retrieves **u.account_id** and **u.full_name** from **user_accounts**, while calculating the mean value (**AVG(t.transaction_amount)**) and volume (**COUNT(t.transaction_id)**) from **transactions**.
*   **Relationships:** Connects **user_accounts** and **transactions** using an inner join on the matching **account_id** key.
*   **Filtering & Grouping:** Restricts rows to completed transactions dated within the calendar year 2025 (**WHERE**), aggregates results per user account (**GROUP BY**), and retains only accounts with more than 15 transactions (**HAVING**).
*   **Sorting & Limits:** Orders qualifying accounts from highest to lowest average ticket size (**ORDER BY avg_ticket DESC**) and restricts the final report to the top 25 records (**LIMIT 25**).

## ⚡ Performance Notes & Bottlenecks
*   Applying the function `YEAR(t.transaction_timestamp)` prevents the database from using standard range indexes on **transaction_timestamp**, forcing a full scan of candidate rows.
*   The inner join will discard user accounts that have zero transactions, which may exclude newly registered users if complete account coverage is intended.
*   Aggregating high-volume transaction history without covering composite indexes on **(status, transaction_timestamp, account_id)** will require significant disk and memory overhead during sorting and grouping.

## 🛠️ Suggested Optimizations
*   Replace `YEAR(t.transaction_timestamp) = 2025` with an explicit sargable date range: `t.transaction_timestamp >= '2025-01-01' AND t.transaction_timestamp < '2026-01-01'`.
*   Create a composite index on `transactions(status, transaction_timestamp, account_id, transaction_amount)` to allow an index-only scan for filtering and aggregation.
*   If users with zero transactions need to be shown, convert the `JOIN` to a `LEFT JOIN` and move date criteria into the join condition.

> ℹ️ The sample above is illustrative. Always review generated content for accuracy and compliance before using it.

---

## Customization

- **Target Database Dialect:** Add an instruction under `# Core Directives` specifying engine-specific quirks (e.g., PostgreSQL, Snowflake, MySQL, BigQuery).
- **Stakeholder Tone:** Adjust Directive 1 to enforce either executive summary style or technical DBA-to-DBA documentation.
- **Output Headings:** Modify the markdown headers under `# Output Structure` if your workflow requires dedicated execution plan (`EXPLAIN`) sections.
- **Optimization Scope:** Add explicit directives to mandate index syntax (e.g., `CREATE INDEX ...`) within the Suggested Optimizations section.

---

## Tips for Best Results

1. **Provide Schema Context:** Mention available indexes, primary keys, or approximate table row counts alongside `{{sql_query}}` for more accurate bottleneck detection.
2. **Specify SQL Dialect:** State your database engine (PostgreSQL, MySQL, SQL Server, Snowflake) to ensure optimization advice uses compatible functions and index types.
3. **Include EXPLAIN Plans:** When diagnosing stubborn production queries, include the query execution plan text in your prompt for precise row-scan analysis.
4. **Iterate on Refactoring:** Ask follow-up questions like "Rewrite this query using a CTE" or "Provide the exact DDL to add the suggested indexes."

---

## Disclaimer

This skill produces automated drafts and performance suggestions that should be tested in staging environments before applying to production systems. Query optimization dynamics depend heavily on database engine versions, table statistics, storage configuration, and indexing topology. This project is not affiliated with or endorsed by any database engine vendor or AI tooling provider.

---

## Contributing and License

Contributions and improvements are welcome! Consider submitting PRs for:
- Adding support for database-specific execution plan (`EXPLAIN ANALYZE`) parsers.
- Expanding directives to cover window functions and recursive CTE optimizations.
- Tailoring optimization templates for analytical data warehouses (Snowflake, BigQuery, ClickHouse).

Add your preferred license here (e.g., MIT).
