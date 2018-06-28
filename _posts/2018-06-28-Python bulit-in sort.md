---
layout: post
title: "파이썬과 빌트인함수, 정렬"
subtitle: "Python, Built-in Function, Sort"
categories: devlog
tags: python

---



> ## 빌트인 함수, 파이썬에서 기본 제공하는 함수

여타 언어들처럼, 파이썬도 기본적으로 제공하는 함수들이 있다.

<div id="contents" />

|                                |                            | Built-in Functions           |                              |                                  |
| ------------------------------ | -------------------------- | ---------------------------- | ---------------------------- | -------------------------------- |
| [abs()](#absf)                 | [delatter()](#delatterf)   | [hash()](#hashf)             | [memoryview()](#memoryviewf) | [set()](#setf)                   |
| [all()](#allf)                 | [dict()](#dictf)           | [help()](#helpf)             | [min()](#minf)               | [setattr](#setattrf)             |
| [any()](#anyf)                 | [dir()](#dirf)             | [hex()](#hexf)               | [next()](#nextf)             | [slice()](#slicef)               |
| [ascii()](#asciif)             | [divmod()](#divmodf)       | [id](#idf)                   | [object()](#objectf)         | [sorted()](#sortedf)             |
| [bin()](#binf)                 | [enumerate()](#enumeratef) | [input()](#inputf)           | [oct()](#octf)               | [staticmethod()](#staticmethodf) |
| [bool()](#boolf)               | [eval()](#evalf)           | [int()](#intf)               | [open()](#openf)             | [str()](#strf)                   |
| [breakpoint()](#breakpointf)   | [exec()](#execf)           | [isinstance()](#isinstancef) | [ord()](#ordf)               | [sum()](#sumf)                   |
| [bytearray()](#bytearrayf)     | [filter()](#filterf)       | [issubclass()](#issubclassf) | [pow()](#powf)               | [super()](#superf)               |
| [bytes()](#bytesf)             | [float()](#floatf)         | [iter()](#iterf)             | [print()](#printf)           | [tuple()](#tuplef)               |
| [callable()](#callablef)       | [format()](#formatf)       | [len()](#lenf)               | [property()](#propertyf)     | [type()](#typef)                 |
| [chr()](#chrf)                 | [frozenset()](#frozensetf) | [list()](#listf)             | [range()](#rangef)           | [vars()](#varsf)                 |
| [classmethod()](#classmethodf) | [getattr()](#getattrf)     | [locals()](#localsf)         | [repr()](#reprf)             | [zip()](#zipf)                   |
| [compile()](#compilef)         | [globals()](#globalsf)     | [map()](#mapf)               | [reversed()](#reversedf)     | [\_\_import\_\_()](#__importf__) |
| [complex()](#complexf)         | [hasattr()](#hasattrf)     | [max()](#maxf)               | [round()](#roundf)           |                                  |

<div id="absf">abs(x)</div>[TOP](#contents)

<div id="allf">all(iterable)</div>[TOP](#contents)

<div id="anyf">any(iterable)</div>[TOP](#contents)

<div id="asciif">ascii(object)</div>[TOP](#contents)

<div id="binf">bin(x)</div>[TOP](#contents)

<div id="boolf">class bool([x])</div>[TOP](#contents)

<div id="breakpointf">breakpoint(*args, **kws)</div>[TOP](#contents)

<div id="bytearrayf">class bytearray([source[, encoding[,errors]]])</div>[TOP](#contents)

<div id="bytesf">class bytes([source[, encoding[,errors]]])</div>[TOP](#contents)

<div id="callablef">callable(object)</div>[TOP](#contents)

<div id="chrf">chr(i)</div>[TOP](#contents)

<div id="classmethodf">@classmethod</div>[TOP](#contents)

<div id="compilef">compile(source, filename, mode, flags=0, dont_inherit=False, optimize=-1)</div>[TOP](#contents)

<div id="complexf">class complex([real[, imag]])</div>[TOP](#contents)

<div id="delattr">delattr(object, name)</div>[TOP](#contents)

<div id="dictf">class dict(**kwarg)<br />class dict(mapping, **kwarg)<br />class dict(iterable, **kwarg)<br /></div>[TOP](#contents)

<div id="dirf">dir([object])</div>[TOP](#contents)

<div id="divmodf">divmod(a, b)</div>[TOP](#contents)

<div id="enumeratef">enumerate(iterable, start=0)</div>[TOP](#contents)

<div id="evalf">eval(expression, globals=None, locals=None)</div>[TOP](#contents)

<div id="execf">exec(object[, globlas[, locals]])</div>[TOP](#contents)

<div id="filterf">filter(function, iterable)</div>[TOP](#contents)

<div id="floatf">class float([x])</div>[TOP](#contents)

<div id="formatf">format(value[, format_spec])</div>[TOP](#contents)

<div id="frozensetf">frozenset([iterable])</div>[TOP](#contents)

<div id="getattrf">getattr(object, name[, default])</div>[TOP](#contents)

<div id="globalsf">globals()</div>[TOP](#contents)

<div id="hasattrf">hasattr(object, name)</div>[TOP](#contents)

<div id="hashf">hash(object)</div>[TOP](#contents)

<div id="helpf">help([object])</div>[TOP](#contents)

<div id="hexf">hex(x)</div>[TOP](#contents)

<div id="idf">id(object)</div>[TOP](#contents)

<div id="inputf">input([prompt])</div>[TOP](#contents)

<div id="intf">class int(X=0)<br />class int(X, base=10)</div>[TOP](#contents)

<div id="isinstancef">isinstance(object, classinfo)</div>[TOP](#contents)

<div id="iterf">iter(object[, sentinel])</div>[TOP](#contents)

<div id="lenf">len(s)</div>[TOP](#contents)

<div id="listf">class list([iterable])</div>[TOP](#contents)

<div id="localsf">locals()</div>[TOP](#contents)

<div id="mapf">map(function, iterable, …)</div>[TOP](#contents)

<div id="maxf">max(iterable, *[, key, default]<br />max(arg1, arg2, *args[, key]))</div>[TOP](#contents)

<div id="memoryviewf">memoryview(object)</div>[TOP](#contents)

<div id="minf">min(iterable, *[, key, default]<br />min(arg1, arg2, *args[, key]))</div>[TOP](#contents)

<div id="nextf">next(iterator[, default])</div>[TOP](#contents)

<div id="objectf">class object</div>[TOP](#contents)

<div id="octf">oct(x)</div>[TOP](#contents)

<div id="openf">open(file, mode='r', buffering=-1, encoding=None, errors=Nonne, newline=None, closefd=True, opener=None)</div>[TOP](#contents)

<div id="ordf">ord(c)</div>[TOP](#contents)

<div id="powf">pow(x, y[, z])</div>[TOP](#contents)

<div id="printf">print(*objects, sep=' ', end=\'\\n\', file=sys.stdout. flush=False)</div>[TOP](#contents)

<div id="propertyf">class property(fget=None, fset=None, fdel=None, doc=None)</div>[TOP](#contents)

<div id="rangef">range(stop)<br />range(start, stop[, step])</div>[TOP](#contents)

<div id="reprf">repr(object)</div>[TOP](#contents)

<div id="reversedf">reversed(seq)</div>[TOP](#contents)

<div id="roundf">round(number[, ndigits])</div>[TOP](#contents)

<div id="setf">class set([iterable])</div>[TOP](#contents)

<div id="setattrf">setattr(object, name, value)</div>[TOP](#contents)

<div id="slice">class slice(stop)<br />class slice(start, stop[, step])</div>[TOP](#contents)

<div id="sortedf">sorted(iterable, *, key=None, reverse=False)</div>[TOP](#contents)

<div id="staticmethodf">@staticmethod</div>[TOP](#contents)

<div id="strf">class str(object='')<br />class str(object=b'', encoding='utf-8', errors='strict')</div>[TOP](#contents)

<div id="sumf">sum(iterable[, start])</div>[TOP](#contents)

<div id="superf">super([type[, object-or-type]])</div>[TOP](#contents)

<div id="tuplef">tuple([iterable])</div>[TOP](#contents)

<div id="typef">class type(object)<br />class type(name, bases, dict)</div>[TOP](#contents)

<div id="varsf">vars([object])</div>[TOP](#contents)

<div id="zipf">zip(*iterables)</div>[TOP](#contents)

<div id="__import__f">\_\_import\_\_(name, globals=None, locals=None, fromlist=(), level=0)</div>[TOP](#contents)

<div id="">(X)</div>[TOP](#contents)





이번엔 여기까지 : )
