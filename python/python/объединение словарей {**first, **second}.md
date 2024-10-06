[[dict (словарь)]]
```python
>>> first = {'a': 'agony', 'b': 'bliss'}
>>> second = {'b': 'bagels', 'c': 'candy'}
>>> {**first, **second}
{'a': 'agony', 'b': 'bagels', 'c': 'candy'} # b взято из 2-го словаря
```
- [i] это поверхностная копия