---
Description: 파트너 센터를 사용 하 여 실험을 실행할 A 사용 하 여 유니버설 Windows 플랫폼 (UWP) 앱에 대 한 / B 테스트 합니다.
title: A/B 테스트로 앱 실험 실행
ms.assetid: 790B4B37-C72D-4CEA-97AF-D226B2216DCC
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10 uwp, Microsoft Store Services SDK A/B 테스트, 실험
ms.localizationpriority: medium
ms.openlocfilehash: d4f5271d70cefea99a9caff04e7203e05043440c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599778"
---
# <a name="run-app-experiments-with-ab-testing"></a>A/B 테스트로 앱 실험 실행

파트너 센터를 사용 하 여 유니버설 Windows 플랫폼 (UWP) 앱에서 런타임에 검색할 수 있는 원격 변수를 정의 하 고 사용자가 이끌어내기 원하는 사용자 동작에 대 한 가장 효과적인 값을 식별을 사용 하 여 다양 한 이러한 값을 테스트할 수 있습니다. 원격 변수를 사용하여 앱에서 바로 구매, 등록 흐름, 캡션 및 광고 배치 등의 앱 환경을 구성할 수 있습니다.

A/B 테스트의 목적은 보다 흥미로운 앱 환경을 제공하여 전환율 향상(예: 앱에서 바로 구매 증가) 가능성이 큰 원격 변수 값의 변형을 식별하는 것입니다. 성공적인 변형을 식별 한 후 즉시 실험 종료를 앱에 다시 게시 하지 않고도 파트너 센터에서 전체 사용자 대상 그룹에 대 한 해당 변형을 사용 하도록 설정 합니다.

## <a name="create-and-run-an-ab-test"></a>A/B 테스트 만들기 및 실행

A/B 테스트를 만들고 실행하려면 다음 단계를 따르세요.

1. [프로젝트를 만들고 파트너 센터에서 원격 변수 정의](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)합니다. 이 프로젝트에는 실험에 대한 변수 및 기본 변수 값이 포함되어 있습니다.  
2. [실험용 앱을 코딩합니다](code-your-experiment-in-your-app.md). 파트너 센터에서 만든 프로젝트에서 원격 변수 값을 얻으려면,이 데이터를 테스트 하는 기능의 동작을 수정 하는 데, 뷰 이벤트를 보내고 파트너 센터로 변환 이벤트를 Microsoft Store Services SDK API를 사용 합니다.
3. [파트너 센터에서 실험 정의 ](define-your-experiment-in-the-dev-center-dashboard.md)합니다. A/B 테스트에 대한 고유한 목표와 변형을 정의하는 프로젝트에서 실험을 만듭니다.
4. [실행 및 파트너 센터 ashboard 실험 관리](manage-your-experiment.md)합니다. 실험을 활성화 하 고 실험의 결과 검토 하 고 실험을 완료 하려면 파트너 센터를 사용 합니다.

엔드투엔드 프로세스를 보여 주는 연습은 [A/B 테스트로 첫 번째 실험 만들기 및 실행](create-and-run-your-first-experiment-with-a-b-testing.md)을 참조하세요.

## <a name="requirements"></a>요구 사항

A / B 테스트 파트너 센터에서은 UWP 앱에 대해서만 지원 합니다.

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
| Experiment    |   사용자가 받는 A/B 테스트를 정의하는 매개 변수의 집합입니다. 실험은 프로젝트의 범위로 정의되고 각 실험은 다음으로 구성됩니다. <p></p><ul><li>사용자가 실험의 일부인 변형을 보기 시작하는 경우를 나타내는 *보기 이벤트*</li><li>목표에 도달한 경우를 나타내는 *전환 이벤트*가 있는 하나 이상의 목표</li><li>실험에서 사용되는 변수 데이터를 정의하는 하나 이상의 *변형*. *컨트롤* 변형은 실험의 프로젝트에서 정의된 기본 변수 값을 사용합니다. 일반적으로 실험에는 컨트롤 변형 외에 실험에 고유한 변형 변수 값을 갖는 하나 이상의 추가 변형이 있습니다. </li></ul>          |
| 프로젝트 ID    |   파트너 센터 계정에서 프로젝트를 사용 하 여 앱을 연결 하는 고유 ID입니다. A를 사용 하 여 연결 하려면이 ID를 사용 해야 / B 변형을 데이터를 받고 파트너 센터로 뷰와 변환 이벤트를 보고 하기 위해 앱 코드에 서비스를 테스트 합니다. 자세한 내용은 [실험용 앱 코딩](code-your-experiment-in-your-app.md)을 참조하세요.<p></p><p>각 프로젝트 및 프로젝트의 모든 실험은 정확히 하나의 프로젝트 ID와 연결됩니다. 프로젝트 ID를 사용하여 다른 실험 집합 간을 구분할 수 있습니다. 예를 들어 조직의 테스터에게 릴리스한 하나의 실험 집합이 있고 앱의 외부 사용자에게만 릴리스한 다른 실험 집합이 있을 수 있습니다.  여러 실험을 구현하는 경우 앱은 여러 프로젝트 ID를 참조할 수 있습니다.</p>         |
| 변형    |   실험에서 테스트할 하나 이상의 변형 컬렉션입니다. 모든 실험에는 최소 한 개의 변수와 두 개의 변형(컨트롤 포함)이 있어야 합니다. 하나의 실험에는 최대 다섯 개의 변형이 있을 수 있습니다.           |
| 변수    |  앱에서 속성 또는 일부 다른 값을 초기화하는 데 사용하는 값입니다. 실험 중 변수 값은 여러 변형으로 변경됩니다. 실험을 종료하면 앱의 모든 사용자에게 릴리스하도록 선택한 변형의 값이 변수에 할당됩니다. 변수에는 문자열, 부울, 이중 및 정수 유형이 있습니다.
| 보기 이벤트    |  사용자가 실험의 일부인 변형을 보기 시작할 때 작업을 나타내는 임의의 문자열입니다. 일반적으로 코드의 이벤트 이름입니다. 앱 코드는 변형 보기 시작할 때이 뷰 이벤트 문자열 파트너 센터를 전송 합니다. 자세한 내용은 [실험용 앱 코딩](code-your-experiment-in-your-app.md)을 참조하세요.
| 전환 이벤트    |  실험의 목표를 나타내는 임의의 문자열입니다. 일반적으로 코드의 이벤트 이름입니다. 앱 코드는 목표에 도달한 경우 파트너 센터를이 변환 이벤트 문자열을 전송 합니다. 자세한 내용은 [실험용 앱 코딩](code-your-experiment-in-your-app.md)을 참조하세요.  

## <a name="related-topics"></a>관련 항목

* [프로젝트를 만들고 파트너 센터에서 원격 변수를 정의 합니다.](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [실험에 대 한 앱 코드](code-your-experiment-in-your-app.md)
* [파트너 센터에서 실험을 정의 합니다.](define-your-experiment-in-the-dev-center-dashboard.md)
* [파트너 센터에서 실험 관리](manage-your-experiment.md)
* [만들고 A를 사용 하 여 첫 번째 실험 실행 / B 테스트](create-and-run-your-first-experiment-with-a-b-testing.md)
