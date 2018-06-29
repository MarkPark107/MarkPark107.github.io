---
layout: post
title: "파이썬과 빌트인함수 2"
subtitle: "Python, Built-in Function"
categories: devlog
tags: python

---

> ## 파이썬과 빌트인함수2

파이썬에서의 정렬 또한 sort를 사용한다. 함수명은 `sorted()`이다.

> #### Sorted
>
> 정렬된 리스트를 반환

sorted_list = sorted(iterable, key=None, reverse=False)

- key : 정렬 기준값을 생성할 **함수**를 지정. iterable 객체의 각 원소마다 key함수가 호출되고 그 리턴값으로 정렬을 수행

```python
def sort_fn(value):
    return value % 10 # 1의 자리수로서 정렬을 수행할려면

>>> sorted_list = sorted([19, 25, 32, 45], key=sort_fn, reverse=True)
[19, 25, 45, 32] # 1의 자리수만 보면 [9, 5, 5, 2]로 역순정렬되었음을 확인할 수 있음. 같은 수의 경우 reverse옵션과 상관없이 입력순서로 적게 됨. Why? 1의 자리수가 기준이므로, 10의 자리수를 판단하지 않고 reverse이기 때문에; 5와 5를 reverse 옵션에따라 비교해도 굳이 서로 자릴 바꿀 이유가 없음.
>>> sorted([19, 25, 32, 45], key=sort_fn, reverse=False)
[32, 25, 45, 19]
```



> #### filter
>
> 지정함수로 필터링된 결과를 생산할 Iterator를 반환
>
> 각 원소마다 지정함수가 호출되어, 리턴값이 True일 경우 통과

iterator = filter(필터링여부를결정할함수, iterable)

```python
def judge_fn(value):
    return value % 2 == 0 # 짝수인 경우 통과

>>> iterator = filter(judge_fn, [1, 2, 3, 4, 5, 6, 7])
>>> iterator
<filter at 0x104888211>
>>> list(iterator)
[2, 4, 6]
```



> #### map
>
> 지정함수의 리턴값을 반환할 Iterator 반환. 이라고 어렵게 써있는데,
>
> map은 mapping의 약자로 지정된 함수를 통하여 각 value들을 변환한 값을 가지는 iterator를 반환한다.

iterator = Map(값을변환하는함수, iterable)

```python
def power_fn(value):
    return value ** 2

>>> iterator = map(power_fn, [1, 2, 3, 4, 5, 6, 7])
>>> iterator
<map at 0x10558a127>
>>> list(iterator)
[1, 4, 9, 16, 25, 36, 49]
```



> #### filter, map 두 가지를 엮어보자.

```python
def power3_fn(value):
    return value ** 3
def odd_fn(value):
    return value % 2 == 1

>>> iterator = map(power3_fn, filter(odd_fn, [1, 2, 3, 4, 5, 6, 7]))
>>> iterator # 마지막에 적용된 map으로 object가 생성됨을 확인
<map at 0x1643917e898>
>>> [1, 27, 125, 343]

# 익명함수, lambda를 이용한 구현

>>> iterator2 = map(lambda value:value ** 3, filter(lambda value: value % 2 == 1, [1, 2, 3, 4, 5, 6, 7]))
>>> iterator2
<map at 0x1643919b128>
>>> list(iterator2)
[1, 27, 125, 343]
```



> #### max / min
>
> iterable에서 key함수를 거친 결과값 중에 가장 큰 결과값을 가지는 값을 반환

최대값 = max(iterable[, default=obj]\[, key=값을변환할함수])

최소값 = min(iterable[, default=obj]\[, key=값을변환할함수])

- iterable이 비었을 경우, default 값을 반환
- 디폴트값을 지정하지 않고 Iterable 객체가 비었을 경우, ValueError: max/min arg is an empty sequence 예외가 발생

```python
>>> max([-7, 1, 2, 3, 4])
4
>>> min([-7, 1, 2, 3, 4])
-7
>>> max([-7, 1, 2, 3, 4], key=lambda value: abs(value))
-7
>>> min([-7, 1, 2, 3, 4], key=lambda value: abs(value))
1

>>> max([])
---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
<ipython-input-15-a48d8f8c12de> in <module>()
----> 1 max([])

ValueError: max() arg is an empty sequence

>>> max([], default=0)
0
>>> max([], default='Empty')
'Empty'
>>> max([], default=None) # 아무값도 출력하지 않음

# 각종 예
>>> max([], defalut=0)
0
>>> max([1, 2, 3], default=0)
3
>>> max([1, 2, 3, 10], key=lambda value : value%10, default=0)
3
>>> max([-20, 1, 2, 3, 10], key=abs, default=0)
-20
>>> min([-20, 1, 2, 3, 10], key=abs, default=0)
1
```



> ## list의 sort 멤버함수

