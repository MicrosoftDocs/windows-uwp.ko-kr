---
author: TylerMSFT
title: "Windows 설정 앱 실행"
description: "앱에서 Windows 설정 앱을 시작하는 방법을 알아봅니다. 이 항목에서는 ms-settings URI 체계에 대해 설명합니다. 이 URI 스키마로 Windows 설정 앱을 실행하여 특정 설정 페이지를 표시할 수 있습니다."
ms.assetid: C84D4BEE-1FEE-4648-AD7D-8321EAC70290
translationtype: Human Translation
ms.sourcegitcommit: f90ba930db60f338ee0ebcc80934281363de01ee
ms.openlocfilehash: 249e485f74364475ff96a8256ee88bdb79749259

---

# Windows 설정 앱 실행


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


**중요 API**

-   [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)
-   [**PreferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482)
-   [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314)

앱에서 Windows 설정 앱을 실행하는 방법을 알아봅니다. 이 항목에서는 **ms-settings:** URI 스키마를 설명합니다. 이 URI 스키마로 Windows 설정 앱을 실행하여 특정 설정 페이지를 표시할 수 있습니다.

설정 앱 실행은 개인 정보 인식 앱 작성의 중요한 부분입니다. 앱에서 중요한 리소스에 액세스할 수 없는 경우 사용자에게 해당 리소스의 개인 정보 설정에 대한 편리한 링크를 제공하는 것이 좋습니다. 자세한 내용은 [개인 정보 인식 앱에 대한 지침](https://msdn.microsoft.com/library/windows/apps/hh768223)을 참조하세요.

## 설정 앱을 실행하는 방법

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
using Windows.System;
...
bool result = await Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-webcam"));
```

위의 코드는 카메라의 개인 정보 설정 페이지를 실행합니다.

![카메라 개인 정보 설정](images/privacyawarenesssettingsapp.png)



URI를 실행하는 방법에 대한 자세한 내용은 [URI에 대한 기본 앱 실행](launch-default-app.md)을 참조하세요.

## ms-settings: URI 스키마 참조


다음 URI를 사용하여 설정 앱의 여러 페이지를 열 수 있습니다. 지원되는 SKU 열은 설정 페이지가 데스크톱용 Windows 10 버전(Home, Pro, Enterprise 및 Education), Windows 10 Mobile 또는 둘 다에 있는지 여부를 나타냅니다.

| 범주           | 설정 페이지                          | 지원되는 SKU | URI                                       |
|--------------------|----------------------------------------|----------------|-------------------------------------------|
| 홈 페이지          | 설정을 위한 랜딩 페이지              | 모두           | ms-settings:                              |
| 시스템             | 디스플레이                                | 모두           | ms-settings:screenrotation                |
|                    | 알림 및 동작                | 모두           | ms-settings:notifications                 |
|                    | 전화                                  | 모바일만 해당    | ms-settings:phone                         |
|                    | 메시지                              | 모바일만 해당    | ms-settings:messaging                     |
|                    | 배터리 절약 모드                          | 태블릿과 같은 배터리 사용 디바이스의 모바일 및 데스크톱 | ms-settings:batterysaver                  |
|                    | 배터리 절약 모드 / 배터리 절약 모드 설정 | 태블릿과 같은 배터리 사용 디바이스의 모바일 및 데스크톱 | ms-settings:batterysaver-settings         |
|                    | 배터리 절약 모드 / 배터리 사용            | 태블릿과 같은 배터리 사용 디바이스의 모바일 및 데스크톱 | ms-settings:batterysaver-usagedetails     |
|                    | 전원 및 절전                          | 데스크톱에만 해당   | ms-settings:powersleep                    |
|                    | 데스크톱: 정보                         | 모두           | ms-settings:deviceencryption              |
|                    |                                        |                |                                           |
|                    | 모바일: 디바이스 암호화              |                |                                           |
|                    | 오프라인 지도                           | 모두           | ms-settings:maps                          |
|                    | 정보                                  | 모두           | ms-settings:about                         |
| 장치            | 기본 카메라                         | 모바일만 해당    | ms-settings:camera                        |
|                    | Bluetooth                              | 데스크톱에만 해당   | ms-settings:bluetooth                     |
|                    | 마우스 및 터치 패드                       | 모두           | ms-settings:mousetouchpad                 |
|                    | NFC                                    | 모두           | ms-settings:nfctransactions               |
| 네트워크 및 무선 | Wi-Fi                                  | 모두           | ms-settings:network-wifi                  |
|                    | 비행기 모드                          | 모두           | ms-settings:network-airplanemode          |
| 네트워크 및 인터넷 | 데이터 사용량                             | 모두           | ms-settings:datausage                     |
|                    | 셀룰러 및 SIM                         | 모두           | ms-settings:network-cellular              |
|                    | 모바일 핫스팟                         | 모두           | ms-settings:network-mobilehotspot         |
|                    | 프록시                                  | 모두           | ms-settings:network-proxy                 |
|                    | 상태                                 | 데스크톱에만 해당   | ms-settings:network-status                |
| 개인 설정    | 개인 설정(범주)             | 모두           | ms-settings:personalization               |
|                    | 배경                             | 데스크톱에만 해당   | ms-settings:personalization-background    |
|                    | 색                                 | 모두           | ms-settings:personalization-colors        |
|                    | 소리                                 | 모바일만 해당    | ms-settings:sounds                        |
|                    | 잠금 화면                            | 모두           | ms-settings:lockscreen                    |
| Accounts           | 회사 또는 학교 계정에 액세스                  | 모두           | ms-settings:workplace                     |
|                    | 메일 및 앱 계정                   | 모두           | ms-settings:emailandaccounts              |
|                    | 가족 및 다른 사용자                  | 모두           | ms-settings:otherusers                    |
|                    | 로그인 옵션                        | 모두           | ms-settings:signinoptions                 |
|                    | 설정 동기화                     | 모두           | ms-settings:sync                          |
|                    | 다른 사용자                           | 모두           | ms-settings:otherusers                    |
|                    | 사용자 정보                              | 모두           | ms-settings:yourinfo                      |
| 시간 및 언어  | 날짜 및 시간                            | 모두           | ms-settings:dateandtime                   |
|                    | 국가 및 언어                      | 데스크톱에만 해당   | ms-settings:regionlanguage                |
| 접근성     | 내레이터                               | 모두           | ms-settings:easeofaccess-narrator         |
|                    | 돋보기                              | 모두           | ms-settings:easeofaccess-magnifier        |
|                    | 고대비                          | 모두           | ms-settings:easeofaccess-highcontrast     |
|                    | 자막                        | 모두           | ms-settings:easeofaccess-closedcaptioning |
|                    | 키보드                               | 모두           | ms-settings:easeofaccess-keyboard         |
|                    | 마우스                                  | 모두           | ms-settings:easeofaccess-mouse            |
|                    | 기타 옵션                          | 모두           | ms-settings:easeofaccess-otheroptions     |
| 개인 정보            | 위치                               | 모두           | ms-settings:privacy-location              |
|                    | Camera                                 | 모두           | ms-settings:privacy-webcam                |
|                    | 마이크                             | 모두           | ms-settings:privacy-microphone            |
|                    | 동작                                 | 모두           | ms-settings:privacy-motion                |
|                    | 음성, 수동 입력 및 입력                | 모두           | ms-settings:privacy-speechtyping          |
|                    | 계정 정보                           | 모두           | ms-settings:privacy-accountinfo           |
|                    | 연락처                               | 모두           | ms-settings:privacy-contacts              |
|                    | Calendar                               | 모두           | ms-settings:privacy-calendar              |
|                    | 통화 기록                           | 모두           | ms-settings:privacy-callhistory           |
|                    | 메일                                  | 모두           | ms-settings:privacy-email                 |
|                    | 메시지                              | 모두           | ms-settings:privacy-messaging             |
|                    | 무선                                 | 모두           | ms-settings:privacy-radios                |
|                    | 배경 앱                        | 모두           | ms-settings:privacy-backgroundapps        |
|                    | 기타 장치                          | 모두           | ms-settings:privacy-customdevices         |
|                    | 피드백 및 진단                 | 모두           | ms-settings:privacy-feedback              |
| 업데이트 및 보안  | 개발자용                         | 모두           | ms-settings:developers                    |
 



<!--HONumber=Aug16_HO4-->


