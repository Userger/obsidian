
## Полная выборка
`SELECT *`
`FROM products;`

## Частичная выборка
`SELECT product_id, product_name, unit_price`
`FROM products;`

*также можно использовать мат. операторы (+ - * / )*
`SELECT product_id, product_name, unit_price` $*$ `units_in_stock`
`FROM products;`


## DISTINCT - только уникальные значения
`SELECT DISTINCT title`
`FROM employees;`

*уникальные сочетания*
`SELECT DISTINCT country, title`
`FROM employees;`