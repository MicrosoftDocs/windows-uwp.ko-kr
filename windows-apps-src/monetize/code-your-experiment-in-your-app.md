---
Description: A/B 테스트를 사용 하 여 유니버설 Windows 플랫폼 (UWP) 앱에서 실험을 실행 하려면 앱에서 실험을 코딩 해야 합니다.
title: 실험용 앱 코딩
ms.assetid: 6A5063E1-28CD-4087-A4FA-FBB511E9CED5
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store Services SDK, A/B 테스트, 실험
ms.localizationpriority: medium
ms.openlocfilehash: 3a7709311539d3f9c50f600f617c211f99c7b507
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171677"
---
# <a name="code-your-app-for-experimentation"></a>실험용 앱 코딩

[프로젝트를 만들고 파트너 센터에서 원격 변수를 정의한](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)후 UWP (유니버설 Windows 플랫폼) 앱에서 코드를 업데이트할 준비가 된 것입니다.
* 파트너 센터에서 원격 변수 값을 수신 합니다.
* 원격 변수를 사용 하 여 사용자에 대 한 앱 환경을 구성 합니다.
* 사용자가 실험을 보고 원하는 작업 ( *변환*이 라고도 함)을 수행한 시기를 나타내는 파트너 센터에 이벤트를 기록 합니다.

앱에이 동작을 추가 하려면 Microsoft Store Services SDK에서 제공 하는 Api를 사용 합니다.

다음 섹션에서는 실험을 위한 변형과 파트너 센터에 이벤트를 기록 하는 일반적인 프로세스에 대해 설명 합니다. 실험을 위해 앱을 코딩 한 후 [파트너 센터에서 실험을 정의할](define-your-experiment-in-the-dev-center-dashboard.md)수 있습니다. 실험을 만들고 실행 하는 종단 간 프로세스를 보여 주는 연습은 [a/B 테스트를 사용 하 여 첫 번째 실험 만들기 및 실행](create-and-run-your-first-experiment-with-a-b-testing.md)을 참조 하세요.

> [!NOTE]
> Microsoft Store Services SDK의 실험 Api 중 일부는 [비동기 패턴](../threading-async/asynchronous-programming-universal-windows-platform-apps.md) 을 사용 하 여 파트너 센터에서 데이터를 검색 합니다. 즉, 메서드가 호출 된 후에 이러한 메서드 실행 부분이 수행 될 수 있으므로 작업이 완료 되는 동안에는 앱의 UI가 응답성을 유지할 수 있습니다. 비동기 패턴을 사용 하려면이 문서의 코드 예제에 나와 있는 것 처럼 Api를 호출할 때 앱에서 **async** 키워드 및 **wait** 연산자를 사용 해야 합니다. 규칙에 따라 비동기 메서드는 **Async**로 끝납니다.

## <a name="configure-your-project"></a>프로젝트 구성

시작 하려면 개발 컴퓨터에 Microsoft Store Services SDK를 설치 하 고 프로젝트에 필요한 참조를 추가 합니다.

