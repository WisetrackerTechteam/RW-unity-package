
### 1. 유니티 플러그인 설치 (AOS/IOS 공통 설정)

#### 1.1 유니티 패키지 다운로드
[유니티 플러그인 패키지](https://github.com/WisetrackerTechteam/RW-unity-package) 다운로드 해주세요.

#### 1.2 유니티 패키지 임포트
Unity Tools에서 Assets > Import Package > Custom Package 메뉴 선택.

다운로드 받은 **RW.unitypackage** 파일을 선택해주세요.

![]()
![]()

### 2. 유니티 안드로이드 설정

#### 2.1 strings.xml 설정 (/Assets/Plugins/Android/res/values/strings.xml)

#### a) dotAuthorizationKey 설정
발급받은 App Analytics Key 정보 추가

```xml
<!-- 예시는 샘플 코드이며, 관리자 페이지에서 직접 발급 받은 값을 적용해 주세요  -->
<string-array name="dotAuthorizationKey">
    <item name="usdMode">1</item> // 1. DOT.DOX 2. DOT
    <item name="domain">http://collector.naver.wisetracker.co.kr</item> // DOT END POINT
    <item name="domain_x">http://collector.naver.wisetracker.co.kr</item> // DOX END POINT
    <item name="serviceNumber">103</item>
    <item name="expireDate">14</item>
    <item name="isDebug">false</item>
    <item name="isInstallRetention">true</item>
    <item name="isFingerPrint">true</item>
    <item name="accessToken">access_token_string</item>
</string-array>
```

#### b) customKeyList 설정 **Custom-Key 값 사용을 원하는 경우에만 설정**
**'#'** 구분자 기준으로 **왼쪽**은 기본 사용되고 있는 키 값 **오른쪽**은 변경하고자 하는 키 값을 적용해주세요.

```xml
<!-- 예시는 디폴트 advtId 키 값을 advt_id 값으로 변경하는 설정입니다. -->
<string-array name="customKeyList">
  <item name="custom_key_value1">advtId#advt_id</item>
</string-array>
```

#### 2.2 AndroidManifest.xml 설정 (/Assets/Plugins/Android/AndroidManifest.xml)

##### a) 인스톨 레퍼러 활성화 여부

```xml
<!-- true 변경시 Wisetracker 통한 인스톨 레퍼러 미수신 -->
<meta-data 
    android:name="disableDotReceiver"
    android:value="false" />
```

##### b) 딥링크 설정

딥링크로 진입할 **android:scheme="YOUR_SCHEME"** 스키마와 **android:host="YOUR_HOST"** 호스트를 설정해 주세요.
              
```xml
<!--  예시는 wisetracker://wisetracker.co.kr 링크로 진입시 딥링크 분석이 가능 -->
<activity android:name="kr.co.wisetracker.UnityDeepLink" 
          android:launchMode="singleTop" >
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
      	<!-- 딥링크로 진입될 스키마와 호스트 정보 수정 -->
        <data android:host="wisetracker.co.kr"
              android:scheme="wisetracker" />
    </intent-filter>
</activity>
```

#### 2.2 SDK 초기화
유니티 앱 실행시 최초 실행되는 MonoBehavior 상속받아 구현된 MainScene 클래스의 Awake() 함수에 다음과 같이 초기화 코드를 삽입하세요.

```java
public class MainScene : MonoBehavior { 
    void Awake() { 
        DOT.initialization(); // initialize 코드 삽입
        DOT.setPage(new DOT_Model.Page.Builder().build()); // 기본 페이지 분석 코드 삽입
    }
}
```
