---
description: A/B 테스트를 사용 하 여 UWP (유니버설 Windows 플랫폼) 앱에서 실험을 실행 하려면 먼저 파트너 센터에서 실험을 정의 해야 합니다.
title: 파트너 센터에서 실험 정의
ms.assetid: 675F2ADE-0D4B-41EB-AA4E-56B9C8F32C41
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store Services SDK, A/B 테스트, 실험
ms.localizationpriority: medium
ms.openlocfilehash: 909fa6b36f0b6dc51a3bb6eac117abae3b8b73ae
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033216"
---
# <a name="define-your-experiment-in-partner-center"></a>파트너 센터에서 실험 정의

[프로젝트를 만들고 파트너 센터에서 원격 변수를 정의](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md) 하 고 [실험을 위해 앱을 코딩](code-your-experiment-in-your-app.md)한 후에는 프로젝트에서 실험을 만들 준비가 된 것입니다. 실험을 만들 때 사용자가 받게 되는 목표와 변형을 정의 합니다.

실험을 만들고 실행 하는 종단 간 프로세스를 보여 주는 연습은 [a/B 테스트를 사용 하 여 첫 번째 실험 만들기 및 실행](create-and-run-your-first-experiment-with-a-b-testing.md)을 참조 하세요.

<span id="get-an-api-key" />
<span id="create-an-experiment" />

## <a name="create-your-experiment"></a>실험 만들기

