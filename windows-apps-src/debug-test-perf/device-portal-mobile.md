---
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: 모바일용 디바이스 포털
description: Windows Device Portal에서 모바일 디바이스를 원격으로 구성 및 관리하는 방법에 대해 알아봅니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 디바이스 포털
ms.localizationpriority: medium
ms.openlocfilehash: 1f0a91dd0370b8eda8763b63034d7c3ffaa1acd5
ms.sourcegitcommit: a30808f38583f7c88fb5f54cd7b7e0b604db9ba6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/06/2020
ms.locfileid: "91763002"
---
# <a name="device-portal-for-mobile"></a>모바일용 디바이스 포털

Windows 10 버전 1511부터 추가적인 개발자 기능을 모바일 디바이스 패밀리에 사용할 수 있습니다. 이러한 기능은 개발자 모드를 디바이스에서 사용하는 경우에만 사용됩니다.

개발자 모드를 사용하는 방법에 대한 내용은 [개발을 위해 디바이스 사용](../get-started/enable-your-device-for-development.md)을 참조하세요.

![디바이스 검색 및 장치 포털 설정의 스크린샷.](images/device-portal/mob-dev-mode-options.png)

## <a name="set-up-device-portal-on-windows-phone"></a>Windows Phone에서 디바이스 포털 설정

### <a name="turn-on-device-discovery-and-pairing"></a>디바이스 검색 및 페어링 켜기

Device Portal에 연결하려면 휴대폰 설정에서 디바이스 검색 및 Device Portal을 사용하도록 설정해야 합니다. 이렇게 하면 휴대폰을 PC 또는 다른 Windows 10 디바이스와 페어링할 수 있습니다. 두 디바이스는 유선 또는 무선으로 연결하여 동일한 네트워크 서브넷에 연결되거나 USB로 연결되어야 합니다.

처음으로 디바이스 포털에 연결할 때 대/소문자가 구분된 6개의 문자로 된 보안 코드를 입력해야 합니다. 이렇게 하면 휴대폰에 액세스할 수 있으며 공격자로부터 보안을 유지할 수 있습니다. 휴대폰의 페어링 단추를 눌러 코드를 생성하고 표시한 다음 브라우저의 텍스트 상자에 6개의 문자를 입력합니다.

![개발자 모드 디바이스 검색 설정](images/device-portal/mob-dev-mode-pairing.png)

USB, 로컬 호스트 및 로컬 네트워크(VPN 및 테더링 포함)를 통하는 세 가지 방법 중 선택하여 디바이스 포털에 연결할 수 있습니다.

**Device Portal에 연결하려면**

1. 브라우저에 사용할 연결 형식으로 여기에 표시된 주소를 입력합니다.

    - USB: `http://127.0.0.1:10080`

    USB 연결을 통해 휴대폰을 PC에 연결할 때 이 주소를 사용합니다. 두 디바이스에 Windows 10 버전 1511 이상이 설치되어 있어야 합니다.
    
    - Localhost: `http://127.0.0.1`

    Windows 10 Mobile용 Microsoft Edge 휴대폰에서 로컬로 디바이스 포털을 보려면 이 주소를 사용합니다.
    
    - 로컬 네트워크: `https://<The IP address or hostname of the phone>`

    로컬 네트워크를 통해 연결하려면 이 주소를 사용합니다.

    휴대폰의 IP 주소는 휴대폰의 디바이스 포털 설정에 표시됩니다. 인증 및 보안 통신을 위해 HTTPS가 필요합니다. 호스트 이름(설정 > 시스템 > 정보에서 편집 가능)은 로컬 네트워크(예: http://Phone360) )에서 Device Portal 액세스에 사용될 수도 있습니다. 이는 네트워크 또는 IP 주소를 자주 변경 또는 공유해야 할 수 있는 디바이스에 대해 유용합니다. 

2. 휴대폰에서 페어링 단추를 눌러 필요한 보안 코드를 생성하고 표시합니다.

3. 브라우저에서 디바이스 포털 암호 상자에 6개의 문자 보안 코드를 입력합니다.

4. (옵션) 나중에 이 페어링을 기억하려면 브라우저에서 Remember my computer(내 컴퓨터 기억) 상자를 선택합니다.

다음은 Windows Phone 개발자 설정 페이지의 디바이스 포털 섹션입니다.

![Windows Phone의 장치 포털 설정 페이지 스크린샷.](images/device-portal/mob-dev-mode-portal.png)

로컬 네트워크에 있는 모든 사용자를 신뢰하고, 디바이스에 개인 정보가 없고, 고유한 요구 사항이 있는 테스트 랩 같은 보호 환경에서 Device Portal을 사용하는 경우 인증을 사용하지 않도록 설정할 수 있습니다. 이렇게 하면 암호화되지 않은 통신이 가능하며 휴대폰의 IP 주소를 가진 모든 사용자가 제어할 수 있습니다.

## <a name="tool-notes"></a>도구 참고 사항

## <a name="device-portal-pages"></a>Device Portal 페이지
### <a name="processes"></a>프로세스

Windows Mobile Device Portal에는 임의 프로세스를 종료하는 기능이 포함되지 않습니다. 

모바일 디바이스에서 디바이스 포털은 표준 페이지 집합을 제공합니다. 자세한 설명은 [Windows Device Portal 개요](device-portal.md)를 참조하세요.

- 앱 관리자
- 앱 파일 탐색기(격리된 Storage Explorer)
- 프로세스
- 성능 차트
- ETW(Windows용 이벤트 추적)
- 성능 추적(WPR) 
- 장치
- 네트워킹

## <a name="see-also"></a>참고 항목

* [Windows 장치 포털 개요](device-portal.md)
* [장치 포털 핵심 API 참조](./device-portal-api-core.md)