---
author: jnHs
Description: "고객에게 앱 패키지를 제공하는 방법 및 특정 패키지 시나리오를 관리하는 방법을 알아봅니다."
title: "앱 패키지 관리 지침"
ms.assetid: 55405D0B-5C1E-43C8-91A1-4BFDD336E6AB
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 68ed87147ed3cc3e1155eb1ab6d301867ba1ae55

---

# 앱 패키지 관리 지침


고객에게 앱 패키지를 제공하는 방법 및 특정 패키지 시나리오를 관리하는 방법에 대해 알아봅니다.

-   [OS 버전 및 패키지 배포](#os-versions-and-package-distribution)
-   [이전에 게시된 앱에 Windows 10용 패키지 추가](#adding-packages-for-windows-10-to-a-previously-published-app)
-   [Windows Phone 8.1용 패키지 호환성 유지 관리](#maintaining-package-compatibility-for-windows-phone-8-1)
-   [스토어에서 앱 제거](#removing-an-app-from-the-store)
-   [이전에 지원되던 디바이스 패밀리용 패키지 제거](#removing-packages-for-a-previously-supported-device-family)

## OS 버전 및 패키지 배포


여러 운영 체제에서 서로 다른 유형의 패키지를 실행할 수 있습니다. 고객의 장치에서 둘 이상의 패키지를 실행할 수 있는 경우 Windows 스토어는 최적의 패키지를 제공합니다.

일반적으로 이후 OS 버전은 동일한 디바이스 패밀리에 대해 이전 OS 버전을 대상으로 하는 패키지를 실행할 수 있습니다. 그러나 앱에 현재 OS 버전을 대상으로 하는 패키지가 없는 경우에만 이러한 패키지를 받게 됩니다.

예를 들어 Windows 10 장치는 이전에 지원된 모든 OS 버전(디바이스 패밀리별)을 실행할 수 있습니다. Windows 10 데스크톱 장치는 Windows 8.1 또는 Windows 8용으로 빌드된 앱을 실행할 수 있습니다. Windows 10 Mobile 장치는 Windows Phone 8.1, Windows Phone 8 및 Windows Phone 7.x용으로 빌드된 앱을 실행할 수 있습니다.

다음 예제에서는 서로 다른 OS 버전을 대상으로 하는 패키지가 포함된 앱에 대한 다양 한 시나리오를 보여 줍니다. 경우에 따라 패키지의 특정 제약 조건으로 인해 여기에 나열된 일부 OS 버전 및 장치 유형에서 패키지를 실행하지 못할 수 있지만(예: 아키텍처가 적합해야 함) 이러한 예제는 특정 패키지에서 실행할 수 있는 OS 버전을 이해하는 데 도움이 됩니다.

### 예제 앱 1

| 패키지의 대상 운영 체제 | 이 패키지를 얻을 수 있는 운영 체제 |
|-------------------------------------|----------------------------------------------|
| Windows 8.1                         | Windows 10 데스크톱 장치, Windows 8.1      |
| Windows Phone 8.1                   | Windows 10 Mobile 장치, Windows Phone 8.1 |
| Windows Phone 8                     | Windows Phone 8                              |
| Windows Phone 7.1                   | Windows Phone 7.x                            |

예제 앱 1에는 특별히 Windows 10 장치용으로 빌드된 UWP(유니버설 Windows 플랫폼) 패키지가 아직 없지만 Windows 10 고객은 앱을 가져올 수 있습니다. 이러한 고객은 해당 장치 유형에 따라 사용 가능한 최상의 패키지를 얻게 됩니다.

### 예제 앱 2

| 패키지의 대상 운영 체제  | 이 패키지를 가져올 수 있는 운영 체제 |
|--------------------------------------|----------------------------------------------|
| Windows 10(유니버설 디바이스 패밀리) | Windows 10(모든 디바이스 패밀리)             |
| Windows 8.1                          | Windows 8.1                                  |
| Windows Phone 8.1                    | Windows Phone 8.1                            |
| Windows Phone 7.1                    | Windows Phone 7.x, Windows Phone 8           |

예제 앱 2에는 Windows 8에서 실행할 수 있는 패키지가 없습니다. 그 외에 다른 모든 OS 버전을 실행하는 고객은 앱을 받을 수 있습니다.

### 예제 앱 3

| 패키지의 대상 운영 체제 | 이 패키지를 가져올 수 있는 운영 체제                  |
|-------------------------------------|---------------------------------------------------------------|
| Windows 10(데스크톱 디바이스 패밀리)  | Windows 10 데스크톱 장치                                    |
| Windows Phone 8                     | Windows 10 Mobile 장치, Windows Phone 8, Windows Phone 8.1 |

예제 앱 3에는 모바일 디바이스 패밀리를 대상으로 하는 UWP 패키지가 없으므로 Windows 10 Mobile 장치 고객은 Windows Phone 8 패키지를 가져오게 됩니다. 이 앱에서 나중에 모바일 디바이스 패밀리(또는 범용 디바이스 패밀리)를 대상으로 하는 패키지를 추가한 경우 Windows 10 Mobile 장치 고객은 Windows Phone 8 패키지 대신 이 패키지를 사용할 수 있습니다.

이 예제 앱에는 Windows 7.x에서 실행할 수 있는 패키지가 포함되어 있지 않습니다.

### 예제 앱 4

| 패키지의 대상 운영 체제  | 이 패키지를 가져올 수 있는 운영 체제 |
|--------------------------------------|----------------------------------------------|
| Windows 10(유니버설 디바이스 패밀리) | Windows 10(모든 디바이스 패밀리)             |

예제 앱 4에서는 Windows 10을 실행하는 장치에서 앱을 가져올 수 있지만 이전 OS 버전의 고객은 사용할 수 없습니다. UWP 패키지는 유니버설 디바이스 패밀리를 대상으로 하므로 데스크톱 및 모바일 Windows 10 장치 둘 다에서 사용할 수 있습니다.

## 이전에 게시된 앱에 Windows 10용 패키지 추가


스토어에 앱이 있는 경우 [패키지](upload-app-packages.md) 단계 중 Windows 10용 앱을 업데이트하려면 새 제출을 만들고 UWP .appxupload 패키지를 업로드합니다. 앱이 인증 프로세스를 통과하면 Windows 10으로 업그레이드하기 전에 앱이 이미 있는 고객은 스토어에서 업데이트로 UWP 패키지를 가져올 수 있습니다. Windows 10의 고객도 UWP 패키지를 새로 구매할 수 있습니다.

> **중요** Windows 10의 고객이 UWP 패키지를 구입한 후에는 이전 OS 버전용 패키지를 사용하여 해당 고객을 롤백할 수 없습니다. 제출에 Windows 10의 UWP 패키지를 추가하기 전에 철저하게 테스트했는지 확인하세요.

다른 패키지를 동시에 업데이트하거나 제출을 변경할 수 있습니다. 예를 들어 [플랫폼 관련 설명을 작성](create-platform-specific-descriptions.md)하여 이전 OS 버전 고객에게 표시할 수 있습니다. 원하는 경우 모든 것으로 정확히 동일하게 유지할 수도 있습니다.

> **참고** Windows 10 패키지의 버전 번호는 동일한 앱에 대해 게시할 예정인(또는 이전에 게시한 해당 OS 버전용 패키지) 모든 Windows 8, Windows 8.1 및/또는 Windows Phone 8.1 패키지의 버전 번호보다 높아야 합니다. Windows 10의 버전 번호에 대한 자세한 내용은 [패키지 버전 번호](package-version-numbering.md)를 참조하세요.

새 제출에 대한 인증 프로세스가 완료되면 아직 Windows 10이 없는 고객에게 제공한 다른 패키지와 함께 UWP 패키지를 사용할 수 있습니다.

스토어용 UWP 앱을 패키징하는 방법에 대한 자세한 내용은 [Windows 10용 유니버설 Windows 앱 패키징](http://go.microsoft.com/fwlink/p/?LinkId=620193 )을 참조하세요.

> **중요** 범용 디바이스 패밀리를 대상으로 하는 패키지를 제공한 경우 이전 운영 체제(Windows Phone 8, Windows 8.1 등)에 앱이 이미 있는 모든 고객은 Windows 10으로 업그레이드하면 Windows 10 유니버설 패키지로 업데이트됩니다.
> 
> **디바이스 패밀리** 선택은 새 가져오기에만 적용되므로 이는 제출의 [가격 책정 및 가용성](set-app-pricing-and-availability.md#windows-10-device-families) 단계에서 특정 디바이스 패밀리를 제외한 경우에도 마찬가지입니다. 일부 고객이 새 Windows 10 패키지를 가져오지 못하도록 하려면 지원할 특정 디바이스 패밀리를 포함하도록 appx 매니페스트의 [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903) 요소를 업데이트해야 합니다.
> 
> 예를 들어 Windows 10으로 업그레이드한 Windows 8 및 Windows 8.1 고객만 UWP 앱을 가져올 수 있도록 하고 Windows Phone 8.1 이하 고객은 이전에 제공된 패키지(Windows Phone 8 또는 Windows Phone 8.1 대상)를 유지하도록 할 수 있습니다. 이렇게 하려면 Microsoft Visual Studio에서 appx 매니페스트에 기본적으로 포함하는 **Windows.Universal** 값(유니버설 디바이스 패밀리의 경우)으로 두지 말고 **Windows.Desktop**(데스크톱 디바이스 패밀리의 경우)만 포함하도록 appx 매니페스트의 [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903) 요소를 업데이트해야 합니다. 유니버설 또는 모바일 디바이스 패밀리를 대상으로 하는 UWP 패키지를 제출하지 마세요(**Windows.Universal** 또는 **Windows.Universal**). Windows 10 Mobile 고객이 UWP 패키지를 가져오지 못합니다.
> 
> 디바이스 패밀리에 대한 자세한 내용은 [UWP(유니버설 Windows 플랫폼) 앱 지침](https://msdn.microsoft.com/library/windows/apps/dn894631)을 참조하세요.

## Windows Phone 8.1용 패키지 호환성 유지 관리


이전에 Windows Phone 8.1용으로 게시된 앱을 업데이트할 때 패키지 형식에 대한 특정 요구 사항이 적용됩니다.

-   앱에서 Windows Phone 8.1 패키지를 게시한 후에는 이후의 모든 업데이트에도 Windows Phone 8.1 패키지를 포함해야 합니다.
-   앱에서 Windows Phone 8.1 XAP를 게시한 후에는 후속 업데이트에 Windows Phone 8.1 XAP, Windows Phone 8.1 appx 또는 Windows Phone 8.1 appxbundle이 있어야 합니다.
-   앱에서 Windows Phone 8.1 XAP를 게시한 경우 후속 업데이트에 Windows Phone 8.1 .appx 또는 Windows Phone 8.1 .appxbundle이 있어야 합니다. 즉, Windows Phone 8.1 XAP는 허용되지 않습니다. 이는 Windows Phone 8.1.appx를 포함하는.appxupload에도 적용됩니다.
-   앱에서 Windows Phone 8.1 .appxbundle을 게시한 후에는 후속 업데이트에 Windows Phone 8.1 .appxbundle이 있어야 합니다. 즉, Windows Phone 8.1 XAP 또는 Windows Phone 8.1 .appx는 허용되지 않습니다. 이는 Windows Phone 8.1.appxbundle을 포함하는.appxupload에도 적용됩니다.

이러한 규칙을 따르지 않으면 패키지 업로드 오류가 발생하여 제출을 완료하지 못하게 됩니다.

## 스토어에서 앱 제거


고객에 대한 앱 제공을 완전히 중지, 즉 "게시를 취소"해야 할 때가 있습니다. 이렇게 하려면 앱 개요 페이지에서 **앱을 사용할 수 없도록 설정**을 클릭합니다. 앱을 사용할 수 없도록 설정하면 몇 시간 내에 해당 앱이 스토어에 더 이상 표시되지 않고 새 고객이 홍보 코드를 포함한 어떠한 방법으로도 다운로드할 수 없게 됩니다.

> **중요** 이 작업은 제출에서 선택한 모든 [배포 및 표시 여부](set-app-pricing-and-availability.md#distribution-and-visibility) 설정보다 우선합니다.

앱을 이미 구매한 고객은 계속 사용할 수 있으며, 나중에 새 패키지를 제출한 경우 업데이트를 받을 수도 있습니다.

앱을 사용할 수 없도록 설정한 후에도 대시보드에 계속 표시됩니다. 고객에게 앱을 다시 제공하려면 앱 개요 페이지에서 **앱을 사용할 수 있도록 설정**을 클릭합니다. 마지막 제출 시 설정에서 제한하지 않았다면 확인 후 몇 시간 내에 새 고객이 앱을 사용할 수 있게 됩니다.

> **참고** 앱을 계속 사용할 수 있도록 하지만 특정 OS 버전의 고객에게는 제공하지 않으려는 경우 새 제출을 만들고, 새로운 구입을 방지하려는 OS 버전의 모든 패키지를 제거할 수 있습니다. 예를 들어 Windows Phone 8, Windows Phone 8.1 및 Windows 10용 패키지가 이미 있으며 새로운 Windows Phone 8 고객에게는 앱을 계속 제공하지 않으려는 경우 제출에서 Windows Phone 8 패키지를 제거합니다. 업데이트가 게시되면 새로운 Windows Phone 8 고객은 앱을 구입할 수 없습니다. 그러나 기존 보유 고객은 계속 사용할 수 있습니다. 또한 새로운 Windows Phone 8.1 고객 및 Windows 10 고객은 여전히 앱을 사용할 수 있습니다.

## 이전에 지원되던 디바이스 패밀리용 패키지 제거


앱에서 이전에 지원하던 특정 디바이스 패밀리용 패키지를 모두 제거하는 경우 **패키지** 페이지에서 변경 내용을 저장하기 전에 먼저 이러한 제거가 의도한 내용인지 확인하는 메시지가 표시됩니다.

앱에서 이전에 지원하던 디바이스 패밀리용 패키지를 제거하는 제출을 게시하는 경우 새 고객은 해당 디바이스 패밀리에서 앱을 구입할 수 없습니다. 항상 나중에 또 다른 업데이트를 게시하여 해당 디바이스 패밀리용 패키지를 다시 제공할 수 있습니다.

특정 디바이스 패밀리를 지원하는 모든 패키지를 제거하더라도 이미 해당 장치 유형에 앱을 설치한 기존 고객은 계속 해당 앱을 사용할 수 있으며 나중에 제공되는 업데이트를 다운로드하게 되니 주의하세요.

 

 







<!--HONumber=Aug16_HO3-->