- sorted : 다양한 iterable 객체를 정렬한 새로운 리스트를 리턴
  - 원본 \<iterable 객체>의 순서는 변경하지 않음
- list는 자체적으로 sort함수를 지원
  - list 자체의 순서를 변경
  - sorted와 다르게 리턴값이 없습니다. 즉 None을 리턴
  - sorted와 마찬가지로 key에 임의 함수를 넣어 정렬 가능

```python
>>> mylist = [-4, -2, 0, 1, 3, 5]
>>> sorted(mylist, key=abs, reverse=False)
[0, 1, -2, 3, -4, 5] # 절대값에 따라 순서가 바뀐 것을 확인
>>> mylist
[-4, -2, 0, 1, 3, 5] # 원본 list에는 영향이 없음
>>> mylist.sort(key=abs, reverse=False)
>>> mylist
[0, 1, -2, 3, -4, 5] # 원본 list가 변경되는 것을 확인

# 임의함수를 멤버함수 sort에 인자로 넘겨 정렬
def tmp_fn(value):
	return value % 3
>>> mylist.sort(key=tmp_fn, reverse=False)
>>> mylist
[0, 3, 1, -2, -4, 5]
>>> mylist.sort(key=lambda value : value ** 3, reverse=False)
[-4, -2, 0, 1, 3, 5]
```

위와 같이 key의 return 값을 기준으로 비교를 통하여 정렬함을 알 수 있다. 그렇다면 과연 무엇이 대소 비교를 가능하게 하는 것일까?



> ## 파이썬의 대소비교

- 기본적인 정렬을 위해선 대소비교가 가능해야함
  - 대소비교가 불가능하다면, 정렬도 불가능

```python
>>> 1 < 2
True
>>> 'a' < 'b'
True
>>> 'd' > 'e'
False

# 다른 형식 간 비교
>>> 0 < 'a'
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-17-17cb7a6cc606> in <module>()
----> 1 0 < 'a'

TypeError: '<' not supported between instances of 'int' and 'str'
```

다른 형식 간 비교는 파이썬에선 지원하니 않음을 확인 가능하다.

- 한 문자끼리 비교는 각 문자에 매핑된 문자코드(ACSII)를 따라 비교
- 문자열끼리의 비교는 첫 번째 인덱스부터 차례대로 대소비교를 판단
- list/tuple끼리의 비교도 문자열과 동일

```python
>>> 'c' > 'd' # 'c'의 ASCII코드 = 99, 'd'의 ASCII코드 = 100
False
>>> 'abcd' < 'acegi' # 'a' vs 'a' 무승부 / 'b' vs 'c' 'c'가 큼 우측이 더 큼
True
>>> [8] < [0] # 첫 번째 인덱스 8 < 0 은 좌측이 큼
False
>>> [1, 2, 3, 4] < [1, 3, 5, 7] # 1 vs 1 무승부 / 2 vs 3 3이 더 크므로 우측이 큼
True
```



> ## QUIZ

- 다음을 기준으로 정렬해보시오.
  - 1차 기준 : 자리수(e.g. 11 은 2자리수, 111은 3자리수)
  - 2차 기준 : 1의 자리 숫자(e.g. 21은 1, 32는 2, 321은 1)

```python
def sort_fn(value):
    pass # 해당 함수 내부의 코드만을 작성하여 아래와 같이 실행시 결과가 나오도록 할 것

>>> mylist = [10, 11, 9, 20, 12, 313, 211, 121]
>>> mylist.sort(key=sort_fn)
>>> print(mylist)
[9, 10, 20, 11, 12, 211, 121, 313]
```



**풀이**

기준을 잘 생각해보면,

1. 1차기준에 따라 자리수가 큰 것이 더 큰 값으로 판단되어야 함.
2. 1차 기준에서 무승부가 나는 경우 2차기준에 따라 판단하며, 1의 자리수가 클수록 큰 값으로 판단.
3. 2차 기준은 1자리 숫자에서 벗어나지 않음
4. 1차 기준을 10의자리에 사용하여, 2차기준보다 기준 value에 크게 영향을 미치도록 하자.
5. 따라서, [2차기준식] \* 10 + [1차기준식] 의 형태를 반환하면 됨.
6. 기본적으로 자리수의 판단은 해당 정수형 자료를 문자열 자료형태로 변환하여, len함수를 통해 구할 수 있음.
7. [2차기준식]은 len(str(value))가 되며, [1차기준식]은 value % 10이 된다.
8. 5.의 항을 다시 정리하면 (len(str(value))) \* 10 + (value % 10) 이다.

```python
def sort_fn(value):
    return len(str(value)) * 10 + (value % 10)

>>> mylist = [10, 11, 9, 20, 12, 313, 211, 121]
>>> mylist.sort(key=sort_fn)
>>> print(mylist)
[9, 10, 20, 11, 12, 211, 121, 313]
```



이번엔 여기까지 : )
