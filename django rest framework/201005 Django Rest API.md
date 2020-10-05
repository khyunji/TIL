# 201005 Django Rest API



## API_Server

- API (Application Programming Interface): 응용 프로그램 프로그래밍 인터페이스
: 응용 프로그램에서 사용할 수 있도록, 운영 체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만든 인터페이스



" 프로그래밍을 통해 요청에 **RESTful한 방식**으로 **JSON을 응답**하는 서버를 만들자! "



## RESTful API

- REpresentational State Transfer
  : 월드 와이드 웹과 같은 분산 하이퍼미디어 시스템을 위한 소프트웨어 아키텍처의 한 형식

=> "자원과 주소를 어떻게 지정할 것인지에 대한 방법론"
=> "개발자들끼리의 약속이기 때문에 지켜줘야 한다." Youtube, 카카오, 네이버 각자 API가 다 다르다면? 그걸 익히고 개발하는데 시간이 압도적으로 늘어날 것이다.



- REST 구성
  - 자원: URI
  - 행위: HTTP Method (GET/POST/PUT/DELETE)
  - 표현: Representations



- REST 중심 규칙
  1. URI는 정보의 자원을 표현해야 한다.
  2. 자원에 대한 (어떠한) 행위는 HTTP Method로 표현한다.



- URL(Uniform Resource Locator): 파일 식별자

  : "네트워크 상에서 지원이 어디에 있는지를 알려주기 위한 규약"

- URI(Uniform Resource Identifier): 통합 자원 식별자

  : "인터넷에 있는 자원을 나타내는 유일한 주소"



## HTTP Method

- HTTP (Hyper Text Transfer Protocol) 기본 속성
  - Stateless: 상태 정보가 저장되지 않음
  - Connectless: 서버에 요청하고 응답한 이후에 연결은 끊어짐



- HTTP Method
  - GET: 지정 리소스의 표시를 요청하며 오직 데이터를 받지만 함
  - POST: 클라이언트 데이터를 서버로 보냄
  - PUT / PATCH: 서버로 보낸 데이터를 저장 / 지정 리소스의 부분만을 수정
  - DELETE: 지정 리소스를 삭제
- 표현 (Representations)



- JSON(JavaScript Object Notation): JavaScript 객체 표기법

  - 언어 독립적
  - 변환이 쉽다.
  - 우리는 데이터를 제공해줄테니까 알아서 받아와서 사용해!




## Django REST Framework

- `serialization` (직렬화): 다른 환경, 포맷 변환 -> 어떤 데이터 타입을 JSON 등의 데이터 타입으로 변경하는 작업
- Django vs DRF

|          |  Django   |       DRF       |
| :------: | :-------: | :-------------: |
| Response |   HTML    |      JSON       |
|  Model   | ModelForm | ModelSerializer |

  

## 실습

> 00_api_project



```bash
# 기본 설정하고 시작

python -m venv venv
pip install -r requirements.txt
python manage.py makemigrations
mython manage.py migrate
```



- Django seed -> 더미 데이터 만들어 준다. 참고: https://github.com/Brobin/django-seed


```bash
# 랜덤 데이터 20개 형성

$ python manage.py seed articles --number=20
```

runserver한 뒤 http://127.0.0.1:8000/articles/html/ 에서 검사 (Ctrl+Shift+I) > Network 탭에서 새로고침 > Headers 확인 시 `Content-Type: text/html; charset=utf-8`로 뜬다.

하지만 앞으로는 `JSON` 파일로 응답을 줄 것이다!



```python
# articles > views.py 실험

def article_list_json_1(request):
    article = Article.objects.get(pk=1)
    
    test = {
        'id': article.id,
        'title': article.title,
        'content': article.content,
        'created_at': article.created_at,
        'updated_at': article.updated_at,
    }
        
    return JsonResponse(test)
```

이렇게 수정해본다면? `Content-Type: application/json`으로 뜬다.



