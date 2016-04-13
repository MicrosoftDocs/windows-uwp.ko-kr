---
Description: 개발자 센터 대시보드에서 실험을 정의하면 앱에 실험을 코딩할 준비가 된 것입니다.
title: 실험용 앱 코딩
ms.assetid: 6A5063E1-28CD-4087-A4FA-FBB511E9CED5
---

# 실험용 앱 코딩

[개발자 센터 대시보드에서 실험을 정의](define-your-experiment-in-the-dev-center-dashboard.md)하면 실험에 대한 변형 데이터를 가져오고, 이 데이터를 사용하여 테스트할 기능의 동작을 수정하고, 보기 이벤트 및 전환 이벤트를 개발자 센터에 기록하도록 UWP(유니버설 Windows 플랫폼) 앱의 코드를 업데이트할 준비가 된 것입니다.

다음 섹션에서는 실험에 대한 변형을 가져오고 이벤트를 개발자 센터에 기록하는 일반적인 프로세스를 설명합니다. 실험 만들기 및 실행의 종단 간 프로세스를 보여 주는 연습에 대한 자세한 내용은 [A/B 테스트로 첫 번째 실험 만들기 및 실행](create-and-run-your-first-experiment-with-a-b-testing.md)을 참조하세요.

## 프로젝트 구성

시작하려면 개발 컴퓨터에 Microsoft 스토어 참여 및 수익 창출 SDK를 설치하고 필수 참조를 프로젝트에 추가합니다.

