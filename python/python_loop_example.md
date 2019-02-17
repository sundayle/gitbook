# 循环例题与练习

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

1. 判断学生成绩，成绩等级A-E，其中，90分以上为'A',80-89分为'B',70-79分为'C'，60-69分为'D',60分以下为'E'

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

2. 求1到5阶乘之和
```
nums = 1
x = 0
for n in range(1,6)
	num *= n
	x += nums
print(x)
```
3. 给一个数，判断它是否是为素数（质数）
   \#质数：一个大于1的自然数只能被1和它本身整除。
```
num=int(input('Please input a number:'))
if num==2:
    print('prime')
if num>2:
    for i in range(2,num):
        if num % i == 0:
            print(num,'not prime')
            break
    else:
        print('prime')
```

## 作业：

1. 打印99乘法表
```
for i in range (1,10):
    for j in range(1,i+1):
        #print(str(j)+'*'+str(i)+'='+str(i*j),end=' ')
        print('{0}*{1}={2:<2}'.format(i,j,i*j),end=' ')
    print()
```
反序
```
for i in range (1,10):
    for m in range(i):
        print("      ",end=" ")
    for j in range(1,10):
        #print(str(j)+'*'+str(i)+'='+str(i*j),end=' ')
        print('{0}*{1}={2:<2}'.format(i,j,i*j),end=' ')
    print()
```
2. 打印右侧菱形
```
   *
  ***
 *****
*******
 *****
  ***
   *
   
for i in range(-3,4):
    if i<0:
        prespace = -i
    else:
        prespace = i
    print(' '*prespace +'*'*(7-prespace*2))
```
```
for i in range(-3,4):
    if i<0:
        print(' '*(-i)+'*'*(4+i))
    elif i>0:
        print(' '*3+'*'*(4-i))
    else:
        print('*'*7)
   *
  **
 ***
*******
   ***
   **
   *
```

3. 打印100以内的斐波那契数列
```
a=0
b=1
while True:
    c=a+b
    if c>100: break
    a=b
    b=c
    print(c)
```
```
i,j=1,1
while i<100:
    i,j=j,i+j
    print(i,end=' ')
```
4. 求斐波那契数列第101项
```
a=1
b=1
index=2
print('{0},{1}'.format(0,0))
print('{0},{1}'.format(1,1))
print('{0},{1}'.format(2,1))
while True:
    c=a+b
    a=b
    b=c
    index +=1
    print('{0},{1}'.format(index,c))
    if index == 101:
        break
```
5. 求10万内的所有素数
```
count=0
for x in range(2,10000):
    for i in range(2,x):
        if x % i == 0:
            break
    else:
        count +=1
print(count)
```
优化后
```
count=0
for x in range(2,100000):
    for i in range(2,int(x ** 0.5)+1):
        if x%i==0:
            break
    else:
        count+=1
print(count)
```
算法优化
```
import math
n=100
primenumber=[]
for x in range(2,n):
    for i in primenumber:
        if x%i==0:
            break
    else:
        print(x)
        primenumber.append(x)

import math
primenumber=[]
flag=False
for x in range(2,1000000):
    for i in primenumber:
        if x%i==0:
            flag=True
            break
        if i>=math.ceil(math.sqrt(x)):
            flag=False
            break
    if not flag:
        print(x)
        primenumber.append(x)
```
