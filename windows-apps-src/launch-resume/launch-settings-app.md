---
author: TylerMSFT
title: Windows 설정 앱 실행
description: 앱에서 Windows 설정 앱을 시작하는 방법을 알아봅니다. 이 항목에서는 ms-settings URI 체계에 대해 설명합니다. 이 URI 스키마로 Windows 설정 앱을 실행하여 특정 설정 페이지를 표시할 수 있습니다.
ms.assetid: C84D4BEE-1FEE-4648-AD7D-8321EAC70290
ms.author: twhitney
ms.date: 03/20/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b86298ca671282dea201e3088bc60845231fe007
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5839596"
---
# <a name="launch-the-windows-settings-app"></a>Windows 설정 앱 실행


**중요 API**

-   [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)
-   [**PreferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482)
-   [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314)

Windows 설정 앱을 시작하는 방법을 알아봅니다. 이 항목에서는 **ms-settings:** URI 스키마를 설명합니다. 이 URI 스키마로 Windows 설정 앱을 실행하여 특정 설정 페이지를 표시할 수 있습니다.

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

또는 앱이 [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) 메서드를 호출하여 **설정** 앱을 시작할 수 있습니다. 이 예제에서는 `ms-settings:privacy-webcam` URI를 사용하여 카메라에 대한 개인 정보 설정 페이지를 시작하는 방법을 보여 줍니다.

```cs
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-webcam"));
```

위의 코드는 카메라의 개인 정보 설정 페이지를 실행합니다.

![카메라 개인 정보 설정](images/privacyawarenesssettingsapp.png)

URI를 실행하는 방법에 대한 자세한 내용은 [URI에 대한 기본 앱 실행](launch-default-app.md)을 참조하세요.

## <a name="ms-settings-uri-scheme-reference"></a>ms-settings: URI 스키마 참조

다음 URI를 사용하여 설정 앱의 여러 페이지를 열 수 있습니다.

> 설정 페이지의 사용 가능 여부는 Windows SKU에 따라 다릅니다. Windows 10 데스크톱 버전에서 사용할 수 있는 설정 페이지 중 일부는 Windows 10 Mobile에서 사용할 수 없으며, 그 반대도 마찬가지입니다. 또한 참고 열은 페이지를 사용하기 위해 충족해야 하는 추가 요구 사항을 캡처합니다.

## <a name="accounts"></a>계정

|설정 페이지| URI |
|-------------|-----|
| 회사 또는 학교 계정에 액세스 | ms-settings:workplace |
| 메일 및 앱 계정  | ms-settings:emailandaccounts |
| 가족 및 다른 사용자 | ms-settings:otherusers |
| 로그인 옵션 | ms-settings:signinoptions<br>ms-settings:signinoptions-dynamiclock |
| 설정 동기화 | ms-settings:sync |
| Windows Hello 설정 | ms-settings:signinoptions-launchfaceenrollment<br>ms-settings:signinoptions-launchfingerprintenrollment |
| 사용자 정보 | ms-settings:yourinfo |

## <a name="apps"></a>앱

|설정 페이지| URI |
|-------------|-----|
| 앱 및 기능 | ms-settings:appsfeatures |
| 앱 기능 | ms-settings:appsfeatures-app(앱의 추가 기능 및 다운로드 가능 콘텐츠 관리, 다시 설정 등)|
| 웹 사이트용 앱 | ms-settings:appsforwebsites |
| 기본 앱 | ms-settings:defaultapps |
| 선택적 기능 관리 | ms-settings:optionalfeatures |
| 오프라인 지도 | ms-settings:maps |
| 시작 앱 | ms-settings:startupapps |
| 비디오 재생 | ms-settings:videoplayback |

## <a name="cortana"></a>Cortana

|설정 페이지| URI |
|-------------|-----|
| 사용 권한 및 기록 | ms-settings:cortana-permissions |
| 추가 정보 | ms-settings:cortana-moredetails |
| 장치에서 Cortana | ms-settings:cortana-notifications |
| Cortana에게 말하기 | ms-settings:cortana-language |

