# SPA vs MPA

<br>
<p align="center">
<img src="https://vaporvm.com/wp-content/uploads/2021/12/single-page-application-vs-multiple-page-application476514065.jpg" width="350px">
</p>
<br>

참고문서 : [SPA기반 웹사이트의 SEO](https://www.ascentkorea.com/seo-for-spa/), [SPA / MPA 이란, 장단점](https://gxnzi.tistory.com/105), [SPA(single page application) vs MPA(multi page application)](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=sthwin&logNo=221214109560), [SSR? CSR?](https://kyj7337.github.io/posts/SSRCSR), [SPA, CSR과 SSR, SEO](https://velog.io/@ksh4820/SPA-CSR%EA%B3%BC-SSR-SEO)

## SPA(Single Page Application)란?
SPA는 단어의 의미대로 단일 페이지의 웹 어플리케이션 또는 웹사이트를 의미한다. 최초 로딩 시 웹사이트의 전체 페이지를 하나의 페이지에 담고, 서버로부터 새로운 페이지 자원을 불러오지 않고 현재의 페이지를 동적으로 표현하는 방식으로 동작한다.

현재의 HTML은 고정하고 필요 시 변경 부분에 대해서만 서버로부터 전달받아(하기 이미지 참고) 클라이언트 사이드에서 렌더링하는 방식을 통해 자연스러운 페이지 이동과 사용자 경험(UX)을 제공한다.

SPA 구현을 위한 대표적인 프레임워크로는 React, Vue, Angular 등이 있다.

<br>
<p align="center">
<img src="https://www.ascentkorea.com/wp-content/uploads/2021/08/image-5.png" width="600px">
<br>
기존 웹사이트 로딩 방식(좌) vs SPA 웹사이트 로딩 방식(우)
</p>
<br>

<br>

### SPA의 장점
1. 최초 로딩 이후 페이지 로딩속도가 빠르다.
   - 최초 요청 시 서버로부터 모든 재료(html, css, js 등)를 가져온 후 화면 변경이 필요한 부분만 렌더링되므로 화면 전환이 빠르다.
2. 사용자 경험(UX)이 우수하다.
   - 전체 페이지 새로고침으로 인한 깜빡거림이 없고 페이지의 일부분만 동적으로 로드하여 응답시간이 크게 단축된다.
3. 백엔드 의존성이 낮다.
   - 서버 없이도 프론트엔드 일부를 개발/검증 가능하다.
   - 모바일로 확장 시 백엔드를 재사용 가능하므로 확장성 측면에서도 유리하다.
4. 컴포넌트별 개발이 용이하여 생산성이 우수하다.

### SPA의 단점
1. 최초 페이지 로딩속도가 느리다.
   - 클라이언트가 최초로 서버에 요청 시 모든 데이터를 가져오므로 MPA에 비해 초기 로딩속도가 상대적으로 느리다.
2. SEO에 적합하지 않다.
   - 한 페이지에 여러 페이지를 클라이언트사이드에서 자바스크립트로 구현하는 방식이므로 자바스크립트를 읽지 못하는 검색엔진에 대해서는 크롤링이 되지 않아 색인이 불가한 문제가 발생 가능하다.
3. 보안에 취약할 수 있다.
   - 서버 로직에 의해 완성된 페이지를 제공하는 MPA 방식과 달리 클라이언트 사이드에 로직을 가지고 있는 SPA는 XSS(크로스사이트스크립트) 공격에 취약할 수 있다.

<br>

## MPA(Multi Page Application)란?
MPA는 전통적인 웹 어플리케이션의 개발 방식으로 SPA와 달리 여러 개의 페이지로 이루어진 어플리케이션이다. 최초 로딩 시 클라이언트는 서버로부터 하나의 정적인 페이지의 소스를 가져오고, 그 후 다른 페이지 또는 게시물로 이동 시 새로운 정적 페이지 전체를 서버로부터 전달받는다.

<br>
<p align="center">
<img src="https://i0.wp.com/hanamon.kr/wp-content/uploads/2021/03/SPA.png?resize=1280%2C691&ssl=1" width="400px">
<br>
SPA 방식<br><br>
<img src="https://i0.wp.com/hanamon.kr/wp-content/uploads/2021/03/MPA.png?w=1050&ssl=1" width="400px">
<br>
전통적인 MPA 방식
</p>
<br>

<br>

### MPA의 장점
1. SEO 관점에서 유리하다.
   - MPA는 완성된 형태의 HTML 파일을 서버로부터 전달받기 때문에 검색 엔진이 페이지를 크롤링하기에 적합하다.
2. 최초 페이지 로딩속도가 빠르다.
   - 서버에서 렌더링 완료된 HTML 문서가 클라이언트로 전달되므로 최초 로딩이 빠르다.
   - 하지만 클라이언트가 Javascript 파일을 모두 다운로드하고 적용하기 전까지는 각 기능이 정상 동작하지 않을 수 있다.

### MPA의 단점
1. 사용자 경험(UX)이 SPA에 비해 떨어질 수 있다.
   - 새로운 페이지로 이동 시 전체 페이지가 새로고침되므로 깜빡거림이 발생한다.
   - 다만 Ajax로 화면의 일부만 갱신 가능하기도 하다.
2. 프론트엔드와 백엔드 간의 결합도가 높다.
   - 높은 결합도로 인해 개발이 복잡하다.
   - 모바일 버전 개발 시 백엔드를 새로 개발해야 하므로 확장성이 SPA보다 낮다.
3. 서버 렌더링에 따른 서버 부하가 발생한다.

<br>

### 언제 SPA를 쓰는 것이 좋을까?
SPA는 많은 장점을 가지고 있으나 그만큼 단점도 있기 때문에 사이트의 목적과 성격에 따라 SPA 또는 MPA만 사용할지, 두 방식을 혼용해서 사용할 지 고민해봐야 한다.

1. 사이트가 대규모 콘텐츠를 담는 경우
   - E-Commerce 사이트와 같이 방대한 양의 콘텐츠를 담아내야 하는 경우 한번에 모든 요소를 요청하는 SPA 방식보다는 MPA 방식으로 구성하는 것이 SEO 뿐만 아니라 페이지 속도 측면에서도 더 유리할 수 있다.
2. 사이트 규모가 작은 경우
   - 사이트의 콘텐츠가 적고 화려하면서 부드러운 이미지 구현이 필요한 사이트의 경우 SPA가 적합할 수 있다. 또한 모바일 환경에서 UX가 우수한 SPA 방식은 SEO에 적합하지 않은 점을 감수하더라도 사용하기 좋을 수 있다.
3. SPA와 MPA를 섞은 하이브리드 방식
   - SPA로 페이지 구현이 효과적인 일부 페이지만 SPA로 제한하고, 검색에 노출되어야 하는 페이지는 MPA로 제작하는 하이브리드 방식도 채택 가능하다.

<br>

### SPA, MPA를 적용한 사례
- SPA : Facebook, Google, Slack
- MPA : Amazon, eBay 
