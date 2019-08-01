---
Description: 패키지 페이지에서는 제출 중인 앱에 대 한 모든 패키지 파일 (.appxupload, .appx, .appxbundle 및/또는 .xap)을 업로드 합니다.
title: 앱 패키지 업로드
ms.assetid: B1BB810D-3EAA-4FB5-B03C-1F01AFB2DE36
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp, 패키지, 업로드, 패키지 업로드
ms.localizationpriority: medium
ms.openlocfilehash: 570ccc1329fd1b2f768ca528b75fe22b982bdaf6
ms.sourcegitcommit: 350d6e6ba36800df582f9715c8d21574a952aef1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682620"
---
# <a name="upload-app-packages"></a>앱 패키지 업로드

**패키지** 페이지에서는 제출 중인 앱에 대 한 모든 패키지 파일 (. msix,. msixupload,. msixupload, .appx, .appxupload 및/또는 .appxbundle)을 업로드 합니다. 이 페이지에서 동일한 앱에 대 한 모든 패키지를 업로드할 수 있으며, 고객이 앱을 다운로드 하면 스토어는 각 고객에 게 장치에 가장 적합 한 패키지를 자동으로 제공 합니다. 패키지를 업로드하면 [특정 Windows 10 디바이스 패밀리(및 해당하는 경우 이전 OS 버전)에 제공될 패키지](#device-family-availability)를 등급순으로 나타내는 표가 표시됩니다.

> [!IMPORTANT]
> 2018 년 10 월 31 일까 지 새로 만든 제품은 Windows 8.x/Windows Phone 8.x이 하 버전을 대상으로 하는 패키지를 포함할 수 없습니다. 자세한 내용은이 [블로그 게시물](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store)을 참조 하세요.

패키지 내용 및 패키지 구성 방법에 대한 자세한 내용은 [앱 패키지 요구 사항](app-package-requirements.md)을 참조하세요. 또한 [특정 고객에 게 제공 되는 패키지](package-version-numbering.md) 에 대 한 버전 번호의 영향 및 다양 한 [시나리오에 대 한 패키지를 관리](guidance-for-app-package-management.md)하는 방법에 대해 알아볼 수 있습니다.


## <a name="uploading-packages-to-your-submission"></a>제출에 패키지 업로드

패키지를 업로드하려면 패키지를 업로드 필드로 끌거나 파일을 클릭하여 찾아봅니다. **패키지** 페이지에서는. msix,. msixupload,. msixupload, .appx, .appxupload 및/또는 .appxbundle 파일을 업로드할 수 있습니다.

> [!IMPORTANT]
> Windows 10의 경우. m 6, .appx,. msixupload 또는 .appxbundle 대신. msixupload 또는 .appxupload 파일을 업로드 하는 것이 좋습니다.  스토어에서의 UWP 앱 패키징에 대한 자세한 내용은 [Visual Studio를 사용하여 UWP 앱 패키징](/windows/msix/package/packaging-uwp-apps)을 참조하세요.

앱에 대한 [패키지 플라이트](package-flights.md)를 만든 경우 패키지 플라이트 중 하나에서 패키지를 복사하는 옵션이 포함된 드롭다운이 표시됩니다. 끌어오려는 패키지가 있는 패키지 플라이트를 선택합니다. 그런 다음 해당 패키지를 일부 또는 전부 선택하여 이 제출에 포함할 수 있습니다.

유효성을 검사 하는 동안 패키지를 사용 하 여 오류를 감지 하면 무엇이 잘못 되었는지 알려 주는 메시지가 표시 됩니다. 패키지를 제거 하 고 문제를 해결 한 다음 다시 업로드 해 보십시오. 문제를 일으킬 수 있지만 제출을 계속하지 못하게 차단하지는 않는 정보를 알리는 경고가 표시될 수도 있습니다.


## <a name="device-family-availability"></a>디바이스 패밀리 가용성

패키지가 성공적으로 업로드되면 **디바이스 패밀리 가용성** 섹션에 특정 Windows 10 디바이스 패밀리(및 해당하는 경우 이전 OS 버전)에 제공될 패키지를 등급순으로 나타내는 표가 표시됩니다. 이 섹션에서는 특정 Windows 10 장치 패밀리의 고객에게 제출을 제공할 것인지도 선택할 수 있습니다.

자세한 내용은 [디바이스 패밀리 가용성](device-family-availability.md)을 참조하세요.


## <a name="package-details"></a>패키지 세부 정보

업로드 된 패키지는 대상 운영 체제별로 그룹화 되어 나열 됩니다. 패키지의 이름, 버전 및 아키텍처가 표시됩니다. 각 패키지의 지원되는 언어, 앱 접근 권한 값 및 파일 크기와 같은 추가 정보를 표시하려면 **세부 정보 표시**를 클릭합니다.

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

 




