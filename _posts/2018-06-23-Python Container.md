---
layout: post
title: "파이썬의 기본 자료 구조"
subtitle: "Python Container"
categories: devlog
tags: python


---





> ## 파이썬의 기본 자료 구조(Container)

파이썬에서는 자료(데이터)의 관리를 쉽게 하기 위해 여러 원소들을 가지고 있는 몇 가지 자료구조를 제공한다. 이들을 Container라고 분류하며, Container들은

- **list**, deque
- **set**, frozensets
- **dict**, defaultdict, OrderedDict, Counter
- **tuple**, namedtuple
- \*\*str

이 있다. 이들은 in 연산자로 멤버쉽 테스트를 할 수 있는데 예를 들면

```python
>>> 'hello' in 'hello world'
True
```

이며, 컨테이너 안에 해당 멤버가 있는지 검사해주는 기능이다.



> ## list

- 생성 문법 : [], list(), list(iterable)
- 여러값을 순차적으로 저장하며, 순서를 보장
- 리스트를 한 줄로 쓸 때에는 대개 끝에 쉼표를 쓰지 않음
- 여러줄로 나눠 쓸 때에는 끝에 쉼표를 쓴다. 이유는 항목의 추가/삭제에서 용이하기 때문

```python
>>> numbers = [1, 3, 5, 7, 9]
>>> names = [
    'Sed',
    'Fra',
    'Ryan',
    'Mark',
]

>>> 2 in numbers
False
>>> 3 in numbers
True
```

처럼 사용한다.

또한, 색인(Index)를 지원하는데 맨 처음 멤버에 0가 매칭되며 1씩 증가한다.

재미있는 점은 음수 색인을 지원한다는 점인데, 끝 멤버에 -1이 매칭되며 -1씩 감소하며 인덱싱된다.

더욱 재밌는 건 한 리스트에 다양한 데이터 타입의 값을 포함 시킬 수 있다는점!

~~근데 혼자쓸거 아니면 그러지 말자. . .~~

리스트의 출력

```python
>>> numbers = [1, 3, 5, 7, 9]
>>> for i in numbers
>>> 	print(i)
1
3
5
7
9
# print는 자동으로 개행함을 알 수 있다
```

*강의 중간에 len()의 설명이 나오는데, length로 인자로 전달된 리스트의 길이(멤버수)를 반환한다.*



리스트의 데이터 변경

```python
>>> numbers = [1, 3, 5, 7, 9] # [1, 3, 5, 7, 5]
>>> numbers[0] = 10 # 인덱스 0의 데이터 변경, [10, 3, 5, 7, 5]
>>> numbers.append(9) # numbers 리스트의 끝에 9 삽입, [10, 3, 5, 7, 5, 9]
>>> numbers.pop(3) # 특정 인덱스의 값(3)을 가져옴과 동시에 제거, [10, 3, 5, 5, 9]
>>> numbers.remove(5) # 특정 값(5)을 한 번 제거, [10, 3, 5, 9]
>>> numbers.insert(1, 11) # 특정 인덱스(1)에 색인값(11) 삽입, [10, 11, 3, 5, 9]
```



값 잘라내기(Slice)

```python
>>> numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
>>> print(numbers[1:]) # 인덱스 1부터
[2, 3, 4, 5, 6, 7, 8, 9, 10]
>>> print(numbers[1:7]) # 인덱스 1부터 7 미만까지
[2, 3, 4, 5, 6, 7]
>>> print(numbers[1:7:2]) # 인덱스 1부터 7 미만까지 2씩 증가시키며
[2, 4, 6]
>>> print(numbers[::-1]) # 전체 인덱스를 -1씩 증가시키며 = Reverse
```



리스트 합치기

```python
>>> number1 = [1, 3, 5, 7]
>>> number2 = [2, 4, 6, 8]
>>> print(number1, number2)
[1, 3, 5, 7, 2, 4, 6, 8]
```



List Comprehension

