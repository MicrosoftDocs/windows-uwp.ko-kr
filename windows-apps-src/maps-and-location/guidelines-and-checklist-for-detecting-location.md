---
description: 이 항목에서는 사용자의 위치에 액세스 해야 하는 앱에 대 한 성능 지침을 설명 합니다.
title: 위치 인식 앱에 대 한 지침
ms.assetid: 16294DD6-5D12-4062-850A-DB5837696B4D
ms.date: 10/20/2020
ms.topic: article
keywords: windows 10, uwp, 위치, 지도, 지리적 위치
ms.localizationpriority: medium
ms.openlocfilehash: af9ea1a214bb3cb49dd65a77d1fde30e4bb3a064
ms.sourcegitcommit: 7aaf0740a5d3a17ebf9214aa5e5d056924317673
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92297741"
---
# <a name="guidelines-for-location-aware-apps"></a>위치 인식 앱에 대 한 지침

**중요 API**

-   [**지리적 위치**](/uwp/api/Windows.Devices.Geolocation)
-   [**메서드에**](/uwp/api/Windows.Devices.Geolocation.Geolocator)

이 항목에서는 사용자의 위치에 액세스 해야 하는 앱에 대 한 성능 지침을 설명 합니다.

## <a name="recommendations"></a>권장 사항


-   응용 프로그램에서 위치 데이터를 필요로 하는 경우에만 location 개체 사용을 시작 합니다.

    사용자의 위치에 액세스 하기 전에 [**Requestaccessasync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) 를 호출 합니다. 이때 앱이 포그라운드에 있어야 하고 **RequestAccessAsync**가 UI 스레드에서 호출되어야 합니다. 사용자가 자신의 위치에 대한 권한을 앱에 부여하기 전에는 앱이 위치 데이터에 액세스할 수 없습니다.

-   위치가 앱에 필수적이 지 않은 경우 사용자가 요구 하는 작업을 완료 하려고 할 때까지 액세스 하지 마세요. 예를 들어 소셜 네트워킹 앱에 "내 위치에 체크 인" 단추가 있는 경우 사용자가 단추를 클릭할 때까지 앱에서 위치에 액세스 하지 않아야 합니다. 응용 프로그램의 주 함수에 필요한 경우 위치에 즉시 액세스할 수 있습니다.

-   사용자에 대 한 동의 확인 프롬프트를 트리거하기 위해 먼저, 전경 앱의 주 UI 스레드에서 [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) 개체를 사용 해야 합니다. **Geolocator** 를 처음으로 사용 하는 것은 [**Getgeopositionasync**](/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync) 에 대 한 첫 번째 호출 이거나 [**positionchanged**](/uwp/api/windows.devices.geolocation.geolocator.positionchanged) 이벤트에 대 한 처리기의 첫 번째 등록 일 수 있습니다.

-   위치 데이터를 사용 하는 방법을 사용자에 게 알려 줍니다.
-   사용자가 수동으로 위치를 새로 고칠 수 있도록 UI를 제공 합니다.
-   위치 데이터를 가져오기 위해 대기 하는 동안 진행률 표시줄 또는 링을 표시 합니다. <!--For info on the available progress controls and how to use them, see [**Guidelines for progress controls**](guidelines-and-checklist-for-progress-controls.md).-->
-   위치 서비스를 사용 하지 않도록 설정 하거나 사용할 수 없는 경우 적절 한 오류 메시지 또는 대화 상자를 표시 합니다.

    위치 설정에서 앱이 사용자의 위치에 액세스 하는 것을 허용 하지 않는 경우 **설정** 앱에서 **위치 개인 정보 설정** 에 대 한 편리한 링크를 제공 하는 것이 좋습니다. 예를 들어 Hyperlink 컨트롤을 사용 하거나 [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync) 메서드를 호출 하 여 URI를 사용 하는 코드에서 **설정** 앱을 시작할 수 있습니다 `ms-settings:privacy-location` . 자세한 내용은 [Windows 설정 앱 시작](../launch-resume/launch-settings-app.md)을 참조 하세요.

