---
author: QuinnRadich
title: Windows 10의 새로운 기능
description: Windows 10 Anniversary SDK Preview 빌드 및 새로운 개발자 도구는 새로운 유니버설 Windows 플랫폼을 기반으로 하는 도구, 기능 및 환경을 제공합니다.
---

# Windows의 새로운 기능

Windows 10 Anniversary SDK Preview 빌드 14295 및 Windows 개발자 도구에 대한 업데이트는 유니버설 Windows 플랫폼에서 제공하는 도구, 기능 및 환경을 계속 제공합니다. Windows 10에 [도구 및 SDK를 설치](https://developer.microsoft.com/en-us/windows/downloads#_blank)하면 [새로운 유니버설 Windows 앱을 생성](https://msdn.microsoft.com/library/windows/apps/bg124288)하거나 [Windows의 기존 앱 코드](https://msdn.microsoft.com/library/windows/apps/mt238321)를 사용하는 방법을 알아볼 수 있습니다.

## Windows 10 Anniversary SDK Preview 빌드 12295

기능 | 설명
 :---- | :----
네트워킹 | 이제 [HttpBaseProtocolFilter.ServerCustomValidationRequest](https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpbaseprotocolfilter.aspx#_blank) 이벤트를 구독하여 서버 SSL/TLS 인증서의 자체 사용자 지정 유효성 검사를 제공할 수 있습니다. 또한 HTTP 요청에 [HttpCacheReadBehavior.NoCache](https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpcachereadbehavior.aspx#_blank) 열거 값을 지정하여 캐시에서 HTTP 응답을 완전히 읽지 못하게 설정할 수도 있습니다. 이제 [HttpBaseProtocolFilter.ClearAuthenticationCache](https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpbaseprotocolfilter.aspx#_blank) 메서드 호출을 통해 인증 자격 증명을 지워 "로그아웃" 시나리오를 사용하도록 설정할 수 있습니다.
확장 | Microsoft Edge에는 확장을 사용하는 기능이 새로 추가되었습니다. 확장을 사용하여 Microsoft Edge의 기능을 확장할 수 있으므로 대상 사용자에게 중요한 풍부한 기능을 제공할 수 있습니다. 자세한 내용은 [확장 설명서](https://developer.microsoft.com/en-us/microsoft-edge/platform/documentation/extensions/#_blank)를 참조하세요.
Bluetooth API | 이제 앱에서 원격 Bluetooth 주변 장치와 먼저 페어링하지 않고도 [Windows.Devices.Bluetooth 및 Windows.Devices.Bluetooth.Rfcomm](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.aspx#_blank)을 통해 이러한 주변 장치의 RFCOMM 서비스에 액세스할 수 있습니다. 새 메서드를 사용하여 앱에서 페어링되지 않은 디바이스의 RFCOMM 서비스를 검색하고 액세스할 수 있습니다.
채팅 API | 새 [ChatSyncManager](https://msdn.microsoft.com/library/windows/apps/mt414181.aspx#_blank) 클래스를 사용하여 클라우드와 문자 메시지를 동기화할 수 있습니다.
[Android 및 iOS 개발자용 Windows 앱 개념 매핑](https://msdn.microsoft.com/windows/uwp/porting/android-ios-uwp-map#_blank) | Android 또는 iOS 기술이나 코드를 사용하고 Windows 10 및 UWP(유니버설 Windows 플랫폼)로 이동하려는 개발자의 경우 세 가지 플랫폼 간에 플랫폼 기능과 지식을 매핑하는 데 필요한 모든 정보가 들어 있는 이 리소스를 참조하세요.
[EDP(엔터프라이즈 데이터 보호)](https://msdn.microsoft.com/windows/uwp/enterprise/edp-hub?branch=build2016#_blank) | EDP는 MDM(모바일 디바이스 관리)을 위한 데스크톱, 노트북, 태블릿 및 휴대폰의 기능 모음입니다. EDP는 엔터프라이즈에서 관리하는 디바이스에서 데이터(엔터프라이즈 파일 및 데이터 Blob)가 처리되는 방식을 보다 강력하게 제어할 수 있도록 합니다.
[Windows.ApplicationModel.AppExtensions](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appextensions.aspx#_blank) | 새로운 AppExtensions 네임스페이스를 사용하면 Windows 스토어 앱에서 다른 Windows 스토어 앱이 제공하는 콘텐츠를 호스트할 수 있습니다. 해당 앱의 읽기 전용 콘텐츠를 검색, 열거 및 액세스할 수 있습니다.
Windows IoT | Windows 10 IoT Core는 친숙한 Windows 기능을 사용하여 IoT 응용 프로그램을 만들 수 있도록 하며, 최신 Raspberry Pi 보드인 Raspberry Pi 3에서 사용할 수 있습니다.
미디어 API | Windows.Media.Playback 네임스페이스의 새 MediaBreak API를 사용하면 MediaSource 및 MediaPlaybackItem으로 미디어를 재생할 때 미디어 중단을 쉽게 예약하고 관리할 수 있습니다. Windows.Media.Audio 네임스페이스의 새 AudioGraph API는 오디오 그래프 노드에 3D 위치 송신기 및 수신기를 할당할 수 있는 공간 오디오 처리 기능을 추가적으로 제공합니다.
지도 API | 개발자가 거리가 멀거나 수평선 가까이의 시야가 심하게 경사진 지역을 제외하고 카메라 가까이에 있는 보이는 지역을 파악할 수 있도록 [MapControl](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.maps.mapcontrol.aspx#_blank)이 개선되었습니다. [MapLocationFinder](https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maplocationfinder.aspx#_blank) 클래스가 확장되어 개발자들은 원하는 정확도를 지정함으로써 역 지오코딩을 수행할 때 네트워크 트래픽을 최적화할 수 있습니다. 이제 개발자들은 [LaunchUriAsync](https://msdn.microsoft.com/library/windows/apps/hh701480.aspx#_blank) 메서드를 사용하고 경도와 위도를 지정하여 오프라인 지도를 다운로드할 수 있습니다. 자세한 내용은 [Windows 지도 앱 실행](https://msdn.microsoft.com/windows/uwp/launch-resume/launch-maps-app#_blank)을 참조하세요.


<!--HONumber=Jun16_HO3-->


