---
author: mcleanbyron
Description: After you define your experiment in the Dev Center dashboard and code your experiment in your app, you are ready to active your experiment and use the Dev Center dashboard to review the results of your experiment.
title: 대시보드에서 실험 관리
ms.assetid: D48EE0B4-47F2-455C-8FB9-630769AC5ACE
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store Services SDK, A/B 테스트, 실험
ms.localizationpriority: medium
ms.openlocfilehash: 073d5ec67bb012cbfe21c279ee97ec3c50b8458f
ms.sourcegitcommit: 7efffcc715a4be26f0cf7f7e249653d8c356319b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/30/2018
ms.locfileid: "3117550"
---
# <a name="manage-your-experiment-in-the-dashboard"></a>대시보드에서 실험 관리

[개발자 센터 대시보드에서 실험을 정의](define-your-experiment-in-the-dev-center-dashboard.md)하고 [실험에 대해 앱을 코딩](code-your-experiment-in-your-app.md)하면 실험을 활성화할 준비가 된 것이며 개발자 센터 대시보드를 사용하여 실험 결과를 검토합니다. 필요한 데이터를 모두 얻은 후 실험을 종료하고 모든 앱에서 컨트롤 변형의 변수 값을 계속 사용할지, 아니면 다른 변형 중 하나의 변수 값을 사용하도록 전환할지를 선택할 수 있습니다.

> [!NOTE]
> 실험을 활성화하면 개발자 센터에서 실험에 대한 데이터를 기록하기 위해 계측되는 모든 앱에서 즉시 데이터를 수집하기 시작합니다. 그러나 실험 데이터를 대시보드에 표시하려면 몇 시간이 걸릴 수 있습니다.

실험 만들기 및 실행의 종단 간 프로세스를 보여 주는 연습에 대한 자세한 내용은 [A/B 테스트로 첫 번째 실험 만들기 및 실행](create-and-run-your-first-experiment-with-a-b-testing.md)을 참조하세요.

## <a name="activate-your-experiment"></a>실험 활성화

대시보드의 실험 매개 변수에 만족하고 앱 코드를 업데이트한 경우 실험을 활성화할 준비가 된 것이며 앱에서 실험 데이터 수집을 시작할 수 있습니다. 실험이 활성화된 경우 앱은 변형 변수를 검색하고 개발자 센터에 보기 및 전환 이벤트를 보고할 수 있습니다.

1. [개발자 센터 대시보드](https://dev.windows.com/overview)에 로그인합니다.
2. **앱**에서 활성화하려는 실험이 있는 앱을 선택합니다.
3. 탐색 창에서 **서비스**를 선택한 다음 **실험**을 선택합니다.
4. **프로젝트** 섹션의 프로젝트 표에서 해당 실험이 포함된 프로젝트를 확장하고 다음 중 하나를 수행합니다.
  * 해당 실험에 대한 **활성화** 링크를 클릭합니다. 해당 실험이 페이지 위쪽의 **활성화된 실험** 섹션에 추가됩니다.
  * 실험 이름을 클릭하고 실험 페이지 하단으로 스크롤하여 **활성화**를 클릭합니다.

> [!IMPORTANT]
> 실험을 만들 때 **편집 가능한 실험** 확인란을 클릭하지 않은 경우 실험을 활성화한 후에는 더 이상 실험 매개 변수를 수정할 수 없습니다. 실험을 활성화하기 전 앱에서 실험을 코딩하는 것이 좋습니다.

## <a name="review-the-results-of-your-experiment"></a>실험 결과 검토

1. 개발자 센터에서 앱의 **실험** 페이지로 돌아갑니다.
2. **활성 실험** 섹션에서 활성 실험 이름을 클릭하여 실험 페이지로 이동합니다.
3. 활성 또는 완료된 실험의 경우 이 페이지의 처음 두 섹션에서 실험 결과를 제공합니다.
  * **결과 요약** 섹션에는 실험 목표 및 각 변형의 전환율(%)이 나열됩니다.
  * **결과 정보** 섹션에서는 보기, 전환, 고유 사용자, 전환율, 델타 %, 신뢰도 및 중요도를 포함하여 모든 실험 목표의 각 변형에 대한 세부 정보를 제공합니다. *신뢰도*는 오차 범위를 계산하는 예측 안정성의 통계 측정값입니다. *중요도*는 결과가 운 때문이 아니라 특정 원인에서 발생할 가능성을 확인하기 위한 샘플 크기 기반의 통계 측정값입니다.

> [!NOTE]
> 개발자 센터에서는 24시간 내의 각 사용자에 대한 첫 번째 전환 이벤트만 보고합니다. 사용자가 24시간 내에 앱에서 여러 전환 이벤트를 트리거하는 경우 첫 번째 전환 이벤트만 보고됩니다. 이렇게 하면 많은 전환 이벤트를 트리거하는 단일 사용자가 샘플 그룹 사용자의 실험 결과를 왜곡시키지 않을 수 있습니다.


## <a name="complete-your-experiment"></a>실험 완료

1. 대시보드에서 실험 페이지로 돌아갑니다. 자세한 내용은 이전 섹션을 참조하세요.
2. **결과 요약** 섹션에서 다음 중 하나를 수행합니다.
  * 실험을 종료하고 앱에서 컨트롤의 변형 변수 값을 계속 사용하려는 경우 **유지**를 클릭합니다.
  * 실험을 종료하지만 앱에서 다른 변형의 변수 값 사용으로 전환하려는 경우 전환할 변형 아래에 있는 **전환**을 클릭합니다.
3. **확인**을 클릭하여 실험을 종료할 것인지 확인합니다.


## <a name="related-topics"></a>관련 항목

* [개발자 센터 대시보드에서 프로젝트 만들기 및 원격 변수 정의](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [실험용 앱 코딩](code-your-experiment-in-your-app.md)
* [개발자 센터 대시보드에서 실험 정의](define-your-experiment-in-the-dev-center-dashboard.md)
* [A/B 테스트로 첫 번째 실험 만들기 및 실행](create-and-run-your-first-experiment-with-a-b-testing.md)
* [A/B 테스트로 앱 실험 실행](run-app-experiments-with-a-b-testing.md)