> [!NOTE] 
> 이 설정 섹션에서 데스크톱 PC 영역 Cortana가 사용 하지 않도록 하거나 Cortana를 현재 사용할 수 없는 위치에 설정 된 경우 검색을 호출 됩니다. Cortana 관련 페이지 (Cortana 내 장치 간) 및 Cortana에 음성 채팅이 경우 나열 되지 않습니다. 

## <a name="devices"></a>장치

|설정 페이지| URI |
|-------------|-----|
| 오디오 및 음성 | ms-settings:holographic-audio(Mixed Reality 포털 앱이 설치된 경우에만 사용 가능 - Microsoft Store에서 사용 가능) |
| 자동 실행 | ms-settings:autoplay |
| Bluetooth | ms-settings:bluetooth |
| 연결 장치 | ms-settings:connecteddevices |
| 기본 카메라 | ms-settings:camera |
| 마우스 및 터치 패드 | ms-settings:mousetouchpad(터치 패드가 있는 장치에서만 사용 가능한 터치 패드 설정) |
| 펜 및 Windows Ink | ms-settings:pen |
| 프린터 및 스캐너 | ms-settings:printers |
| 터치 패드 | ms-settings:devices-touchpad(터치 패드 하드웨어가 있는 경우에만 사용 가능) |
| 타이핑 | ms-settings:typing |
| USB | ms-settings:usb |
| 휠 | ms-settings:wheel(Dial이 페어링된 경우에만 사용 가능) |
| 사용자 전화 | ms-settings:mobile-devices  |

## <a name="ease-of-access"></a>접근성

|설정 페이지| URI |
|-------------|-----|
| 오디오 | ms-settings:easeofaccess-audio |
| 선택 자막 | ms-settings:easeofaccess-closedcaptioning |
| 색상 필터 | ms-설정: easeofaccess-colorfilter |
| 디스플레이 | ms-settings:easeofaccess-display |
| 아이 컨트롤 | ms-settings:easeofaccess-eyecontrol |
| 글꼴 | ms-settings:fonts |
| 고대비 | ms-settings:easeofaccess-highcontrast |
| 홀로그램 헤드셋 | ms-settings:holographic-headset(홀로그램 하드웨어 필요) |
| 키보드 | ms-settings:easeofaccess-keyboard |
| 돋보기 | ms-settings:easeofaccess-magnifier |
| 마우스 | ms-settings:easeofaccess-mouse |
| 내레이터 | ms-settings:easeofaccess-narrator |
| 기타 옵션 | ms-settings:easeofaccess-otheroptions |
| 음성 명령 | ms-settings:easeofaccess-speechrecognition |

## <a name="extras"></a>추가 콘텐츠

|설정 페이지| URI |
|-------------|-----|
| 추가 콘텐츠 | ms-settings:extras("설정 앱"이 설치된 경우에만 사용 가능, 예: 타사에서 설치) |

## <a name="gaming"></a>게임

|설정 페이지| URI |
|-------------|-----|
| 브로드캐스팅 | ms-settings:gaming-broadcasting |
| 게임 바 | ms-settings:gaming-gamebar |
| 게임 DVR | ms-settings:gaming-gamedvr |
| 게임 모드 | ms-settings:gaming-gamemode |
| 전체 화면에서 게임 플레이 | ms-settings:quietmomentsgame |
| TruePlay | ms-settings:gaming-trueplay |
| Xbox 네트워킹 | ms-settings:gaming-xboxnetworking |

## <a name="home-page"></a>홈페이지

|설정 페이지| URI |
|-------------|-----|
| 설정 홈페이지 | ms-settings: |


## <a name="network--internet"></a>네트워크 및 인터넷

