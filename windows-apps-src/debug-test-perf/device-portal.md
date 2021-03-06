---
ms.assetid: 60fc48dd-91a9-4dd6-a116-9292a7c1f3be
title: Windows Device Portal 개요
description: Windows Device Portal을 사용하여 네트워크 또는 USB 연결을 통해 원격으로 디바이스를 구성하고 관리할 수 있는 방법에 대해 알아봅니다.
ms.date: 01/08/2021
ms.topic: article
keywords: windows 10, uwp, 디바이스 포털
ms.localizationpriority: medium
ms.openlocfilehash: b860b081ba7693964b419def670da2f30d1c54c2
ms.sourcegitcommit: afc4ff2c89f148d32073ab1cc42063ccdc573a8c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104534"
---
# <a name="windows-device-portal-overview"></a>Windows Device Portal 개요

WDP(Windows 장치 포털)는 Windows 디바이스에 포함된 웹 서버로, 네트워크 또는 USB 연결을 통해 디바이스에 대한 설정을 구성하고 관리할 수 있게 해줍니다(디바이스에서 웹 브라우저를 사용한 로컬 연결도 지원됨).

또한 WDP는 Windows 디바이스의 문제를 해결하고 실시간 성능을 볼 수 있는 고급 진단 도구를 제공합니다.

WDP 기능은 [REST API](device-portal-api-core.md) 컬렉션을 통해 프로그래밍 방식으로 공개됩니다.

이 문서는 Windows 장치 포털에 대한 일반적인 설명과 각 디바이스 패밀리에 대한 구체적인 정보가 있는 문서의 링크를 제공합니다.

> [!NOTE]
> 디바이스 패밀리는 여러 디바이스 클래스에서 기대할 수 있는 API, 시스템 특성 및 동작을 식별합니다.

## <a name="setup"></a>설치 프로그램

각 디바이스 패밀리는 WDP 버전을 제공하지만 기능과 설정은 디바이스의 요구 사항에 따라 달라집니다.

모든 디바이스의 기본 단계는 다음과 같습니다.

1. 디바이스에서 개발자 모드와 Device Portal을 사용하도록 설정합니다(설정 앱에서 구성).

2. 로컬 네트워크 또는 USB를 통해 디바이스와 PC를 연결합니다.

3. 브라우저에서 디바이스 포털 페이지로 이동합니다. 이 표는 각 디바이스 제품군에 사용되는 포트 및 프로토콜을 보여 줍니다.

다음 표에서는 WDP의 디바이스별 세부 정보를 제공합니다.

