
input:
`a = ("John", "Charles", "Mike")`
`b = ("Jenny", "Christy", "Monica")`

`a = ["John", "Charles", "Mike"]`
`b = ["Jenny", "Christy", "Monica"]`

`x = zip(a, b)`

`print(x)`

output:
`<zip object at 0x15118d6de400>`




input:
`print(list(x))`
output:
`[('John', 'Jenny'), ('Charles', 'Christy'), ('Mike', 'Monica')]`

input:
`print(tuple(x))`
output:
`(('John', 'Jenny'), ('Charles', 'Christy'), ('Mike', 'Monica'))`

input:
`print(dict(x))`
output: 
`{'John': 'Jenny', 'Charles': 'Christy', 'Mike': 'Monica'}`