-   캐시 된 위치 데이터를 지우고 사용자가 위치 정보에 대 한 액세스를 사용 하지 않도록 설정할 때 [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) 를 해제 합니다.

    사용자가 설정을 통해 위치 정보에 대 한 액세스를 해제 하는 경우 [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) 개체를 해제 합니다. 그러면 앱이 모든 위치 API 호출에 대 한 **액세스 \_ 거부** 결과를 수신 합니다. 앱에서 위치 데이터를 저장 하거나 캐시 하는 경우 사용자가 위치 정보에 대 한 액세스 권한을 해지할 때 캐시 된 모든 데이터를 지웁니다. Location services를 통해 위치 데이터를 사용할 수 없는 경우 위치 정보를 수동으로 입력할 수 있는 대체 방법을 제공 합니다.

-   ) 준비가 location services에 대 한 UI를 제공 합니다. 예를 들어 [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) 개체를 다시 인스턴스화하고 위치 정보를 다시 가져오려고 하는 새로 고침 단추를 제공 합니다.

    앱이) 준비가 location services에 대 한 UI를 제공 하도록 합니다.

    -   사용자가 사용 하지 않도록 설정한 후에 위치 액세스를 다시 사용 하는 경우 앱에 대 한 알림이 없습니다. [**status**](/uwp/api/windows.devices.geolocation.statuschangedeventargs.status) 속성이 변경되지 않고 [**statusChanged**](/uwp/api/windows.devices.geolocation.geolocator.statuschanged) 이벤트도 발생하지 않습니다. 앱에서 새 [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) 개체를 만들고 [**Getgeopositionasync**](/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync) 를 호출 하 여 업데이트 된 위치 데이터를 가져오거나, [**positionchanged**](/uwp/api/windows.devices.geolocation.geolocator.positionchanged) 이벤트를 다시 구독 해야 합니다. 상태에서 위치가 했다가 다시 설정할 된 것으로 표시 되 면 앱이 이전에 위치 서비스를 사용 하지 않도록 설정 된 사용자에 게 알리고 새 상태에 적절 하 게 응답 하는 UI를 모두 지웁니다.
    -   또한 앱은 활성화 될 때 또는 사용자가 위치 정보를 요구 하는 기능을 명시적으로 사용 하거나 다른 시나리오에 적절 한 시간을 사용 하려고 할 때 위치 데이터를 다시 시도해 야 합니다.

**성능**

-   앱이 위치 업데이트를 수신 하지 않아도 되는 경우 일회성 위치 요청을 사용 합니다. 예를 들어 위치 태그를 사진에 추가 하는 앱은 위치 업데이트 이벤트를 받을 필요가 없습니다. 대신 [현재 위치 가져오기](./get-location.md)에 설명 된 대로 [**Getgeopositionasync**](/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync)를 사용 하 여 위치를 요청 해야 합니다.

    일회성 위치 요청을 수행 하는 경우 다음 값을 설정 해야 합니다.

    -   [**DesiredAccuracy**](/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracy) 또는 [**DesiredAccuracyInMeters**](/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracyinmeters)를 설정 하 여 앱에서 요청 하는 정확도를 지정 합니다. 이러한 매개 변수 사용에 대 한 권장 사항은 아래를 참조 하십시오.
    -   [**Getgeopositionasync**](/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync) 의 max age 매개 변수를 설정 하 여 앱에 유용한 위치를 얻을 수 있는 기간을 지정 합니다. 앱에서 몇 초 또는 몇 분의 이전 위치를 사용할 수 있는 경우 앱은 거의 즉시 위치를 수신 하 여 장치 전원을 절약 하는 데 도움이 될 수 있습니다.
    -   [**Getgeopositionasync**](/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync)의 timeout 매개 변수를 설정 합니다. 앱에서 위치 또는 오류가 반환 될 때까지 대기할 수 있는 시간입니다. 사용자에 대 한 응답성과 앱에 필요한 정확도 간의 장단점을 파악 해야 합니다.
