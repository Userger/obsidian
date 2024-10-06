# :=
оператор используется в **if, while, for, в аргументе функции**
для того чтобы сразу присвоить значение переменной

### example:
```python
  1 tweet_limit = 100
  2 tweet_string = 'Blah' * 50
  3 if (diff := tweet_limit - len(tweet_string)) >= 0:
  4     print('a fitting tweet')
  5 else:
  6     print('went over by', abs(diff))
```
#### output:
```
went over by 100
```