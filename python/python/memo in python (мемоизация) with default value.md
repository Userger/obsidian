```python
  1 def func_with_memo(arg, memorized={}):
  2     if arg in memorized:
  3         print('has taken from memory')
  4         return memorized[arg]
  5     res = arg ** 10
  6     memorized[arg] = res
  7     return res
  8
  9 print(func_with_memo(12))
 10 print(func_with_memo(22))
 11 print(func_with_memo(12))
 12 print(func_with_memo(22))
```
output:
```
61917364224
26559922791424
has taken from memory
61917364224
has taken from memory
26559922791424
```