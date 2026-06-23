# SQL Practice - E-commerce Store

A hands-on SQL assignment built on a small **e-commerce** SQLite database
(~120,000 rows). You will write `SELECT` queries and data-modifying statements
to solve tasks.

## Getting started

1. **Fork** this repository on GitHub, then clone your fork:
   ```bash
   git clone https://github.com/<your-username>/sql-tasks.git
   cd sql-tasks
   ```
2. Create and activate a virtual environment:

   **Windows (PowerShell):**
   ```powershell
   python -m venv .venv
   .venv\Scripts\Activate
   ```

   **macOS / Linux (bash):**
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   ```

3. With the virtual environment active, install the requirements:
   ```bash
   pip install -r requirements.txt
   ```
4. Open the notebook:
   ```bash
   jupyter notebook tasks.ipynb
   ```
   Or, if you use **PyCharm** (Professional), just open the project folder and
   double-click `tasks.ipynb` - PyCharm runs notebooks directly, so you can skip
   the `jupyter` command. Point its Python interpreter at the `.venv` you created
   in step 2.

   The database `sqlite.db` is already included - you do **not** need to build it.

## How to complete the assignment

- Work through `tasks.ipynb` from top to bottom.
- Run the **setup cell** once. It defines two helpers:
  - `run_query(sql)` - runs a read-only `SELECT` and shows the result as a table.
  - `run_dml(sql, verify=...)` - runs `INSERT`/`UPDATE`/`DELETE` against a
    **throwaway copy** of the database, so `sqlite.db` is never modified and
    every cell is safe to re-run.
- For each task, write your SQL inside the `task_NN = """ ... """` string and
  run the cell.
- Only edit the `task_NN` query strings. Do **not** change `sqlite.db`,
  `seed.py`, the helper cell, or the task descriptions.

## Submitting

Commit your filled-in `tasks.ipynb` and open a **Pull Request** against the
original repository:
```bash
git add tasks.ipynb
git commit -m "Complete SQL tasks"
git push origin main
```

## The data model

```
suppliers ──< products >── categories (self-referencing: parent_id -> category_id)
                  │
customers ──< addresses
customers ──< orders ──< order_items >── products
customers ──< reviews >── products
orders ──< payments
```

| Table | Rows | Notes |
|-------|------|-------|
| `suppliers` | 50 | product suppliers |
| `categories` | 27 | tree via `parent_id` (8 top-level, 19 subcategories) |
| `products` | 2,000 | `-> categories`, `-> suppliers` |
| `customers` | 5,000 | |
| `addresses` | 7,000 | `-> customers` (one-to-many) |
| `orders` | 14,000 | `-> customers`; `status` in pending/paid/shipped/delivered/cancelled |
| `order_items` | ~63,000 | links `orders` <-> `products` (many-to-many) |
| `reviews` | 18,000 | `-> products`, `-> customers` |
| `payments` | ~11,000 | `-> orders` (only completed orders) |

Dates (`order_date`, `created_at`, `paid_at`) are ISO text (`YYYY-MM-DD`).
