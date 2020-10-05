```markdown
# Django
- Framework
- MTV pattern: 다른 웹 프레임워크들은 Model / View / Controller
- variable routing
- django namespace: app_name, url_name, template_name
- template inheritance : base.html

# Django model
- Database: SQL
- ORM
- Migrations: 모델에 변경사항이 생기면 makemigrations, migrate
- DB API: 장고가 기본 제공 ex. Article.objects.QuerySet API
- QuerySet API: all(), filter(), get() 등
- CRUD: 생성, 조회, 수정, 삭제

# etc.
- Virtual Environment: 독립적 가상환경
- 패키지 관리: pip install -r requirements.txt
- django fixtures: django가 DB로부터 import할 수 있는 데이터에 대한 모음 -> 보통 초기 데이터를 import할 때 사용한다.
```



# Django Form, Static, Media

- 데이터 요청이 들어오는 순서

1. `urls. py`
2. `views.py`
3. Template `<filename>.html`

(필요에 따라 models.py도 작성, 설정)



## Form

- Form은 django 프로젝트의 주요 유효성 검사 도구들 중 하나이며, 공격 및 우연한 데이터 손상에 대한 중요한 방어수단.
- django는 Form에 관련된 작업의 아래 세 부분을 처리해 준다.
  1. 렌더링을 위한 데이터 준비 및 재구성
  2. 데이터에 대한 HTML forms 생성
  3. 클라이언트로부터 받은 데이터 수신 및 처리



## Form Class

- django Form 관리 시스템의 핵심
- form 내 field, field 배치, 디스플레이, widget, label, 초기값, 유효하지 않은 field에 관련된 에러메시지를 결정
- django는 사용자의 데이터를 받을 때 해야 할 과중한 작업(데이터 유효성 검증, 필요시 입력된 데이터 검증 결과 재출력, 유효한 데이터에 대해 요구되는 동작 수행 등)과 반복 코드를 줄여 준다.



## GET & POST

Method에는 GET과 POST가 있다.

- GET: 데이터를 조회할 때만 사용. 검색을 했을 때 검색어가 남는 것처럼, url에 쿼리스트링이 담겨져서 넘어간다. (cf. 우리가 로그인을 한다고 아이디, 비밀번호가 url에 남지는 않는다. 이 경우는 POST 방식을 사용했기 때문이다.)
- POST: 그 외 나머지는 모두 POST 사용. DB에 조작을 가하는 경우.
  - 필수: `csrf_token` POST의 짝꿍이다.



## ModelForm

```python
class ArticleForm(forms.ModelForm):

    class Meta: # 메타 데이터: 정보에 대한 정보, 클래스에 대한 정보
        model = Article
        fields = '__all__'
```

Article Model에서 모든 정보를 불러 올거야!!



## new(GET) + create(POST) == CREATE

```python
# articles/views.py

# def new(request): # GET 궁극적으로는 같은 목적(Create)
#     form = ArticleForm()
#     context = {
#         'form': form,
#     }
#     return render(request, 'articles/new.html', context)


# def create(request): # POST 궁극적으로는 같은 목적(Create)
#     title = request.POST.get('title') 
#     content = request.POST.get('content')
#     article = Article(title=title, content=content)
#     article.save()
#     return redirect('articles:detail', article.pk)

def create(request):
    # http method가 POST일 때
    if request.method == 'POST':
        form = ArticleForm(request.POST)
        # 유효성 검증
        if form.is_valid():
            article = form.save()
            return redirect('articles:detail', article.pk)
    # http method가 POST가 아닌 다른 method일 때
    else: # new의 역할(GET)
        form = ArticleForm()
    context = {
        # 1. is_valid에서 내려온 form: 에러메시지를 포함
        # 2. else 구문에서 내려온 form
        'form': form,
    }
    return render(request, 'articles/create.html', context)
```

- if문은 왜 GET이 아닌 POST로 먼저 거르는가?

  : POST일 때와 POST가 아닐 때(GET, PUT 등..)로 나누어야 하기 때문이다.

- 왜 context의 들여쓰기가 if-else와 같은 선상에서 이루어졌는가?

  : 만약 유효성 검사에서 통과하지 못한 상황에서 context가 없으면 마지막에 return해줄 값이 없기 때문이다.



## Form이 invalid할 때 error_messages

```python
# articles/forms.py
from django import forms
from .models import Article


class ArticleForm(forms.ModelForm):
    title = forms.CharField(
        label='제목',
        widget=forms.TextInput(attrs={
            'class': 'my-title',
            'placeholder': 'Enter the title',
        })
    )
    content = forms.CharField(
        label='내용',
        widget=forms.Textarea(attrs={
            'class': 'my-content',
            'rows': 5,
            'cols': 50,
        }),
        error_messages={
            'required': '내용 넣어라-_-;',
        }
    )

    class Meta: # 메타 데이터: 정보에 대한 정보, 클래스에 대한 정보
        model = Article
        fields = '__all__'
```

