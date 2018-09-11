---
author: PatrickFarley
ms.assetid: bf0a8b01-79f1-4944-9d78-9741e235dbe9
title: Xbox용 디바이스 포털
description: Xbox One용 디바이스 포털을 사용하는 방법에 대해 알아봅니다.
ms.author: pafarley
ms.date: 02/12/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 장치 포털
ms.localizationpriority: medium
ms.openlocfilehash: 404db3963d2f9508d7c81053abf96b0e742103f7
ms.sourcegitcommit: 72710baeee8c898b5ab77ceb66d884eaa9db4cb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/11/2018
ms.locfileid: "3848787"
---
# <a name="device-portal-for-xbox"></a>Xbox용 디바이스 포털

## <a name="set-up-device-portal-on-xbox"></a>Xbox에서 디바이스 포털 설정

다음 단계는 개발 Xbox에 대해 원격 액세스를 제공하는 Xbox 장치 포털을 구현하는 방법을 보여줍니다.

1. 개발자 홈을 엽니다. 기본값으로 개발 Xbox를 부팅하면 열리도록 되어 있지만, 홈 화면에서도 열 수 있습니다.

    ![디바이스 포털 개발자 홈](images/device-portal-xbox-1.png)

2. **Home** 탭의 개발자 홈에서 **원격 액세스** 아래 **원격 액세스 설정**을 선택합니다.

    ![장치 포털 RemoteManagement 도구](images/device-portal-xbox-15.png)

3. **Xbox 장치 포털 사용** 설정을 확인합니다.

4. **인증** 아래에서 **사용자 이름 및 암호 설정**을 선택합니다. 브라우저를 이용한 개발자 키트 액세스 인증에 사용할 **사용자 이름** 및 **암호**를 입력한 후 **저장**합니다.

5. **원격 액세스** 페이지를 **닫고** **홈** 탭의 **원격 액세스** 아래 나열된 URL을 메모합니다.

6. 브라우저에 URL을 입력한 다음 구성한 자격 증명으로 로그인합니다.

7. 제공된 인증서에 대해 아래 그림과 비슷한 경고를 받게 됩니다. Edge에서 **세부 정보**를 클릭한 다음 **웹 페이지로 이동**하여 Xbox 장치 포털에 액세스할 수 있습니다. 팝업 대화 상자에 앞서 Xbox에 입력한 사용자 이름과 암호를 입력합니다.

    ![장치 포털 인증서 오류](images/device-portal-xbox-3.png)

## <a name="device-portal-pages"></a>장치 포털 페이지

Xbox 장치 포털은 Winodws 장치 포털에서 사용할 수 있는 것과 유사한 표준 페이지 세트와 고유한 몇몇 페이지를 제공합니다. Windows 장치 포털에 대한 자세한 설명은 [Windows 장치 포털 개요](device-portal.md)를 참조하세요. 다음 섹션에서는 Xbox 장치 포털에 고유한 페이지들에 대해 설명합니다.

### <a name="home"></a>홈

**앱 관리자** 페이지의 Windows 장치 포털과 유사하게 Xbox 장치 포털 **홈** 페이지는 **내 게임 및 앱** 아래 설치된 게임과 앱의 목록을 표시합니다. 게임이나 앱의 이름을 클릭하면 **패키지 패밀리 이름** 같은 세부 정보를 확인할 수 있습니다. **작업** 드롭다운에서 **시작** 같이 게임이나 앱에 대해 작업을 할 수 있습니다.

**Xbox Live 테스트 계정** 아래에서 Xbox와 연결된 계정을 관리할 수 있습니다. 사용자와 게스트 계정을 추가하고, 새 사용자를 만들고, 사용자 로그인을 하고, 계정을 제거할 수 있습니다.

![홈](images/device-portal-xbox-16.png)

### <a name="xbox-live-game-saves"></a>Xbox Live(게임 저장)

