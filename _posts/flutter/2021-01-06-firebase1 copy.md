---
title: "Flutter + Firebase 1편"
subtitle: "Firebase 설정"
layout: post
author: "Minki"
# header-img: "img/seoulpark_series/seoul_park.jpg"
# header-mask: 0.6
header-style: text
catalog: true
tags:
  - Flutter
  - Firebase
---

# 들어가기

Flutter는 IOS, Android 그리고 2.0 업데이트를 거쳐 Desktop, Web까지 지원하게 됐지만 그래도 어찌됐든 프론트엔드 프레임워크다. 그렇기 때문에 단순 날씨앱이 아닌 그 이상의 상용화 가능한 앱을 만들기 위해서는 무엇보다 백엔드가 필요하다. 그러나 나는 아직 서버에 대한 지식이 많이 그것도 아주 많이 부족하기 때문에 Java 등의 언어를 이용해서 서버를 구축한다는 것은 매우 매우 어려운 일이다.

그럼 백엔드 지식이 없으면 상용화 가능한 앱을 만드는게 불가능한 일일까? 답은 'No'이다. '서버리스(Serverless)'라는 말을 들어보신 분들도 계시겠지만 최근 클라우드 서비스들이 발전하면서, 대표적으로 AWS, GCP가 있다, 서버를 직접 개발하지 않고 서버를 구축할 수 있는 개발 환경이 구축되어 있다. 이번편은 이런 클라우드 서비스들 중에서 가장 보편화되어 있고 또 저렴한 Firebase를 이용해 간단한 Note App을 구현해볼 생각이다.

