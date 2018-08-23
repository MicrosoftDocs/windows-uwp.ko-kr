---
author: jnHs
Description: The Packages page is where you upload all of the package files (.appxupload, .appx, .appxbundle, and/or .xap) for the app that you're submitting.
title: 앱 패키지 업로드
ms.assetid: B1BB810D-3EAA-4FB5-B03C-1F01AFB2DE36
ms.author: wdg-dev-content
ms.date: 5/10/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 패키지, 업로드, 패키지 업로드
ms.localizationpriority: medium
ms.openlocfilehash: 6013a238cff8db3b85dd98af58cccaf344a72f51
ms.sourcegitcommit: 9c79fdab9039ff592edf7984732d300a14e81d92
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/23/2018
ms.locfileid: "2817371"
---
# <a name="upload-app-packages"></a>앱 패키지 업로드

**패키지** 페이지에서는 제출 중인 앱의 모든 패키지 파일(.appx, .appxupload, .appxbundle 및/또는 .xap)을 업로드합니다. 이 단계에서 앱의 대상으로 지정한 운영 체제의 패키지를 업로드할 수 있습니다. 고객이 앱을 다운로드하면 스토어에서는 각 고객에게 해당 장치에 가장 적합한 패키지를 자동으로 제공합니다. 패키지를 업로드하면 [특정 Windows 10 장치 패밀리(및 해당하는 경우 이전 OS 버전)에 제공될 패키지](#device-family-availability)를 등급순으로 나타내는 표가 표시됩니다.

패키지 내용 및 패키지 구성 방법에 대한 자세한 내용은 [앱 패키지 요구 사항](app-package-requirements.md)을 참조하세요. 또한 [특정 고객에 게 전달 되는 버전 번호는 패키지에 끼칠 수](package-version-numbering.md) 및 [다양 한 운영 체제에 패키지는 배포 하는 방법](guidance-for-app-package-management.md)에 대해 설명 합니다.

## <a name="uploading-packages-to-your-submission"></a>제출에 패키지 업로드

패키지를 업로드하려면 패키지를 업로드 필드로 끌거나 파일을 클릭하여 찾아봅니다. **패키지** 페이지에서 .xap, .appx, .appxupload 및/또는 .appxbundle 파일을 업로드할 수 있습니다.

> [!IMPORTANT]
> Windows 10의 경우 여기에 .appx 또는.appxbundle이 아니라 .appxupload 파일을 업로드하는 것이 좋습니다.  스토어에서의 UWP 앱 패키징에 대한 자세한 내용은 [Visual Studio를 사용하여 UWP 앱 패키징](../packaging/packaging-uwp-apps.md)을 참조하세요.

앱에 대한 [패키지 플라이트](package-flights.md)를 만든 경우 패키지 플라이트 중 하나에서 패키지를 복사하는 옵션이 포함된 드롭다운이 표시됩니다. 끌어오려는 패키지가 있는 패키지 플라이트를 선택합니다. 그런 다음 해당 패키지를 일부 또는 전부 선택하여 이 제출에 포함할 수 있습니다.

유효성을 검사 하는 동안 패키지를 사용 하 여 오류를 검색 했습니다 잘못 된 것을 알 수 있습니다 메시지가 표시 됩니다 했습니다. 패키지를 제거, 문제를 해결 하 고 다음 다시 업로드를 시도에 필요 합니다. 문제를 일으킬 수 있지만 제출을 계속하지 못하게 차단하지는 않는 정보를 알리는 경고가 표시될 수도 있습니다.


## <a name="device-family-availability"></a>디바이스 패밀리 가용성

패키지가 성공적으로 업로드되면 **장치 패밀리 가용성** 섹션에 특정 Windows 10 장치 패밀리(및 해당하는 경우 이전 OS 버전)에 제공될 패키지를 등급순으로 나타내는 표가 표시됩니다. 이 섹션에서는 특정 Windows 10 장치 패밀리의 고객에게 제출을 제공할 것인지도 선택할 수 있습니다.

자세한 내용은 [디바이스 패밀리 가용성](device-family-availability.md)을 참조하세요.


## <a name="package-details"></a>패키지 세부 정보

업로드 된 패키지는 대상 운영 체제에 따라 그룹화 되는 여기에 나열 됩니다. 패키지의 이름, 버전 및 아키텍처가 표시됩니다. 각 패키지에서 지원되는 언어, 앱 기능 및 파일 크기 등 추가 정보를 표시하려면 **세부 정보 표시**를 클릭합니다.

제출에서 패키지를 제거해야 하는 경우 각 패키지의 **세부 정보** 섹션 아래쪽에서 **제거** 링크를 클릭합니다.


## <a name="removing-redundant-packages"></a>중복 패키지 제거

중복된 패키지가 하나 이상 감지되면 이 제출에서 중복 패키지를 제거하도록 제안하는 경고가 표시됩니다. 이전에 패키지를 업로드했고 이제 같은 고객 집합을 지원하는 더 높은 버전의 패키지를 제공하면 이 문제가 발생합니다. 이 경우 이제 이러한 고객을 지원하는 더 나은 패키지(상위 버전)가 있으므로 고객에게 중복 패키지가 제공되지 않습니다.

중복 패키지가 감지되면 모든 중복 패키지를 이 제출에서 자동으로 제거하도록 설정할 수 있습니다. 필요하면 패키지를 개별적으로 제출에서 제거할 수도 있습니다.


## <a name="gradual-package-rollout"></a>점진적 패키지 배포

제출이 이전에 게시된 앱에 대한 업데이트인 경우 **이 제출을 게시(Windows 10 고객에게만)한 후 업데이트를 단계적으로 롤아웃하세요.** 라는 확인란이 표시됩니다. 이렇게 하면 제출에서 패키지를 가져올 고객의 비율을 선택하고 피드백 및 분석 데이터를 모니터링하여 보다 광범위하게 출시하기 전에 업데이트의 품질을 확인할 수 있습니다. 새 제출을 만들 필요 없이 언제든지 비율을 늘리거나 업데이트를 중지할 수 있습니다. 

자세한 내용은 [점진적 패키지 출시](gradual-package-rollout.md)를 참조하세요.


## <a name="mandatory-update"></a>필수 업데이트

제출이 이전에 게시된 앱에 대한 업데이트인 경우 **이 업데이트를 필수로 설정하세요.** 라는 확인란이 표시됩니다. 이를 통해 필수 업데이트의 날짜 및 시간을 설정할 수 있습니다. 앱에서 프로그래밍 방식으로 패키지 업데이트를 확인하고 업데이트된 패키지를 다운로드하고 설치할 수 있도록 하는 데 Windows.Services.Store API를 사용했다고 가정합니다. 이 옵션을 사용하려면 앱에서 Windows 10 버전 1607 이상을 대상으로 해야 합니다.

자세한 내용은 [앱에 대한 패키지 업데이트 다운로드 및 설치](../packaging/self-install-package-updates.md)를 참조하세요.

 




