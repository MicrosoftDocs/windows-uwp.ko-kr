---
description: 패키지를 성공적으로 업로드 한 후에는 특정 Windows 10 장치 제품군 (해당 하는 경우 이전 OS 버전)에 제공 되는 패키지를 순위에 따라 나타내는 표가 표시 됩니다.
title: 디바이스 패밀리 가용성
ms.date: 03/21/2019
ms.topic: article
keywords: windows 10, uwp, 패키지, 업로드, 장치 패밀리 가용성
ms.localizationpriority: medium
ms.openlocfilehash: 48cbb0fd9ecf27c9926d55e22abc17d039d3674b
ms.sourcegitcommit: efa5f793607481dcae24cd1b886886a549e8d6e5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89411967"
---
# <a name="device-family-availability"></a>디바이스 패밀리 가용성

**패키지 페이지에서** 패키지를 업로드 한 후에는 **장치 패밀리 가용성** 섹션에 특정 Windows 10 장치 제품군 (해당 하는 경우 이전 OS 버전)에 제공 되는 패키지를 순위에 따라 나타내는 표가 표시 됩니다. 또한이 섹션에서는 특정 Windows 10 장치 제품군에 대 한 고객의 제출을 제공할 것인지 여부를 선택할 수 있습니다.

> [!NOTE]
> 아직 패키지를 업로드 하지 않은 경우 **장치 제품군 가용성** 섹션에는 해당 장치 제품군의 고객에 게 제출이 제공 되는지 여부를 나타낼 수 있는 확인란이 있는 Windows 10 장치 패밀리가 표시 됩니다. 하나 이상의 패키지를 업로드 하면 테이블이 표시 됩니다.

또한이 섹션에는 Microsoft에서 향후 Windows 10 장치 제품군에 앱을 사용할 수 있도록 허용할지 여부를 지정할 수 있는 확인란만 포함 되어 있습니다. 새 장치 패밀리가 도입 되 면 앱을 더 많은 잠재 고객에 게 제공할 수 있도록이 확인란을 선택 된 상태로 유지 하는 것이 좋습니다.


## <a name="choosing-which-device-families-to-support"></a>지원할 장치 패밀리 선택

하나의 개별 장치 제품군을 대상으로 하는 패키지를 업로드 하는 경우 해당 유형의 장치에서 새 고객이 해당 패키지를 사용할 수 있도록 하는 확인란을 선택 합니다. 예를 들어 패키지가 Windows 데스크톱을 대상으로 하는 경우 해당 패키지에 대 한 **windows 10 Desktop** 상자를 확인 하 고 다른 장치 패밀리의 상자를 선택할 수 없습니다.

Windows를 대상으로 하는 패키지는 windows 10 장치 (Xbox One 포함)에서 실행할 수 있습니다. 기본적으로 Xbox를 *제외한* 모든 장치 유형에 서 새 고객이 해당 패키지를 사용할 수 있도록 합니다.

해당 장치 유형에 서 고객에 게 제출을 제공 하지 않으려는 경우 Windows 10 장치 제품군에 대 한 확인란의 선택을 취소할 수 있습니다. 장치 패밀리의 상자를 선택 하지 않으면 해당 장치 유형의 새 고객이 앱을 획득할 수 없게 됩니다. 앱이 이미 있는 고객은 앱을 계속 사용할 수 있지만 제출 하는 모든 업데이트를 받게 됩니다.

