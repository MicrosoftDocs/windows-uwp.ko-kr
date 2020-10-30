---
description: 파트너 센터에서 실험을 정의 하 고 앱에서 실험을 코딩 한 후 실험을 활성화 하 고 파트너 센터를 사용 하 여 실험의 결과를 검토할 수 있습니다.
title: 파트너 센터에서 실험 관리
ms.assetid: D48EE0B4-47F2-455C-8FB9-630769AC5ACE
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store Services SDK, A/B 테스트, 실험
ms.localizationpriority: medium
ms.openlocfilehash: dfcd9819940d21dcc81c5ac698b76381adf05af6
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030556"
---
# <a name="manage-your-experiment-in-partner-center"></a>파트너 센터에서 실험 관리

[파트너 센터에서 실험을 정의](define-your-experiment-in-the-dev-center-dashboard.md) 하 고 실험을 [위해 앱을 코딩](code-your-experiment-in-your-app.md)한 후 실험을 활성화 하 고 파트너 센터를 사용 하 여 실험 결과를 검토할 수 있습니다. 필요한 모든 데이터를 가져온 후 실험을 종료 하 고 모든 앱에 대 한 컨트롤 변형에서 변수 값을 유지할지 여부를 선택 하거나 다른 변형 중 하나에 변수 값을 사용 하도록 전환할 수 있습니다.

> [!NOTE]
> 실험을 활성화할 때 파트너 센터는 실험을 위해 로그 데이터에 계측 된 모든 앱에서 데이터 수집을 즉시 시작 합니다. 그러나 데이터를 파트너 센터에 표시 하는 데는 몇 시간이 걸릴 수 있습니다.

실험을 만들고 실행 하는 종단 간 프로세스를 보여 주는 연습은 [a/B 테스트를 사용 하 여 첫 번째 실험 만들기 및 실행](create-and-run-your-first-experiment-with-a-b-testing.md)을 참조 하세요.

## <a name="activate-your-experiment"></a>실험 활성화

파트너 센터에서 실험의 매개 변수에 만족 하 고 앱 코드를 업데이트 한 경우 앱에서 실험 데이터 수집을 시작할 수 있도록 실험을 활성화할 준비가 된 것입니다. 실험이 활성화 되 면 앱에서 변형 값과 보고서 보기 및 변환 이벤트를 파트너 센터에 검색할 수 있습니다.

1. [파트너 센터](https://partner.microsoft.com/dashboard)에 로그인합니다.
2. **앱** 에서 활성화 하려는 실험을 사용 하 여 앱을 선택 합니다.
3. 탐색 창에서 **서비스** 를 선택 하 고 **실험** 을 선택 합니다.
4. **프로젝트 섹션의** 프로젝트 표에서 실험을 포함 하는 프로젝트를 확장 하 고 다음 중 하나를 수행 합니다.
  * 실험에 대 한 **활성화** 링크를 클릭 합니다. 실험은 페이지 맨 위 근처의 **활성 실험** 섹션에 추가 됩니다.
  * 실험 이름을 클릭 하 고 실험 페이지의 아래쪽으로 스크롤한 다음 **활성화** 를 클릭 합니다.

> [!IMPORTANT]
> 실험을 활성화 한 후에는 실험을 만들 때 **편집 가능한 실험** 확인란을 클릭 하지 않은 경우 실험 매개 변수를 더 이상 수정할 수 없습니다. 실험을 활성화 하기 전에 앱의 실험을 코딩 하는 것이 좋습니다.

## <a name="review-the-results-of-your-experiment"></a>실험 결과 검토

1. 파트너 센터에서 앱에 대 한 **실험** 페이지로 돌아갑니다.
2. **활성 실험** 섹션에서 활성 실험의 이름을 클릭 하 여 실험 페이지로 이동 합니다.
3. 활성 또는 완료 된 실험의 경우이 페이지의 처음 두 섹션은 실험의 결과를 제공 합니다.
  * **결과 요약** 섹션에는 실험 목표와 각 변동에 대 한 변환율 비율이 나열 됩니다.
  * **결과 세부 정보** 섹션에서는 보기, 변환, 고유 사용자, 변환 비율, 델타%, 신뢰도 및 중요도를 포함 하 여 실험에서 모든 목표의 각 변동에 대 한 자세한 정보를 제공 합니다. *신뢰도* 는 오류의 이익률을 계산 하는 예상 안정성의 통계 측정입니다. *중요* 한 이유는 샘플 크기를 기반으로 하는 통계 측정으로 결과가 기회가 아닐 가능성을 결정 하는 것이 아니라 특정 원인에 대 한 특성을 지정 하는 것입니다.

> [!NOTE]
> 파트너 센터는 24 시간 동안 각 사용자에 대 한 첫 번째 변환 이벤트만 보고 합니다. 사용자가 24 시간 이내에 앱에서 여러 변환 이벤트를 트리거하는 경우 첫 번째 변환 이벤트만 보고 됩니다. 이는 여러 변환 이벤트를 포함 하는 단일 사용자가 샘플 사용자 그룹의 실험 결과를 기울이면 도움을 주기 위한 것입니다.


## <a name="complete-your-experiment"></a>실험 완료

1. 파트너 센터에서 실험 페이지로 돌아갑니다. 자세한 내용은 이전 섹션을 참조 하세요.
2. **결과 요약** 섹션에서 다음 중 하나를 수행 합니다.
  * 실험을 종료 하 고 앱의 컨트롤 변형에서 변수 값을 계속 사용 하려면 **유지** 를 클릭 합니다.
  * 실험을 종료 하지만 앱에서 다른 변형으로 변수 값을 사용 하도록 전환 하려면 전환 하려는 변형에서 **전환** 을 클릭 합니다.
3. **확인** 을 클릭 하 여 실험을 종료 하 고 있는지 확인 합니다.


## <a name="related-topics"></a>관련 항목

* [파트너 센터에서 프로젝트 만들기 및 원격 변수 정의](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [실험용 앱 코딩](code-your-experiment-in-your-app.md)
* [파트너 센터에서 실험 정의](define-your-experiment-in-the-dev-center-dashboard.md)
* [A/B 테스트를 사용 하 여 첫 번째 실험 만들기 및 실행](create-and-run-your-first-experiment-with-a-b-testing.md)
* [A/B 테스트로 앱 실험 실행](run-app-experiments-with-a-b-testing.md)
