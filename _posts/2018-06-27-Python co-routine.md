---
layout: post
title: "파이썬과 코루틴"
subtitle: "Python, Co-routine, Generator"
categories: devlog
tags: python

---



> ## 파이썬2에는 왜 range와 xrange가 있었나?

파이썬2에는 range와 xrange함수가 있었다. 둘의 동작은 동일했는데, 왜 그랬을까?

```python
# Python2
for i in range(7): # python2에서 range 함수를 이용한 경우
	print(i)
0
1
2
3
4
5
6

for i in xragne(7): # python2에서 xrange 함수를 이용한 경우
    print(i)
0
1
2
3
4
5
6

# 아래의 경우를 통해 구현방식의 차이를 엿볼 수 있다.
>>> range(10)
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> xrange(10)
xrange(10)
```



그 이유는 동작 구현 방식에 있다.

- range : 범위의 값들을 리스트로 전부 구현 한 뒤 동작.
- xrange : 필요할 때마다 범위의 값을 생성.

위와 같이 두 함수의 동작 구현 방식이 달라, 퍼포먼스에서의 차이가 나타나게 된다.

- range의 경우, 메모리 공간이 추가적으로 필요하게 되고,
- xrange의 경우, CPU연산을 지속적으로 요구하게 된다.

리소스를 아끼기 보단 퍼포먼스를 얻는 것이 기대 가치가 높기 때문에, 파이썬3부턴 range가 사라지고, xrange가 range로 이름을 바꾸어 동작하게 된다.

```python
#Python3에서 기존 Python2방식의 range와 xrange의 기능 동작의 이해
def my_python2_range(start, end, step): # range 기능 동작 구성
    mylist = []
    while start < end:
        mylist.append(start)
        start += step
    return mylist

def my_python2_xrange(start, end, stop):
    while start < end:
    	yield start
        start += step
```

위와 같이 두 함수의 차이점을 볼 수 있는데, range는 범위의 리스트를 전부 만든 뒤 리스트 반환하므로, 리스트를 전부 만들 때까진 다른 동작을 하지 못한다. 반면 xrange는 값을 그때 그때 생성하여 반환하기에, 불필요한 대기시간을 요구하지 않게 된다. 실질적으로 동작시켜보면,

```python
>>> my_python2_range(0, 10, 2)
[0, 2, 4, 6, 8]
>>> my_python2_xrange(0, 10, 2)
<generator object my_python2_xrange at 0x106c65720>
```

로 range는 리스트를 반환하고, xrange는 generator object를 반환한 것을 확인 할 수 있다. *저번 시간의 순회 가능한 객체 참조하면 이해가 좀 더 쉽다*

```python
for i in my_python2_range(0, 10, 2):
    print(i)
0
2
4
6
8

for i in my_python2_xrange(0, 10, 2):
    print(i)
0
2
4
6
8
```

기능은 동일한 것 또한 확인 가능하다.



이러한 generator object, yield를 이용하여 어떤 프로그램을 만들 수 있을까?



> ## 코루틴(Co-routine)

바로, 코루틴을 가지는 프로그램을 작성할 수 있다. 일반적으로 우리가 아는 함수는

- Sub Routine : 서브 루틴
  - 진입점이 하나이며, 부모/자식의 종속적인 관계가 성립
  - 매 호출시마다, Routine 내 context가 초기화

이다. 반면 코루틴은

- Co-routine : 코루틴
  - 진입점이 여럿이며, 병렬(Concurrensy, not Parallelism) 수행
  - 호출부와 대등한 관계
  - 여러번 호출되어도, Routine 내 Contect 유지

이다. 이를 이용하여, 상태를 유지하며, 상호작용 가능한 프로그램의 설계가 가능하다.

*아직 상호작용이라기 보단 협업하며, wait&go 형태의 느낌이다*

