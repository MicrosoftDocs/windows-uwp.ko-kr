---
Description: To run an experiment in your Universal Windows Platform (UWP) app with A/B testing, you must code the experiment in your app.
title: 실험용 앱 코딩
ms.assetid: 6A5063E1-28CD-4087-A4FA-FBB511E9CED5
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Services SDK, A/B 테스트, 실험
ms.localizationpriority: medium
ms.openlocfilehash: f0d977d41cea873fc0f5e00bea8d0259586517d5
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8332117"
---
# <a name="code-your-app-for-experimentation"></a>실험용 앱 코딩

하면 [프로젝트 만들기 및 파트너 센터에서 원격 변수 정의](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)유니버설 Windows 플랫폼 (UWP) 앱에서 코드를 업데이트할 준비가:
* 파트너 센터에서 원격 변수 값을 수신 합니다.
* 원격 변수를 사용하여 사용자를 위한 앱 환경 구성
* 사용자가 실험을 표시 하 고 ( *변환*라고도 함) 원하는 작업을 수행 하는 때를 표시 하는 파트너 센터에 이벤트를 기록 합니다.

앱에 이 동작을 추가하려면 Microsoft Store Services SDK에서 제공하는 API를 사용합니다.

다음 섹션에서는 실험에 대 한 변형을 가져오고 이벤트를 파트너 센터에 기록 하는 일반적인 프로세스를 설명 합니다. 실험용 앱을 코딩 하면 [파트너 센터에서 실험을 정의할](define-your-experiment-in-the-dev-center-dashboard.md)수 있습니다. 실험 만들기 및 실행의 종단 간 프로세스를 보여 주는 연습에 대한 자세한 내용은 [A/B 테스트로 첫 번째 실험 만들기 및 실행](create-and-run-your-first-experiment-with-a-b-testing.md)을 참조하세요.

> [!NOTE]
> Microsoft Store Services SDK에서 실험 Api 중 일부 [비동기 패턴](../threading-async/asynchronous-programming-universal-windows-platform-apps.md) 을 사용 하 여 파트너 센터에서 데이터를 검색 합니다. 즉 이러한 메서드 실행 중 일부는 메서드를 호출한 이후에 발생할 수 있으므로 작업이 완료되더라도 앱 UI는 응답 상태를 유지할 수 있습니다. 비동기 패턴에서는 이 문서의 코드 예제에서 살펴본 것처럼 API를 호출할 때 앱에서 **async** 키워드 및 **await** 연산자를 사용해야 합니다. 규칙에 따라 비동기 메서드는 **Async**로 끝납니다.

## <a name="configure-your-project"></a>프로젝트 구성

시작하려면 개발 컴퓨터에 Microsoft Store Services SDK를 설치하고 필수 참조를 프로젝트에 추가합니다.

1. [Microsoft Store Services SDK를 설치합니다.](microsoft-store-services-sdk.md#install-the-sdk)
2. Visual Studio에서 프로젝트를 엽니다.
3. 솔루션 탐색기에서 프로젝트 노드를 확장하여 **참조**를 마우스 오른쪽 단추로 클릭하고 **참조 추가**를 클릭합니다.
3. **참조 관리자**에서 **유니버설 Windows**를 확장하고 **확장**을 클릭합니다.
4. SDK 목록에서 **Microsoft Engagement Framework**(Microsoft 참여 프레임워크) 옆의 확인란을 선택하고 **확인**을 클릭합니다.

> [!NOTE]
> 이 문서의 코드 예제에서는 코드 파일 **System.Threading.Tasks** 및 **Microsoft.Services.Store.Engagement** 네임 스페이스에 대 한 **using** 문을 있다고 가정 합니다.

## <a name="get-variation-data-and-log-the-view-event-for-your-experiment"></a>변형 데이터를 가져와 실험에 대한 보기 이벤트 기록

프로젝트에서 실험에서 수정할 기능에 대한 코드를 찾습니다. 변형에 대 한 데이터를 검색 하는 코드를 추가 하 고이 데이터를 사용 하 여는 테스트할 기능의 동작을 수정한 다음 a 실험에 대 한 보기 이벤트를 기록 / B 테스트 서비스 파트너 센터에 있습니다.

필요한 특정 코드는 앱에 따라 달라지긴 하지만 다음 예제에서는 기본 프로세스를 보여 줍니다. 전체 코드 예제를 보려면 [A/B 테스트로 첫 번째 실험 만들기 및 실행](create-and-run-your-first-experiment-with-a-b-testing.md)을 참조하세요.

[!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#ExperimentCodeSample)]

다음 단계에서는 이 프로세스에서 중요한 부분을 자세히 설명합니다.

1. 현재 변형 할당을 나타내는 [StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) 개체 및 파트너 센터에 보기 및 변환 이벤트를 기록 하는 데 사용할 [StoreServicesCustomEventLogger](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger) 개체를 선언 합니다.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet1)]

2. 검색할 실험의 [프로젝트 ID](run-app-experiments-with-a-b-testing.md#terms)에 할당되는 문자열 변수를 선언합니다.
    > [!NOTE]
    > 프로젝트를 가져올 때 ID [파트너 센터에서 프로젝트를 만듭니다](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md). 아래 표시된 프로젝트 ID는 예제용으로만 사용됩니다.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet2)]

3. 정적 [GetCachedVariationAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getcachedvariationasync) 메서드를 호출하여 실험에 대해 현재 캐시된 변형 할당을 가져오고 실험의 프로젝트 ID를 메서드에 전달합니다. 이 메서드는 [ExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariationresult.experimentvariation) 속성을 통해 변형 할당에 액세스할 수 있는 [StoreServicesExperimentVariationResult](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariationresult) 개체를 반환합니다.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet3)]

