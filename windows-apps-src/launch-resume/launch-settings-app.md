---
author: TylerMSFT
title: "Windows 설정 앱 실행"
description: "앱에서 Windows 설정 앱을 시작하는 방법을 알아봅니다. 이 항목에서는 ms-settings URI 체계에 대해 설명합니다. 이 URI 스키마로 Windows 설정 앱을 실행하여 특정 설정 페이지를 표시할 수 있습니다."
ms.assetid: C84D4BEE-1FEE-4648-AD7D-8321EAC70290
translationtype: Human Translation
ms.sourcegitcommit: 1135feec72510e6cbe955161ac169158a71097b9
ms.openlocfilehash: f762d7eb70a0e9119f32350a815691109f994c75

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

다음 URI를 사용하여 설정 앱의 여러 페이지를 열 수 있습니다. 지원되는 SKU 열은 설정 페이지가 데스크톱용 Windows 10 버전(Home, Pro, Enterprise 및 Education), Windows 10 Mobile 또는 둘 다에 있는지 여부를 나타냅니다.

<table border="1">
    <tr>
        <th>범주</th>
        <th>설정 페이지</th>
        <th>지원되는 SKU</th>
        <th>URI</th>
    </tr>
    <tr>
        <td>홈페이지</td>
        <td>설정 방문 페이지</td>
        <td>모두</td>
        <td>ms-settings:</td>
    </tr>
    <tr>
        <td rowspan="10">시스템</td>
        <td>디스플레이</td>
        <td>모두</td>
        <td>ms-settings:screenrotation</td>
    </tr>
    <tr>
        <td>알림 및 동작</td>
        <td>모두</td>
        <td>ms-settings:notifications</td>
    </tr>
    <tr>
        <td>전화</td>
        <td>모바일만 해당</td>
        <td>ms-settings:phone</td>
    </tr>
    <tr>
        <td>메시지</td>
        <td>모바일만 해당</td>
        <td>ms-settings:messaging</td>
    </tr>
    <tr>
        <td>배터리 절약 모드</td>
        <td>둘 다<br>태블릿과 같은 배터리 사용 디바이스에서만 사용 가능</td>
        <td>ms-settings:batterysaver</td>
    </tr>
    <tr>
        <td>배터리 사용</td>
        <td>둘 다<br>태블릿과 같은 배터리 사용 디바이스에서만 사용 가능</td>
        <td>ms-settings:batterysaver-usagedetails</td>
    </tr>
    <tr>
        <td>전원 및 절전</td>
        <td>데스크톱에만 해당</td>
        <td>ms-settings:powersleep</td>
    </tr>
    <tr>
        <td>정보</td>
        <td>모두</td>
        <td>ms-settings:about</td>
    </tr>
    <tr>
        <td>암호화</td>
        <td>둘 다</td>
        <td>ms-settings:deviceencryption</td>
    </tr>
    <tr>
        <td>오프라인 지도</td>
        <td>모두</td>
        <td>ms-settings:maps</td>
    </tr>
    <tr>
        <td rowspan="4">디바이스</td>
        <td>기본 카메라</td>
        <td>모바일만 해당</td>
        <td>ms-settings:camera</td>
    </tr>
    <tr>
        <td>Bluetooth</td>
        <td>데스크톱에만 해당</td>
        <td>ms-settings:bluetooth</td>
    </tr>
    <tr>
        <td>연결 장치</td>
        <td>데스크톱에만 해당</td>
        <td>ms-settings:connecteddevices</td>
    </tr>
    <tr>
        <td>마우스 및 터치 패드</td>
        <td>둘 다<br>터치 패드 설정은 터치 패드가 있는 디바이스에서만 사용 가능</td>
        <td>ms-settings:mousetouchpad</td>
    </tr>
    <tr>
        <td rowspan="3">네트워크 및 무선</td>
        <td>NFC</td>
        <td>모두</td>
        <td>ms-settings:nfctransactions</td>
    </tr>
    <tr>
        <td>Wi-Fi</td>
        <td>모두</td>
        <td>ms-settings:network-wifi</td>
    </tr>
    <tr>
        <td>비행기 모드</td>
        <td>모두</td>
        <td>ms-settings:network-airplanemode</td>
    </tr>
    <tr>
        <td rowspan="5">네트워크 및 인터넷</td>
        <td>데이터 사용량</td>
        <td>모두</td>
        <td>ms-settings:datausage</td>
    </tr>
    <tr>
        <td>셀룰러 및 SIM</td>
        <td>모두</td>
        <td>ms-settings:network-cellular</td>
    </tr>
    <tr>
        <td>모바일 핫스팟</td>
        <td>모두</td>
        <td>ms-settings:network-mobilehotspot</td>
    </tr>
    <tr>
        <td>프록시</td>
        <td>데스크톱에만 해당</td>
        <td>ms-settings:network-proxy</td>
    </tr>
    <tr>
        <td>상태</td>
        <td>데스크톱에만 해당</td>
        <td>ms-settings:network-status</td>
    </tr>
    <tr>
        <td rowspan="5">개인 설정</td>
        <td>개인 설정(범주)</td>
        <td>모두</td>
        <td>ms-settings:personalization</td>
    </tr>
    <tr>
        <td>배경</td>
        <td>데스크톱에만 해당</td>
        <td>ms-settings:personalization-background</td>
    </tr>
    <tr>
        <td>색</td>
        <td>모두</td>
        <td>ms-settings:personalization-colors</td>
    </tr>
    <tr>
        <td>소리</td>
        <td>모바일만 해당 </td>
        <td>ms-settings:sounds</td>
    </tr>
    <tr>
        <td>잠금 화면</td>
        <td>모두</td>
        <td>ms-settings:lockscreen</td>
    </tr>
    <tr>
        <td rowspan="7">Accounts</td>
        <td>회사 또는 학교 계정에 액세스</td>
        <td>모두</td>
        <td>ms-settings:workplace</td>
    </tr>
    <tr>
        <td>메일 및 앱 계정</td>
        <td>모두</td>
        <td>ms-settings:emailandaccounts</td>
    </tr>
    <tr>
        <td>가족 및 다른 사용자</td>
        <td>모두</td>
        <td>ms-settings:otherusers</td>
    </tr>
    <tr>
        <td>로그인 옵션</td>
        <td>모두</td>
        <td>ms-settings:signinoptions</td>
    </tr>
    <tr>
        <td>설정 동기화</td>
        <td>모두</td>
        <td>ms-settings:sync</td>
    </tr>
    <tr>
        <td>다른 사용자</td>
        <td>모두</td>
        <td>ms-settings:otherusers</td>
    </tr>
    <tr>
        <td>사용자 정보</td>
        <td>모두</td>
        <td>ms-settings:yourinfo</td>
    </tr>
    <tr>
        <td rowspan="2">시간 및 언어</td>
        <td>날짜 및 시간</td>
        <td>모두</td>
        <td>ms-settings:dateandtime</td>
    </tr>
    <tr>
        <td>국가 및 언어</td>
        <td>데스크톱에만 해당</td>
        <td>ms-settings:regionlanguage</td>
    </tr>
    <tr>
        <td rowspan="7">접근성</td>
        <td>내레이터</td>
        <td>모두</td>
        <td>ms-settings:easeofaccess-narrator</td>
    </tr>
    <tr>
        <td>돋보기</td>
        <td>모두</td>
        <td>ms-settings:easeofaccess-magnifier</td>
    </tr>
    <tr>
        <td>고대비 </td>
        <td>모두</td>
        <td>ms-settings:easeofaccess-highcontrast</td>
    </tr>
    <tr>
        <td>자막</td>
        <td>모두</td>
        <td>ms-settings:easeofaccess-closedcaptioning</td>
    </tr>
    <tr>
        <td>키보드</td>
        <td>모두</td>
        <td>ms-settings:easeofaccess-keyboard</td>
    </tr>
    <tr>
        <td>마우스</td>
        <td>모두</td>
        <td>ms-settings:easeofaccess-mouse</td>
    </tr>
    <tr>
        <td>기타 옵션</td>
        <td>모두</td>
        <td>ms-settings:easeofaccess-otheroptions</td>
    </tr>
    <tr>
        <td rowspan="15">개인 정보</td>
        <td>위치</td>
        <td>모두</td>
        <td>ms-settings:privacy-location</td>
    </tr>
    <tr>
        <td>Camera</td>
        <td>모두</td>
        <td>ms-settings:privacy-webcam</td>
    </tr>
    <tr>
        <td>마이크</td>
        <td>모두</td>
        <td>ms-settings:privacy-microphone</td>
    </tr>
    <tr>
        <td>동작</td>
        <td>모두</td>
        <td>ms-settings:privacy-motion</td>
    </tr>
    <tr>
        <td>음성, 수동 입력 및 입력</td>
        <td>모두</td>
        <td>ms-settings:privacy-speechtyping</td>
    </tr>
    <tr>
        <td>계정 정보</td>
        <td>모두</td>
        <td>ms-settings:privacy-accountinfo</td>
    </tr>
    <tr>
        <td>연락처</td>
        <td>모두</td>
        <td>ms-settings:privacy-contacts</td>
    </tr>
    <tr>
        <td>Calendar</td>
        <td>모두</td>
        <td>ms-settings:privacy-calendar</td>
    </tr>
    <tr>
        <td>통화 기록</td>
        <td>모두</td>
        <td>ms-settings:privacy-callhistory</td>
    </tr>
    <tr>
        <td>메일</td>
        <td>모두</td>
        <td>ms-settings:privacy-email</td>
    </tr>
    <tr>
        <td>메시지</td>
        <td>모두</td>
        <td>ms-settings:privacy-messaging</td>
    </tr>
    <tr>
        <td>무선</td>
        <td>모두</td>
        <td>ms-settings:privacy-radios</td>
    </tr>
    <tr>
        <td>배경 앱</td>
        <td>모두</td>
        <td>ms-settings:privacy-backgroundapps</td>
    </tr>
    <tr>
        <td>기타 장치</td>
        <td>모두</td>
        <td>ms-settings:privacy-customdevices</td>
    </tr>
    <tr>
        <td>피드백 및 진단</td>
        <td>모두</td>
        <td>ms-settings:privacy-feedback</td>
    </tr>
    <tr>
        <td>업데이트 및 보안</td>
        <td>개발자용</td>
        <td>모두</td>
        <td>ms-settings:developers</td>
    </tr>
</table><br/>



<!--HONumber=Dec16_HO1-->


