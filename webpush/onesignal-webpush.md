# OneSignal

<br>
<p align="center">
<img src="https://user-images.githubusercontent.com/32903334/164640816-667a6db5-411c-4512-842b-a7982f2f9c59.png" width="300px">
</p>
<br>

참고문서 : [OneSignal Document](https://documentation.onesignal.com/docs/web-push-quickstart)

## OneSignal이란?

OneSignal은 웹 사이트 및 모바일 응용 프로그램을 위한 무료 푸시 알림 서비스입니다. 각 플랫폼에 전용 SDK를 제공하여 모든 주요 네이티브 및 모바일 플랫폼을 지원합니다. RESTful 서버 API 및 마케터가 푸시 알림을 디자인하고 보내는 온라인 대시 보드를 제공합니다.

### 제약사항

- 가격 정책에 따라 지원되는 웹푸쉬 구독자 수가 다릅니다.

  - [One Signal priceplan](https://onesignal.com/pricing)

- 웹 프로젝트의 경우 OneSignal은 iOS에서 동작하지 않습니다. iOS 앱을 따로 만들면 상관 없지만, 모바일 웹만 개발하는 경우 이 점을 유의해야 합니다.

  - FCM과 달리 OneSignal은 macOS의 safari 브라우저를 지원합니다.

- OneSignal Web Push는 [Browser Support by Operating System](https://documentation.onesignal.com/docs/web-push-setup-faq) 환경에서만 동작합니다.

- OneSignal SDK는 localhost와 HTTPS만 제공하는 FCM과 달리 <font color="blue">http에 대한 웹푸쉬 알람도 지원</font>합니다. (제약은 존재합니다. [Limitations of the HTTP SDK](https://documentation.onesignal.com/docs/web-push-http-vs-https#limitations-of-the-http-sdk))

<br>

## React 에서 OneSignal 웹푸시 기능 구현하기

OneSignal을 사용하려면 먼저 계정을 생성해야 합니다. 계정 생성은 무료입니다.

- [OneSignal 계정 생성](https://app.onesignal.com/signup)

React의 경우 OneSignal 용 라이브러리인 react-onesignal을 설치합니다.

[React OneSignal](https://www.npmjs.com/package/react-onesignal)

Yarn

```
$ yarn add react-onesignal
```

NPM

```
$ npm install react-onesignal
```

<br>

### OneSignal 시작하기

1. OneSignal 로그인 후 Apps에서 신규 앱 생성하기

<br>
<p align="center">
<img src="https://files.readme.io/f2869ef-f23ff71-create_an_app.jpeg" width="400px">
<br>New App/Website 클릭
</p>
<br>

- 기존 app이 없다면 신규 app을 생성합니다.
- app의 명칭은 자유롭게 구성해도 됩니다.

<br>

2. 웹 프로젝트를 생성해야 하므로 Web을 선택하고 "Next: Configure Your Platform" 버튼을 클릭합니다.

<br>
<p align="center">
<img src="https://files.readme.io/4cb8ae7-Screen_Shot_2021-07-09_at_10.22.17_AM.png" width="500px">
</p>
<br>

3. 일반적인 웹 어플리케이션인 경우 'Typical Site'를 선택 후 진행합니다.

<br>
<p align="center">
<img src="https://files.readme.io/804f114-Screen_Shot_2021-07-09_at_10.36.10_AM.png" width="500px">
</p>
<br>

4. 사이트 기본 설정을 입력합니다.

| 항목                          | 설명                                                                                                                                                    |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| SITE NAME                     | 웹푸시 알람에 표시될 웹앱 명칭                                                                                                                          |
| SITE URL                      | 웹푸시 알람을 발생시킬 대상 웹 사이트. localhost의 경우 'http://localhost:3000' 를 입력하고 이 때 화면에 표시되는 LOCAL TESTING 항목에 체크하면 됩니다. |
| AUTO RESUBSCRIBE (HTTPS ONLY) | 웹푸시 구독자가 브라우저 데이터를 clear하거나 다른 푸시 제공자에서 OneSignal로 전환할 때 prompt 메시지 없이 자동으로 재구독 처리하도록 하는 설정        |
| DEFAULT ICON URL              | 웹푸시 알람과 Slide Prompt 에서 표시할 아이콘 (png, jpg, gif 만 지원)                                                                                   |

<br>
'My Site is not fully HTTPS' 는 아래 사항 중 하나라도 해당되는 경우 체크합니다.
  
- 웹 사이트가 HTTP인 경우
- 웹 호스트의 서버에 파일을 업로드할 수 없는 경우
- WordPress.com 또는 HubSpot COS에 의해 호스팅된 웹사이트인 경우

<br>
<p align="center">
<img src="https://files.readme.io/662221c-site-setup.jpg" width="500px">
</p>
<br>

5. Permission Prompt 설정

- OneSignal은 디폴트로 사이트 접속 10초 후 [Push Slide Prompt](https://documentation.onesignal.com/docs/slide-prompt)를 노출시킵니다.

<br>
<p align="center">
<img src="https://files.readme.io/af9542c-Screen_Shot_2021-07-09_at_11.16.03_AM.png" width="500px">
</p>
<br>

Prompt 추가/수정을 통해 해당 설정을 변경 가능합니다.

<br>
<p align="center">
<img src="https://user-images.githubusercontent.com/32903334/164661600-66b9a3f4-6946-4523-a839-f41ec6b97644.png" width="500px">
</p>
<br>

그 외 설정들은 선택사항이므로 필요 시 [OneSignal 공식문서](https://documentation.onesignal.com/docs/web-push-quickstart#step-4-welcome-notification-optional)를 참고하여 셋팅 후 Save 버튼을 클릭하면 됩니다.

6. Service Worker 설정

- 백그라운드 알림을 표시하려면 Service Worker가 필요합니다.
- 'DOWNLOAD ONESIGNAL SDK FILES'를 클릭해 'OneSignalSDKWorker.js' 파일을 다운로드 후 웹 어플리케이션의 public 디렉토리에 해당 파일을 저장합니다.

<br>
<p align="center">
<img src="https://user-images.githubusercontent.com/32903334/164674020-7c2d6be2-3ae5-4471-ba32-1b81088fe81f.png" width="500px">
</p>
<br>

7. 웹 어플리케이션에 OneSignal 코드를 추가합니다.

- OneSignal을 셋팅하려면 appId가 필요합니다. 위 셋팅들이 끝난 후 'Finish'를 누르면 앱이 생성됩니다. 생성된 앱의 [Settings-Keys & IDs]에서 OneSignal의 App ID를 획득 가능합니다. (아래 이미지 참조)

<br>
<p align="center">
<img src="https://user-images.githubusercontent.com/32903334/164676981-8f2131fc-a36c-4338-9133-8617890ee7b4.png" width="500px">
<br>API key는 API 호출 시 사용하므로 기억합니다.
</p>
<br>

- OneSignal은 <React.StrictMode> 내에서 호출되는 경우 init 이슈가 있습니다. 따라서 해당 컴포넌트 밖에 위치시키도록 합니다.

> index.js

```javascript
import OneSignal from "react-onesignal";

window.OneSignal = window.OneSignal || [];

OneSignal.init({
  appId: "OneSignal 내 어플리케이션 id",
});

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById("root")
);

reportWebVitals();
```

이로써 OneSignal 웹 푸시 설정이 완료되었습니다.

OneSignal init 시 추가로 설정 가능한 option과 그 외 기능들은 아래 링크에서 참고 가능합니다.

[OneSignal Init Options & API](https://documentation.onesignal.com/docs/web-push-sdk#init)

### POSTMAN으로 API 호출하기

OneSignal 푸시를 발생시키기 위해 하기 url로 POST 요청을 보내야 합니다.

https://onesignal.com/api/v1/notifications?

> Headers

```json
Content-Type:application/json
Authorization:Basic {API키}
```

[Settings-Keys & IDs] 내의 Rest API Key를 Authorization에 추가해야 합니다.
Authorization:Basic 뒤에 API키를 붙여넣습니다.
(ex. Basic A123)

> Body

```json
{
  "app_id": "app id",
  "included_segments": ["Subscribed Users"],
  "contents": { "en": "Push Message" }
}
```

POST API를 요청하면 해당 OneSignal 어플리케이션의 구독자들에게 알림이 발생합니다.
구독자들은 OneSignal 어플리케이션의 [Audience-All Users]에서 확인 가능합니다.

<br>
<p align="center">
<img src="https://user-images.githubusercontent.com/32903334/164682533-71203f18-601c-45da-acf8-15f516f80f18.png" width="500px">
<br>구독에 동의하여 Subscribed 체크 된 사용자에게만 알림이 push됩니다.
</p>
<br>

추가로 OneSignal의 Server Rest API 관련 정보는 아래 공식문서에서 확인 가능합니다.

[OneSignal Server REST API](https://documentation.onesignal.com/reference/create-notification)

주의) 크롬의 경우 OneSignal의 알림이 기본적으로 1개까지만 노출됩니다. 사용자 어플리케이션에 도착한 알림 메시지를 확인 or 지우기를 통해 처리하지 않는 경우 새 알림은 기존 알림을 덮어쓰기 때문에 알림이 오지 않은 것으로 생각되는 경우도 있습니다.

크롬에서 알림을 구분해서 띄우고 싶은 경우 위 json의 body에 'web_push_topic' 속성을 추가하고 요청마다 해당 속성의 값을 다르게 전달하면 됩니다.