4. [IsStale](htthttps://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.isstale) 속성을 검사하여 캐시된 변형 할당을 서버의 원격 변형 할당으로 새로 고쳐야 할지 여부를 확인합니다. 새로 고쳐야 하는 경우 서버에서 업데이트된 변형 할당을 확인하는 정적 [GetRefreshedVariationAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getrefreshedvariationasync) 메서드를 호출하고 로컬에 캐시된 변형을 새로 고칩니다.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet4)]

5. [StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) 개체의 [GetBoolean](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getboolean), [GetDouble](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getdouble), [GetInt32](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getint32) 또는 [GetString](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring) 메서드를 사용하여 변형 할당 값을 가져올 수 있습니다. 각 메서드에서 첫 번째 매개 변수는 검색할 변형의 이름 (이 크기는 파트너 센터에 입력 하는 변형 동일한 이름). 두 번째 매개 변수 (예를 들어, 네트워크에 연결 되지 않은 경우), 파트너 센터에서 지정 된 값을 가져올 수 없는 경우 메서드에서 반환 하는 기본 값 이며 캐시 된 변형의 버전을 사용할 수 없는.

    다음 예제에서는 [GetString](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring)을 사용하여 *buttonText*라는 변수를 가져오고 기본 변수 값인 **Grey Button**을 지정합니다.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet5)]

6. 코드에서 변수 값을 사용하여 테스트할 기능의 동작을 수정합니다. 예를 들어 다음 코드는 앱에서 *buttonText* 값을 단추의 콘텐츠에 할당합니다. 이 예제에서는 프로젝트의 다른 위치에서 이 단추를 이미 정의했다고 가정합니다.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet6)]

7. 마지막으로, a 실험에 대 한 [보기 이벤트](run-app-experiments-with-a-b-testing.md#terms) 를 기록 / B 테스트 서비스 파트너 센터에 있습니다. ```logger``` 필드를 [StoreServicesCustomEventLogger](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger) 개체로 초기화하고 [LogForVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation) 메서드를 호출합니다. 현재 변형 할당 (이 개체 파트너 센터에 이벤트에 대 한 컨텍스트를 제공 하는 데 사용)을 나타내는 [StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) 개체를 전달 하 고 실험에 대 한 보기 이벤트의 이름입니다. 이 파트너 센터에서 실험에 대 한 입력 하는 보기 이벤트 이름과 일치 해야 합니다. 사용자가 실험의 일부인 변형을 보기 시작하는 경우 코드에 보기 이벤트를 기록해야 합니다.

    다음 예제에서는 이름이 **userViewedButton**인 보기 이벤트를 기록하는 방법을 보여 줍니다. 이 예제에서 실험 목표는 사용자가 앱에서 단추를 클릭하여 앱이 변형 데이터(이 경우 단추 텍스트)를 검색하고 해당 데이터를 단추 콘텐츠에 할당한 후에 보기 이벤트가 기록되도록 하는 것입니다.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet7)]

## <a name="log-conversion-events-to-partner-center"></a>파트너 센터에 변환 이벤트 기록

다음으로 [전환 이벤트](run-app-experiments-with-a-b-testing.md#terms) 를 기록 하는 코드를 추가 / B 테스트 서비스 파트너 센터에 있습니다. 사용자가 실험에 대한 목표를 이루었을 때 코드에 변환 이벤트를 기록해야 합니다. 필요한 특정 코드는 앱에 따라 달라지지만 일반적인 단계는 다음과 같습니다. 전체 코드 예제를 보려면 [A/B 테스트로 첫 번째 실험 만들기 및 실행](create-and-run-your-first-experiment-with-a-b-testing.md)을 참조하세요.

1. 사용자가 실험 목표 중 하나에 도달할 때 실행되는 코드에서 [LogForVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation) 메서드를 다시 호출하고 실험의 전환 이벤트 이름과 [StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) 개체를 전달합니다. 이 파트너 센터에서 실험에 대 한 입력 변환 이벤트 이름과 일치 해야 합니다.

    다음 예제에서는 단추에 대한 **Click** 이벤트 처리기에서 이름이 **userClickedButton**인 전환 이벤트를 기록합니다. 이 예제에서 실험 목표는 사용자가 단추를 클릭하도록 하는 것입니다.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet8)]

## <a name="next-steps"></a>다음 단계

앱에서 실험을 코딩하면 다음 단계에 대한 준비가 완료된 것입니다.
1. [파트너 센터에서 실험을 정의](define-your-experiment-in-the-dev-center-dashboard.md)합니다. 이벤트 보기, 변환 이벤트 및 A/B 테스트용 고유한 변형을 정의하는 실험을 만듭니다.
2. [실행 하 고 파트너 센터에서 실험을 관리](manage-your-experiment.md)합니다.


## <a name="related-topics"></a>관련 항목

* [프로젝트 만들기 및 파트너 센터에서 원격 변수 정의](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [파트너 센터에서 실험 정의](define-your-experiment-in-the-dev-center-dashboard.md)
* [파트너 센터에서 실험 관리](manage-your-experiment.md)
* [A/B 테스트로 첫 번째 실험 만들기 및 실행](create-and-run-your-first-experiment-with-a-b-testing.md)
* [A/B 테스트로 앱 실험 실행](run-app-experiments-with-a-b-testing.md)
