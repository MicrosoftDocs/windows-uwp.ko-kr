---
author: Xansky
Description: You can use the Windows Dev Center dashboard to run experiments for your Universal Windows Platform (UWP) apps with A/B testing.
title: A/B 테스트로 앱 실험 실행
ms.assetid: 790B4B37-C72D-4CEA-97AF-D226B2216DCC
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10 uwp, Microsoft Store Services SDK A/B 테스트, 실험
ms.localizationpriority: medium
ms.openlocfilehash: bbb72098a7a0b4557183bfb2f1a4e0c2650c4a29
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5946636"
---
# <a name="run-app-experiments-with-ab-testing"></a>A/B 테스트로 앱 실험 실행

Windows 개발자 센터 대시보드를 사용하여 UWP(유니버설 Windows 플랫폼) 앱에서 런타임 시 검색할 수 있는 원격 변수를 정의할 수 있고, 이러한 값의 변형을 테스트하여 원하는 사용자 동작을 유도하는 데 가장 효과적인 값을 확인할 수 있습니다. 원격 변수를 사용하여 앱에서 바로 구매, 등록 흐름, 캡션 및 광고 배치 등의 앱 환경을 구성할 수 있습니다.

A/B 테스트의 목적은 보다 흥미로운 앱 환경을 제공하여 전환율 향상(예: 앱에서 바로 구매 증가) 가능성이 큰 원격 변수 값의 변형을 식별하는 것입니다. 성공적인 변형을 식별한 후 즉시 실험을 종료하고, 앱을 다시 게시할 필요 없이 개발자 센터 대시보드에서 전체 사용자 대상 그룹에 대해 해당 변형을 사용하도록 설정할 수 있습니다.

## <a name="create-and-run-an-ab-test"></a>A/B 테스트 만들기 및 실행

A/B 테스트를 만들고 실행하려면 다음 단계를 따르세요.

1. [개발자 센터 대시보드에서 프로젝트 만들기 및 원격 변수를 정의합니다](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md). 이 프로젝트에는 실험에 대한 변수 및 기본 변수 값이 포함되어 있습니다.  
2. [실험용 앱을 코딩합니다](code-your-experiment-in-your-app.md). Microsoft Store Services SDK의 API를 사용하여 대시보드에서 만든 프로젝트에서 원격 변수 값을 가져오고, 이 데이터를 사용하여 테스트할 기능의 동작을 수정하고, 개발자 센터에 보기 이벤트 및 전환 이벤트를 전송합니다.
3. [개발자 센터 대시보드에서 실험을 정의](define-your-experiment-in-the-dev-center-dashboard.md)합니다. A/B 테스트에 대한 고유한 목표와 변형을 정의하는 프로젝트에서 실험을 만듭니다.
4. [개발자 센터 대시보드에서 실험을 실행하고 관리합니다](manage-your-experiment.md). 실험을 활성화하고 대시보드를 사용하여 실험의 결과를 검토하고 실험을 완료합니다.

종단 간 프로세스를 보여 주는 연습은 [A/B 테스트로 첫 번째 실험 만들기 및 실행](create-and-run-your-first-experiment-with-a-b-testing.md)을 참조하세요.

## <a name="requirements"></a>요구 사항

Windows 개발자 센터의 A/B 테스트는 UWP 앱에 대해서만 지원됩니다.

A/B 테스트로 실험을 실행하려면 먼저 개발 컴퓨터를 설정해야 합니다.

