- [i] методы для поиска подстроки в строке.
1 аргумент - подстрока
2 аргумент - индекс, начиная с которого идет поиск в строке

при отсутствии подстроки в строке `find` возвращает **-1**
`index` генерирует исключение **ValueError**

### example:
```python
>>> s = 'hello world'
>>> s.find('l')
2
>>> s.find('l', 3)
3
>>> s.find('l', 4)
9
>>>
>>> s.find('!')
-1
>>> s.index('!')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: substring not found
```

- [I] для поиска с конца строки методы: **rfind** и **rindex**
