---
layout: post
title: "파이썬과 모듈, 패키지"
subtitle: "Python, Module, Package"
categories: devlog
tags: python
---

> ## 파이썬과 모듈, 패키지

파이썬도 다른 소스코드에서 구현된 함수나 클래스 등을 이용해야 할 경우들이 있다. 그러한 경우에 사용되는 것들이 있는데

- import
- module
- package

가 있으며, 이번 시간엔 이 것을 알아볼 예정이다.



> #### import

- 다른 파이썬 소스파일 내 함수/클래스 등을 현재 공간으로 가져오기 위함
- import 시점에서 해당 코드가 실행



module.py (some_fn()) # 임의로 some_fn1 로 주석 내 표시

pkg1

├─     \_\_init\_\_.py

└──  pkg2

​            └─     \_\_init\_\_.py (some_fn()) # 임의로 some_fn2로 주석 내 표시

```python
import module # module.py
module.some_fn() # module.py 내 some_fn1

from module import some_fn() # module.py 내 some_fn1
some_fn()

import pkg1 # pkg1/__init__.py
import pkg1.pkg2 # pkg1/pkg2/__init__.py
pkg1.pkg2.some_fn() # pkg1/pkg2/__init__.py 내 some_fn2

from pkg1 import pkg2 # pkg1/pkg2/__init__.py
pkg2.some_fn() # pkg1/pkg2/__init__.py 내 some_fn2


import mymodule1 # mymodule1.py 내 사항을 가져옴
mymodule1.mysum(1, 2) # mymodule1.py 내 mysum 함수

import pkg1.pkg2 # pkg1/pkg2/__init__.py 사항을 가져옴
pkg1.pkg2.mysum(1, 2) # pkg1/pkg2/__init__.py 내 mysum 함수

import pkg1.pkg2.module # pkg1/pkg2/module.py/__init__.py 내 사항을 가져옴
pkg1.pkg2.module.mysum(1, 2) # 가능은 하나 너무 김
# 변경
from pkg1.pkg2 import module # pkg1/pkg2 의 module.py를 가져옴
module.mysum(1, 2) # module.py 내 mysum 함수
```



> #### module

- 다수의 함수/클래스들을 정의해둔 파이썬 소스코드 파일

```python
# mymodule.py
def mysum(x, y):
    return x + y

mymultiply = lamda x, y: x * y
```

실행

```python
>>> import mymodule # mymodule.py 가져옴
>>> mymodule.mysum(1, 2) # mymodule.py 내 mysum 함수
3
>>> mymodule.mymultiply(1, 2) # mymodule.py 내 mymultiply 함수
2

>>> from mymodule import mysum, mymultifly # mymodule.py 내 mysum과 mymultifly함수를 가져옴
>>> mysum(1, 2) # mymoudle.py에서 가져온 mysum 함수
3
>>> mymultifly(1, 2) # mymodule.py에서 가져온 mymultifly 함수
```



> #### package

- 파이썬 코드가 들어있는 디렉토리
- 해당 디렉토리에는 필히 \_\_init\_\_.py 파일이 있어야 파이썬의 패키지로써 인식.(파이썬 3.3 부터는 없어도 인식)
- 패키지를 improt할 때는 \_\_init\_\_.py가 import의 대상임.(패키지 내부 파이썬 소스코드들(\*.py 들)이 아님)

mylib

  ├     \_\_init\_\_.py

   │      └   mysum4 (import된 함수)

  └     math.py

​            └   mysum4 (실제 함수 위치)

```python
# mylib/__init__.py
from .math import mysum4 # 현재 있는 mylib/__init__.py에 같은 디렉토리(mylib)의 math.py에서 mysum4 함수를 import

# mylib/math.py
def mysum4(a, b, c, d): # 함수의 실제 정의
    return a + b + c + d
```

쉘에서 실행

```python
>>> from mylib import mysum4 # mylib/__init__.py 내 import된 mysum4 함수
>>> mysum(1, 2, 3, 4)
10

>>> from mylib.math import mysum4 # mylib/math.py 내 실제 mysum4 함수
>>> mysum(1, 2, 3, 4)
10

# 둘 다 정상적으로 기능함을 알 수 있음
```



만약 import한 함수들의 이름이 중복되면 어떻게 될까?

mylib.py

   ├   mysum4(함수)

mylib2.py

   └   mysum4(함수)

와 같은 구조일 때

```python
>>> from mylib import mysum4
>>> from mylib2 import mysum4 # 앞서 import된 mysum4를 덮어씀
>>> mysum4(1, 2, 3, 4) # 둘 다 따로 쓰고 싶지만 늦게 improt된 mylib2의 mysum4만 사용됨
mylib2.mysum4 : 10
>>> mysum4(1, 2, 3, 4)
mylib2.mysum4 : 10
```

위와 같은 일이 일어난다.

이를 해결하기 위해서 파이썬에서는 이름을 재지정하는 방법을 지원한다.

import 시 **as**를 통하여 원하는 이름으로 변경 가능하다.

```python
>>> from mylib import mysum4 as mylib_mysum4
>>> from mylib import mysum4 as mylib2_mysum4
>>> mylib_mysum4(1, 2, 3, 4)
mylib.mysum4 : 10
>>> mylib2_mysum4(1, 2, 3, 4)
mylib2.mysum4 : 10
```



> #### Relative Import

패키지 내에서 다른 모듈/패키지 가져오기

main.py

pkg1

   ├    \_\_init\_\_.py

   │        └    mysum (import된 함수)

   └    math.py

​              └    mysum (실제 함수)

```python
# pkg1/math.py
def mysum(x, y):
	return x + y

# pkg/__init__.py
from .math import mysum # mysum함수를 현재 이름공간(name space)에 가져옴

# main.py : 아래 2가지 방법 모두 mysum을 사용 가능
from pkg1 import mysum
from pkg1.math import mysum
```



**import 경로**

- import를 수행할 떄, **sys.path** 경로에서 모듈/패키지 탐색
  - 환경변수 PATH와 유사
- **sys.path**는 list이기 때문에, 자유롭게 추가/수정/삭제 가능
  - 수정된 내용은 현재 프로세스에서만 유효
  - 관리성이 나빠지기 때문에 권장하지 않음
- 현재 디렉토리와 sys.path 경로에서 지정 모듈/패키지를 찾지 못한 경우, **ImportError** 발생



> #### 파이썬의 \_\_file\_\_ , \_\_name\_\_

- **\_\_file\_\_**을 호출하면, 해당 파이썬 소스코드의 파일 경로를 반환해준다.
- **\_\_name\_\_**을 호출하면, 해당 파이썬 코드의 파일명을 반환해준다.
  - 최초 진입 스크립트의 경우, \_\_main\_\_
  - import된 경우,  본래 \_\_name\_\_이 유지된 채로 실행
    - **이 차이를 이용하여 import시에는 실행되지 않지만, 최초 진입 시 실행할 코드 작성 가능**





이번엔 여기까지 : )