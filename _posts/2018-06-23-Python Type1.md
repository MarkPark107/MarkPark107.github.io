---
layout: post
title: "파이썬의 자료형"
subtitle: "파이썬의 변수, int float str"
categories: devlog
tags: Python

---





> ## 파이썬의 변수(Variables)

자료형을 알기전에 일단 파이썬에서 말하는 변수가 무엇인지 알 필요가 있다.

~~물론 난 안다. 변수가 다 거기서 거기지 근데 정리 겸~~

변수란 어떠한 데이터를 담아두는 그릇으로 생각하는게 편하다. 물론 초기의 개념 이해를 위해서이다. 

*즉 정확한 이해는 아니란 소리*

조금 더 심도있게 나름의(주관적) 정의를 해보자면,

특정 규칙에 따라 할당된 메모리 공간의 주소를 쉽게 말하기 위한 별칭이라고 볼 수 있을 것 같다.

특정 규칙이라고 말했지만, 이는 각 **자료형 별 필요한 메모리 크기나, 저장 방식이 다른 것**을 말한다.

컴퓨터 자원은 유한하기에 이걸 얼마나 효율적으로 잘 쓰느냐가 중요하며, 이를 위한 적당한 크기와 적절한 변수 선언이 중요한 것.

*여하튼 변수의 개념 설명은 이정도로 하자. 산으로 갈 우려가 있다.*



> ## 데이터 타입(Data Type)

쉽게 자료형 또는 데이터 타입이라고 불리는데, 파이썬에서는 현재 int, float, str의 기본 자료형을 제공한다. 이 밖에 list, tuple, set, dict 등이 존재한다. 하나하나 파헤쳐볼까?



> #### 숫자 형식(Numeric Type)

숫자의 형식에는 두가지가 존재하는데, int와 float이다. 사실 파이썬 2에선 int, long, float, double의 네가지 숫자 형식이 존재했지만, 이제는 `int = int | long` 이고,  `float = float | double`으로 두가지로 사용된다.

**정수형 : int**

**실수형 : float**

