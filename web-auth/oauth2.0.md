# OAuth2.0

<br>
<p align="center">
<img src="https://user-images.githubusercontent.com/32903334/165744538-88683b27-c36a-41dd-a3aa-8dc8e047ebab.png" width="350px">
</p>
<br>

참고문서 : [OAuth 2.0 동작 방식의 이해](https://blog.naver.com/mds_datasecurity/222182943542), [OAuth 2.0 개념 총 정리](https://charming-kyu.tistory.com/36), [[OAuth2] 인증 ( Authentication) vs 인가 ( Authorization )](https://gintrie.tistory.com/36), [OAuth(2) - 역할 및 유형](https://hammerstudy.tistory.com/4)

## OAuth2.0이란?

OAuth2.0(Open Authorization 2.0)은 인가를 위한 개방형 표준 프로토콜이다.

이 프로토콜에서는 서드파티 어플리케이션에게 리소스 소유자(Resource Owner)를 대신해 리소스 서버에서 제공하는 자원에 대한 접근 권한을 위임하는 방식을 제공한다. 구글, 페이스북, 카카오, 네이버 등에서 제공하는 간편 로그인 기능도 OAuth2.0 프로토콜 기반의 사용자 인증 기능을 제공하고 있다.

다시 말해, 특정 어플리케이션에만 가입되어 있어도 해당 어플리케이션으로부터 받은 권한을 여러 어플리케이션에서 사용할 수 있다.

OAuth2.0의 이해를 위해 인증과 인가의 차이를 이해해야 한다.

### 인증 vs 인가

인증과 인가는 출입증에 비교해서 설명하면 쉽게 이해 가능하다.
방문자가 회사 건물에 방문했다고 가정했을 때

1. 인증(Authentication) : 방문자가 자신이 회사 건물에 출입 가능한지 확인받는 과정이다.

2. 인가(Authorization) : 방문자가 회사 건물에 방문 시, 허가된 공간에만 접근 가능하다. 이 때 개인마다 허가된 공간(=>획득 가능한 리소스)이 다른데 이런 접근 권한을 확인하는 것이 인가이다.

OAuth2.0은 '인가'를 위한 프레임워크이다.

### FYI) OAuth1.0 vs OAuth2.0

- [OAuth1.0과 OAuth2.0의 차이](https://itwiki.kr/w/OAuth#OAuth1.0.EA.B3.BC_OAuth2.0.EC.9D.98_.EC.B0.A8.EC.9D.B4)

<br>

### OAuth2.0의 4가지 역할

<br>

<p align="center">
<img src="https://user-images.githubusercontent.com/32903334/165762600-935507c9-f058-4800-a544-2796fba8bbb7.png" width="300px">
</p>
<br>

| 역할                 | 설명                                                                                                                                                                                                                    |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Resource Server      | **자원 서버**는 보호된 정보를 제공하는 서버로 일반적으로 웹 어플리케이션이다. ex) 구글, 페이스북 등의 소셜 서비스 제공자                                                                                                |
| Resource Owner       | **자원 소유자**는 자원 서버에 계정을 가지고 있는 사용자로, 클라이언트가 그들의 계정을 통해 자원 서버에 접근하는 것을 인가(Authorize)한다. ex) 대부분의 경우 사용자                                                      |
| Client               | **클라이언트**는 자원 소유자를 대신해 리소스 서버의 서비스를 사용하고자 하는 어플리케이션이다. ex) 쇼핑몰, 게임 등 소셜 로그인 이용자(업체)                                                                             |
| Authorization Server | **인가 서버**는 인증/인가를 수행하는 권한 서버로 클라이언트의 접근 자격을 확인하고 **Access Token을 발행**하여 서비스 사용 권한을 부여하는 역할을 수행한다. ex) 구글, 페이스북 등의 소셜 서비스 제공자의 권한 부여 서버 |

<br>

### OAuth2.0의 주요 용어

