[[tuple (кортеж)]]


- от кортежей можно получить срезы 
	`t = ("hello", "world")`
	`new_t = t[0:1]`
	*срез тоже кортеж* - `("hello",)`

## операции с помощью срезов:

- удаление элемента - создание нового кортежа без определенного элемента(элементов)
	`t = t[:2] + t[3:]` - *удаление элемента с индексом* **2**

- вставка элементов в кортеж: 
	
	`c = c[:3] + ("a", "b") + c[3:]` - *вставка "a", "b" перед третьим элементом в кортеже*

- замена определенного элемента в кортеже:
	`c = c[:2] + (7,) + c[3:]` - *замена элемента с индексом* **2**