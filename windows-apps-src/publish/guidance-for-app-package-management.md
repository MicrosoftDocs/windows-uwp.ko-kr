---
Description: 고객에게 앱 패키지를 제공하는 방법 및 특정 패키지 시나리오를 관리하는 방법을 알아봅니다.
title: 앱 패키지 관리 지침
ms.assetid: 55405D0B-5C1E-43C8-91A1-4BFDD336E6AB
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0548ae9f9b3b33808cd7420eb542bcbac6a1a431
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607368"
---
# <a name="guidance-for-app-package-management"></a>앱 패키지 관리 지침

고객에게 앱 패키지를 제공하는 방법 및 특정 패키지 시나리오를 관리하는 방법을 알아봅니다.

-   [OS 버전 및 패키지 배포](#os-versions-and-package-distribution)
-   [Windows 10 용 패키지를 이전에 게시 된 앱 추가](#adding-packages-for-windows-10-to-a-previously-published-app)
-   [Windows Phone 8.1에 대 한 패키지 호환성을 유지 관리](#maintaining-package-compatibility-for-windows-phone-81)
-   [스토어에서 앱을 제거합니다.](#removing-an-app-from-the-store)
-   [이전에 지원 되는 장치 제품군에 대 한 패키지를 제거합니다.](#removing-packages-for-a-previously-supported-device-family)


## <a name="os-versions-and-package-distribution"></a>OS 버전 및 패키지 배포

여러 운영 체제에서 서로 다른 유형의 패키지를 실행할 수 있습니다. 고객의 장치에서 하나 이상의 패키지를 실행할 수 있는 경우 Microsoft Store는 최적의 패키지를 제공합니다.

일반적으로 이후 OS 버전은 동일한 디바이스 패밀리에 대해 이전 OS 버전을 대상으로 하는 패키지를 실행할 수 있습니다. Windows 10 장치는 이전 (장치 제품군) 당 지원 되는 모든 OS 버전을 실행할 수 있습니다. Windows 10 데스크톱 장치는 Windows 8.1 또는 Windows 8;에 대해 빌드된 앱을 실행할 수 있습니다. Windows 10 mobile 장치는 Windows Phone 8.1, Windows Phone 8 및 Windows Phone 빌드된 앱을 실행할 수 7.x입니다. 그러나 고객은 Windows 10에서 앱 적용 가능한 장치 제품군을 대상으로 하는 UWP 패키지에 포함 되지 않은 경우에 해당 패키지 받게 됩니다.

> [!IMPORTANT]
> 2018 년 10 월 31 일부 터 새로 만든 제품 Windows 8.x/Windows를 대상으로 하는 패키지를 포함할 수 없습니다 Phone 8.x 또는 이전 버전입니다. 자세한 내용은 참조 [블로그 게시물](https://blogs.windows.com/buildingapps/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store/)합니다.


## <a name="removing-an-app-from-the-store"></a>스토어에서 앱 제거

고객에 대한 앱 제공을 중지, 즉 "게시를 취소"해야 할 때가 있습니다. 이렇게 하려면 **앱 개요** 페이지에서 **앱을 사용할 수 없도록 설정**을 클릭합니다. 앱을 사용할 수 없도록 설정하면 몇 시간 내에 해당 앱이 Store에 더 이상 표시되지 않고 새 고객이 가져올 수 없게 됩니다([프로모션 코드](generate-promotional-codes.md)를 보유한 Windows 10 장치 사용 고객은 예외).

> [!IMPORTANT]
> 이 옵션은 제출에서 선택한 [표시 여부](choose-visibility-options.md#discoverability) 설정보다 우선합니다. 

이 옵션은 제출을 만들었을 때와 똑같은 효과가 있으며, **취득 중지** 옵션에서 **Store에서 사용할 수 있지만 검색 되지 않는 제품으로 설정**을 선택합니다. 그러나 새 제출을 만들 필요는 없습니다.

앱을 이미 구매한 고객은 계속 사용하고 다시 다운로드할 수 있으며, 나중에 새 패키지를 제출한 경우 업데이트를 받을 수도 있습니다.

앱에 사용할 수 없는 확인 한 후 계속 표시 됩니다 것 파트너 센터에서. 고객에게 앱을 다시 제공하려면 앱 개요 페이지에서 **앱을 사용할 수 있도록 설정**을 클릭합니다. 마지막 제출 시 설정에서 제한하지 않았다면 확인 후 몇 시간 내에 새 고객이 앱을 사용할 수 있게 됩니다.

> [!NOTE]
> 앱을 계속 사용할 수 있도록 하지만 특정 OS 버전의 새 고객에게는 제공하지 않으려는 경우 새 제출을 만들고, 새로운 구입을 방지하려는 OS 버전의 모든 패키지를 제거할 수 있습니다. 예를 들어, Windows Phone 8.1 및 Windows 10에 대 한 패키지를 이전에 Windows Phone 8.1에서 신규 고객에 게 앱 제공을 유지 하지 않으려는 경우 모든 Windows Phone 8.1 패키지에서에서 제거를 제출 합니다. 업데이트가 게시 된 후 Windows Phone 8.1에서 신규 고객 됩니다 아직 고객이 계속 해 서 사용할 수 있지만 앱을 획득할 수). 그러나 앱은 새 고객에 게 Windows 10에서 사용할 수 있습니다.


## <a name="removing-packages-for-a-previously-supported-device-family"></a>이전에 지원되던 디바이스 패밀리용 패키지 제거

특정에 대 한 모든 패키지를 제거 하는 경우 [장치 제품군](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview) 는 앱 이전에 지원 되는, 메시지가 표시 됩니다에 변경 내용을 저장 하기 전에 의도 임을 확인 하는 **패키지** 페이지입니다.

모든 앱에서 이전에 지원 되는 장치 제품군에서 실행 될 수 있는 패키지를 제거 하는 제출물을 게시 하면 신규 고객에 게 해당 장치 제품군에서 앱을 가져올 수 없습니다. 항상 나중에 또 다른 업데이트를 게시하여 해당 디바이스 패밀리용 패키지를 다시 제공할 수 있습니다.

특정 디바이스 패밀리를 지원하는 모든 패키지를 제거하더라도 이미 해당 장치 유형에 앱을 설치한 기존 고객은 계속 해당 앱을 사용할 수 있으며 나중에 제공되는 업데이트를 다운로드하게 되니 주의하세요.


<a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>

## <a name="adding-packages-for-windows10-to-a-previously-published-app"></a>Windows 10 용 패키지를 이전에 게시 된 앱 추가

만 Windows에 대 한 패키지를 포함 하는 저장소의 앱이 있는 경우 8.x 및/또는 Windows Phone 8.x, 및 있습니다 하려면 Windows 10 용 앱을 업데이트, 새 전송 중에 UWP.msixupload 또는.appxupload 패키지를 추가 합니다 [패키지](upload-app-packages.md) 단계. 앱 인증 프로세스를 진행 합니다 UWP 패키지는 Windows 10에서 고객이 새 구매에 사용할 수 있는 수 있습니다.

> [!NOTE]
> Windows 10에서 고객 UWP 패키지를 가져온 후 해당 고객 이전 OS 버전에 대 한 패키지를 사용 하도록 다시 롤포워드할 수 없습니다. 

Windows 10 패키지의 버전 번호를 사용 하는 모든 Windows 8, Windows 8.1, 및/또는 Windows Phone 8.1 패키지에 대 한 보다 높은 해야 note 합니다. 자세한 내용은 [패키지 버전 번호](package-version-numbering.md)를 참조하세요.

Store의 UWP 앱 패키징에 대한 자세한 내용은 [앱 패키징](../packaging/index.md)을 참조하세요.
