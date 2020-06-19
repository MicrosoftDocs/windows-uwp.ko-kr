---
Description: 앱의 패키지를 고객에 게 제공 하는 방법 및 특정 패키지 시나리오를 관리 하는 방법을 알아봅니다.
title: 앱 패키지 관리에 대 한 지침
ms.assetid: 55405D0B-5C1E-43C8-91A1-4BFDD336E6AB
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4615503c41df6cef891ab8e77024d9951c489b38
ms.sourcegitcommit: 96b7be654a0922eeb421b5fa51ebfc586abe74fe
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84945950"
---
# <a name="guidance-for-app-package-management"></a>앱 패키지 관리에 대 한 지침

앱의 패키지를 고객에 게 제공 하는 방법 및 특정 패키지 시나리오를 관리 하는 방법을 알아봅니다.

-   [OS 버전 및 패키지 배포](#os-versions-and-package-distribution)
-   [이전에 게시 된 앱에 Windows 10 용 패키지 추가](#adding-packages-for-windows-10-to-a-previously-published-app)
-   [저장소에서 앱 제거](#removing-an-app-from-the-store)
-   [이전에 지원 되는 장치 제품군에 대 한 패키지 제거](#removing-packages-for-a-previously-supported-device-family)


## <a name="os-versions-and-package-distribution"></a>OS 버전 및 패키지 배포

운영 체제 마다 다른 유형의 패키지를 실행할 수 있습니다. 두 개 이상의 패키지를 고객의 장치에서 실행할 수 있는 경우 Microsoft Store은 가장 적합 한 일치 항목을 제공 합니다.

일반적으로 이후 OS 버전은 동일한 장치 제품군에 대 한 이전 OS 버전을 대상으로 하는 패키지를 실행할 수 있습니다. Windows 10 장치는 모든 이전 버전의 지원 되는 OS 버전 (장치 제품군 기준)을 실행할 수 있습니다. Windows 10 데스크톱 장치는 Windows 8.1 또는 Windows 8 용으로 빌드된 앱을 실행할 수 있습니다. Windows 10 mobile 장치는 Windows Phone 8.1, Windows Phone 8 뿐만 아니라 Windows Phone 7.x 용으로 빌드된 앱을 실행할 수 있습니다. 그러나 Windows 10의 고객은 해당 하는 장치 제품군을 대상으로 하는 UWP 패키지를 앱에 포함 하지 않는 경우에만 해당 패키지를 가져옵니다.

> [!IMPORTANT]
> Windows Phone .x SDK를 사용 하 여 빌드된 새 XAP 패키지는 더 이상 업로드할 수 없습니다. 이미 XAP 패키지를 사용 하 여 저장 된 앱은 Windows 10 Mobile 장치에서 계속 작동 합니다. 자세한 내용은이 [블로그 게시물](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store)을 참조 하세요.


## <a name="removing-an-app-from-the-store"></a>저장소에서 앱 제거

때때로 앱을 고객에 게 제공 하는 것을 중지 하 고 효과적으로 "게시 취소" 할 수 있습니다. 이렇게 하려면 **앱 개요** 페이지에서 **앱을 사용할 수 없음** 을 클릭 합니다. 앱을 사용할 수 없도록 설정 하는 것을 확인 한 후 몇 시간 내에 더 이상 스토어에 표시 되지 않으며, [프로 모션 코드가](generate-promotional-codes.md) 없고 Windows 10 장치를 사용 하 고 있는 경우를 제외 하 고는 새 고객이이를 받을 수 없습니다.

> [!IMPORTANT]
> 이 옵션은 제출에서 선택한 모든 [표시 유형](choose-visibility-options.md#discoverability) 설정을 재정의 합니다. 

이 옵션은 제출을 만든 후 **이 제품을 사용할 수 있도록** 선택 하 고 **취득 중지** 옵션을 사용 하 여 스토어에서 검색할 수 없는 것과 동일한 효과를 가집니다. 그러나 새 제출을 만들 필요는 없습니다.

앱이 이미 있는 고객은 계속 해당 앱을 사용할 수 있으며 나중에 다운로드할 수 있습니다 (새 패키지를 나중에 제출 하는 경우에도 업데이트를 받을 수 있음).

앱을 사용할 수 없게 되 면 파트너 센터에도 표시 됩니다. 응용 프로그램을 고객에 게 다시 제공 하기로 결정 한 경우 앱 개요 페이지에서 **사용 가능한 앱 만들기** 를 클릭 하면 됩니다. 확인 한 후에는 몇 시간 내에 마지막 제출의 설정에 의해 제한 되지 않는 한 새 고객이 앱을 사용할 수 있게 됩니다.

> [!NOTE]
> 앱을 계속 사용할 수 있지만 특정 OS 버전의 신규 고객에 게 제공 하지 않으려는 경우 새 제출을 만들고 새 합병을 방지 하려는 OS 버전에 대 한 모든 패키지를 제거할 수 있습니다. 예를 들어 이전에 Windows Phone 8.1 및 Windows 10 용 패키지가 있고 Windows Phone 8.1에서 새로운 고객에 게 앱을 제공 하지 않으려는 경우, 제출에서 모든 Windows Phone 8.1 패키지를 제거 합니다. 업데이트를 게시 한 후에는 Windows Phone 8.1에 대 한 새 고객이 해당 앱을 계속 사용할 수 있지만 앱을 가져올 수 없습니다. 그러나 Windows 10의 새 고객은 앱을 계속 사용할 수 있습니다.


## <a name="removing-packages-for-a-previously-supported-device-family"></a>이전에 지원 되는 장치 제품군에 대 한 패키지 제거

앱이 이전에 지 원하는 특정 [장치 제품군](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview) 에 대 한 모든 패키지를 제거 하는 경우 **패키지** 페이지에 변경 내용을 저장 하기 전에 이것이 의도 한 것인지 확인 하는 메시지가 표시 됩니다.

앱이 이전에 지 원하는 장치 제품군에서 실행할 수 있는 모든 패키지를 제거 하는 제출을 게시할 때 새 고객이 해당 장치 제품군에서 앱을 획득할 수 없게 됩니다. 나중에 언제 든 지 다른 업데이트를 게시 하 여 해당 장치 제품군에 대 한 패키지를 다시 제공할 수 있습니다.

특정 장치 제품군을 지 원하는 모든 패키지를 제거 하는 경우에도 해당 장치 유형에 앱을 이미 설치한 기존 고객은 해당 장치를 계속 사용할 수 있으며 나중에 제공 하는 모든 업데이트를 받게 됩니다.


<a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>

## <a name="adding-packages-for-windows10-to-a-previously-published-app"></a>이전에 게시 된 앱에 Windows 10 용 패키지 추가

스토어에 Windows 8.x 및/또는 Windows Phone 8.x 용 패키지만 포함 된 앱이 있고 Windows 10 용 응용 프로그램을 업데이트 하려면 [패키지](upload-app-packages.md) 단계 중에 새 제출을 만들고 UWP. msixupload 또는 .appxupload 패키지를 추가 합니다. 앱이 인증 프로세스를 통과 한 후에는 Windows 10에서 고객이 새를 사용할 때 UWP 패키지를 사용할 수도 있습니다.

> [!NOTE]
> Windows 10의 고객이 UWP 패키지를 가져온 후에는 이전 OS 버전에 대 한 패키지를 사용 하도록 해당 고객을 롤백할 수 없습니다. 

Windows 10 패키지의 버전 번호는 사용한 Windows 8, Windows 8.1 및/또는 Windows Phone 8.1 패키지의 버전 번호 보다 높아야 합니다. 자세한 내용은 [패키지 버전 번호 매기기](package-version-numbering.md)를 참조 하세요.

스토어 용 UWP 앱을 패키징하는 방법에 대 한 자세한 내용은 [앱 패키지](../packaging/index.md)를 참조 하세요.
