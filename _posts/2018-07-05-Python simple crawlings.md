---
layout: post
title: "파이썬 간단한 크롤링 예제들"
subtitle: "Python, Simple Crawlings"
categories: devlog
tags: python
---



> ## 파이썬 간단한 크롤링 예제들

#### LV. 1 HTML 크롤링

```python
import requests
from bs4 import BeautifulSoup
request_url = 'https://askdjango.github.io/lv1/'
response = requests.get(request_url)
html = response.text
soup = BeautifulSoup(html, 'html.parser')
course_list = soup.select('.course a')

for course in course_list:
    print(course.text, course['href'])
```

```
개발환경 구축하기 https://www.askcompany.kr/vod/setup/
파이썬 차근차근 시작하기 https://www.askcompany.kr/vod/python/
크롤링 차근차근 시작하기 https://www.askcompany.kr/vod/crawling/
파이썬으로 업무 자동화 https://www.askcompany.kr/vod/automation/
장고 - 기본편 https://www.askcompany.kr/vod/django/
장고걸스 튜토리얼 https://www.askcompany.kr/vod/djangogirls/
장고 - Form/ModelForm 잘 알고 쓰기 https://www.askcompany.kr/vod/form/
장고 - 클래스 기반 뷰. 잘 알고 쓰기 https://www.askcompany.kr/vod/cbv/
장고 - 결제 시스템 연동 https://www.askcompany.kr/vod/payment/
장고 - 웹 프론트엔드 시작편 https://www.askcompany.kr/vod/frontend/
장고 - 다양한 위젯 만들기 https://www.askcompany.kr/vod/widgets/
장고 - API 서버 만들기 및 초간단 안드로이드 앱 만들기 https://www.askcompany.kr/vod/apiserver/
장고 - 하이브리드 앱 만들기 https://www.askcompany.kr/vod/hybrid/
장고 - 서비스 배포하기 https://www.askcompany.kr/vod/deploy/
장고 - 실전편 (Feat. 배달의 민족 St. 만들기) https://www.askcompany.kr/vod/django-baemin/
장고 - 실전편 (Feat. 인스타그램 St. 만들기) https://www.askcompany.kr/vod/django-instagram/
```





#### LV. 2  JSON 크롤링

```python
import json
import requests

json_url = 'https://askdjango.github.io/lv2/data.json'
json_string = requests.get(json_url).text
data_list = json.loads(json_string)

for data in data_list:
    print('{name} {url}'.format(**data))
```

```
개발환경 구축하기 https://www.askcompany.kr/vod/setup/
파이썬 차근차근 시작하기 https://www.askcompany.kr/vod/python/
크롤링 차근차근 시작하기 https://www.askcompany.kr/vod/crawling/
파이썬으로 업무 자동화 https://www.askcompany.kr/vod/automation/
장고 - 기본편 https://www.askcompany.kr/vod/django/
장고걸스 튜토리얼 https://www.askcompany.kr/vod/djangogirls/
장고 - Form/ModelForm 잘 알고 쓰기 https://www.askcompany.kr/vod/form/
장고 - 클래스 기반 뷰. 잘 알고 쓰기 https://www.askcompany.kr/vod/cbv/
장고 - 결제 시스템 연동 https://www.askcompany.kr/vod/payment/
장고 - 웹 프론트엔드 시작편 https://www.askcompany.kr/vod/frontend/
장고 - 다양한 위젯 만들기 https://www.askcompany.kr/vod/widgets/
장고 - API 서버 만들기 및 초간단 안드로이드 앱 만들기 https://www.askcompany.kr/vod/apiserver/
장고 - 하이브리드 앱 만들기 https://www.askcompany.kr/vod/hybrid/
장고 - 서비스 배포하기 https://www.askcompany.kr/vod/deploy/
장고 - 실전편 (Feat. 배달의 민족 St. 만들기) https://www.askcompany.kr/vod/django-baemin/
장고 - 실전편 (Feat. 인스타그램 St. 만들기) https://www.askcompany.kr/vod/django-instagram/
```



#### LV. 3  JavaScript 크롤링

```python
import re
import json
import requests

get_url = 'https://askdjango.github.io/lv3/'
html = requests.get(get_url).text
matched = re.search(r'var courses = (.+?);', html, re.S)
json_string = matched.group(1)
json_list = json.loads(json_string)

for course in json_list:
    print('{name} {url}'.format(**course))
```

```
개발환경 구축하기 https://www.askcompany.kr/vod/setup/
파이썬 차근차근 시작하기 https://www.askcompany.kr/vod/python/
크롤링 차근차근 시작하기 https://www.askcompany.kr/vod/crawling/
파이썬으로 업무 자동화 https://www.askcompany.kr/vod/automation/
장고 - 기본편 https://www.askcompany.kr/vod/django/
장고걸스 튜토리얼 https://www.askcompany.kr/vod/djangogirls/
장고 - Form/ModelForm 잘 알고 쓰기 https://www.askcompany.kr/vod/form/
장고 - 클래스 기반 뷰. 잘 알고 쓰기 https://www.askcompany.kr/vod/cbv/
장고 - 결제 시스템 연동 https://www.askcompany.kr/vod/payment/
장고 - 웹 프론트엔드 시작편 https://www.askcompany.kr/vod/frontend/
장고 - 다양한 위젯 만들기 https://www.askcompany.kr/vod/widgets/
장고 - API 서버 만들기 및 초간단 안드로이드 앱 만들기 https://www.askcompany.kr/vod/apiserver/
장고 - 하이브리드 앱 만들기 https://www.askcompany.kr/vod/hybrid/
장고 - 서비스 배포하기 https://www.askcompany.kr/vod/deploy/
장고 - 실전편 (Feat. 배달의 민족 St. 만들기) https://www.askcompany.kr/vod/django-baemin/
장고 - 실전편 (Feat. 인스타그램 St. 만들기) https://www.askcompany.kr/vod/django-instagram/
```



이번엔 여기까지 : )