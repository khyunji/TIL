# 201006 drf-yasg

### drf-yasg란?

```markdown
- API 페이지의 각 기능, url을 편하게 볼 수 있도록 설정하기 위한 API 문서화 도구
- Swagger/OpenAPI 2.0 specifications from a Django Rest Framework API
  Swagger/OpenAPI 2.0 기반으로 작성된 DRF용 문서 자동화 라이브러리
```



### 설치 과정

```bash
$ python -m venv venv
$ source venv/Scripts/activate
```

가상환경 설정한 뒤 실행하고, pip list가 비어있는지 확인하기



```bash
$ pip install django django-seed django-extensions djangorestframework==3.11
```

django, django-seed, django-extensions, djangorestframework 설치



drf-yasg에서 djangorestframework 3.12 버전을 지원하고는 있지만 호환 이슈가 있기 때문에
만약 djangorestframework 3.12 버전이 이미 설치되어 있다면

```bash
$ pip uninstall djangorestframework
```

위 명령어를 통해 기존 버전 삭제 후 재설치



```bash
$ pip install -U drf-yasg
```

drf-yasg Github에서 table of content 확인하며 사용법 익히기

https://github.com/axnsan12/drf-yasg#quickstart



### 설치 이후의 설정

`settings.py`

```python
INSTALLED_APPS = [
   ...
   'drf_yasg',
   ...
]
```



`urls.py`

```python
...
from rest_framework import permissions
from drf_yasg.views import get_schema_view
from drf_yasg import openapi

...

schema_view = get_schema_view(
   openapi.Info(
      title="Music API",
      default_version='v1',
#       description="Test description",
#       terms_of_service="https://www.google.com/policies/terms/",
#       contact=openapi.Contact(email="contact@snippets.local"),
#       license=openapi.License(name="BSD License"),
   ),
#    public=True,
#    permission_classes=(permissions.AllowAny,),
)

app_name='music'
urlpatterns = [
    path('redocs/', schema_view.with_ui('redoc'), name='api_docs'),
    path('swagger/', schema_view.with_ui('swagger'), name='api_swagger'),
    ...
]
```