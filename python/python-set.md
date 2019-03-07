# Python 集合
## 并集
```
a={1,2,3}
b={2,3,4}
c=a.union(b)
c
{1, 2, 3, 4}
a | b
```
update
```
c=a.update(b)
a
{1, 2, 3, 4}

>>> a|={5,6} | {7,8}
>>> a
{1, 2, 3, 4, 5, 6, 7, 8}
```
## 交集
intersection(other)
&
intersection_update(other)
&=

## 差集
减去相同部分

difference(other)
-
difference_update(other)
-=
```
>>> a={1,2,3,4,5}
>>> b={3}
>>> a-b
{1, 2, 4, 5}
```
## 对称差集
公式 (a-b)u(b-a)

symmetric_differece(other)
^
symmetric_differece_update(other)
^=

集合运算
issubset(other) <=
判断当前集合是否在另一个集合的子集
set1<set2
判断set是否是set2的真子集
issuperset(other)>=
判断当前集合是否是other的超集
set1>set2
判断set1是否是set2的真超集
isdisjoint(other)
没有交集，返回true


## 练题
共同好友
  你的好友A、B、C ，他的好友 C、 B、D，求共同好友
  {'A','B','C'}.intersection({'B','C','D'})

微信群提醒
  XXX与群里其他人都不是微信好友关系
  并集：userid in (A|B|C|...) == False, A,B,C等是微信好友的并集，用户ID不在这个并集中，说明他和任何人都不是朋友

权限判断
有一个API，要求权限同时具备A，B，C才能访问，用户权限 是B，C，D，判断用户是否能够访问该API
  API集合A，权限集合P
  A-P={}，A-P为空集，说明P包含A
  A.issubset(P)也行，A是P的子集也行。
  A&P=A 也行
有一个API，要求权限具备A，B，C任意一项，用户权限 是B，C，D，判断用户是否能够访问该API
  API集合A，权限集合P
  A&P !{}就可以
  A.isdisjonit(P)==False 表示有交集

一个总任务列表，存储所有任务，一个已完成的任务列表，找出未完成的任务
  业务中，任务ID一般不可以重复
  所有任务ID放到一个set中，假设为ALL
  所有已完成的任务ID放到一个set中,假设为COMPLETED，它是ALL的子集
  ALL-COMPLETED=UNCOMPLETED

随机产生2组各10个数字的列表。如下要求。
  每个数字取值范围[10,20]
  统计20个数字中，一共有多少个不同的数字? 
  2组中，不重复的数字有几个?分别是什么? 
  2组中，重复的数字有几个?分别是什么?
```
a = [1, 9, 7, 5, 6, 7, 8, 8, 2, 6]
b = [1, 9, 0, 5, 6, 4, 8, 3, 2, 3] 
s1 = set(a)
s2 = set(b)
print(s1)
print(s2)
print(s1.union(s2)) print(s1.symmetric_difference(s2)) print(s1.intersection(s2))
```