-   자주 위치를 업데이트 해야 하는 경우 연속 위치 세션을 사용 합니다. 특정 임계값을 지난 이동을 검색 하거나 발생 하는 연속 위치 업데이트의 경우 [**positionchanged**](/uwp/api/windows.devices.geolocation.geolocator.positionchanged) 및 [**statuschanged**](/uwp/api/windows.devices.geolocation.geolocator.statuschanged) 이벤트를 사용 합니다.

    위치 업데이트를 요청 하는 경우 [**DesiredAccuracy**](/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracy) 또는 [**DesiredAccuracyInMeters**](/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracyinmeters)를 설정 하 여 앱에서 요청 하는 정확도를 지정할 수 있습니다. 또한 [**Movementthreshold**](/uwp/api/windows.devices.geolocation.geolocator.movementthreshold) 또는 [**reportinterval**](/uwp/api/windows.devices.geolocation.geolocator.reportinterval)을 사용 하 여 위치 업데이트가 필요한 빈도를 설정 해야 합니다.

    -   이동 임계값을 지정 합니다. 일부 앱은 사용자가 멀리 떨어져 이동한 경우에만 위치를 업데이트 해야 합니다. 예를 들어, 로컬 뉴스 또는 날씨 업데이트를 제공 하는 앱에는 사용자의 위치가 다른 도시로 변경 되지 않는 한 위치 업데이트가 필요 하지 않을 수 있습니다. 이 경우 [**Movementthreshold**](/uwp/api/windows.devices.geolocation.geolocator.movementthreshold) 속성을 설정 하 여 위치 업데이트 이벤트에 대해 필요한 최소 이동을 조정 합니다. 이렇게 하면 [**Positionchanged**](/uwp/api/windows.devices.geolocation.geolocator.positionchanged) 이벤트를 필터링 하는 효과가 있습니다. 이러한 이벤트는 위치 변경이 이동 임계값을 초과 하는 경우에만 발생 합니다.

    -   앱 환경과 맞추고 시스템 리소스 사용을 최소화 하는 [**Reportinterval**](/uwp/api/windows.devices.geolocation.geolocator.reportinterval) 을 사용 합니다. 예를 들어 날씨 앱에는 15 분 마다 데이터 업데이트가 필요할 수 있습니다. 실시간 탐색 앱이 아닌 대부분의 앱에는 정확히 정확한 위치 업데이트 스트림이 필요 하지 않습니다. 앱에서 가능한 데이터 스트림이 가장 정확 하 게 필요 하지 않거나 업데이트가 자주 필요 하지 않은 경우에는 **Reportinterval** 속성을 설정 하 여 앱에 필요한 위치 업데이트의 최소 빈도를 표시 합니다. 위치 원본은 필요한 경우에만 위치를 계산 하 여 전원을 절약할 수 있습니다.

        실시간 데이터가 필요한 앱은 최소 간격이 지정 되지 않았음을 나타내기 위해 [**Reportinterval**](/uwp/api/windows.devices.geolocation.geolocator.reportinterval) 을 0으로 설정 해야 합니다. 기본 보고서 간격은 1 초 또는 하드웨어에서 지원할 수 있는 빈도 만큼 자주 발생 합니다.

        위치 데이터를 제공 하는 장치는 다른 앱에서 요청 된 보고서 간격을 추적 하 고 요청 된 최소 간격으로 데이터 보고서를 제공할 수 있습니다. 따라서 정확도가 가장 많은 앱에서 필요한 데이터를 받습니다. 따라서 다른 앱이 더 자주 업데이트를 요청한 경우 위치 공급자가 앱이 요청한 것 보다 더 높은 빈도로 업데이트를 생성할 수 있습니다.

        **참고**    위치 원본이 지정 된 보고서 간격에 대 한 요청을 인식 하지 못하는 것은 아닙니다. 모든 위치 공급자 장치에서 보고서 간격을 추적 하는 것은 아니지만이를 수행 하는 경우에도 해당 장치를 제공 해야 합니다.

    -   전원을 절약 하려면 앱에 높은 정확도 데이터가 필요한 지 여부를 위치 플랫폼에 표시 하도록 [**desiredAccuracy**](/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracy) 속성을 설정 합니다. 앱에 높은 정확도 데이터가 필요 하지 않은 경우 시스템에서 GPS 공급자를 설정 하지 않고 전원을 절약할 수 있습니다.

        -   [**DesiredAccuracy**](/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracy) 를 **HIGH** 로 설정 하면 GPS에서 데이터를 가져올 수 있습니다.
        -   앱이 ad 대상 지정에만 위치 정보를 사용 하는 경우 [**desiredAccuracy**](/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracy) 을 **기본값으로** 설정 하 고 단일 샷 호출 패턴을 사용 하 여 전원 소비량을 최소화 합니다.

        앱에 정확성에 대 한 특정 요구 사항이 있는 경우 [**DesiredAccuracy**](/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracy)를 사용 하는 대신 [**DesiredAccuracyInMeters**](/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracyinmeters) 속성을 사용 하는 것이 좋습니다. 이 기능은 휴대폰 신호, Wi-Fi 탐지 장치 및 위성을 기반으로 일반적으로 위치를 가져올 수 있는 Windows Phone에 특히 유용 합니다. 보다 구체적인 정확도 값을 선택 하면 위치를 제공할 때 가장 낮은 전원 비용으로 사용할 올바른 기술을 시스템에서 식별할 수 있습니다.

        예를 들면 다음과 같습니다.

        -   앱이 광고 튜닝, 날씨, 뉴스 등의 위치를 가져오는 경우 일반적으로 5000 미터의 정확도가 충분 합니다.
        -   앱이 환경에서 가까운 거래를 표시 하는 경우 300 미터의 정확도는 일반적으로 결과를 제공 하는 데 유용 합니다.
        -   사용자가 가까운 식당에 대 한 권장 사항을 찾고 있는 경우 블록 내에서 위치를 가져올 수 있습니다. 따라서 정확도가 100 미터 면 충분 합니다.
        -   사용자가 자신의 위치를 공유 하려는 경우 앱은 약 10 미터의 정확도를 요청 해야 합니다.
    -   앱에 특정 정확도 요구 사항이 있는 경우에는 [**Geocoordinate 정확도**](/uwp/api/windows.devices.geolocation.geocoordinate.accuracy) 속성을 사용 합니다. 예를 들어 탐색 앱은 사용 가능한 위치 데이터가 앱의 요구 사항을 충족 하는지 여부를 확인 하려면 **Geocoordinate. 정확도** 속성을 사용 해야 합니다.