- `error_messages`[¶](https://docs.djangoproject.com/en/3.1/ref/forms/fields/#error-messages)



## ModelForm Class

- Model을 통해 Form Class를 만들 수 있는 Helper
- 일반 Form Class와 완전히 같은 방식(객체 생성)으로 view에서 사용 가능
- Meta Class
  - Model에 대한 정보를 작성하는 곳
  - 해당 model에 정의한 field 정보를 Form에 적용하기 위해 사용



## Form & ModelForm

- Form

  - 어떤 model에 저장해야 하는지 알 수 없으므로 유효성 검사 이후 cleaned_data 딕셔너리를 생성

    (cleaned_data 딕셔너리에서 데이터를 가져온 후 .save() 호출)

  - model에 연관되지 않은 데이터를 받을 때 사용

- ModelForm

  - django가 해당 model에서 양식에 필요한 대부분의 정보를 이미 정의
  - 어떤 레코드를 만들어야할지 알고 있으므로 바로 .save() 호출 가능



## UPDATE

- `NoReverseMatch`: 무조건 url tag만 보면 된다.

```python
# articles/views.py

def update(request, pk): # 몇 번째 글이 업데이트 돼야 하는지 알아야 하기 때문에 pk값도 같이 받아온다.
    # 수정을 하려면 수정할 대상이 있어야 한다.
    article = Article.objects.get(pk=pk)
    if request.method == 'POST': # update
        form = ArticleForm(request.POST, instance=article)
        if form.is_valid():
            form.save()
            return redirect('articles:detail', article.pk)
    else: # edit
        # Creating a form to change an existing article.
        form = ArticleForm(instance=article)
    context = {
        'form': form,
        'article': article, #article 빼먹지 말기! 필수!!
    }
    return render(request, 'articles/update.html', context)
```

```html
<!-- articles/update.html -->
{% extends 'base.html' %}

{% block content %}
  <h1 class="text-center">UPDATE</h1>
  <!-- 현재 있는 곳으로 (같은 위치로) 다시 돌아온다. -->
  <form action="" method="POST">
    {% csrf_token %}
    {{ form.as_p}}
    <input type="submit">
  </form>
  <hr>
  <a href="{% url 'articles:detail' article.pk %}">[BACK]</a>
{% endblock content %}
```



```python
# Creating a form to change an existing article.
>>> article = Article.objects.get(pk=1)
>>> form = ArticleForm(instance=article)
```

https://docs.djangoproject.com/en/3.1/topics/forms/modelforms/

```python
# Create a form to edit an existing Article, but use
# POST data to populate the form.
>>> a = Article.objects.get(pk=1)
>>> f = ArticleForm(request.POST, instance=a)
>>> f.save()
```

- The `save()` method[¶](https://docs.djangoproject.com/en/3.1/topics/forms/modelforms/#the-save-method)



## DELETE

```python
# articles/views.py

def delete(request, pk):
    article = Article.objects.get(pk=pk)
    # GET 방식일 때는 동작해서는 안 된다.
    if request.method == 'POST':
        article.delete()
        return redirect('articles:index')
    return redirect('articles:detail', article.pk)
```

```html
<!-- articles/detail.html -->

<form action="{% url 'articles:delete' article.pk %}" method="POST">
    {% csrf_token %}
    <input type="submit" value="DELETE">
</form>
```



## 커스텀을 위해 form.as_p 쪼개기

- `Rendering fields manually`[¶](https://docs.djangoproject.com/en/3.1/topics/forms/#rendering-fields-manually)

```html
  <form action="{% url 'articles:create' %}" method="POST" enctype="multipart/form-data">
    {% csrf_token %}
    <div>
      {{ form.title.errors }}
      {{ form.title.label_tag }}
      {{ form.title }}
    </div>
    <div>
      {{ form.content.errors }}
      {{ form.content.label_tag }}
      {{ form.content }}
    </div>
    <input type="submit">
  </form>
```

```html
  <form action="{% url 'articles:create' %}" method="POST" enctype="multipart/form-data">
    {% csrf_token %}
    <div class="form-group">
      {% for field in form %}
        {{ field.errors }}
        {{ field.label_tag }}
        {{ field }}
      {% endfor %}
    </div>
    <input type="submit">
  </form>
```



## View Decorators

- `View decorators`[¶](https://docs.djangoproject.com/en/3.1/topics/http/decorators/#module-django.views.decorators.http)

- Decorator (데코레이터)

  - 어떤 함수에 기능을 추가하고 싶을 때, 해당 함수를 수정하지 않고 기능만 **확장**하게 해주는 함수
  - django는 다양한 기능을 지원하기 위해 view 함수에 적용할 수 있는 여러 데코레이터를 제공

- Allowed HTTP methods

  - ```python
    from django.views.decorators.http import require_http_methods
    
    @require_http_methods(["GET", "POST"])
    ```
  
  - 요청 method에 따라 view 함수에 대한 액세스를 제한
  - 요청 조건을 충족시키지 못하면 `HttpResponseNotAllowed`를 return
  - `require_http_methods()`, require_GET(), `require_POST()`, `require_safe()`



```python
# articles > views.py

@require_http_methods(['GET', 'POST'])
def create(request):

@require_http_methods(['GET', 'POST'])
def update(request, pk):

@require_POST
def delete(request, pk):
    article = Article.objects.get(pk=pk)
    article.delete()
    return redirect('articles:index')
```