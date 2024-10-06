функция поиска наиболее часто встречающихся значений в списке

```python
def topKFrequent(nums, c):
    counts = {}
    freq = [[] for _ in range(len(nums)+1)]

    for num in nums:
        counts[num] = counts.get(num, 0) + 1

    for key, item in counts.items():
        freq[item].append(key)

    res = []
    for i in range(len(nums), 0, -1):
        for e in freq[i]:
            res.append(e)
            if len(res) == c:
                return res
    return res

```