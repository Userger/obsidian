[[методы словаря (dict) (methods)]]

- ## `setdefault(key, default)`
	Возвращает значение по ключу `key`, но если его нет, не бросает исключение, а создает элемент со значением `default` (по умолчанию `None`) по ключу `key`.
- ## update(value)
	добавляет в словарь элементы другого словаря, списка (кортежа) элементов с длиной 2. (`[("key", "value"), ("key1", "value1"), ("key2", "value2")] или ("he", "ll", "o!")`)
	если в словаре уже есть элементы с такими ключами, значения заменяются на новые 
	- [i] в качестве аргумента можно передавать именованные аргументы как в функцию создания словаря `dict()`
		`some_dict.update(hello="world")`
- ## `dict_[key] = value`
	создаст элемент по ключу `key` со значением `value` или изменит существующий элемент 