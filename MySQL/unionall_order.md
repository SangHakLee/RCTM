# How to order by in union all query.

## Simple union all
```sql
SELECT id, name FROM table_a
UNION ALL
SELECT id, name FROM table_b
```

## Wrong Case
```sql
SELECT id, name FROM table_a ORDER BY name DESC
UNION ALL
SELECT id, name FROM table_b ORDER BY name DESC
```

## Correct Case
```sql
SELECT id, name FROM table_a
UNION ALL
SELECT id, name FROM table_b
ORDER BY name DESC
```
