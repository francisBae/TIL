# FCM (Firebase Cloud Messaging)

<br>
<p align="center">
<img src="https://firebase.google.com/docs/cloud-messaging/images/diagram-FCM.png" width="500px">
</p>
<br>

참고문서 : [FCM 공식 문서](https://firebase.google.com/docs/cloud-messaging?hl=ko), [React - FCM으로 웹 푸시 기능 구현하기](https://2dowon.netlify.app/react/react-fcm-web-push/), [Push notifications with React and Firebase](https://blog.logrocket.com/push-notifications-react-firebase/)

## FCM이란?

Firebase 클라우드 메시징(FCM)은 무료로 메시지를 안정적으로 전송할 수 있는 교차 플랫폼 메시징 솔루션입니다.

사용자 앱 인스턴스를 FCM 서버에 등록하면 서버에서 ID, 그룹, 주제별로 해당 기기를 식별하고 메시지를 사용자에게 전송합니다.

### 제약사항

- 웹 프로젝트의 경우 FCM이 **IE와 Safari에서는 동작하지 않습니다.**
  iOS 앱을 따로 만들면 상관 없지만, 모바일 웹만 개발하는 경우 이 점을 유의해야 합니다.

- [Firebase 공식 문서](https://firebase.google.com/support/troubleshooter/fcm/delivery/diagnose/web)에 의하면 FCM JS SDK는 [Push API-supported browsers](https://caniuse.com/push-api) 환경에서만 동작합니다.

- FCM SDK는 <font color="red">localhost와 HTTPS를 통해 제공</font>되는 페이지에서만 지원됩니다. 이는 HTTPS 사이트에서만 사용 가능한 **서비스 워커**를 사용하기 때문입니다. 제공업체 필요 시 자체 도메인에서 HTTPS 호스팅에 무료 등급을 제공하는 Firebase 호스팅을 사용하는 방법이 있습니다.

<br>

## React 에서 FCM 웹푸시 기능 구현하기

FCM은 [Firebase](https://firebase.google.com/)에서 제공해주는 서비스이기 때문에 가장 먼저 Firebase를 시작해야한다.

### Firebase 시작하기

1. 구글계정 로그인 후 Firebase 프로젝트 추가하기
<br>
<p align="center">
<img src="https://user-images.githubusercontent.com/32903334/164597430-b4c38fce-f615-4bf8-beaf-be873dc5c981.png" width="300px">
</p>
<br>

2. 웹 프로젝트를 진행 중이기 때문에 </> 버튼을 눌러서 웹 앱에 Firebase 추가를 하게 되면 하기와 같이 firebaseConfig 값을 알 수 있습니다. 해당 코드는 리액트 앱의 App.js 컴포넌트에 정의하면 됩니다.

<br>
<p align="center">
<img src="https://user-images.githubusercontent.com/32903334/164597425-fcbb4d2c-7460-43f8-bede-ec8d9e6a4b1f.png" width="250px">
</p>
<br>

#### Firebase SDK 추가

```
$ npm install firebase
```

하기 코드를 App.js에 추가합니다. (Firebase 웹앱 추가 시 해당 앱에 맞는 config 코드가 생성되므로 해당 코드를 복사해서 사용합니다.)

- 앱 생성 후 [프로젝트 설정-일반]에서도 확인 가능합니다.

> App.js

```javascript
// Import the functions you need from the SDKs you need
import { initializeApp } from "firebase/app";
import { getAnalytics } from "firebase/analytics";
// TODO: Add SDKs for Firebase products that you want to use
// https://firebase.google.com/docs/web/setup#available-libraries

// Your web app's Firebase configuration
// For Firebase JS SDK v7.20.0 and later, measurementId is optional
const firebaseConfig = {
  apiKey: "api key sample",
  authDomain: "auth domain sample",
  projectId: "project id sample",
  storageBucket: "storage bucket sample",
  messagingSenderId: "messaging sender id sample",
  appId: "app id sample",
  measurementId: "measurement id sample",
};

// Initialize Firebase
const app = initializeApp(firebaseConfig);
const analytics = getAnalytics(app);
```

3. [프로젝트 설정-클라우드 메시징] 탭에 들어가면 서버 키를 확인할 수 있습니다. (만약 백에서 등록 토큰을 준다면 프론트에서 서버 키를 몰라도 되긴 하지만, 테스트 시 사용할 수 있으니 메모해두면 좋습니다. ⚠️ 서버 키의 경우 보안이 중요하기 때문에 사용하게 된다면 환경변수를 통해 git에 올리지 않도록 주의해야 합니다.)

<br>
<p align="center">
<img src="https://user-images.githubusercontent.com/32903334/164597409-de16e275-77b2-4963-b0f0-950cc124aeea.png" width="450px">
</p>
<br>

### 리액트에서 Firebase 설정하기

FCM JavaScript API를 시작하려면 웹 앱에 Firebase를 추가하고 등록 토큰에 액세스하는 로직을 추가해야 합니다.

1. 리액트에 Firebase 모듈을 설치합니다.

   - yarn ⇒ yarn add firebase
   - npm ⇒ npm install firebase

2. firebase를 초기화하는 코드를 작성합니다. initialize 부분은 모듈로 만들어 import해도 되나 이번 가이드에서는 App.js 기준으로 설명하겠습니다.

> App.js

```javascript
import { initializeApp } from "firebase/app";

const App = () => {
  const firebaseConfig = {
    apiKey: "",
    authDomain: "",
    projectId: "",
    storageBucket: "",
    messagingSenderId: "",
    appId: "",
    measurementId: "",
  };
  const firebaseApp = initializeApp(firebaseConfig);
};
```

3. FCM 메세지 알림을 받기 위해서는 아래처럼 알림 권한을 요청 후 허용을 통해 등록 토큰을 가져와야 합니다. ~~firebase의 messaging에 있는 함수 중 하나인 requestPermission() 으로 권한을 요청하고, 허용을 누를 경우 getToken() 함수를 통해 등록 토큰을 받을 수 있습니다.~~
   => [FCM의 requestPermission()은 6.10 버전부터 f/o](https://firebase.google.com/support/release-notes/js#cloud-messaging_10) 되었으므로 네이티브 브라우저의 Notification.requestPermission()을 사용해야 합니다.

<br>
<p align="center">
<img src="https://user-images.githubusercontent.com/32903334/164603236-df023023-9fe1-4e65-8d2d-ae1ad8f15271.png" width="250px">
<br>chrome 브라우저의 알림 허용 요청
</p>
<br>

- firebase의 messaging() 함수를 통해 FCM 인스턴스를 불러오고 해당 인스턴스를 vapidKey와 함께 getToken의 함수로 전달해줍니다.

- vapidKey는 웹 푸시 인증서 키 쌍으로 FCM은 이 키 쌍이 있어야 외부 푸시 서비스에 연결이 됩니다.
- [프로젝트 설정-클라우드 메시징] 탭의 웹 구성 설정을 통해 키 쌍을 획득 가능합니다. (키 쌍이 없는 경우 Generate key pair를 통해 새 키를 생성합니다.)
<br>
<p align="center">
<img src="https://user-images.githubusercontent.com/32903334/164608296-88a6b917-d8b0-4bef-843b-786252737a3f.png" width="450px">
<br>chrome 브라우저의 알림 허용 요청
</p>
<br>

> App.js

```javascript
import { initializeApp } from "firebase/app";
import { getMessaging } from "firebase/messaging";
import * as firebaseMessaging from "firebase/messaging";

const App = () => {
  const firebaseConfig = {
    apiKey: "",
    authDomain: "",
    projectId: "",
    storageBucket: "",
    messagingSenderId: "",
    appId: "",
    measurementId: "",
  };

  const firebaseApp = initializeApp(firebaseConfig);
  const messaging = getMessaging(firebaseApp);

  Notification.requestPermission()
    .then(() => {
      return firebaseMessaging.getToken(messaging, {
        vapidKey: "vapid key 필요",
      }); // 등록 토큰을 받는 함수. messaging 객체와 vapidKey를 넣어야 합니다.
    })
    .then(function (token) {
      console.log(token); //토큰 출력
    })
    .catch(function (error) {
      console.log("FCM Error : ", error);
    });
};
```

getToken()의 결과로 수신받은 토큰은 FCM에서 바라보는 해당 사용자의 id이고, api 호출 시 해당 token 값을 json에 지정 시 해당 사용자에게 알림 전송이 가능합니다.

4. FCM 포그라운드에서는 onMessage() 함수를 통해 알림 메시지를 받을 수 있습니다. FCM은 백그라운드(화면을 보고 있지 않을 때 = 다른 사이트를 보고 있거나 브라우저가 백그라운드에 있을 때)에서는 아래와 같이 log를 찍어줄 수도 있습니다.

```javascript
import { onMessage } from 'firebase/messaging';
...
onMessage((payload) => {
  console.log(payload.notification.title);
  console.log(payload.notification.body);
});
...
```

5. 마지막으로 Service Worker 설정을 해야 백그라운드 환경인 경우에도 알림을 수신할 수 있습니다.

- 리액트에서 Service Worker를 설정하기 위해서는 public 폴더 아래에 firebase-messaging-sw.js 라는 이름의 파일을 만들어줘야 한다. (⚠️ 파일명 주의)

> firebase-messaging-sw.js

```javascript
importScripts("https://www.gstatic.com/firebasejs/8.7.1/firebase-app.js");
importScripts("https://www.gstatic.com/firebasejs/8.7.1/firebase-messaging.js");

firebase.initializeApp({
  apiKey: "",
  authDomain: "",
  projectId: "",
  storageBucket: "",
  messagingSenderId: "",
  appId: "",
  measurementId: "",
});

const messaging = firebase.messaging();
```

6. 백그라운드 알람 커스터마이징

firebase-messaging-sw.js 파일에 messaging() 함수의 호출까지만 정의가 되어있는 경우는 일반적인 notification이 옵니다. 아래는 일반적인 FCM POST api의 body 구조입니다.

```json
{
  "to": "getToken 통해 획득한 토큰",
  "notification": {
    "title": "백그라운드 알림",
    "body": "알림테스트"
  }
}
```

<br>
<p align="center">
<img src="https://user-images.githubusercontent.com/32903334/164612612-bc078e2a-0dea-4506-9133-80a5df127575.png" width="250px">
<br>위 json 으로 호출한 결과
</p>
<br>

이를 아래와 같은 코드의 추가로 커스터마이징 가능합니다.

> firebase-messaging-sw.js

```javascript
...
messaging.onBackgroundMessage(function (payload) {
  console.log('Received background message ', payload);

  const notificationTitle = payload.data.title;
  const notificationOptions = {
    body: payload.data.body,
    icon: '/logo192.png',
    data: {
      time: new Date(Date.now()).toString(),
      click_action: payload.data.click_action,
    },
  };
  return registration.showNotification(notificationTitle, notificationOptions);
});
```

json 호출 시 아래와 같이 notification 대신 data를 전달하고 임의의 알림 메시지를 registration.showNotification 메시지로 출력 가능합니다.

```json
{
  "to": "getToken 통해 획득한 토큰",
  "data": {
    "title": "백그라운드 알림",
    "body": "알림테스트",
    "click_action": "https://google.com"
  }
}
```

<br>
<p align="center">
<img src="https://user-images.githubusercontent.com/32903334/164612879-d8662af5-80cc-4ca7-a7d5-6afa8704e11a.png" width="250px">
<br>위 json 으로 호출한 결과
</p>
<br>

이 때, 클릭 이벤트도 구성 가능합니다.
위 코드와 같이 onBackgroundMessage에 click_action에 대한 정의를 추가하고, json의 데이터에 click_action 파라미터를 추가해줍니다. 그리고 firebase-messaging-sw.js 에 하기 코드를 추가해줍니다.

> firebase-messaging-sw.js

```javascript
...
self.addEventListener('notificationclick', (event) => {
  event.preventDefault();
  let action_click = event.notification.data.click_action;
  event.notification.close();

  event.waitUntil(clients.openWindow(action_click));

  return event;
});
```

아래는 서비스워커와 app.js 의 최종 소스코드입니다.

> firebase-messaging-sw.js

```javascript
importScripts("https://www.gstatic.com/firebasejs/8.10.0/firebase-app.js");
importScripts(
  "https://www.gstatic.com/firebasejs/8.10.0/firebase-messaging.js"
);

const config = {
  apiKey: "",
  authDomain: "",
  projectId: "",
  storageBucket: "",
  messagingSenderId: "",
  appId: "",
  measurementId: "",
};

firebase.initializeApp(config);

const messaging = firebase.messaging();

messaging.onBackgroundMessage(function (payload) {
  console.log("Received background message ", payload);

  const notificationTitle = payload.data.title;
  const notificationOptions = {
    body: payload.data.body,
    icon: "/logo192.png",
    data: {
      time: new Date(Date.now()).toString(),
      click_action: payload.data.click_action,
    },
  };
  return registration.showNotification(notificationTitle, notificationOptions);
});

self.addEventListener("notificationclick", (event) => {
  console.log(event);
  event.preventDefault();
  console.log("click click");
  let action_click = event.notification.data.click_action;
  event.notification.close();

  event.waitUntil(clients.openWindow(action_click));

  return event;
});
```

> App.js

```javascript
import React, { useState } from "react";
import { initializeApp } from "firebase/app";
import { getMessaging, onMessage, isSupported } from "firebase/messaging";
import * as firebaseMessaging from "firebase/messaging";

const App = () => {
  const [notificationOptions, setNotificationOptions] = useState({
    title: "",
    body: "",
  });

  //미지원 브라우저(ex. 사파리)인 경우 fcm 오류가 발생하므로 isSupported 확인 필요
  //PromiseResult : boolean 값으로 확인
  const isBrowserSupported = Promise.resolve(isSupported());

  isBrowserSupported
    .then((result) => {
      if (result === true) {
        const firebaseConfig = {
          apiKey: "",
          authDomain: "",
          projectId: "",
          storageBucket: "",
          messagingSenderId: "",
          appId: "",
          measurementId: "",
        };

        const firebaseApp = initializeApp(firebaseConfig);
        const messaging = getMessaging(firebaseApp);
        const vapidKey = "";

        Notification.requestPermission()
          .then(() => {
            return firebaseMessaging.getToken(messaging, {
              vapidKey: vapidKey,
            }); // 등록 토큰 받기
          })
          .then(function (token) {
            console.log(token); //토큰 출력
          })
          .catch(function (error) {
            console.log("FCM Error : ", error);
          });

        onMessage(messaging, (payload) => {
          console.log("foreground 상태에서 push 오는 경우 로그 확인");
        });
      } else {
        console.log("FCM webpush is not supported on current browser");
      }
    })
    .catch((err) => {
      console.log(err);
    });

  return <div>Hello World</div>;
};

export default App;
```

<br>

### POSTMAN으로 API 호출하기

FCM을 백엔드에서 호출하기 위해 하기 url로 POST 요청을 보내야 합니다.

https://fcm.googleapis.com/fcm/send

> Headers

```json
Content-Type:application/json
Authorization:key=서버키
```

[프로젝트 설정-클라우드 메시징] 내의 서버 키를 Authorization에 추가해야 합니다.
Authorization:key= 뒤에 서버키를 붙여넣습니다.

> Body

```json
{
  "to": "getToken을 통해 받은 토큰",
  "notification": {
    "title": "백그라운드 알림",
    "body": "알림테스트"
  },
  "data": {
    "title": "백그라운드 알림",
    "body": "알림테스트",
    "click_action": "클릭 시 오픈할 url"
  }
}
```

to 대신 배열 형태의 registration_ids 로 대상 토큰들을 셋팅해서 보낼 수 있는데, 이 때 배열의 갯수 제한은 1000개입니다.

주의) notification과 data가 동시에 전송되면 알림이 2회 발생합니다. 백그라운드 알림을 커스터마이징하려는 경우 notification을 제외하고 전송하면 됩니다.