```python
# Co-routine 함수의 예
def cofunc():
    yield 1
    yield 3
    yield 5
    yield 7
    yield 'End of cofunc'

>>> cofunc() # 호출시 생성되어 generator object가 생성되었음을 확인 가능
<generator object cofunc at 0x000001BD4A575CA8>
>>> next(cofunc()) # 호출 생성되어 next까지 진행.
1
>>> next(cofunc()) # 호출 생성되어 next까지 진행이므로, 원하는 3이 아닌 1이 나옴
1
>>> next(cofunc()) # 호출 생성되어 next까지 진행이므로, 원하는 5이 아닌 1이 나옴
1

# 위의 상황은 인스턴스가 지속 생성되는 것을 생각지 않아 발생한 문제이므로 아래와 같이 사용.
>>> genaratedcofunc = cofunc() # 매번 confuc()으로 호출할 시 새로 다른 generator object가 생성되므로, 위치를 변수에 할당하여 호출
>>> next(generatedcofunc)
1
>>> next(generatedcofunc)
3
>>> next(generatedcofunc)
5
>>> next(generatedcofunc)
7
>>> next(generatedcofunc)
'End of cofunc'
>>> next(generatedcofunc)
---------------------------------------------------------------------------
StopIteration                             Traceback (most recent call last)
<ipython-input-50-82a7576b64d2> in <module>()
----> 1 next(generatedcofunc)

StopIteration: 
```

호출에 따라 Context의 변화 없이 진행되는 것을 볼 수 있다.

```python
# 간단한 코루틴 예제
def mysum(x, y):
    x += 1
    y += 1
    return x + y

def base_increased(base):
    i = 0
    yield base
    i += 1
    yield base + 1
    i += 1
    yield base + 2
    i += 1
    yield base + 3
    yield i

def main():
    a = 1
    b = 2
    
    generated_obj1 = base_increased(10)
    generated_obj2 = base_increased(20)
    generated_obj3 = base_increased(30)
    
    c = a + b
    
    print(mysum(a, b)) # 5
    
    print(next(generated_obj1)) # 10
    print(next(generated_obj1)) # 11
    print(next(generated_obj2)) # 20
    print(next(generated_obj2)) # 21
    print(next(generated_obj3)) # 30
    print(c) # 3

# 실행
>>> main()
5
10
11
20
21
30
3
```



Generator를 생산하게 되는 Co-routine 함수에서는 return을 만나도 함수가 종료될 뿐, 반환 값을 실제로 이용하지는 않는다.

**추가적인 yield가 없는 경우, StopIteration 예외가 자동 발생됨**

이를 이용하여, 새로운 예외처리 상황을 코딩해도 되나, 예외처리를 직접하지는 않아도 되며, for 구문의 경우 StopIteration 예외를 자동으로 처리해준다. 만약 특정 조건에 의해 StopIteration 에러를 발생시켜야 할 경우 `raise StopIteration`을 통해 가능하다.

```python
def cofunc2():
    yield 1
    yield 3
    raise StopIteration
    yield 5
    yield 7
    yield 'End of cofunc'

>>> generated_cofunc2_obj = cofunc2()
>>> next(generated_cofunc2_obj)
1
>>> next(generated_cofunc2_obj)
3
>>> next(generated_cofunc2_obj)
---------------------------------------------------------------------------
StopIteration                             Traceback (most recent call last)
<ipython-input-57-f66f1ce62e54> in <module>()
----> 1 next(generated_cofunc2_obj)

<ipython-input-53-b31805f61ab6> in cofunc2()
      2     yield 1
      3     yield 3
----> 4     raise StopIteration
      5     yield 5
      6     yield 7

StopIteration: 

# for 문의 경우
>>> for i in cofunc2():
    	print(i)
1
3
```



> ## Pipe, 중첩된 Generator

중첩된 Generator의 경우, Pipe라고 불리는데, 일반적으로 컴퓨터 공학에서 pipe구조란 어떠한 특정 작업을 한 뒤, 이걸 다음 작업대로 넘겨주는 구조를 말한다.

