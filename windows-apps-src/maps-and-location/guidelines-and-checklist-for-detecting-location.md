---
author: msatranjr
Description: "이 항목에서는 사용자 위치에 액세스해야 하는 앱에 대한 성능 지침에 대해 설명합니다."
title: "위치 인식 앱에 대한 지침"
ms.assetid: 16294DD6-5D12-4062-850A-DB5837696B4D
ms.sourcegitcommit: 92285ce32548bd6035c105e35c2b152432f8575a
ms.openlocfilehash: 6a5451d449719d979bce7e83f5a2949661dd7834

---

# 위치 인식 앱에 대한 지침


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


**중요 API**

-   [**지리적 위치**](https://msdn.microsoft.com/library/windows/apps/br225603)
-   [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534)

이 항목에서는 사용자 위치에 액세스해야 하는 앱에 대한 성능 지침에 대해 설명합니다.

## 권장 사항


-   앱에 위치 데이터가 필요한 경우에만 위치 개체 사용을 시작합니다.

    사용자의 위치에 액세스하기 전에 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152)을(를) 호출합니다. 이때 앱이 포그라운드에 있어야 하고 **RequestAccessAsync**이(가) UI 스레드에서 호출되어야 합니다. 사용자가 자신의 위치에 대한 권한을 앱에 부여하기 전에는 앱이 위치 데이터에 액세스할 수 없습니다.

-   위치가 앱에 꼭 필요한 것이 아니면 사용자가 위치가 필요한 작업을 완료할 때까지 장치에 액세스하지 마세요. 예를 들어 소셜 네트워킹 앱에 "Check in with my location(내 위치 확인)" 단추가 있는 경우 사용자가 단추를 클릭하기 전에는 앱에서 위치에 액세스하지 않습니다. 앱의 주요 기능에 대해 필요한 경우 위치에 즉시 액세스할 수 있습니다.

-   동의 확인 프롬프트를 사용자에게 트리거하기 위해 [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) 개체를 포그라운드 앱의 기본 UI 스레드에서 처음 사용해야 합니다. **Geolocator**를 처음 사용할 때 [**getGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536)를 처음 호출하거나 [**positionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540) 이벤트에 대한 처리기를 처음으로 등록할 수 있습니다.

