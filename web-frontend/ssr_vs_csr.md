# SSR vs CSR

<br>
<p align="center">
<img src="https://tateeda.com/wp-content/uploads/2020/09/1-9.jpg" width="350px">
</p>
<br>

참고문서 : [렌더링 이란](https://itworldyo.tistory.com/150), [CSR, SSR, SPA, MPA? 상사한테 혼나기 전에 알아야하는 것](https://blog.hahus.kr/csr-ssr-spa-mpa-ede7b55c5f6f), [CSR(클라이언트 사이드 렌더링) vs SSR(서버 사이드 렌더링) 단골질문 뽀개기](https://eunjinii.tistory.com/105), [SSR? CSR?](https://kyj7337.github.io/posts/SSRCSR), [SPA, CSR과 SSR, SEO](https://velog.io/@ksh4820/SPA-CSR%EA%B3%BC-SSR-SEO)

## 웹 Rendering이란?
렌더링이란 HTML, CSS, Javascript 등 개발자가 작성한 문서를 브라우저가 화면에 그려주는 동작을 의미한다.

<br>

### 렌더링 과정
1. HTML을 파싱하여 DOM(Document Object Model) 트리를 생성
2. CSS를 파싱하여 CSSOM(CSS Object Model) 트리를 생성
3. DOM과 CSSOM을 결합하여 렌더링 트리를 생성
   - 이 때 화면에 드러나지 않는 head 태그나 display:none 스타일을 가진 요소는 렌더 트리에 포함되지 않는다.
4. 렌더링 트리에서 각 노드의 크기 및 위치를 계산
5. 개별 노드를 화면에 그려낸다.

<br>
렌더링에 대해 더 알고 싶다면?

- [객체 모델 생성 - developers.google.com](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/constructing-the-object-model?hl=ko)

- [렌더링 트리 생성, 레이아웃 및 페인트 - developers.google.com](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-tree-construction?hl=ko)

<br>

## CSR vs SSR
브라우저에서 우리가 보는 화면을 최종적으로 어디서 렌더링해서 보여주는지, 어떻게 개발하는지에 따라 csr, ssr로 나뉜다.
<br>
<p align="center">
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fv8V9B%2FbtrmbmgMjuX%2FoJXLQ46uukMAg0Zi1WyG2K%2Fimg.png" width="500px">
<br>
CSR과 SSR의 차이
</p>
<br>

<br>

## CSR(Client Side Rendering)
CSR(Client Side Rendering)은 최초에 브라우저가 서버로부터 HTML과 static 파일을 받아오고 나면, 사용자의 요청에 따라 Javascript를 통해 View를 동적으로 렌더링하는 방식이다.

클라이언트 측에서 화면에 보여줄 내용을 생성하므로 클라이언트 사이드 렌더링이라 한다.
<br>
<p align="center">
<img src="https://miro.medium.com/max/1400/1*c955FMt4om-cyNU9d11OiQ.png" width="600px">
<br>
CSR 방식에서 웹서버에 최초 요청 시에는 데이터가 없는 깡통 문서가 반환된다.
</p>
<br>
<p align="center">
<img src="https://miro.medium.com/max/1400/1*nmfJo2FUGSF9aL45JwG-Lg.png" width="600px">
<br>
CSR은 HTML 및 static 파일들이 로드됨에 따라 데이터 또한 서버에 요청하고, 그 결과가 최종적으로 화면에 나타나게 된다.
</p>
<br>

## SSR(Server Side Rendering)
SSR(Server Side Rendering)은 서버에 새로운 페이지에 대한 요청이 이루어질 때마다 서버로부터 HTML과 data가 덧붙여진 템플릿을 받아오는 방식이며, 요청 시마다 브라우저의 새로고침이 발생한다.

서버 측에서 화면에 보여줄 내용을 해석하여 보내주기 때문에 서버 사이드 렌더링이라 한다.
<br>
<p align="center">
<img src="https://miro.medium.com/max/1400/1*fuDcEQEaNQXEg4S78n-lUQ.png" width="600px">
<br>
SSR은 데이터가 포함된 완성된 HTML을 반환한다.
</p>
<br>

## CSR과 SSR의 장단점
<br>
<p align="center">
<img src="https://kyj7337.github.io/static/d53c5b0658e5aaba27d9cb87b387c034/af3f0/CSRSSR.webp" width="550px">
<br>
</p>
<br>

- SSR : 초기 로딩 속도 빠름, SEO에 유리, View 변경 시 서버에 지속적으로 요청(서버 부담 큼)
- CSR : 초기 로딩 속도 느림, SEO에 불리, 초기 로딩 후 데이터 요청 시에만 서버에 요청(서버 부담 적음)

<br>

#### 참고) SEO(Search Engine Optimization)
- 대부분의 웹크롤러, 봇들은 JS를 실행시키지 못하고 HTML에서만 컨텐츠를 수집하므로 CSR 방식으로 개발된 페이지를 빈 페이지로 인식한다.
- SSR 방식은 View를 서버에서 전부 렌더링하므로 HTML에 모든 컨텐츠가 저장되어 있어 SEO 사용 시 문제가 없다.


