# Lazyloading

<br>
<p align="center">
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FnUKUG%2FbtqOFikUozU%2FrcyMIMQXXczrP9Mke1Flw0%2Fimg.jpg" width="400px">
</p>
<br>

참고문서 : [Image Lazy Loading 기법으로 웹 성능 최적화하기](https://onlydev.tistory.com/104), [레이지 로딩(Lazy loading)이란](https://happist.com/570198/%ED%94%8C%EB%9F%AC%EA%B7%B8%EC%9D%B8-%EC%86%8C%EA%B0%9C-%EC%B5%9C%EC%8B%A0-%EA%B8%B0%EC%88%A0%EB%A1%9C-%EB%AC%B4%EC%9E%A5%ED%95%B4-%EB%B6%80%EB%93%9C%EB%9F%AC%EC%9A%B4-%EB%A0%88%EC%9D%B4%EC%A7%80), [웹 성능 최적화를 위한 Image Lazy Loading 기법](https://helloinyong.tistory.com/297)

## Lazy Loading이란?

Lazy Loading이란 로딩을 뒤로 미루는 것을 의미한다.

웹 페이지 초기에 보이는 영역(Browser ViewPort)에 해당하는 컨텐츠(이미지, 동영상 등)의 재생에 필요한 자바스크립트 등의 리소스를 먼저 내려받고 필요에 따라 리소스를 추가로 로드하여 화면에 보여주는 방식이다.
다시 말해 지금 필요하지 않은 모든 요소들의 로딩을 지연시키는 것이다.

<br>

### Lazy Loading의 장점

<br>
<p align="center">
<img src="https://velog.velcdn.com/images/qowhdgn/post/586268d7-bc88-4ddd-828c-c359c5afb450/image.png" width="300px">
</p>

1. 성능 향상
   - 페이지 초기 로딩 시 필요한 이미지의 수를 줄일 수 있다.
   - 필요한 이미지의 수가 줄어들기 때문에 유저가 다운로드하는 byte 용량이 줄어들고, 이는 유저가 사용 가능한 제한된 네트워크 대역폭의 경쟁을 줄이는 것을 의미한다. 따라서 디바이스는 유저에게 페이지를 빠르게 전달해줄 수 있다.
2. 비용 감소
   - 통신 비용 관점에서 이점이 있다. 사용자가 페이지에 접속 시 전체 리소스를 받아 대량의 데이터가 전송되는 일반적인 방식에 비해 Lazy Loading은 필요한 리소스만 전송받기 때문에 경제적이다.

<br>

참고 : Lazy Loading의 일반적인 구현 방법

- [Lazy Loading을 다루는 여러가지 기술](https://helloinyong.tistory.com/297#title-4)
- [[React] Infinite Scroll(무한 스크롤) with Intersection Observer](https://im-developer.tistory.com/196)
