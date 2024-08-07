```python
some_dict = {
    1: "hello",
    "world": 2,
}
del some_dict[1]
print(some_dict)
```
**output:**
```
{'world': 2}
```

- [!] при попытке удаления несуществующего значения упадет ошибка KeyError

[[методы словаря (dict) (methods)]]