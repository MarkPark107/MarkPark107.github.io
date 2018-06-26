---
layout: post
title: "파이썬과 순회가능한 객체"
subtitle: "Python, Iterable"
categories: devlog
tags: python


---

> ## 파이썬과 순회가능한 객체

![a](C:\Users\gunhe\Desktop\a.jpg)

파이썬의 순회가능한 객체를 이해할 때, 세가지 파트로 나누어서 이해해 볼 수가 있다.

- Generator : 값을 생산해내는 객체
  - Generator expression : 제네레이터를 표현하는 표현식
  - Generator fuction : 제네레이터의 함수
- Iterable : 순회가능한, 여기서는 대부분 Iterable (object), 순회가능한 객체이다.
- Iterator : 순회하는 객체를 말함.

해석해보자면,

제네레이터 표현식과 제네레이터 함수가 제네레이터이며, 제네레이터는 언제나 순환하는 객체이다.

list, set, dict의 컨테이너는 전통적으로 '순회 가능한 객체이며' `iter()`를 통해 iterator로 변환할 수 있다.

iterator는 `next()`를 통하여 순회된다.



> #### Generator 맛보기

```python
# a generator expression
>>> (i ** 2 for i in range(10))
<generator object <genexpr> at 0x104c8d4c0

# generator expression 으로 list 생성
>>> list(i ** 2 for i in range (10))
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

# a generator function
def power():
    for i in range(10):
        yield i ** 2 # 함수 내에서 return 대신 yield로 쓰며, yield 시 마다 값을 생산

>>> power()
<generator object power at 0x10667dbf8>
>>> list(power())
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```



> #### 순회가능한(Iterable) 객체

- set, list, dict, tuple, string, generator는 모두 순회 가능한 객체
- Custum 클래스에서도 순회가능토록 만드는 것이 가능
  - \_\_iter\_\_ 멤버함수 구현 : self가 iterator로서 동작하기 위해 self를 반환
  - \_\_next\_\_ 멤버함수 구현 : iterator로서 동작
- for in 구문에서 활용 가능

```python
# string의 경우
for ch in "hello world"
h
e
l
l
o
 
w
o
r
l
d

# list의 경우
for i in [1, 2, 3]
	print(i)
1
2
3

# dict의 경우
mydict = {'a' = 1, 'b' = 2}

for key in mydict:
    print(key, mydict[key])
a 1
b 2

for key in mydict.keys():
    print(key)
a
b

for value in mydict.value():
    print(value)
1
2

for item in mydict.items():
    print(item)
('a', 1)
('b', 2)

for (key, value) in mydict.items():
    print(key, value)
a 1
b 2
```



> #### 클래스를 통한 Iterable 객체 만들기

```python
class MyRange(object):
    def __init__(self, start, end):
        self.start = start
        self.end = end

    def __iter__(self): # iterator를 요구 받고, 현 instance에서 next 처리
        return self
    
    def __next__(self):
        if self.start >= self.end:
            raise StopIteration # 남은 요소가 없을 떄, StopIteration 강제 발생
        value = self.start # 이전 next에서 다음으로 옮긴, 현재 값을 value에 할당
        self.start += 1 # 미리 값을 다음으로 옮김
        return value # 다음 요소를 return

>>> myiterable = MyRange(0, 3)
>>> for obj in myiterable:
    	print(obj)
0
1
2

# 다른 방식의 iterator 확인
>>> myrange = MyRange(0, 5)
>>> myiterator = iter(myrange)
>>> myiterator
<__main__.MyRange at 0x1bd4a5b44e0>
>>> next(myiterator)
0
>>> next(myiterator)
1
>>> next(myiterator)
2
>>> next(myiterator)
3
>>> next(myiterator)
4
>>> next(myiterator)
---------------------------------------------------------------------------
StopIteration                             Traceback (most recent call last)
<ipython-input-15-29fb3b4dbbec> in <module>()
----> 1 next(myiterator)

<ipython-input-5-5f1cc067d5da> in __next__(self)
      9     def __next__(self):
     10         if self.start >= self.end:
---> 11             raise StopIteration # 남은 요소가 없을 떄, StopIteration 강제 발생
     12         value = self.start # 이전 next에서 다음으로 옮긴, 현재 값을 value에 할당
     13         self.start += 1 # 미리 값을 다음으로 옮김

StopIteration: 
```



이번엔 여기까지 : )
