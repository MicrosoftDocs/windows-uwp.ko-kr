---
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: Windows 데스크톱의 장치 포털
description: Windows Device Portal이 Windows 데스크톱의 진단 및 자동화를 제공하는 방법을 알아봅니다.
ms.date: 02/06/2019
ms.topic: article
keywords: windows 10, uwp, 장치 포털
ms.localizationpriority: medium
ms.openlocfilehash: 73f7e827c0ec8ca289d3523da06601de978a91d2
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210189"
---
# <a name="device-portal-for-windows-desktop"></a>Windows 데스크톱의 장치 포털

Windows 장치 포털을 사용하여 진단 정보를 살펴보고 브라우저 창에서 HTTP를 통해 데스크톱을 조작할 수 있습니다. Device Portal을 사용하여 다음 작업을 수행할 수 있습니다.
- 실행 중인 프로세스 목록 확인 및 조작
- 앱 설치, 삭제, 시작 및 종료
- Wi-Fi 프로필 변경, 신호 강도 보기 및 ipconfig 확인
- CPU의 라이브 그래프, 메모리, I/O 네트워크 및 GPU 사용 보기
- 프로세스 덤프 수집
- ETW 추적 수집 
- 테스트용으로 로드된 앱의 격리된 스토리지 조작

## <a name="set-up-device-portal-on-windows-desktop"></a>Window 데스크톱에서 장치 포털 설정

### <a name="turn-on-developer-mode"></a>개발자 모드 켜기

Windows 10 1607 버전부터 데스크톱의 새로운 기능 중 일부는 개발자 모드를 켜야만 사용할 수 있습니다. 개발자 모드를 켜는 방법에 대한 내용은 [디바이스를 개발에 사용하도록 설정](../get-started/enable-your-device-for-development.md)을 참조하세요.

