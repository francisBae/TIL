# API란?

<p align="center">  
<img src ="https://media.vlpt.us/images/qowhdgn/post/d73e6f7e-d363-45bf-97a5-cc9663dfc889/API-Platforms.png" width="550px">
<br>이미지 출처 : https://blog.calameo.com/2744/api-quick-guide/
</p>

참고문서 : [API(위키백과)](https://ko.wikipedia.org/wiki/API), [API란? 비개발자가 알기 쉽게 설명해드립니다!](http://blog.wishket.com/api%EB%9E%80-%EC%89%BD%EA%B2%8C-%EC%84%A4%EB%AA%85-%EA%B7%B8%EB%A6%B0%ED%81%B4%EB%9D%BC%EC%9D%B4%EC%96%B8%ED%8A%B8/#API-4), [[IT용어] API란 무엇인가?](https://steemit.com/kr/@yahweh87/it-api)

### API란 무엇인가?
"API(Application Programming Interface)는 응용 프로그램에서 사용할 수 있도록, 운영 체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만든 인터페이스를 뜻한다." - 위키백과

다시 말해, API는 두 응용 프로그램이 서로 통신할 수 있도록 하는 <span style="color:#f78877;">**소프트웨어 중재자**</span>라고 할 수 있다.



### API의 작동 원리
![](https://images.velog.io/images/qowhdgn/post/afca02b4-35ad-4e57-bee8-b0318987f4fc/api-working-1024x377-1.png)
<p align="center">  
이미지 출처 : https://blog.calameo.com/2744/api-quick-guide/
</p>

<span style="color:#f78877;">클라이언트로부터 **요청(Request)**</span>을 받으면 <span style="color:#f78877;">서버는 제공할 데이터를 **응답(Response)**</span>한다. 데이터의 형태는 HTML, XML, json 등 다양하다.



### API의 유형
API 는 공개범위에 따라 3가지 유형으로 분류된다.

**1) Private API (내부 API, 비공개 API)**
- 3자에게 공개하지 않고 조직 내부 개발자 또는 계약자에게 제공하는 API
- 조직의 백엔드 데이터 및 애플리케이션 기능 일부를 여는 인터페이스
- ex) 조직 근태관리 API

**2) Public API (오픈 API,공개 API)**
- 개발자라면 누구나 가져다 사용할 수 있는 공개된 API
- 개발자에게 사유 응용 소프트웨어나 웹 서비스의 프로그래밍적인 권한을 제공
- ex) 네이버맵, 구글맵, 도로명주소조회서비스

**3) Partner API (파트너 API)**
- 조직 외부에서도 허가된 사람들이 사용할 수 있는 API
- 주로 비즈니스 관계에서 사용되고, 파트너 회사 간 소프트웨어 통합을 위해 사용


### API로 서비스를 하면 좋은 점
1) Private API는 개발자들이 애플리케이션 코드를 작성하는 방법을 표준화하여 간소화되고 빠른 프로세스 처리가 가능하게 해준다.

2) Public API와 Partner API 를 사용하면, 기업은 타사 데이터를 활용하여 브랜드 인지도를 높일 수 있다.

3) 대형 플랫폼(네이버, 카카오, 구글 등)은 간편 로그인 등의 기능을 제공해 플랫폼 이탈율을 현저히 줄일 수 있다.