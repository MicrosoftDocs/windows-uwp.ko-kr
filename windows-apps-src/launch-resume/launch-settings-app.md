---
title: Windows 설정 앱 실행
description: 앱에서 Windows 설정 앱을 시작 하는 방법을 알아봅니다. 이 항목에서는 ms 설정 URI 체계에 대해 설명 합니다. 이 URI 체계를 사용 하 여 특정 설정 페이지에서 Windows 설정 앱을 시작 합니다.
ms.assetid: C84D4BEE-1FEE-4648-AD7D-8321EAC70290
ms.date: 04/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: 19H1
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: 1ef32816d04516bf4c8ce8677d7f682d2d59f657
ms.sourcegitcommit: db48036af630f33f0a2f7a908bfdfec945f3c241
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84437174"
---
# <a name="launch-the-windows-settings-app"></a>Windows 설정 앱 실행

**중요 API**

-   [**LaunchUriAsync**](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync)
-   [**PreferredApplicationPackageFamilyName**](https://docs.microsoft.com/uwp/api/windows.system.launcheroptions.preferredapplicationpackagefamilyname)
-   [**DesiredRemainingView**](https://docs.microsoft.com/uwp/api/windows.system.launcheroptions.desiredremainingview)

Windows 설정 앱을 시작 하는 방법을 알아봅니다. 이 항목에서는 **ms 설정:** URI 체계에 대해 설명 합니다. 이 URI 체계를 사용 하 여 특정 설정 페이지에서 Windows 설정 앱을 시작 합니다.

설정 앱을 시작 하는 것은 개인 정보 인식 앱을 작성 하는 데 중요 한 부분입니다. 앱에서 중요 한 리소스에 액세스할 수 없는 경우 해당 리소스에 대 한 개인 정보 설정에 대 한 편리한 링크를 사용자에 게 제공 하는 것이 좋습니다. 자세한 내용은 [개인 정보 인식 앱에 대 한 지침](https://docs.microsoft.com/windows/uwp/security/index)을 참조 하세요.

## <a name="how-to-launch-the-settings-app"></a>설정 앱을 시작 하는 방법

**설정** 앱을 시작 하려면 `ms-settings:` 다음 예제와 같이 URI 체계를 사용 합니다.

이 예제에서 Hyperlink XAML 컨트롤은 URI를 사용 하 여 마이크의 개인 정보 설정 페이지를 시작 하는 데 사용 됩니다 `ms-settings:privacy-microphone` .

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

또는 앱에서 [**LaunchUriAsync**](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) 메서드를 호출 하 여 **설정** 앱을 시작할 수 있습니다. 이 예제에서는 URI를 사용 하 여 카메라에 대 한 개인 정보 설정 페이지를 시작 하는 방법을 보여 줍니다 `ms-settings:privacy-webcam` .

```cs
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-webcam"));
```
```cppwinrt
bool result = co_await Windows::System::Launcher::LaunchUriAsync(Windows::Foundation::Uri(L"ms-settings:privacy-webcam"));
```

위의 코드는 카메라에 대 한 개인 정보 설정 페이지를 시작 합니다.

![카메라 개인 정보 설정.](images/privacyawarenesssettingsapp.png)

Uri를 시작 하는 방법에 대 한 자세한 내용은 [uri에 대 한 기본 응용 프로그램 시작](launch-default-app.md)을 참조 하세요.

## <a name="ms-settings-uri-scheme-reference"></a>ms-설정: URI 체계 참조

다음 Uri를 사용 하 여 설정 앱의 다양 한 페이지를 엽니다.

> 설정 페이지를 사용할 수 있는지 여부는 Windows SKU에 따라 다릅니다. 데스크톱용 Windows 10에서 사용할 수 있는 일부 설정 페이지는 Windows 10 Mobile에서 사용할 수 있으며 그 반대의 경우도 마찬가지입니다. 참고 열은 페이지를 사용할 수 있도록 하기 위해 충족 해야 하는 추가 요구 사항도 캡처합니다.

<!-- TODO: 
* ms-settings:controlcenter
* ms-settings:holographic
* ms-settings:keyboard-advanced
* ms-settings:regionlanguage-adddisplaylanguage (crashed)
* ms-settings:regionlanguage-setdisplaylanguage (crashed)
* ms-settings:signinoptions-launchpinenrollment
* ms-settings:storagecleanup
* ms-settings:update-security -->

## <a name="accounts"></a>계정

|설정 페이지| URI |
|-------------|-----|
| 회사 또는 학교 액세스 | ms-설정: 작업 공간 |
| 이메일 및 앱 계정  | ms-설정: emailandaccounts |
| 가족 및 다른 사용자 | ms-설정: otherusers |
| 키오스크 설정 | ms-설정: assignedaccess |
| 로그인 옵션 | ms-설정: signinoptions<br>ms-설정: signinoptions-dynamiclock |
| 설정 동기화 | ms-설정: 동기화 |
| Windows Hello 설치 | ms-설정: signinoptions-launchfaceenrollment<br>ms-설정: signinoptions-launchfingerprintenrollment |
| 사용자의 정보 | ms-설정: 해당 정보 |

## <a name="apps"></a>앱

|설정 페이지| URI |
|-------------|-----|
| 앱 및 기능 | ms-설정: appsfeatures |
| 앱 기능 | ms-설정: appsfeatures-앱 (앱에 대 한 추가 기능 & 다운로드 가능한 콘텐츠 등)을 관리 합니다.|
| 웹 사이트용 앱 | ms-설정: appsforwebsites |
| 기본 앱 | ms-설정: defaultapps |
| 선택적 기능 관리 | ms-설정: optionalfeatures |
| 오프 라인 맵 | ms-설정: 맵<br/>ms-설정: 맵-다운로드 맵 (maps 다운로드) |
| 시작 앱 | ms-설정: startupapps |
| 비디오 재생 | ms-설정: 비디오 재생 |

## <a name="cortana"></a>Cortana

|설정 페이지| URI |
|-------------|-----|
| 내 장치에서 Cortana | ms-설정: cortana-알림 |
| 자세한 내용 | ms-설정: cortana-moredetails |
| 사용 권한 & 기록 | ms-설정: cortana-사용 권한 |
| Windows 검색 | ms-설정: cortana-windowssearch |
| Cortana에 게 문의 | ms-설정: cortana-언어<br/>ms-설정: cortana<br/>ms-설정: cortana-talktocortana |

> [!NOTE] 
> PC가 현재 Cortana를 사용할 수 없거나 Cortana가 비활성화 된 지역으로 설정 된 경우 데스크톱의이 설정 섹션을 검색 이라고 합니다. Cortana 관련 페이지 (내 장치에서 Cortana 및 Cortana와 대화)는이 경우에 나열 되지 않습니다. 

## <a name="devices"></a>디바이스

|설정 페이지| URI |
|-------------|-----|
| 재생 | ms-설정: 자동 재생 |
| Bluetooth | ms-설정: bluetooth |
| 연결된 디바이스 | ms-설정: connecteddevices |
| 기본 카메라 | ms 설정: 카메라 (**Windows 10에서는 사용 되지 않음, 버전 1809 이상**) |
| 마우스 & 터치 패드 | ms-설정: mousetouchpad (터치 패드가 있는 장치 에서만 사용 가능한 터치 패드 설정) |
| Windows 잉크 & 펜 | ms-설정: 펜 |
| 프린터 & 스캐너 | ms-설정: 프린터 |
| 터치 패드 | ms-설정: 장치-터치 패드 (터치 패드 하드웨어가 있는 경우에만 사용 가능) |
| Typing | ms-설정: 입력 |
| USB | ms-설정: usb |
| 휠 | 밀리초-설정: 휠 (전화 접속이 페어링된 경우에만 사용 가능) |
| 휴대폰 | ms-설정: 모바일-장치  |

## <a name="ease-of-access"></a>간편한 액세스

|설정 페이지| URI |
|-------------|-----|
| 오디오 | ms-설정: easeofaccess |
| 선택 자막 | ms-설정: easeofaccess-closei 캡션 |
| 색 필터 | ms-설정: easeofaccess-colorfilter |
| 커서 및 포인터 크기 | ms-설정: easeofaccess-cursorandpointersize |
| 표시 | ms-설정: easeofaccess |
| 아이 컨트롤 | ms-설정: easeofaccess-eyecontrol |
| Fonts | ms-설정: 글꼴 |
| 고대비 | ms-설정: easeofaccess-system.windows.forms.systeminformation.highcontrast |
| 키보드 | ms-설정: easeofaccess |
| 돋보기 | ms-설정: easeofaccess-돋보기 |
| 마우스 | ms-설정: easeofaccess |
| 내레이터 | ms-설정: easeofaccess-내레이터 |
| 기타 옵션 | easeofaccess: otheroptions (**Windows 10 버전 1809 이상에서 사용 되지 않음**) |
| 음성 | ms-설정: easeofaccess-speechrecognition |

## <a name="extras"></a>추가 항목

|설정 페이지| URI |
|-------------|-----|
| 추가 항목 | ms-설정: 스페셜 (예: 타사에서 "설정 앱"이 설치 된 경우에만 사용 가능) |

## <a name="gaming"></a>게임

|설정 페이지| URI |
|-------------|-----|
| 아니거나 | ms-설정: 게임-브로드캐스팅 |
| 게임 표시줄 | ms-설정: 게임-gamebar |
| 게임 DVR | ms-설정: 게임-gamedvr |
| 게임 모드 | ms-설정: 게임-gamemode |
| 게임 전체 화면 재생 | ms-설정: quietmomentsgame |
| TruePlay | trueplay (**Windows 10, 버전 1809 이상에서 사용 되지 않음**) |
| Xbox 네트워킹 | ms-설정: 게임-xboxnetworking |

## <a name="home-page"></a>홈 페이지

|설정 페이지| URI |
|-------------|-----|
| 설정 홈 페이지 | ms-설정: |

## <a name="mixed-reality"></a>혼합 현실

> [!NOTE]
> 이러한 설정은 혼합 현실 포털 앱이 설치 된 경우에만 사용할 수 있습니다.

| 설정 페이지 | URI |
|---------------|-----|
| 오디오 및 음성 | ms-설정: holographic |
| 환경 | holographic: 개인 정보-환경 |
| 헤드셋 표시 | ms-설정: holographic-헤드셋 |
| 제거 | ms-설정: holographic |

## <a name="network--internet"></a>네트워크 & 인터넷

|설정 페이지| URI |
|-------------|-----|
| 비행기 모드 | ms-설정: 네트워크-방송 계획 emode<br/>ms-설정: 근접 |
| 셀룰러 & SIM | ms-설정: 네트워크-휴대폰 |
| 데이터 사용량 | ms-설정: datausage |
| 전화 접속 | ms-설정: 네트워크-전화 접속 |
| DirectAccess | ms-설정: 네트워크-directaccess (DirectAccess를 사용 하는 경우에만 사용 가능) |
| 이더넷 | ms-설정: 네트워크-이더넷 |
| 알려진 네트워크 관리 | ms-설정: 네트워크-wifisettings |
| 모바일 핫스팟 | ms-설정: 네트워크-mobilehotspot |
| NFC | ms-설정: nfctransactions |
| 프록시 | ms-설정: 네트워크-프록시 |
| 상태 | ms-설정: 네트워크-상태<br/>ms-설정: 네트워크 |
| VPN | ms-설정: 네트워크-vpn |
| Wi-Fi | ms-설정: 네트워크-wifi (장치에 wifi 어댑터가 있는 경우에만 사용 가능) |
| Wi-fi 호출 | wificalling: 네트워크-(Wi-fi 호출이 사용 되는 경우에만 사용 가능) |

## <a name="personalization"></a>Personalization

|설정 페이지| URI |
|-------------|-----|
| 배경 | ms-설정: 개인 설정-배경 |
| 시작에 표시 되는 폴더 선택 | ms-설정: 개인 설정-시작 위치 |
| 색 | ms-chap: 개인 설정-색<br/>ms-설정: 색 |
| 한눈 | ms-설정: 개인 설정-개요 (**Windows 10, 버전 1809 이상에서 사용 되지 않음**) |
| 잠금 화면 | ms-설정: 잠금 화면 |
| 탐색 모음 | ms-chap: 개인 설정-탐색 모음 (**Windows 10 버전 1809 이상에서 사용 되지 않음**) |
| 개인 설정 (범주) | ms-chap: 개인 설정 |
| 시작 | ms-설정: 개인 설정-시작 |
| 작업 표시줄 | ms-설정: 작업 표시줄 |
| 테마 | ms-설정: 테마 |

## <a name="phone"></a>휴대 전화

|설정 페이지| URI |
|-------------|-----|
| 휴대폰 | ms-설정: 모바일-장치<br/>ms-설정: 모바일-장치-addphone<br/>ms-설정: 모바일 장치-휴대폰-직접 ( **전화 앱을** 엽니다.) |

## <a name="privacy"></a>개인 정보 보호

|설정 페이지| URI |
|-------------|-----|
| 액세서리 앱 | ms-설정: 개인 정보-accessoryapps (**Windows 10 버전 1809 이상에서는 사용 되지 않음**) |
| 계정 정보 | ms-설정: 개인 정보-계정 정보 |
| 작업 기록 | ms-설정: 개인 정보-activityhistory |
| 광고 ID | ms-설정: 개인 정보-advertisingid (**Windows 10 버전 1809 이상에서는 사용 되지 않음**) |
| 앱 진단 | ms-설정: 개인 정보-appdiagnostics |
| 자동 파일 다운로드 | ms-설정: 개인 정보-자동 file다운로드할지. |
| 백그라운드 앱 | ms-설정: 개인 정보-backgroundapps |
| 캘린더 | ms-설정: 개인 정보-일정 |
| 통화 기록 | ms-설정: 개인 정보-callhistory |
| 카메라 | ms-설정: 개인 정보-웹캠 |
| 연락처 | ms-설정: 개인 정보-연락처 |
| 문서 | ms-설정: 개인 정보-문서 |
| Email | ms-설정: 개인 정보-전자 메일 |
| 아이 트래커 | ms-설정: 개인 정보-eyetracker (eyetracker 하드웨어 필요) |
| 피드백 & 진단 | ms-설정: 개인 정보-피드백 |
| 파일 시스템 | ms-설정: 개인 정보-broadfilesystemaccess |
| 일반 | ms-설정: 개인 정보 또는 ms 설정: 개인 정보-일반 |
| 입력 잉크 & 입력 |ms-설정: 개인 정보-speechtyping |
| 위치 | ms-설정: 개인 정보-위치 |
| 메시징 | ms-설정: 개인 정보-메시징 |
| 마이크 | ms-설정: 개인 정보-마이크 |
| 동작 | ms-설정: 개인 정보-동작 |
| 공지 | ms-설정: 개인 정보-알림 |
| 기타 디바이스 | ms-설정: 개인 정보-customdevices |
| 전화 통화 | ms-설정: 개인 정보-phonecalls |
| 사진 | ms-설정: 개인 정보-사진 |
| 무선 | ms-설정: 개인 정보-라디오 |
| 음성 | ms-설정: 개인 정보-음성 |
| 작업 | ms-설정: 개인 정보-작업 |
| 동영상 | ms-설정: 개인 정보-비디오 |
| 음성 활성화 | ms-설정: 개인 정보-voiceactivation |

## <a name="surface-hub"></a>Surface Hub

|설정 페이지| URI |
|-------------|-----|
| 계정 | ms-설정: surfacehub |
| 세션 정리 | ms-설정: surfacehub-sessioncleanup |
| 팀 회의 | ms-설정: surfacehub |
| 팀 장치 관리 | ms-설정: surfacehub-devicemanagenent |
| 시작 화면 | ms-설정: surfacehub-시작 |

## <a name="system"></a>System

|설정 페이지| URI |
|-------------|-----|
| 정보 | ms-설정: about |
| 고급 표시 설정 | ms-설정: 고급 표시 (고급 표시 옵션을 지 원하는 장치 에서만 사용 가능) |
| 앱 볼륨 및 장치 기본 설정 | 밀리초-설정: 앱-볼륨 (**Windows 10, 버전 1903에 추가 됨**)|
| 배터리 절약 모드 | ms-설정: batterysaver (태블릿 등의 배터리가 있는 장치 에서만 사용 가능) |
| 배터리 절약 설정 | ms-settings: batterysaver (태블릿 등의 배터리가 있는 장치 에서만 사용 가능) |
| 배터리 사용 | ms-설정: batterysaver-사용량 세부 정보 (태블릿 등의 배터리가 있는 장치 에서만 사용 가능) |
| 클립보드 | ms-설정: 클립보드 |
| 표시 | ms-설정: 표시 |
| 기본 저장 위치 | ms-설정: savelocations |
| 표시 | ms-설정: screenrotation |
| 내 디스플레이 복제 | ms-설정: quietmomentspresentation |
| 이러한 시간 동안 | ms-설정: quietmomentsscheduled |
| 암호화 | ms-설정: deviceencryption |
| 포커스 지원 | ms-설정: quiethours <br> ms-설정: quietmomentshome |
| 그래픽 설정 | ms-설정: advancedgraphics (고급 그래픽 옵션을 지 원하는 장치 에서만 사용 가능) |
| 메시징 | ms-설정: 메시징 |
| 멀티태스킹 | ms-설정: 멀티태스킹 |
| 야간 조명 설정 | ms-설정: nightlight |
| 휴대 전화 | ms-설정: 전화-defaultapps |
| 이 PC에 표시하는 중 | ms-설정: 프로젝트 |
| 공유 환경 | ms-설정: 교차 장치 |
| 태블릿 모드 | ms-설정: tabletmode |
| 작업 표시줄 | ms-설정: 작업 표시줄 |
| 알림 & 작업 | ms-설정: 알림 |
| 원격 데스크톱 | ms-설정: remotedesktop |
| 휴대 전화 | ms-chap: phone (**Windows 10 버전 1809 이상에서 사용 되지 않음**) |
| 전원 & 절전 모드 | ms-설정: powersleep |
| 소리 | ms-설정: 사운드 |
| 스토리지 | ms-설정: storagesense |
| 저장소 센스 | ms-설정: storagepolicies |

## <a name="time-and-language"></a>시간 및 언어

|설정 페이지| URI |
|-------------|-----|
| 날짜 및 시간 | ms-설정: dateandtime |
| 일본 IME 설정 | 밀리초-설정: 지역 언어-jpnime (Microsoft 일본 입력 방법 편집기가 설치 된 경우 사용 가능) |
| 지역 | ms-설정: 지역 서식 |
| 언어 | ms-설정: 키보드<br/>ms-설정: 지역 언어<br/>ms-설정: 지역 언어-bpmfime<br/>ms-설정: 지역 언어-cangjieime<br/>밀리초-설정: 지역 언어-chsime-병음-domainlexicon<br/>밀리초-설정: 지역 언어-chsime-병음-keyconfig<br/>밀리초-설정: 지역 언어-chsime-병음-udp<br/>wubi: 지역 언어-chsime-udp<br/>ms-설정: 지역 언어-quickime |
| 병음 IME 설정 | 밀리초-설정: 지역 언어-chsime-병음 (Microsoft 병음 input 메서드 편집기가 설치 된 경우 사용 가능) |
| 음성 | ms-설정: 음성 |
| Wubi IME 설정  | wubi: 지역 언어-chsime-(Microsoft Wubi 입력 방법 편집기가 설치 된 경우 사용 가능) |

## <a name="update--security"></a>업데이트 및 보안

|설정 페이지| URI |
|-------------|-----|
| 활성화 | ms-설정: 활성화 |
| Backup | ms-설정: 백업 |
| 배달 최적화 | ms-설정: 배달-최적화 |
| 내 장치 찾기 | ms-설정: findmydevice |
| 개발자용 | ms-설정: 개발자 |
| 복구 | ms-설정: 복구 |
| 문제 해결 | ms-설정: 문제 해결 |
| Windows 보안 | ms-설정: windowsdefender |
|  Windows 참가자 프로그램 | ms-settings: windowsinsider (사용자가 WIP에 등록 된 경우에만 있음)<br/>ms-settings: windowsinsider-optin |
| Windows Update | ms-설정: windowsupdate.log<br>ms-설정: windowsupdate.log |
| Windows 업데이트-고급 옵션 | ms-설정: windowsupdate.log |
| Windows 업데이트-다시 시작 옵션 | ms-설정: windowsupdate.log-restartoptions |
| Windows 업데이트-업데이트 기록 보기 | ms-설정: windowsupdate.log |

## <a name="user-accounts"></a>사용자 계정

|설정 페이지| URI |
|-------------|-----|
| 프로비전 | ms-설정: 작업 공간 프로 비전 (enterprise에서 프로 비전 패키지를 배포한 경우에만 사용 가능) |
| 프로비전 | ms-설정: 프로 비전 (모바일 에서만 사용 가능 및 엔터프라이즈에서 프로 비전 패키지를 배포한 경우) |
| Windows Anywhere | ms-설정: windowsanywhere (장치가 Windows Anywhere 가능 해야 함) |
