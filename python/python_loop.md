# Python 循环
## if
例：输入2个数字，输出最大数
```
a=int(input('input a number:'))
b=int(input('input a number:'))
if a>b:
    print (a)
elif a<b:
	print (b)
else:
    print ('=')
#print(max(a,b))
```
## while
```
flag=10
while flag:
    print(flag)
    flag-=1
```
死循环
```
while True:
input(>>>)
```
## for
```
for i in range(10):
	print(i+1)
```

## continue
例：打印 0 2 4 8
```
for i in range(10):
    if not i%2:
        print(i)
```

```
for i in range(10):
    if i%2:
        continue
    print(i)
```
```
for i in range(0,10):
    if i & 1:
        continue
    print(i)
```

## break
终止当前循环
例：计算1000以内的被7整除的前10个数（for）
```
count = 0
for i in range(0,1000,7):
    print(i)
    count += 1
    if count >= 10:
        break
```

## continue break总结  
continue和break是循环的控制语句，只影响当前循环，包括while、for  
如果循环嵌套，continue和break也只影响语句所在的那一层循环  
continue和break不是跳出语句块，所以if cond: break 不是跳出if,而是终止if外的break所在的循环  