-   시작 지연을 고려 합니다. 앱에서 위치 데이터를 처음 요청할 때 위치 공급자가 시작 되는 동안 짧은 지연 (1-2 초)이 있을 수 있습니다. 앱의 UI 디자인에서이를 고려 합니다. 예를 들어 [**Getgeopositionasync**](/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync)에 대 한 호출 완료를 보류 중인 다른 작업이 차단 되지 않도록 할 수 있습니다.

-   백그라운드 동작을 고려 합니다. 앱에 포커스가 없으면 백그라운드에서 일시 중단 된 위치 업데이트 이벤트는 수신 되지 않습니다. 앱에서 위치 업데이트를 기록 하 여 추적 하는 경우이에 주의 하세요. 앱이 포커스를 다시 얻으면 새 이벤트만 수신 합니다. 비활성화 되었을 때 발생 한 업데이트는 가져오지 않습니다.

-   Raw 및 fusion 센서를 효율적으로 사용 합니다. 센서에는 *원시* 및 *fusion*의 두 가지 유형이 있습니다.

    -   원시 센서에는가 속도계, 회전 계 및 지자기 센터가 포함 됩니다.
    -   Fusion 센서에는 방향, 경사 계 및 나침반이 포함 됩니다. Fusion 센서는 원시 센서의 조합에서 데이터를 가져옵니다.

    Windows 런타임 Api는 지자기 센터을 제외 하 고 이러한 모든 센서에 액세스할 수 있습니다. Fusion 센서는 원시 센서 보다 더 정확 하 고 안정적 이지만 더 강력한 기능을 사용 합니다. 올바른 용도의 센서를 사용 해야 합니다. 자세한 내용은 [센서](../devices-sensors/sensors.md)를 참조 하세요.