|설정 페이지| URI |
|-------------|-----|
| 비행기 모드 | ms-settings:network-airplanemode(Windows 8.x에서는 ms-settings:proximity 사용) |
| 셀룰러 및 SIM | ms-settings:network-cellular |
| 데이터 사용량 | ms-settings:datausage |
| 전화 걸기 | ms-settings:network-dialup |
| DirectAccess | ms-settings:network-directaccess(DirectAccess가 활성화된 경우에만 사용 가능) |
| 이더넷 | ms-settings:network-ethernet |
| 알려진 네트워크 관리 | ms-settings:network-wifisettings |
| 모바일 핫스팟 | ms-settings:network-mobilehotspot |
| NFC | ms-settings:nfctransactions |
| 프록시 | ms-settings:network-proxy |
| 상태 | ms-settings:network-status |
| VPN | ms-settings:network-vpn |
| Wi-Fi | ms-settings:network-wifi(장치에 wifi 어댑터가 있는 경우에만 사용 가능) |
| Wi-Fi 통화 | ms-settings:network-wificalling(Wi-Fi 호출이 활성화된 경우에만 사용 가능) |

## <a name="personalization"></a>개인 설정

|설정 페이지| URI |
|-------------|-----|
| 백그라운드 | ms-settings:personalization-background |
| 시작 화면에 표시되는 폴더 선택 | ms-settings:personalization-start-places |
| 색 | ms-settings:personalization-colors |
| 한 눈에 보기 | ms-settings:personalization-glance |
| 잠금 화면 | ms-settings:lockscreen |
| 탐색 모음 | ms-settings:personalization-navbar |
| 개인 설정(범주) | ms-settings:personalization |
| 시작 화면 | ms-settings:personalization-start |
| 작업 표시줄 | ms-settings:taskbar |
| 테마 | ms-settings:themes |

## <a name="phone"></a>Phone

|설정 페이지| URI |
|-------------|-----|
| 사용자 전화 | ms-settings:mobile-devices  |

## <a name="privacy"></a>개인 정보

|설정 페이지| URI |
|-------------|-----|
| 액세서리 앱 | ms-settings:privacy-accessoryapps |
| 계정 정보 | ms-settings:privacy-accountinfo |
| 활동 기록 | ms-settings:privacy-activityhistory |
| 광고 ID | ms-settings:privacy-advertisingid |
| 앱 진단 | ms-settings:privacy-appdiagnostics |
| 자동 파일 다운로드 | ms-settings:privacy-automaticfiledownloads |
| 백그라운드 앱 | ms-settings:privacy-backgroundapps |
| 일정 | ms-settings:privacy-calendar |
| 통화 기록 | ms-settings:privacy-callhistory |
| 카메라 | ms-settings:privacy-webcam |
| 연락처 | ms-settings:privacy-contacts |
| 문서 | ms-settings:privacy-documents |
| 이메일 | ms-settings:privacy-email |
| 아이 트래커 | ms-settings:privacy-eyetracker(eyetracker 하드웨어 필요) |
| 피드백 및 진단 | ms-settings:privacy-feedback |
| 파일 시스템 | ms-settings:privacy-broadfilesystemaccess |
| 일반 | ms-settings:privacy-general |
| 위치 | ms-settings:privacy-location |
| 메시지 | ms-settings:privacy-messaging |
| 마이크 | ms-settings:privacy-microphone |
| 동작 | ms-settings:privacy-motion |
| 알림 | ms-settings:privacy-notifications |
| 기타 장치 | ms-settings:privacy-customdevices |
| 그림 | ms-settings:privacy-pictures |
| 전화 통화 | ms-settings:privacy-phonecall |
| 라디오 | ms-settings:privacy-radios |
| 음성, 수동 입력 및 입력 |ms-settings:privacy-speechtyping |
| 작업 | ms-settings:privacy-tasks |
| 비디오 | ms-settings:privacy-videos |

## <a name="surface-hub"></a>Surface Hub

|설정 페이지| URI |
|-------------|-----|
| 계정 | ms-settings:surfacehub-accounts |
| 세션 정리 | ms-settings:surfacehub-sessioncleanup |
| 팀 회의 | ms-settings:surfacehub-calling |
| 팀 장치 관리 | ms-settings:surfacehub-devicemanagenent |
| 시작 화면 | ms-settings:surfacehub-welcome |

## <a name="system"></a>시스템

