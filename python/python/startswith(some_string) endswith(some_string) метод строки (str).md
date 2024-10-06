### метод **startswith** возвращает True или False в зависимости от того, начинается ли строка с определенной подстроки
#### метод **endswith** - тот же метод, только для конца строки

```python
>>> 'Hello, world!'.startswith('one')
False
>>> 'Hello, world!'.startswith('Hello')
True
```

- [i] также аргументом метод принимает кортеж строк
и возвращает True, если строка начинается с одной из подстрок кортежа.
```python
>>> 'Hello, world!'.startswith(('one', 'Hello'))
True
>>> 'Hello, world!'.startswith(('one', 'two', 'lol'))
False
```