> [!IMPORTANT]
> 경우에 따라 네트워크 또는 호환성 문제로 인해 디바이스에 개발자 모드가 제대로 설치되지 않을 수 있습니다. 이러한 문제 해결에 대한 도움말은 [디바이스를 개발에 사용하도록 설정의 관련 섹션](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#failure-to-install-developer-mode-package)을 참조하세요.

### <a name="turn-on-device-portal"></a>장치 포털 켜기

**설정**의 **개발자** 섹션에서 장치 포털을 사용하도록 설정할 수 있습니다. 장치 포털을 사용하도록 설정하는 경우 해당하는 사용자 이름과 암호도 만들어야 합니다. Microsoft 계정 또는 기타 Windows 자격 증명은 사용하지 마세요. 

![[설정] 앱의 장치 포털 섹션](images/device-portal/device-portal-desk-settings.png) 

장치 포털이 사용하도록 설정되면 섹션 아래쪽에 웹 링크가 표시됩니다. 나열된 URL의 끝에 첨부되어 있는 포트 번호를 적어 둡니다. 이 포트 번호는 장치 포털을 사용하도록 설정할 때 무작위로 생성되지만, 데스크톱을 다시 부팅해도 동일하게 유지됩니다. 

이러한 링크를 사용하여 두 가지 방법으로(로컬 네트워크(VPN 포함)를 통해 또는 로컬 호스트를 통해) 장치 포털에 연결할 수 있습니다.

### <a name="connect-to-device-portal"></a>장치 포털에 연결

로컬 호스트를 통해 연결하려면 브라우저 창을 열고, 여기에 표시된 주소를 사용하려는 연결 유형에 입력합니다.

* Localhost: `http://127.0.0.1:<PORT>` 또는 `http://localhost:<PORT>`
* 로컬 네트워크: `https://<IP address of the desktop>:<PORT>`

인증 및 보안 통신을 위해 HTTPS가 필요합니다.

로컬 네트워크에 있는 모든 사용자를 신뢰하고, 디바이스에 개인 정보가 없고, 고유한 요구 사항이 있는 테스트 랩 같은 보호 환경에서 장치 포털을 사용하는 경우 인증 옵션을 사용하지 않도록 설정해도 됩니다. 이렇게 하면 암호화되지 않은 통신이 가능하며, 개발자 컴퓨터의 IP 주소를 갖고 있는 모든 사용자가 개발자 컴퓨터에 연결하여 제어할 수 있습니다.

## <a name="device-portal-content-on-windows-desktop"></a>Windows 데스크톱의 장치 포털 콘텐츠

Windows 데스크톱의 장치 포털은 표준 페이지 세트를 제공합니다. 자세한 설명은 [Windows 장치 포털 개요](device-portal.md)를 참조하세요.

- 앱 관리자
- 파일 탐색기
- 실행 중인 프로세스
- 성능
- Debug
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

Device Portal용 포트 번호(예: 80 및 443)를 선택하려는 경우 다음 레지스트리 키를 설정할 수 있습니다.

- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WebManagement\Service` 아래
    - `UseDynamicPorts`: 필수 DWORD. 선택한 포트 번호를 유지하려면 이 항목을 0으로 설정합니다.
    - `HttpPort`: 필수 DWORD. Device Portal이 HTTP 연결을 수신 대기하는 포트 번호를 포함합니다.    
    - `HttpsPort`: 필수 DWORD. Device Portal이 HTTPS 연결을 수신 대기하는 포트 번호를 포함합니다.
    
동일한 regkey 경로에서 다음과 같이 인증 요구 사항을 해제할 수 있습니다.
- `UseDefaultAuthorizer` -  사용하지 않으려면 `0`, 사용하려면 `1`로 설정합니다.  
    - 이는 각 연결과 HTTP에서 HTTPS로의 리디렉션에 대한 기본적인 인증 요구 사항을 모두 제어합니다.  
    
### <a name="command-line-options-for-device-portal"></a>장치 포털의 명령줄 옵션
관리 명령 프롬프트에서 장치 포털의 여러 부분을 사용하도록 설정하고 구성할 수 있습니다. 빌드에서 지원되는 최신 명령 세트를 보려면 `webmanagement /?`를 실행합니다.

- `sc start webmanagement` 또는 `sc stop webmanagement` 
    - 서비스를 켜거나 끕니다. 이 경우에도 개발자 모드를 사용하도록 설정해야 합니다. 
- `-Credentials <username> <password>` 
    - 장치 포털의 사용자 이름 및 암호를 설정합니다. 사용자 이름은 기본 인증 표준을 준수해야 하므로 콜론(:)을 포함할 수 없고, 브라우저가 표준 방식으로는 전체 문자 세트를 구문 분석할 수 없기 때문에 표준 ASCII 문자(예: [a-zA-Z0-9])에서 구성해야 합니다.  
- `-DeleteSSL` 
    - 이는 HTTPS 연결에 사용되는 SSL 인증서 캐시를 다시 설정합니다. (예상한 인증서 경고가 아닌) 무시할 수 없는 TLS 연결 오류가 발생하는 경우 이 옵션으로 문제를 해결할 수도 있습니다. 
- `-SetCert <pfxPath> <pfxPassword>`
    - 자세한 내용은 [사용자 지정 SSL 인증서로 장치 포털 프로비저닝](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-ssl)을 참조하세요.  
    - 이렇게 하면 개발자 고유의 SSL 인증서를 설치하여 장치 포털에서 자주 발생하는 SSL 경고 페이지를 해결할 수 있습니다. 
- `-Debug <various options for authentication, port selection, and tracing level>`
    - 특정 구성과 표시되는 디버그 메시지를 사용하는 독립 실행형 장치 포털 버전을 실행합니다. [패키지된 플러그인](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-plugin)을 빌드하는 데 가장 유용합니다. 
    - 패키지된 플러그인을 테스트하기 위해 이를 실행하는 방법에 대한 자세한 내용은 [MSDN 잡지 문서](https://msdn.microsoft.com/magazine/mt826332.aspx)를 참조하세요.

## <a name="common-errors-and-issues"></a>일반적인 오류 및 이슈

다음은 장치 포털을 설정할 때 발생할 수 있는 일반적인 오류입니다.

### <a name="windowsupdatesearch-returns-invalid-number-of-updates-0x800f0950-cbs_e_invalid_windows_update_count"></a>WindowsUpdateSearch에서 잘못된 업데이트 수를 반환(0x800f0950 CBS_E_INVALID_WINDOWS_UPDATE_COUNT)

Windows 10 시험판 빌드에 개발자 패키지를 설치하려고 할 때 이 오류가 발생할 수 있습니다. 이러한 FoD(주문형 기능) 패키지는 Windows 업데이트에 호스팅되며, 시험판 빌드에서 이 패키지를 다운로드하려면 플라이팅을 옵트인해야 합니다. 설치를 올바른 빌드 및 링 조합에 대한 플라이팅에 옵트인하지 않으면 페이로드가 다운로드되지 않습니다. 다음 사항을 철저하게 확인하세요.

1. **설정 > 업데이트 및 보안 > Windows 참가자 프로그램**으로 이동하여 **Windows 참가자 계정** 섹션의 계정 정보가 올바른지 확인합니다. 이 섹션이 보이지 않으면 **Windows 참가자 계정 연결**을 선택하고, 이메일 계정을 추가하고, 해당 계정이 **Windows 참가자 계정** 제목 아래에 표시되는지 확인합니다(새로 추가된 계정을 실제로 연결하려면 **Windows 참가자 계정 연결**을 한 번 더 선택해야 할 수도 있음).
 
2. **어떤 유형의 콘텐츠를 받으시겠습니까?** 아래에서 **Active development of Windows(진행 중인 Windows 개발)** 를 선택합니다.
 
3. **어떤 속도로 새 빌드를 받으시겠습니까?** 아래에서 **Windows 초기 참가자**를 선택합니다.
 
4. 이제 FoD를 설치할 수 있습니다. Windows 초기 참가자에 이름이 있는 것을 확인했지만 여전히 FoD를 설치할 수 없는 경우에는 피드백을 제출하고 **C:\Windows\Logs\CBS**의 로그 파일을 첨부해 주세요.

### <a name="sc-startservice-openservice-failed-1060-the-specified-service-does-not-exist-as-an-installed-service"></a>[SC] StartService: OpenService 실패 1060: 설치된 서비스 중에는 지정된 서비스가 없습니다.

개발자 패키지가 설치되지 않은 경우 이 오류가 발생할 수 있습니다. 개발자 패키지가 없으면 웹 관리 서비스가 없습니다. 개발자 패키지를 다시 설치해 보세요.

### <a name="cbs-cannot-start-download-because-the-system-is-on-metered-network-cbs_e_metered_network"></a>시스템이 요금제 네트워크에 있으므로 CBS에서 다운로드를 시작할 수 없음(CBS_E_METERED_NETWORK)

요금제 인터넷 연결을 사용하는 경우 이 오류가 발생할 수 있습니다. 요금제 연결에서는 개발자 패키지를 다운로드할 수 없습니다.

## <a name="see-also"></a>참고 항목

* [Windows 장치 포털 개요](device-portal.md)
* [장치 포털 핵심 API 참조](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-api-core)