* [여기](../get-started/get-set-up.md)에 있는 지침에 따라 UWP 개발용으로 개발 컴퓨터를 설정합니다.
* [Microsoft Store Services SDK를 설치합니다.](microsoft-store-services-sdk.md#install-the-sdk) 실험에 대한 API 외에, 이 SDK는 광고 표시, 앱에 대한 피드백을 수집하기 위해 고객을 피드백 허브로 보내기 등의 다른 기능을 위한 API도 제공합니다.

## <a name="best-practices"></a>모범 사례

가장 유용한 결과를 얻으려면 A/B 테스트로 실험을 실행할 때 다음과 같은 권장 사항을 따르는 것이 좋습니다.

* 변형 할당에 무작위 50/50 분할 분포를 사용하여 두 가지 변형만으로 실험을 실행하는 것이 좋습니다.
* 통계적으로 의미가 있고 작업 가능한 충분한 데이터를 수집하기 위해 최소 2-4주 동안 실험을 실행합니다.

<span id="terms" />

## <a name="related-terms"></a>관련 용어

|  용어  |  정의  |
|--------|--------------|
| Project    |   Microsoft Store Services SDK를 사용하여 앱에서 액세스할 수 있는 기본값을 갖는 원격 변수의 컬렉션입니다. 프로젝트에는 동일한 원격 변수를 공유하는 하나 이상의 실험이 포함될 수도 있습니다.  |
| 실험    |   사용자가 받는 A/B 테스트를 정의하는 매개 변수의 집합입니다. 실험은 프로젝트의 범위로 정의되고 각 실험은 다음으로 구성됩니다. <p></p><ul><li>사용자가 실험의 일부인 변형을 보기 시작하는 경우를 나타내는 *보기 이벤트*</li><li>목표에 도달한 경우를 나타내는 *전환 이벤트*가 있는 하나 이상의 목표</li><li>실험에서 사용되는 변수 데이터를 정의하는 하나 이상의 *변형*. *컨트롤* 변형은 실험의 프로젝트에서 정의된 기본 변수 값을 사용합니다. 일반적으로 실험에는 컨트롤 변형 외에 실험에 고유한 변형 변수 값을 갖는 하나 이상의 추가 변형이 있습니다. </li></ul>          |
| 프로젝트 ID    |   개발자 센터 계정의 프로젝트와 앱을 연결하는 고유 ID입니다. 변형 데이터를 받고 보기 및 전환 이벤트를 개발자 센터에 보고하기 위해 앱 코드에서 이 ID를 사용하여 A/B 테스트 서비스와 연결해야 합니다. 자세한 내용은 [실험용 앱 코딩](code-your-experiment-in-your-app.md)을 참조하세요.<p></p><p>각 프로젝트 및 프로젝트의 모든 실험은 정확히 하나의 프로젝트 ID와 연결됩니다. 프로젝트 ID를 사용하여 다른 실험 집합 간을 구분할 수 있습니다. 예를 들어 조직의 테스터에게 릴리스한 하나의 실험 집합이 있고 앱의 외부 사용자에게만 릴리스한 다른 실험 집합이 있을 수 있습니다.  여러 실험을 구현하는 경우 앱은 여러 프로젝트 ID를 참조할 수 있습니다.</p>         |
| 변형    |   실험에서 테스트할 하나 이상의 변형 컬렉션입니다. 모든 실험에는 최소 한 개의 변수와 두 개의 변형(컨트롤 포함)이 있어야 합니다. 하나의 실험에는 최대 다섯 개의 변형이 있을 수 있습니다.           |
| 변수    |  앱에서 속성 또는 일부 다른 값을 초기화하는 데 사용하는 값입니다. 실험 중 변수 값은 여러 변형으로 변경됩니다. 실험을 종료하면 앱의 모든 사용자에게 릴리스하도록 선택한 변형의 값이 변수에 할당됩니다. 변수에는 문자열, 부울, 이중 및 정수 유형이 있습니다.
| 보기 이벤트    |  사용자가 실험의 일부인 변형을 보기 시작할 때 작업을 나타내는 임의의 문자열입니다. 일반적으로 코드의 이벤트 이름입니다.  앱 코드는 사용자가 변형을 보기 시작할 때 개발자 센터에 이 보기 이벤트 문자열을 전송합니다. 자세한 내용은 [실험용 앱 코딩](code-your-experiment-in-your-app.md)을 참조하세요.
| 전환 이벤트    |  실험의 목표를 나타내는 임의의 문자열입니다. 일반적으로 코드의 이벤트 이름입니다.  앱 코드는 사용자가 목표에 도달할 때 개발자 센터에 이 전환 이벤트 문자열을 전송합니다. 자세한 내용은 [실험용 앱 코딩](code-your-experiment-in-your-app.md)을 참조하세요.  

## <a name="related-topics"></a>관련 항목

* [개발자 센터 대시보드에서 프로젝트 만들기 및 원격 변수 정의](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [실험용 앱 코딩](code-your-experiment-in-your-app.md)
* [개발자 센터 대시보드에서 실험 정의](define-your-experiment-in-the-dev-center-dashboard.md)
* [개발자 센터 대시보드에서 실험 관리](manage-your-experiment.md)
* [A/B 테스트로 첫 번째 실험 만들기 및 실행](create-and-run-your-first-experiment-with-a-b-testing.md)
