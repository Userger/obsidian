`strip()` - метод обрезающий символы (строку), переданные в аргумент, в начале и конце строки
`rstrip()` - обрезает в конце (right strip)
`lstrip()` - обрезает вначале (left strip)

- [!] в аргумент передается строка, содержащая символы которые нужно удалить в конце/начале строки.

- [i] метод, вызванный без аргумента, уберет пробелы, символы табуляции (`\t`) и переноса строки (`\n`).
```python
' word '.strip() # 'word'
```

```python
>>> a = ' +799999999999 '
>>> a.strip(' +')
'799999999999 '
>>> a.rstrip('9 ')
' +7'
>>> a.strip('79 ')
'+'
```