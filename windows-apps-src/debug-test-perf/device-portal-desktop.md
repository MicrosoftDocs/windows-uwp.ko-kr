---
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: Windows 데스크톱의 장치 포털
description: Windows Device Portal이 Windows 데스크톱의 진단 및 자동화를 제공하는 방법을 알아봅니다.
ms.date: 02/06/2019
ms.topic: article
keywords: windows 10, uwp, 장치 포털
ms.localizationpriority: medium
ms.openlocfilehash: 0f25e882f53bb4f673aa5003495f37d553208721
ms.sourcegitcommit: 445320ff0ee7323d823194d4ec9cfa6e710ed85d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72281997"
---
# <a name="device-portal-for-windows-desktop"></a>Windows 데스크톱의 장치 포털

Windows 장치 포털을 사용하면 진단 정보를 보거나 브라우저 창에서 HTTP를 통해 데스크톱을 조작할 수 있습니다. Device Portal을 사용하여 다음 작업을 수행할 수 있습니다.
- 실행 중인 프로세스 목록 확인 및 조작
- 앱 설치, 삭제, 시작 및 종료
- Wi-Fi 프로필 변경, 신호 강도 보기 및 ipconfig 확인
- CPU의 라이브 그래프, 메모리, I/O 네트워크 및 GPU 사용 보기
- 프로세스 덤프 수집
- ETW 추적 수집 
- 테스트용으로 로드된 앱의 격리된 저장소 조작

## <a name="set-up-device-portal-on-windows-desktop"></a>Window 데스크톱에서 장치 포털 설정

### <a name="turn-on-developer-mode"></a>개발자 모드 켜기

Windows 10 1607 버전부터 데스크톱의 새로운 기능 중 일부는 개발자 모드를 사용할 때에만 사용할 수 있습니다. 개발자 모드를 사용하는 방법에 대한 내용은 [개발을 위해 장치 사용](../get-started/enable-your-device-for-development.md)을 참조하세요.

