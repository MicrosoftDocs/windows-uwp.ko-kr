---
author: Xansky
Description: In this walkthrough, you will create, run, and manage your first experiment with A/B testing.
title: 첫 번째 실험 만들기 및 실행
ms.assetid: 16A2B129-14E1-4C68-86E8-52F1BE58F256
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store Services SDK, A/B 테스트, 실험
ms.localizationpriority: medium
ms.openlocfilehash: eb6e3f245aaaff46156964b5a6b37b21d22a2102
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "5549769"
---
# <a name="create-and-run-your-first-experiment"></a>첫 번째 실험 만들기 및 실행

이 연습에서는 다음을 수행합니다.
* 개발자 센터 대시보드에서 앱 단추의 텍스트 및 색을 나타내는 여러 원격 변수를 정의하는 실험 [프로젝트](run-app-experiments-with-a-b-testing.md#terms)를 만듭니다.
* 원격 변수 값을 검색하고 이 데이터를 사용하여 단추의 배경색을 변경하며 보기 및 변환 이벤트 데이터를 다시 개발자 센터에 기록하는 코드를 사용하여 앱을 만듭니다.
* 앱 단추의 배경색을 변경하면 단추 클릭 수가 늘어나는지 여부를 테스트하도록 프로젝트에 실험을 만듭니다.
* 실험 데이터를 수집하는 앱을 실행합니다.
* 개발자 센터 대시보드에서 실험 결과를 검토하고, 앱의 모든 사용자가 사용할 수 있도록 설정할 변형을 선택하고, 실험을 완료합니다.

개발자 센터를 사용하여 A/B 테스트의 개요를 보려면 [A/B 테스트로 앱 실험 실행](run-app-experiments-with-a-b-testing.md)을 참조하세요.

## <a name="prerequisites"></a>필수 조건

이 연습을 수행하려면 Windows 개발자 센터 계정이 있어야 하며 [A/B 테스트로 앱 실험 실행](run-app-experiments-with-a-b-testing.md)에서 설명한 대로 개발 컴퓨터를 구성해야 합니다.

## <a name="create-a-project-with-remote-variables-in-windows-dev-center"></a>Windows 개발자 센터에서 원격 변수로 프로젝트 만들기

1. [개발자 센터 대시보드](https://dev.windows.com/overview)에 로그인합니다.
2. 개발자 센터에 실험을 만드는 데 사용할 앱이 이미 있는 경우 대시보드에서 앱을 선택합니다. 대시보드에 아직 앱이 없는 경우 [이름을 예약하여 새 앱을 만든](../publish/create-your-app-by-reserving-a-name.md) 다음 대시보드에서 해당 앱을 선택합니다.
3. 탐색 창에서 **서비스**를 클릭한 다음 **실험**을 클릭합니다.
4. 다음 페이지의 **프로젝트** 섹션에서 **새 프로젝트** 단추를 클릭합니다.
5. **새 프로젝트** 페이지에서 새 프로젝트에 대한 이름 **Button Click Experiments**를 입력합니다.
6. **원격 변수** 섹션을 확장하고 **변수 추가**를 4번 클릭합니다. 이제 4개의 빈 변수 행이 표시됩니다.
  * 첫 번째 행에서 변수 이름에 대해 **buttonText**를 입력하고 **기본값** 열에 **Grey Button**을 입력합니다.
  * 두 번째 행에서 변수 이름에 대해 **r**을 입력하고 **기본값** 열에 **128**을 입력합니다.
  * 세 번째 행에서 변수 이름에 대해 **g**를 입력하고 **기본값** 열에 **128**을 입력합니다.
  * 네 번째 행에서 변수 이름에 대해 **b**를 입력하고 **기본값** 열에 **128**을 입력합니다.
7. **저장**을 클릭하고 **SDK 통합** 섹션에 표시되는 [프로젝트 ID](run-app-experiments-with-a-b-testing.md#terms) 값을 기록해 둡니다. 다음 섹션에서 앱 코드를 업데이트하고 코드에서 이 값을 참조합니다.

## <a name="code-the-experiment-in-your-app"></a>앱에서 실험 코딩

1. Visual Studio에서 Visual C#을 사용 하 여 새 유니버설 Windows 플랫폼 프로젝트를 만듭니다. 프로젝트 이름을 **SampleExperiment**로 지정합니다.
2. 솔루션 탐색기에서 프로젝트 노드를 확장하여 **참조**를 마우스 오른쪽 단추로 클릭하고 **참조 추가**를 클릭합니다.
3. **참조 관리자**에서 **유니버설 Windows**를 확장하고 **확장**을 클릭합니다.
4. SDK 목록에서 **Microsoft Engagement Framework**(Microsoft 참여 프레임워크) 옆의 확인란을 선택하고 **확인**을 클릭합니다.
5. **솔루션 탐색기**에서 MainPage.xaml을 두 번 클릭하여 앱에서 기본 페이지에 대한 디자이너를 엽니다.
6. **도구 상자**에서 페이지로 **단추**를 끌어다 놓습니다.
7. 디자이너에서 단추를 두 번 클릭하여 코드 파일을 열고 **Click** 이벤트에 대한 이벤트 처리기를 추가합니다.  
8. 코드 파일의 전체 내용을 다음 코드로 바꿉니다. 이전 섹션에서 개발자 센터 대시보드를 통해 얻은 [프로젝트 ID](run-app-experiments-with-a-b-testing.md#terms) 값에 ```projectId``` 변수를 할당합니다.
    [!code-cs[SampleExperiment](./code/StoreSDKSamples/cs/ExperimentPage.xaml.cs#SampleExperiment)]

9. 코드 파일을 저장하고 프로젝트를 빌드합니다.

## <a name="create-the-experiment-in-windows-dev-center"></a>Windows 개발자 센터에서 실험 만들기

1. Windows 개발자 센터 대시보드의 **Button Click Experiments** 프로젝트 페이지로 돌아갑니다.
2. **실험** 섹션에서 **새 실험** 단추를 클릭합니다.
3. **실험 세부 정보** 섹션에서 **실험 이름** 필드에 **Optimize Button Clicks** 이름을 입력합니다.
4. **보기 이벤트** 섹션에서 **보기 이벤트 이름** 필드에 **userViewedButton**을 입력합니다. 이 이름은 이전 섹션에서 추가한 코드에 기록한 보기 이벤트 문자열과 일치합니다.
5. **목표 및 전환 이벤트** 섹션에서 다음 값을 입력합니다.
  * **목표 이름** 필드에서 **Increase Button Clicks**를 입력합니다.
  * **전환 이벤트 이름** 필드에서 이름 **userClickedButton**을 입력합니다. 이 이름은 이전 섹션에서 추가한 코드에 기록한 변환 이벤트 문자열과 일치합니다.
  * **목표** 필드에서 **최대화**를 선택합니다.
6. 해당 변형이 앱에 고르게 배포되도록 **원격 변수 및 변형** 섹션에서 **고르게 분배** 확인란이 선택되어 있는지 확인합니다.
7. 실험에 변수를 추가합니다.
    1. 드롭다운 컨트롤을 클릭하여 **buttonText**를 선택하고 **변수 추가**를 클릭합니다. **Grey Button** 문자열이 **변형 A** 열(이 값은 프로젝트 설정에서 파생됨)에 자동으로 표시되어야 합니다. **변형 B** 열에서 **Blue Button**을 입력합니다.
    2. 드롭다운 컨트롤을 다시 클릭하여 **r**을 선택하고 **변수 추가**를 클릭합니다. **128** 문자열이 **변형 A** 열에 자동으로 표시됩니다. **변형 B** 열에 **1**을 입력합니다.
    3. 드롭다운 컨트롤을 다시 클릭하여 **g**를 선택하고 **변수 추가**를 클릭합니다. **128** 문자열이 **변형 A** 열에 자동으로 표시됩니다. **변형 B** 열에 **1**을 입력합니다.  
    4. 드롭다운 컨트롤을 다시 클릭하여 **b**를 선택하고 **변수 추가**를 클릭합니다. **128** 문자열이 **변형 A** 열에 자동으로 표시됩니다. **변형 B** 열에 **255**를 입력합니다.  
8. **저장**을 클릭한 다음 **활성화**를 클릭합니다.

> [!IMPORTANT]
> 실험을 만들 때 **편집 가능한 실험** 확인란을 클릭하지 않은 경우 실험을 활성화한 후에는 더 이상 실험 매개 변수를 수정할 수 없습니다. 일반적으로 실험을 활성화하기 전에 앱에서 실험을 코딩하는 것이 좋습니다.

## <a name="run-the-app-to-gather-experiment-data"></a>실험 데이터를 수집하는 앱 실행

1. 이전에 만든 **SampleExperiment** 앱을 실행합니다.
2. 회색이나 파란색 단추가 표시되는지 확인합니다. 단추를 클릭한 다음 앱을 닫습니다.
3. 동일한 컴퓨터에서 위의 단계를 여러 번 반복하여 앱에 동일한 단추 색이 표시되는지 확인합니다.

## <a name="review-the-results-and-complete-the-experiment"></a>결과 검토 및 실험 완료

이전 섹션을 완료한 후 몇 시간 동안 기다린 후 다음 단계를 수행하여 실험의 결과를 검토하고 실험을 완료합니다.

> [!NOTE]
> 실험을 활성화하면 개발자 센터에서 실험에 대한 데이터를 기록하기 위해 계측되는 모든 앱에서 즉시 데이터를 수집하기 시작합니다. 그러나 실험 데이터를 대시보드에 표시하려면 몇 시간이 걸릴 수 있습니다.

1. 개발자 센터에서 앱의 **실험** 페이지로 돌아갑니다.
2. **활성 실험** 섹션에서 **단추 클릭 최적화**를 클릭하여 이 실험 페이지로 이동합니다.
3. **결과 요약** 및 **결과 세부 정보** 섹션에 표시되는 결과가 예상과 일치하는지 확인합니다. 이러한 섹션에 대한 자세한 내용은 [개발자 센터 대시보드에서 실험 관리](manage-your-experiment.md#review-the-results-of-your-experiment)를 참조하세요.
    > [!NOTE]
    > 개발자 센터에서는 24시간 내의 각 사용자에 대한 첫 번째 전환 이벤트만 보고합니다. 사용자가 24시간 내에 앱에서 여러 전환 이벤트를 트리거하는 경우 첫 번째 전환 이벤트만 보고됩니다. 이렇게 하면 많은 전환 이벤트를 트리거하는 단일 사용자가 샘플 그룹 사용자의 실험 결과를 왜곡시키지 않을 수 있습니다.

4. 이제 실험을 끝낼 준비가 완료되었습니다. **결과 요약** 섹션의 **변형 B** 열에서 **스위치**를 클릭합니다. 이렇게 하면 앱의 모든 사용자가 파란색 단추로 전환됩니다.
5. **확인**을 클릭하여 실험을 종료할 것인지 확인합니다.
6. 이전 섹션에서 만든 **SampleExperiment** 앱을 실행합니다.
7. 파란색 단추가 표시되는지 확인합니다. 앱에서 업데이트된 변형 할당을 받으려면 최대 2분이 걸릴 수 있습니다.

## <a name="related-topics"></a>관련 항목

* [개발자 센터 대시보드에서 프로젝트 만들기 및 원격 변수 정의](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [실험용 앱 코딩](code-your-experiment-in-your-app.md)
* [개발자 센터 대시보드에서 실험 정의](define-your-experiment-in-the-dev-center-dashboard.md)
* [개발자 센터 대시보드에서 실험 관리](manage-your-experiment.md)
* [A/B 테스트로 앱 실험 실행](run-app-experiments-with-a-b-testing.md)
