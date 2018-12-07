---
Description: After your packages have been successfully uploaded, you'll see a table that indicates which packages will be offered to specific Windows 10 device families (and earlier OS versions, if applicable), in ranked order.
title: 장치 패밀리 가용성
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, 패키지, 업로드, 장치 패밀리 가용성
ms.localizationpriority: medium
ms.openlocfilehash: 217a6ab9f25ee533a754138db5cf83c2ac81e3e9
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8781419"
---
# <a name="device-family-availability"></a>장치 패밀리 가용성

패키지가 **패키지** 페이지에 성공적으로 업로드되면 **장치 패밀리 가용성** 섹션에 특정 Windows 10 장치 패밀리(및 해당하는 경우 이전 OS 버전)에 제공될 패키지를 등급순으로 나타내는 표가 표시됩니다. 이 섹션에서는 특정 Windows 10 장치 패밀리의 고객에게 제출을 제공할 것인지도 선택할 수 있습니다.

> [!NOTE]
> 아직 패키지를 업로드하지 않은 경우 **장치 패밀리 가용성** 섹션에 Windows 10 장치 패밀리와 함께 제출이 이러한 장치 패밀리 고객에게 제공될지 여부를 나타내는 확인란이 표시됩니다. 표는 하나 이상의 패키지를 업로드할 때까지 표시되지 않습니다.

이 섹션에서는 Microsoft에서 이후의 Windows 10 장치 패밀리에 앱을 제공하도록 허용할지 여부를 지정할 수 있는 확인란도 제공됩니다. 새 장치 패밀리가 도입됨에 따라 더 많은 잠재 고객에게 앱을 제공할 수 있도록 이 상자를 선택된 상태로 유지하는 것이 좋습니다.


## <a name="choosing-which-device-families-to-support"></a>지원할 장치 패밀리 선택

개별 장치 패밀리를 대상으로 하는 패키지를 업로드하는 경우 해당 유형의 장치를 사용하는 신규 고객이 해당 패키지를 사용할 수 있도록 확인란을 선택합니다. 예를 들어, 패키지가 Windows.Desktop을 대상으로 하는 경우 해당 패키지에 대해 **Windows 10 데스크톱** 확인란이 선택되며 다른 장치 패밀리의 확인란은 선택할 수 없습니다.

Windows.Universal 장치 패밀리를 대상으로 하는 패키지는 모든 Windows 10 장치(Xbox One 포함)에서 실행할 수 있습니다. 기본적으로 새로운 고객이 Xbox를 *제외*한 모든 장치 유형에서 패키지를 사용할 수 있게 합니다.

해당 장치 형식의 고객에게 제출을 제공하지 않으려는 경우 Windows 10 장치 패밀리에 대한 확인란의 선택을 취소할 수 있습니다. 장치 패밀리 확인란을 선택하지 않은 경우 해당 장치 형식의 새 고객은 앱을 다운로드할 수 없습니다. 그러나 앱을 이미 다운로드한 고객은 계속 사용하고 제출한 업데이트를 받을 수 있습니다.