디바이스 패밀리 | 기본 설정 여부 | HTTP | HTTPS | USB | 지침 |
--------------|----------------|------|-------|-----|--------------|
데스크톱| 개발자 모드 내에서 사용 설정 | 50080\* | 50043\* | N/A | [데스크톱 디바이스에 Windows 장치 포털 설정](device-portal-desktop.md#set-up-windows-device-portal-on-a-desktop-device) |
Xbox | 개발자 모드 내에서 사용 설정 | 사용 안 함 | 11443 | N/A | [Xbox용 디바이스 포털](../xbox-apps/device-portal-xbox.md) |
HoloLens | 예, 개발자 모드에서 | 80(기본값) | 443(기본값) | http://127.0.0.1:10080 | [HoloLens용 디바이스 포털](/windows/mixed-reality/develop/platform-capabilities-and-apis/using-the-windows-device-portal) |
IoT | 예, 개발자 모드에서 | 8080 | regkey를 통해 사용 설정 | N/A | [IoT용 디바이스 포털](/windows/iot-core/manage-your-device/DevicePortal) |
전화 | 개발자 모드 내에서 사용 설정 | 80| 443 | http://127.0.0.1:10080 | [모바일용 디바이스 포털](device-portal-mobile.md) |

\* 디바이스의 기존 포트 클레임과 충돌을 방지하기 위해 데스크톱의 Device Portal은 임시 범위(&gt;50,000)의 포트를 클레임하기 때문에 항상 적용되지는 않습니다. 자세한 정보는 [데스크톱용 Windows 장치 포털](device-portal-desktop.md)의 [레지스트리 기반 구성](device-portal-desktop.md#registry-based-configuration) 섹션을 참조하세요.  

## <a name="features"></a>기능

### <a name="toolbar-and-navigation"></a>도구 모음 및 탐색

페이지 맨 위에 있는 도구 모음에서 자주 사용하는 기능에 대한 액세스를 제공합니다.

- **전원**: 전원 옵션에 액세스합니다.
  - **종료**: 디바이스를 끕니다.
  - **다시 시작**: 디바이스를 하드 부팅합니다.
- **도움말**: 도움말 페이지를 엽니다.

페이지의 왼쪽 탐색 창에 있는 링크를 사용하여 디바이스에서 사용 가능한 관리 및 모니터링 도구로 이동합니다.

여기에서는 디바이스 패밀리 간에 공통된 도구에 대해 설명합니다. 다른 옵션은 디바이스에 따라 사용할 수 있습니다. 자세한 내용은 디바이스 유형에 대한 특정 페이지를 참조하세요.

### <a name="apps-manager"></a>앱 관리자

앱 관리자는 앱 패키지와 호스트 디바이스에 있는 번들에 대해 설치/제거 및 관리 기능을 제공합니다.

![Device Portal 앱 관리자 페이지](images/device-portal/WDP_AppsManager2.png)

* **앱 배포**: 로컬, 네트워크 또는 웹 호스트에서 패키지된 앱을 배포하고 네트워크 공유에서 느슨한 파일을 등록합니다.
* **설치된 앱**: 드롭다운 메뉴를 사용하여 디바이스에 설치된 앱을 제거하거나 시작합니다.
* **실행 중인 앱**: 현재 실행 중인 앱에 대한 정보를 가져오고 필요에 따라 닫습니다.

#### <a name="install-sideload-an-app"></a>앱 설치(테스트용으로 로드)

Windows Device Portal을 사용하여 개발하는 동안 앱을 테스트용으로 로드할 수 있습니다.

1. 앱 패키지를 만들면 이를 원격으로 디바이스에 설치할 수 있습니다. Visual Studio에서 빌드한 후 출력 폴더가 생성됩니다.

    ![앱 설치](images/device-portal/iot-installapp0.png)

2. Windows Device Portal에서 **앱 관리자** 페이지로 이동합니다.

3. **앱 배포** 섹션에서 **로컬 스토리지** 를 선택합니다.

4. **애플리케이션 패키지 선택** 에서 **파일 선택** 을 선택하고 테스트용으로 로드하려는 앱 패키지를 찾습니다.

5. **앱 패키지 서명에 사용되는 인증서 파일(.cer) 선택** 에서 **파일 선택** 을 선택하고 해당 앱 패키지와 연결된 인증서를 찾습니다.

6. 앱 설치와 함께 선택적 또는 프레임워크 패키지를 설치하려는 경우 해당 확인란을 선택하고 **다음** 을 선택하여 선택합니다.

7. **설치** 를 선택하여 설치를 시작합니다.

8. 디바이스가 S 모드에서 Windows 10을 실행하는 경우 지정된 인증서가 디바이스에 처음으로 설치된 경우입니다. 디바이스를 다시 시작합니다.

#### <a name="install-a-certificate"></a>인증서 설치

또는 Windows Device Portal을 통해 인증서를 설치하고 다른 방법으로 앱을 설치할 수 있습니다.

1. Windows Device Portal에서 **앱 관리자** 페이지로 이동합니다.

2. **앱 배포** 섹션에서 **인증서 설치** 를 선택합니다.

3. **앱 패키지 서명에 사용되는 인증서 파일(.cer) 선택** 에서 **파일 선택** 을 선택하고 테스트용으로 로드하려는 앱 패키지와 연결된 인증서를 찾습니다.

4. **설치** 를 선택하여 설치를 시작합니다.

5. 디바이스가 S 모드에서 Windows 10을 실행하는 경우 지정된 인증서가 디바이스에 처음으로 설치된 경우입니다. 디바이스를 다시 시작합니다.

#### <a name="uninstall-an-app"></a>앱 제거

1. 앱이 실행되고 있지 않은지 확인합니다.
2. 실행 중이면 **실행 중인 앱** 으로 이동하고 앱을 닫습니다. 앱이 실행되는 동안 제거를 시도할 경우 해당 앱을 다시 설치하려고 할 때 문제가 발생합니다.
3. 드롭다운에서 앱을 선택하고 **제거** 를 클릭합니다.

### <a name="running-processes"></a>실행 중인 프로세스

이 페이지에는 호스트 디바이스에서 현재 실행 중인 프로세스에 대한 세부 정보가 표시됩니다. 앱 및 시스템 프로세스 모두 포함합니다. 일부 플랫폼(데스크톱, IoT 및 HoloLens)에서 프로세스를 종료할 수 있습니다.

![Device Portal의 실행 중인 프로세스 페이지](images/device-portal/mob-device-portal-processes.png)

### <a name="file-explorer"></a>파일 탐색기

이 페이지에서는 테스트용으로 로드된 앱에서 저장한 파일을 보고 조작할 수 있습니다. 파일 탐색기 및 사용 방법에 대한 자세한 내용은 [Using the App File Explorer(앱 파일 탐색기 사용)](https://blogs.windows.com/buildingapps/2016/06/08/using-the-app-file-explorer-to-see-your-app-data/) 블로그 게시물을 참조하세요.

![Device Portal 파일 탐색기 페이지](images/device-portal/mob-device-portal-AppFileExplorer.png)

### <a name="performance"></a>성능

성능 페이지에는 전력 사용량, 프레임 속도 및 CPU 로드와 같은 시스템 진단 정보가 실시간 그래프로 표시됩니다.

사용 가능한 메트릭은 다음과 같습니다.

- **CPU**: 사용 가능한 총 CPU 사용률
- **메모리**: 전체, 사용 중, 사용 가능한 약정, 페이징 및 비페이징 메모리
- **I/O**: 데이터 읽기 및 쓰기 수량
- **네트워크**: 수신 및 전송 데이터
- **GPU**: 사용 가능한 총 GPU 엔진 사용률

![Device Portal 성능 페이지](images/device-portal/mob-device-portal-perf.png)

### <a name="event-tracing-for-windows-etw-logging"></a>ETW(Windows용 이벤트 추적) 로깅

ETW 로깅 페이지에서 디바이스의 실시간 ETW(Windows용 이벤트 추적) 정보를 관리합니다.

![Device Portal ETW 로깅 페이지](images/device-portal/mob-device-portal-etw.png)

**공급자 숨기기** 를 선택하여 이벤트 목록만 표시합니다.

- **등록된 공급자**: 이벤트 공급자와 추적 수준을 선택합니다. 추적 수준은 다음 값 중 하나입니다.
  1. 비정상적인 끝내기 또는 종료
  2. 심각한 오류
  3. 경고
  4. 오류가 아닌 경고
  5. 세부 추적

  추적을 시작하려면 **사용** 을 클릭 또는 탭합니다. 공급자가 **활성화된 공급자** 드롭다운에 추가됩니다.
- **사용자 지정 공급자**: 사용자 지정 ETW 공급자와 추적 수준을 선택합니다. 공급자를 GUID로 식별합니다. GUID에 대괄호를 포함하지 마세요.
- **활성화된 공급자**: 활성화된 공급자를 나열합니다. 추적을 중지하려면 드롭다운에서 공급자를 선택하고 **사용 안 함** 을 클릭 또는 탭합니다. 모든 추적을 일시 중단하려면 **모두 중지** 를 클릭 또는 탭합니다.
- **공급자 기록**: 현재 세션 중 활성화된 ETW 공급자를 보여 줍니다. 비활성화된 공급자를 활성화하려면 **사용** 을 클릭 또는 탭합니다. 기록을 지우려면 **지우기** 를 클릭 또는 탭합니다.
- **필터/이벤트**: **이벤트** 섹션은 선택된 공급자의 ETW 이벤트를 표 형식으로 나열합니다. 표는 실시간으로 업데이트됩니다. **필터** 메뉴를 사용하여 이벤트를 표시하는 사용자 지정 필터를 설정할 수 있습니다. 모든 ETW 이벤트를 표에서 삭제하려면 **지우기** 단추를 클릭합니다. 이렇게 해도 공급자는 비활성화되지 않습니다. **파일로 저장** 을 클릭하여 현재 수집된 ETW 이벤트를 로컬 CSV 파일로 내보낼 수 있습니다.

ETW 로깅 사용에 대한 자세한 내용은 [Use Device Portal to view debug logs(Device Portal을 사용하여 디버그 로그 보기)](https://blogs.windows.com/buildingapps/2016/06/10/using-device-portal-to-view-debug-logs-for-uwp/) 블로그 게시물을 참조하세요.

### <a name="performance-tracing"></a>성능 추적

성능 추적 페이지에서는 호스트 디바이스의 [WPR(Windows Performance Recorder)](/previous-versions/windows/it-pro/windows-8.1-and-8/hh448205(v=win.10)) 추적을 볼 수 있습니다.

![Device Portal 성능 추적 페이지](images/device-portal/mob-device-portal-perf-tracing.png)

- **사용 가능한 프로필**: 드롭다운 목록에서 WPR 프로필을 선택하고 **시작** 을 클릭 또는 탭하여 추적을 시작합니다.
- **사용자 지정 프로필**: PC에서 WPR 프로필을 선택하려면 **찾아보기** 를 클릭 또는 탭합니다. 추적을 시작하려면 **업로드 및 시작** 을 클릭 또는 탭합니다.

추적을 중지하려면 **중지** 를 클릭합니다. 추적 파일(.ETL) 다운로드가 완료될 때까지 이 페이지에 계속 있습니다.

캡처된 .ETL 파일은 [Windows Performance Analyzer](/previous-versions/windows/it-pro/windows-8.1-and-8/hh448170(v=win.10))에서 분석을 위해 열 수 있습니다.

### <a name="device-manager"></a>디바이스 관리자

디바이스 관리자 페이지는 디바이스에 연결된 모든 주변 디바이스를 열거합니다. 설정 아이콘을 클릭하여 각각의 속성을 볼 수 있습니다.

![Device Portal 디바이스 관리자 페이지](images/device-portal/mob-device-portal-devices.png)

### <a name="networking"></a>네트워킹

네트워킹 페이지는 디바이스에서 네트워크 연결을 관리합니다. USB를 통해 Device Portal에 연결하지 않는 한 이러한 설정 변경으로 인해 Device Portal과의 연결이 끊어질 수 있습니다.

- **사용 가능한 네트워크**: 디바이스에서 사용할 수 있는 WiFi 네트워크를 표시합니다. 네트워크에서 클릭 또는 탭하면 여기에 연결되며 필요한 경우 암호를 제공합니다. Device Portal은 아직 엔터프라이즈 인증을 지원하지 않습니다. **프로필** 드롭다운을 사용하여 디바이스에 알려진 모든 WiFi 프로필에 연결을 시도할 수도 있습니다.
- **IP 구성**: 각 호스트 디바이스의 네트워크 포트에 대한 주소 정보를 표시합니다.

![Device Portal 네트워킹 페이지](images/device-portal/mob-device-portal-network.png)

## <a name="service-features-and-notes"></a>서비스 기능 및 참고 사항

### <a name="dns-sd"></a>DNS-SD

디바이스 포털은 DNS-SD를 사용하여 로컬 네트워크에서 존재 여부를 알립니다. 모든 디바이스 포털 인스턴스는 디바이스 유형에 관계없이 "WDP._wdp._tcp.local"에서 알립니다. 서비스 인스턴스에 대한 TXT 레코드는 다음을 제공합니다.

키 | 유형 | 설명
----|------|-------------
S | Int | 디바이스 포털의 보안 포트입니다. 0(영)인 경우 디바이스 포털은 HTTPS 연결을 수신 대기하지 않습니다.
D | string | 디바이스의 유형입니다. "Windows.*" 형식으로 제공됩니다(예: Windows.Xbox 또는 Windows.Desktop).
A | string | 디바이스 아키텍처입니다. ARM, x86 또는 AMD64입니다.  
T | null 문자로 구분된 문자열 목록 | 디바이스에 대해 사용자가 적용한 태그입니다. 사용 방법은 태그 REST API를 참조하세요. 목록은 이중 null로 종료됩니다.  

일부 디바이스는 DNS-SD 레코드에 의해 알려진 HTTP 포트에서 수신 대기하지 않으므로 HTTPS 포트에서 연결하는 것이 좋습니다.

### <a name="csrf-protection-and-scripting"></a>CSRF 보호 및 스크립팅

[CSRF 공격](https://en.wikipedia.org/wiki/Cross-site_request_forgery)으로부터 보호하기 위해 모든 비 GET 요청에서 고유한 토큰이 필요합니다. 이 토큰(X-CSRF-Token 요청 헤더)은 세션 쿠키(CSRF-Token)에서 파생됩니다. 디바이스 포털 웹 UI에서 CSRF-Token 쿠키는 각 요청에서 X-CSRF-Token 헤더로 복사됩니다.

> [!IMPORTANT]
> 이 보호는 독립 실행형 클라이언트(예: 명령줄 유틸리티)에서 REST API의 사용을 방지합니다. 이 문제는 다음 세 가지 방법으로 해결할 수 있습니다.
> - "auto-" 사용자 이름을 사용합니다. 사용자 이름 앞에 "auto-"를 추가하는 클라이언트는 CSRF 보호를 우회하게 됩니다. 이 사용자 이름은 브라우저를 통해 디바이스 포털에 로그인하는 데 사용할 수 없습니다. 서비스가 CSRF 공격에 노출되기 때문입니다. 예제: Device Portal의 사용자 이름이 “admin”인 경우 CSRF 보호를 우회하려면 ```curl -u auto-admin:password <args>```를 사용해야 합니다.
> - 클라이언트에서 쿠키-헤더 체계를 구현합니다. 이를 위해서는 GET 요청으로 세션 쿠키를 설정한 다음 모든 후속 요청에서 헤더와 쿠키를 둘 다 포함해야 합니다.
> - 인증을 사용하지 않도록 설정하고 HTTP를 사용합니다. CSRF 보호는 HTTPS 엔드포인트에만 적용되므로 HTTP 엔드포인트의 연결에서는 위의 작업을 수행할 필요가 없습니다.

#### <a name="cross-site-websocket-hijacking-cswsh-protection"></a>CSWSH(사이트 간 WebSocket 하이재킹) 보호

[CSWSH 공격](https://www.christian-schneider.net/CrossSiteWebSocketHijacking.html)으로부터 보호하려면 디바이스 포털에 대한 WebSocket 연결을 여는 모든 클라이언트에서 호스트 헤더와 일치하는 원본 헤더도 제공해야 합니다. 이를 통해 요청이 디바이스 포털 UI 또는 유효한 클라이언트 애플리케이션에서 비롯되었음을 디바이스 포털에 입증할 수 있습니다. 원본 헤더가 없으면 요청이 거부됩니다.

## <a name="see-also"></a>추가 정보

[장치 포털 핵심 API 참조](device-portal-api-core.md)