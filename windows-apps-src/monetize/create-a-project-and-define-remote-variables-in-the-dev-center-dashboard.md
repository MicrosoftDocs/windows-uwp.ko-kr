---
Description: 사용 하 여 유니버설 Windows 플랫폼 (UWP) 앱에서 실험을 실행 하기 전에 / B 테스트, 프로젝트를 만들고 파트너 센터에서 원격 변수를 정의 해야 합니다.
title: 파트너 센터에서 실험 프로젝트 만들기
ms.assetid: C3809FF1-0A6A-4715-B989-BE9D0E8C9013
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10 uwp, Microsoft Store Services SDK A/B 테스트, 실험
ms.localizationpriority: medium
ms.openlocfilehash: acfd654f02cb7fb727d35271175e59966e2abdc4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650798"
---
# <a name="create-an-experiment-project-in-partner-center"></a>파트너 센터에서 실험 프로젝트 만들기

실험을 시작 하려면 실험을 만듭니다 [프로젝트](run-app-experiments-with-a-b-testing.md#terms) 파트너 센터에서 앱에 대 한 앱에 액세스할 수 있는 원격 변수를 정의 합니다.

다음 지침에서는 프로젝트를 만들 때 중요한 단계에 대해 설명합니다. 프로젝트를 만들고 실험을 실행하는 종단 간 프로세스를 보여 주는 연습에 대한 자세한 내용은 [A/B 테스트로 첫 번째 실험 만들기 및 실행](create-and-run-your-first-experiment-with-a-b-testing.md)을 참조하세요.

## <a name="instructions"></a>지침

1. [파트너 센터](https://partner.microsoft.com/dashboard)에 로그인합니다.
2. **앱**에서 실험을 만들 앱을 선택합니다.
3. 탐색 창에서 **서비스**를 선택한 다음 **실험**을 선택합니다.
4. **실험** 페이지에서 **프로젝트** 섹션에 있는 **새 프로젝트** 단추를 클릭합니다. 하나 이상의 프로젝트를 이미 만든 경우 **프로젝트** 섹션에 해당 프로젝트가 나열됩니다.
5. **새 프로젝트** 페이지에서 새 프로젝트의 이름을 입력합니다.
6. **원격 변수** 섹션에서 이 프로젝트의 모든 실험에 사용할 수 있는 [변수](run-app-experiments-with-a-b-testing.md#terms)를 추가하고 각 변수의 기본값을 정의합니다. 여기서 지정한 기본값은 실험의 컨트롤 그룹과 실험에 참여하지 않는 모든 사용자에 사용됩니다.
  1. **원격 변수** 섹션이 축소된 경우 섹션 제목에서 **표시**를 클릭합니다.
  2. **변수 추가**를 클릭하여 이 프로젝트의 모든 실험에 사용할 수 있게 하려는 각 변수를 새로 만들고 변수 이름과 변수 기본값을 입력합니다.
  3. 변수 추가를 마쳤으면 **저장**을 클릭합니다.
3. **SDK 통합** 섹션에서 [프로젝트 ID](run-app-experiments-with-a-b-testing.md#terms) 값을 적어둡니다. 경우 있습니다 [실험에 대 한 앱을 코딩](code-your-experiment-in-your-app.md), 변형 데이터를 수신 하 고 파트너 센터로 뷰와 변환 이벤트를 보고할 수 있도록 코드에서이 프로젝트 ID를 참조 해야 합니다.

> [!NOTE]
> 프로젝트에서 실험이 활성화된 경우에는 원격 변수를 편집, 추가 또는 제거할 수 없습니다. 이 제한 사항은 활성 실험의 컨트롤 그룹에 대해 데이터 무결성을 보호하는 데 도움이 됩니다.


## <a name="next-steps"></a>다음 단계

프로젝트를 만든 후 [실험용 앱을 코딩](code-your-experiment-in-your-app.md)하여 앱에서 원격 변수 값 검색을 시작하고 [프로젝트에서 실험을 만들](define-your-experiment-in-the-dev-center-dashboard.md) 수 있습니다.

## <a name="related-topics"></a>관련 항목

* [실험에 대 한 앱 코드](code-your-experiment-in-your-app.md)
* [파트너 센터에서 실험을 정의 합니다.](define-your-experiment-in-the-dev-center-dashboard.md)
* [파트너 센터에서 실험 관리](manage-your-experiment.md)
* [만들고 A를 사용 하 여 첫 번째 실험 실행 / B 테스트](create-and-run-your-first-experiment-with-a-b-testing.md)
* [사용 하 여 앱 실험 실행 / B 테스트](run-app-experiments-with-a-b-testing.md)