Firebase 포스팅편은 유튜버 헤비프렌님의 [영상](https://www.youtube.com/channel/UCqxo_5t5-_Uhq9TfhTAat0A)을 참고하여 진행될 예정입니다.

# 1. Firebase 설정하기.

Firebase를 사용하기 위해서는 먼저 우리가 만들고 있는 앱에 Firebase를 연동해 주어야 한다. 연동 과정은 어렵지 않으므로 밑의 과정을 하나하나 따라하면 될 것이다.

## 1-1. Flutter Project 생성.

먼저 Firebase를 연동할 Flutter Project를 생성해보자.

```dart
flutter create --org com.sample1.sample2 appName
ex) flutter create --org com.sudar.snslogin sns_login_test
```

firebase와 연동하기 위해서는 bundle 및 package가 중요하기 때문에 vscode에서 생성하거나 단순 ```flutter create```대신 위의 코드를 직접 terminal에 입력하여 생성하는 편이 좋다. 여기서 주의할 점은 앱 이름에 될 수 있으면 대문자를 지양하고 소문자만 사용하도록 하자.

## 1-2. Firebase Project 생성

다음으로는 Firebase Project를 생성할 차례이다. 생성은 [파이어베이스 사이트](https://firebase.google.com/?hl=ko)에서 할 수 있다. 해당 사이트에 들어가서 로그인 한 뒤, 시작하기를 누르는 순간부터 파이어베이스 생성이 시작된다. 그럼 차근차근 살펴보도록 하자.

<img src="/img/Flutter/Firebase/firebase1.png" style="width: 500px;"/>

프로젝트 시작하기를 누르면 당연히 프로젝트 이름부터 만들어야 한다. 프로젝트 이름은 그냥 원하는대로 만들어주면 된다. 위의 이름은 그냥 예시일 뿐이므로 따라할 필요는 없다.

## 1-3. iOS Setting

프로젝트를 만들었으면 해당 프로젝트에 들어가서 앱 추가를 누른 뒤 iOS 버튼을 누르면 iOS 셋팅 페이지로 넘어갈 것이다. 그럼 지금부터 iOS Setting을 진행해보자.

<img src="/img/Flutter/Firebase/firebase2.png" style="width: 500px;"/>

맨 처음 iOS 번들 ID를 입력하는 공간이 나온다. iOS 번딜 아이디는 다음의 장소에서 발견할 수 있다.

<img src="/img/Flutter/Firebase/firebase3.png" style="width: 700px;"/>

vscode를 사용중이라면 iOS 폴더를 우클릭해 Xcode를 열어준 뒤에 'Runner -> General'부분의 'Bundle Identifier'를 확인하면 된다. 복사 붙여넣기를 통해 iOS 번들 ID에 넣어주자.

이제 다음을 누르면 'info file'을 다운받으라고 한다. 시키는대로 다운받자. 그리고 xcode의 runner 폴더에 넣어주자. 이때 그냥 드래그엔 드랍으로 넣어주지 말고 import를 통해 넣어주자.

<img src="/img/Flutter/Firebase/firebase4.png" style="width: 700px;"/>

이후 추가된 파일을 열면 reversed_client_id가 있는데 이를 복사한 뒤 Runner → info tab의 URL Types에 추가해주면 된다.

<img src="/img/Flutter/Firebase/firebase5.png" style="width: 700px;"/>

이렇게하면 iOS Setting이 끝났다. android로 넘어가자.

## 1-4. Android Setting

먼저 android 패키지 이름을 추가해주어야 한다. 이는 vscode를 연 뒤, android → app → build.gradle 파일에서 'applicationId'에서 확인할 수 있다. 여기서 minSdkVersion 이슈가 날 수도 있으니 21정도로 버전업을 해주면 해결될 수 있다.(정확한 해결책은 항상 StackOverFlow 형님들에게 물어볼것!)

<img src="/img/Flutter/Firebase/firebase6.png" style="width: 500px;"/>

이렇게 패키지 이름을 추가하면 그 다음에 디버그 서명 인증서 SH1을 등록해야한다. 이 과정에서는 key tool을 사용할 수 있다. key tool은 자바가 설치되어야 사용할 수 있으니 주의하자.

<img src="/img/Flutter/Firebase/firebase7.png" style="width: 500px;"/>

위에 링크 걸린 페이지로 이동해주고, key 툴을 terminal에 붙히면 다음과 같은 결과가 나온다. 여기서 비밀번호는 android이므로 당황하지 말자.

<img src="/img/Flutter/Firebase/firebase8.png" style="width: 500px;"/>

이제 다음으로 파일을 다운받은 뒤에 이를 app 수준에 넣어주면 된다.

<img src="/img/Flutter/Firebase/firebase9.png" style="width: 300px;"/>

마지막으로 주어진 코드들을 정해진 자리에 붙여넣어주면 된다. 밑의 그림들을 순서대로 따라해보자.
<img src="/img/Flutter/Firebase/firebase10.png" style="width: 500px;"/>
<img src="/img/Flutter/Firebase/firebase11.png" style="width: 500px;"/>
<img src="/img/Flutter/Firebase/firebase12.png" style="width: 500px;"/>
<img src="/img/Flutter/Firebase/firebase13.png" style="width: 500px;"/>
<img src="/img/Flutter/Firebase/firebase14.png" style="width: 500px;"/>
<img src="/img/Flutter/Firebase/firebase15.png" style="width: 500px;"/>

이제 Android Setting도 끝났다. 

## 1-5. Firebase Package Import

이제 iOS와 Android Setting이 모두 끝났으므로 flutter의 pubspec.yaml 파일에 firebase 관련 라이브러리를 pub.dev에서 찾아 import 해주자. import를 해주면 iOS 폴더 내에 podfile이 생기는데 만약 에러가 난다면 다음과 같이 수정함으로써 해결할 수 있다.

<img src="/img/Flutter/Firebase/firebase16.png" style="width: 500px;"/>

이제 firebase를 사용할 준비가 끝났다. 다음 포스팅에서는 Firebase Security Rule에 대해 알아보겠다.



<br>

<center>
<button type="button" class="navyBtn" onClick="location.href='https://www.paypal.me/Minki94'" style="background-color:transparent;  border:0px transparent solid;">
  이 포스팅이 도움이 되셨다면 저에케 커피 한잔 사주세요!
  <img src="https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif" alt="HTML donation button tutorial"/>
</button>
</center>