1. [파트너 센터](https://partner.microsoft.com/dashboard)에 로그인합니다.
2. **앱** 아래에서 실험을 만들려는 앱을 선택 합니다.
3. 탐색 창에서 **서비스** 를 선택 하 고 **실험** 을 선택 합니다.
4. **실험** 페이지에서 프로젝트 테이블의 실험을 추가 하려는 프로젝트를 확인 하 고 해당 프로젝트에 대 한 **실험 추가** 링크를 클릭 합니다.
5. 실험 **이름** 필드에 실험을 쉽게 식별 하는 데 사용할 수 있는 이름을 입력 합니다. 실험을 만든 후이 이름은 앱 및 프로젝트 페이지의 **실험** 페이지에서 기존 실험의 목록에 표시 됩니다.
6. 실험을 활성화 하는 동안 편집 하려면 편집 **가능한 실험** 확인란을 클릭 합니다. 내부 테스트를 통해 모든 변형에 대해 유효성을 검사 하는 실험을 만드는 경우에만이 확인란을 선택 합니다. 자세한 내용은 [내부 테스트를 위한 실험 만들기](define-your-experiment-in-the-dev-center-dashboard.md#test_experiments)를 참조 하세요.
    > [!NOTE]
    > 고객에 게 릴리스할 실험 (즉, 고객에 게 제공 되는 앱의 버전에서 사용 되는 프로젝트 ID와 연결 된 실험)을 만드는 경우에는이 확인란을 선택 하지 마십시오. 실험을 활성 상태에서 편집 하면 실험 결과가 무효화 됩니다.

7. **프로젝트 이름** 드롭다운에서 현재 프로젝트가 자동으로 선택 됩니다. 다른 프로젝트에 새 실험을 추가 하려는 경우 여기에서 해당 프로젝트를 선택할 수 있습니다. 그렇지 않으면이 선택만 그대로 둡니다.
8.   [프로젝트 ID](run-app-experiments-with-a-b-testing.md#terms) 값을 기록해 둡니다. 실험을 [위해 앱을 코딩](code-your-experiment-in-your-app.md)하는 경우에는 코드에서이 ID를 참조 해야 합니다. 그러면 데이터와 보고서 뷰 및 변환 이벤트를 파트너 센터로 수신할 수 있습니다.
9. **이벤트 보기** 섹션의 **이벤트 이름 보기** 필드에 실험의 [뷰 이벤트](run-app-experiments-with-a-b-testing.md#terms) 이름을 입력 합니다.
10. **목표 및 변환 이벤트** 섹션에서 실험에 대 한 목표를 하나 이상 정의 합니다.
  * **목표 이름** 필드에 목표에 대 한 설명이 포함 된 이름을 입력 합니다. 실험을 실행 한 후이 이름이 실험의 결과 요약에 나타납니다.
  * **변환 이벤트 이름** 필드에이 목표에 대 한 [변환 이벤트](run-app-experiments-with-a-b-testing.md#terms) 의 이름을 입력 합니다.
  * 변환 이벤트의 발생 빈도를 최대화 하거나 최소화할 지 여부에 따라 **목표** 필드에서 **최대화** 또는 **최소화** 를 선택 합니다. 이 정보는 실험의 결과 요약에 사용 됩니다.

> [!NOTE]
> 파트너 센터는 24 시간 동안 각 사용자 보기에 대 한 첫 번째 변환 이벤트만 보고 합니다. 사용자가 24 시간 이내에 앱에서 여러 변환 이벤트를 트리거하는 경우 첫 번째 변환 이벤트만 보고 됩니다. 이는 변환을 수행 하는 사용자 수를 최대화 하는 것이 목표 일 때 단일 사용자가 샘플 사용자 그룹에 대 한 실험 결과를 기울여 못하도록 방지 하기 위한 것입니다.

<span id="define-the-variations-and-settings-for-the-experiment" />

### <a name="define-the-remote-variables-and-variations-for-your-experiment"></a>실험에 대 한 원격 변수 및 변형 정의

그런 다음 실험의 원격 [변수](run-app-experiments-with-a-b-testing.md#terms) 및 [변형을](run-app-experiments-with-a-b-testing.md#terms) 정의 합니다.

1. **원격 변수 및 변형** 섹션에서 **변형 A (컨트롤)** 와 **변형 B** 의 두 가지 기본 변형이 표시 되어야 합니다. 더 많은 변형이 필요한 경우 **변형 추가** 를 클릭 합니다. 필요에 따라 각 변형의 이름을 바꿀 수 있습니다.
2. 기본적으로 변형은 앱 사용자에 게 동일 하 게 배포 됩니다. 특정 분포 비율을 선택 하려면 **균등** 하 게 분포 확인란의 선택을 취소 하 고 **분포 (%)** 행에 백분율을 입력 합니다.
3. 변형에 원격 변수를 추가 합니다. 이 섹션의 맨 아래에 있는 드롭다운 컨트롤에서 추가 하려는 각 변수를 선택 하 고 **변수 추가** 를 클릭 합니다.
    > [!NOTE]
    > 이 컨트롤에 나열 된 변수는 실험을 위해 프로젝트에서 상속 됩니다. 프로젝트에 정의 된 변수의 기본값은 컨트롤 변형에 자동으로 할당 됩니다. 여기에 나열 되지 않은 새 변수를 만들려는 경우 관련 프로젝트 페이지로 이동 하 여 여기에 변수를 추가 합니다.

4. 실험의 각 고유 변형에 대 한 변수 값 (즉, 컨트롤 변형 이외의 변형)을 편집 합니다.

<span id="save-and-activate-your-experiment" />

### <a name="save-and-activate-your-experiment"></a>실험 저장 및 활성화

실험에 필요한 필드를 입력 했으면 **저장** 을 클릭 하 여 실험을 저장 합니다.

실험의 매개 변수를 만족 하 고 응용 프로그램에서 실험 데이터 수집을 시작할 수 있도록 해당 매개 변수를 활성화할 준비가 되 면 **활성화** 를 클릭 합니다. 실험이 활성화 되 면 앱이 변형 변수와 보고서 뷰 및 변환 이벤트를 파트너 센터에 검색할 수 있습니다. 자세한 내용은 [파트너 센터에서 실험 실행 및 관리](manage-your-experiment.md)를 참조 하세요.

> [!IMPORTANT]
> 프로젝트에는 한 번에 하나의 활성 실험만 포함할 수 있습니다. 실험을 활성화 한 후에는 실험을 만들 때 **편집 가능한 실험** 확인란을 선택 하지 않은 경우 실험 매개 변수를 더 이상 수정할 수 없습니다. 실험을 활성화 하기 전에 앱의 실험을 코딩 하는 것이 좋습니다.

<span id="test_experiments"/>

## <a name="create-an-experiment-for-internal-testing"></a>내부 테스트용 실험 만들기

제어 된 대상 그룹 (예: 내부 테스터 집합)으로 실험을 테스트 하 고 고객에 대 한 실험을 활성화 하기 전에 모든 변형이 예상 대로 작동 하는지 확인 하는 것이 좋습니다. **편집 가능한 실험** 옵션을 선택한 실험을 만들어이를 수행할 수 있습니다.

실험을 고객에 게 릴리스하기 전에 테스트 하려면 다음 프로세스를 따르세요.

1. 두 개의 프로젝트를 만듭니다. 하나는 응용 프로그램의 공용 빌드를 위한 것이 고 다른 하나는 테스트 대상 사용자만 사용할 수 있는 앱의 전용 빌드를 위한 것입니다. 다음 지침에서는 이러한 프로젝트를 각각 공용 프로젝트 및 테스트 프로젝트로 참조 합니다.
2. 실험을 [위해 앱을 코딩](code-your-experiment-in-your-app.md)하는 경우 응용 프로그램의 공용 빌드에서 공용 프로젝트의 프로젝트 ID를 참조 합니다. 앱의 개인 빌드에서 테스트 프로젝트의 프로젝트 ID를 참조 합니다.
3. 테스트 프로젝트에서 실험을 만들고 실험에 대해 편집할 수 있는 **실험** 옵션을 선택 합니다.
4. 테스트 프로젝트에서 실험을 활성화 합니다. 100% 분포를 하나의 변형에 할당 하 고이 변형이 테스터에 대해 예상 대로 작동 하는지 확인 합니다. 다른 변형에 대해이 프로세스를 반복 합니다.
5. 변형이 예상 대로 작동 하는지 확인 한 후 테스트 프로젝트에서 실험에 대 한 최종 변경을 수행 합니다. 실험을 고객에 게 릴리스할 준비가 되 면 실험을 공용 프로젝트에 복제 합니다. 이 실험에서는 **편집 가능한 실험** 옵션을 선택 하지 마십시오.
4. 복제 된 실험에서 대상 변형 배포가 올바른지 확인 합니다.
5. 복제 된 실험을 활성화 하 여 고객에 게 실험을 릴리스 합니다.

## <a name="next-steps"></a>다음 단계

파트너 센터에서 실험을 정의 하 고 앱에서 실험을 코딩 하면 [파트너 센터에서 실험을 실행 하 고 관리할](manage-your-experiment.md)준비가 된 것입니다.

## <a name="related-topics"></a>관련 항목

* [파트너 센터에서 프로젝트 만들기 및 원격 변수 정의](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [실험용 앱 코딩](code-your-experiment-in-your-app.md)
* [파트너 센터에서 실험 관리](manage-your-experiment.md)
* [A/B 테스트를 사용 하 여 첫 번째 실험 만들기 및 실행](create-and-run-your-first-experiment-with-a-b-testing.md)
* [A/B 테스트로 앱 실험 실행](run-app-experiments-with-a-b-testing.md)
