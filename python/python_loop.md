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

## 例题
给定一个不超过5位的正整数，判断该数的位数，依次打印出个位、十位、千位、万位的数字
```
n=int(input('input a number:'))
n*=10
while(n//10)!=0
	n//=10
    print(n%10)
```
```
num=input('num is:')
numV=len(num)
print(numV)
num=int(num)
for i in range(numV):
    print(num%10)
    num=num//10
```

## 练习：
1.打印一个边长为n的正方形
```
n = int(input('正方形边数:')) #4
print('*'*n) #****
for i in range(n-2):
    print('*'+' '*(n-2)+'*')
print('*'*n) #****
```

2.求100内所有奇数的和（2500)
```
n = 0
for i in range(100):
    if i%2:
        n+=i
print(n)
```
更优
```
n = 0
for i in range(1,100,2):
    n += i
print(n)
```

3. 判断学生成绩，成绩等级A-E，其中，90分以上为'A',80-89分为'B',70-79分为'C'，60-69分为'D',60分以下为'E'
```
s = int(input('请输入成绩:'))
if s >= 60:
    if s >= 90:
        print('A')
    elif s >= 80:
        print('B')
    elif s> = 70:
        print('C')
    else:
        print('D')
else:
    print('E')
```
4. 求1到5阶乘之和
给一个数，判断它是否是为素数（质数）
\#质数：一个大于1的自然数只能被1和它本身整除。

## 作业：
打印99乘法表
打印右侧菱形
打印100以内的斐波那契数列
求斐波那契数列第101项
求10万内的所有素数