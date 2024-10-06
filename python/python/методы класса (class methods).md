методы класса должны на первом месте в аргументах иметь **self**
```python
class Solution:
	def __init__(self):
		self.delimiter = '|'
	def encode(self, strings):
		string = ""
		for word in strings:
			string += f'{len(word)}{self.delimiter}{word}'
```