|설정 페이지| URI |
|-------------|-----|
| 정보 | ms-settings:about |
| 고급 디스플레이 설정 | ms-settings:display-advanced(고급 디스플레이 옵션을 지원하는 장치에서만 사용 가능) |
| 배터리 절약 모드 | ms-settings:batterysaver(태블릿 같이 배터리가 있는 장치에서만 사용 가능) |
| 배터리 절약 모드 설정 | ms-settings:batterysaver-settings(태블릿 같이 배터리가 있는 장치에서만 사용 가능) |
| 배터리 사용 | ms-settings:batterysaver-usagedetails(태블릿 같이 배터리가 있는 장치에서만 사용 가능) |
| 디스플레이 | ms-settings:display |
| 기본 저장 위치 | ms-settings:savelocations |
| 디스플레이 | ms-settings:screenrotation |
| 내 디스플레이 복제 | ms-settings:quietmomentspresentation |
| 이 시간 동안 | ms-settings:quietmomentsscheduled |
| 암호화 | ms-settings:deviceencryption |
| 집중 지원 | ms-settings:quiethours <br> ms-settings:quietmomentshome |
| 그래픽 설정 | ms-settings:display-advancedgraphics(고급 그래픽 옵션을 지원하는 장치에서만 사용 가능) |
| 메시지 | ms-settings:messaging |
| 멀티태스킹 | ms-settings:multitasking |
| 야간 설정 | ms-settings:nightlight |
| 휴대폰 | ms-settings:phone-defaultapps |
| 이 PC에 표시 | ms-settings:project |
| 공유 환경 | ms-settings:crossdevice |
| 태블릿 모드 | ms-settings:tabletmode |
| 작업 표시줄 | ms-settings:taskbar |
| 알림 및 작업 | ms-settings:notifications |
| 원격 데스크톱 | ms-settings:remotedesktop |
| 휴대폰 | ms-settings:phone |
| 전원 및 절전 | ms-settings:powersleep |
| 소리 | ms-settings:sounds |
| 저장소 | ms-settings:storagesense |
| 저장소 센스 | ms-settings:storagepolicies |

## <a name="time-and-language"></a>시간 및 언어

|설정 페이지| URI |
|-------------|-----|
| 날짜 및 시간 | ms-settings:dateandtime |
| 일본 IME 설정 | ms-settings:regionlanguage-jpnime(Microsoft 일본 입력기가 설치된 경우 사용 가능) |
| Pinyin IME 설정 | ms-settings:regionlanguage-chsime-pinyin(Microsoft 병음 입력기가 설치된 경우 사용 가능) |
| 국가 및 언어 | ms-settings:regionlanguage |
| 음성 언어 | ms-settings:speech |
| Wubi IME 설정  | ms-settings:regionlanguage-chsime-wubi(Microsoft Wubi 입력기가 설치된 경우 사용 가능) |

## <a name="update--security"></a>업데이트 및 보안

|설정 페이지| URI |
|-------------|-----|
| 활성화 | ms-settings:activation |
| 백업 | ms-settings:backup |
| 배달 최적화 | ms-settings:delivery-optimization |
| 내 장치 찾기 | ms-settings:findmydevice |
| 개발자용 | ms-settings:developers |
| 복구 | ms-settings:recovery |
| 문제 해결 | ms-settings:troubleshoot |
| Windows 보안 | ms-settings:windowsdefender |
| Windows 참가자 프로그램 | ms-settings:windowsinsider(사용자가 WIP에 등록한 경우에만 표시) |
| Windows 업데이트 | ms-settings:windowsupdate<br>ms-settings:windowsupdate-action |
| Windows 업데이트-고급 옵션 | ms-settings:windowsupdate-options |
| Windows 업데이트-다시 시작 옵션 | ms-settings:windowsupdate-restartoptions |
| Windows 업데이트-업데이트 기록 보기 | ms-settings:windowsupdate-history |

## <a name="user--accounts"></a>사용자 계정

|설정 페이지| URI |
|-------------|-----|
| 프로비전 | ms-settings:workplace-provisioning(기업에서 프로비전 패키지를 배포한 경우에만 사용 가능) |
| 프로비전 | ms-settings:provisioning(기업에서 프로비전 패키지를 배포하고 모바일에서만 사용 가능) |
| Windows Anywhere | ms-settings:windowsanywhere(장치가 Windows Anywhere를 지원해야 함) |