앱이 이를 지원하는 경우 앱을 다운로드할 수 있는 Windows 10 장치 형식에 제한이 없는 한 모든 확인란을 선택하는 것이 좋습니다. 예를 들어 앱이 [Surface Hub](https://developer.microsoft.com/windows/surfacehub) 및/또는 [Microsoft HoloLens](https://developer.microsoft.com/windows/mixed-reality)에 대해 적절한 환경을 제공하지 않는 경우 **Windows 10 Team** 및/또는 **Windows 10 Holographic** 확인란을 선택 해제할 수 있습니다. 이렇게 하면 새로운 고객이 해당 장치에서 앱을 다운로드할 수 없습니다. 나중에 이러한 고객에게 제공할 준비가 되면 이 확인란을 선택하여 새 제출을 생성할 수 있습니다.

<span id="xbox" />

Windows.Universal 패키지에 대해 기본적으로 선택되지 않은 유일한 Windows 10 장치 패밀리는 **Windows 10 Xbox**입니다. 앱이 게임이 아니거나 또는 게임인데 [Xbox Live 크리에이터스 프로그램](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md)을 활성화했거나 [개념 승인](../gaming/concept-approval.md) 프로세스를 마쳤고 제출에 Windows 10 SDK 버전 14393 이상을 사용하여 컴파일된 중립 및/또는 x64 UWP 패키지가 포함된 경우 **Windows 10 Xbox** 확인란을 선택하여 Xbox One 고객에게 앱을 제공할 수 있습니다.

> [!IMPORTANT]
> Xbox 장치에서 앱을 시작하려면 Windows SDK 버전 14393 이상으로 컴파일된 중립 또는 x64 패키지를 포함해야 합니다. 그러나 **Windows 10 Xbox**를 선택한 경우, 이전 버전의 SDK를 사용하여 컴파일된 경우에도 항상 Xbox에서 적용할 수 있는 가장 높은 버전의 패키지(즉, Xbox 또는 유니버설 장치 패밀리를 대상으로 하는 중립 또는 x64 패키지)가 Xbox 고객에게 제공됩니다. 그러므로 Xbox에 적용할 수 있는 가장 높은 버전의 패키지가 Windows SDK 버전 14393 이상을 사용하여 컴파일되었는지 확인하는 것이 중요합니다. 그렇지 않은 경우 Xbox 고객이 앱을 시작할 수 없음을 나타내는 오류 메시지가 표시됩니다. 
> 
> 이 오류를 해결하려면 다음 중 하나를 수행합니다.
> - 해당 패키지를 Windows SDK 버전 14393 이상을 사용하여 컴파일된 새 패키지로 바꿉니다.
> - 패키지가 Xbox를 지원하고 Windows SDK 14393 이상 버전으로 컴파일된 경우 제출에 더 높은 버전의 패키지가 포함되도록 해당 버전 번호를 높입니다.
> - **Windows 10 Xbox** 확인란의 선택을 취소합니다.
>   
> 이 문제를 계속 해결할 수 없는 경우 고객 지원 센터에 문의합니다.

Windows 10 IoT Core에 대한 UWP 앱을 제출하는 경우, 패키지를 업로드한 후 기본 선택을 변경해서는 안 되기 때문에 Windows 10 IoT을 위한 별도의 확인란이 없습니다. IoT Core UWP 앱 게시에 대한 자세한 내용은 [IoT Core UWP앱에 대한 Microsoft Store 지원](https://docs.microsoft.com/windows/iot-core/commercialize-your-device/installingandservicing)을 참조하세요.

이전에 게시 된 앱 제출에 **Windows 8/8.1** 에서 실행할 수 있는 패키지를 포함 하는 경우와 **Windows Phone 8.x 이전**, 이러한 패키지 사용 가능 하 게 해당 OS 버전 고객에 게 합니다. 이러한 고객에게 앱을 제공하지 않으려면 제출에서 해당 패키지를 제거합니다.

> [!IMPORTANT]
> 특정 Windows 10 장치 패밀리에서 제출을 완전히 방지 하려면 지원 하려는 장치 패밀리를 대상으로 매니페스트의 [**TargetDeviceFamily**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) 요소를 업데이트 (즉, Windows.Mobile 또는 Windows.Desktop) 대신 그대로 두지 말고는 Windows.Universal 값 (유니버설 장치 패밀리의 경우) 하는 보다 Microsoft Visual Studio가 기본적으로 매니페스트에 포함 합니다.

**장치 패밀리 가용성** 섹션에서 선택한 내용은 새 구입에만 적용되는 점을 기억하세요. 앱을 이미 소유하고 있는 사용자는 이를 계속 사용할 수 있으며, 여기에서 해당 장치 패밀리를 제거한 경우에도 제출한 업데이트를 받게 됩니다. 이는 Windows 10으로 업그레이드하기 전에 앱을 다운로드한 고객에게도 적용됩니다. 예를 들어 Windows Phone 8.1 패키지를 사용 하 여 게시 된 앱이 있는 Windows.Universal 장치 패밀리를 대상으로 Windows 10 (UWP) 패키지를 추가 하는 경우 Windows 10 mobile 고객이 Windows Phone 8.1 패키지는 업데이트 제공이 창 10 (UWP) 패키지를 선택한 경우에 선택 되지 않은 **Windows 10 Mobile**용 상자.

장치 패밀리에 대한 자세한 내용은 [**장치 패밀리 개요**](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview)를 참조하세요.


## <a name="understanding-ranking"></a>순위 이해

외 제출을 다운로드할 수 있는 Windows 10 장치 패밀리, **장치 패밀리 가용성** 섹션에는 사용 가능 하 게 다른 장치 패밀리에 특정 패키지를 보여 줍니다. 특정 장치 패밀리에서 실행할 수 있는 패키지가 두 개 이상 있는 경우 패키지 버전 번호를 기준으로 패키지를 제공할 순서가 표에 나타납니다. 버전 번호를 기준으로 Store에서 패키지의 순위를 매기는 방법은 [패키지 버전 번호](package-version-numbering.md)를 참조하세요. 

예를 들어 Package_A.appxupload 및 Package_B.appxupload 두 가지 패키지가 있다고 가정합니다. 특정 장치 패밀리의 경우 Package_A.appxupload의 순위가 1이고 Package_B.appxupload의 순위가 2이면 해당 장치 형식의 고객이 앱을 다운로드할 때 Store에서 먼저 Package_A.appxupload를 제공한다는 의미입니다. 고객의 장치에서 Package_A.appxupload를 실행할 수 없는 경우 Store에서는 Package_B.appxupload를 제공합니다. 고객의 장치에 해당 디바이스 패밀리에 대 한 패키지를 실행할 수 없는 (예를 들어 **MinVersion** 앱 지 원하는 경우는 고객의 장치에서 버전 보다 높으면) 다음 고객이 해당 장치에서 앱을 다운로드할 수 없습니다.

> [!NOTE]
> (이전에 게시 된 앱)에 대 한.xap 패키지의 버전 번호는 지정 된 고객에 게 제공할 패키지를 결정할 때 아닙니다. 그러므로 동일한 순위의 .xap 패키지가 두 개 이상인 경우 번호 대신 별표가 표시되고 고객이 두 패키지 중 하나를 받게 됩니다. 한 .xap 패키지에서 최신 패키지로 고객을 업데이트하려면 새 제출에서 이전 .xap를 제거해야 합니다.

