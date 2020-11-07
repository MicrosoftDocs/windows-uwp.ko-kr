---
description: 파트너 센터를 사용 하 여 A/B 테스트를 통해 유니버설 Windows 플랫폼 (UWP) 앱에 대 한 실험을 실행할 수 있습니다.
title: A/B 테스트로 앱 실험 실행
ms.assetid: 790B4B37-C72D-4CEA-97AF-D226B2216DCC
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store Services SDK, A/B 테스트, 실험
ms.localizationpriority: medium
ms.openlocfilehash: 7cc63c1bdf5f3357bed596e5afcf03681eeb513a
ms.sourcegitcommit: aaa72ddeb01b074266f4cd51740eec8d1905d62d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2020
ms.locfileid: "94339742"
---
# <a name="run-app-experiments-with-ab-testing"></a>A/B 테스트로 앱 실험 실행

파트너 센터를 사용 하 여 유니버설 Windows 플랫폼 (UWP) 앱에서 런타임에 검색할 수 있는 원격 변수를 정의할 수 있으며, 사용자와 이러한 값의 변형을 테스트 하 여 원하는 사용자 동작을 제어 하기 위한 가장 효과적인 값을 식별할 수 있습니다. 앱에서 원격 변수를 사용 하 여 앱 내 구매, 등록 흐름, 캡션 및 광고 배치와 같은 앱 환경을 구성할 수 있습니다.

A/B 테스트의 목표는 더 매력적인 앱 환경을 제공 하 여 향상 된 변환 속도 (예: 앱에서 더 많은 구매)를 얻을 수 있는 원격 변수 값의 변형을 식별 하는 것입니다. 변형이 성공적으로 완료 되 면 앱을 다시 게시 하지 않고도 실험을 즉시 종료 하 고 파트너 센터에서 전체 사용자 대상에 대해 해당 변형을 사용 하도록 설정할 수 있습니다.

## <a name="create-and-run-an-ab-test"></a>A/B 테스트 만들기 및 실행

A/B 테스트를 만들고 실행 하려면 다음 단계를 수행 합니다.

1. [파트너 센터에서 프로젝트를 만들고 원격 변수를 정의](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)합니다. 이 프로젝트는 실험에 대 한 변수 및 기본 변수 값을 포함 합니다.  
2. [실험을 위해 앱을 코딩](code-your-experiment-in-your-app.md)합니다. Microsoft Store Services SDK의 API를 사용 하 여 파트너 센터에서 만든 프로젝트에서 원격 변수 값을 가져오고,이 데이터를 사용 하 여 테스트 중인 기능의 동작을 수정 하 고, 보기 이벤트 및 변환 이벤트를 파트너 센터로 보냅니다.
3. [파트너 센터에서 실험을 정의 ](define-your-experiment-in-the-dev-center-dashboard.md)합니다. 프로젝트에서 A/B 테스트에 대 한 고유한 목표와 변형을 정의 하는 실험을 만듭니다.
4. [파트너 센터 ashboard에서 실험을 실행 하 고 관리](manage-your-experiment.md)합니다. 실험을 활성화 하 고 파트너 센터를 사용 하 여 실험 결과를 검토 하 고 실험을 완료 합니다.

종단 간 프로세스를 보여 주는 연습은 [a/B 테스트를 사용 하 여 첫 번째 실험 만들기 및 실행](create-and-run-your-first-experiment-with-a-b-testing.md)을 참조 하세요.

## <a name="requirements"></a>요구 사항

파트너 센터의 A/B 테스트는 UWP 앱 에서만 지원 됩니다.

A/B 테스트를 사용 하 여 실험을 실행 하려면 먼저 개발 컴퓨터를 설정 해야 합니다.