**연결 된 대기**
- PC가 연결 된 대기 상태 이면 [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) 개체가 항상 인스턴스화될 수 있습니다. 그러나 **Geolocator** 개체는 집계할 센서를 찾지 않으므로 [**Getgeopositionasync**](/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync) 에 대 한 호출은 7 초 후에 시간 초과 됩니다. [**positionchanged**](/uwp/api/windows.devices.geolocation.geolocator.positionchanged) 이벤트 수신기는 호출 되지 않으며, [**statuschanged**](/uwp/api/windows.devices.geolocation.geolocator.statuschanged) 이벤트 수신기는 **NoData** 상태를 사용 하 여 한 번 호출 됩니다.

## <a name="additional-usage-guidance"></a>추가 사용법 지침


### <a name="detecting-changes-in-location-settings"></a>위치 설정의 변경 내용 검색

사용자는 **설정** 앱에서 **위치 개인 정보 설정** 을 사용 하 여 위치 기능을 해제할 수 있습니다.

-   사용자가 위치 서비스를 사용 하지 않도록 설정 하거나,을 (를) 해제 하는 경우 검색
    -   [**Statuschanged**](/uwp/api/windows.devices.geolocation.geolocator.statuschanged) 이벤트를 처리 합니다. **Statuschanged** 이벤트에 대 한 인수의 [**Status**](/uwp/api/windows.devices.geolocation.statuschangedeventargs.status) 속성은 사용자가 위치 서비스를 끄면 **사용 하지 않도록 설정** 된 값을 갖습니다.
    -   [**Getgeoposit, async**](/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync)에서 반환 된 오류 코드를 확인 합니다. 사용자가 위치 서비스를 사용 하지 않도록 설정 하면 **액세스 \_ 거부** 오류가 발생 하 여 **Getgeoposit, async** 에 대 한 호출이 실패 하 고 [**Locationstatus**](/uwp/api/windows.devices.geolocation.geolocator.locationstatus) 속성의 값이 **사용 하지 않도록 설정**됩니다.
-   위치 데이터가 중요 한 응용 프로그램 (예: 매핑 앱)이 있는 경우 다음을 수행 해야 합니다.
    -   사용자 위치가 변경 되 면 업데이트를 가져오기 위해 [**Positionchanged**](/uwp/api/windows.devices.geolocation.geolocator.positionchanged) 이벤트를 처리 합니다.
    -   이전에 설명한 대로 [**Statuschanged**](/uwp/api/windows.devices.geolocation.geolocator.statuschanged) 이벤트를 처리 하 여 위치 설정의 변경 내용을 검색 합니다.

Location service는 데이터를 사용할 수 있게 되 면 데이터를 반환 합니다. 먼저 더 큰 오류 반지름이 있는 위치를 반환 하 고 사용할 수 있게 되 면 더 정확한 정보를 사용 하 여 위치를 업데이트할 수 있습니다. 사용자 위치를 표시 하는 앱은 일반적으로 더 정확한 정보를 사용할 수 있게 되 면 위치를 업데이트 하려고 합니다.

### <a name="graphical-representations-of-location"></a>위치의 그래픽 표현

앱에서 [**Geocoordinate**](/uwp/api/windows.devices.geolocation.geocoordinate.accuracy) 를 사용 하도록 하 여 지도에서 사용자의 현재 위치를 명확 하 게 나타냅니다. 정확성에 대 한 세 가지 기본 밴드, 약 10 미터의 오류 반지름, 약 100 미터의 오류 반지름, 1 kilometer 이상의 오류 반지름이 있습니다. 정확도 정보를 사용 하 여 앱이 사용할 수 있는 데이터의 컨텍스트에서 위치를 정확 하 게 표시 하도록 할 수 있습니다. 지도 컨트롤을 사용하는 방법에 대한 일반적인 정보는 [2D, 3D 및 거리 뷰가 있는 지도 표시](./display-maps.md)를 참조하세요.

