# django란?

<br>
<p align="center">
<img src="https://wikidocs.net/images/page/78004/django-logo-negative.png" width="250px">
</p>
<br>

참고문서 : [1-01 필자가 생각하는 장고란?](https://wikidocs.net/78004), [장고 (웹 프레임워크)](https://ko.wikipedia.org/wiki/%EC%9E%A5%EA%B3%A0_(%EC%9B%B9_%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC))

## "장고는 웹 프로그램을 쉽고 빠르게 만들어 주는 웹 프레임워크다"

### 웹 프레임워크란?
<br>
<p align="center">
<img src="https://wikidocs.net/images/page/78004/1-01_2.png">
</p>
<br>

웹 프레임워크란 쉽게 말하면 "**웹 프로그램을 만들기 위한 스타터 키트**"이다.   
기본적으로 웹 프로그램을 구현할 때 쿠키나 세션 처리, 로그인/로그아웃 처리, 권한 처리, DB연동 등 구현해야 할 기능이 산더미다.   
웹 프레임워크는 이런 기능들을 제공하고 우리는 **웹 프레임워크의 기능만 익혀서 사용하면 된다.**   

파이썬으로 만들어진 웹 프레임워크 중 하나가 바로 **장고**(**django**)다.

## 특징
장고는 웹 개발에서 번거로운 요소들을 새로 개발할 필요 없이 내장된 기능만을 이용해 빠른 개발을 할 수 있다.   
웹 브라우저에 'Hello World'를 출력하려면 장고가 요구하는 간단한 url 규칙을 정의하고 다음과 같은 함수 하나만 작성하면 된다.

```python
def index(request):   
    return HttpResponse("Hello World")
```

## 보안이 강한 장고
장고는 튼튼한 웹 프레임워크다. 보안 공격을 기본적으로 잘 방어한다.   
예를 들어 SQL 인젝션, XSS(cross-site scripting), CSRF(cross-site request forgery), 클릭재킹(clickjacking)과 같은 보안 공격을 기본으로 막아 준다.   
즉, 장고를 사용하면 이런 보안 공격에 대한 코드를 직접 짤 필요가 없다.