1. [Microsoft 스토어 참여 및 수익 창출 SDK](http://aka.ms/store-em-sdk)를 설치합니다.
2. Visual Studio에서 프로젝트를 엽니다.
3. 솔루션 탐색기에서 프로젝트 노드를 확장하여 **참조**를 마우스 오른쪽 단추로 클릭하고 **참조 추가**를 클릭합니다.
3. **참조 관리자**에서 **유니버설 Windows**를 확장하고 **확장**을 클릭합니다.
4. SDK 목록에서 **Microsoft 스토어 참여 SDK** 옆의 확인란을 선택하고 **확인**을 클릭합니다.

## 변형 설정을 가져오는 코드 추가

프로젝트에서 실험에서 수정할 기능에 대한 코드를 찾습니다. 변형에 대한 설정을 검색하는 코드를 추가하고 이 데이터를 사용하여 테스트할 기능의 동작을 수정합니다. 필요한 특정 코드는 앱에 따라 달라지지만 일반적인 단계는 다음과 같습니다. 전체 코드 예제를 보려면 [A/B 테스트로 첫 번째 실험 만들기 및 실행](create-and-run-your-first-experiment-with-a-b-testing.md)을 참조하세요.

1. 실험에 대한 변형을 검색하는 데 사용할 [ExperimentClient](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentclient.aspx) 개체 및 현재 변형 할당을 나타내는 [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.aspx) 개체를 선언합니다.
```
private readonly ExperimentClient experiment;
private ExperimentVariation variation;
```

2. [ExperimentClient](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentclient.aspx) 개체를 초기화하고 대시보드의 **Experiments** 페이지에서 검색한 API 키를 생성자에게 전달합니다. API 키에 대한 자세한 내용은 [개발자 센터 대시보드에서 실험 정의](define-your-experiment-in-the-dev-center-dashboard.md#generate-an-api-key)를 참조하세요. 아래에 표시되는 API 키는 예제용으로만 사용됩니다.
```
experiment = new ExperimentClient("F48AC670-4472-4387-AB7D-D65B095153FB");
```

3. [GetVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentclient.getvariationasync.aspx) 메서드를 호출하여 실험에 대해 현재 캐시된 변형 할당을 가져옵니다. 이 메서드는 [Variation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariationresult.variation.aspx) 속성을 통해 변형 할당에 액세스할 수 있는 [ExperimentVariationResult](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariationresult.aspx) 개체를 반환합니다.
```
ExperimentVariationResult result = await experiment.GetVariationAsync();
variation = result.Variation;
```

4. 캐시된 변형 할당을 새로 고칠지 여부를 결정하는 [NeedsRefresh](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.needsrefresh.aspx) 속성을 확인합니다. 새로 고칠 필요가 없는 경우 서버에서 업데이트된 변형 할당을 확인하는 [RefreshVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentclient.refreshvariationasync.aspx) 메서드를 호출하고 로컬 캐시된 변형을 새로 고칩니다.
```
// Check whether the cached variation assignment needs to be refreshed.
// If so, then refresh it.
if (result.ErrorCode != EngagementErrorCode.Success || result.Variation.NeedsRefresh)
{
      result = await experiment.RefreshVariationAsync();

      // If the call succeeds, use the new result. Otherwise, use the
      // cached value we retrieved earlier.
      if (result.ErrorCode == EngagementErrorCode.Success)
      {
          variation = result.Variation;
      }
}
```

5. [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.aspx) 개체의 [GetBoolean](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getboolean.aspx), [GetDouble](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getdouble.aspx), [GetInteger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getinteger.aspx) 또는 [GetString](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getstring.aspx)의 메서드를 사용하여 변형 할당에 대한 설정을 가져옵니다. 각 메서드에서 첫 번째 매개 변수는 검색할 설정의 이름(개발자 센터 대시보드에서 입력한 이름)입니다. 두 번째 매개 변수는 개발자 센터에서 지정된 값을 검색할 수 없고(예: 네트워크에 연결되지 않은 경우) 캐시된 변형의 버전을 사용할 수 없는 경우 메서드에서 반환하는 기본값입니다.

  다음 예제에서는 [GetString](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getstring.aspx)을 사용하여 이름이 *buttonText*인 설정을 가져오고 **Grey Button**의 기본 설정 값을 지정합니다.
```
var buttonText = currentVariation.GetString("buttonText", "Grey Button");
```
4. 코드에서 설정 값을 사용하여 테스트할 기능의 동작을 수정합니다. 예를 들어 다음 코드는 *buttonText* 값을 단추의 콘텐츠에 할당합니다.
```
button.Content = buttonText;
```

## 개발자 센터에 보기 및 전환 이벤트를 기록하는 코드 추가

그런 다음 개발자 센터의 A/B 테스트 서비스에 보기 및 전환 이벤트를 기록하는 코드를 추가합니다. 코드는 사용자가 실험의 일부인 변형을 보기 시작할 때 보기 이벤트를 기록하고 사용자가 실험의 목표에 도달할 때 전환 이벤트를 기록합니다.

필요한 특정 코드는 앱에 따라 달라지지만 일반적인 단계는 다음과 같습니다. 전체 코드 예제를 보려면 [A/B 테스트로 첫 번째 실험 만들기 및 실행](create-and-run-your-first-experiment-with-a-b-testing.md)을 참조하세요.

1. 사용자가 변형을 보기 시작할 때 실행되는 코드에서 [StoreServicesCustomEvents](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomevents.aspx) 개체의 정적 [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomevents.log.aspx) 메서드를 호출합니다. 개발자 센터 대시보드에서 실험에 정의한 보기 이벤트의 이름과 현재 변형 할당을 나타내는 [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.aspx) 개체(이 개체는 개발자 센터에 이벤트에 대한 컨텍스트 제공)를 전달합니다. 다음 예제에서는 이름이 **userViewedButton**인 보기 이벤트를 기록합니다.
```
StoreServicesCustomEvents.Log("userViewedButton", variation);
```
2. 사용자가 실험 목표 중 하나에 도달할 때 실행되는 코드에서 [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomevents.log.aspx) 메서드를 다시 호출하고 실험에서 정의한 전환 이벤트의 이름과 [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.aspx) 개체를 전달합니다. 다음 예제에서는 이름이 **userClickedButton**인 전환 이벤트를 기록합니다.
```
StoreServicesCustomEvents.Log("userClickedButton", variation);
```

## 다음 단계

개발자 센터 대시보드에서 실험을 정의하고 앱에 실험을 코딩하면 [개발자 센터 대시보드에서 실험을 실행하고 관리](manage-your-experiment.md)할 준비가 된 것입니다.

## 관련 항목

  * [개발자 센터 대시보드에서 실험 정의](define-your-experiment-in-the-dev-center-dashboard.md)
  * [개발자 센터 대시보드에서 실험 관리](manage-your-experiment.md)
  * [A/B 테스트로 첫 번째 실험 만들기 및 실행](create-and-run-your-first-experiment-with-a-b-testing.md)
  * [A/B 테스트로 앱 실험 실행](run-app-experiments-with-a-b-testing.md)


<!--HONumber=Mar16_HO5-->


