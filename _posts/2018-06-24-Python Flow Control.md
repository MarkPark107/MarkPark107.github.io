---
layout: post
title: "파이썬의 흐름제어"
subtitle: "파이썬, if, for"
categories: devlog
tags: Python
---





> ## 파이썬, if

다른 프로그래밍 언어에서도 마찬가지로 본격적으로 무언가를 해보기 위해선 if라는 조건문이 필요하다.*이 시점이 사실 프로그래밍이 즐거우 지는 순간*

파이썬도 마찬가지로 if문을 지원하는데, 형식이 다른것보다 특이하다. ~~물론 구조적관점에선 그 나물에 그 밥~~

```python
if 조건 1:
    위 조건에 만족할 때 실행할 코드 블럭
elif 조건 2: # 다른 프로그래밍 언어의 else if 구문
    조건 1을 만족하지 않았지만 조건 2를 만족한 경우 실행할 코드 블럭
elif 조건 3:
    조건 1~2를 만족하지 않았지만 조건 3을 만족한 경우 실행할 코드 블럭
elif 조건 4:
    조건 1~3을 만족하지 않았지만 조건 4를 만족할 경우 실행할 코드 블럭
else # 모든 if문이 그렇듯 하나의if당 else는 한 번뿐이다.
   위의 모든 조건(조건 1~4)를 만족하지 않은 경우 실행할 코드 블럭
```



예로 입력된 정수 값을 음수와 양수, 0로 구분하는 파이썬 코드를 작성해보자

```python
# 일단 분류할 '정수' 데이터부터 받아야하니까 강제 정수로 입력 값을 받고(예외 처리 귀찮)
number = int(input('정수 값을 넣으세요. 어차피 정수아님 오류 뿜을거야 : '))
# 입력받은 값이 있는 number를 구분 하려면 조건을 걸어서 분류해줘야하니까 if
if number > 0: # 양수이면
    print('처리된 정수 %d 는 양수입니다.' % number)
elif number < 0: # 음수이면
    print('처리된 정수 %d 는 음수입니다.' % number)
else: # 양수도 음수도 아닌 정수면 0겠지
   print('처리된 정수 %d 는 0 입니다.' % number) # 사실 0인게 명확하면 number를 넣을 필요도 없음
```



> ## 파이썬, 반복문, for

Iterable Object 로 부터 가져올 값들을 모두 가져올 때까지 반복

```python
for 변수 in 리스트:
    반복 수행할 코드 블록

>>> for i in range(20):
>>>     print(i)
>>>     if 10 > break;
0
1
2
3
4
5
6
7
8
9
10
11
```

#### 중첩 반복문

간단하게 말해서 반복문을 중첩하여 사용하는 것이다.

```python
#아래와 같은 경우가 중첩 반복문이다.
>>> for i in range(2, 5):
    	for j in range(1, 4):
            print(i, j)
2 1
2 2
2 3
2 4
3 1
3 2
3 3
3 4
4 1
4 2
4 3
4 4
5 1
5 2
5 3
5 4
```

#### break?

반복문을 상용하다보면, 특정 상황에선 반복을 종료하고, 코드 블록을 빠져나와야 하는 경우가 생긴다.

이런 경우에 break를 통하여 **인접한 코드 블록**을 빠져나올 수 있다.

```python
# 중첩 반복문에서 break를 이용하여 인접한 코드 블록 빠져나오기
>>> for i in range(2, 5):
		for j in range(1, 4):
            print(i, j)
            break
2 1
3 1
4 1
5 1
```

위 의 예제를 보면, 두 번째 반복문 내에서 break를 만나 인접 반복문인 두 번째 반복문을 빠져나오는 것을 볼 수 있다. 조금 더 이해를 돕기 위해 하나의 예제를 추가적으로 보자면

```python
# 중첩 반복문에서 break를 사용한 예제2
>>> for i in range(2, 5):
    	for j in range (1, 4):
            print(i, j)
            if j > 2
            	break
2 1
2 2
3 1
3 2
4 1
4 2
5 1
5 2
```

가 된다.  반복문을 한번에 종료시키는 방법으로 return을 알려주던데, 사실 return은 함수의 값을 반환하는 것으로 함수 자체를 종료시킨다는 의미이다. 따라서 **메인 프로그램 상에서 중첩 반복문을 종료시키겠다고 return을 쓰면 프로그램이 종료될 수 있으니 주의**

해당 예제는 다음과 같다.

```python
>>> def gugu():
    	for i in range(2, 10):
            for j in range(1, 10):
                print(i, j)
                return None # None은 값 없다는 것을 의미, 즉 리턴값은 없음
>>> gugu()
2 1
```



> ## Range

지금까지 반복문을 보면 range라는 함수가 계속 등장한다.

- range(stop) : 0부터 stop 미만의 범위에서 1씩 증가시킨 값으로 순회가능한 객체를 구성해주는 함수
  - 문법적으로는 리스트가 아닌, 순회가능한(Iterable) 객체이다.
- range(start, stop[, step]) : start 값 이상, stop 미만의 범위에서 step 씩 증가시킨 순회가능한 객체를 구성해주는 함수

```python
>>> range(10)
range(0, 10)

>>> list(range(10))
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

>>>tuple(range(10))
(0, 1, 2, 3, 4, 5, 6, 7, 8, 9)
```



> ## 파이썬, 반복문, while

다른 프로그래밍 언어와 마찬가지로 파이썬에도 while이 있다.

```python
while 조건:
    조건이 만족할 경우 수행할 코드 블록
```

으로 이루어지며, 마찬가지로 break를 통하여 while문을 빠져나올 수 있다.

```python
i = 0
>>> while i < 20:
    print(i)
    if i > 10:
        break
    i += 1
```



> ## 무한루프

쉽게 말해 빠져나올 수 없는 반복문을 말한다.

- 반복문의 조건이 항상 True일 때
- itertools.count를 이용하여

만들 수 있다.

```python
# 반복문의 조건이 항상 True일 때
>>> i = 10
    while i < 13:
    	print i
    	i -= 1

# itertools.count를 이용
from itertools import count # itertools에서 count를 import
for i in count(1):
    print(i)
```

와 같이 만들 수 있다.



연습문제로 2단, 4단, 6단, 8단 구구단을 출력하는 코드를 작성하라는데, 구조적 설계부터 차근차근해보자.

조건은 간단하다.

- 2부터 시작되여 8에서 종료되며, step이 2개씩 올라간다.
- 구구단이므로, 하나의 단에는 1부터 9 까지 반복되며, 이를 출력해야 한다.
- 출력 조건(단은 가로로 출력해야한다 등)은 없으며, 형식 또한 없지만 a x b = c 의 형식으로 보기좋게 해보자.

```python
# Easy
for i in range(2, 9, 2):
    for j in range (1, 10):
        print('{} X {} = {}'.format(i, j, i * j))
```



이번엔 여기까지 : )