```python
# articles > views.py

def article_list_json_1(request):
    articles = Article.objects.all()
    articles_json = []

    for article in articles:
        articles_json.append({
            'id': article.id,
            'title': article.title,
            'content': article.content,
            'created_at': article.created_at,
            'updated_at': article.updated_at,
        })
    return JsonResponse(articles_json)
```

다시 돌아와서, 이 코드는 오류가 난다. 
오류 나는 이유: 데이터가 {'foo': 'bar'} 같은 딕셔너리 형태로 들어오지 않기 때문에, 강제로 json으로 변환시키려 하면 오류가 난다.

#### Serializing non-dictionary objects[¶](https://docs.djangoproject.com/en/3.1/ref/request-response/#serializing-non-dictionary-objects)

In order to serialize objects other than `dict` you must set the `safe` parameter to `False`:

```
>>> response = JsonResponse([1, 2, 3], safe=False)
```

Without passing `safe=False`, a [`TypeError`](https://docs.python.org/3/library/exceptions.html#TypeError) will be raised.



#### 해결방안

```python
# articles > views.py

def article_list_json_1(request):
    articles = Article.objects.all()
    articles_json = []

    for article in articles:
        articles_json.append({
            'id': article.id,
            'title': article.title,
            'content': article.content,
            'created_at': article.created_at,
            'updated_at': article.updated_at,
        })
    return JsonResponse(articles_json, safe=False)
```

`safe=False`를 추가하여 문제 해결!!



## Serializing Data

- `Serializing data`[¶](https://docs.djangoproject.com/en/3.1/topics/serialization/#serializing-data)

```python
# articles > views.py

from django.core import serializers

def article_list_json_2(request):
    articles = Article.objects.all()
    data = serializers.serialize("json", articles)
    return ??? # html 파일이 아니기 때문에 render는 아니다.. 그렇다면 뭘 return 해주지?
```

- `HttpResponse` objects[¶](https://docs.djangoproject.com/en/3.1/ref/request-response/#httpresponse-objects)

```python
from django.http import HttpResponse

return HttpResponse(data, content_type='application/json')
```

article에 해당하는 필드를 우리가 정의하지 않았기 때문에 `article_list_json_2`의 코드길이가 더 짧다. 우리가 받아온 articles라는 쿼리셋을 넣었을 뿐인데 Django에서 알아서 serialization 했다.



- `article_list_json_1`: 코드가 길지만 원하는 정보만 취사선택하기 쉽다.

- `article_list_json_2`: 모든 정보가 다 넘어오기 때문에 customize할 때마다 매번 filed를 설정해주기 귀찮다.

  ```python
  def article_list_json_2(request):
      articles = Article.objects.all()
      data = serializers.serialize("json", articles, fields=('id', 'title', 'content',))
      # print(data)
      # print(type(data)) --> str
      return HttpResponse(data, content_type='application/json')
  ```



- content_type
  - **MIME 타입**이란 클라이언트에게 전송된 문서의 다양성을 알려주기 위한 메커니즘
  - 브라우저들은 리소스를 내려받았을 때 해야 할 기본 동작이 무엇인지를 결정하기 위해 대게 MIME 타입을 사용
  - https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/MIME_types



## Django REST Framework

- API 개발을 위한 도움을 주는 프레임워크
- 앞으로는 DRF라는 프레임워크를 통해서 조작한 다음에 JSON을 응답할 것이다.



- [Installation](https://www.django-rest-framework.org/#installation)

```bash
$ pip install djangorestframework
```

Add `'rest_framework'` to your `INSTALLED_APPS` setting.

```python
# settings.py

INSTALLED_APPS = [
    ...
    'rest_framework',
]
```



```python
# articles > serializers.py 생성

from rest_framework import serializers
from .models import Article

class ArticleSerializer(serializers.ModelSerializer):
    
    class Meta:
        model = Article
        fields = ('id', 'title', 'content', 'created_at', 'updated_at',)
```

ModelForm 정의하는 방식과 똑같다.



```python
# articles > views.py

from rest_framework.decorators import api_view # 필수적으로 넣어줘야 한다!!
from rest_framework.response import Response
from articles.serializes import ArticleSerializer


@api_view(['GET'])
def article_list_json_3(request):
    # article = Article.objects.get(pk=1)
    article = Article.objects.all()
    serializer = ArticleSerializer(article, many=True) # many=True 언제 넣는가? 데이터가 한 개가 아닐 때! 쿼리셋을 serializer에 삽입할 때 many=True 작성
    return Response(serializer.data)
```



# CRUD 구현

## READ

```python
# articles > serializers.py 생성

from rest_framework import serializers
from .models import Article

class ArticleSerializer(serializers.ModelSerializer):
    
    class Meta:
        model = Article
        fields = ()
```



```bash
$ python manage.py makemigrations
$ python manage.py migrate
$ python manage.py seed articles --number=20
```



```python
# articles > serializers.py 생성

from rest_framework import serializers
from .models import Article

class ArticleSerializer(serializers.ModelSerializer):
    
    class Meta:
        model = Article
        fields = ('id', 'title', 'content',)
```

```python
# articles > urls.py

from django.urls import path
from . import views 


urlpatterns = [
    path('', views.article_list),
    path('<int:article_pk>', views.article_detail),
]
```

```python
# articles > views.py

from django.shortcuts import get_object_or_404

from rest_framework.decorators import api_view
from rest_framework.response import Response

from .serializers import ArticleSerializer
from .models import Article

# Create your views here.
# DRF에서는 이 데코레이터를 안 쓰면 에러가 난다.
# 괄호만 열어놓으면 GET이 default 값이다!
@api_view(['GET'])
def article_list(request):
    #1. 모델에서 데이터를 가져온다.
    articles = Article.objects.all()

    #2. 모델에서 가져온 데이터를 Serializing 한다.
    serializer = ArticleSerializer(articles, many=True)

    #3. 응답해준다.
    return Response(serializer.data)


@api_view(['GET'])
def article_detail(request, article_pk):
    #1. 모델에서 데이터를 가져온다.
    articles = get_object_or_404(Article, pk=article_pk)

    #2. 모델에서 가져온 데이터를 Serializing 한다.
    serializer = ArticleSerializer(articles)

    #3. 응답해준다.
    return Response(serializer.data)
```

여기까지 코드 작성한 뒤 runserver하면 잘 돌아간다. 
다만 수정하고 싶은 내용! `index 페이지`(Article List)와 `detail 페이지` 간의 차이가 없다.



#### 해결방안

```python
# articles > serializers.py

from rest_framework import serializers
from .models import Article

class ArticleListSerializer(serializers.ModelSerializer):
    
    class Meta:
        model = Article
        fields = ('id', 'title',) # index


class ArticleSerializer(serializers.ModelSerializer):
    
    class Meta:
        model = Article
        fields = ('id', 'title', 'content', 'created_at', 'updated_at',)
```

```python
# articles > views.py

from .serializers import ArticleSerializer, ArticleListSerializer

@api_view(['GET'])
def article_list(request):
    #1. 모델에서 데이터를 가져온다.
    articles = Article.objects.all()

    #2. 모델에서 가져온 데이터를 Serializing 한다.
    # ArticleListSerializer으로 수정
    serializer = ArticleListSerializer(articles, many=True)

    #3. 응답해준다.
    return Response(serializer.data)
```

이렇게 수정하고 나면 `index 페이지` (Article List) 에서는 id와 title에 대한 정보만 나타나고,
`detail 페이지`에서는 자세한 정보 (id, title, content, created_at, updated_at)가 나타난다. 



## CREATE

```python
# articles > urls.py

path('create/', views.create_article),
```

```python
# articles > views.py

@api_view(['POST'])
def create_article(request):
    # print(request.data)
    serializer = ArticleSerializer(data=request.data)
    if serializer.is_valid(raise_exception=True):
        serializer.save()
        return Response(serializer.data)
```

- [Saving instances](https://www.django-rest-framework.org/api-guide/serializers/#saving-instances)

- [ValidationError](https://www.django-rest-framework.org/api-guide/exceptions/#validationerror)

- [Status Codes](https://www.django-rest-framework.org/api-guide/status-codes/#status-codes)



## DELETE

```python
# articles > urls.py

    path('<int:article_pk>/delete/', views.delete_article),
```

```python
# articles > views.py

@api_view(['DELETE'])
def delete_article(request, article_pk):
    article = get_object_or_404(Article, pk=article_pk)
    article.delete()
    return Response({ 'id': article_pk }, status=status.HTTP_204_NO_CONTENT)
```



## UPDATE

```python
# articles > urls.py

    path('<int:article_pk>/update/', views.update_article),
```

```python
# articles > views.py

@api_view(['PUT'])
def update_article(request, article_pk):
    # print(request.data)
    article = get_object_or_404(Article, pk=article_pk)
    serializer = ArticleSerializer(article, data=request.data)
    if serializer.is_valid(raise_exception=True):
        serializer.save()
        return Response(serializer.data, status=status.HTTP_201_CREATED)
```



## 하나로 다 합쳐버리기

```python
# articles > urls.py

from django.urls import path
from . import views 


urlpatterns = [
    # path('', views.article_list),
    # path('create/', views.create_article),
    # path('<int:article_pk>', views.article_detail),
    # path('<int:article_pk>/delete/', views.delete_article),
    # path('<int:article_pk>/update/', views.update_article),

    path('', views.article_list_create),
    path('<int:article_pk>/', views.article_detail_update_delete),
]
```



```python
# articles > views.py

from django.shortcuts import get_object_or_404

from rest_framework.decorators import api_view
from rest_framework.response import Response
from rest_framework import status

from .serializers import ArticleSerializer, ArticleListSerializer
from .models import Article


# Create your views here.
# DRF에서는 이 데코레이터를 안 쓰면 에러가 난다.
# 괄호만 열어놓으면 GET이 default 값이다!
@api_view(['GET', 'POST'])
def article_list_create(request):
    if request.method == 'GET':
        #1. 모델에서 데이터를 가져온다.
        articles = Article.objects.all()
        #2. 모델에서 가져온 데이터를 Serializing 한다.
        serializer = ArticleListSerializer(articles, many=True)
        #3. 응답해준다.
        return Response(serializer.data)
    else:
        serializer = ArticleSerializer(data=request.data)
        if serializer.is_valid(raise_exception=True):
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)


@api_view(['GET', 'PUT', 'DELETE'])
def article_detail_update_delete(request, article_pk):
    article = get_object_or_404(Article, pk=article_pk)
    if request.method == 'GET':
        # 모델에서 가져온 데이터를 Serializing 한다.
        serializer = ArticleSerializer(article)
        # 응답해준다.
        return Response(serializer.data)
    elif request.method == 'PUT':
        serializer = ArticleSerializer(article, data=request.data)
        if serializer.is_valid(raise_exception=True):
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)
    else:
        article.delete()
        return Response({ 'id': article_pk }, status=status.HTTP_204_NO_CONTENT)
```



## Comment

- db.sqlite3 파일 삭제
- migrations 파일 삭제

```python
# articles > models.py

class Comment(models.Model):
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    article = models.ForeignKey(Article, on_delete=models.CASCADE)
```

- makemigrations, migrate 작업
  (안 될 경우, 터미널 완전히 종료했다가 다시 시도해보기!!)



- 더미 데이터 생성

```bash
$ python manage.py seed articles --number=20
```



```python
# articles > urls.py

    path('<int:article_pk>/comments/', views.create_comments),
```

```python
# articles > serializers.py

from .models import Article, Comment

class CommentSerializer(serializers.ModelSerializer):

    class Meta:
        model = Comment
        fields = ('id', 'content', 'article',)
```

```python
# articles > views.py

from .serializers import ArticleSerializer, ArticleListSerializer, CommentSerializer

@api_view(['POST'])
def create_comments(request, article_pk):
    article = get_object_or_404(Article, pk=article_pk)
    serializer = CommentSerializer(data=request.data)
    if serializer.is_valid(raise_exception=True):
        serializer.save(article=article)
        return Response(serializer.data, status=status.HTTP_201_CREATED)
```