---
layout: post
title: "파이썬 퀴즈1"
subtitle: "Python, Quiz1"
categories: devlog
tags: python
---



> ## 파이썬 Quiz

Q1. 1부터 n까지의 합을 출력하기

```python
def solve_n1():
    indata = int(input("input integer value of n : "))
    sum = 0
    for i in range(indata):
        sum += i + 1
    print("sum from 1 to {} : {}".format(indata, sum))
```

```python
>>> solve_n1()
input integer value of n : 10
sum from 1 to 10 : 55

>>> solve_n1()
input integer value of n : 100
sum from 1 to 100 : 5050
```



Q2. 1부터 n까지 연속한 숫자의 제곱의 합을 구하는 프로그램을 for 반복문으로 만들기

```python
def solve_n2():
    indata = int(input("input integer value of n : "))
    sum = 0
    for i in range(indata):
        sum += (i + 1) ** 2
    print("sum of i^2 from 1 to {} : {}".format(indata, sum))
```

```python
>>> solve_n2()
input integer value of n : 10
sum of i^2 from 1 to 10 : 385
```



Q3. 주어진 n개의 숫자를 받아 가장 큰 숫자를 찾는 알고리즘을 만들어 보세요

```python
def solve_n3():
    indatan = int(input("input integer value of number of a numeric list : "))
    nlist = []
    for i in range(indatan):
        innum = int(input("input number : "))
        nlist.append(innum)
    maxdata = None
    for i,element in zip(range(len(nlist)), nlist):
        if i == 0:
            maxdata = element
        else:
            if maxdata < element:
                maxdata = element
    print("Maximum number of the list is {}".format(maxdata))
```

```python
>>> solve_n3()
input integer value of number of a numeric list : 5
input number : 2
input number : 100
input number : 2
input number : 101
input number : 4
Maximum number of the list is 101
```



Q4. 주어진 n개의 숫자를 받아 가장 작은 숫자를 찾는 알고리즘을 만들어 보세요

```python
def solve_n4():
    indatan = int(input("input integer value of number of a numeric list : "))
    nlist = []
    for i in range(indatan):
        innum = int(input("input number : "))
        nlist.append(innum)
    mindata = None
    for i,element in zip(range(len(nlist)), nlist):
        if i == 0:
            mindata = element
        else:
            if mindata > element:
                mindata = element
    print("Minimum number of the list is {}".format(mindata))
```

```python
>>> solve_n4()
input integer value of number of a numeric list : 5
input number : 2
input number : -10
input number : 20
input number : 100
input number : 5
Minimum number of the list is -10
```



Q5. 주어진 n개의 숫자를 받아 가장 큰 숫자의 Index를 찾는 알고리즘을 만들어 보세요.

```python
def solve_n5():
    indatan = int(input("input integer value of number of a numeric list : "))
    nlist = []
    for i in range(indatan):
        innum = int(input("input number : "))
        nlist.append(innum)
    maxdata = None
    maxidx = None
    for i,element in zip(range(len(nlist)), nlist):
        if i == 0:
            maxdata = element
        else:
            if maxdata < element:
                maxdata = element
                maxidx = i
    print("Maximum number index of the list is {}".format(maxidx))
    print("list[{}] = {}".format(maxidx, nlist[maxidx]))
```

```python
>>> solve_n5()
input integer value of number of a numeric list : 5
input number : -129
input number : 29
input number : -20
input number : 500
input number : 1
Maximum number index of the list is 3
list[3] = 500
```



Q6. 주어진 n개의 숫자를 받아 가장 작은 숫자의 Index를 찾는 알고리즘을 만들어 보세요.

```python
def solve_n6():
    indatan = int(input("input integer value of number of a numeric list : "))
    nlist = []
    for i in range(indatan):
        innum = int(input("input number : "))
        nlist.append(innum)
    mindata = None
    minidx = None
    for i,element in zip(range(len(nlist)), nlist):
        if i == 0:
            mindata = element
        else:
            if mindata > element:
                mindata = element
                minidx = i
    print("Minimum number index of the list is {}".format(minidx))
    print("list[{}] = {}".format(minidx, nlist[minidx]))
```

```python
>>> solve_n6()
input integer value of number of a numeric list : 5
input number : 30
input number : 22
input number : -1330
input number : 20
input number : 555
Minimum number index of the list is 2
list[2] = -1330
```



이번엔 여기까지 : )