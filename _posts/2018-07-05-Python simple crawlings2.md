---
layout: post
title: "파이썬 간단한 크롤링 예제들 2"
subtitle: "Python, Simple Crawlings 2"
categories: devlog
tags: python
---



> ## 파이썬, 간단한 크롤링 예제들 2

#### 네이버 실시간 검색어 크롤링, HTML

```python
import requests
from bs4 import BeautifulSoup

lists = BeautifulSoup(requests.get('https://www.naver.com').text, 'html.parser').select('.PM_CL_realtimeKeyword_list_base .ah_k')
for idx, lst in enumerate(lists, 1) :
    print(idx, lst.text)
```

```
1 홍명보
2 화서역 파크 푸르지오
3 손희주
4 정몽규
5 엠카운트다운
6 화서역
7 김구라 여자친구
8 송종국
9 이대호
10 김흥국
11 리프트 라이벌즈
12 김수미막김치
13 호날두
14 수미네 반찬 레시피
15 이대호 김재호
16 김혜진
17 유벤투스
18 이과인
19 성수동 두루찌개
20 앤트맨과 와스프
```



#### 네이버 블로그 게시물 크롤링

```python
import requests
from bs4 import BeautifulSoup
from collections import OrderedDict
from itertools import count

def simple_blog_search(searchingword):
    headers = {
        'accept' : 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8',
        'accept-encoding' : 'gzip, deflate, br',
        'upgrade-insecure-requests' : '1',
        'referer' : 'https://search.naver.com/search.naver?where=post&query=',
        'user-agent' : 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/67.0.3396.99 Safari/537.36',
        
    }
    params = {
        'query' : searchingword,
        'where' : 'post',
    }
    pre_url = 'https://search.naver.com/search.naver?'
    titles_dict = OrderedDict()
    times_dict = OrderedDict()
    passages_dict = OrderedDict()
    repetitivecounter = 0
    for page in count(1):
        params['start'] = (page-1)*10 + 1
        basesoup = BeautifulSoup(requests.get(pre_url, params=params, headers=headers).text, 'html.parser')
        times = basesoup.select('._blogBase .sh_blog_top .txt_inline')
        titles = basesoup.select('._blogBase .sh_blog_top .sh_blog_title')
        passages = basesoup.select('._blogBase .sh_blog_top .sh_blog_passage')
        titleslen = len(titles)
        for time, title, passage in zip(times, titles, passages):
            if title['href'] in titles_dict:
                repetitivecounter += 1
            else:
                titles_dict[title['href']] = title.text
                times_dict[title['href']] = time.text
                passages_dict[title['href']] = passage.text
        if titleslen == repetitivecounter:
            for (href, title) in titles_dict.items():
                print('{} / {}\n >> {}\n Link : {}\n'.format(title, times_dict[href], passages_dict[href], href))
            return
        else:
            repetitivecounter = 0
```

```python
>>> simple_blog_search('AskDjango')
```

