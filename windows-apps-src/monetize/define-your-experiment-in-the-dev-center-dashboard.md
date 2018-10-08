---
author: mcleanbyron
Description: Before you can run an experiment in your Universal Windows Platform (UWP) app with A/B testing, you must define your experiment in the Dev Center dashboard.
title: 대시보드에서 실험 정의
ms.assetid: 675F2ADE-0D4B-41EB-AA4E-56B9C8F32C41
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, Microsoft Store Services SDK, A/B 테스트, 실험
ms.localizationpriority: medium
ms.openlocfilehash: f690aaede800f76b2eb006355e982ea6b3509b78
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4416704"
---
# <a name="define-your-experiment-in-the-dashboard"></a>대시보드에서 실험 정의

[개발자 센터 대시보드에서 프로젝트 만들기 및 원격 변수 정의](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md) 및 [실험용 앱 코딩](code-your-experiment-in-your-app.md)을 완료하면 프로젝트에서 실험을 만들 준비가 된 것입니다. 실험을 만들 때 사용자가 받을 목표와 변형을 정의합니다.

실험 만들기 및 실행의 종단 간 프로세스를 보여 주는 연습에 대한 자세한 내용은 [A/B 테스트로 첫 번째 실험 만들기 및 실행](create-and-run-your-first-experiment-with-a-b-testing.md)을 참조하세요.

<span id="get-an-api-key" />
<span id="create-an-experiment" />

## <a name="create-your-experiment"></a>실험 만들기

