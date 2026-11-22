# Enterprise Data Architecture & Reconciliation

Python pipeline that reconciles an HR database, 100,000 raw server logs and daily EUR/USD rates into a CFO revenue dashboard.

## Files
| File | Purpose |
|---|---|
| `Enterprise_Data_Reconciliation_Solution.ipynb` | Full solution (SQL, regex, merge, dashboard, audit, CFO summary) |
| `enterprise_database.db` | SQLite user history (`users` table) |
| `server_logs.txt` | Raw server logs |
| `daily_exchange_rates.csv` | Daily EUR to USD rates |
| `cfo_dashboard.png` | Dashboard output |

Place the three data files next to the notebook with exactly these names (remove any `(1)` suffix).

## Run
```
pip install pandas matplotlib seaborn jupyter
jupyter notebook Enterprise_Data_Reconciliation_Solution.ipynb
```
Runtime on 100,000 rows: about 1-2 seconds.

## Approach
1. **SQL:** `ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY updated_at DESC)`, then keep rank 1 with status Active.
2. **Regex:** one named-group `str.extract`, ERROR rows dropped; cross-checked with JSON parsing.
3. **Temporal join:** `ffill` on rates + left join (a naive inner join loses 28.4% of rows).
4. **Dashboard:** USD revenue by month and Top 5 customers.