-   정확도가 10 미터 (GPS 해상도)와 일치 하도록 하려면 지도에 위치를 점으로 표시 하거나 고정할 수 있습니다. 이러한 정확도를 사용 하 여 위도-경도 좌표와 주소를 표시할 수도 있습니다.

    ![약 10 미터의 gps 정확도에 표시 되는 지도의 예입니다.](images/10metererrorradius.png)

-   10 500 미터 (약 100 미터) 사이의 정확도를 위해 위치는 일반적으로 Wi-Fi 확인을 통해 수신 됩니다. 셀룰러에서 얻은 위치는 300 미터를 기준으로 정확도가 높아집니다. 이 경우 앱에서 오류 radius를 표시 하는 것이 좋습니다. 중심 점이 필요한 방향을 표시 하는 앱의 경우 이러한 점이 주변의 오류 반지름으로 표시 될 수 있습니다.

    ![약 100 미터의 wi-fi 정확도에 표시 되는 지도의 예입니다.](images/100metererrorradius.png)

-   반환 된 정확성이 1 kilometer 보다 크면 IP 수준 해상도에서 위치 정보를 수신 하는 것일 수 있습니다. 이러한 정확도 수준은 종종 지도의 특정 지점을 정확 하 게 찾을 수 없을 정도로 낮습니다. 앱은 지도의 도시 수준을 확대 하거나 오류 반지름 (예: 지역 수준)에 따라 적절 한 영역으로 확대 해야 합니다.

    ![약 1 kilometer의 wi-fi 정확도에 표시 되는 지도의 예입니다.](images/1000metererrorradius.png)

위치 정확도가 정확도의 한도에서 다른 대역으로 전환 되는 경우 다양 한 그래픽 표현 사이에서 정상적인 전환을 제공 합니다. 이 작업은 다음을 수행 하 여 수행할 수 있습니다.

-   전환 애니메이션을 부드러운 상태로 전환 하 고 신속 하 게 전환 합니다.
-   몇 번의 연속 보고서가 정확성을 확인 하는 데 도움이 되는 경우 원치 않는 변경 내용을 확인 하 고 너무 자주 확대/축소를 방지 합니다.

### <a name="textual-representations-of-location"></a>위치의 텍스트 표현

일부 유형의 앱 (예: 날씨 앱 또는 로컬 정보 앱)은 다양 한 정확도의 위치를 표시 하는 방법이 필요 합니다. 데이터에 제공 된 정확도 수준 까지만 위치를 명확 하 게 표시 해야 합니다.

-   정확도가 10 미터 (GPS 해상도)와 같은 경우 수신 되는 위치 데이터는 매우 정확 하므로 환경 이름 수준으로 통신할 수 있습니다. 도시 이름, 시/도 이름 및 국가/지역 이름을 사용할 수도 있습니다.
-   정확도가 100 미터 (Wi-fi 해상도)와 일치 하는 경우 수신 되는 위치 데이터는 약간 정확 하므로 도시 이름에 정보를 표시 하는 것이 좋습니다. 환경 이름을 사용 하지 마십시오.
-   정확도가 1 kilometer (IP 확인) 경우 시/도 또는 국가/지역 이름만 표시 합니다.

### <a name="privacy-considerations"></a>개인정보처리방침 고려 사항

사용자의 지리적 위치는 PII (개인 식별이 가능한 정보)입니다. 다음 웹 사이트에서는 사용자 개인 정보 보호에 대 한 지침을 제공 합니다.

-   [Microsoft 개인 정보]( https://www.microsoft.com/privacy/dpd/default.aspx)

<!--For more info, see [Guidelines for privacy-aware apps](guidelines-for-enabling-sensitive-devices.md).-->

## <a name="related-topics"></a>관련 항목

* [지오펜스 설정](./set-up-a-geofence.md)
* [현재 위치 가져오기](./get-location.md)
* [2D, 3D 및 Streetside 뷰가 있는 지도 표시](./display-maps.md)
<!--* [Design guidelines for privacy-aware apps](guidelines-for-enabling-sensitive-devices.md)-->
* [UWP 위치 샘플 (지리적 위치)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)
 

 
