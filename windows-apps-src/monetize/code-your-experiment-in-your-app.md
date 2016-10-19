---
author: mcleanbyron
Description: "UWP(유니버설 Windows 플랫폼) 앱에서 A/B 테스트로 실험을 실행하려면 앱에서 실험을 코딩해야 합니다."
title: "실험용 앱 코딩"
ms.assetid: 6A5063E1-28CD-4087-A4FA-FBB511E9CED5
translationtype: Human Translation
ms.sourcegitcommit: 29a94fd14d11256ade28463c04abfec81287cf39
ms.openlocfilehash: e5de32dcc7b0694e72d9686b3b9a64de17a02277

---

# 실험용 앱 코딩

[개발자 센터 대시보드에서 프로젝트를 만들고 원격 변수를 정의](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)하면 다음 작업에 대해 UWP(유니버설 Windows 플랫폼) 앱의 코드를 업데이트할 준비가 된 것입니다.
* Windows 개발자 센터에서 원격 변수 값 수신
* 원격 변수를 사용하여 사용자를 위한 앱 환경 구성
* 사용자가 실험을 보고 원하는 작업(*변환*이라고도 함)을 수행한 때를 나타내는 이벤트를 개발자 센터에 기록

다음 섹션에서는 실험에 대한 변형을 가져오고 이벤트를 개발자 센터에 기록하는 일반적인 프로세스를 설명합니다. 실험용 앱을 코딩한 후 [개발자 센터 대시보드에서 실험을 정의](define-your-experiment-in-the-dev-center-dashboard.md)할 수 있습니다. 실험 만들기 및 실행의 종단 간 프로세스를 보여 주는 연습에 대한 자세한 내용은 [A/B 테스트로 첫 번째 실험 만들기 및 실행](create-and-run-your-first-experiment-with-a-b-testing.md)을 참조하세요.

## 프로젝트 구성

시작하려면 개발 컴퓨터에 Microsoft Store Services SDK를 설치하고 필수 참조를 프로젝트에 추가합니다.