1. [Microsoft Store SERVICES SDK를 설치](microsoft-store-services-sdk.md#install-the-sdk)합니다.
2. Visual Studio에서 프로젝트를 엽니다.
3. 솔루션 탐색기에서 프로젝트 노드를 확장 하 고 **참조**를 마우스 오른쪽 단추로 클릭 한 다음 **참조 추가**를 클릭 합니다.
3. **참조 관리자**에서 **유니버설 Windows** 를 확장 하 고 **확장**을 클릭 합니다.
4. Sdk 목록에서 **Microsoft Engagement 프레임 워크** 옆의 확인란을 선택 하 고 **확인**을 클릭 합니다.

> [!NOTE]
> 이 문서의 코드 예제에서는 코드 파일에 **system.object** 및 **Microsoft** .. t a t 네임 스페이스에 대 한 문을 **사용 하** 는 것으로 가정 합니다.

## <a name="get-variation-data-and-log-the-view-event-for-your-experiment"></a>변형 데이터 가져오기 및 실험을 위한 뷰 이벤트 기록

프로젝트에서 실험에서 수정 하려는 기능에 대 한 코드를 찾습니다. 변형에 대 한 데이터를 검색 하는 코드를 추가 하 고,이 데이터를 사용 하 여 테스트 중인 기능의 동작을 수정한 다음, 실험을 위한 view 이벤트를 파트너 센터의 A/B 테스트 서비스에 기록 합니다.

필요한 특정 코드는 앱에 따라 달라 지지만 다음 예제에서는 기본 프로세스를 보여 줍니다. 전체 코드 예제를 보려면 [a/B 테스트를 사용 하 여 첫 번째 실험 만들기 및 실행](create-and-run-your-first-experiment-with-a-b-testing.md)을 참조 하세요.

[!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#ExperimentCodeSample)]

다음 단계에서는이 프로세스의 중요 한 부분에 대해 자세히 설명 합니다.

1. 보기 및 변환 이벤트를 파트너 센터에 기록 하는 데 사용 하는 [StoreServicesCustomEventLogger](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger) 개체 및 현재 변형 할당을 나타내는 [StoreServicesExperimentVariation](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) 개체를 선언 합니다.

    [!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet1)]

2. 검색 하려는 실험의 [프로젝트 ID](run-app-experiments-with-a-b-testing.md#terms) 에 할당 된 문자열 변수를 선언 합니다.
    > [!NOTE]
    > [파트너 센터에서 프로젝트를 만들](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)때 프로젝트 ID를 가져옵니다. 아래에 표시 된 프로젝트 ID는 예를 들어 목적 으로만 사용 됩니다.

    [!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet2)]

3. 정적 [Getcachedvariationasync](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getcachedvariationasync) 메서드를 호출 하 여 실험에 대 한 현재 캐시 된 변형 할당을 가져오고 실험의 프로젝트 ID를 메서드에 전달 합니다. 이 메서드는 [ExperimentVariation](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariationresult.experimentvariation) 속성을 통해 변형 할당에 대 한 액세스를 제공 하는 [StoreServicesExperimentVariationResult](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariationresult) 개체를 반환 합니다.

    [!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet3)]

4. [Isstale](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.isstale) 속성을 확인 하 여 캐시 된 변형 할당을 서버의 원격 변형 할당으로 새로 고쳐야 하는지 여부를 확인 합니다. 새로 고쳐야 하는 경우 정적 [Getrefreshedvariationasync](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getrefreshedvariationasync) 메서드를 호출 하 여 서버에서 업데이트 된 변형 할당을 확인 하 고 로컬 캐시 된 변형을 새로 고칩니다.

    [!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet4)]

5. [StoreServicesExperimentVariation](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) 개체의 [getboolean](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getboolean), [Getboolean](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getdouble), [GetInt32](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getint32)또는 [GetString](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring) 메서드를 사용 하 여 변형 할당 값을 가져옵니다. 각 메서드에서 첫 번째 매개 변수는 검색 하려는 변형의 이름입니다 (파트너 센터에서 입력 하는 변형의 이름과 동일). 두 번째 매개 변수는 파트너 센터에서 지정 된 값을 검색할 수 없는 경우 (예: 네트워크 연결이 없는 경우) 메서드가 반환 해야 하는 기본값이 며, 캐시 된 버전의 변형을 사용할 수 없습니다.

    다음 예에서는 [GetString](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring) 를 사용 하 여 *buttontext* 라는 변수를 가져오고 기본 변수 값인 **회색 단추**를 지정 합니다.

    [!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet5)]

6. 코드에서 변수 값을 사용 하 여 테스트 중인 기능의 동작을 수정 합니다. 예를 들어 다음 코드는 응용 프로그램의 단추 내용에 *Buttontext* 값을 할당 합니다. 이 예제에서는 프로젝트의 다른 위치에서이 단추를 이미 정의 했다고 가정 합니다.

    [!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet6)]

