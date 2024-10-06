[[collections]]
deque - двусторонняя очередь (double end queue)
- можно забирать/добавлять элементы с конца и с начала очереди:
```python
from collections import deque
queue = deque()`
queue.popleft() ## удаляет элемент с начала очереди и возвращает его
queue.pop() ## удаляет элемент с конца и возвращает его

queue.appendleft() ## добавить элемент в начало очереди 
queue.append() ## добавить элемент в конец очереди 

```