```
Askdjango) 파이썬 기본문법 정리)2.파이썬의 기본... / 2017.02.22. 
 >> - 지난 포스팅 Askdjango) python 기본문법 정리 1.python 의 코드 실행방법... 참고한 자료: askdjango github: https://github.com/askdjango 아 그리고 저는 페이스북...
 Link : http://myjorney.tistory.com/2

[AskDjango] 강의 노트 #1 / 2018.02.01. 
 >> 진행하는 ‘AskDjango’ VOD를 구독하여 배울 기회가 생겼습니다. 기존에... FBV에 충분히 익숙해지면 CBV는 차후에 공부하기 편하다. Reference AskDjango - VOD
 Link : https://kirade.github.io/django/2018/02/01/askdjango-%EA%B0%95%EC%9D%98-%EB%85%B8%ED%8A%B8-1/

[온오프믹스][AskDjango 특강] 장고 간단 튜토리얼 및 Azure... / 2017.12.06. 
 >> [온오프믹스][AskDjango 특강] 장고 간단 튜토리얼 및 Azure 플랫폼 배포... 08 [온오프믹스][AskDjango 특강] 장고 간단 튜토리얼 및 Azure 플랫폼 배포 [종료]...
 Link : http://secure-edu.tistory.com/1846

[askDjango] 파이썬 차근차근 시작하기 (1) / 2018.05.09.  
 >> 오늘까지 절반정도 들었는데.. 이 걸로는 첫글. 장고를 배우기 위해 기초파이썬 문법을 가르쳐주는 강의이다. 그렇게 기초는 아니고.. 클래스와 객체 개념도 설명하니.....
 Link : https://blog.naver.com/sym0222?Redirect=Log&logNo=221271991066

2017.02 네이버 맞춤법 검사기 (출처: askdjango) / 2017.02.20. 
 >> 2017.02 네이버 맞춤법 검사기 (askdjango 이진석님 강의 따라하기) 시연영상 (26초 1.3MB) | "어떻게"로 고쳐야 하는데 "어떡해"로 고친 것 빼고는 다...
 Link : https://the7mincheol.blog.me/220940162131

askdjango - 개발 환경 강의 정리 / 2018.01.05. 
 >> 1. python3 설치 및 jupyter notebook -> 이미 구동되어 있고 간단하여 메모 생략 2. PyPI 패키지 목록 -> Python 라이브러리 공식 저장소 Python3 명령일 경우 pip3를 사용...
 Link : https://blog.naver.com/scarletbreez?Redirect=Log&logNo=221178294385

Askdjango) 파이썬 기본문법 정리 4)python 파이썬 flow... / 2017.02.26. 
 >> - 지난 포스팅 Askdjango) python 기본문법 정리 1.python 의 코드 실행방법 , python 데이터 타 입 : http://myjorney.tistory.com/1 Askdjango) python 기본문법 정리)2....
 Link : http://myjorney.tistory.com/5

[AskDjango] 강의 노트 #4 / 2018.02.07. 
 >> 형태로 동일 키가 다수의 Value를 지원한다. QueryDict 는 변경이 불가능하다. (Immutable) HttpResponse 는 뷰 함수가 리턴하는 값이다. Reference AskDjango - VOD
 Link : https://kirade.github.io/django/2018/02/07/askdjango-%EA%B0%95%EC%9D%98-%EB%85%B8%ED%8A%B8-4/

Askdjango) 파이썬 기본문법 정리 18) 파이썬 클래스... / 2017.05.03. 
 >> 파이썬 askdjango를 강의로 복습하던 중에 파이썬의 클래스를 부분을... for i in range(5): marine = Marine('서울') 참고한 자료: askdjango github: https://github.com/askdjango...
 Link : http://myjorney.tistory.com/26

[askDjango] 파이썬 차근차근 시작하기 (2) / 2018.05.15. 
 >> 확실히 뭔가 프로젝트(?) 비슷한걸 해본 부분에 대한 내용은 명확히 이해가 된다. 그냥 아무 생각없이 따라쓰던 import에 대한 설명을 들으니 전체적인 구조가 눈에...
 Link : https://blog.naver.com/sym0222?Redirect=Log&logNo=221275875928

2017.03 멜론 노래 검색기 (출처: askdjango) / 2017.03.01. 
 >> 강의출처 - https://www.youtube.com/watch?v=Axjz12E_jyA 동영상 시연 영상 (37초)
 Link : https://the7mincheol.blog.me/220947602901

[AskDjango] 강의 노트 #6 / 2018.02.08. 
 >> 를 사용하면 정보를 가져올수 없는 정보를 호출 시점에 가져오기 위해 만들어졌다. 전역변수 혹은 클래스 변수에 사용된다. Reference AskDjango - VOD
 Link : https://kirade.github.io/django/2018/02/08/askdjango-%EA%B0%95%EC%9D%98-%EB%85%B8%ED%8A%B8-6/

Askdjango) 파이썬 기본문법 정리 17) 파이썬 file... / 2017.03.29. 
 >> Askdjango) python 기본문법 정리 1.python 의 코드 실행방법 , python 데이터 타 입 : http://myjorney.tistory.com/1 Askdjango) python 기본문법 정리)2....
 Link : http://myjorney.tistory.com/23

[askDjango] DjangoGirls tutorials (2) / 2018.05.12. 
 >> 장고부분을 공부하기 시작했는데 생각보다... 쉽지않다. 이렇게 설명하면 누가 알아듣겠나 싶기도하고.. 세부적인 설명없이 이렇게 하면 된다 식으로 진행하면서...
 Link : https://blog.naver.com/sym0222?Redirect=Log&logNo=221273741456

Askdjango) 파이썬 기본문법 정리1_파이썬의 코드 실행방법... / 2017.02.19. 
 >> - 먼저 이 내용들은 이진석님의 Askdjango 강의노트를 jupyer로 공부하며 복습하기 위해 정리한 내용입니다. (밑에 askdjango github 링크는 참고자료에...
 Link : http://myjorney.tistory.com/1

[AskDjango] 강의 노트 #2 / 2018.02.01. 
 >> update 함수를 사용하여 속성을 지정하면, 한 개의 SQL로 동작하므로 동작이 빠르다. 인스턴스를 삭제하는 경우도 마찬가지이다. Reference AskDjango - VOD
 Link : https://kirade.github.io/django/2018/02/01/askdjango-%EA%B0%95%EC%9D%98-%EB%85%B8%ED%8A%B8-2/

Askdjango) 파이썬 기본문법 정리 14) 파이썬 대소비교... / 2017.03.18. 
 >> - 지난 포스팅 Askdjango) python 기본문법 정리 1.python 의 코드 실행방법 , python 데이터 타 입 : http://myjorney.tistory.com/1 Askdjango) python 기본문법...
 Link : http://myjorney.tistory.com/17

[askDjango] 파이썬 차근차근 시작하기 (완) / 2018.05.16. 
 >> 총평을 말하자면... 기본에도 충실하고 다른 파이썬 무료 온라인 강의에서 잘 다루지 않는 클래스나 데코레이터에 대한 설명도 좋았다. 사실 매번 모르고 넘어가던...
 Link : https://blog.naver.com/sym0222?Redirect=Log&logNo=221276726557

[AskDjango] 강의 노트 #7 / 2018.02.08. 
 >> 를 적용하여 쿼리를 줄일 수 있다. Post 와 Comment, Tags의 관계에서 Post.objects.all().prefetch_related('comment_set', 'tag_set') Reference AskDjango - VOD
 Link : https://kirade.github.io/django/2018/02/08/askdjango-%EA%B0%95%EC%9D%98-%EB%85%B8%ED%8A%B8-7/

Askdjango) 파이썬 기본문법 정리 19) 파이썬 호출... / 2017.05.07. 
 >> (0) 2017.08.12 Askdjango) 파이썬 기본문법 정리 19) 파이썬 호출 가능한 객체, 파이썬 callable instance (2) 2017.05.07 Askdjango) 파이썬 기본문법 정리 18) 파이썬 클래스...
 Link : http://myjorney.tistory.com/28

[AskDjango] 강의 노트 #3 / 2018.02.05. 
 >> 특정 모델에 대한 DetailView 를 작성할 때에는, 해당 뷰의 URLConf 설정 후, 반드시 get_absolute_url 을 작성하면 코드가 간결해진다. Reference AskDjango - VOD
 Link : https://kirade.github.io/django/2018/02/05/askdjango-%EA%B0%95%EC%9D%98-%EB%85%B8%ED%8A%B8-3/

[askDjango] 크롤링 차근차근 시작하기 (1) / 2018.05.16. 
 >> 오오... 정말 말그대로 크롤링을 차근차근 시작해보았다. requests 모듈만 가지고 웹툰까지 크롤링이 될줄은 몰랐네... 이런 구조를 짜는게 중요한거 같다. for 문에...
 Link : https://blog.naver.com/sym0222?Redirect=Log&logNo=221276753501

[AskDjango] 강의 노트 #5 / 2018.02.08. 
 >> 자주 사용되는 변수들은 요청시 따로 명시하지 않아도 장고에서 Context Processors 를 통해 넘겨준다. debug, request, auth, messages Background AskDjango - VOD
 Link : https://kirade.github.io/django/2018/02/08/askdjango-%EA%B0%95%EC%9D%98-%EB%85%B8%ED%8A%B8-5/

[askDjango] Djangogirls tutorials(1) / 2018.05.11. 
 >> python Django를 통해 간단한 웹페이지를 만드는 강의인듯. 왜 이런데까지 girls를 강조하는지는 모르겠지만..(railsgirls, djangogirls 등..) 7 섹션으로 나누어진다....
 Link : https://blog.naver.com/sym0222?Redirect=Log&logNo=221272860552

[ Android ] FCM 푸시 관련 / 2017.05.16. 
 >> 이것을 사용해서 메시지를 설정하니 정상 작동하였다... 야호!! 참조: https://medium.com/askdjango-android/firebase-cloud-messaging-과-함께-푸쉬-지원-717fa1945d2 – –
 Link : https://blog.naver.com/lys1900?Redirect=Log&logNo=221007039328

크롤링 강의 / 2018.01.17. 
 >> 만든 언어는 c# Django-admin startproject askdjango Cd askdjango Python manage.py runserver 그럼 서버에서 적솝. Localhost/8000 서버는 항상 켜둔 상태로 둔다. Python manage.py...
 Link : https://blog.naver.com/hyunji107?Redirect=Log&logNo=221187145456

[QuerySet]쿼리셋 수정을 통한 웹서비스 성능 개선 / 2018.02.23. 
 >> 사실 AskDjango 에서 관련된 강의를 들었을 때는 ‘아 이런게... reference (AskDjango 강의) Relation QuerySet 성능 높이기 - selected_related, prefetch...
 Link : http://roxxy.tistory.com/entry/QuerySet%EC%BF%BC%EB%A6%AC%EC%85%8B-%EC%88%98%EC%A0%95%EC%9D%84-%ED%86%B5%ED%95%9C-%EC%9B%B9%EC%84%9C%EB%B9%84%EC%8A%A4-%EC%84%B1%EB%8A%A5-%EA%B0%9C%EC%84%A0

[Python] 크롤링 예제. Lv3 자바스크립트 렌더링 크롤링 풀이 / 2017.07.21. 
 >> 문제주소 https://askdjango.github.io/lv3/ 문제 화면 여기서 list 제목만... 1 2 3 4 5 6 import re #정규표현식 import requests url = 'https://askdjango.github.io/lv3...
 Link : http://rednooby.tistory.com/104

파이썬의 리스트와 튜플 조금 더 깊게 이해하기. / 2017.11.12. 
 >> - 참조자료 *고성능파이썬 http://www.yes24.com/searchcorner/Search?keywordAd=&keyword=&domain=ALL&qdomain=%C0%FC%C3%BC&Wcode=001_005&query=%B0%ED%BC%BA%B4%C9+%C6%C4%C0%CC%BD%E3 * askdjango...
 Link : http://myjorney.tistory.com/76

Global Azure Bootcamp x Korea 2017 / 2017.03.20. 
 >> 실습이 포함되어있습니다.) 이진석 Azure MVP AskDjango 운영진 남정현 Azure MVP Azure 한국 사용자 그룹 운영진 16:30 ~ 16:50 휴식 시간 16:50 ~ 17:50 세션...
 Link : https://cyberpass.blog.me/220962782139

데브옵스 코리아 야간 과외 / 2018.05.04. 
 >> * 데브옵스 코리아 야간 과외 * 주제 : 3시간만에 서비스 개발하기 부제 : 아이돌 안면인식(Anmyeon Insik = A.I.) 장애를 해결하자 강사 : AskDjango...
 Link : https://blog.naver.com/dancingmarionet?Redirect=Log&logNo=221267923025

[온오프믹스]2018 데이터로 준비하는 패션 4차 산업혁명 포럼... / 2017.12.08. 
 >> 08 [온오프믹스][AskDjango 특강] 장고 간단 튜토리얼 및 Azure 플랫폼 배포 [종료] (0) 2017.12.06 [온오프믹스]2017 공개SW 커뮤니티 컨퍼런스 데이 [종료] (0) 2017.12.06...
 Link : http://secure-edu.tistory.com/1855

[엔코아]공감토크이공20 : 14탄 파이썬을 통한 웹페이 크롤링... / 2018.01.14. 
 >> 사회자가 '국내에서 가장 파이썬을 잘하는 분'이라고 소개한 이번 강연의 강연자 이진석 대표는 'askdjango'라는 닉네임으로 파이썬 관련 교육 개발 등 다양한 활동을...
 Link : https://blog.naver.com/shamcoding?Redirect=Log&logNo=221184554048

[Python] 크롤링 예제. Lv2 Ajax 렌더링 크롤링 풀이 / 2017.07.21. 
 >> 문제 주소 https://askdjango.github.io/lv2/ 1. 페이지 탐색 크롤링할 페이지... json_url = 'https://askdjango.github.io/lv2/data.json' #받아온 json을 텍스트로 변환...
 Link : http://rednooby.tistory.com/103

NoReverseMatch Error / 문제 해결 / 2018.04.19. 
 >> askDjango의 이진석 님께서 조언을 해주셨는데요. Slug 필드를 확인을 해보았습니다. 위 사진과 같이 SLUG 필드가 한개...
 Link : http://politic.co.kr/176

[Python] 크롤링 예제. Lv1 단순 HTML 크롤링 풀이 / 2017.07.21. 
 >> 문제주소 https://askdjango.github.io/lv1/ 1. 페이지 탐색 이 페이지의... get( 'https://askdjango.github.io/lv1/' ) print (response) print (response.text) Colored...
 Link : http://rednooby.tistory.com/102

[Python] 파이썬 코드로 네이버 맞춤법 검사해보기... / 2018.05.11. 
 >> 검사해보기 AskDjango VOD 서비스를 오픈했습니다. 체계적인 장고 VOD 강의를 경험해보세요. / ---- AskDjango 페이스북 페이지에 공유했던 스크린캐스트를 유튜브에...
 Link : https://blog.naver.com/tkwnthwd727?Redirect=Log&logNo=221272883292

[온오프믹스]2017 제3회 오픈소스 성공사례 세미나 [종료] / 2017.12.09. 
 >> 데이터로 준비하는 패션 4차 산업혁명 포럼 [모집종료] (0) 2017.12.08 [온오프믹스][AskDjango 특강] 장고 간단 튜토리얼 및 Azure 플랫폼 배포 [종료] (0) 2017.12.06
 Link : http://secure-edu.tistory.com/1857

World Nomade의 크롤링한 내용 + 카카오톡 봇 만들기... / 2017.08.13. 
 >> ---------------------------------------------------------------------- 이 자료는 askdjango 강의인 http://nomade.kr 의 강의 내용을 기반으로 만든 자료입니다....
 Link : http://myjorney.tistory.com/41

<world nomade의 django 여행> Django admin url 변경을 통안... / 2017.06.16. 
 >> askdjango 페이스북에 질문을 올리고 -> 첫번자 인자가 문자열로서 정규 표현식 패턴을 기입하는 것이므로, 변수와 조합하시면 됩니다. 라는...
 Link : http://myjorney.tistory.com/30

ImportError: cannot import name "Model" / 문제 해결 / 2018.04.16. 
 >> AskDjango의 이진석 님께서 답변을 주셨습니다. 에러는 생각 이상으로 쉽게 잡혔네요... Models.py에 from .views import CmpDetail...
 Link : http://politic.co.kr/175

[온오프믹스]제2회 게임문화포럼 [종료] / 2017.12.06. 
 >> [온오프믹스][AskDjango 특강] 장고 간단 튜토리얼 및 Azure 플랫폼 배포 [종료] (0) 2017.12.06 [온오프믹스]2017 공개SW 커뮤니티 컨퍼런스 데이 [종료] (0) 2017.12.06...
 Link : http://secure-edu.tistory.com/1844

앱이름/post_list.html 작성 / 2018.05.11. 
 >> <h1>AskDjango</h1> <p>안녕하세요. 여러분의 파이썬/장고 페이스메이커가 되겠습니다. ;)</p> <ul> <li>http://localhost:8000/blog/ 주소로...
 Link : https://blog.naver.com/jslims71?Redirect=Log&logNo=221273506776

Firebase Cloud Messaging(FCM) / 2017.06.26. 
 >> 출처 - https://medium.com/askdjango-android/firebase-cloud-messaging-과-함께-푸쉬-지원-717fa1945d2 Firebase Cloud Messaging 과 함께 푸쉬 지원 Set Up a Firebase...
 Link : http://darksilber.tistory.com/entry/Firebase-Cloud-MessagingFCM

Django 설치 및 Overview / 2017.07.03. 
 >> 본 내용은 AskDjango에서 제공하는 VOD 및 PDF 자료를 적극 참고했다.... 웹 프레임워크에 관심이 있다면 AskDjango VOD 자료를 구독해보는 것도 좋을 것 같다.
 Link : https://blog.naver.com/thnam91?Redirect=Log&logNo=221043315992

[Python] Selenium 카페 글쓰기 자동화 / 2017.10.25. 
 >> find_element_by_css_selector('[class*=Submit]').click()()finally: pass 참고 : https://nomade.kr/vod/automation/137/ AskDjango 이진석님.
 Link : https://blog.naver.com/answn3475?Redirect=Log&logNo=221125073519

크롤링 실습 - 빅카인즈 검색 결과 / 2017.07.06. 
 >> strip(), doc_url) 본 내용은 AskDjango에서 제공하는 VOD 및 PDF 자료를... 관심이 있다면 AskDjango VOD 자료를 구독해보는 것도 좋을 것 같다.
 Link : https://blog.naver.com/thnam91?Redirect=Log&logNo=221045219083

크롤링 실습 - 네이버 맞춤법 검사기 / 2017.07.07. 
 >> 내용은 AskDjango에서 제공하는 VOD 및 PDF 자료를 적극 참고했다.... 크롤링에 관심이 있다면 AskDjango VOD 자료를 구독해보는 것도 좋을 것 같다.
 Link : https://blog.naver.com/thnam91?Redirect=Log&logNo=221045805076

크롤링 실습 - 네이버 블로그 / 2017.07.05. 
 >> 예시로는 'askdjango' 검색어를 입력했으며, 그 결과 화면에서 '개발자... 네이버 검색창에 'askdjango' 입력 후 블로 그 검색화면에서 URL은...
 Link : https://blog.naver.com/thnam91?Redirect=Log&logNo=221044591975

Web Crawling / 2017.07.04. 
 >> 적절한 예로 AskDjango에서 제공하는 크롤링 도장깨기 웹페이지가 있다.... import requestsfrom bs4 import BeautifulSouplv1_url = 'https://askdjango.github.io/lv1/'html...
 Link : https://blog.naver.com/thnam91?Redirect=Log&logNo=221043942283

Slack bot으로 메세지 자동으로 보내기 / 2017.07.02. 
 >> post_message('#channelname', 'message', attachments=attachments) 본 내용은 AskDjango에... 자동화에 관심이 있다면 AskDjango VOD 자료를 구독해보는 것도 좋을 것 같다.
 Link : https://blog.naver.com/thnam91?Redirect=Log&logNo=221042243083

크롤링 실습 - 네이버 실시간 검색어 / 2017.07.05. 
 >> text) 본 내용은 AskDjango에서 제공하는 VOD 및 PDF 자료를 적극 참고했다.... 크롤링에 관심이 있다면 AskDjango VOD 자료를 구독해보는 것도 좋을 것 같다.
 Link : https://blog.naver.com/thnam91?Redirect=Log&logNo=221044548892

개발자에게 도움이되는 사이트나 글 / 2017.11.04. 
 >> com/askdjango/vod-django 크롤링자료 http://blog.naver.com/PostView.nhn?blogId=dudwo567890&logNo=220914435973&parentCategoryNo=&categoryNo=39&viewDate...
 Link : https://blog.naver.com/erp2001?Redirect=Log&logNo=221132495962

자동으로 SMS 보내기 / 2017.06.08. 
 >> php', data=data) 본 내용은 AskDjango에서 제공하는 VOD 및 PDF 자료를 적극... 자동화에 관심이 있다면 AskDjango VOD 자료를 구독해보는 것도 좋을 것 같다.
 Link : https://blog.naver.com/thnam91?Redirect=Log&logNo=221024077044

[컨퍼런스 정보] Saturday Azure Live, 1703 (17.03.11) / 2017.03.06. 
 >> 이진석 (AskDjango 운영자, Azure MVP, 어썸마트 CTO) 17:00 ~ 17:50 세션 3: Angular 2 Animation와 함께하는 ASP.NET Core CRUD, 웹 API, Entity Framework 그리고 SQL Server...
 Link : http://harryp.tistory.com/517

TIL - 20180323 / 2018.03.23. 
 >> 구글링 한 것들 gunicorn gracefully reload : askDjango 이진석님께 도움을 받음 http://docs.gunicorn.org/en/stable/faq.html#how-do-i-reload-my-application-in-gunicorn
 Link : https://suwoni-codelab.com/til/2018/03/23/TIL/

자동으로 이메일 보내기 / 2017.06.06. 
 >> ') 본 내용은 AskDjango에서 제공하는 VOD 및 PDF 자료를 적극 참고했다.... 관심이 있다면 AskDjango VOD 자료를 구독해보는 것도 좋을 것 같다.
 Link : https://blog.naver.com/thnam91?Redirect=Log&logNo=221022895051

크롤링 실습 - 굿모니터링 웹페이지 / 2017.07.07. 
 >> strip()) print(cols) 본 내용은 AskDjango에서 제공하는 VOD 및 PDF 자료를 적극... 크롤링에 관심이 있다면 AskDjango VOD 자료를 구독해보는 것도 좋을 것 같다.
 Link : https://blog.naver.com/thnam91?Redirect=Log&logNo=221046183530

2018-02-08 TIL / 2018.02.08. 
 >> Today I Learned What I Did AskDjango Social Login, Logging with Sentry Django... Blog AskDjango 강의노트 정리 완료 To-Do Two Scoops Of Django tourblog...
 Link : https://kirade.github.io/til/2018/02/08/2018-02-08-til/

크롤링 실습 - 멜론 TOP100 / 시대별 차트 / 2017.07.07. 
 >> ', title, '-', artist, song_url) 본 내용은 AskDjango에서 제공하는 VOD 및... 관심이 있다면 AskDjango VOD 자료를 구독해보는 것도 좋을 것 같다.
 Link : https://blog.naver.com/thnam91?Redirect=Log&logNo=221046102121

2018-02-07 TIL / 2018.02.07. 
 >> Today I Learned What I Did AskDjango Static, Media Files, Image Processing... Django AskDjango 강의 수강 노트필기 포스팅 하면서 정리 Reference...
 Link : https://kirade.github.io/til/2018/02/07/2018-02-07-til/

2018-01-11 TIL / 2018.01.11. 
 >> Today I Learned What I Did 폭설로인한 제주코딩캠프 결석 AskDjango youtube를... 8일 ~ 1월 12일 오전 9시 ~ 오후 10시 Reference 제주코딩베이스캠프 AskDjango- youtube
 Link : https://kirade.github.io/til/2018/01/11/2018-01-11-til/

20170304 오늘의 자기계발__계획을 세우는 이유와 의미 / 2017.03.04. 
 >> [프로그래밍] Python - KMOOC (19/60) 31% [프로그래밍] Django - askdjango 실습 - 국회의원 이름 및 ID 크롤링 | CSS 선택할 때 select 함수 활용하기 for member_tag in soup.select('.memberna...
 Link : https://the7mincheol.blog.me/220949806745

[마트,장애] Microsoft AI와 파이썬... / 2018.04.23. 
 >> 파이썬/장고 페이스메이커가 되겠습니다. :-) EP01 (슬라이드 설명편) : https://youtu.be/09fxZbB-aoQ AskDjango VOD ( https://nomade.kr/vod/ ) 를 통해...
 Link : http://topb.cafe24.com/bbs/board.php?bo_table=c139&wr_id=1553

[마트,장애] Microsoft AI와 파이썬... / 2018.04.23. 
 >> 파이썬/장고 페이스메이커가 되겠습니다. :-) EP2 (코드 실전편) : https://youtu.be/fUkBNSlhYj8 AskDjango VOD ( https://nomade.kr/vod/ ) 를 통해 파이썬/장...
 Link : http://topb.cafe24.com/bbs/board.php?bo_table=c139&wr_id=1552

Pillow 라이브러리 / 2017.07.04. 
 >> jpg') 본 내용은 AskDjango에서 제공하는 VOD 및 PDF 자료를 적극 참고했다.... 관심이 있다면 AskDjango VOD 자료를 구독해보는 것도 좋을 것 같다.
 Link : https://blog.naver.com/thnam91?Redirect=Log&logNo=221044025458

파이썬(Python) 학습의 주요분야 및 학습리소스 / 2017.08.10. 
 >> com/groups/askdjango/ - Django의 다양한 개발 사례를 공유 - 오프라인 세미나 개최 2) flask – 경량 파이썬 웹 프레임워크, Easy & Simple - http://flask....
 Link : https://blog.naver.com/bhk1304?Redirect=Log&logNo=221071294533

2018-01-31 TIL / 2018.01.31. 
 >> 적절한 자료형, 리스트 컴프리핸션 등 ) To-Do Two Scoops Of Django AskDjango 강의 수강 노트필기 포스팅 하면서 정리 Reference 파이썬 개념 정리 잘된 글
 Link : https://kirade.github.io/til/2018/01/31/2018-01-31-til/

2018-02-19 TIL / 2018.02.19. 
 >> Today I Learned What I Did AskDjango CBV 강의 수강 이사 준비 시작 To-Do 정리한 내용 포스팅 멀티 쓰레드, 멀티 프로세스, 비동기작업 AskDjango...
 Link : https://kirade.github.io/til/2018/02/19/2018-02-19-til/

Django Project Azure WebApp 배포 + Git 연동 / 2016.11.30. 
 >> 마이그레이션을 진행한다 (migrate, createsuperuser .. 등) * 이 포스트는 AskDjango 세션 정리내용입니다 https://github.com/askdjango/askdjango-demo
 Link : https://blog.naver.com/sdzaq?Redirect=Log&logNo=220874342874

2018-02-06 TIL / 2018.02.06. 
 >> Today I Learned What I Did AskDjango Form Customize, Message Framework Vi... Django AskDjango 강의 수강 노트필기 포스팅 하면서 정리 Reference...
 Link : https://kirade.github.io/til/2018/02/06/2018-02-06-til/

BeautifulSoup4 라이브러리 / 2017.07.05. 
 >> text) 본 내용은 AskDjango에서 제공하는 VOD 및 PDF 자료를 적극... 더 상세한 설명 또는 다양한 파이썬 라이브러리에 관심이 있다면 AskDjango VOD...
 Link : https://blog.naver.com/thnam91?Redirect=Log&logNo=221044229334

2018-02-20 TIL / 2018.02.20. 
 >> Today I Learned What I Did 서울 이사 준비 To-Do 정리한 내용 포스팅 멀티 쓰레드, 멀티 프로세스, 비동기작업 AskDjango 필기 정리 AskDjango CBV 강의...
 Link : https://kirade.github.io/til/2018/02/20/2018-02-20-til/

2018-02-01 TIL / 2018.02.01. 
 >> Today I Learned What I Did Blog AskDjango 필기 노트 정리 서울 방문 제주 코딩캠프 보강 To-Do Two Scoops Of Django AskDjango 강의 수강 노트필기...
 Link : https://kirade.github.io/til/2018/02/01/2018-02-01-til/

[Python] find & CSS Selector을 이용한 크롤링 / 2017.10.26. 
 >> wrap_song_info a[href*=playSong]')for idx, tag in enumerate(tag_list, 1): print(idx, tag.text) 참고 : https://nomade.kr/vod/crawling/125/ AskDjango 이진석님
 Link : https://blog.naver.com/answn3475?Redirect=Log&logNo=221125660580

2018-02-21~22 TIL / 2018.02.22. 
 >> Today I Learned What I Did 이사 및 짐정리 서류, 은행, 동사무소 To-Do 정리한 내용 포스팅 멀티 쓰레드, 멀티 프로세스, 비동기작업 AskDjango CBV 강의...
 Link : https://kirade.github.io/til/2018/02/22/2018-02-21-22-til/

2018-02-04 TIL / 2018.02.04. 
 >> Today I Learned What I Did AskDjango 템플릿 엔진, 필터, 폼 To-Do Two Scoops Of Django AskDjango 강의 수강 노트필기 포스팅 하면서 정리 Reference...
 Link : https://kirade.github.io/til/2018/02/04/2018-02-04-til/

2018-01-30 TIL / 2018.01.30. 
 >> Today I Learned What I Did AskDjango 장고 강의 - 기초편 HTTP Status Code... To-Do AskDjango 강의 수강 노트필기 포스팅 하면서 정리 Reference...
 Link : https://kirade.github.io/til/2018/01/30/2018-01-30-til/

2018-02-05 TIL / 2018.02.05. 
 >> Did AskDjango Csrf, HttpRequest, HttpResponse, Django Form, Validation Two Scoops Of Django ~ ch5 To-Do Two Scoops Of Django AskDjango 강의 수강...
 Link : https://kirade.github.io/til/2018/02/05/2018-02-05-til/

2018-01-29 TIL / 2018.01.29. 
 >> Today I Learned What I Did AskDjango 장고 강의 수강 시작 프로젝트 세팅, URLConf, View, Regular Expression, Model, Migration, Shell To-Do AskDjango...
 Link : https://kirade.github.io/til/2018/01/29/2018-01-29-til/

[Django] Jupyter notebook 사용하기 - Django shell / 2017.02.13. 
 >> NOTEBOOK_KERNEL_SPEC_NAMES = ['Python [Root]'] Thanks to AskDjango - 출처 : https://www.facebook.com/groups/askdjango/permalink/1560400380642166/?pnref=story 공감...
 Link : http://codemath.tistory.com/34

2018-02-23 TIL / 2018.02.23. 
 >> Today I Learned What I Did 이삿짐 정리 Blog Dash 멀티 쓰레드, 멀티 프로세스 비동기 작업 To-Do AskDjango CBV 강의 마무리 Reference
 Link : https://kirade.github.io/til/2018/02/23/2018-02-23-til/

2018-02-24 TIL / 2018.02.24. 
 >> Today I Learned What I Did AskDjango CBV강의 수강 파이썬 차근차근 기초강의 수강 To-Do Reference AskDjango VOD
 Link : https://kirade.github.io/til/2018/02/24/2018-02-24-til/

2018-02-13 TIL / 2018.02.13. 
 >> Today I Learned What I Did Book yield, raise 키워드에 관한 내용 추가 AskDjango CBV 강의 수강 To-Do Two Scoops Of Django tourblog 개발 진행 Reference
 Link : https://kirade.github.io/til/2018/02/13/2018-02-13-til/

2018-02-09 TIL / 2018.02.09. 
 >> Today I Learned What I Did Django Column App Model To-Do Two Scoops Of Django tourblog 개발 진행 Reference AskDjango tourblog
 Link : https://kirade.github.io/til/2018/02/09/2018-02-09-til/

Requests 라이브러리 / 2017.06.08. 
 >> org/post', data=json_str) 본 내용은 AskDjango에서 제공하는 VOD 및 PDF... 관심이 있다면 AskDjango VOD 자료를 구독해보는 것도 좋을 것 같다.
 Link : https://blog.naver.com/thnam91?Redirect=Log&logNo=221024784006

2018-02-03 TIL / 2018.02.03. 
 >> Today I Learned What I Did Two Scoops of django 책 정독 To-Do Two Scoops Of Django AskDjango 강의 수강 노트필기 포스팅 하면서 정리 Reference
 Link : https://kirade.github.io/til/2018/02/03/2018-02-03-til/

2018-02-02 TIL / 2018.02.02. 
 >> Today I Learned What I Did Python OOP, 객체, mro , 클래스 메서드, 스태틱 메서드, 인스턴스 메서드 공부 To-Do Two Scoops Of Django AskDjango 강의 수강...
 Link : https://kirade.github.io/til/2018/02/02/2018-02-02-til/

2.3 피로그래밍 후의 계획 / 2017.02.20. 
 >> https://goo.gl/sB669q 피로그래밍 6기 슬라이드 - https://github.com/askdjango/programming-6th > GitHub 피로그래밍 6기 https://gist.github.com/allieus...
 Link : https://blog.naver.com/scarletbreez?Redirect=Log&logNo=220940213899

20170323 오늘의 자기계발 / 2017.03.23. 
 >> 시퀀스를 생성하는 객체 [프로그래밍] Django - askdjango (3/38) 7% - URLConf and Regular Expression | resume : 맵핑연습#3 차례 (00:44:14~)
 Link : https://the7mincheol.blog.me/220965280771

카카오톡 플러스친구 봇 제작 - GIST 식단표 알림 / 2017.08.02. 
 >> 내용은 AskDjango에서 제공하는 VOD 및 PDF 자료를 적극 참고했다.... 자동화에 관심이 있다면 AskDjango VOD 자료를 구독해보는 것도 좋을 것 같다.
 Link : https://blog.naver.com/thnam91?Redirect=Log&logNo=221064875953

중고나라에 찾는 물건 올라오면 알려주기 / 2017.07.06. 
 >> sleep(60) 본 내용은 AskDjango에서 제공하는 VOD 및 PDF 자료를... 관심이 있다면 AskDjango VOD 자료를 구독해보는 것도 좋을 것 같다.
 Link : https://blog.naver.com/thnam91?Redirect=Log&logNo=221045717832

Django Girls Seoul! 프로그래밍 워크샵에 참여하다! / 2016.11.16. 
 >> gitbook.com/book/djangogirlsseoul/tutorial/details *** 6월 4일 실습영상 : https://www.facebook.com/askdjango/videos/vb.535852453244480/627554657407592/?type=2&theater
 Link : https://blog.naver.com/2002shihoo?Redirect=Log&logNo=220863468052

2017-02-03 (12일차) / 2017.02.03. 
 >> py에 urlpatterns=[] 4) 상위 프로젝트(askdjango) url.py에 새로운 프로젝트 url include 시켜주기 2. 대부분의 언어는 MVC framework (Model View Controller)...
 Link : https://blog.naver.com/davidkim9204?Redirect=Log&logNo=220926355947

크롤링 / 2017.06.14. 
 >> https://m.facebook.com/groups/373189689430865?view=permalink&id=1388215874594903 https://askdjango.github.io/lv1/ https://askdjango.github.io/lv2...
 Link : https://blog.naver.com/timothym?Redirect=Log&logNo=221029045962

[Django] Pycharm에서 Django 실습 - 게시판,댓글... / 2016.05.15. 
 >> 참고 : AskDjango 이진석님 강의 공감 sns 신고 ' 프로그래밍 > Python ' 카테고리의 다른 글 Python - map 함수 (0) 2016.12.27 RSA 암호화 / 복호화 (0) 2016.12.26...
 Link : http://suspected.tistory.com/172

2018-02-25 TIL / 2018.02.25. 
 >> Today I Learned What I Did AskDjango 파이썬 차근차근 기초강의 마무리 To-Do Reference AskDjango VOD
 Link : https://kirade.github.io/til/2018/02/25/2018-02-25-til/

20170312 오늘의 자기계발 / 2017.03.12. 
 >> [프로그래밍] Django - askdjango VOD [프로그래밍] Python - K-MOOC (29/62) 46% - 함수의 호출 방식 - 변수의 범위 : 지역변수, 전역변수 - 재귀함수...
 Link : https://the7mincheol.blog.me/220956084165

Python ::: 경로에서 파일 이름만 추출하기 / 2017.03.28. 
 >> import osfilepath = r'E:\Google_Drive\lawjmc\nomade\django\askdjango\gdplev.xls'filename = os.path.basename(filepath)
 Link : https://the7mincheol.blog.me/220969532801

내가 Django를 처음 시작했던 방법 / 2017.05.24. 
 >> 한참 지나서 다시 Django를 시작하는 지금 보고있는 것은 AskDjango 온라인 강의... AskDjango의 큰 장점은 직접 질문을 할 수 있다는 점 그리고 비용이 생각보다...
 Link : https://dojunblog.wordpress.com/2017/05/24/%EB%82%B4%EA%B0%80-django%EB%A5%BC-%EC%B2%98%EC%9D%8C-%EC%8B%9C%EC%9E%91%ED%96%88%EB%8D%98-%EB%B0%A9%EB%B2%95/

Django Multiple Database / 2017.03.28. 
 >> 이전에 Ask DJango를 통해 질문 : https://www.facebook.com/groups/askdjango/permalink/1609471845735019/?comment_id=1609486389066898&ref=notif&notif_t=group_comment...
 Link : http://dooha.tistory.com/15

Google Firebase를 통한 로그인 처리 관련 / 2017.12.03. 
 >> com/askdjango-android/firebase-auth-%EC%99%80-%ED%95%A8%EA%BB%98-%EA%B5%AC%EA%B8%80-%ED%8E%98%EC%9D%B4%EC%8A%A4%EB%B6%81-%EB%A1%9C%EA%B7%B8%EC%9D%B8...
 Link : https://blog.naver.com/jjusik2?Redirect=Log&logNo=221154408195

Python-국회 사이트 국회의원 목록 크롤링 / 2016.12.26. 
 >> 부쩍 국회에 관심이 많아져 국회의원 목록 크롤링을 해보겠습니다.AskDjango님께 도움 받았습니다. 크롤링이 뭐냐...
 Link : https://blog.naver.com/nagano_i?Redirect=Log&logNo=220895024592

펌 IT 인터넷 0318 / 2017.03.19. 
 >> 0 Askdjango) 파이썬 기본문법 정리 14) 파이썬 대소.. jordan17님 | 2017.... 18 16:56 0 Askdjango) 파이썬 기본문법 정리 13) 파이썬 빌트.. jordan17님...
 Link : http://happytime3.tistory.com/2143

숭의동빌라매매 ★벌써잔여!! / 2017.05.08. 
 >> Contribute to askdjango-4th development by creating an account on GitHub. 한 때 불화설이 불거지기도 했던 신화는9 장고 차근차근 시작하기 4기. 트위치 유저들 화이팅.
 Link : https://blog.naver.com/brce32777?Redirect=Log&logNo=221001214323

펌 IT 인터넷 0313 / 2017.03.14. 
 >> 0 Askdjango) 파이썬 기본문법 정리 7) 파이썬 빌트인.. jordan17님 | 2017.03.13 21:32 0 The lecture about big data. 키약님 | 2017.03.13 21:29 The lecture about Big...
 Link : http://happytime3.tistory.com/1996

펌 IT 인터넷 0312 / 2017.03.12. 
 >> 애즈락 메인보드 드라이버 - asrock 설치 ¸¸님 | 2017.03.12 20:58 0 연결 리스트 - 단순 연결 리스트 모노모님 | 2017.03.12 20:58 0 mega 사용법 (Cloud) 2...
 Link : http://happytime3.tistory.com/1916

펌 IT 인터넷 0324 / 2017.03.24. 
 >> JazzNumbe..님 | 2017.03.23 22:53 0 LED튜닝 조립컴퓨터~(부산해운대) 디카이오스님 | 2017.03.23 22:47 0 Askdjango) 파이썬 기본문법 정리 16) 파이썬 인코.....
 Link : http://happytime3.tistory.com/2476

펌 IT 인터넷 0316 / 2017.03.16. 
 >> kaki104님 | 2017.03.15 23:33 0 Askdjango) 파이썬 기본문법 정리 7) 파이썬 빌트인.. jordan17님 | 2017.03.15 23:29 0 애드센스 2차 승인 사이트 검토 중 해결 방법 내...
 Link : http://happytime3.tistory.com/2093
```



이번엔 여기까지 : )