```python
>>> number1 = [1, 3, 5, 7]
>>> number2 = [2, 4, 6, 8]
>>> print([i + j for (i, j) in zip(number1, number2)]) # number1, number2의 zip을 순회하는 i, j를 더한 값을 리스트로써 출력하라
[3, 7, 11, 15]
```



> ## 튜플(Tuple)

- 생성문법 : (), tuple(), tuple(iterable)
- list와 유사하지만 변경 불가능(Immutable)

```python
>>> numbers = (1, 3, 5, 7, 9)
>>> numbers[0] = 10
TypeError                                 Traceback (most recent call last)
<ipython-input-20-c7c74a272490> in <module>()
      1 numbers = (1, 3, 5, 7, 9)
----> 2 numbers[0] = 10

TypeError: 'tuple' object does not support item assignment

>>> numbers.append(9)
AttributeError                            Traceback (most recent call last)
<ipython-input-21-676288dec4f2> in <module>()
----> 1 numbers.append(9)

AttributeError: 'tuple' object has no attribute 'append'
```

**Tuple이용시 주의사항**

소괄호()는 우선순위 연산자 혹은 튜플로 쓰이기에, 각 상황에 소괄호가 무엇으로 해석될지 주의해야한다. **자신없으면 리스트 쓰자**

```python
>>> tuple1 = (1 + 3) # 우선순위, 개수가 1개일 때, 콤마를 생략하면 tuple이 아니라 우선순위
>>> tuple2 = (1 + 3,) # tuple
>>> tuple3 = (3) # 우선순위
>>> tuple4 = (3,) # tuple

#리스트는 어떨까?
>>> list1 = [1 + 3] # list 개수나 뭐나 상관없이 다 리스트!
>>> list2 = [1 + 3,] # list
>>> list3 = [3] # list
>>> list4 = [3,] # list
```



> ## Packing / Unpacking

파이썬에는 패킹과 언패킹이라는 것이 존재한다. list/tuple에 동일하게 적용되며, 주의할 점은 **unpacking시 개수가 맞지 않으면 ValueError가 난다.**

```python
>>> numbers = (1, 2, 3, 4, 5, 6, 7, 8, 9, 10) # packing : 다수의 값을 하나의 변수(numbers)에 담음
>>> v1, v2, v3, v4, v5, v6, v7, v8, v9, v10 = numbers # unpacking : 하나의 값을 다수의 변수(v1, v2, ... , v10)에 나눠 담음. 개수가 맞는지 중요!

>>> v1, v2, v3, v4 = numbers[:4] # unpacking : 슬라이스를 이용, numbers[:4] : [1, 2, 3, 4], v1 = 1, v2 = 2, v3, 3, v4 = 4
>>> v1, v2, v3, v4, *others = numbers # unpacking : *others를 이용, v1 = 1, v2 = 2, v3 = 3, v4 = 4, *ohters = [5, 6, 7, 8, 9, 10]

>>> *others, v8, v9, v10 = numbers # unpacking : *others = [1, 2, 3, 4, 5, 6, 7], v8 = 8, v9 = 9, v10 = 10
>>> v1, v2, *others, v9, v10 = numbers # unpacking : v1 = 1, v2 = 2, *others = [3, 4, 5, 6, 7, 8], v9 = 9, v10 = 10
>>> v1, v2, v3, *others = numbers # unpacking : v1 = 1, v2 = 2, v3 = 3, *others = [4, 5, 6, 7, 8, 9, 10]

>>> v1, v2, v3, v4, v5, v6, v7, v8, v11, v12, v13, v14 = (*numbers, 11, 12, 13, 14) # unpacking with additional values
```



> ## 스왑(Swap)

파이썬의 신박함과 심플함 간결함 배려심에 놀랄 시간이다.

```c
// C의 스왑예제
int a = 1;
int b = 2;
int tmp;
print("%d, %d", a, b);
>> 1, 2
// C의 스왑코드
tmp = a;
a = b;
b = tmp;
print("%d, %d", a, b);
>> 2, 1
```

이런 코드를 많이 써보았을건데, 갓이썬님은 이런거 필요 없다고 하신다.

