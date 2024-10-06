
```python
nums = [1,2,3,4,5,6]
value = 6


def two_sum(nums, value):
	hash_map = {}
	for index, num in enumerate(nums):
		desire = value - num
		if desire in hash_map:
			return (hash_map[desire], index)
		hash_map[num] = index

```