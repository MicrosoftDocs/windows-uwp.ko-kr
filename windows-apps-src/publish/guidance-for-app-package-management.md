---
author: jnHs
Description: Learn how your app's packages are made available to your customers, and how to manage specific package scenarios.
title: 앱 패키지 관리 지침
ms.assetid: 55405D0B-5C1E-43C8-91A1-4BFDD336E6AB
ms.author: wdg-dev-content
ms.date: 03/28/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9b0b6315b1177138c3ede7834e2dbc792ee106dd
ms.sourcegitcommit: 3727445c1d6374401b867c78e4ff8b07d92b7adc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/29/2018
ms.locfileid: "2910207"
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

일반적으로 이후 OS 버전은 동일한 디바이스 패밀리에 대해 이전 OS 버전을 대상으로 하는 패키지를 실행할 수 있습니다. 그러나 고객 앱 현재 OS 버전을 대상으로 하는 패키지에 포함 되지 않은 경우에 이러한 패키지 얻게 됩니다.

예를 들어 Windows 10 장치는 이전에 지원된 모든 OS 버전(디바이스 패밀리별)을 실행할 수 있습니다. Windows 10 데스크톱 장치는 Windows 8.1 또는 Windows 8용으로 빌드된 앱을 실행할 수 있습니다. Windows 10 Mobile 장치는 Windows Phone 8.1, Windows Phone 8 및 Windows Phone 7.x용으로 빌드된 앱을 실행할 수 있습니다. 

다음 예는 서로 다른 OS 버전을 대상으로 하는 패키지가 포함된 앱의 다양한 시나리오를 보여줍니다(패키지의 아키텍처가 장치에 적합해야 하는 경우처럼 패키지의 특정 제약 조건으로 인해 여기에 나열된 OS 버전/장치 유형 중 일부에서 실행할 수 없는 경우는 제외). 

### <a name="example-app-1"></a>예제 앱 1

| 패키지의 대상 운영 체제 | 이 패키지를 얻을 수 있는 운영 체제 |
|-------------------------------------|----------------------------------------------|
| Windows 8.1                         | Windows 10 데스크톱 장치, Windows 8.1      |
| Windows Phone 8.1                   | Windows 10 Mobile 장치, Windows Phone 8.1 |
| Windows Phone 8                     | Windows Phone 8                              |
| Windows Phone 7.1                   | Windows Phone 7.x                            |

예제 앱 1에는 특별히 Windows 10 장치용으로 빌드된 UWP(유니버설 Windows 플랫폼) 패키지가 아직 없지만 Windows 10 고객은 앱을 가져올 수 있습니다. 이러한 고객은 해당 장치 유형에 사용 가능한 최상의 패키지를 얻게 됩니다.

### <a name="example-app-2"></a>예제 앱 2

| 패키지의 대상 운영 체제  | 이 패키지를 가져올 수 있는 운영 체제 |
|--------------------------------------|----------------------------------------------|
| Windows 10(유니버설 디바이스 패밀리) | Windows 10(모든 디바이스 패밀리)             |
| Windows 8.1                          | Windows 8.1                                  |
| Windows Phone 8.1                    | Windows Phone 8.1                            |
| Windows Phone 7.1                    | Windows Phone 7.x, Windows Phone 8           |

예제 앱 2에는 Windows 8에서 실행할 수 있는 패키지가 없습니다. 그 외에 다른 모든 OS 버전을 실행하는 고객은 앱을 받을 수 있습니다. 모든 Windows 10 고객이 동일한 패키지를 받게 됩니다.

### <a name="example-app-3"></a>예제 앱 3

| 패키지의 대상 운영 체제 | 이 패키지를 가져올 수 있는 운영 체제                  |
|-------------------------------------|---------------------------------------------------------------|
| Windows 10(데스크톱 디바이스 패밀리)  | Windows 10 데스크톱 장치                                    |
| Windows Phone 8                     | Windows 10 Mobile 장치, Windows Phone 8, Windows Phone 8.1 |

예제 앱 3에는 모바일 디바이스 패밀리를 대상으로 하는 UWP 패키지가 없으므로 Windows 10 Mobile 장치 고객은 Windows Phone 8 패키지를 가져오게 됩니다. 이 앱에서 나중에 모바일 장치 패밀리(또는 범용 장치 패밀리)를 대상으로 하는 패키지를 추가한 경우 Windows 10 Mobile 장치 고객은 Windows Phone 8 패키지 대신 이 패키지를 사용할 수 있습니다.

이 예제 앱에는 Windows Phone 7.x에서 실행할 수 있는 패키지가 포함되어 있지 않습니다.