*이게 무슨 소린가 싶겠지만, pipe를 연결하는 구조와 같으며, 파이프는 쇠파이프, 플라스틱 파이프, 구리 파이프 등이 있듯 연결된 구조와 방식에 따라 다른 동작이 가능하고, 교체 및 방향의 전환까지 가능하다는 개념 전재를 가지고 있는 편이 좋다*

```python
# 매 Generator가 완료 된 후, 다음 Generator가 수행되는 것이 아니라,
# 예, 위의 예제들 중,
#    print(next(generated_obj1)) # 10
#    print(next(generated_obj1)) # 11
#    print(next(generated_obj2)) # 20
#    print(next(generated_obj2)) # 21
#    print(next(generated_obj3)) # 30
# 한 Generator가 값을 생산할 때마다 다음 Generator로 넘겨주는 구조

>>> gen1 = (i ** 2 for i in range(10)) # gen1에서 0이 생산되자마자
>>> gen2 = (j + 10 for j in gen1) # gen2로 전달되고 다시 생산된 값은
>>> gen3 = (k * 10 for k in gen2) # gen3로 전달

>>> for i in gen3:
    	print(i, end=' ')
100 110 140 190 260 350 460 590 740 910
```

Generator를 활용하여 피보나치 수열 생성하기

```python
def fib()
	x, y = 1, 1
    while True:
    	yield x
        x, y = y, x + y

>>> count = 0
        for x in fib():
            print(x)
            count += 1
            if count >= 10:
                break
1 1 2 3 4 8 13 21 34 55

# islice 활용, islice : iterable 객체를 특정 값까지만 iter 되도록 제한된 객체를 생성하는 함수
from itertools import islice

>>> islice(fib(), 10)
<itertools.islice at 0x106864958>
>>> tuple(islice(fib(), 10))
(1, 1, 2, 3, 5, 8, 13, 21, 24, 55)
```



> ## list/set/dict Comprehension

- 순회가능한 객체를 조작하여, 필터링/새로운 리스트/사전/집합을 만들 수 있는 방법
- tuple comprehension은 없으나, 필요시 tuple(순회가능한 객체) 함수를 통하여 생성 가능

```python
# list comprehension
[표현식 for 변수 in 순회가능한 객체]
[표현식 for 변수 in 순회가능한 객체 if 조건]

[i ** 2 for i in range(5)] # [0, 1, 4, 9, 16]
[i ** 2 for i in range(5) if i % 2 == 0] # [0, 4, 16]

# dict comprehension
{Key:표현식 for 변수 in 순회가능한 객체}
{Key:표현식 for 변수 in 순회가능한 객체 if 조건}

{i:i ** 2 for i in range(5)} # {0:0, 1:1, 2:4, 3:9, 4:16}
{i:i ** 2 for i in range(5) if i % 2 == 0} # {0:0, 2:4, 4:16}

#set comprehension
{표현식 for 변수 in 순회가능한 객체}
{표현식 for 변수 in 순회가능한 객체 if 조건}

{i%5 for i in range(20)} # {0, 1, 2, 3, 4}
{i%5 for i in range(20) if i % 2 == 0} # {0, 1, 2, 3, 4}
```



> ## Generator 표현식

문법 비교 : list comprehension v.s. Generator comprehension

```python
# list comprehension
>>> [i ** 2 for in range(10)]
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

# Generator comprehension
>>> (i ** 2 for in range(10))
<generator object <genexpr> at 0x10654ec50>

# make a list with Generator comprehension with list()
>>> list((i ** 2 for in range(10)))
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

# make a tuple with Generator comprehension with tuple()
>>> tuple((i ** 2 for in range(10)))
(0, 1, 4, 9, 16, 25, 36, 49, 64, 81)
```



이번엔 여기까지 : )
