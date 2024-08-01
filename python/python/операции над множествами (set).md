[[set (множество)]]

- [I] проверить есть ли элемент в множестве:
	`print(7 in {1,2,3,4,5,6,7,8})`
	**output:** True
	`print(1 not in {1,2,3,4,5,6,7,8})`
	**output:** False




## a.union(b) или a | b   -   объеденение множеств
```
a = {1,2,3}
b = {3,4,5}
print(a | b)
print(a.union(b))

output:
{1,2,3,4,5}
{1,2,3,4,5}
```

## a.intersection(b) или a & b   -   пересечение множеств
```
a = {1,2,3}
b = {3,4,5}
print(a & b)
print(a.intersection(b))

output:
{3}
{3}
```


## a.difference(b) или a - b   -   разность множеств
результат - множество, включающее элементы 
```
a = {1,2,3}
b = {3,4,5}
print(a - b)
print(a.difference(b))

output:
{1,2}
{1,2}
```
## a.symmetric_difference(b) или a ^ b   -   симметричная разность множеств
```
a = {1,2,3}
b = {3,4,5}
print(a ^ b)
print(a.symmetric_difference(b))

output:
{1,2,4,5}
{1,2,4,5}
```

- [*] Изменение множеств
- a.update(b)  -  добавляем элементы множества **b** в множество **a** (a = a.union(b))
- a.intersection_update(b) - по сути то же самое, что и a&=b или a = a.intersection(b)
- a.difference_update(b)   -   a = a.difference(b)
- a.symmetric_difference_update(b)   -   a = a.symmetric_difference(b)

- [*] operators:
- a|=b  ==  a.update(b)
- a&=b  ==  a.intersection_update(b)
- a-=b  ==  a.difference_update(b)
- a^=b  ==  a.symmetric_difference_update(b)