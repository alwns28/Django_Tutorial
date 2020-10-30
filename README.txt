이 디렉토리는 장고를 연습하기 위한 폴더입니다. 
----------------------------------------------------------------------------------------------------------
1. Introduction

먼저 장고를 만들기 전에 우리는 가상환경을 만들어야 한다. 만드는 이유는 다음과 같다. 
만약 기존에 쓰고있던 장고의 버전이 1.0 이였는데 버전을 2.0으로 올려야하는 경우 어떻게 할 것인가? 1.0을 지우고 2.0을 만들면 되지 않을까 하는 생각이 들수도 있는데 그러면 기존 1.0을 사용하던 사용자들은 어떻게 처리해야 하는가? 이를 해결하기 위해 가상 환경을 만들어두고 각각 다음과 같이 버전을 설정해주면 된다. 

    1. 가상환경 1 : version 1.0
    2. 가상환경 2 : version 2.0 

이와 같이 조정하면 새로운 버전이 나와도 크게 부담 되지 않는다. 가상환경 셋팅 방법은 다음과 같다.

    1. pip install virtualenv              // 가상환경 설치
    2. virtualenc [가상환경 이름]           // 가상환경 생성
    3. [가상환경 이름]\scripts\activate     // 가상환경 접근
    4. deactivate                          // 가상환경 나가기

이렇게 접근하면 다음 명령어를 통해서 장고를 설치하면 된다. 

            - pip install django                      // 장고 설치 명령어
            - python -m django --version              // 장고 버전 확인 명령어

그러면 버전이 뜨게 되는데 이 문서를 작성할 당시에는 장고 버전이 3.1.2 이다. 지금 설치한 장고는 가상환경에서 설치한 것이기 때문에 가상환경에서 나가면 장고 버전을 볼 수 없게 된다.  
----------------------------------------------------------------------------------------------------------
2. django

이제 본격적으로 장고를 만들어보자. 먼저 장고의 기본 틀을 만들어주어야 한다. 이는 간단하게 다음 명령어를 통해서 만들수 있다.

            - django-admin startproject [폴더명]       // 장고 기본 틀 설치 

이렇게 만들어진 파이썬 코드들을 수정해 하면서 장고를 사용하면 된다. 먼저 수정해야 될 것은 위 명령어에 있는 [폴더명]에 있는 urls.py의 urlpatterns 변수를 수정해야 한다. 이때 사용되는 라이브러리는 다음과 같다. 

            - from django.contrib import admin
            - from django.urls import path
            - from django.conf.urls import include
            - from django.views.generic import RedirectView
            - from django.conf import settings
            - from django.conf.urls.static import static

그리고 변수에 다음을 추가하면 된다. 

    - [리스트 안]
            - path('catalog/', include("catalog.urls")),            // catalog 라는 폴더가 있어야 한다. 
            - path('', RedirectView.as_view(url="/catalog/"))       // 기본 url만 검색해서 들어오면 /catalog/로 들어오게 한다. 

    - [리스트 밖]
        - + static(settings.STATIC_URL, document_root=settings.STATIC_ROOT) // png 이미지, 텍스트파일등 요청된 파일을 그대로 전달할때 static 사용

위를 보면 catalog 라는 폴더도 장고의 형식에 맞게 만들어 주어야 한다. 이를 위해서는 cmd창에 다음과 같은 명령어를 입력해 주면 된다.

            - python manage.py startapp catalog

그리고 urls.py 를 만들어서 다음 코드를 입력한다. 

            '''
            from django.shortcuts import render

            # Create your views here.
            def index(request):
                return render(request, "index.html")
            '''

그리고 views.py에 다음 코드를 추가한다.

            '''
            from django.urls import path
            from catalog import views

            urlpatterns = [
                path('', views.index, name='index'),
            ]
            '''

이렇게만 하면 아마 에러가 뜰것이다. 그 이유는 우리는 index.html 파일을 아직 안만들었기 때문이다. 이제 catalog/templates 폴더를 만들어서 index.html을 다음과 같이 만든다. 

            - <h1> 어서오세요 </h1>

이렇게 만든 catalog에 대한 것들을 manage.py에 적용해야한다. 이는 manage.py의 INSTALLED_APPS 변수에 다음을 추가하면 된다. 

            - "catalog.apps.CatalogConfig"

이때 INSTALLED_APPS는 장고가 신경써야 할 앱의 목록을 의미한다. 여기까지 잘 수정되었으면 cmd 창에 다음 명령어를 입력한다. 

            - python manage.py makemigrations           // DB 모델의 변경점을 확인하는 역할
            - python manage.py makemigrate              // DB 모델의 변경점을 DB에 반영

그러면 우리가 수정한 장고 파일들을 적용하게되고 마지막으로 다음 명령어를 통해 서버를 작동시키면 웹 사이트를 만들게 되는 것이다. 

            - python manage.py runserver

여기까지가 장고 기본 틀, 서버 셋팅과 아주 간단한 예시정도이다.