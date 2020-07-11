---
title: python 常用
categorys:
  - python
date: 2020-07-11 14:12:59
---

#### 常用类型

https://www.jianshu.com/p/ed1a76ae0b87  python中的数组、字典、以及他们的遍历方式

* list：可变数组，可以增删改查。

  ```python
  #定义一个list变量
  L1 = ['1','2',3,True]
  # len()获取元素个数
  print(len(L1))
  # 索引取值,注意不要越界哈，最后一个元素的索引是len(L1) - 1
  print(L1[2])
  
  #倒叙索引【-3，-2，-1】
  print(L1[-1])
  
  #增删改查
  
  L1.append("我是增加的元素，会追加到list末尾位置")
  
  L1.index(1,"我是插入的元素,第一个参数是索引")
  
  L1.pop(2)#如果这个参数不写，会自动删除最后一个元素
  
  L1[1]="用来替换的新元素"
  ```

  

* tuple: 有序，但是一旦初始化就不可更改

  ```python
  #没有元素、多个元素、一个元素，注意一个元素的写法t2（1）这样写是不对的
  t = ()
  t1= ("1","2")
  t2=(1,)
  ```

  

* dict

  ```python
  d = {'name':'hello_world', 'age': 27, 'sex': 'man'}
  
  #key不存在会报错
  #判断key值是否存在
  if 'age' in d:
      print(d['name'])
  
  #还可以用get（）取值，如果key不存在，可以返回None，或者自己指定的value
  d.get('price')
  d.get('price',30)
  
  #删除key，它对应的value也会删除掉,注意pop方法调用返回的是对应的value值
  print(d.pop('age'))
  ```

  

* set: 和dict类似，也是一组key的集合，但不存储value。由于key不能重复，所以，在set中，没有重复的key。创建一个set，需要提供一个list作为输入集合。

  ```python
  s=set([1,2,3])
  s1=set([2,3,4])
  s2=set([1,1,2,2,3,4])
  
  #重复元素在set中自动被过滤
  print(s2)
  
  #添加元素，因为元素不重复机制，多次添加同一个值，只有一个会被添加进去，只要记住他元素不重复这个规则就好
  s.add(5)
  #删除元素,如果set离没有元素你还要删除这个元素，那么活该你报错。s.remove(10)，这就是错误的示范
  s.remove(3)
  
  #set之间的交集和并集操作
  
  print(s & s1)
  print(s | s1)
  
  
  print out:
  {1, 2, 3, 4}
  {2}
  {1, 2, 3, 4, 5}
  ```

  

#### 其他

* 直接运行.py文件, 方法是在.py文件的第一行加上一个特殊的注释, 给文件执行权限， 就可以直接运行./*.py：

  ```python
  #!/usr/bin/env python3
  
  print('hello, world')
  ```

  