| 용어                 | 설명                                                                              |
| -------------------- | --------------------------------------------------------------------------------- |
| Authentication(인증) | 접근 자격이 있는지 검증하는 단계                                                  |
| Authorization(인가)  | 자원에 대한 접근 권한을 부여하고 자원 접근 권한이 담긴 Access Token을 제공하는 것 |
| Access Token         | 자원 서버에게서 자원 소유자의 정보를 획득할 때 사용되는 '만료 기간이 있는 Token'  |
| Refresh Token        | Access Token 만료시 이를 재발급하기 위한 용도로 사용되는 Token                    |

<br>

### OAuth2.0 프로세스

OAuth2.0 프로세스는 PAYCO 개발자센터 페이지 및 블로그에서 명확하게 제시하고 있어 해당 이미지를 첨부하였다.

<br>
<p align="center">
<img src="https://developers.payco.com/static/img/@img_guide2.jpg" width="600px">
<br>
출처 : PAYCO 개발자 센터
</p>

위 이미지에서 PAYCO 인증 서비스는 Authorization Server, PAYCO API 서비스는 Resource Server이다.

이해를 위해 아래 이미지와 설명을 첨부하였다.

<br>
<p align="center">
<img src="https://yoonhogo.github.io/assets/images/Protocol-Flow-f91c799a30ac082024f358be85f5bd55.png" width="600px">
<br>