1. [개발자 센터 대시보드](https://dev.windows.com/overview)에 로그인합니다.
2. **앱**에서 실험을 만들 앱을 선택합니다.
3. 탐색 창에서 **서비스**를 선택한 다음 **실험**을 선택합니다.
4. **실험** 페이지에서 프로젝트 테이블에 실험을 추가할 프로젝트를 식별하고 해당 프로젝트에 대한 **실험 추가** 링크를 클릭합니다.
5. **실험 이름** 필드에서 실험을 쉽게 식별하는 데 사용할 수 있는 이름을 입력합니다. 실험을 만든 후 이 이름은 앱의 **실험** 페이지와 프로젝트 페이지의 기존 실험 목록에 표시됩니다.
6. 실험이 활성화되어 있는 동안 편집하려는 경우 **편집 가능한 실험** 확인란을 클릭합니다. 내부 테스트를 통해 모든 변형의 유효성을 검사할 실험을 만드는 경우에만 이 확인란을 선택합니다. 자세한 내용은 [내부 테스트를 위해 실험 만들기](define-your-experiment-in-the-dev-center-dashboard.md#test_experiments)를 참조하세요.
    > [!NOTE]
    > 고객에게 릴리스할 실험(즉, 고객에게 사용할 수 있는 앱 버전에 사용되는 프로젝트 ID와 연결된 실험)을 만들 경우 이 확인란을 선택하지 마세요. 활성 상태일 때 실험을 편집하면 실험 결과가 무효화됩니다.

7. **프로젝트 이름** 드롭다운에서 현재 프로젝트가 자동으로 선택됩니다. 다른 프로젝트에 새 실험을 추가하려는 경우 여기서 해당 프로젝트를 선택할 수 있습니다. 그렇지 않으면 이 선택을 그대로 둡니다.
8.   [프로젝트 ID](run-app-experiments-with-a-b-testing.md#terms) 값을 적어둡니다. [실험용 앱을 코딩](code-your-experiment-in-your-app.md)하는 경우 변형 데이터를 받고 보기 및 변환 이벤트를 개발자 센터에 보고할 수 있도록 코드에서 이 ID를 참조해야 합니다.
9. **보기 이벤트** 섹션에서 **보기 이벤트 이름** 필드에 실험에 대한 [보기 이벤트](run-app-experiments-with-a-b-testing.md#terms)의 이름을 입력합니다.
10. **목표와 전환 이벤트** 섹션에서 하나 이상의 실험 목표를 정의합니다.
  * **목표 이름** 필드에서 목표의 설명이 포함된 이름을 입력합니다. 실험을 실행한 후 이 이름은 실험에 대한 결과 요약에 표시됩니다.
  * **전환 이벤트 이름** 필드에서 이 목표에 대한 [전환 이벤트](run-app-experiments-with-a-b-testing.md#terms)의 이름을 입력합니다.
  * **목표** 필드에서 전환 이벤트의 발생 횟수를 최대화 또는 최소화할지 여부에 따라 **최대화** 또는 **최소화**를 선택합니다. 이 정보는 실험 결과 요약에 사용됩니다.

> [!NOTE]
> 개발자 센터에서는 24시간 내의 각 사용자 보기에 대한 첫 번째 전환 이벤트만 보고합니다. 사용자가 24시간 내에 앱에서 여러 전환 이벤트를 트리거하는 경우 첫 번째 전환 이벤트만 보고됩니다. 이렇게 하면 목표가 전환을 수행하는 사용자의 수를 최대화하는 것일 때 단일 사용자가 샘플 그룹 사용자의 실험 결과를 왜곡시키지 않을 수 있습니다.

<span id="define-the-variations-and-settings-for-the-experiment" />

### <a name="define-the-remote-variables-and-variations-for-your-experiment"></a>실험에 대한 원격 변수 및 변형 정의

다음으로 실험에 대한 원격 [변수](run-app-experiments-with-a-b-testing.md#terms) 및 [변형](run-app-experiments-with-a-b-testing.md#terms)을 정의합니다.

1. **변수 및 변형 제거** 섹션에는 두 가지 기본 변형인 **변형 A(컨트롤)** 및 **변형 B**가 표시됩니다. 더 많은 변형을 사용하려면 **변형 추가**를 클릭합니다. 선택적으로 각 변형의 이름을 바꿀 수 있습니다.
2. 기본적으로 변형은 앱 사용자에게 고르게 배포됩니다. 특정 배포 비율을 선택하려는 경우 **고르게 분배** 확인란을 선택 취소하고 **분포(%)** 행에 백분율을 입력합니다.
3. 변형에 원격 변수를 추가합니다. 이 섹션의 맨 아래 드롭다운 컨트롤에서 추가할 각 변수를 선택하고 **변수 추가**를 클릭합니다.
    > [!NOTE]
    > 이 컨트롤에 나열된 변수는 실험 프로젝트에서 상속됩니다. 프로젝트에 정의된 대로 변수의 기본값은 자동으로 컨트롤 변형에 할당됩니다. 여기에 나열되지 않은 새 변수를 만들려면 관련 프로젝트 페이지로 이동하여 변수를 추가합니다.

4. 실험의 고유한 각 변형(즉, 컨트롤 변형 이외의 변형)에 대한 변수 값을 편집합니다.

<span id="save-and-activate-your-experiment" />

### <a name="save-and-activate-your-experiment"></a>실험을 저장하고 활성화합니다.

실험에 대한 필수 필드 입력을 마치면 **저장**을 클릭하여 실험을 저장합니다.

원하는 실험 매개 변수를 설정하고 활성화할 준비가 완료되어 앱에서 실험 데이터 수집을 시작할 수 있으면 **활성화**를 클릭합니다. 실험이 활성화된 경우 앱은 변형 변수를 검색하고 개발자 센터에 보기 및 전환 이벤트를 보고할 수 있습니다. 자세한 내용은 [개발자 센터 대시보드에서 실험 실행 및 관리](manage-your-experiment.md)를 참조하세요.

> [!IMPORTANT]
> 프로젝트는 한 번에 하나의 활성 실험만 포함할 수 있습니다. 실험을 만들 때 **편집 가능한 실험** 확인란을 선택하지 않은 경우 실험을 활성화한 후에는 더 이상 실험 매개 변수를 수정할 수 없습니다. 실험을 활성화하기 전 앱에서 실험을 코딩하는 것이 좋습니다.

<span id="test_experiments"/>

## <a name="create-an-experiment-for-internal-testing"></a>내부 테스트용 실험 만들기

제어 대상 그룹(예: 내부 테스터 집합)으로 실험을 테스트하고 고객을 위해 실험을 활성화하기 전에 모든 변형이 예상대로 작동하는지 확인하는 것이 좋습니다. **편집 가능한 실험** 옵션이 선택된 실험을 만들어 이 작업을 수행할 수 있습니다.

고객에게 릴리스하기 전에 실험을 테스트하려면 다음 프로세스를 수행합니다.

1. 두 개의 프로젝트(앱의 공용 빌드용 및 테스트 대상 그룹에만 사용할 수 있는 앱의 개인 빌드용)를 만듭니다. 다음 지침에서 이 두 개의 프로젝트를 공개 프로젝트 및 테스트 프로젝트로 각각 참조합니다.
2. [실험용 앱을 코딩](code-your-experiment-in-your-app.md)하는 경우 앱의 공용 빌드에서 공용 프로젝트의 프로젝트 ID를 참조합니다. 앱의 개인 빌드에서 테스트 프로젝트의 프로젝트 ID를 참조합니다.
3. 테스트 프로젝트에서 실험을 만들고 이 실험에 대해 **편집 가능한 실험** 옵션을 선택합니다.
4. 테스트 프로젝트에서 실험을 활성화합니다. 하나의 변형에 100% 배포를 할당하고 이 변형이 테스터에 예상대로 작동하는지 확인합니다. 다른 변형에 대한 프로세스를 반복합니다.
5. 변형이 예상대로 작동하는지 확인한 후 테스트 프로젝트에서 실험을 최종 변경합니다. 고객에게 실험을 릴리스할 준비가 되면 공용 프로젝트에 실험을 복제합니다. 이 실험에서는 **편집 가능한 실험** 옵션을 선택하지 않습니다.
4. 복제 실험에서 대상 변형 배포가 올바른지 확인합니다.
5. 복제 실험을 활성화하여 고객에게 실험을 릴리스합니다.

## <a name="next-steps"></a>다음 단계

개발자 센터 대시보드에서 실험을 정의하고 앱에 실험을 코딩하면 [개발자 센터 대시보드에서 실험을 실행하고 관리](manage-your-experiment.md)할 준비가 된 것입니다.

## <a name="related-topics"></a>관련 항목

* [개발자 센터 대시보드에서 프로젝트 만들기 및 원격 변수 정의](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [실험용 앱 코딩](code-your-experiment-in-your-app.md)
* [개발자 센터 대시보드에서 실험 관리](manage-your-experiment.md)
* [A/B 테스트로 첫 번째 실험 만들기 및 실행](create-and-run-your-first-experiment-with-a-b-testing.md)
* [A/B 테스트로 앱 실험 실행](run-app-experiments-with-a-b-testing.md)
