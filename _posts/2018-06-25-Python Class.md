---
layout: post
title: "파이썬과 클래스(Class)"
subtitle: "Python, Class, Object, Instance"
categories: devlog
tags: python

---

> ## 파이썬과 클래스(Class), 그 전에

파이썬을 알기 전에 사실, 절차지향적 언어에서 객체지향적 언어로 프로그래밍 언어 구조가 바뀐 것을 확실히 이해해야 한다. 많은 사람이 객체에 대한 이해를 매우 어려워하는 사실이 놀라웠다. 

기술의 이해는 모두 인간의 욕심, 욕망, 이상에서 찾아야 한다.

인간의 욕심과 욕망, 이상을 이루기 위해 인간은 무언갈 만들어 내고, 이것이 도구나 기술이라고 불리게 된다.

마찬가지로 프로그래밍 또한, 이 축에서 벗어나지 않는다. 초창기 프로그램이란, 인간이 계산해야 할 수학 문제를 대신 풀게끔 하는 것이 목적이었다. *처음 컴퓨터는 계산기로부터 시작했듯*

계산을 맡기는 것이 어느정도 정착되고 쉬워지자. 자신이 원하는 기능을 만들어 내길 원했고, 만들었으며, 이는 OS, GUI기반 OS, 각종 어플리케이션들(=프로그램들, Excel, Photoshop, Games)로 점차 진화했다.

이와 마찬가지로, 단순 계산 및 기능의 구현을 하기위해 절차지향적 언어(예, C언어)였던 것이 프로그램 구현 상의 편의 혹은 적합성을 위해서 진화한다. 무엇을 목적으로 진화했냐고? 대부분의 기능을 프로그램을 만들 수 있었던 사람들은 조금 더 복잡하고 다양한 것을 표현하길 원했다.

예를 들면, 현실 세계를 반영한 세계라던가. 현실의 무언가를 가상에서 똑같이 만들어내고, 실험하고 싶어하기 시작했다. 그래서 각각을 모두 객체(Object)화 시킨 접근 방식에 도달하게 된다. 이게 객체지향형 프로그래밍(Object-Oriented Programing)이 된다.

객체 각각이 기능과 데이터를 가지고 있으며, 이를 가지고 다양한 것을 표현하고, 객체 간 상호작용을 통해 어떤 결과를 도출해내는 방식이다. 조금 더 유기적이고 살아있는 듯한 느낌이지 않나 싶다. ~~물론 객체지향프로그래밍은 문제점이 있다. 프로그램이 복잡해지고 다양한 기능을 지원함에 따라, 거대한 수의 객체들이 생성되며, 각 객체의 state정보와 함수를 처리할 연산량이 어마어마해진다.~~

그래서 객체를 설명해줄 때, '이 세상 모든 것을 객체로써 표현할 수 있다.'라고 말해주는 편이다. 간단한 예로 문고리를 들자면, 문고리의 설계도는 클래스(Class)가 되고, 이를 바탕으로 만든 문고리는 문고리 설계도(클래스)로 만든 인스턴스(Instance)이며, 문고리 자체는 객체(Object)가 된다. 문고리 내부에는 문고리를 만들 때 쓰인 여러 부품들(Instance들)이 존재할 수 있고, 문고리에 새겨진 생산 넘버는 문고리 객체 안에 state로써 저장할 수 있다. 그리고 문고리가 동작하는 방식을 기능적으로 정의한 것이 해당 클래스의 함수(Function)으로써 정의되어 있을 것이다. 문고리를 돌릴 때 어떤 기능이 이루어지는지, 문고리의 잠금잠치를 눌렀을 때는 어떤 기능이 이루어지는지 등을 코드로써 정의한 것이 문고리의 기능이 된다.



서론이 길었다. 본론으로 가자.

> ## 파이썬과 OOP

파이썬은 OOP(객체지향적 프로그래밍, Object-Oriented Programing) 언어이다. 따라서 모든 OOP의 특징을 가지고 있다.

**OOP의 특징**

- Encapsulation(캡슐화) : 관련 특성/기능을 하나의 클래스에 결합
- Inheritance(상속) : 코드 재활용성이 증대됨
  - 부모 클래스의 특성/기능을 자식 클래스가 물려받음
  - 자식 클래스는 물려받은 특성/기능을 활용/변경/무시/재정의 가능
- Polymorphism(다형성) : 같은 함수명을 가지고 있으나, 다른 기능을 할 수 있음.



> ## 클래스(Class)