### <a name="example-app-4"></a>예제 앱 4

| 패키지의 대상 운영 체제  | 이 패키지를 가져올 수 있는 운영 체제 |
|--------------------------------------|----------------------------------------------|
| Windows 10(유니버설 디바이스 패밀리) | Windows 10(모든 디바이스 패밀리)             |

예제 앱 4에서는 Windows 10을 실행하는 장치에서 앱을 가져올 수 있지만 이전 OS 버전의 고객은 사용할 수 없습니다. UWP 패키지는 유니버설 디바이스 패밀리를 대상으로 하므로 모든 Windows 10 장치 (따라 [디바이스 패밀리 가용성 선택](device-family-availability.md))를 사용할 수 있는 됩니다.


## <a name="removing-an-app-from-the-store"></a>Store에서 앱 제거

고객에 대한 앱 제공을 중지, 즉 "게시를 취소"해야 할 때가 있습니다. 이렇게 하려면 **앱 개요** 페이지에서 **앱을 사용할 수 없도록 설정**을 클릭합니다. 앱을 사용할 수 없도록 설정하면 몇 시간 내에 해당 앱이 Store에 더 이상 표시되지 않고 새 고객이 가져올 수 없게 됩니다([프로모션 코드](generate-promotional-codes.md)를 보유한 Windows 10 장치 사용 고객은 예외).

