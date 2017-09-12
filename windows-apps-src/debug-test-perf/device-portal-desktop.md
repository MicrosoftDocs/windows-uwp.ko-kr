---
author: PatrickFarley
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: "데스크톱용 Device Portal"
description: "Windows Device Portal이 Windows 데스크톱의 진단 및 자동화를 제공하는 방법을 알아봅니다."
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 32155bfbb676a5f79dd4b1629f0a88368da36828
ms.sourcegitcommit: 0fa9ae00117e8e6b04ed38956e605bb74c1261c6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/07/2017
---
# <a name="device-portal-for-desktop"></a>데스크톱용 디바이스 포털

Windows 10, 버전 1607부터 추가적인 데스크톱용 개발자 기능을 사용할 수 있습니다. 이러한 기능은 개발자 모드를 사용하는 경우에만 사용됩니다.

개발자 모드를 사용하는 방법에 대한 내용은 [개발을 위해 디바이스 사용](../get-started/enable-your-device-for-development.md)을 참조하세요.

Device Portal을 사용하면 진단 정보를 보거나 브라우저에서 HTTP를 통해 데스크톱을 조작할 수 있습니다. Device Portal을 사용하여 다음 작업을 수행할 수 있습니다.
- 실행 중인 프로세스 목록 확인 및 조작
- 앱 설치, 삭제, 시작 및 종료
- Wi-Fi 프로필 변경, 신호 강도 보기 및 ipconfig 확인
- CPU의 라이브 그래프, 메모리, I/O 네트워크 및 GPU 사용 보기
- 프로세스 덤프 수집
- ETW 추적 수집 
- 테스트용으로 로드된 앱의 격리된 저장소 조작

## <a name="set-up-device-portal-on-windows-desktop"></a>Window 데스크톱에서 Device Portal 설정

### <a name="turn-on-device-portal"></a>Device Portal 켜기

**개발자 설정** 메뉴에서 개발자 모드가 사용되도록 설정된 상태에서 Device Portal을 사용하도록 설정할 수 있습니다.  

Device Portal을 사용하도록 설정하는 경우 Device Portal에 대한 사용자 이름 및 암호도 만들어야 합니다. Microsoft 계정 또는 기타 Windows 자격 증명은 사용하지 마세요.  

Device Portal이 사용되도록 설정되면 **설정** 섹션 아래쪽에 Device Portal에 대한 링크가 표시됩니다. URL 끝에 적용되어 있는 포트 번호를 적어두세요. 이 포트 번호는 Device Portal이 사용되도록 설정될 때 무작위로 생성되지만 데스크톱을 다시 부팅해도 동일하게 유지됩니다. 영구적으로 유지되도록 포트 번호를 수동으로 설정하려면 [포트 번호 설정](device-portal-desktop.md#setting-port-numbers)을 참조하세요.

로컬 호스트 및 로컬 네트워크(VPN 포함)를 통하는 2가지 방법 중 선택하여 Device Portal에 연결할 수 있습니다.

**Device Portal에 연결하려면**

1. 브라우저에 사용할 연결 형식으로 여기에 표시된 주소를 입력합니다.

    - Localhost: `http://127.0.0.1:PORT` 또는 `http://localhost:PORT`

    Device Portal을 로컬로 보려면 이 주소를 사용합니다.
    
    - 로컬 네트워크: `https://<The IP address of the desktop>:PORT`

    로컬 네트워크를 통해 연결하려면 이 주소를 사용합니다.

인증 및 보안 통신을 위해 HTTPS가 필요합니다.

로컬 네트워크에 있는 모든 사용자를 신뢰하고, 디바이스에 개인 정보가 없고, 고유한 요구 사항이 있는 테스트 랩 같은 보호 환경에서 Device Portal을 사용하는 경우 인증을 사용하지 않도록 설정할 수 있습니다. 이렇게 하면 암호화되지 않은 통신이 가능하며 컴퓨터의 IP 주소를 가진 모든 사용자가 제어할 수 있습니다.

## <a name="device-portal-pages"></a>Device Portal 페이지

데스크톱의 Device Portal은 표준 페이지 집합을 제공합니다. 자세한 설명은 [Windows Device Portal 개요](device-portal.md)를 참조하세요.

- 앱
- 프로세스
- 성능
- 디버깅
- ETW(Windows용 이벤트 추적)
- 성능 추적
- 디바이스
- 네트워킹
- 앱 파일 탐색기 

## <a name="setting-port-numbers"></a>포트 번호 설정

Device Portal용 포트 번호(예: 80 및 443)를 선택하려는 경우 다음 레지스트리 키를 설정할 수 있습니다.

- HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WebManagement\Service
    - UseDynamicPorts: 필수 DWORD. 선택한 포트 번호를 유지하려면 이 항목을 0으로 설정합니다.
    - HttpPort: 필수 DWORD. Device Portal이 HTTP 연결을 수신 대기하는 포트 번호를 포함합니다.  
    - HttpsPort: 필수 DWORD. Device Portal이 HTTPS 연결을 수신 대기하는 포트 번호를 포함합니다.

## <a name="failure-to-install-developer-mode-package"></a>개발자 모드 패키지 설치 실패
경우에 따라 네트워크 또는 호환성 문제로 인해 개발자 모드가 제대로 설치되지 않습니다. PC에 **원격**으로 배포하려면, 다시 말해서 브라우저 또는 디바이스 검색에서 디바이스 포털을 사용하여 SSH를 설정하려면 개발자 모드 패키지가 필요하지만 로컬 배포에는 필요하지 않습니다.  이러한 문제가 발생하더라도 Visual Studio를 사용하여 앱을 로컬로 계속 배포할 수 있습니다. 

[알려진 문제](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues&sort=relevancedesc&brandIgnore=True&searchTerm=%22device+portal%22) 포럼과 [개발자 모드 페이지](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#failure-to-install-developer-mode-package)를 참조하여 이러한 문제 등에 대한 해결책을 찾아보십시오. 