1. [Microsoft Store Services SDK](http://aka.ms/store-em-sdk)를 설치합니다.
2. Visual Studio에서 프로젝트를 엽니다.
3. 솔루션 탐색기에서 프로젝트 노드를 확장하여 **참조**를 마우스 오른쪽 단추로 클릭하고 **참조 추가**를 클릭합니다.
3. **참조 관리자**에서 **유니버설 Windows**를 확장하고 **확장**을 클릭합니다.
4. SDK 목록에서 **Microsoft Engagement Framework**(Microsoft 참여 프레임워크) 옆의 확인란을 선택하고 **확인**을 클릭합니다.

## 변형 데이터를 가져오는 코드 추가

프로젝트에서 실험에서 수정할 기능에 대한 코드를 찾습니다. 변형에 대한 데이터를 검색하는 코드를 추가하고 이 데이터를 사용하여 테스트할 기능의 동작을 수정합니다. 필요한 특정 코드는 앱에 따라 달라지지만 일반적인 단계는 다음과 같습니다. 전체 코드 예제를 보려면 [A/B 테스트로 첫 번째 실험 만들기 및 실행](create-and-run-your-first-experiment-with-a-b-testing.md)을 참조하세요.

1. 현재 변형 할당을 나타내는 [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) 개체와 개발자 센터에 보기 및 변환 이벤트를 기록하는 데 사용할 [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.aspx) 개체를 선언합니다.
```CSharp
private StoreServicesExperimentVariation variation;
private StoreServicesCustomEventLogger logger;
```

2. 정적 [GetCachedVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getcachedvariationasync.aspx) 메서드를 호출하여 실험에 대한 현재 캐시된 변형 할당을 가져오고 실험에 대한 [프로젝트 ID](run-app-experiments-with-a-b-testing.md#terms)를 메서드에 전달합니다. 이 메서드는 [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariationresult.experimentvariation.aspx) 속성을 통해 변형 할당에 액세스할 수 있는 [StoreServicesExperimentVariationResult](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariationresult.aspx) 개체를 반환합니다.
  >**참고**&nbsp;&nbsp;[개발자 센터 대시보드에서 프로젝트를 만들](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md) 때 프로젝트 ID를 가져옵니다. 아래 표시된 프로젝트 ID는 예제용으로만 사용합니다.

  ```CSharp
var result = await StoreServicesExperimentVariation.GetCachedVariationAsync(
      "F48AC670-4472-4387-AB7D-D65B095153FB");
variation = result.ExperimentVariation;
```

4. [IsStale](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.isstale.aspx) 속성을 확인하여 캐시된 변형 할당이 서버에서 원격 변형 할당을 사용하여 새로 고쳐져야 할지 여부를 결정합니다. 새로 고칠 필요가 없는 경우 서버에서 업데이트된 변형 할당을 확인하는 정적 [GetRefreshedVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getrefreshedvariationasync.aspx) 메서드를 호출하고 로컬 캐시된 변형을 새로 고칩니다.
```CSharp
// Check whether the cached variation assignment needs to be refreshed.
// If so, then refresh it.
if (result.ErrorCode != StoreServicesEngagementErrorCode.None || result.ExperimentVariation.IsStale)
{
      result = await StoreServicesExperimentVariation.GetRefreshedVariationAsync(projectId);

      // If the call succeeds, use the new result. Otherwise, use the cached value.
      if (result.ErrorCode == StoreServicesEngagementErrorCode.None)
      {
          variation = result.ExperimentVariation;
      }
}
```

5. [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) 개체의 [GetBoolean](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getboolean.aspx), [GetDouble](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getdouble.aspx), [GetInt32](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getint32.aspx) 또는 [GetString](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring.aspx)의 메서드를 사용하여 변형 할당에 대한 설정을 가져옵니다. 각 메서드에서 첫 번째 매개 변수는 검색할 변형의 이름(개발자 센터 대시보드에서 입력하는 변형과 동일한 이름)입니다. 두 번째 매개 변수는 개발자 센터에서 지정된 값을 검색할 수 없고(예: 네트워크에 연결되지 않은 경우) 캐시된 변형의 버전을 사용할 수 없는 경우 메서드에서 반환하는 기본값입니다.

  다음 예제에서는 [GetString](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring.aspx)을 사용하여 이름이 *buttonText*인 설정을 가져오고 **Grey Button**의 기본 변수 값을 지정합니다.
```CSharp
var buttonText = variation.GetString("buttonText", "Grey Button");
```
4. 코드에서 변수 값을 사용하여 테스트할 기능의 동작을 수정합니다. 예를 들어 다음 코드는 *buttonText* 값을 단추의 콘텐츠에 할당합니다.
```CSharp
button.Content = buttonText;
```

## 개발자 센터에 보기 및 전환 이벤트를 기록하는 코드 추가

그런 다음 개발자 센터의 A/B 테스트 서비스에 [보기 이벤트](run-app-experiments-with-a-b-testing.md#terms) 및 [전환 이벤트](run-app-experiments-with-a-b-testing.md#terms)를 기록하는 코드를 추가합니다. 코드는 사용자가 실험의 일부인 변형을 보기 시작할 때 보기 이벤트를 기록하고 사용자가 실험의 목표에 도달할 때 전환 이벤트를 기록합니다.

필요한 특정 코드는 앱에 따라 달라지지만 일반적인 단계는 다음과 같습니다. 전체 코드 예제를 보려면 [A/B 테스트로 첫 번째 실험 만들기 및 실행](create-and-run-your-first-experiment-with-a-b-testing.md)을 참조하세요.

1. 사용자가 변형 보기를 시작할 때 실행되는 코드에서 ```logger``` 필드를 [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.aspx) 개체로 초기화하고 [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx) 메서드를 호출합니다. 현재 변형 할당을 나타내는 [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) 개체(이 개체는 개발자 센터에 이벤트에 대한 컨텍스트를 제공함) 및 보기 이벤트의 이름(개발자 센터 대시보드에서 입력하는 보기 이벤트와 동일한 이름)을 전달합니다. 다음 예제에서는 이름이 **userViewedButton**인 보기 이벤트를 기록합니다.

  ```CSharp
  if (logger == null)
  {
      logger = StoreServicesCustomEventLogger.GetDefault();
  }

  logger.LogForVariation(variation, "userViewedButton");
  ```

2. 사용자가 실험 목표 중 하나에 대한 목표에 도달할 때 실행되는 코드에서 [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx) 메서드를 다시 호출하고 [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) 개체 및 변환 이벤트의 이름(개발자 센터 대시보드에서 입력하는 변환 이벤트와 동일한 이름)을 호출합니다. 다음 예제에서는 이름이 **userClickedButton**인 전환 이벤트를 기록합니다.
```CSharp
logger.LogForVariation(variation, "userClickedButton");
```

## 다음 단계

앱에서 실험을 코딩하면 다음 단계에 대한 준비가 완료된 것입니다.
1. [개발자 센터 대시보드에서 실험을 정의](define-your-experiment-in-the-dev-center-dashboard.md)합니다. 이벤트 보기, 변환 이벤트 및 A/B 테스트용 고유한 변형을 정의하는 실험을 만듭니다.
2. [개발자 센터 대시보드에서 실험을 실행하고 관리합니다](manage-your-experiment.md).


## 관련 항목

* [개발자 센터 대시보드에서 프로젝트 만들기 및 원격 변수 정의](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [개발자 센터 대시보드에서 실험 정의](define-your-experiment-in-the-dev-center-dashboard.md)
* [개발자 센터 대시보드에서 실험 관리](manage-your-experiment.md)
* [A/B 테스트로 첫 번째 실험 만들기 및 실행](create-and-run-your-first-experiment-with-a-b-testing.md)
* [A/B 테스트로 앱 실험 실행](run-app-experiments-with-a-b-testing.md)



<!--HONumber=Sep16_HO1-->


