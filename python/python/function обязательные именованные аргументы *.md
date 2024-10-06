```python
def some_funct(*, named_argument)
def some_funct(*args, named_argument)
```
- [i] все аргументы после оператора `*` обязательно именованные



```python
def some_funct(*, named_argument, **kwargs)
def some_funct(*args, named_argument, second_named=0, **kwargs)
```
- [i] после обязательно именованных можно использовать `**kwargs`