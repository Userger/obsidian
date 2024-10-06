
## Полная выборка
```sql
SELECT *
FROM products;
```

## Частичная выборка
```sql
SELECT product_id, product_name, unit_price
FROM products;
```

*также можно использовать мат. операторы (+ - * / )*
```sql
SELECT product_id, product_name, unit_price * units_in_stock
FROM products;
```

## DISTINCT - только уникальные значения

```sql
SELECT DISTINCT title
FROM employees;
```
#### *уникальные сочетания*

```sql
SELECT DISTINCT country, title
FROM employees;
```