-   위치 데이터 사용 방법을 알립니다.
-   사용자가 자신의 위치를 수동으로 새로 고칠 수 있도록 UI를 제공합니다.
-   위치 데이터를 가져올 때까지 기다리는 동안 진행률 표시줄을 표시합니다. <!--For info on the available progress controls and how to use them, see [**Guidelines for progress controls**](guidelines-and-checklist-for-progress-controls.md).-->
-   위치 서비스를 사용하지 않거나 사용할 수 없는 경우 적절한 오류 메시지 또는 대화 상자를 표시합니다.

    위치 설정에 따라 앱이 사용자의 위치에 액세스할 수 없는 경우 **설정** 앱에서 **위치 개인 정보 설정**으로 편리한 링크를 제공하는 것이 좋습니다. 예를 들어 하이퍼링크 컨트롤을 사용하거나 [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) 메서드를 호출하여 `ms-settings:privacy-location` URI를 통해 코드에서 **설정** 앱을 시작할 수 있습니다. 자세한 내용은 [Windows 설정 앱 실행](https://msdn.microsoft.com/library/windows/apps/mt228342)을 참조하세요.

-   사용자가 위치 정보에 대한 액세스를 사용할 수 없게 설정하면 캐시된 위치 데이터를 지우고 [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534)를 해제합니다.

    사용자가 설정을 통해 위치 정보에 대한 액세스를 해제할 경우 [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) 개체를 해제합니다. 앱은 위치 API 호출에 대한 **ACCESS\_DENIED** 결과를 받게 됩니다. 앱이 위치 데이터를 저장하거나 캐시할 경우 사용자가 위치 정보에 대한 액세스를 취소하면 캐시된 데이터를 지웁니다. 위치 서비스를 통해 위치 데이터를 사용할 수 없을 때 위치를 수동으로 입력하는 대체 방법을 제공합니다.

-   위치 서비스를 다시 사용하도록 설정할 수 있는 UI를 제공합니다. 예를 들어 [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) 개체를 다시 인스턴스화하고 위치 정보를 다시 가져오려고 시도하는 새로 고침 단추를 제공합니다.

    앱에서 위치 서비스를 다시 사용하도록 설정할 수 있는 UI를 제공합니다.

    -   사용자가 위치 액세스를 사용하지 않도록 설정한 후 다시 사용하도록 설정할 경우 앱에는 알림이 표시되지 않습니다. [
            **status**](https://msdn.microsoft.com/library/windows/apps/br225601) 속성이 변경되지 않고 [**statusChanged**](https://msdn.microsoft.com/library/windows/apps/br225542) 이벤트도 발생하지 않습니다. 앱은 새 [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) 개체를 만들고 [**getGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536)를 호출하여 업데이트된 위치 데이터를 가져오거나 [**positionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540) 이벤트를 다시 구독해야 합니다. 그런 다음 상태가 위치를 다시 사용하도록 다시 설정되었다고 표시되는 경우 이전에 위치 서비스를 사용할 수 없다고 사용자에게 알리기 위해 표시한 UI를 지우고 새 상태에 적절하게 응답합니다.
    -   앱은 또한 활성화 시, 사용자가 위치 정보가 필요한 기능을 명시적으로 사용하려고 할 경우 또는 다른 시나리오에서 위치 데이터를 다시 가져오려고 시도해야 합니다.

**성능**

-   앱에서 위치 업데이트를 받을 필요가 없는 경우 일회성 위치 요청을 사용합니다. 예를 들어 사진에 위치 태그를 추가하는 앱은 위치 업데이트 이벤트를 받을 필요가 없습니다. 대신 [현재 위치 가져오기](https://msdn.microsoft.com/library/windows/apps/mt219698)에서 설명한 대로 [**getGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536)를 사용해 위치를 요청해야 합니다.

    일회성 위치 요청을 생성하는 경우 다음 값을 설정해야 합니다.

    -   [
            **DesiredAccuracy**](https://msdn.microsoft.com/library/windows/apps/br225535) 또는 [**DesiredAccuracyInMeters**](https://msdn.microsoft.com/library/windows/apps/jj635271)를 설정하여 앱에서 요청하는 정확성을 지정합니다. 이러한 매개 변수 사용에 대한 권장 사항은 다음을 참조하세요.
    -   [
            **GetGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536)의 max age 매개 변수를 설정하여 앱에 유용한 위치 정보를 얻을 수 있는 과거 기간을 지정합니다. 앱에서 몇 초 또는 몇 분 전의 위치를 사용할 수 있으면 앱은 거의 즉시 위치를 수신할 수 있으므로 장치 전원이 절약됩니다.
    -   [
            **GetGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536)의 timeout 매개 변수를 설정합니다. 위치 또는 오류가 반환될 때까지 앱이 대기할 수 있는 기간입니다. 사용자에 대한 응답성과 앱에 필요한 정확성 사이의 절충점을 찾아야 합니다.
-   위치를 자주 업데이트해야 하는 경우에는 연속 위치 세션을 사용합니다. 특정 임계값을 초과하는 이동을 검색하거나 위치 업데이트가 지속적으로 수행되는 경우 [**positionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540) 및 [**statusChanged**](https://msdn.microsoft.com/library/windows/apps/br225542) 이벤트를 사용합니다.

    위치 업데이트를 요청할 때 [**DesiredAccuracy**](https://msdn.microsoft.com/library/windows/apps/br225535) 또는 [**DesiredAccuracyInMeters**](https://msdn.microsoft.com/library/windows/apps/jj635271)를 설정하여 앱에서 요청하는 정확성을 지정할 수 있습니다. 또한 [**MovementThreshold**](https://msdn.microsoft.com/library/windows/apps/br225539) 또는 [**ReportInterval**](https://msdn.microsoft.com/library/windows/apps/br225541)을 사용하여 위치 업데이트가 필요한 빈도를 설정해야 합니다.

    -   이동 임계값을 지정합니다. 일부 앱에서는 사용자가 멀리 이동한 경우에만 위치 업데이트가 필요합니다. 예를 들어 지역 뉴스나 날씨 업데이트를 제공하는 앱은 사용자 위치가 다른 도시로 변경되지 않는 한 위치 업데이트가 필요하지 않을 수 있습니다. 이 경우 [**MovementThreshold**](https://msdn.microsoft.com/library/windows/apps/br225539) 속성을 설정하여 위치 업데이트 이벤트에 대해 최소 필요 이동을 조정합니다. [
            **PositionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540) 이벤트를 필터링하는 효과가 있습니다. 이러한 이벤트는 위치 변경이 이동 임계값을 초과하는 경우에만 발생합니다.

    -   앱 환경에 맞춰지고 시스템 리소스 사용을 최소화하는 [**reportInterval**](https://msdn.microsoft.com/library/windows/apps/br225541)을 사용합니다. 예를 들어 날씨 앱은 15분 간격으로 데이터 업데이트가 필요할 수 있습니다. 실시간 내비게이션 앱이 아닌 대부분의 앱에는 매우 정확하고 지속적인 위치 업데이트 스트림이 필요하지 않습니다. 가장 정확한 데이터 스트림이 필요하지 않거나 자주 업데이트할 필요가 없는 앱에서는 **ReportInterval** 속성을 설정하여 앱에 필요한 위치 업데이트의 최소 빈도를 표시합니다. 그러면 위치 소스에서 필요할 때만 위치를 계산함으로써 전기를 절약할 수 있습니다.

        실시간 데이터가 필요한 앱은 [**ReportInterval**](https://msdn.microsoft.com/library/windows/apps/br225541)을 0으로 설정하여 최소 간격이 지정되지 않았음을 나타냅니다. 기본 보고서 간격은 1초와 하드웨어에서 지원할 수 있는 간격 중 더 짧은 간격입니다.

        위치 데이터를 제공하는 장치는 서로 다른 앱에서 요청하는 보고서 간격을 추적하고, 최소 요청 간격으로 데이터 보고서를 제공합니다. 그러면 정확성에 대한 요구가 가장 큰 앱이 필요한 데이터를 받을 수 있습니다. 따라서 다른 앱이 업데이트를 더 자주 요청한 경우, 위치 제공자가 앱에서 요청한 것보다 더 높은 빈도로 업데이트를 생성할 수 있습니다.

        **참고** 위치 소스가 지정된 보고서 간격에 대한 요청을 지킬 것인지는 보장되지 않습니다. 모든 위치 제공자 장치가 보고서 간격을 추적하는 것은 아니지만, 추적하는 장치를 위해 여전히 정보를 제공해야 합니다.

    -   전기를 절약하려면 앱에 매우 정확한 데이터가 필요한지 여부를 위치 플랫폼에 알리기 위해 [**desiredAccuracy**](https://msdn.microsoft.com/library/windows/apps/br225535) 속성을 설정해야 합니다. 매우 정확한 데이터를 요구하는 앱이 없으면 GPS 공급자를 켜지 않음으로써 시스템에서 전기를 절약할 수 있습니다.

        -   [
            **desiredAccuracy**](https://msdn.microsoft.com/library/windows/apps/br225535)를 **HIGH**로 설정하여 GPS가 데이터를 구입할 수 있도록 합니다.
        -   앱에서 광고 목적으로만 위치 정보를 사용하는 경우 [**desiredAccuracy**](https://msdn.microsoft.com/library/windows/apps/br225535)를 **Default**로 설정하고 일회성 통화 패턴을 사용하여 전력 소비를 최소화합니다.

        앱에 특히 정확성이 필요한 경우 [**DesiredAccuracy**](https://msdn.microsoft.com/library/windows/apps/br225535)가 아니라 [**DesiredAccuracyInMeters**](https://msdn.microsoft.com/library/windows/apps/jj635271) 속성을 사용할 수 있습니다. 이는 대개 셀룰러 오류 신호, Wi-Fi 오류 신호 및 위성을 기반으로 위치를 얻을 수 있는 Windows Phone에서 특히 유용합니다. 더욱 구체적인 정확성 값을 선택하면 시스템이 위치를 제공할 때 가장 낮은 전원 비용으로 사용할 적합한 기술을 식별하는 데 도움이 됩니다.

        예를 들면 다음과 같습니다.

        -   앱이 광고 튜닝, 날씨, 뉴스 등에 대한 위치를 얻고 있는 경우 일반적으로 5000미터 정확성이면 충분합니다.
        -   앱이 이웃의 인근 거래를 표시하고 있는 경우 일반적으로 300미터 정확성이 결과를 제공하는 데 적합합니다.
        -   사용자가 인근 음식점에 대한 추천을 찾고 있는 경우 블록 내에서 위치를 얻으려고 하므로 100미터 정확성이 충분합니다.
        -   사용자가 자신의 위치를 공유하려고 하는 경우 앱은 약 10미터 정확성을 요청해야 합니다.
    -   앱에 특정 정확성 요구 사항이 있는 경우 [**Geocoordinate.accuracy**](https://msdn.microsoft.com/library/windows/apps/br225526) 속성을 사용하세요. 예를 들어 내비게이션 앱은 사용 가능한 위치 데이터가 앱의 요구 사항을 충족하는지 확인하기 위해 **Geocoordinate.accuracy** 속성을 사용해야 합니다.

-   시작 지연을 고려합니다. 앱이 위치 데이터를 처음 요청하면 위치 제공자가 시작되는 동안 1-2초 정도 짧은 지연이 있을 수 있습니다. 앱 UI 디자인에서 이를 고려하세요. 예를 들면 [**GetGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536)에 대한 호출 완료를 보류하는 다른 작업의 차단을 피하고자 할 수 있습니다.

-   백그라운드 동작을 고려합니다. 앱에 포커스가 없는 경우, 백그라운드에서 일시 중단된 동안 위치 업데이트 이벤트를 받지 못합니다. 앱이 기록을 통해 위치 업데이트를 추적하는 경우 이 점에 유의해야 합니다. 앱이 초점을 다시 받으면 새로운 이벤트만 수신하게 됩니다. 비활성 상태에서 발생한 업데이트는 받지 못합니다.

-   원시 및 퓨전 센서를 효율적으로 사용합니다. *원시* 및 *퓨전*이라는 두 가지 유형의 센서가 있습니다.

    -   원시 센서에는 가속도계, 회전계 및 자력계가 포함됩니다.
    -   퓨전 센서에는 방향, 경사계 및 나침반이 포함됩니다. 퓨전 센서는 원시 센서의 조합에서 데이터를 가져옵니다.

    Windows 런타임 API는 자력계를 제외한 모든 센서에 액세스할 수 있습니다. 퓨전 센서는 원시 센서보다 더 정확하고 안정적이지만 전원을 더 많이 사용합니다. 따라서 용도에 적합한 센서를 사용해야 합니다. 자세한 내용은 [센서](https://msdn.microsoft.com/library/windows/apps/mt187358)를 참조하세요.

**연결된 대기 상태:**PC가 연결된 대기 상태이면 [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) 개체를 항상 인스턴스화할 수 있습니다. 그러나 **Geolocator** 개체에서 집계할 센서를 찾을 수 없으므로 [**GetGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536) 호출은 7초 후 시간 초과되고 [**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540) 이벤트 수신기가 호출되지 않으며 [**StatusChanged**](https://msdn.microsoft.com/library/windows/apps/br225542) 이벤트 수신기는 **NoData** 상태로 한 번 호출됩니다.

## 추가 사용법 지침


### 위치 설정의 변경 검색

사용자는 **설정** 앱의 **위치 개인 정보 설정**을 사용하여 위치 기능을 끌 수 있습니다.

-   사용자가 위치 서비스를 사용할 수 없게 설정하거나 다시 사용 가능하게 설정하는 경우를 감지하려면
    -   [
            **StatusChanged**](https://msdn.microsoft.com/library/windows/apps/br225542) 이벤트를 처리합니다. 사용자가 위치 서비스를 끄면 **StatusChanged** 이벤트에 대한 인수의 [**Status**](https://msdn.microsoft.com/library/windows/apps/br225601) 속성 값은 **Disabled**가 됩니다.
    -   [
            **GetGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536)에서 반환된 오류 코드를 확인합니다. 사용자가 위치 서비스를 사용하지 않도록 설정한 경우 **ACCESS\_DENIED** 오류로 인해 **GetGeopositionAsync** 호출이 실패하고 [**LocationStatus**](https://msdn.microsoft.com/library/windows/apps/br225538) 속성은 **Disabled** 값을 가집니다.
-   위치 데이터가 중요한 앱(예: 매핑 앱)이 있는 경우 다음을 수행해야 합니다.
    -   사용자의 위치가 변경될 경우 [**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540) 이벤트를 처리하여 업데이트를 가져옵니다.
    -   앞에서 설명한 대로 [**StatusChanged**](https://msdn.microsoft.com/library/windows/apps/br225542) 이벤트를 처리하여 위치 설정의 변경을 감지합니다.

위치 서비스는 사용 가능해지면 데이터를 반환합니다. 먼저 오차 반경이 더 큰 위치를 반환한 다음 사용 가능해지면 보다 정확한 정보로 위치를 업데이트할 수 있습니다. 사용자의 위치를 표시하는 앱은 일반적으로 보다 정확한 정보가 사용 가능해지면 위치를 업데이트하려고 합니다.

### 위치에 대한 그래픽 표현

앱에서 [**Geocoordinate.accuracy**](https://msdn.microsoft.com/library/windows/apps/br225526)를 사용하여 맵에 사용자의 현재 위치를 명시적으로 표시해야 합니다. 정확성에 대한 세 가지 주요 밴드가 있습니다. 즉, 약 10미터의 오차 반경, 약 100미터의 오차 반경 및 1킬로미터 이상의 오차 반경이 있습니다. 정확성 정보를 사용하면 앱이 사용 가능한 데이터 컨텍스트에 정확하게 위치를 표시하도록 할 수 있습니다. 지도 컨트롤을 사용하는 방법에 대한 일반적인 정보는 [2D, 3D 및 거리 뷰가 있는 지도 표시](https://msdn.microsoft.com/library/windows/apps/mt219695)를 참조하세요.

-   정확성이 약 10미터인 경우(GPS 해상도) 지도에서 점이나 핀으로 위치를 표시할 수 있습니다. 이 정확성을 사용하여 위도-경도 좌표와 주소도 표시할 수 있습니다.

    ![약 10미터의 GPS 정확성으로 표시된 지도의 예](images/10metererrorradius.png)

-   정확성이 10에서 500미터 사이(약 100미터)인 경우 일반적으로 Wi-Fi 해상도를 통해 위치를 받습니다. 셀룰러에서 얻은 위치의 정확성은 약 300미터입니다. 이런 경우 앱에서 오차 반경을 표시하는 것이 좋습니다. 중심 점이 필요한 방향을 표시하는 앱의 경우 이러한 점은 해당 점 주위의 오차 반경으로 표시할 수 있습니다.

    ![약 100미터의 Wi-Fi 정확성으로 표시된 지도의 예](images/100metererrorradius.png)

-   반환된 정확성이 1킬로미터보다 큰 경우 IP 수준 해상도로 위치 정보를 받을 수 있습니다. 이 정확성 수준은 너무 낮아서 지도에서 특정 지점을 정확히 나타낼 수 없습니다. 앱에서 지도를 도시 수준 또는 오차 반경에 따라 적절한 영역(예: 지역 수준)으로 확대해야 합니다.

    ![약 1킬로미터의 Wi-Fi 정확성으로 표시된 지도의 예](images/1000metererrorradius.png)

위치 정확성이 한 밴드에서 다른 밴드로 전환되면 서로 다른 그래픽 표현 간의 점진적 전환을 제공합니다. 이 작업을 다음과 같이 수행할 수 있습니다.

-   전환 애니메이션을 원활하게 하고 전환을 신속하고 유연하게 유지합니다.
-   몇 가지 연속되는 보고서에서 정확성의 변경을 확인할 때까지 기다려 원하지 않는 확대/축소가 너무 자주 수행되지 않도록 합니다.

### 위치에 대한 텍스트 표현

일부 유형의 앱(예: 날씨 앱 또는 지역 정보 앱)에는 다른 정확성 밴드의 위치를 텍스트로 표현할 방법이 필요합니다. 위치를 명확하게 표시하고 데이터에 제공된 정확성 수준으로만 표시해야 합니다.

-   정확성이 약 10미터(GPS 해상도)인 경우 수신된 위치 데이터가 매우 정확하므로 주변 이름 수준으로 전달할 수 있습니다. 구/군/시 이름, 시/도 이름 및 국가/지역 이름을 사용할 수도 있습니다.
-   정확성이 약 100미터(Wi-Fi 해상도)인 경우 수신된 위치 데이터가 적당히 정확하므로 정보를 구/군/시 이름 수준으로 표시하는 것이 좋습니다. 주변 이름은 사용하지 마세요.
-   정확성이 1킬로미터보다 큰 경우(IP 해상도) 시/도 또는 국가/지역 이름만 표시합니다.

### 개인 정보 고려 사항

사용자의 지리적 위치는 PII(개인 식별이 가능한 정보)입니다. 다음 웹 사이트에서는 사용자 개인 정보 보호에 대한 지침을 제공합니다.

-   [Microsoft 개인 정보]( http://go.microsoft.com/fwlink/p/?LinkId=259692)

<!--For more info, see [Guidelines for privacy-aware apps](guidelines-for-enabling-sensitive-devices.md).-->

## 관련 항목

* [지오펜스 설정](https://msdn.microsoft.com/library/windows/apps/mt219702)
* [현재 위치 가져오기](https://msdn.microsoft.com/library/windows/apps/mt219698)
* [2D, 3D 및 Streetside 뷰가 있는 지도 표시](https://msdn.microsoft.com/library/windows/apps/mt219695)
<!--* [Design guidelines for privacy-aware apps](guidelines-for-enabling-sensitive-devices.md)-->
* [UWP 위치 샘플(지리적 위치)](http://go.microsoft.com/fwlink/p/?linkid=533278)
 

 







<!--HONumber=Jun16_HO4-->


