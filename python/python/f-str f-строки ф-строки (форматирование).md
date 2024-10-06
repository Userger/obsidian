- [i] самый новый стиль форматирования строк 
syntax:

```python
>>> hello, world = 'hi', 'man!'
>>> f"{hello}, {world}"
'hi, man!'
```

## фишки с f-строками

```python
  1 thing = 'wereduck'
  2 place = 'werepond'
  3
  4 print(f'The {thing:>20} is in the {place:.^20}')

output:
The             wereduck is in the ......werepond......
```