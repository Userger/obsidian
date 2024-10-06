### `replace(o, n)`
метод заменяет все символы `o` на `n`
### example:
```python
string = 'Penny'
new_string = string.replace('n', 'g')
print(new_string) # 'Peggy'
```

- [i] также заменить символы в строке можно, используя срезы
### example:
```python
string = "hello world!"
new_string = string[0:5]
print(new_string) # 'hello'
```

## Про третий аргумент:
- [I] в качестве третьего аргумента метод принимает кол-во заменяемых подстрок
```python
>>> 'aaaa'.replace('a', 'b', 2)
'bbaa'
```