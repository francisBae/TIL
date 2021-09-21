# django 개발환경 구축하기(1)

<br>
<p align="center">
<img src="https://wikidocs.net/images/page/78004/django-logo-negative.png" width="250px">
</p>
<br>

참고문서 : [1-03 장고 개발 환경 준비하기](https://wikidocs.net/70588), [47.11 가상환경 사용하기](https://dojang.io/mod/page/view.php?id=2470)

이번 django 게시글에서는 파이참 IDE를 기준으로 장고 개발환경을 셋팅해 볼 것이다.

### 파이썬 가상 환경 알아보기
파이썬 가상 환경은 파이썬 프로젝트 진행 시 독립된 환경을 제공해주는 도구다.

파이썬을 사용하다 보면 pip로 패키지를 설치하게 된다. 이 패키지들은 파이썬 설치 폴더(디렉토리)의 Lib/site-packages 안에 저장되는데, 해당 폴더에 설치된 패키지들은 모든 파이썬 스크립트에서 사용 가능하다.
일반적으로는 문제가 없는 방식이지만 여러 프로젝트를 동시에 수행하는 경우 아래 예시와 같이 패키지의 버전 문제가 발생 가능하다.

<span style="color:#ff5f2e;">파이썬 개발자 A가 2개의 파이썬 프로젝트(A, B)를 개발하고 관리한다</span>고 가정하자. 이 때 필요한 **프로젝트 A**에서는 **패키지X 1.5**를 사용해야 하고, **프로젝트 B**에서는 **패키지X2.0**을 사용해야 한다면 하나의 데스크톱에 서로 다른 버전의 파이썬을 설치해야 하는 문제가 발생한다.

<br>
<p align="center">
<img src="https://dojang.io/pluginfile.php/14099/mod_page/content/4/047005.png">
<br>
글로벌 파이썬 환경에서 패키지가 호환되지 않는 경우 (그림 출처: https://dojang.io/mod/page/view.php?id=2470)
</p>
<br>

이런 문제를 해결하기 위해 파이썬은 아래 이미지와 같이 <span style="color:#ff5f2e;">'가상 환경'이라는 독립적인 공간</span>을 제공한다. 가상 환경에서 pip로 패키지를 설치하면 가상 환경 디렉토리 내 Lib/site-packages 에 패키지가 저장된다.
가상 환경에 파이썬 인터프리터도 포함되므로 <span style="color:#ff5f2e;">개발 환경을 유연하게 구축 가능하다.</span>

<br>
<p align="center">
<img src="https://dojang.io/pluginfile.php/14099/mod_page/content/4/047006.png">
<br>
파이썬 가상 환경으로 독립된 공간을 구성 (그림 출처: https://dojang.io/mod/page/view.php?id=2470)
</p>
<br>

### 파이참에서 파이썬 가상 환경 사용하기
파이참(PyCharm) 환경에서는 파이썬 가상 환경 구축이 CLI(Command Line Interface) 방식보다는 비교적 간단하다.

파이참에서 <code style="color:#c72e45;">File-New Project</code>을 클릭해서 새 프로젝트 생성 화면으로 진입 후 아래와 같이 <code style="color:#c72e45;">Virtualenv</code> 환경을 선택하고 Create을 클릭한다.

![](https://images.velog.io/images/qowhdgn/post/3a5c7d35-466c-4e6f-9eec-cb133bcdbfc9/02.PNG)

프로젝트 생성 후  <code style="color:#c72e45;">File-Settings</code>을 클릭해서 프로젝트의 Python Interpreter을 위에서 만든 신규 가상 환경으로 설정해준다. (톱니바퀴 모양 클릭 후 Add)

![](https://images.velog.io/images/qowhdgn/post/ba93c4bb-f81a-474c-809d-8c6ba9bd5f4e/03.PNG)

![](https://images.velog.io/images/qowhdgn/post/808f541b-5b83-4626-b46c-74e326de692e/04.PNG)

이제 파이썬 가상 환경을 사용하기 위한 준비가 되었다.

### 파이참에서 django 설치하기

이제 파이참의 파이썬 가상 환경에 django 패키지를 설치하도록 하자.   
Python Interpreter 설정 내에서 \+ 버튼을 클릭하면 설치 가능한 패키지 목록이 조회된다.

![](https://images.velog.io/images/qowhdgn/post/0b881ff4-5c27-4739-a564-f73803587004/05.PNG)

검색창에 django를 검색하고 Install Package를 클릭하면 아래와 같이 django를 사용하기 위한 패키지들이 설치되었음을 확인 가능하다.
![](https://images.velog.io/images/qowhdgn/post/b97927ec-7e5b-4e5f-a9d8-84a21e40f576/06.PNG)

![](https://images.velog.io/images/qowhdgn/post/4b254c09-a238-43ca-96b3-5ed1fe358572/07.PNG)