7. 마지막으로, 실험을 위한 [view 이벤트](run-app-experiments-with-a-b-testing.md#terms) 를 파트너 센터의 A/B 테스트 서비스에 기록 합니다. ```logger``` [StoreServicesCustomEventLogger](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger) 개체에 대 한 필드를 초기화 하 고 [logforvariation](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation) 메서드를 호출 합니다. 현재 변형 할당을 나타내는 [StoreServicesExperimentVariation](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) 개체를 전달 합니다 .이 개체는 파트너 센터에 이벤트에 대 한 컨텍스트를 제공 합니다 .이 개체는 실험에 대 한 뷰 이벤트의 이름입니다. 파트너 센터에서 실험에 대해 입력 하는 뷰 이벤트 이름과 일치 해야 합니다. 사용자가 실험의 일부인 변형 보기를 시작할 때 코드에서 view 이벤트를 기록해 야 합니다.

    다음 예에서는 **userViewedButton**라는 뷰 이벤트를 기록 하는 방법을 보여 줍니다. 이 예제에서 실험의 목표는 사용자가 앱의 단추를 클릭 하 여 응용 프로그램에서 변형 데이터 (이 경우 단추 텍스트)를 검색 하 고 단추 내용에 할당 한 후에 보기 이벤트가 기록 되도록 하는 것입니다.

    [!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet7)]

## <a name="log-conversion-events-to-partner-center"></a>파트너 센터에 변환 이벤트 기록

다음으로, [변환 이벤트](run-app-experiments-with-a-b-testing.md#terms) 를 파트너 센터의 A/B 테스트 서비스에 기록 하는 코드를 추가 합니다. 사용자가 실험에 대 한 목표에 도달 하면 코드에서 변환 이벤트를 기록해 야 합니다. 필요한 특정 코드는 앱에 따라 달라 지지만 일반적인 단계는 다음과 같습니다. 전체 코드 예제를 보려면 [a/B 테스트를 사용 하 여 첫 번째 실험 만들기 및 실행](create-and-run-your-first-experiment-with-a-b-testing.md)을 참조 하세요.

1. 사용자가 실험 목표 중 하나에 대 한 목표에 도달할 때 실행 되는 코드에서 [Logforvariation](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation) 메서드를 다시 호출 하 고 [StoreServicesExperimentVariation](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) 개체와 실험을 위한 변환 이벤트의 이름을 전달 합니다. 이는 파트너 센터에서 실험에 대해 입력 하는 변환 이벤트 이름 중 하 나와 일치 해야 합니다.

    다음 예에서는 단추에 대 한 **Click** 이벤트 처리기에서 **userClickedButton** 라는 변환 이벤트를 로깅합니다. 이 예제에서 실험의 목표는 사용자가 단추를 클릭할 수 있도록 하는 것입니다.

    [!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet8)]

## <a name="next-steps"></a>다음 단계

앱에서 실험을 코딩 한 후에는 다음 단계를 수행할 준비가 된 것입니다.
1. [파트너 센터에서 실험을 정의](define-your-experiment-in-the-dev-center-dashboard.md)합니다. A/B 테스트에 대 한 뷰 이벤트, 변환 이벤트 및 고유 변형을 정의 하는 실험을 만듭니다.
2. [파트너 센터에서 실험을 실행 하 고 관리](manage-your-experiment.md)합니다.


## <a name="related-topics"></a>관련 항목

* [파트너 센터에서 프로젝트 만들기 및 원격 변수 정의](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [파트너 센터에서 실험 정의](define-your-experiment-in-the-dev-center-dashboard.md)
* [파트너 센터에서 실험 관리](manage-your-experiment.md)
* [A/B 테스트를 사용 하 여 첫 번째 실험 만들기 및 실행](create-and-run-your-first-experiment-with-a-b-testing.md)
* [A/B 테스트로 앱 실험 실행](run-app-experiments-with-a-b-testing.md)