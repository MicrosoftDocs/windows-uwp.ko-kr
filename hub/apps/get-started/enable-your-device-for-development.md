---
ms.assetid: 54973C62-9669-4988-934E-9273FB0425FD
title: 디바이스를 개발에 사용하도록 설정
description: Visual Studio에서 개발자 모드를 활성화하여 개발 및 디버깅을 위해 Windows 10 디바이스를 사용하는 방법을 알아봅니다.
keywords: 개발자 라이선스 Visual Studio 시작, 개발자 라이선스 디바이스 활성화
ms.date: 10/13/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 642dd2ac6db5509e9bc4ea6ff8cb756312c3e90b
ms.sourcegitcommit: 56e9cab45d1c6e54841d61fdf23044fa01f50c43
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/13/2020
ms.locfileid: "92011903"
---
# <a name="enable-your-device-for-development"></a>디바이스를 개발에 사용하도록 설정

## <a name="activate-developer-mode-sideload-apps-and-access-other-developer-features"></a>앱 개발자 모드를 활성화하고, 앱을 테스트용으로 로드하고, 기타 개발자 기능에 액세스

![디바이스를 개발에 사용하도록 설정](images/developer-poster.png)

> [!IMPORTANT]
> PC에서 자신만의 고유한 애플리케이션을 만드는 것이 아닌 경우 개발자 모드를 사용할 필요가 없습니다. 컴퓨터의 문제를 해결하려는 경우[ Windows 도움말](https://support.microsoft.com/hub/4338813/windows-help?os=windows-10)을 확인하세요. 처음으로 개발하는 경우 필요한 도구를 다운로드하여 [준비](get-set-up.md)할 수 있습니다.

게임, 웹 검색, 이메일, Office 앱 등의 일반적인 일상 활동에 컴퓨터를 사용하는 경우 개발자 모드를 활성화할 필요가 *없으며* 사실 활성화해서는 안 됩니다. 이 페이지의 나머지 정보를 볼 필요가 없으며, 이전에 하던 작업으로 안전하게 돌아가시면 됩니다. 들러 주셔서 감사합니다!

그러나 컴퓨터에서 Visual Studio를 사용하여 처음으로 소프트웨어를 개발하는 경우 개발용 PC 그리고 코드 테스트에 사용할 모든 디바이스에서 개발자 모드를 사용하도록 *설정해야* 합니다. 개발자 모드가 활성화되지 않은 상태에서 UWP 프로젝트를 열면 **개발자용** 설정 페이지가 열리거나 Visual Studio에 이 대화 상자가 표시됩니다.

![Visual Studio에서 표시되는 개발자 모드 대화 상자를 사용하도록 설정](images/latestenabledialog.png)

이 대화 상자가 표시되면 **개발자용 설정**을 클릭하여 **개발자용** 설정 페이지를 엽니다.

> [!NOTE]
> 언제든지 **개발자용** 페이지로 이동하여 개발자 모드 사용 여부를 설정할 수 있습니다. 작업 표시줄의 Cortana 검색 상자에 "개발자용"을 입력하기만 하면 됩니다.

## <a name="accessing-settings-for-developers"></a>개발자용 설정에 액세스

개발자 모드를 활성화하거나 다른 설정에 액세스하려면 다음을 수행합니다.

1.  **개발자용** 설정 대화 상자에서 필요한 액세스 수준을 선택합니다.
2.  선택한 설정에 대한 고지 사항을 읽은 다음 **예**를 클릭하여 변경 내용을 적용합니다.

> [!NOTE]
> 개발자 모드를 사용하도록 설정하려면 관리자 권한이 필요합니다. 디바이스가 조직의 소유인 경우 이 옵션을 사용하지 못하게 설정해 놓았을 수도 있습니다.

### <a name="developer-mode-features"></a>개발자 모드 기능

개발자 모드는 개발자 라이선스에 대한 Windows 8.1 요구 사항을 대체합니다.  테스트용 로드 외에도 개발자 모드 설정을 사용하면 디버깅 및 추가 배포 옵션을 사용하도록 설정할 수 있습니다. 여기에는 이 디바이스에 배포할 수 있도록 허용하는 SSH 서비스 시작이 포함됩니다. 이 서비스를 중지하려면 개발자 모드를 사용하지 않도록 설정해야 합니다.

데스크톱에서 개발자 모드를 사용하는 경우 다음이 포함된 기능 패키지가 설치됩니다.
- Windows Device Portal. **디바이스 포털 사용** 옵션이 켜진 경우에만 Device Portal을 사용할 수 있고 방화벽 규칙이 구성됩니다.
- 앱의 원격 설치를 허용하는 SSH 서비스에 대한 방화벽 규칙을 설치 및 구성합니다. **디바이스 검색**을 사용하도록 설정하면 SSH 서버가 켜집니다.

이러한 기능에 대 한 자세한 내용을 보거나 설치 프로세스에 문제가 발생 하는 경우 [개발자 모드 기능 및 디버깅](developer-mode-features-and-debugging.md)을 확인 하세요.

## <a name="see-also"></a>참고 항목

* [Windows 계정 등록](sign-up.md)
* [개발자 모드 기능 및 디버깅](developer-mode-features-and-debugging.md).