파이썬에서의 클래스 또한 다른 프로그래밍 언어의 클래스와 다르지 않다.

객체에 필요한 각종 변수들과 함수들로 이루어져 있다.

정의 방식만 확인하면 될 듯 하다.

**정의 방식에서 가장 중요한 점은 언더바(\_)를 쓰지 않으며, 처음 문자를 대문자로 쓴다는 점**

**파이썬에서 함수는 snake_case, 클래스는 CamelCase로 쓰자는 말**

*물론 그렇게 안한다고 큰 문제가 생기진 않지만, 혼자쓸거 아니면, 대중성에 맞춰주자.*

```python
>>> class circle: # 처음 문자는 대문자라고! 이렇게 쓰지말자.
    	pass

>>> class favorite_article: # 처음 문자도 대문자가 아니고, 언더바. . . 이렇게 쓰지말자.
    	pass

>>> class Circle: # 굳굳
    	pass

>>> class FavoriteArticle: # 굳굳
    	pass
```

이정도면 대충 클래스 제목을 정하는덴 문제가 없을 듯 하다.

파이썬3로 넘어오면서 바뀐 점이 있는데, 이는 기본적인 상속을 안해도 된다는 점이다.

```python
# Python2에서는 object를 상속받아야 New-style Class
>>> class Python2OldStyleClass: # 이러면 New-style Class가 아니야
    	pass
>>> class Python2NewStyleClass(object): # 이래야 New-style Class
    	pass

# Python3에서는 안그래도 된다!
>>> class Python3NewStyleClass: # 이렇게 해도 New-style Class
    	pass
>>> class Python3NewStyleClass(object): # 이래도 New-style Class
    	pass
```



그럼 실제적으로 Circle 클래스를 구현해보자.

```python
# Class를 활용한 Circle의 구현

from math import sqrt # math에서 sqrt함수를 import

class Circle(object): # Circle class를 생성함을 명시, Python3에선 object 상속 생략 가능
    def __init__(self, x, y, radius): # 생성자(Constructor) 함수
        self.x = x # 각 파라메터를 self의 변수에 할당
        self.y = y
        self.radius = radius

    def area(self): # area 값을 리턴해주는 함수
        return 3.14 * self.radius ** 2 # 파이는 3.14로 생각하자

    def distance(self, other):
        return sqrt((self.x - other.x) ** 2 + (self.y - other.y) ** 2) - (self.radius + other.radius)

# 활용
>>> circle1 = Circle(10, 20, 3)
>>> circle1.area()
28.26

>>> circle2 = Circle(100, -40, 10)
>>> circle2.area()
314.0

>>> circle1.distance(circle2)
95.16653826391968
```



파이썬에서는 그러면 클래스 간 공유되는 변수는 어떻게 선언하는가? 까지 정리한 클래스 구조를 알아보자.

```python
class 클래스명(object): # 클래스명은 영문으로 CamelCase를 맞춰 작성하며, python3이후로는 object 상속을 명시하지 않아도 됨
    
    classVariable1 = [] # 클래스 간 공유되는 변수들 선언
    classVariable2 = {}
    
    def __init__(self[, *params]): # 클래스의 생성자로써, 생성시 사용할 인자값들을 받아 저장하거나 생성시 필요한 변수할당 및 기능을 정의 []는 있어도 없어도 된다는 뜻. 인자를 받던지 말던지!
        self.*params = *params # 인자가 있다면 받는 코드 작성, 각 인스턴스 별 고유의 변수가 된다.
        def init_function1(self[, *params]): # 인스턴트 생성 시 쓰이는 함수 정의
            pass

    def class_fucntion1(self[, *params]): # 클래스의 함수 정의
        pass
```

이 정도면 원하는 커스텀 클래스를 구현하는덴 무리가 없을 듯.

이쯤 되면 하나 없는게 보인다. private은? public은? protect는?

**파이썬에서 지원하지 않는다.**

다만, Data Hiding과 Name Mangling을 위한 언더스코어 2개(\_\_)로 시작하는 이름에 한하여 Name Mangling을 제공하지만, 딱히 그렇다고 정말 private가 되는 것도 아니라서, 생각보다 안쓰게 될듯.

예를 들어 인스턴트 변수로 \_\_name을 선언한 경우에도, 클래스 내부에선 self.\_\_name으로 접근해야하고, 밖에서는 인스턴트명.\_클래스명\_\_변수명으로 접근 가능해서 딱히. . . 물론 맹글링된 경우 무시가 되기도 하는데, 흠.



이번엔 여기까지 : )
