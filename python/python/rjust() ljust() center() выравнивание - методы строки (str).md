```python
>>> 'hello world!'.rjust(30)
'                  hello world!'


>>> 'hello world!'.ljust(30)
'hello world!                  '


>>> 'hello world!'.center(30)
'         hello world!         '
```

# Выравнивание с помощью старого типа форматирования

```python
>>> thing = 'woodchuck'
>>> '%12s' % thing
'   woodchuck'
>>> '%+12s' % thing
'   woodchuck'
>>> '%-12s' % thing
'woodchuck   '
>>> '%.3s' % thing
'woo'
>>> '%12.3s' % thing
'         woo'
```

# Выравнивание с функцией форматирования (format())

```python
thing = 'wraith'
place = 'window'
print('The {} is at the {}'.format(thing, place))
print('The {:^12s} is at the {}'.format(thing, place))
print('The {:>12s} is at the {}'.format(thing, place))
print('The {:<12s} is at the {}'.format(thing, place))
print('The {:!<12s} is at the {}'.format(thing, place))


output:
The wraith is at the window
The    wraith    is at the window
The       wraith is at the window
The wraith       is at the window
The wraith!!!!!! is at the window
```

# Выравнивание самый новый стиль (f-строки)
```python
  1 thing = 'wereduck'
  2 place = 'werepond'
  3
  4 print(f'The {thing:>20} is in the {place:.^20}')

output:
The             wereduck is in the ......werepond......
```