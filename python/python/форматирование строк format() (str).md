```python
>>> '{}'.format('hello')
'hello'
>>> '{}, {}'.format('hello', 'world')
'hello, world'
>>> '{}, {}'.format(4, 1998)
'4, 1998'
```
*всё просто*


- [i] также можно указывать индексы:
```python
>>> '{1}, {0}'.format('world', 'hello')
'hello, world'
```

- [i] аргументы могут быть именованными:
```python
>>> '{h}, {w}'.format(w='world!', h='hello')
'hello, world!'
```