Windows 장치 포털과 Xbox 장치 포털 모두에 **Xbox Live** 페이지가 있습니다. 그러나 Xbox 장치 포털에는 Xbox에 설치된 게임 데이터를 저장할 수 있는 **Xbox Live 게임 저장**이라는 고유 섹션이 있습니다. **SCID(서비스 구성 ID)**(자세한 정보는 [Xbox Live 서비스 구성](../xbox-live/xbox-live-service-configuration.md#get-your-ids)을 참조), **Membername (MSA)**, 제목 및 게임 저장과 연결된 **PFN(패키지 패밀리 이름)** 을 입력하고, **Input File (.json or .xml)** 을 찾고, 단추(**다시 설정**, **가져오기**, **내보내기**, **삭제**) 중 하나를 선택해 저장 데이터를 조작합니다.

**생성** 섹션에서 더미 데이터를 생성하고 지정된 입력 파일을 저장할 수 있습니다. **컨테이너(기본값 2)**, **Blob(기본값 3)**, **Blob 크기(기본값 1024)** 를 입력하고 **생성**을 선택합니다.

![Xbox Live](images/device-portal-xbox-17.png)

### <a name="http-monitor"></a>HTTP 모니터

HTTP 모니터를 사용해 Xbox One에서 실행되는 앱이나 게임의 해독된 HTTP와 HTTPS 트래픽을 볼 수 있습니다.

![HTTP 모니터](images/device-portal-xbox-18.png)

이를 사용하도록 설정하려면 Xbox One의 개발자 홈을 열어 **설정** 탭으로 간 후 **HTTP 모니터 설정** 상자에서 **HTTP 모니터를 사용하도록 설정**을 선택합니다.

![개발자 홈: 네트워킹](images/device-portal-xbox-14.png)

Xbox 장치 포털에서 사용하도록 설정한 후 각 단추를 선택해 HTTP 및 HTTPS를 **중지**, **지우기**,  **파일 저장**할 수 있습니다.

### <a name="network-fiddler-tracing"></a>네트워크(Fiddler 추적)

Xbox 장치 포털의 **네트워크** 페이지는 Windows 장치 포털의 **네트워킹** 페이지와 거의 동일합니다. 그러나 **Fiddler 추적**은 Xbox 장치 포털에 고유합니다. 이를 사용해 PC에서 Fiddler를 실행해 Xbox One과 인터넷 사이의 HTTP 및 HTTPS 트래픽을 로그하고 검사할 수 있습니다. 자세한 정보는 [UWP용으로 개발할 때 Xbox One의 Fiddler 사용 방법](../xbox-apps/uwp-fiddler.md)을 참조하세요.

![네트워크](images/device-portal-xbox-19.png)

### <a name="media-capture"></a>미디어 캡처

**미디어 캡처** 페이지에서 **스크린샷 캡처**를 선택해 Xbox One의 스크린샷을 캡처할 수 있습니다. 이렇게 하면 브라우저에 파일 다운로드가 프롬프트 됩니다. 스크린샷을 HDR로 캡처하고 싶으면(콘솔이 지원하는 경우) **HDR 선호**를 선택합니다.

![미디어 캡처](images/device-portal-xbox-12.png)

### <a name="settings"></a>설정

**설정** 페이지에서 Xbox One에 대한 설정을 보고 편집할 수 있습니다. 맨 위에서 **가져오기**를 선택해 파일에서 설정을 가져오고, **내보내기**를 선택해 현재 설정을 .txt 파일로 내보낼 수 있습니다. 가져오기 설정은 여러 콘솔을 구성하는 경우를 중심으로 더 쉽게 대량 편집을 할 수 있습니다. 가져오기를 할 설정 파일을 만들려면, 설정을 원하는 방식으로 변경한 후에 이 설정을 내보냅니다. 그런 후 이 파일을 사용해 다른 콘솔에서 쉽고 빠르게 설정을 가져올 수 있습니다.

아래에서 설명하겠지만, 보기 및 편집과 관련된 여러 설정에 대한 섹션들이 있습니다.

![설정](images/device-portal-xbox-20.png)

![설정](images/device-portal-xbox-21.png)

#### <a name="device-information"></a>장치 정보

* **장치 이름**: 장치의 이름. 편집하려면 상자의 이름을 변경한 후 **저장**을 선택합니다.

* **OS 버전**: 읽기 전용 운영 체제 버전 번호.

* **OS 버전**: 읽기 전용 운영 체제 주 릴리스의 이름.

* **Xbox Live 장치 ID**: 읽기 전용.

* **콘솔 ID**: 읽기 전용.

* **일련 번호**: 읽기 전용.

* **콘솔 유형**: 읽기 전용 Xbox One 장치 유형(Xbox One, Xbox One S 또는 Xbox One X).

* **개발자 모드**: 읽기 전용. 장치에 위치한 개발자 모드.

#### <a name="audio-settings"></a>오디오 설정

* **오디오 bitstream 형식**: 오디오 데이터 형식.

* **HDMI 오디오**: HDMI 포트를 이용하는 오디오의 유형.

* **헤드셋 형식**: 헤드폰을 통해 나오는 오디오의 형식.

* **광 오디오**: 광 포트를 통해 나오는 오디오의 유형.

* **HDMI 또는 광 오디오 헤드셋 사용**: 광 포트나 HDMI를 통해 연결되는 헤드셋을 사용하면 이 확인란을 선택합니다.

#### <a name="display-settings"></a>디스플레이 설정

* **색 농도**: 단일 픽셀의 각 색 구성 요소에 사용되는 비트의 수.

* **색 공간**: 디스플레이에서 사용할 수 있는 색 영역.

* **디스플레이 해상도**: 디스플레이의 해상도.

* **디스플레이 연결**: 디스플레이 연결 유형.

* **HDR(High Dynamic Range) 허용**: 디스플레이에 HDR을 사용할 수 있도록 설정. 호환 디스플레이에서만 사용할 수 있습니다.

* **4K 허용**: 디스플레이에 4K 해상도를 사용할 수 있도록 설정. 호환 디스플레이에서만 사용할 수 있습니다.

* **VRR(Variable Refresh Rate) 허용**: 디스플레이에 VRR을 사용할 수 있도록 설정. 호환 디스플레이에서만 사용할 수 있습니다.

#### <a name="kinect-settings"></a>Kinect 설정

Kinect 센서를 콘솔에 연결해야 이 설정을 변경할 수 있습니다.

* **Kinect 사용**: 연결된 Kinect 센서를 사용할 수 있도록 설정.

* **앱 변경 시 Kinect 다시 로드**: 다른 앱이나 게임이 실행될 때마다 연결된 Kinect 센서를 다시 로드.

#### <a name="localization-settings"></a>지역화 설정

* **지리적 지역**: 장치가 설정된 지리적 지역. 특정한 2 문자의 국가 코드여야 합니다(예: 미국은 **US**).

* **기본 설정 언어**: 장치가 설정되어 있는 언어.

* **표준 시간대**: 장치에 설정된 표준 시간대.

#### <a name="network-settings"></a>네트워크 설정

* **무선 전파 설정**: 장치의 무선 설정(무선 LAN 같은 특정 요소가 켜지거나 꺼짐).

#### <a name="power-settings"></a>전원 설정

* **유휴 상태일 때 화면이 흐려짐(몇 분 후)**: 장치가 이 시간 동안 유효 상태일 때 화면이 흐려짐. **0**으로 설정하면 화면이 흐려지지 않습니다.

* **유휴 상태일 때 화면이 꺼짐(몇 분 후)**: 장치가 이 시간 동안 유효 상태일 때 화면이 꺼짐.

* **전원 모드**: 장치의 전원 모드. 더 자세한 정보는 [절전 및 대기 전원 모드](http://support.xbox.com/xbox-one/console/learn-about-power-modes)를 참조하세요.

* **전원 연결 시 콘솔 자동 부팅**: 전원에 연결될 때 장치가 자동으로 켜짐.

#### <a name="preference-settings"></a>기본 설정

* **기본 홈 경험**: 장치를 켰을 때 표시될 홈 화면을 설정.

* **Xbox 앱에서 연결 허용**: 다른 장치(예: Windows 10 PC)의 Xbox 앱이 이 콘솔을 연결할 수 있음.

* **기본값으로 UWP 앱을 게임으로 처리**: 게임과 앱이 Xbox에 할당된 여러 리소스를 가져옴. 이 확인란을 선택하면 UWP 패키지가 게임으로 식별되어 더 많은 리소스를 가져옵니다.

#### <a name="user-settings"></a>사용자 설정

* **자동 사용자 로그인**: 장치가 켜져 있을 때 선택된 사용자를 자동 로그인.

* **자동 사용자 컨트롤러 로그인**: 특정 컨트롤로 유형과 특정 사용자를 자동으로 연결.

#### <a name="xbox-live-sandbox"></a>Xbox Live 샌드박스

장치가 위치한 Xbox Live 샌드박스를 변경할 수 있습니다. 상자에 샌드박스 이름을 입력하고 **변경**을 선택하세요.

### <a name="scratch"></a>스크래치

원하는 대로 사용자 지정할 수 있는 빈 작업 영역입니다. 메뉴를 사용해(맨 위의 메뉴 버튼 클릭) 도구를 추가할 수 있습니다(**작업 영역에 도구 추가**를 선택한 후 추가할 도구에서 **추가**를 클릭). 이 메뉴로 모든 작업 영역에 도구를 추가하고 작업 영역을 관리할 수 있습니다.

![작업 영역에 도구 추가](images/device-portal-xbox-13.png)

### <a name="game-event-data"></a>게임 이벤트 데이터

**게임 이벤트 데이터** 페이지에서 Xbox One에서 현재 기록 된 이벤트 추적 ETW (Windows 용) 게임 이벤트를 대폭 실시간 그래프에 해당 스트림 볼 수 있습니다. (이벤트 이름, 발생 한 이벤트 및 게임 타이틀) 세부 정보를 볼 수도 있습니다 시스템에 기록 된 게임 이벤트 인 경우 데이터 그래프 아래 데이터 테이블의 각 이벤트를 설명 합니다. 테이블은 이벤트 기록 된 경우에 사용할 수만 있습니다.

![게임 이벤트 데이터](images/device-portal-xbox-22.PNG)

## <a name="see-also"></a>참고 항목

* [Windows 장치 포털 개요](device-portal.md)
* [장치 포털 핵심 API 참조](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-api-core)