앱에서 지 원하는 경우 앱을 획득할 수 있는 Windows 10 장치 유형을 제한 하는 특별 한 이유가 없다면 모든 확인란을 선택 된 상태로 유지 하는 것이 좋습니다. 예를 들어 앱이 [Surface Hub](https://developer.microsoft.com/windows/surfacehub) 및/또는 [Microsoft HoloLens](https://developer.microsoft.com/mixed-reality)에서 적절 한 환경을 제공 하지 않는 경우 **windows 10 팀** 및/또는 **windows 10 Holographic** 상자의 선택을 취소할 수 있습니다. 이렇게 하면 새 고객이 해당 장치에서 앱을 획득할 수 없습니다. 나중에 해당 고객에 게 제공할 준비가 된 것으로 판단 되 면 확인란을 선택 하 여 새 제출을 만들 수 있습니다.

<span id="xbox" />

Windows에 대해 기본적으로 선택 되어 있지 않은 유일한 Windows 10 장치 패밀리입니다. 범용 패키지는 **windows 10 Xbox**입니다. 앱이 게임이 아닌 경우 (또는 게임이 고 [Xbox Live 크리에이터 프로그램](/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators) 을 사용 하도록 설정 했거나 [개념 승인](../gaming/concept-approval.md) 프로세스를 통해) 제출에 windows 10 SDK 버전 14393 이상을 사용 하 여 컴파일된 중립 및/또는 x64 UWP 패키지가 포함 된 경우, **windows 10 xbox** box를 확인 하 여 앱을 xbox one의 고객에 게 제공할 수 있습니다.

> [!IMPORTANT]
> Xbox 장치에서 앱을 시작 하려면 Windows SDK 버전 14393 이상으로 컴파일된 중립 또는 x64 패키지를 포함 해야 합니다. 그러나 **Windows 10 xbox**를 확인 하는 경우 xbox에 적용 가능한 가장 높은 버전의 패키지 (즉, Xbox 또는 유니버설 장치 제품군을 대상으로 하는 중립 또는 x64 패키지)가 이전 SDK 버전을 사용 하 여 컴파일된 경우에도 xbox의 고객에 게 항상 제공 됩니다. 이로 인해 Xbox에 적용 가능한 가장 높은 버전의 패키지가 Windows SDK 버전 14393 이상으로 컴파일되는지 확인 하는 것이 중요 합니다. 그렇지 않으면 Xbox 고객이 앱을 시작할 수 없다는 오류 메시지가 표시 됩니다. 
> 
> 이 오류를 해결 하려면 다음 중 하나를 수행 하면 됩니다.
> - 적용 가능한 패키지를 Windows SDK 버전 14393 이상을 사용 하 여 컴파일되는 새 패키지로 바꿉니다.
> - Xbox를 지 원하는 패키지가 이미 있고 Windows SDK 버전 14393 이상으로 컴파일된 경우 제출에서 가장 높은 버전의 패키지가 되도록 버전 번호를 늘립니다.
> - **Windows 10 Xbox**의 확인란을 선택 취소 합니다.
>   
> 그래도 문제를 해결할 수 없는 경우 지원 담당자에 게 문의 하세요.

Windows 10 IoT Core 용 UWP 앱을 제출 하는 경우 패키지를 업로드 한 후에 기본 선택 항목을 변경 하면 안 됩니다. Windows 10 IoT에 대 한 별도의 확인란은 없습니다. IoT Core UWP 앱을 게시 하는 방법에 대 한 자세한 내용은 [Iot CORE uwp 앱에 대 한 Microsoft Store 지원](/windows/iot-core/commercialize-your-device/installingandservicing)을 참조 하세요.

이전에 게시 된 앱에 대 한 제출에 **Windows 8/8.1** 및 **Windows Phone .x**에서 실행할 수 있는 패키지가 포함 된 경우 해당 패키지는 해당 OS 버전에서 고객이 사용할 수 있게 됩니다. 이러한 고객에 게 앱의 제공을 중지 하려면 제출에서 해당 패키지를 제거 합니다.

> [!IMPORTANT]
> 특정 Windows 10 장치 패밀리가 제출을 얻지 못하게 하려면 매니페스트에 [**Targetdevicefamily**](/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) 요소를 업데이트 하 여 기본적으로 매니페스트에 포함 Microsoft Visual Studio 하는 Windows. 범용 값 (유니버설 장치 제품군의 경우)으로 남겨 두지 말고 지원 하려는 장치 제품군만 대상으로 지정 합니다 (예: windows Mobile 또는 windows 데스크톱).

**장치 제품군 가용성** 섹션에서 선택 하는 항목은 새로운 합병에만 적용 된다는 점을 명심 해야 합니다. 앱이 이미 있는 모든 사용자는이를 계속 사용할 수 있으며 여기에서 장치 제품군을 제거 하는 경우에도 제출 하는 모든 업데이트를 받게 됩니다. 이는 Windows 10으로 업그레이드 하기 전에 앱을 구입한 고객에도 적용 됩니다. 예를 들어 Windows Phone 8.1 패키지를 포함 하는 게시 된 응용 프로그램이 있고 windows 10 Mobile을 대상으로 하는 windows 10 (UWP) 패키지를 추가 하는 경우, windows 10 **mobile**에 대 한 확인란의 선택을 취소 한 경우에도 Windows Phone 8.1 패키지를 사용 하는 windows 10 mobile 고객에 게이 windows 10 (uwp) 패키지에 대 한 업데이트가 제공 됩니다.

장치 제품군에 대 한 자세한 내용은 [확장 sdk를 사용한 프로그래밍](/uwp/extension-sdks/device-families-overview)을 참조 하세요.


## <a name="understanding-ranking"></a>순위 이해

어떤 Windows 10 장치 제품군이 제출을 다운로드할 수 있는지를 지정 하는 것 외에도, **장치 제품군 가용성** 섹션에는 여러 장치 제품군에서 사용할 수 있는 특정 패키지가 표시 됩니다. 특정 장치 제품군에서 실행할 수 있는 패키지가 두 개 이상 있는 경우이 테이블은 패키지의 버전 번호를 기준으로 패키지가 제공 되는 순서를 표시 합니다. 저장소에서 버전 번호를 기준으로 패키지의 순위를 지정 하는 방법에 대 한 자세한 내용은 [패키지 버전 번호 매기기](package-version-numbering.md)를 참조 하세요. 

예를 들어 두 개의 패키지 (Package_A .appxupload Package_B 및 .appxupload)가 있다고 가정 합니다. 지정 된 장치 제품군의 Package_A 경우 .appxupload가 1 및 Package_B 순위를 지정 합니다. 즉, .appxupload의 순위는 2로 지정 됩니다. 즉, 해당 장치 형식의 고객이 앱을 가져오면 스토어는 먼저 .appxupload를 Package_A 배달 하려고 시도 합니다. 고객의 장치가 .appxupload을 Package_A 실행할 수 없는 경우 스토어는 .appxupload을 Package_B 제공 합니다. 고객의 장치가 해당 장치 제품군에 대 한 패키지를 실행할 수 없는 경우 (예: 앱이 지 원하는 **MinVersion** 가 고객 장치의 버전 보다 높은 경우) 고객은 해당 장치에서 앱을 다운로드할 수 없습니다.

> [!NOTE]
> 이전에 게시 된 앱에 대 한 .xap 패키지의 버전 번호는 지정 된 고객을 제공할 패키지를 결정할 때 고려 되지 않습니다. 이 때문에 동일한 순위의 .xap 패키지가 두 개 이상 있는 경우 숫자가 아닌 별표가 표시 되 고 고객은 두 패키지를 받을 수 있습니다. 하나의 .xap 패키지에서 최신 버전으로 고객을 업데이트 하려면 새 전송에서 이전 .xap를 제거 해야 합니다.