출처 : [yoonho's dev notes](https://yoonhogo.github.io/blog/2019-11-06/OAuth2-1/)

</p>

1. 클라이언트는 자원 소유자(사용자)에게 허가를 요청한다. 허가는 리소스 소유자에게 바로 요청하거나 권한 부여 서버(인가 서버)가 중개자로서 간접적으로 이루어질 수 있다.
2. 클라이언트는 자원 소유자의 인가를 나타내는 권한 부여를 받는다. 권한 부여 유형은 4가지 방식 중 하나를 사용하거나 확장된 유형으로 사용할 수 있다. 권한 부여 유형은 클라이언트의 요청과 권한 부여 서버가 지원하는 타입에 따라 다르다.
3. 클라이언트는 권한 부여 서버에게 권한 부여 허가에 따라 액세스 토큰을 요청한다.
4. 권한 부여 서버는 클라이언트를 인증하고, 권한 부여 허가를 검증한다. 만약 잘 되었다면, 액세스 토큰을 발행한다.
5. 클라이언트는 자원 서버에게 보호된 자원을 요청한다(API 등의 수단 이용). 이 때, 액세스 토큰으로 인증한다.
6. 자원 서버는 액세스 토큰을 검증한다. 검증이 완료되면, 클라이언트의 요청에 응답한다.

<br>

### OAuth2.0의 권한 부여 방식

원문 : [OAuth 2.0 동작 방식의 이해](https://blog.naver.com/mds_datasecurity/222182943542)

OAuth 2.0 프로토콜에서는 다양한 클라이언트 환경에 적합하도록 권한 부여 방식에 따른 프로토콜을 4가지 종류로 구분하여 제공한다.

<br>

#### 1. Authorization Code Grant : 권한 부여 승인 코드 방식

<br>

권한 부여 승인을 위해 자체 생성한 Authorization Code를 전달하는 방식으로, 많이 쓰이고 기본이 되는 방식이다. 간편 로그인 기능에서 사용되는 방식으로 클라이언트가 사용자를 대신하여 특정 자원에 접근을 요청할 때 사용되는 방식이다. 보통 타사의 클라이언트에게 보호된 자원을 제공하기 위한 인증에 사용된다. Refresh Token의 사용이 가능한 방식이다.

<br>

<p align="center">
<img src="https://postfiles.pstatic.net/MjAyMDEyMjNfMTEw/MDAxNjA4NzAyMjIyMjMz.cfPTbiYCv9yv3_4qr32FiPCno1lvit8c5e_cQJ1bnPgg.cYlP9nFG5rB3at9G-N3bXi9Qy9TOS_KpGDNVF0QmNtkg.PNG.mds_datasecurity/image.png?type=w966">
</p>
<br>

권한 부여 승인 요청 시 response_type을 code로 지정하여 요청한다. 이후 클라이언트는 권한 서버에서 제공하는 로그인 페이지를 브라우저를 띄워 출력한다. 이 페이지를 통해 사용자가 로그인을 하면 권한 서버는 권한 부여 승인 코드 요청 시 전달받은 redirect_url로 Authorization Code를 전달한다. Authorization Code는 권한 서버에서 제공하는 API를 통해 Access Token으로 교환된다.

<br>

#### 2. Implicit Grant : 암묵적 승인 방식

<br>

자격증명을 안전하게 저장하기 힘든 클라이언트(ex: JavaScript등의 스크립트 언어를 사용한 브라우저)에게 최적화된 방식이다.

암시적 승인 방식에서는 권한 부여 승인 코드 없이 바로 Access Token이 발급된다. Access Token이 바로 전달되므로 만료기간을 짧게 설정하여 누출의 위험을 줄일 필요가 있다.
​
Refresh Token 사용이 불가능한 방식이며, 이 방식에서 권한 서버는 client_secret를 사용해 클라이언트를 인증하지 않는다. Access Token을 획득하기 위한 절차가 간소화되기에 응답성과 효율성은 높아지지만 Access Token이 URL로 전달된다는 단점이 있다.

<br>

<p align="center">
<img src="https://postfiles.pstatic.net/MjAyMDEyMjNfMjMz/MDAxNjA4NzAyMjQxOTg2.GTTG4UxxFQrWdRcrJHeQqoZODvOsdLwH70iTaJKsGJUg.wRwxtAmdLuOzscjqOjrm5MOaSF3gVJ-Q1wysvprXMTkg.PNG.mds_datasecurity/image.png?type=w966">
</p>
<br>

권한 부여 승인 요청 시 response_type을 token으로 설정하여 요청한다. 이후 클라이언트는 권한 서버에서 제공하는 로그인 페이지를 브라우저를 띄워 출력하게 되며 로그인이 완료되면 권한 서버는 Authorization Code가 아닌 Access Token를 redirect_url로 바로 전달한다.

<br>

#### 3. Resource Owner Password Credentials Grant : 자원 소유자 자격증명 승인 방식

<br>

간단하게 username, password로 Access Token을 받는 방식이다.

클라이언트가 타사의 외부 프로그램일 경우에는 이 방식을 적용하면 안된다. 자신의 서비스에서 제공하는 어플리케이션일 경우에만 사용되는 인증 방식이다. Refresh Token의 사용도 가능하다.

<br>

<p align="center">
<img src="https://postfiles.pstatic.net/MjAyMDEyMjNfMTQ4/MDAxNjA4NzAyMjYwNjY0.9NccTFwC2vvPezXtsufXP5jaltyM4f3T0Szk7Ykqe50g.4XIlCvZQnGvXWvEdHNImR53EvUs72joB3dqsambiwX4g.PNG.mds_datasecurity/image.png?type=w966">
</p>
<br>

위와 같이 흐름은 간단하다. 제공하는 API를 통해 username, password을 전달하여 Access Token을 받는 것이다. 중요한 점은 이 방식은 권한 서버, 리소스 서버, 클라이언트가 모두 같은 시스템에 속해 있을 때 사용되어야 하는 방식이라는 점이다.

<br>

#### 4. Client Credentials Grant : 클라이언트 자격증명 승인 방식

<br>
클라이언트의 자격증명만으로 Access Token을 획득하는 방식이다.

OAuth2.0의 권한 부여 방식 중 가장 간단한 방식으로 클라이언트 자신이 관리하는 리소스 혹은 권한 서버에 해당 클라이언트를 위한 제한된 리소스 접근 권한이 설정되어 있는 경우 사용된다.

이 방식은 자격증명을 안전하게 보관할 수 있는 클라이언트에서만 사용되어야 하며, Refresh Token은 사용할 수 없다.

<br>

<p align="center">
<img src="https://postfiles.pstatic.net/MjAyMDEyMjNfMjIx/MDAxNjA4NzAyNDQwMzQ5.86I4OrUs0bHxoJQKBbFcPkLuXxfEf1slgI_fnF40ja8g.9a4JaG7DQf8pGcyuRmEkpSbGdbff6hETz01uGPwiwRog.PNG.mds_datasecurity/image.png?type=w966">
</p>
<br>