```python
>>> x, y = 1, 2
>>> print(x, y)
1, 2
>>> x, y = y, x # 파이썬의 스왑 코드
>>> print(x, y)
2, 1
```

고생 끝 해피 시작이다.  list/tuple에도 동일하게 적용된다. ~~외쳐 갓이썬~~



> ## 집합(set)

**중복을 허용하지 않는다.** 이거면 충분하지 않나? 리스트나 튜플에서 중복을 제거하여 사용하고자 할 때 유용하다. **다만, 순서를 유지해주지 않는다**

```python
>>> set_numbers = {1, 3, 4, 5, 1, 4, 3, 1}
>>> set_numbers
{1, 3, 4, 5}

>>> list_numbers = [1, 1, 2, 1, 1, 2, 2, 3, 1, 2, 3]
>>> list_numbers = list(set(list_numbers))
>>> list_numbers
[1, 2, 3]
```

합집합, 교집합, 차집합, 여집합을 지원하며, 예제를 통해 알아보자.

```python
>>> set_numbers1 = {1, 3, 4, 5, 1, 4, 3, 1}
>>> set_numbers2 = {1, 3, 4, 11, 13, 14, 15, 11, 14, 13, 11}
>>> set_numbers1, set_numbers2
{1, 3, 4, 5}, {1, 3, 4, 11, 13, 14, 15}

>>> print(set_numbers1 - set_numbers2) # 차집합
{5}
>>> print(set_numbers1 | set_numbers2) # 합집합
{1, 3, 4, 5, 11, 13, 14, 15}
>>> print(set_numbers1 & set_numbers2) # 교집합
{1, 3, 4}
>>> print(set_numbers1 ^ set_numbers2) # 여집합
{5, 11, 13, 14, 15}
```



> ## 사전형(dict)

- Key와 Value의 쌍으로 구성된 집합
- Key의 중복을 허용하지 않음
- 중괄호 내에 콜론으로 Key: Value를 구분
- in 연산자로 key의 멤버쉽 체크 지원
- 순회 시에는 key목록만 순회
- 멤버함수
  - .keys() : key 목록
  - .values() : value 목록
  - .items() : (key, value) 목록

```python
>>> dict_values = {'blue': 10, 'yellow': 3, 'blue': 9, 'red': 7} # Key는 중복되지 않기에 뒤의 blue값인 9로 바뀜
>>> 10 in dict_values
False
>>> 'blue' in dict_values
True

#순회
>>> for key in dict_values: # Key 순회
    	print(key)
yellow
blue
red
>>> for key in dict_values.keys(): # Key 순회
    	print(key)
yellow
blue
red

>>> for value in dict_values.values(): # Value 순회
    	print(value)
3
9
7

>>> for (key, value) in dict_values.items(): # Item(Key&Value) 순회
    	print(key, value)
yellow 3
blue 9
red 7

#조회
>>> print(dic_values['blue']) # 특정 Key(blue)의 Value를 조회
9
>>> print(dic_values['white']) # 등록되지 않은 Key(white)를 조회하면 KeyError
KeyError                                  Traceback (most recent call last)
<ipython-input-25-b5ff6a75f1c3> in <module>()
----> 1 print(dict_values['white'])

KeyError: 'white'

#Key의 Value 변경 및 새 Key&Value 추가
>>> dict_values['blue'] = 20 #Key 'blue'의 값을 20으로 변경
>>> print(dict_values['blue'])
20
>>> dict_values['black'] = 30 #Key는 'black'이고 Value는 30인 값을 추가
>>> print(dict_values['black'])
30

#Key&Value 삭제
>>> del dict_values['black'] #Key 'black'인 Key&Value 삭제
>>> del dict_values['green'] #등록되지 않은 Key 'green'을 삭제하려고 하면 KeyError
KeyError                                  Traceback (most recent call last)
<ipython-input-26-12c01675e8c3> in <module>()
----> 1 del dict_values['green']

KeyError: 'green'
```



이번엔 여기까지 : )