> [!IMPORTANT]
> 이 옵션은 제출에서 선택한 [표시 여부](choose-visibility-options.md#discoverability) 설정보다 우선합니다. 

이 옵션은 제출을 만들었을 때와 똑같은 효과가 있으며, **취득 중지** 옵션에서 **Store에서 사용할 수 있지만 검색 되지 않는 제품으로 설정**을 선택합니다. 그러나 새 제출을 만들 필요는 없습니다.

앱을 이미 구매한 고객은 계속 사용하고 다시 다운로드할 수 있으며, 나중에 새 패키지를 제출한 경우 업데이트를 받을 수도 있습니다.

앱을 사용할 수 없도록 설정한 후에도 대시보드에 계속 표시됩니다. 고객에게 앱을 다시 제공하려면 앱 개요 페이지에서 **앱을 사용할 수 있도록 설정**을 클릭합니다. 마지막 제출 시 설정에서 제한하지 않았다면 확인 후 몇 시간 내에 새 고객이 앱을 사용할 수 있게 됩니다.

> [!NOTE]
> 앱을 계속 사용할 수 있도록 하지만 특정 OS 버전의 새 고객에게는 제공하지 않으려는 경우 새 제출을 만들고, 새로운 구입을 방지하려는 OS 버전의 모든 패키지를 제거할 수 있습니다. 예를 들어 Windows Phone 8.1 및 Windows 10용 패키지가 이미 있으며 새로운 Windows Phone 8.1 고객에게는 앱을 계속 제공하지 않으려는 경우 제출에서 Windows Phone 8.1 패키지를 모두 제거합니다. 업데이트가 게시되면 새로운 Windows Phone 8.1 고객은 앱을 구입할 수 없습니다. 그러나 기존 보유 고객은 계속 사용할 수 있습니다. 그러나 Windows 10의 새 고객은 앱을 계속 사용할 수 있습니다.


## <a name="removing-packages-for-a-previously-supported-device-family"></a>이전에 지원되던 디바이스 패밀리용 패키지 제거

경우 대해 특정 [디바이스 패밀리](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview) 지원 하 던 앱 이전에 메시지가 표시 됩니다 **패키지** 페이지에서 변경 내용을 저장 하기 전에 의도 한 내용 인지 확인 하려면 패키지를 모두 제거 합니다.

모든 앱에서 이전에 지원 되는 디바이스 패밀리에서 실행 될 수 있는 패키지를 제거 하는 제출을 게시 하는 경우 새 고객에 게 해당 디바이스 패밀리에서 앱을 구입할 수 없습니다. 항상 나중에 또 다른 업데이트를 게시하여 해당 디바이스 패밀리용 패키지를 다시 제공할 수 있습니다.

특정 디바이스 패밀리를 지원하는 모든 패키지를 제거하더라도 이미 해당 장치 유형에 앱을 설치한 기존 고객은 계속 해당 앱을 사용할 수 있으며 나중에 제공되는 업데이트를 다운로드하게 되니 주의하세요.


<a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>

## <a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>이전에 게시된 앱에 Windows 10용 패키지 추가

스토어에 Windows 8.x 및/또는 Windows Phone 8.x를 대상으로 하는 앱이 있는 경우 [패키지](upload-app-packages.md) 단계 중 Windows 10용 앱을 업데이트하려면 새 제출을 만들고 UWP .appxupload 패키지를 업로드합니다. 앱 인증 프로세스를 거치면 이미 앱 및 Windows 10은 이제 고객은 업데이트로 UWP 패키지를 스토어에서 받을 합니다. Windows 10의 고객도 UWP 패키지를 새로 구매할 수 있습니다.

> [!NOTE]
> Windows 10의 고객이 UWP 패키지를 구입한 후에는 이전 OS 버전용 패키지를 사용하여 해당 고객을 롤백할 수 없습니다. 

Windows 10 패키지의 버전 번호는 포함할(또는 이전에 게시한 해당 OS 버전용 패키지) 모든 Windows 8, Windows 8.1 및/또는 Windows Phone 8.1 패키지의 버전 번호보다 높아야 합니다. 자세한 내용은 [패키지 버전 번호](package-version-numbering.md)를 참조하세요.

Store의 UWP 앱 패키징에 대한 자세한 내용은 [앱 패키징](../packaging/index.md)을 참조하세요.

> [!IMPORTANT]
> 범용 디바이스 패밀리를 대상으로 하는 패키지를 제공한 경우 이전 운영 체제(Windows Phone 8, Windows 8.1 등)에 앱이 이미 있는 모든 고객은 Windows 10으로 업그레이드하면 Windows 10 패키지로 업데이트됩니다.
> 
> 이런 이후 제출의 [장치 패밀리 가용성](device-family-availability.md) 단계에서 특정 디바이스 패밀리를 제외한 경우에는 섹션은 새 가져오기에만 적용 합니다. 일부 고객이 유니버설 Windows 10 패키지를 가져오지 못하도록 하려면 지원할 특정 장치 패밀리를 포함하도록 appx 매니페스트의 [**TargetDeviceFamily**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) 요소를 업데이트해야 합니다.
> 
> 예를 들어 새 UWP 앱을 다운로드할 Windows 10 데스크톱 장치를 업그레이드 하는 Windows 8 및 Windows 8.1 고객을 원하는 하지만 이제 이전에 패키지를 유지 하도록 Windows 10 Mobile 장치에서 내용이 availabl Windows Phone 고객 e (Windows Phone 8 또는 Windows Phone 8.1 대상). 이렇게 하려면 해야 **Windows.Desktop** (데스크톱 디바이스 패밀리)를 포함 하도록 appx 매니페스트의 [**TargetDeviceFamily**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) 업데이트 하는 **Windows.Universal** 값 (유니버설 디바이스 패밀리)으로 두지 말고 Microsoft Visual Studio에 기본적으로 매니페스트에 포함 됩니다. 유니버설 또는 모바일 디바이스 패밀리를 대상으로 하는 UWP 패키지를 제출하지 마세요(**Windows.Universal** 또는 **Windows.Universal**). Windows 10 Mobile 고객이 UWP 패키지를 가져오지 못합니다.


## <a name="maintaining-package-compatibility-for-windows-phone-81"></a>Windows Phone 8.1용 패키지 호환성 유지 관리

이전에 Windows Phone 8.1용으로 게시된 앱을 업데이트할 때 패키지 형식에 대한 특정 요구 사항이 적용됩니다.

-   앱에서 Windows Phone 8.1 패키지를 게시한 후에는 이후의 모든 업데이트에도 Windows Phone 8.1 패키지를 포함해야 합니다.
-   앱에서 Windows Phone 8.1 XAP를 게시한 후에는 후속 업데이트에 Windows Phone 8.1 XAP, Windows Phone 8.1 appx 또는 Windows Phone 8.1 appxbundle이 있어야 합니다.
-   앱에서 Windows Phone 8.1 XAP를 게시한 경우 후속 업데이트에 Windows Phone 8.1 .appx 또는 Windows Phone 8.1 .appxbundle이 있어야 합니다. 즉, Windows Phone 8.1 XAP는 허용되지 않습니다. 이는 Windows Phone 8.1.appx를 포함하는.appxupload에도 적용됩니다.
-   앱에서 Windows Phone 8.1 .appxbundle을 게시한 후에는 후속 업데이트에 Windows Phone 8.1 .appxbundle이 있어야 합니다. 즉, Windows Phone 8.1 XAP 또는 Windows Phone 8.1 .appx는 허용되지 않습니다. 이는 Windows Phone 8.1.appxbundle을 포함하는.appxupload에도 적용됩니다.

이러한 규칙을 따르지 않으면 패키지 업로드 오류가 발생하여 제출을 완료하지 못할 수 있습니다.