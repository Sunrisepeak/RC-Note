---
title: 'Python-Collection >> Basic Operator of List[]'
mathjax: false
tags:
  - Python
  - Collection
  - List
  - CRUD
categories:
  - cs
  - python
abbrlink: 1010458050
---



#### Basic Characteristics

> A list is a collection of items in a particular order.

##### Declaration & define -- store any object[type]

```python
lt = [1, 1.1, 'str', ['list']] # contain type : [int, float, string, list] 
```



#### Basic Operator[CRUD]

##### Accessing Elements by index

```python
           [1, 1.1, 'str', ['list']]
postive:    0   1     2        3
negative:  -4  -3    -2       -1
```
<!-- more -->

###### Positive Index -- index of interval is [0, length), which begin from 0.

```python
print(lt[0]) #output: 1
```

###### Negative Index -- index of interval is [-lentgh, -1]

```python
# Tiny skill: output last element of list by -1(index), rather than 3(lenght - 1)
print(lt[-1]) # output: ['list']
```



##### Adding Elements -- append(), insert()

###### append() -- at the end of a list

```python
lt.append('Adding')
print(lt) # output: [1, 1.1, 'str', ['list'], 'Adding']
```

###### insert(index, obj) -- index is position of adding you want.  interval is [0, length) OR [-length, -1]

```python
lt.insert(0, 'insert_0')
lt.insert(-1, 'insert_end(-1)')
print(lt) # output:['insert_0', 1, 1.1, 'str', ['list'], 'insert_end(-1)', 'Adding']
# Is consuqence of output different with you ?
# insert(index, obj) method is backwards move a unit for elements of
# index rihgt-hand and itself.To -1(index), position of end before change. So:
# ['insert_0', 1, 1.1, 'str', ['list'], 'Adding']
#                                          -1
# ['insert_0', 1, 1.1, 'str', ['list'], 'insert_end(-1)', 'Adding']
#                                              | 
#                                       insert of position
```



##### Remove Elements --  del, pop(), remove()

###### del statement --- certain position by index

```python
del lt[-1]
print(lt) # output: ['insert_0', 1, 1.1, 'str', ['list'], 'insert_end(-1)']
```

###### pop() & pop(index)   --  remove with return-value

```python
# like stack -- DS
re_value = lt.pop()
print(re_value)	# output: insert_end(-1)
print(lt) # ['insert_0', 1, 1.1, 'str', ['list']]
```

###### remove(value) -- by value

```python
lt.remove(1)
print(lt) # output: ['insert_0', 1.1, 'str', ['list']]
```



##### Change Elements -- by index

```python
lt[-2] = 'string' # change 'str' to 'string'
print(lt) # output: ['insert_0', 1.1, 'string', ['list']]
```



#### Code Summary

```python
lt = [1, 1.1, 'str', ['list']] # contain type : [int, float, string, list]

print(lt[0]) #output: 1

# Tiny skill: output last element of list by -1(index), rather than 3(lenght - 1)
print(lt[-1]) # output: ['list']

lt.append('Adding')
print(lt) # output: [1, 1.1, 'str', ['list'], 'Adding']

lt.insert(0, 'insert_0')
lt.insert(-1, 'insert_end(-1)')
print(lt) # output:['insert_0', 1, 1.1, 'str', ['list'], 'insert_end(-1)', 'Adding']

del lt[-1]
print(lt) # output: ['insert_0', 1, 1.1, 'str', ['list'], 'insert_end(-1)']

# like stack -- DS
re_value = lt.pop()
print(re_value)
print(lt)

lt.remove(1)
print(lt)

lt[-2] = 'string' # change 'str' to 'string'
print(lt)

```



#### Environment

+ Ubuntu18.x
+ python 3.6.9

#### Reference 

+ [Python Crash Course](https://book.douban.com/subject/26284937/)

#### QuickLink

+ [SPeakNote Blog](https://sunrisepeak.github.io/)
+ [Makedown File Address](https://github.com/Sunrisepeak/RC-Note)
+ [Github](https://github.com/Sunrisepeak)