* [여기](/windows/apps/get-started/get-set-up) 에 설명 된 지침에 따라 UWP 개발에 대 한 개발 컴퓨터를 설정 합니다.
* [Microsoft Store SERVICES SDK를 설치](microsoft-store-services-sdk.md#install-the-sdk)합니다. 이 SDK는 실험을 위한 API 외에도 광고를 표시 하 고 고객에 게 피드백 허브를 전달 하 여 앱에 대 한 피드백을 수집 하는 등의 기타 기능을 위한 Api를 제공 합니다.

## <a name="best-practices"></a>모범 사례

가장 유용한 결과를 보려면 A/B 테스트를 사용 하 여 실험을 실행할 때 다음 권장 사항을 따르는 것이 좋습니다.

* 변형 할당을 위해 임의 50/50 분할 분포를 사용 하 여 두 가지 변형으로 실험을 실행 하는 것이 좋습니다.
* 통계적으로 중요 하 고 조치를 취할 수 있는 충분 한 데이터를 수집 하려면 최소 2 ~ 4 주 동안 실험을 실행 합니다.

<span id="terms" />

## <a name="related-terms"></a>관련 용어

|  용어  |  정의  |
|--------|--------------|
| Project    |   Microsoft Store Services SDK를 사용 하 여 앱에서 액세스할 수 있는 기본값이 포함 된 원격 변수의 컬렉션입니다. 또한 프로젝트는 동일한 원격 변수를 공유 하는 하나 이상의 실험을 선택적으로 포함할 수 있습니다.  |
| 실험    |   사용자가 받을 A/B 테스트를 정의 하는 매개 변수 집합입니다. 실험은 프로젝트의 범위에서 정의 되며 각 실험은 다음으로 구성 됩니다. <p></p><ul><li>사용자가 실험의 일부인 변형 보기를 시작 하는 시점을 나타내는 *뷰 이벤트* 입니다.</li><li>목표에 도달 했을 때를 나타내는 *변환 이벤트* 를 포함 하는 하나 이상의 목표입니다.</li><li>실험에서 사용 하는 변수 데이터를 정의 하는 하나 이상의 *변형* 입니다. *컨트롤* 변형은 실험을 위해 프로젝트에 정의 된 기본 변수 값을 사용 합니다. 컨트롤 변형 외에도 실험에는 일반적으로 실험에 고유한 변수 값이 포함 된 추가 변형이 하나 이상 있습니다. </li></ul>          |
| 프로젝트 ID    |   파트너 센터 계정의 프로젝트와 앱을 연결 하는 고유 ID입니다. 이 ID를 사용 하 여 응용 프로그램 코드에서 A/B 테스트 서비스와 연결 하 여 변형 데이터 및 보고서 보기 및 변환 이벤트를 파트너 센터로 받도록 해야 합니다. 자세한 내용은 [실험을 위한 앱 코드](code-your-experiment-in-your-app.md)를 참조 하세요.<p></p><p>프로젝트의 각 프로젝트와 모든 실험은 정확히 하나의 프로젝트 ID에 연결 됩니다. 프로젝트 Id를 사용 하 여 서로 다른 실험 집합을 구분할 수 있습니다. 예를 들어 조직의 테스터에 게 릴리스되는 실험 집합과 앱의 외부 사용자 에게만 릴리스되는 다른 실험 집합이 있을 수 있습니다.  앱은 여러 실험을 구현 하는 경우 여러 프로젝트 Id를 참조할 수 있습니다.</p>         |
| 변형    |   실험에서 테스트 하는 하나 이상의 변수 컬렉션입니다. 모든 실험에는 하나 이상의 변수와 두 개의 변형 (컨트롤 포함)이 있어야 합니다. 실험에는 최대 5 개의 변형이 있을 수 있습니다.           |
| 변수    |  앱에서 속성 또는 다른 값을 초기화 하는 데 사용 하는 값입니다. 실험을 수행 하는 동안 변수 값은 변형에서 변형으로 변경 됩니다. 실험을 종료 한 후에는 앱의 모든 사용자에 게 릴리스를 선택 하는 변형의 값이 변수에 할당 됩니다. 변수에는 문자열, 부울, double 및 정수 등의 형식을 사용할 수 있습니다.
| 이벤트 보기    |  사용자가 실험의 일부인 변형 보기를 시작할 때 활동을 나타내는 임의의 문자열입니다. 일반적으로 코드의 이벤트 이름입니다. 사용자가 변형 보기를 시작 하면 앱 코드에서 파트너 센터에이 보기 이벤트 문자열을 보냅니다. 자세한 내용은 [실험을 위한 앱 코드](code-your-experiment-in-your-app.md)를 참조 하세요.
| 변환 이벤트    |  실험 목표의 목표를 나타내는 임의의 문자열입니다. 일반적으로 코드의 이벤트 이름입니다. 사용자가 목표에 도달 하면 앱 코드에서 파트너 센터에이 변환 이벤트 문자열을 보냅니다. 자세한 내용은 [실험을 위한 앱 코드](code-your-experiment-in-your-app.md)를 참조 하세요.  

## <a name="related-topics"></a>관련 항목

* [파트너 센터에서 프로젝트 만들기 및 원격 변수 정의](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [실험용 앱 코딩](code-your-experiment-in-your-app.md)
* [파트너 센터에서 실험 정의](define-your-experiment-in-the-dev-center-dashboard.md)
* [파트너 센터에서 실험 관리](manage-your-experiment.md)
* [A/B 테스트를 사용 하 여 첫 번째 실험 만들기 및 실행](create-and-run-your-first-experiment-with-a-b-testing.md)