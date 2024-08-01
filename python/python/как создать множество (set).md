```python
a = {1,2,3,4}
b = set("hello")
c = set([1,2,3,4,5,4,3,2,1])
d = set((1,2,3,4,"lol","lol","hello","hello"))
e = set(range(5))
f = set()
g = {e for e in range(10)}

```



- [!] такое приведет к ошибке "unhashable type"
```python
 h = set((["hello"],[1,2,3]))
 i = set([["hello"],[1,2,3]])
```


[[set (множество)]]