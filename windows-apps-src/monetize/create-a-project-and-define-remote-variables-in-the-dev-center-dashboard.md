---
description: A/B 테스트를 사용 하 여 UWP (유니버설 Windows 플랫폼) 앱에서 실험을 실행 하려면 먼저 프로젝트를 만들고 파트너 센터에서 원격 변수를 정의 해야 합니다.
title: 파트너 센터에서 실험 프로젝트 만들기
ms.assetid: C3809FF1-0A6A-4715-B989-BE9D0E8C9013
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store Services SDK, A/B 테스트, 실험
ms.localizationpriority: medium
ms.openlocfilehash: 19eccb6ebc7556fb3dc2c6c11969361e19f802f3
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032766"
---
# <a name="create-an-experiment-project-in-partner-center"></a>파트너 센터에서 실험 프로젝트 만들기

실험을 시작 하려면 파트너 센터에서 앱에 대 한 실험 [프로젝트](run-app-experiments-with-a-b-testing.md#terms) 를 만들고 앱에서 액세스할 수 있는 원격 변수를 정의 합니다.

다음 지침에서는 프로젝트를 만드는 핵심 단계에 대해 설명 합니다. 프로젝트를 만든 다음 실험을 실행 하는 종단 간 프로세스를 보여 주는 자세한 연습은 [a/B 테스트를 사용 하 여 첫 번째 실험 만들기 및 실행](create-and-run-your-first-experiment-with-a-b-testing.md)을 참조 하세요.

## <a name="instructions"></a>Instructions

1. [파트너 센터](https://partner.microsoft.com/dashboard)에 로그인합니다.
2. **앱** 아래에서 실험을 만들려는 앱을 선택 합니다.
3. 탐색 창에서 **서비스** 를 선택 하 고 **실험** 을 선택 합니다.
4. **실험** 페이지의 **프로젝트** 섹션에서 **새 프로젝트** 단추를 클릭 합니다. 이미 하나 이상의 프로젝트를 만든 경우 해당 프로젝트는 **프로젝트** 섹션에 나열 됩니다.
5. **새 프로젝트** 페이지에서 새 프로젝트의 이름을 입력 합니다.
6. **원격 변수** 섹션에서이 프로젝트의 모든 실험에 사용할 수 있도록 하려는 [변수](run-app-experiments-with-a-b-testing.md#terms) 를 추가 하 고 각 변수에 대 한 기본값을 정의 합니다. 여기에서 지정 하는 기본값은 실험의 컨트롤 그룹과 실험에 참여 하지 않는 사용자에 대해 사용 됩니다.
  1. **원격 변수** 섹션이 축소 된 경우 섹션 제목에서 **표시** 를 클릭 합니다.
  2. **변수 추가** 를 클릭 하 여이 프로젝트의 모든 실험에 사용할 수 있도록 할 새 변수를 각각 만들고 변수 이름과 변수의 기본값을 입력 합니다.
  3. 변수 추가가 완료 되 면 **저장** 을 클릭 합니다.
3. **SDK 통합** 섹션에서 [프로젝트 ID](run-app-experiments-with-a-b-testing.md#terms) 값을 기록해 둡니다. 실험을 [위해 앱을 코딩](code-your-experiment-in-your-app.md)하는 경우에는 코드에서이 프로젝트 ID를 참조 하 여 데이터 및 보고서 뷰 및 변환 이벤트를 파트너 센터에 받을 수 있도록 해야 합니다.

> [!NOTE]
> 프로젝트의 실험을 활성화 하는 동안에는 원격 변수를 편집, 추가 또는 제거할 수 없습니다. 이러한 제한 사항은 활성 실험의 컨트롤 그룹에 대 한 데이터의 무결성을 보호 하는 데 도움이 됩니다.


## <a name="next-steps"></a>다음 단계

프로젝트를 만든 후 응용 프로그램에서 원격 변수 값 검색을 시작 하기 위해 앱을 [코딩](code-your-experiment-in-your-app.md) 하 고 [프로젝트에서 실험을 만들](define-your-experiment-in-the-dev-center-dashboard.md)수 있습니다.

## <a name="related-topics"></a>관련 항목

* [실험용 앱 코딩](code-your-experiment-in-your-app.md)
* [파트너 센터에서 실험 정의](define-your-experiment-in-the-dev-center-dashboard.md)
* [파트너 센터에서 실험 관리](manage-your-experiment.md)
* [A/B 테스트를 사용 하 여 첫 번째 실험 만들기 및 실행](create-and-run-your-first-experiment-with-a-b-testing.md)
* [A/B 테스트로 앱 실험 실행](run-app-experiments-with-a-b-testing.md)
