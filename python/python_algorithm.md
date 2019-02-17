
## 冒泡法
```
num_list=[
    [1,9,8,5,6,7,4,3,2],
    [1,2,3,4,5,6,7,8,9]
]
nums=num_list[0]
print(nums)
length=len(nums)
count_swap=0
count=0

for i in range(length):
    for j in range(length-i-1):
        count+=1
        if nums[j] > nums[j+1]:
            tmp=nums[j]
            nums[j]=nums[j+1]
            nums[j+1]=tmp
            count_swap+=1
print(nums,count_swap,count)
```
优化版
```
num_list=[
    [1,9,8,5,6,7,4,3,2],
    [1,2,3,4,5,6,7,8,9]
]
nums=num_list[0]
print(nums)
length=len(nums)
flag=False
count_swap=0
count=0

for i in range(length):
    for j in range(length-i-1):
        count+=1
        if nums[j] > nums[j+1]:
            tmp=nums[j]
            nums[j]=nums[j+1]
            nums[j+1]=tmp
            flag=True
            count_swap+=1
    if not flag:
        break
print(nums,count_swap,count)
```
```
123   147
456=> 258
789   369
```
```
matrix=[[1,2,3],[4,5,6],[7,8,9]]
print(matrix)

count=0
for i,row in enumerate(matrix):
    for j,col in enumerate(row):
        if i<j:
            temp=matrix[i][j]
            matrix[i][j]=matrix[j][i]
            matrix[j][i]=temp
            count+=1
print(matrix)
```
```
import random
num=[]
for _ in range(10):
    num.append(random.randint(1,20))
print(num)
repeat=[]
single=[]
for i in num:
    if num.count(i)>1:
        if i not in repeat:
            repeat.append(i)
    else:
        single.append(i)

print('repeat:{},{}'.format(len(repeat),repeat))
print('single:{},{}'.format(len(single),single))
```
随机产生2组各10位的列表，如下要求：
每个数字取值范围[10,20]
统计20个数字中，一共有多少个不同的数字？
2组中，不重复的数字有几个？分别是什么？
2组中，重复的数字有几个？分别是什么？
```
a=[1,9,7,5,6,7,8,8,2,6]
b=[1,9,0,5,6,4,8,3,2,3]
s1=set(a)
s2=set(b)
print(s1)
print(s2)
print(s1.union(s2))
print(s1.symmetric_difference(s2))
print(s1.intersection(s2))
```
##字典遍历
```
for item in d.items():
	print(item)
for k,v in d.items():
 	print(k,v)
```
##列表解析
打印1-10的平方
```
print([i**2 for i in range(1,11)])
```
有一个列表lst=[1,4,9,16,2,5,10,15],生成一个新列表，要求新列表的元素是lst相邻两项的和
```
lst=[1,4,9,15,2,5,10,15]
print(len(lst))
lstnew=[lst[i]+lst[i+1] for i in range(len(lst)-1)]
print (lstnew)

```
```
[print('{}*{}={:<3}{}'.format(j,i,i*j,'\n' if i==j else ''),end="")for i in range(1,10) for j in range(1,i+1)]
```
生成100个 "0001.abcdicddws" ID格式，左边是4位从1开始，右边是10位随机小写英文。
```
import random
a=["{:04}.{}".format(i,"".join([chr(random.randint(97,123))for j in range(10)]))for i in range(1,101)]
print(a)
```