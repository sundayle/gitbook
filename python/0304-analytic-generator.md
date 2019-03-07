# 解析式、生成器

## 标准库datetime
```
>>> datetime.datetime.now().timestamp()
1550507872.40152
a=datetime.datetime.now()
datetime.datetime(2019, 2, 19, 0, 53, 57, 302356)
>>> a.date()
datetime.date(2019, 2, 19)
>>> a.time()
datetime.time(0, 53, 57, 302356)
>>> a.isocalendar()
(2019, 8, 2)
>>> a.strftime('%Y~%m~%d %H-%M-%S')
'2019~02~19 00-53-57'
>>> '{} {} {}'.format(a.year,a.month,a.day)
'2019 2 19'
>>> dt=datetime.datetime.strptime("21/11/06 16:30","%d/%m/%y %H:%M")
>>> print(dt.strftime("%Y-%m-%d %H:%M:%S"))
2006-11-21 16:30:00

>>> h=datetime.timedelta(hours=24)
>>> h
datetime.timedelta(days=1)
>>> datetime.datetime.now()
datetime.datetime(2019, 2, 19, 1, 3, 21, 544053)
>>> datetime.datetime.now()-h
datetime.datetime(2019, 2, 18, 1, 3, 0, 776120)

>>> n=datetime.datetime.now()
>>> (datetime.datetime.now() -n).total_seconds()
5.1751
>>> 
```
列表解析式
```
newlist=[]
for i in range(10):
    newlist.append((i+1)**2)
print(newlist)

newlist1=[ (i+1)**2 for i in range(10)]
print(newlist1)
```