이며, 모두 사칙연산 (+, -, *, /)를 지원하고, 다른 언어와의 차이점은 나눗셈은 정말 사칙연산의 나눗셈인 점이 다르다.(일반적으로 다른 언어에서 나눗셈 / 은 몫을 의미하는 경우가 많다.) 또한 몫(//), 나머지(%), 지수 승(***) 연산자가 존재한다.

```python
10 + 3 # 3
10 - 3 # 7
10 * 3 # 30
10 / 3 # 3.333333
10 // 3 # 3
10 % 3 # 1
10 ** 3 # 1000
```



> ## 참/거짓(Boolean Type)

참 거짓에 관한 자료형도 존재하는데 참은 True, 거짓은 False로 표현하며, 그 밖에 비교/논리 연산자들에 의한 결과들도 참과 거짓으로 나타난다.

**비교 연산자 : <(작다), <=(작거나 같다), ==(같다), >=(크거나 같다), >(크다), !=(같지 않다)**

**조건 연산자 : and(그리고의 의미로 모두 참이어야 참), or(또는의 의미로 하나라도 참이면 참), not(부정의 의미로 ~~가 아니어야 참)**

이에 관련하여 재미있는 예제가 있는데 파이썬에서 비교를 예로 든다.

```python
>>> a = []
>>> b = []
>>> id(a), id(b)
4377709832, 4377945992
>>> a == b
True
>>> a is b
False
```

라는 부분이다. 이 예제를 통해 파이썬에는 id라는 값이 존재한다는 것을 알 수 있고 ==는 값을 비교하는 연산, is는 실제로 그들이 하나도 다름 없이 동일한 것인가?(id가 같은가?)라는 질문인 것을 파악 할 수 있다. 무슨 말이냐면,

A와 B가 있는데 둘의 키를 A, B라고 하자. 둘의 키가 180으로 동일할 때,

A == B 연산은 (A의 키 값)이 (B의 키 값)과 같은가? 를 묻는다면

A is B 는 (A)가 (B)인가? 라는 질문과 같다.

즉 같은 값을 가지기에 A==B에서는 참(True)를 반환하지만, A is B 에서는 False를 반환한다.

여기까지 보면, id는 고유 값이라고 생각할 수 있는데, 실제적으로 파이썬의 id는 고유 값은 아니었다.



예제를 보던 중

```python
>>> id(a), id(b), id(c)
4407740744, 4408686024, 4408686024
```

가 나오고

```python
>>>b is c
True
```

인 것을 보고 ~~충격. . .~~ 도대체 id는 뭔가. . .

나중에 알아봐야 할 듯 하다.



이 밖에 Boolean Type이 아닌데, 참과 거짓으로 쓰일 수 있는 다른 타입의 조건들이 있는데,

- 숫자 0은 False, 나머지는 True
- 빈 문자열은 False, 나머지는 True
- 빈 list, tuple, set, dict은 False, 나머지는 True

```python
>>> bool(0), bool(1), bool(-1)
False, True, True
>>> bool(''), bool(' '), bool('a')
False, True, True
>>> bool([]), bool(()), bool(set()), bool({}), bool([' '])
False, False, False, False, True
```



> ## 문자열(String Type)

문자열은 홑따옴표(')로 감싸거나, 쌍따옴포(")로 감싸서 표현

```python
name1 = 'Python'
name2 = "Python"
```

만약 홑(쌍)따옴표 1개로 감싼 문자열 안에서 홑(쌍)따옴표를 사용해야 하는 경우 ESCAPE 문자(`\`)를 이용하여 문자열을 정상적으로 처리할 수 있다.

```python
str1 = 'I'm Mark' #잘못된 처리, 문자열이 "I" 이며, 나머지는 코드로써 "m Mark"로 됨. Error
str2 = 'I\'m Mark' #올바른 처리
```

여러 줄 문자열 문법의 표현 방식은 홑(쌍)따옴표 3개로 감싸서 표현하며, 개행문자`\n`를 일일히 삽입하지 않고 Enter로써 표현 가능.

```python
#일반 문자열의 Enter 사용 Error
>>> lyrics = 'The snow glows white on the mountain tonight
Now a footprint to be seen
A kingdom of isolation and it looks like I`m the queen'
  File "<ipython-input-1-c6743f6c55e2>", line 1
    lyrics = 'The snow glows white on the mountain tonight
                                                          ^
SyntaxError: EOL while scanning string literal

#일반 문자열의 \n 사용 >> 일반 출력시 개행문자가 나옴, print 사용시 줄 바뀜
>>> lyrics = 'The snow glows white on the mountain tonight\nNow a footprint to be seen\nA kingdom of isolation and it looks like I`m the queen'
>>> lyrics
'The snow glows white on the mountain tonight\nNow a footprint to be seen\nA kingdom of isolation and it looks like I`m the queen'
>>> print(lyrics)
The snow glows white on the mountain tonight
Now a footprint to be seen
A kingdom of isolation and it looks like I`m the queen

#여러줄 문법이지만 Enter 미사용. >> 일반 출력과 print 모두 한줄로 나옴
>>> lyrics = '''The snow glows white on the mountain tonight Now a footprint to be seen A kingdom of isolation and it looks like I`m the queen'''
>>> lyrics
'The snow glows white on the mountain tonight Now a footprint to be seen A kingdom of isolation and it looks like I`m the queen'
>>> print(lyrics)
The snow glows white on the mountain tonight Now a footprint to be seen A kingdom of isolation and it looks like I`m the queen

#여러줄 문법에 Enter 사용 >> 일반 출력시 개행문자가 나옴, print 사용시 줄 바뀜
>>> lyrics = '''The snow glows white on the mountain tonight
Now a footprint to be seen
A kingdom of isolation and it looks like I`m the queen'''
>>> lyrics
'The snow glows white on the mountain tonight\nNow a footprint to be seen\nA kingdom of isolation and it looks like I`m the queen'
>>> print(lyrics)
The snow glows white on the mountain tonight
Now a footprint to be seen
A kingdom of isolation and it looks like I`m the queen
```



문자열 형식 지정자라는 친구가 있다.

문자열 내에 "{}"를 이용하여 슬롯을 만들고, format 함수를 통해 슬롯에 필요한 데이터를 치환 입력하는  방식이다.

format함수에서 함수인자로서 슬롯을 지정하는 방법에는 두가지가 있는데

1. 위치(Positional)
2. 키워드(Keyword)

가 있다.

위치는,  슬롯의 위치를 인덱스화하여, 포멧 함수의 함수인자들을 매칭시켜 전달하는 방식인데, 예를 보는 것이 빠르다.

```python
>>> print('{0}, {1}, {2}'.format('a', 'b', 'c')) # 0은 첫 번째인 a, 1은 b, 2는 c에 매칭
a, b, c
>>> print('{}, {}, {}'.format('a', 'b', 'c')) # 인덱스가 없으면 0부터 순서대로 배치
a, b, c
>>> print('{2}, {1}, {0}'.format('a', 'b', 'c')) # 2에 매칭되는 c부터 들어가겠지?
c, b, a
>>> print('{0}, {1}, {2}'.format(*'abc')) #unpacking argument sequence 라고 하는데, 알아서 잘 넣어준다. 자세히는 모르겠다
c, b, a
>>> print('{1}, {2}, {0}'.format(*'abc')) #요런 느낌?
b, c, a
>>> print('{0}, {1}, {0}'.format('a', 'b')) # 전달되는 인자는 반복 사용이 가능하다.
a, b, a
```



키워드를 이용하는 방식은

```python
>>> print('Coordinates: {lat}, {lng}'.format(lat='37.24N', lng='-115.81W'))
Coordinates: 37.24N, -115.81W

>>> coord = {'lat': '37.24N', 'lng': '-115.81W'} # dict 사용
>>> print('Coordinates: {lat}, {lng}'.format(**coord))
Coordinates: 37.24N, -115.81W

# 호기심에 해보는 coord의 앞의 별을 하나 뺴면? **coord -> *coord
>>> coord = {'lat': '37.24N', 'lng': '-115.81W'} # dict 사용
>>> print('Coordinates: {lat}, {lng}'.format(*coord))
KeyError                                  Traceback (most recent call last)
<ipython-input-14-2ed9deadbf9e> in <module>()
----> 1 print('Coordinates: {lat}, {lng}'.format(*coord))

KeyError: 'lat' #Error가 난다. 아마 포인터 형식의 자료 구조를 표현한 방식이 아닌가 싶다.
```



> ## NameError

정의되지 않은 변수에 접근 시에 발생. alpha를 정의하지 않고 사용하는 아래 코드를 보자.

```python
>>> print(alpha)
NameError                                 Traceback (most recent call last)
<ipython-input-15-7b5db3b01685> in <module>()
----> 1 print(alpha)

NameError: name 'alpha' is not defined
```

alpha가 정의되지 않았음을 알려주는 에러가 뜬다. 이와 관련해 재밌는 코드를 테스트하기에 적는다.

```python
# 당연히 False이기 떄문에 alpha는 정의되지 않고 alpha를 부르기에 에러가 난다.
>>> if False
>>> 	alpha = 10
>>> alpha
NameError                                 Traceback (most recent call last)
<ipython-input-15-7b5db3b01685> in <module>()
----> 1 print(alpha)

NameError: name 'alpha' is not defined
# 이제 정의하며 내려가면 에러가 나지 않는다.
>>> if True
>>> 	alpha = 11
>>> alpha
11
```

~~나름 재밌는 코드라 생각했는데, 지금보니 섭섭하게 재미없어보이기도 한다.~~



오늘은 여기까지 : )

