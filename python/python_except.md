```
try:
    year = int(input('input year:'))
except ValueError:
    print('Please input year.')
```
```
try:
    print(1/0)
except ZeroDivisionError as e:
	print("0 is error %s" %e)
```

定义错误
```
try:
    raise NameError('helloError')
except NameError:
    print('my custom error')
```
```
try:
    a=open('name.txt')
except Exception as e:
    print(e)
finally:
    a.close()
```