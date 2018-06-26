---
layout: post
title: "파이썬과 호출가능한 객체"
subtitle: "Python, Object that is able to be called"
categories: devlog
tags: python


---

> ## 파이썬과 호출가능한 객체

호출문법 : obj()

- 함수를 호출해서, 리턴값을 취한다

```python
>>> def mysum(x, y):
    	return x + y
>>> mysum(1, 2)
3
```

- 클래스를 호출해서, 인스턴스를 생성한다.
  - 인스턴스를 호출 가능토록 만들려면 \_\_call\_\_ 함수를 구현해주어야 함
    - 인스턴스를 호출하면, 파이썬에서 멤버함수 \_\_call\_\_ 를 호출해줌

```python
class Calculator(object):
    def __init__(self, base):
        self.base = base
    def mysum(self, x, y):
        return self.base + x + y

>>> calc_10 = Calculator(10)
>>> calc_10.mysum(1, 2)
13

# call 함수를 활용하여 계산하기
class Calculator2(object):
    def __init__(self, base):
        self.base = base
    def __call__(self, x, y):
        return self.base + x + y

>>> calc2_20 = Calculator2(20)
>>> calc2_20.__call__(1, 3)
24
>>> calc2_20.__call__(2, 4)
26
>>> calc2_20(4, 5) # __call__을 호출해 줌
29
```



> ## 상속을 이용하여, 개발 생산성 향상하기

똑같은 코드를 대부분 중복하여 사용하지만, 약간의 기능만이 다른 경우, 일반적으로 함수로 구현하게되면, 모든 코드를 다시 써야하는 문제가 발생한다. 이를 객체지향 프로그래밍에서는 상속을 통하여 코드 중복을 최소화하고, 오버라이딩을 이용하여, 약간의 기능수정을 쉽게 할 수 있도록 지원한다.

```python
# 파이썬을 이용하여 특정 url의 문자열을 수집하는 클래스
import requests
from bs4 import BeautifulSoup
from collections import Counter

class WordCount(object):
    def get_text(self, url):
        '문자열 수집'
        html = requests.get(url).text
        soup = BeautifulSoup(html, 'html.parser')
        return soup.text
    def get_list(self, text):
        '단어 리스트로 반환'
        return text.split()
    def __call__(self, url):
        text = self.get_text(url)
        words = self.get_list(text)
        counter = Counter(words)
        return counter

# 위의 클래스를 상속받아 한국어 단어수만 세는 클래스
import re

class KoreanWordCount(WordCount):
    def get_list(self, text):
        '한국어만 리스트로 반환'
        words = text.split()
        return [word for word in words if re.match(r'^[ㄱ-힣]+$', word)]
```



이번엔 여기까지 : )
