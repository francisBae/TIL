# django 구조

<br>
<p align="center">
<img src="https://wikidocs.net/images/page/78004/django-logo-negative.png" width="250px">
</p>
<br>

참고문서 : [장고 (웹 프레임워크)](https://ko.wikipedia.org/wiki/%EC%9E%A5%EA%B3%A0_(%EC%9B%B9_%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC)), [Django 소개](https://developer.mozilla.org/ko/docs/Learn/Server-side/Django/Introduction)

## 장고의 구성
장고는 파이썬으로 코딩한 모델을 관계형DB로 구축해주는 **모델(Model)**,   
HTTP 요청을 처리하는 웹 템플릿 시스템인 **뷰(View)**,   
URL의 라우팅을 처리하는 URL **컨트롤러(Controller)** 로 구성된 **MVC 디자인 패턴**을 따른다.

MVC패턴은 Model, View, Controller를 독립적으로 구성해 한 요소가 다른 요소들에게 영향을 주지 않도록 설계하는 패턴이다.

<br>
<p align="center">
<img src="https://s3.ap-northeast-2.amazonaws.com/opentutorials-user-file/module/327/1262.png">
<br>
MVC 패턴 (그림 출처: https://opentutorials.org/course/697/3828)
</p>
<br>

하지만 django는 전통적인 MVC 디자인 패턴에서 이야기하는 컨트롤러의 기능을 프레임워크를 자체에서 하기 때문에 
모델(Model), 템플릿(Template), 뷰(View)로 분류해 **MTV 프레임워크**라고 보기도 한다.

다음은 장고를 구성하는 요소들이다.

### 모델
모델은 <span style="color:#7f9eb2">**데이터에 관한 정보**</span>를 담는다.   
구현은 클래스 형태로 이루어지며 하나의 모델 클래스는 데이터베이스의 테이블과 매핑된다.
<br>

**models.py**
```python3
class Question(models.Model):
    subject = models.CharField(max_length=200)
    content = models.TextField()
    create_date = models.DateTimeField()

    def __str__(self):
        return self.subject
```
위 모델에서 subject, content, create_date 속성들은 테이블의 필드가 된다.

### 뷰
뷰는 <span style="color:#7f9eb2">**어떤 데이터가 표시될 것인지**</span>를 정의한다.   
웹 요청을 받아 데이터베이스 처리 등 로직에 맞는 처리를 수행한 후 가공된 결과 데이터를 템플릿에 전달하고, 템플릿에서 반환 받은 HTML형태의 응답을 최종으로 클라이언트에게 반환한다. 

뷰는 HTTP 응답(response)를 반환해야 하며 응답의 종류는 웹 페이지, 리디렉션, 문서 등의 다양한 형태가 가능하다.

**views.py**
```python3
def index(request):
    question_list = Question.objects.order_by('-create_date')
    context = {'question_list':question_list}

    """ render함수는 파이썬 데이터를 템플릿에 적용해서 HTML로 변환하는 함수"""
    return render(request,'pybo/question_list.html',context)
```

### 템플릿
템플릿은 <span style="color:#7f9eb2">**데이터가 어떻게 표시되는지**</span>를 정의한다.   
사용자에게 실제로 보여지는 웹 페이지나 문서를 다루며 장고 템플릿 태그(python 문법기반)를 통한 동적 페이지 구현이 가능하다.<br>

**template.html**
```html
{% if question_list %}
    <ul>
    {% for question in question_list %}
        <li><a href="/pybo/{{ question.id }}/">{{ question.subject }}</a></li>
    {% endfor %}
    </ul>
{% else %}
    <p>질문이 없습니다.</p>
{% endif %}

```

### URL
URL은 **view와 template을 이어주는 역할**을 하고, 이 부분을 만들어 주는 작업을 URLconf라고 한다.   
클라이언트로부터 요청을 받으면 django는 요청에 있는 URL을 분석하고, 이를 urls.py에 정의된 URL패턴과 매칭한다.
<br>

**urls.py**
```python3
urlpatterns = [
    path('', views.index),
    path('<int:question_id>/',views.detail),
]
```


### MTV 다이어그램
위 내용을 정리하면 아래 다이어그램과 같다.

<br>
<p align="center">
<img src="https://mdn.mozillademos.org/files/13931/basic-django.png"><br>
django의 코드 구조 (그림 출처 : https://developer.mozilla.org/ko/docs/Learn/Server-side/Django/Introduction)
</p>
<br>

다이어그램의 흐름은 다음과 같다.
1) 브라우저에서 로컬 서버로 페이지(ex. http://xxxx.com/index) 를 요청한다.   
2) urls.py 파일에서 /index URL 매핑을 확인하여 views.py 파일의 index 함수를 호출한다.   
3) 호출된 뷰는 요청에 따라 로직에 맞는 처리를 수행하고, 그 과정에서 모델에게 CRUD를 지시한다.   
4) 모델은 ORM을 통해 DB와 소통하며 CRUD를 수행한다.
5) 뷰는 템플릿에 데이터를 전달하고 렌더링된 결과를 전달받는다.
6) 최종 결과를 HTTP 응답으로 반환한다.