> [!IMPORTANT]
> 경우에 따라 네트워크 또는 호환성 문제로 인해 개발자 모드가 제대로 설치되지 않을 수도 있습니다. 이런 문제 해결에 도움을 받으려면 [개발을 위해 장치 사용](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#failure-to-install-developer-mode-package)의 관련 섹션을 참조하세요.

### <a name="turn-on-device-portal"></a>장치 포털 켜기

**설정**의 **개발자** 섹션에서 장치 포털을 사용하도록 설정할 수 있습니다. 장치 포털을 사용하도록 설정하는 경우 사용자 이름과 암호도 만들어야 합니다. Microsoft 계정 또는 기타 Windows 자격 증명은 사용하지 마세요. 

![설정 앱의 장치 포털 섹션](images/device-portal/device-portal-desk-settings.png) 

장치 포털을 사용할 수 있도록 설정하면 섹션 아래쪽에 웹 링크가 표시됩니다. URL 목록의 끝에 첨부되어 있는 포트 번호를 적어두세요. 이 포트 번호는 장치 포털을 사용할 수 있도록 설정할 때 무작위로 생성되지만 데스크톱을 다시 부팅해도 동일하게 유지됩니다. 

이 링크를 이용해 로컬 네트워크(VPN 포함)이나 로컬 호스트의 2가지 방법 중 하나로 장치 포털에 연결할 수 있습니다.

### <a name="connect-to-device-portal"></a>장치 포털 연결

로컬 호스트를 통해 연결하려면 브라우저 창을 열고 사용한 연결 유형에 대해 여기에 표시된 주소를 입력하세요.

* Localhost: `http://127.0.0.1:<PORT>` 또는 `http://localhost:<PORT>`
* 로컬 네트워크: `https://<IP address of the desktop>:<PORT>`

인증 및 보안 통신을 위해 HTTPS가 필요합니다.

로컬 네트워크에 있는 모든 사용자를 신뢰하고, 디바이스에 개인 정보가 없고, 고유한 요구 사항이 있는 테스트 랩 같은 보호 환경에서 Device Portal을 사용하는 경우 인증 옵션을 사용하지 않도록 설정할 수 있습니다. 이렇게 하면 암호화되지 않은 통신이 가능하며 컴퓨터의 IP 주소를 가진 모든 사용자가 연결해 제어할 수 있습니다.

## <a name="device-portal-content-on-windows-desktop"></a>Windows 데스크톱의 장치 포털 콘텐츠

Windows 데스크톱의 장치 포털은 표준 페이지 세트를 제공합니다. 자세한 설명은 [Windows 장치 포털 개요](device-portal.md)를 참조하세요.

- 앱 관리자.
- 파일 탐색기
- 실행 프로세스
- 성능
- 디버그
- ETW(Windows용 이벤트 추적)
- 성능 추적
- 디바이스 관리자
- 네트워킹
- 크래시 데이터
- 기능
- 혼합 현실
- 스트리밍 설치 디버거
- 위치
- 스크래치

## <a name="more-device-portal-options"></a>기타 장치 포털 옵션

### <a name="registry-based-configuration-for-device-portal"></a>장치 포털의 레지스트리 기반 구성

장치 포털용 포트 번호(예: 80 및 443)를 선택하려는 경우 다음 레지스트리 키를 설정할 수 있습니다.

- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WebManagement\Service`에서
    - `UseDynamicPorts`: 필수 DWORD입니다. 선택한 포트 번호를 유지하려면 이 항목을 0으로 설정합니다.
    - `HttpPort`: 필수 DWORD입니다. 장치 포털이 HTTP 연결을 수신 대기하는 포트 번호를 포함합니다.    
    - `HttpsPort`: 필수 DWORD입니다. Device Portal이 HTTPS 연결을 수신 대기하는 포트 번호를 포함합니다.
    
동일한 regkey 경로에서 인증 요구 사항을 헤제할 수 있습니다.
- 사용 하지 않도록 설정 된  - `0` `UseDefaultAuthorizer`사용 하도록 `1`.  
    - 이는 각 연결과 HTTP에서 HTTPS로의 기본적인 인증 요구 사항을 모두 제어합니다.  
    
### <a name="command-line-options-for-device-portal"></a>장치 포털의 명령줄 옵션
관리자 명령 프롬프트에서 장치 포털의 일부를 사용하도록 설정하고 구성할 수 있습니다. 빌드에 대해 지원 되는 최신 명령 집합을 보려면 `webmanagement /?`를 실행 하면 됩니다.

- `sc start webmanagement` 또는 `sc stop webmanagement` 
    - 서비스 켜기 또는 끄기. 이 경우에도 개발자 모드를 사용하도록 설정해야 합니다. 
- `-Credentials <username> <password>` 
    - 장치 포털에 대한 사용자 이름 및 암호를 설정합니다. 사용자 이름은 기본 인증 표준을 준수 해야 하므로 콜론 (:)을 포함할 수 없습니다. 및는 표준 ASCII 문자 (예: [A-za-z0-9])를 기반으로 작성 해야 합니다. 브라우저는 표준 방식으로 전체 문자 집합을 구문 분석 하지 않습니다.  
- `-DeleteSSL` 
    - 이는 HTTPS 연결에 사용되는 SSL 인증서 캐시를 다시 설정합니다. (예상했던 인증서 경고가 아닌)무시할 수 없는 TLS 연결 오류가 발생하는 경우, 이 옵션으로 문제를 해결할 수도 있습니다. 
- `-SetCert <pfxPath> <pfxPassword>`
    - 자세한 내용은 [사용자 지정 SSL 인증서로 장치 포털 프로비전](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-ssl)을 참조하세요.  
    - 그러면 자신의 SSL 인증서를 설치해 장치 포털에 자주 표시되는 SSL 경고 페이지를 해결할 수 있습니다. 
- `-Debug <various options for authentication, port selection, and tracing level>`
    - 특정한 구성과 표시되는 디버그 메시지로 독립 실행형 장치 포털 버전을 실행합니다. 이는 [패키지된 플러그인](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-plugin) 구성에 가장 유용합니다. 
    - 패키지된 플러그인을 테스트하기 위해 이를 실행하는 방법에 대한 자세한 내용은 [MSDN 잡지 문서](https://msdn.microsoft.com/en-us/magazine/mt826332.aspx)를 참조하세요.

## <a name="common-errors-and-issues"></a>일반적인 오류 및 문제

다음은 장치 포털을 설정할 때 발생할 수 있는 몇 가지 일반적인 오류입니다.

### <a name="windowsupdatesearch-returns-invalid-number-of-updates-0x800f0950-cbs_e_invalid_windows_update_count"></a>WindowsUpdateSearch는 잘못 된 업데이트 수를 반환 합니다 (0x800f0950 CBS_E_INVALID_WINDOWS_UPDATE_COUNT).

Windows 10의 시험판 빌드에 개발자 패키지를 설치 하려고 하면이 오류가 발생할 수 있습니다. 이러한 주문형 (주문형) 패키지는 Windows 업데이트에서 호스팅되며 시험판 빌드 시 해당 패키지를 다운로드 하려면 flighting 옵트인 해야 합니다. 설치가 올바른 빌드 및 링 조합에 대해 플 라이팅 옵트인 하지 않는 경우 페이로드가 다운로드 되지 않습니다. 다음을 두 번 확인 합니다.

1. **설정 > 업데이트 & 보안 > Windows Insider Program** 로 이동 하 여 **windows 참가자 계정** 섹션에 올바른 계정 정보가 있는지 확인 합니다. 해당 섹션이 표시 되지 않으면 **Windows 참가자 계정 연결**을 선택 하 고 전자 메일 계정을 추가 하 고 **windows 참가자 계정** 제목 아래에 표시 되는지 확인 합니다 ( **windows 참가자 계정 연결** 을 선택 하 여 실제로 새로 추가한 계정을 연결 하려면 두 번 선택 해야 할 수 있음).
 
2. **어떤 종류의 콘텐츠를 받고 싶으세요?** 에서 **Windows의 활성 개발** 이 선택 되어 있는지 확인 합니다.
 
3. **새 빌드를 가져올 속도**에서 **Windows Insider Fast** 가 선택 되어 있는지 확인 합니다.
 
4. 이제는 Ods를 설치할 수 있습니다. Windows 참가자가 신속 하 게 진행 중이 고 여전히 해당 Ds를 설치할 수 없는 경우 사용자 의견을 제공 하 고 **C:\Windows\Logs\CBS**아래에 로그 파일을 첨부 하세요.

### <a name="sc-startservice-openservice-failed-1060-the-specified-service-does-not-exist-as-an-installed-service"></a>번체 StartService: OpenService FAILED 1060: 지정 된 서비스가 설치 된 서비스로 존재 하지 않습니다.

개발자 패키지가 설치 되지 않은 경우이 오류가 발생할 수 있습니다. 개발자 패키지가 없으면 웹 관리 서비스가 없습니다. 개발자 패키지를 다시 설치 해 보세요.

### <a name="cbs-cannot-start-download-because-the-system-is-on-metered-network-cbs_e_metered_network"></a>시스템이 요금제 네트워크 (CBS_E_METERED_NETWORK)에 있으므로 CBS에서 다운로드를 시작할 수 없습니다.

요금제 인터넷 연결을 사용할 경우이 오류가 발생할 수 있습니다. 데이터 통신 연결에서 개발자 패키지를 다운로드할 수 없습니다.

## <a name="see-also"></a>참고 항목

* [Windows 장치 포털 개요](device-portal.md)
* [장치 포털 코어 API 참조](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-api-core)
