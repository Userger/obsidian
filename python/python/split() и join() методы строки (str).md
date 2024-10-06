# split()
```python
>>> string = "Hello world! Split this message."
>>> string.split(" ")
['Hello', 'world!', 'Split', 'this', 'message.']
```
метод строки, который возвращает список подстрок, разделенных символом, который передается в аргумент метода


- [i] можно передать любую строку в качестве аргумента
- [!] кроме пустой строки (если хочешь получить список символов строки, используй функцию `list()`)
```python
>>> string.split("world")
['Hello ', '! Split this message.']
```
# join()
```python
>>> split_string = "hello world!".split(" ")
>>> split_string
['hello', 'world!']
>>> " ".join(split_string)
'hello world!'
>>> join_symbol = ", "
>>> join_symbol.join(split_string)
'hello, world!'
```
метод строки, который соединяет строки списка, переданного как аргумент метода
строкой, от которой вызвали метод