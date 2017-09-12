---
author: TylerMSFT
title: "Windows 설정 앱 실행"
description: "앱에서 Windows 설정 앱을 시작하는 방법을 알아봅니다. 이 항목에서는 ms-settings URI 체계에 대해 설명합니다. 이 URI 스키마로 Windows 설정 앱을 실행하여 특정 설정 페이지를 표시할 수 있습니다."
ms.assetid: C84D4BEE-1FEE-4648-AD7D-8321EAC70290
ms.author: twhitney
ms.date: 06/12/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 2a30e14f7c275c48f52253157fc9c67bf05d259e
ms.sourcegitcommit: 00c3f5a1208bd0125f5b275f972cf2a82d8eb9b6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2017
---
# <a name="launch-the-windows-settings-app"></a>Windows 설정 앱 실행

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

**중요 API**

-   [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)
-   [**PreferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482)
-   [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314)

앱에서 Windows 설정 앱을 실행하는 방법을 알아봅니다. 이 항목에서는 **ms-settings:** URI 스키마를 설명합니다. 이 URI 스키마로 Windows 설정 앱을 실행하여 특정 설정 페이지를 표시할 수 있습니다.

설정 앱 실행은 개인 정보 인식 앱 작성의 중요한 부분입니다. 앱에서 중요한 리소스에 액세스할 수 없는 경우 사용자에게 해당 리소스의 개인 정보 설정에 대한 편리한 링크를 제공하는 것이 좋습니다. 자세한 내용은 [개인 정보 인식 앱에 대한 지침](https://msdn.microsoft.com/library/windows/apps/hh768223)을 참조하세요.

## <a name="how-to-launch-the-settings-app"></a>설정 앱을 실행하는 방법

**설정** 앱을 시작하려면 다음 예제에 표시된 대로 `ms-settings:` URI 체계를 사용합니다.

이 예제에서는 하이퍼링크 XAML 컨트롤은 `ms-settings:privacy-microphone` URI를 사용하여 마이크에 대한 개인 정보 설정 페이지를 시작하는 데 사용됩니다.

```xml
<!--Set Visibility to Visible when access to the microphone is denied -->  
<TextBlock x:Name="LocationDisabledMessage" FontStyle="Italic"
                 Visibility="Collapsed" Margin="0,15,0,0" TextWrapping="Wrap" >
          <Run Text="This app is not able to access the microphone. Go to " />
              <Hyperlink NavigateUri="ms-settings:privacy-microphone">
                  <Run Text="Settings" />
              </Hyperlink>
          <Run Text=" to check the microphone privacy settings."/>
</TextBlock>
```

또는 앱이 [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) 메서드를 호출하여 코드에서 **설정** 앱을 실행할 수 있습니다. 이 예제에서는 `ms-settings:privacy-webcam` URI를 사용하여 카메라에 대한 개인 설정 페이지를 시작하는 방법을 보여 줍니다.

```cs
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-webcam"));
```

위의 코드는 카메라의 개인 정보 설정 페이지를 실행합니다.

![카메라 개인 정보 설정](images/privacyawarenesssettingsapp.png)

URI를 실행하는 방법에 대한 자세한 내용은 [URI에 대한 기본 앱 실행](launch-default-app.md)을 참조하세요.

## <a name="ms-settings-uri-scheme-reference"></a>ms-settings: URI 스키마 참조

다음 URI를 사용하여 설정 앱의 여러 페이지를 열 수 있습니다.

> 설정 페이지의 사용 가능 여부는 Windows SKU에 따라 다릅니다. Windows 10 데스크톱 버전에서 사용할 수 있는 설정 페이지 중 일부는 Windows 10 Mobile에서 사용할 수 없으며, 그 반대도 마찬가지입니다. 또한 참고 열은 페이지를 사용하려면 충복해야 하는 추가 요구 사항을 캡처합니다.

<table border="1">
 <tr>
  <th>범주</th>
  <th>설정 페이지</th>
  <th>URI</th>
  <th>참고</th>
 </tr>
 <tr>
  <td rowspan="6">계정</td>
  <td>회사 또는 학교 계정에 액세스</td>
  <td>ms-settings:workplace</td>
  <td></td>
 </tr>
 <tr>
  <td>메일 및 앱 계정</td>
  <td>ms-settings:emailandaccounts</td>
  <td></td>
 </tr>
 <tr>
  <td>가족 및 다른 사용자</td>
  <td>ms-settings:otherusers</td>
  <td></td>
 </tr>
 <tr>
  <td>로그인 옵션</td>
  <td>ms-settings:signinoptions</td>
  <td></td>
 </tr>
 <tr>
  <td>설정 동기화</td>
  <td>ms-settings:sync</td>
  <td></td>
 </tr>
 <tr>
  <td>사용자 정보</td>
  <td>ms-settings:yourinfo</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="4">앱</td>
  <td>앱 및 기능</td>
  <td>ms-settings:appsfeatures</td>
  <td></td>
 </tr>
 <tr>
  <td>웹 사이트용 앱</td>
  <td>ms-settings:appsforwebsites</td>
  <td></td>
 </tr>
 <tr>
  <td>기본 앱</td>
  <td>ms-settings:defaultapps</td>
  <td></td>
 </tr>
 <tr>
  <td>앱 및 기능</td>
  <td>ms-settings:optionalfeatures</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="12">장치</td>
  <td>USB</td>
  <td>ms-settings:usb</td>
  <td></td>
 </tr>
 <tr>
  <td>오디오 및 음성</td>
  <td>ms-settings:holographic-audio</td>
  <td>Mixed Reality 포털 앱이 설치된 경우에만 사용 가능(Windows 스토어에서 사용 가능)</td>
 </tr>
 <tr>
  <td>자동 실행</td>
  <td>ms-settings:autoplay</td>
  <td></td>
 </tr>
 <tr>
  <td>터치 패드</td>
  <td>ms-settings:devices-touchpad</td>
  <td>터치 패드 하드웨어가 있어야만 사용 가능</td>
 </tr>
 <tr>
  <td>펜 및 Windows Ink</td>
  <td>ms-settings:pen</td>
  <td></td>
 </tr>
 <tr>
  <td>프린터 및 스캐너</td>
  <td>ms-settings:printers</td>
  <td></td>
 </tr>
 <tr>
  <td>타이핑</td>
  <td>ms-settings:typing</td>
  <td></td>
 </tr>
 <tr>
  <td>휠</td>
  <td>ms-settings:wheel</td>
  <td>Dial이 페어링된 경우에만 사용 가능</td>
 </tr>
 <tr>
  <td>기본 카메라</td>
  <td>ms-settings:camera</td>
  <td></td>
 </tr>
 <tr>
  <td>Bluetooth</td>
  <td>ms-settings:bluetooth</td>
  <td></td>
 </tr>
 <tr>
  <td>연결 장치</td>
  <td>ms-settings:connecteddevices</td>
  <td></td>
 </tr>
 <tr>
  <td>마우스 및 터치 패드</td>
  <td>ms-settings:mousetouchpad</td>
  <td>터치 패드 설정은 터치 패드가 있는 장치에서만 사용 가능</td>
 </tr>
 <tr>
  <td rowspan="7">접근성</td>
  <td>내레이터</td>
  <td>ms-settings:easeofaccess-narrator</td>
  <td></td>
 </tr>
 <tr>
  <td>돋보기</td>
  <td>ms-settings:easeofaccess-magnifier</td>
  <td></td>
 </tr>
 <tr>
  <td>고대비</td>
  <td>ms-settings:easeofaccess-highcontrast</td>
  <td></td>
 </tr>
 <tr>
  <td>선택 자막</td>
  <td>ms-settings:easeofaccess-closedcaptioning</td>
  <td></td>
 </tr>
 <tr>
  <td>키보드</td>
  <td>ms-settings:easeofaccess-keyboard</td>
  <td></td>
 </tr>
 <tr>
  <td>마우스</td>
  <td>ms-settings:easeofaccess-mouse</td>
  <td></td>
 </tr>
 <tr>
  <td>기타 옵션</td>
  <td>ms-settings:easeofaccess-otheroptions</td>
 </tr>
 <tr>
  <td>추가 콘텐츠</td>
  <td>추가 콘텐츠</td>
  <td>ms-settings:extras</td>
  <td>"설정 앱"이 설치된 경우에만 사용 가능(예: 타사에서 설치)</td>
 </tr>
 <tr>
  <td rowspan="4">게임</td>
  <td>브로드캐스팅</td>
  <td>ms-settings:gaming-broadcasting</td>
  <td></td>
 </tr>
 <tr>
  <td>게임 바</td>
  <td>ms-settings:gaming-gamebar</td>
  <td></td>
 </tr>
 <tr>
  <td>게임 DVR</td>
  <td>ms-settings:gaming-gamedvr</td>
  <td></td>
 </tr>
 <tr>
  <td>게임 모드</td>
  <td>ms-settings:gaming-gamemode</td>
  <td></td>
 </tr>
 <tr>
  <td>홈페이지</td>
  <td>설정 방문 페이지</td>
  <td>ms-settings:</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="10">네트워크 및 인터넷</td>
  <td>이더넷</td>
  <td>ms-settings:network-ethernet</td>
  <td></td>
 </tr>
 <tr>
  <td>VPN</td>
  <td>ms-settings:network-vpn</td>
  <td></td>
 </tr>
 <tr>
  <td>전화 접속</td>
  <td>ms-settings:network-dialup</td>
  <td></td>
 </tr>
 <tr>
  <td>DirectAccess</td>
  <td>ms-settings:network-directaccess</td>
  <td>DirectAccess가 설정된 경우에만 사용 가능</td>
 </tr>
 <tr>
  <td>Wi-Fi 통화</td>
  <td>ms-settings:network-wificalling</td>
  <td>Wi-Fi 통화가 설정된 경우에만 사용 가능</td>
 </tr>
 <tr>
  <td>데이터 사용량</td>
  <td>ms-settings:datausage</td>
  <td></td>
 </tr>
 <tr>
  <td>셀룰러 및 SIM</td>
  <td>ms-settings:network-cellular</td>
  <td></td>
 </tr>
 <tr>
  <td>모바일 핫스팟</td>
  <td>ms-settings:network-mobilehotspot</td>
  <td></td>
 </tr>
 <tr>
  <td>프록시</td>
  <td>ms-settings:network-proxy</td>
  <td></td>
 </tr>
 <tr>
  <td>상태</td>
  <td>ms-settings:network-status</td>
  <td></td>
 </tr>
 <tr>
  <td>알려진 네트워크 관리</td>
  <td>ms-settings:network-wifisettings</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="3">네트워크 및 무선</td>
  <td>NFC</td>
  <td>ms-settings:nfctransactions</td>
  <td></td>
 </tr>
 <tr>
  <td>Wi-Fi</td>
  <td>ms-settings:network-wifi</td>
  <td>장치에 wifi 어댑터가 있는 경우에만 사용 가능</td>
 </tr>
 <tr>
  <td>비행기 모드</td>
  <td>ms-settings:network-airplanemode</td>
  <td>Windows 8.x에서 ms-settings:proximity 사용</td>
 </tr>
 <tr>
  <td rowspan="10">개인 설정</td>
  <td>시작</td>
  <td>ms-settings:personalization-start</td>
  <td></td>
 </tr>
 <tr>
  <td>테마</td>
  <td>ms-settings:themes</td>
  <td></td>
 </tr>
 <tr>
  <td>한 눈에 보기</td>
  <td>ms-settings:personalization-glance</td>
  <td></td>
 </tr>
 <tr>
  <td>탐색 모음</td>
  <td>ms-settings:personalization-navbar</td>
  <td></td>
 </tr>
 <tr>
  <td>개인 설정(범주)</td>
   <td>ms-settings:personalization</td>
   <td></td>
 </tr>
 <tr>
  <td>배경</td>
   <td>ms-settings:personalization-background</td>
   <td></td>
 </tr>
 <tr>
  <td>색</td>
   <td>ms-settings:personalization-colors</td>
   <td></td>
 </tr>
 <tr>
  <td>소리</td>
   <td>ms-settings:sounds</td>
   <td></td>
 </tr>
 <tr>
  <td>잠금 화면</td>
   <td>ms-settings:lockscreen</td>
   <td></td>
 </tr>
 <tr>
  <td>작업 표시줄</td>
   <td>ms-settings:taskbar</td>
   <td></td>
 </tr>
 <tr>
  <td rowspan="22">개인 정보</td>
  <td>앱 진단</td>
   <td>ms-settings:privacy-appdiagnostics</td>
   <td></td>
 </tr>
 <tr>
  <td>알림</td>
   <td>ms-settings:privacy-notifications</td>
   <td></td>
 </tr>
 <tr>
  <td>작업</td>
   <td>ms-settings:privacy-tasks</td>
   <td></td>
 </tr>
 <tr>
  <td>일반</td>
   <td>ms-settings:privacy-general</td>
   <td></td>
 </tr>
 <tr>
  <td>액세서리 앱</td>
   <td>ms-settings:privacy-accessoryapps</td>
   <td></td>
 </tr>
 <tr>
  <td>광고 ID</td>
   <td>ms-settings:privacy-advertisingid</td>
   <td></td>
 </tr>
 <tr>
  <td>전화 통화</td>
   <td>ms-settings:privacy-phonecall</td>
   <td></td>
 </tr>
 <tr>
  <td>위치</td>
   <td>ms-settings:privacy-location</td>
   <td></td>
 </tr>
 <tr>
  <td>카메라</td>
   <td>ms-settings:privacy-webcam</td>
   <td></td>
 </tr>
 <tr>
  <td>마이크</td>
   <td>ms-settings:privacy-microphone</td>
   <td></td>
 </tr>
 <tr>
  <td>동작</td>
   <td>ms-settings:privacy-motion</td>
   <td></td>
 </tr>
 <tr>
  <td>음성, 수동 입력 및 입력</td>
   <td>ms-settings:privacy-speechtyping</td>
   <td></td>
 </tr>
 <tr>
  <td>계정 정보</td>
   <td>ms-settings:privacy-accountinfo</td>
   <td></td>
 </tr>
 <tr>
  <td>연락처</td>
   <td>ms-settings:privacy-contacts</td>
   <td></td>
 </tr>
 <tr>
  <td>일정</td>
   <td>ms-settings:privacy-calendar</td>
   <td></td>
 </tr>
 <tr>
  <td>통화 기록</td>
   <td>ms-settings:privacy-callhistory</td>
   <td></td>
 </tr>
 <tr>
  <td>메일</td>
  <td>ms-settings:privacy-email</td>
  <td></td>
 </tr>
 <tr>
  <td>메시지</td>
    <td>ms-settings:privacy-messaging</td>
  <td></td>
 </tr>
 <tr>
  <td>무선</td>
    <td>ms-settings:privacy-radios</td>
  <td></td>
 </tr>
 <tr>
  <td>배경 앱</td>
    <td>ms-settings:privacy-backgroundapps</td>
  <td></td>
 </tr>
 <tr>
  <td>기타 장치</td>
    <td>ms-settings:privacy-customdevices</td>
  <td></td>
 </tr>
 <tr>
  <td>피드백 및 진단</td>
    <td>ms-settings:privacy-feedback</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="5">Surface Hub</td>
  <td>계정</td>
    <td>ms-settings:surfacehub-accounts</td>
      <td></td>
  </tr>
  <tr>
    <td>팀 회의</td>
      <td>ms-settings:surfacehub-calling</td>
      <td></td>
  </tr>
  <tr>
    <td>팀 장치 관리</td>
      <td>ms-settings:surfacehub-devicemanagenent</td>
      <td></td>
  </tr>
  <tr>
    <td>세션 정리</td>
      <td>ms-settings:surfacehub-sessioncleanup</td>
      <td></td>
  </tr>
  <tr>
    <td>시작 화면</td>
      <td>ms-settings:surfacehub-welcome</td>
      <td></td>
  </tr>
    <td rowspan="19">시스템</td>
    <td>공유 환경</td>
      <td>ms-settings:crossdevice</td>
    <td></td>
 </tr>
 <tr>
  <td>디스플레이</td>
    <td>ms-settings:display</td>
  <td></td>
 </tr>
 <tr>
  <td>멀티태스킹</td>
    <td>ms-settings:multitasking</td>
  <td></td>
 </tr>
 <tr>
  <td>이 PC에 표시</td>
    <td>ms-settings:project</td>
  <td></td>
 </tr>
 <tr>
  <td>태블릿 모드</td>
    <td>ms-settings:tabletmode</td>
  <td></td>
 </tr>
 <tr>
  <td>작업 표시줄</td>
    <td>ms-settings:taskbar</td>
  <td></td>
 </tr>
 <tr>
  <td>휴대폰</td>
    <td>ms-settings:phone-defaultapps</td>
  <td></td>
 </tr>
 <tr>
  <td>디스플레이</td>
    <td>ms-settings:screenrotation</td>
  <td></td>
 </tr>
 <tr>
  <td>알림 및 동작</td>
    <td>ms-settings:notifications</td>
  <td></td>
 </tr>
 <tr>
  <td>휴대폰</td>
    <td>ms-settings:phone</td>
  <td></td>
 </tr>
 <tr>
  <td>메시지</td>
    <td>ms-settings:messaging</td>
  <td></td>
 </tr>
 <tr>
  <td>배터리 절약 모드</td>
  <td>ms-settings:batterysaver</td>
  <td>태블릿과 같은 배터리 사용 장치에서만 사용 가능</td>
 </tr>
 <tr>
  <td>배터리 사용</td>
  <td>ms-settings:batterysaver-usagedetails</td>
  <td>태블릿과 같은 배터리 사용 장치에서만 사용 가능</td>
 </tr>
 <tr>
  <td>전원 및 절전</td>
  <td>ms-settings:powersleep</td>
  <td></td>
 </tr>
 <tr>
  <td>정보</td>
    <td>ms-settings:about</td>
  <td></td>
 </tr>
 <tr>
  <td>저장소</td>
    <td>ms-settings:storagesense</td>
  <td></td>
 </tr>
 <tr>
  <td>저장소 센스</td>
    <td>ms-settings:storagepolicies</td>
  <td></td>
 </tr>
 <tr>
  <td>암호화</td>
    <td>ms-settings:deviceencryption</td>
  <td></td>
 </tr>
 <tr>
  <td>오프라인 지도</td>
    <td>ms-settings:maps</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="5">시간 및 언어</td>
  <td>날짜 및 시간</td>
    <td>ms-settings:dateandtime</td>
  <td></td>
 </tr>
 <tr>
  <td>국가 및 언어</td>
    <td>ms-settings:regionlanguage</td>
  <td></td>
 </tr>
 <tr>
     <td>음성 언어</td>
     <td>ms-settings:speech</td>
     <td></td>
 </tr>
 <tr>
     <td>Pinyin 키보드</td>
     <td>ms-settings:regionlanguage-chsime-pinyin</td>
     <td>Microsoft Pinyin 입력기가 설치된 경우에만 사용 가능</td>
 </tr>
 <tr>
     <td>Wubi 입력 모드</td>
     <td>ms-settings:regionlanguage-chsime-wubi</td>
     <td>Microsoft Wubi 입력기가 설치된 경우에만 사용 가능</td>
 </tr>
 <tr>
  <td rowspan="13">업데이트 및 보안</td>
  <td>Windows Hello 설정</td>
    <td>ms-settings:signinoptions-launchfaceenrollment<br>ms-settings:signinoptions-launchfingerprintenrollment</td>
  </tr>
  <tr>
    <td>백업</td>
      <td>ms-settings:backup</td>
    <td></td>
 </tr>
 <tr>
  <td>내 장치 찾기</td>
    <td>ms-settings:findmydevice</td>
  <td></td>
 </tr>
 <tr>
  <td>Windows 참가자 프로그램</td>
  <td>ms-settings:windowsinsider</td>
  <td>사용자가 WIP에 등록한 경우에만 표시</td>
 </tr>
 <tr>
  <td>Windows 업데이트</td>
  <td>ms-settings:windowsupdate</td>
  <td></td>
 </tr>
 <tr>
  <td>Windows 업데이트</td>
    <td>ms-settings:windowsupdate-history</td>
  <td></td>
 </tr>
 <tr>
  <td>Windows 업데이트</td>
    <td>ms-settings:windowsupdate-options</td>
  <td></td>
 </tr>
 <tr>
  <td>Windows 업데이트</td>
    <td>ms-settings:windowsupdate-restartoptions</td>
  <td></td>
 </tr>
 <tr>
  <td>Windows 업데이트</td>
    <td>ms-settings:windowsupdate-action</td>
  <td></td>
 </tr>
 <tr>
  <td>정품 인증</td>
    <td>ms-settings:activation</td>
  <td></td>
 </tr>
 <tr>
  <td>복구</td>
    <td>ms-settings:recovery</td>
  <td></td>
 </tr>
 <tr>
  <td>문제 해결</td>
    <td>ms-settings:troubleshoot</td>
  <td></td>
 </tr>
 <tr>
  <td>Windows Defender</td>
    <td>ms-settings:windowsdefender</td>
  <td></td>
 </tr>
 <tr>
  <td>개발자용</td>
    <td>ms-settings:developers</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="2">사용자 계정</td>
  <td>Windows Anywhere</td>
  <td>ms-settings:windowsanywhere</td>
  <td>장치가 Windows Anywhere를 지원해야 함</td>
 </tr>
 <tr>
  <td>프로비전</td>
  <td>ms-settings:workplace-provisioning</td>
  <td>기업에서 프로비전 패키지를 배포한 경우에만 사용 가능</td>
 </tr>
</table><br/>  
