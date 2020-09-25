---
Description: 패키지 페이지에서는 제출 중인 앱에 대 한 모든 패키지 파일 (.appxupload, .appx, .appxbundle 및/또는 .xap)을 업로드 합니다.
title: 앱 패키지 업로드
ms.assetid: B1BB810D-3EAA-4FB5-B03C-1F01AFB2DE36
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp, 패키지, 업로드, 패키지 업로드
ms.localizationpriority: medium
ms.openlocfilehash: 0b4fc0c9dfeed1183a1653b525d0f8cc8a62a4c1
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220216"
---
# <a name="upload-app-packages"></a>앱 패키지 업로드

**패키지** 페이지에서는 제출 중인 앱에 대 한 모든 패키지 파일 (. msix,. msixupload,. msixupload, .appx, .appxupload 및/또는 .appxbundle)을 업로드 합니다. 이 페이지에서 동일한 앱에 대 한 모든 패키지를 업로드할 수 있으며, 고객이 앱을 다운로드 하면 스토어는 각 고객에 게 장치에 가장 적합 한 패키지를 자동으로 제공 합니다. 패키지를 업로드 하면 [특정 Windows 10 장치 제품군](#device-family-availability) (해당 하는 경우 이전 OS 버전, 해당 하는 경우)에 제공 되는 패키지를 순위에 따라 나타내는 표가 표시 됩니다.

> [!IMPORTANT]
> Windows Phone .x SDK를 사용 하 여 빌드된 새 XAP 패키지는 더 이상 업로드할 수 없습니다. 이미 XAP 패키지를 사용 하 여 저장 된 앱은 Windows 10 Mobile 장치에서 계속 작동 합니다. 자세한 내용은이 [블로그 게시물](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store)을 참조 하세요.

패키지에 포함 되는 내용 및 구성 방법에 대 한 자세한 내용은 [앱 패키지 요구 사항](app-package-requirements.md)을 참조 하세요. 또한 [특정 고객에 게 제공 되는 패키지](package-version-numbering.md) 에 대 한 버전 번호의 영향 및 다양 한 [시나리오에 대 한 패키지를 관리](guidance-for-app-package-management.md)하는 방법에 대해 알아볼 수 있습니다.


## <a name="uploading-packages-to-your-submission"></a>패키지를 제출에 업로드 하는 중

패키지를 업로드 하려면 업로드 필드로 끌거나를 클릭 하 여 파일을 찾아봅니다. **패키지** 페이지에서는. msix,. msixupload,. msixupload, .appx, .appxupload 및/또는 .appxbundle 파일을 업로드할 수 있습니다.

> [!IMPORTANT]
> Windows 10의 경우. m 6, .appx,. msixupload 또는 .appxbundle 대신. msixupload 또는 .appxupload 파일을 업로드 하는 것이 좋습니다.  스토어 용 UWP 앱을 패키징하는 방법에 대 한 자세한 내용은 [Visual Studio를 사용 하 여 uwp 앱 패키징](/windows/msix/package/packaging-uwp-apps)을 참조 하세요.

앱에 대 한 [패키지를 항공편](package-flights.md) 으로 만든 경우 패키지 중 하나에서 패키지를 복사 하는 옵션이 있는 드롭다운이 표시 됩니다. 끌어올 패키지가 있는 패키지 비행을 선택 합니다. 그런 다음이 제출에 포함할 패키지의 일부 또는 전부를 선택할 수 있습니다.

유효성을 검사 하는 동안 패키지를 사용 하 여 오류를 감지 하면 무엇이 잘못 되었는지 알려 주는 메시지가 표시 됩니다. 패키지를 제거 하 고 문제를 해결 한 다음 다시 업로드 해 보십시오. 문제를 발생 시킬 수 있는 문제에 대 한 정보를 알려 주는 경고도 표시 될 수 있습니다.


## <a name="device-family-availability"></a>디바이스 패밀리 가용성

패키지를 성공적으로 업로드 한 후에는 특정 Windows 10 장치 제품군 (해당 하는 경우 이전 OS 버전)에 제공 되는 패키지를 순위에 따라 표시 하는 테이블을 **장치 제품군 사용 가능** 섹션에 표시 합니다. 또한이 섹션에서는 특정 Windows 10 장치 제품군에 대 한 고객의 제출을 제공할 것인지 여부를 선택할 수 있습니다.

자세한 내용은 [디바이스 패밀리 가용성](device-family-availability.md)을 참조하세요.


## <a name="package-details"></a>패키지 세부 정보

업로드 된 패키지는 대상 운영 체제별로 그룹화 되어 나열 됩니다. 패키지의 이름, 버전 및 아키텍처가 표시 됩니다. 각 패키지에 대해 지원 되는 언어, 앱 기능 및 파일 크기와 같은 자세한 정보를 보려면 **자세한 정보 표시**를 클릭 합니다.

제출에서 패키지를 제거 해야 하는 경우 각 패키지의 **세부 정보** 섹션 아래쪽에 있는 **제거** 링크를 클릭 합니다.


## <a name="removing-redundant-packages"></a>중복 패키지 제거

하나 이상의 패키지가 중복 되는 경우이 제출에서 중복 패키지를 제거 하는 것을 제안 하는 경고를 표시 합니다. 이전에 패키지를 업로드 했을 때와 동일한 고객 집합을 지 원하는 더 높은 버전의 패키지를 제공 하는 경우가 종종 있습니다. 이 경우 고객은 이러한 고객을 지원 하기 위해 더 나은 (더 높은 버전의) 패키지를 제공 하기 때문에 중복 패키지를 가져올 수 없습니다.

중복 패키지가 있는 경우이 제출에서 중복 패키지를 모두 자동으로 제거 하는 옵션을 제공 합니다. 원하는 경우 개별적으로 제출에서 패키지를 제거할 수도 있습니다.


## <a name="gradual-package-rollout"></a>점진적 패키지 배포

이전에 게시 된 앱에 대 한 업데이트를 제출 하는 경우 **이 제출이 게시 된 후 점진적 업데이트 롤아웃 (Windows 10 고객만 해당)** 이라는 확인란이 표시 됩니다. 이를 통해 사용자 의견 및 분석 데이터를 모니터링 하 여 업데이트에 대 한 정보를 파악 하 여 보다 광범위 하 게 롤링 하기 전에 사용자가 제출에서 패키지를 가져올 고객의 비율을 선택할 수 있습니다. 새 제출을 만들지 않고도 언제 든 지 백분율을 늘리거나 업데이트를 중단할 수 있습니다. 

자세한 내용은 [점진적 패키지 출시](gradual-package-rollout.md)를 참조 하세요.


## <a name="mandatory-update"></a>필수 업데이트

이전에 게시 된 앱에 대 한 업데이트를 제출 하는 경우 **이 업데이트를 필수로 설정**하 라는 확인란이 표시 됩니다. 이를 통해 응용 프로그램에서 패키지 업데이트를 프로그래밍 방식으로 확인 하 고 업데이트 된 패키지를 다운로드 및 설치할 수 있도록 하는 경우 필수 업데이트에 대 한 날짜 및 시간을 설정할 수 있습니다. 이 옵션을 사용 하려면 앱이 Windows 10 버전 1607 이상을 대상으로 해야 합니다.

자세한 내용은 [앱에 대 한 패키지 업데이트 다운로드 및 설치](../packaging/self-install-package-updates.md)를 참조 하세요.

 




