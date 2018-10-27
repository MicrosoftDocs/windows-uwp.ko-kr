---
author: jnHs
Description: Learn how your app's packages are made available to your customers, and how to manage specific package scenarios.
title: 앱 패키지 관리 지침
ms.assetid: 55405D0B-5C1E-43C8-91A1-4BFDD336E6AB
ms.author: wdg-dev-content
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: dd775b1fa653df5aca9b4738249757c052c181ed
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5700086"
---
# <a name="guidance-for-app-package-management"></a>앱 패키지 관리 지침

고객에게 앱 패키지를 제공하는 방법 및 특정 패키지 시나리오를 관리하는 방법에 대해 알아봅니다.

-   [OS 버전 및 패키지 배포](#os-versions-and-package-distribution)
-   [이전에 게시된 앱에 Windows 10용 패키지 추가](#adding-packages-for-windows-10-to-a-previously-published-app)
-   [Windows Phone 8.1용 패키지 호환성 유지 관리](#maintaining-package-compatibility-for-windows-phone-81)
-   [스토어에서 앱 제거](#removing-an-app-from-the-store)
-   [이전에 지원되던 디바이스 패밀리용 패키지 제거](#removing-packages-for-a-previously-supported-device-family)


## <a name="os-versions-and-package-distribution"></a>OS 버전 및 패키지 배포

여러 운영 체제에서 서로 다른 유형의 패키지를 실행할 수 있습니다. 고객의 장치에서 하나 이상의 패키지를 실행할 수 있는 경우 Microsoft Store는 최적의 패키지를 제공합니다.

일반적으로 이후 OS 버전은 동일한 디바이스 패밀리에 대해 이전 OS 버전을 대상으로 하는 패키지를 실행할 수 있습니다. 그러나 고객 앱의 현재 OS 버전을 대상으로 하는 패키지에 포함 되지 않은 경우에 이러한 패키지 얻게 됩니다.

예를 들어 Windows10 장치 이전 (디바이스 패밀리 별) 지원 되는 모든 OS 버전을 실행할 수 있습니다. 데스크톱 장치 Windows10 Windows8.1 또는 Windows8; 용으로 빌드된 앱을 실행할 수 있습니다. Windows10 모바일 장치는 Windows Phone 8.1, WindowsPhone8, 및에 Windows Phone 용으로 빌드된 앱을 실행할 수 7.x 합니다. 

다음 예는 서로 다른 OS 버전을 대상으로 하는 패키지가 포함된 앱의 다양한 시나리오를 보여줍니다(패키지의 아키텍처가 장치에 적합해야 하는 경우처럼 패키지의 특정 제약 조건으로 인해 여기에 나열된 OS 버전/장치 유형 중 일부에서 실행할 수 없는 경우는 제외). 

### <a name="example-app-1"></a>예제 앱 1

| 패키지의 대상 운영 체제 | 이 패키지를 가져올 수 있는 운영 체제 |
|-------------------------------------|----------------------------------------------|
| Windows8.1                         | Windows8.1 Windows10 데스크톱 장치      |
| Windows Phone 8.1                   | Windows10 모바일 장치, Windows Phone 8.1 |
| WindowsPhone8                     | WindowsPhone8                              |
| Windows Phone 7.1                   | Windows Phone 7.x                            |

예제 앱 1에 앱 아직 Windows10 장치용으로 특별히 작성 된 유니버설 Windows 플랫폼 (UWP) 패키지 하지만 Windows10 고객은 앱을 가져올 수 있습니다. 이러한 고객은 해당 장치 유형에 사용 가능한 최상의 패키지를 얻게 됩니다.

### <a name="example-app-2"></a>예제 앱 2

| 패키지의 대상 운영 체제  | 이 패키지를 가져올 수 있는 운영 체제 |
|--------------------------------------|----------------------------------------------|
| Windows10 (유니버설 디바이스 패밀리) | Windows10 (모든 디바이스 패밀리)             |
| Windows8.1                          | Windows8.1                                  |
| Windows Phone 8.1                    | Windows Phone 8.1                            |
| Windows Phone 7.1                    | Windows Phone 7.x, WindowsPhone8           |

예제 앱 2에는 다음과 같이 Windows8에서 실행할 수 있는 패키지가 없습니다. 그 외에 다른 모든 OS 버전을 실행하는 고객은 앱을 받을 수 있습니다. 모든 Windows 10 고객이 동일한 패키지를 받게 됩니다.

### <a name="example-app-3"></a>예제 앱 3

| 패키지의 대상 운영 체제 | 이 패키지를 가져올 수 있는 운영 체제                  |
|-------------------------------------|---------------------------------------------------------------|
| Windows10 (데스크톱 디바이스 패밀리)  | Windows10 데스크톱 장치                                    |
| WindowsPhone8                     | Windows10 모바일 장치, WindowsPhone8, Windows Phone 8.1 |

예제 앱 3에 모바일 디바이스 패밀리를 대상으로 하는 UWP 패키지가 없으므로 이므로 Windows10 모바일 장치의 고객 WindowsPhone8 패키지를 얻게 됩니다. 이 앱에서 나중에 모바일 장치 패밀리 (또는 범용 디바이스 패밀리)를 대상으로 하는 패키지를 추가한 경우 해당 패키지 WindowsPhone8 패키지 대신 Windows10 모바일 장치에서 고객에 게 제공 됩니다.

이 예제 앱에는 Windows Phone 7.x에서 실행할 수 있는 패키지가 포함되어 있지 않습니다.

### <a name="example-app-4"></a>예제 앱 4

| 패키지의 대상 운영 체제  | 이 패키지를 가져올 수 있는 운영 체제 |
|--------------------------------------|----------------------------------------------|
| Windows10 (유니버설 디바이스 패밀리) | Windows10 (모든 디바이스 패밀리)             |

예제 앱 4 Windows10를 실행 하는 모든 장치 앱을 가져올 수 있지만 이전 OS 버전의 고객에 게 제공 되지 않습니다. UWP 패키지는 유니버설 디바이스 패밀리를 대상으로 하므로 [장치 패밀리 가용성 선택](device-family-availability.md)) (당 Windows10 장치에 제공 됩니다.


## <a name="removing-an-app-from-the-store"></a>Store에서 앱 제거

고객에 대한 앱 제공을 중지, 즉 "게시를 취소"해야 할 때가 있습니다. 이렇게 하려면 **앱 개요** 페이지에서 **앱을 사용할 수 없도록 설정**을 클릭합니다. 앱을 사용할 수 없도록 설정하면 몇 시간 내에 해당 앱이 Store에 더 이상 표시되지 않고 새 고객이 가져올 수 없게 됩니다([프로모션 코드](generate-promotional-codes.md)를 보유한 Windows 10 장치 사용 고객은 예외).

> [!IMPORTANT]
> 이 옵션은 제출에서 선택한 [표시 여부](choose-visibility-options.md#discoverability) 설정보다 우선합니다. 

이 옵션은 제출을 만들었을 때와 똑같은 효과가 있으며, **취득 중지** 옵션에서 **Store에서 사용할 수 있지만 검색 되지 않는 제품으로 설정**을 선택합니다. 그러나 새 제출을 만들 필요는 없습니다.

앱을 이미 구매한 고객은 계속 사용하고 다시 다운로드할 수 있으며, 나중에 새 패키지를 제출한 경우 업데이트를 받을 수도 있습니다.

앱을 사용할 수 없도록 설정한 후에도 대시보드에 계속 표시됩니다. 고객에게 앱을 다시 제공하려면 앱 개요 페이지에서 **앱을 사용할 수 있도록 설정**을 클릭합니다. 마지막 제출 시 설정에서 제한하지 않았다면 확인 후 몇 시간 내에 새 고객이 앱을 사용할 수 있게 됩니다.

> [!NOTE]
> 앱을 계속 사용할 수 있도록 하지만 특정 OS 버전의 새 고객에게는 제공하지 않으려는 경우 새 제출을 만들고, 새로운 구입을 방지하려는 OS 버전의 모든 패키지를 제거할 수 있습니다. 예를 들어 Windows10, Windows Phone 8.1 용 패키지가 이미 있으며 WindowsPhone8.1에 새 고객에 게 앱을 계속 제공 하지 않으려는 경우 모든 WindowsPhone8.1 패키지 제출에서 제거 합니다. 업데이트를 게시 한 후에 WindowsPhone8.1 않고 새 고객이 됩니다 이미 고객이 계속 사용할 수 있지만 앱을 구입할 수 없습니다). 그러나 앱 Windows10 새로운 고객에 대해 사용할 수 있습니다.


## <a name="removing-packages-for-a-previously-supported-device-family"></a>이전에 지원되던 디바이스 패밀리용 패키지 제거

모든 패키지는 특정 [디바이스 패밀리](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview) 지원 하 던 앱 이전에, 메시지가 표시 됩니다 **패키지** 페이지에서 변경 내용을 저장 하기 전에 의도 한 내용 인지 확인 제거 하는 경우 합니다.

모든 앱이 이전에 지원 되는 디바이스 패밀리에서 실행 될 수 있는 패키지를 제거 하는 제출을 게시 하는 경우 새 고객에 게 해당 디바이스 패밀리에서 앱을 구입할 수 없습니다. 항상 나중에 또 다른 업데이트를 게시하여 해당 디바이스 패밀리용 패키지를 다시 제공할 수 있습니다.

특정 디바이스 패밀리를 지원하는 모든 패키지를 제거하더라도 이미 해당 장치 유형에 앱을 설치한 기존 고객은 계속 해당 앱을 사용할 수 있으며 나중에 제공되는 업데이트를 다운로드하게 되니 주의하세요.


<a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>

## <a name="adding-packages-for-windows10-to-a-previously-published-app"></a>이전에 게시 된 앱에 Windows10 용 패키지 추가

Windows에 대 한 패키지가 포함 된 저장소에는 앱이 있는 경우 8.x 및/또는 Windows Phone 8.x를 Windows10에 대 한 앱을 업데이트 하 고 새 제출을 만들고 [패키지](upload-app-packages.md) 단계 중에 UWP.msixupload 또는.appxupload 패키지를 추가 하려고 합니다. 앱 인증 프로세스를 거치면 UWP 패키지 Windows10에서 고객이 새 구입에 사용할 수 있는 됩니다.

> [!NOTE]
> 일단 Windows10 고객이 UWP 패키지를 이전 OS 버전용 패키지를 사용 하 여 돌아가기 해당 고객을 롤 수 없습니다. 

노트 Windows10 패키지의 버전 번호를 사용한 Windows8, Windows8.1, 및/또는 Windows Phone 8.1 패키지 보다 높아야 합니다. 자세한 내용은 [패키지 버전 번호](package-version-numbering.md)를 참조하세요.

Store의 UWP 앱 패키징에 대한 자세한 내용은 [앱 패키징](../packaging/index.md)을 참조하세요.
