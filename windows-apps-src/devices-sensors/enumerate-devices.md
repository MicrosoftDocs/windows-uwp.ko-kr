---
ms.assetid: 4311D293-94F0-4BBD-A22D-F007382B4DB8
title: 디바이스 열거
description: 열거 네임 스페이스를 사용 하면 무선 또는 네트워킹 프로토콜을 통해 시스템에 내부적으로 연결 되거나 외부에서 연결 되거나 검색 가능한 장치를 찾을 수 있습니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 499af7929c67064623e15ee1d475e0f643fafd2b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165467"
---
# <a name="enumerate-devices"></a>디바이스 열거


## <a name="samples"></a>샘플

사용 가능한 모든 장치를 열거 하는 가장 간단한 방법은 [**FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) 명령을 사용 하 여 스냅숏을 만드는 것입니다 (아래 섹션에서 자세히 설명).

```CSharp
async void enumerateSnapshot(){
  DeviceInformationCollection collection = await DeviceInformation.FindAllAsync();
}
```

더 고급 사용을 보여 주는 샘플을 다운로드 하려면 [여기](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing)를 클릭 [**하세요.**](/uwp/api/Windows.Devices.Enumeration)

## <a name="enumeration-apis"></a>열거 Api

열거 네임 스페이스를 사용 하면 무선 또는 네트워킹 프로토콜을 통해 시스템에 내부적으로 연결 되거나 외부에서 연결 되거나 검색 가능한 장치를 찾을 수 있습니다. 가능한 장치를 통해 열거 하는 데 사용 하는 Api는 [**Windows.**](/uwp/api/Windows.Devices.Enumeration) i. i 네임 스페이스입니다. 이러한 Api를 사용 하는 몇 가지 이유는 다음과 같습니다.

-   응용 프로그램을 사용 하 여 연결할 장치 찾기
-   시스템에서 연결 하거나 검색할 수 있는 장치에 대 한 정보 가져오기
-   장치를 추가 하거나, 연결 하거나, 연결을 끊거나, 온라인 상태를 변경 하거나, 다른 속성을 변경할 때 앱에서 알림을 받도록 합니다.
-   장치가 연결 하거나, 연결을 끊거나, 온라인 상태를 변경 하거나, 다른 속성을 변경할 때 앱이 백그라운드 트리거를 받도록 합니다.

이러한 Api는 개별 장치와 응용 프로그램을 실행 하는 시스템에서 해당 기술을 지 원하는 경우 다음 프로토콜 및 버스를 통해 장치를 열거할 수 있습니다. 이 목록은 완전 한 목록이 아니며 특정 장치에서 다른 프로토콜을 지원할 수 있습니다.

-   실제로 연결 된 버스. 여기에는 PCI 및 USB가 포함 됩니다. 예를 들어 **Device Manager**에서 볼 수 있는 모든 것이 있습니다.
-   [UPnP](/windows/desktop/UPnP/universal-plug-and-play-start-page)
-   디지털 살아있는 네트워크 동맹 (DLNA)
-   [**검색 및 시작 (전화 걸기)**](/uwp/api/Windows.Media.DialProtocol)
-   [**Dns 서비스 검색 (DNS SD)**](/uwp/api/Windows.Networking.ServiceDiscovery.Dnssd)
-   [장치 (WSD)의 웹 서비스](/windows/desktop/WsdApi/wsd-portal)
-   [Bluetooth](/windows/desktop/Bluetooth/bluetooth-start-page)
-   [**Wi-fi Direct**](/uwp/api/Windows.Devices.WiFiDirect)
-   WiGig
-   [**서비스 지점**](/uwp/api/Windows.Devices.PointOfService)

대부분의 경우 열거형 Api를 사용 하는 것에 대해 걱정할 필요가 없습니다. 이는 장치를 사용 하는 많은 Api가 적절 한 기본 장치를 자동으로 선택 하거나 보다 간소화 된 열거 API를 제공 하기 때문입니다. 예를 들어 [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) 는 기본 오디오 렌더러 장치를 자동으로 사용 합니다. 앱에서 기본 장치를 사용할 수 있는 한 응용 프로그램에서 열거형 Api를 사용할 필요가 없습니다. 열거 Api는 사용 가능한 장치를 검색 하 고 연결 하는 데 일반적이 고 유연한 방법을 제공 합니다. 이 항목에서는 장치를 열거 하는 방법에 대해 설명 하 고 장치를 열거 하는 네 가지 일반적인 방법을 설명 합니다.

-   [**DevicePicker**](/uwp/api/Windows.Devices.Enumeration.DevicePicker) UI 사용
-   현재 시스템에서 검색 가능한 장치의 스냅숏 열거
-   현재 검색 가능 하 고 변경 내용을 감시 하는 장치 열거
-   현재 검색 가능한 장치 열거 및 백그라운드 작업의 변경 내용 감시

## <a name="deviceinformation-objects"></a>DeviceInformation 개체


열거 Api를 사용 하 여 작업 하는 경우에는 [**Deviceinformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 개체를 자주 사용 해야 합니다. 이러한 개체는 장치에 대해 사용 가능한 대부분의 정보를 포함 합니다. 다음 표에서는 관심 있는 **Deviceinformation** 속성 중 일부에 대해 설명 합니다. 전체 목록은 **Deviceinformation**의 참조 페이지를 참조 하세요.

| 속성                         | 의견                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **DeviceInformation.Id**         | 이는 장치의 고유 식별자 이며 문자열 변수로 제공 됩니다. 대부분의 경우이 값은 원하는 특정 장치를 나타내기 위해 한 방법에서 다른 방법으로 전달 하는 불투명 값입니다. 앱을 닫고 다시 연 후이 속성과 **Deviceinformation. Kind** 속성을 함께 사용할 수도 있습니다. 이렇게 하면 동일한 [**Deviceinformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 개체를 복구 하 고 다시 사용할 수 있습니다. |
| **DeviceInformation입니다. Kind**       | 이는 [**Deviceinformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 개체로 표시 되는 장치 개체의 종류를 나타냅니다. 장치 범주 또는 장치 유형이 아닙니다. 단일 장치는 다양 한 종류의 여러 **Deviceinformation** 개체로 나타낼 수 있습니다. 이 속성에 사용할 수 있는 값은 [**Deviceinformationkind**](/uwp/api/windows.devices.enumeration.deviceinformationkind) 와 서로 관련 된 방법에 나와 있습니다.                           |
| **DeviceInformation. 속성** | 이 속성 모음에는 [**deviceinformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 개체에 대해 요청 된 정보가 포함 됩니다. 가장 일반적인 속성은 [**DeviceInformation.Name**](/uwp/api/windows.devices.enumeration.deviceinformation.name)와 같은 **deviceinformation** 개체의 속성으로 쉽게 참조 됩니다. 자세한 내용은 [장치 정보 속성](device-information-properties.md)을 참조 하세요.                                                                |

 

## <a name="devicepicker-ui"></a>DevicePicker UI


[**DevicePicker**](/uwp/api/Windows.Devices.Enumeration.DevicePicker) 은 사용자가 목록에서 장치를 선택할 수 있도록 하는 작은 UI를 만드는 Windows에서 제공 하는 컨트롤입니다. 몇 가지 방법으로 **DevicePicker** 창을 사용자 지정할 수 있습니다.

-   [**DevicePicker**](/uwp/api/windows.devices.enumeration.devicepicker.filter)에 [**Supporteddeviceselectors**](/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceselectors), [**SupportedDeviceClasses**](/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceclasses)또는 둘 다를 추가 하 여 UI에 표시 되는 장치를 제어할 수 있습니다. 대부분의 경우 선택기 나 클래스를 하나만 추가 하면 되지만 둘 이상의 항목을 추가 해야 하는 경우에는 여러 개를 추가할 수 있습니다. 여러 선택기 나 클래스를 추가 하는 경우 또는 논리 함수를 사용 하 여 결합 됩니다.
-   장치에 대해 검색 하려는 속성을 지정할 수 있습니다. [**DevicePicker. RequestedProperties**](/uwp/api/windows.devices.enumeration.devicepicker.requestedproperties)에 속성을 추가 하 여이 작업을 수행할 수 있습니다.
-   [**모양을**](/uwp/api/windows.devices.enumeration.devicepicker.appearance)사용 하 여 [**DevicePicker**](/uwp/api/Windows.Devices.Enumeration.DevicePicker) 모양을 변경할 수 있습니다.
-   표시 될 때 [**DevicePicker**](/uwp/api/Windows.Devices.Enumeration.DevicePicker) 의 크기와 위치를 지정할 수 있습니다.

[**DevicePicker**](/uwp/api/Windows.Devices.Enumeration.DevicePicker) 가 표시 되는 동안 장치가 추가, 제거 또는 업데이트 되 면 UI의 내용이 자동으로 업데이트 됩니다.

**참고**    [**DevicePicker**](/uwp/api/Windows.Devices.Enumeration.DevicePicker)를 사용 하 여 [**deviceinformationkind**](/uwp/api/windows.devices.enumeration.deviceinformationkind) 를 지정할 수 없습니다. 특정 **Deviceinformationkind**의 장치를 포함 하려는 경우 [**device감시자**](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) 를 빌드하고 고유한 UI를 제공 해야 합니다.

 

미디어 콘텐츠 및 전화 걸기를 사용 하려는 경우에도 각각 고유한 선택 기가 제공 됩니다. 각각은 [**CastingDevicePicker**](/uwp/api/Windows.Media.Casting.CastingDevicePicker) 및 [**DialDevicePicker**](/uwp/api/Windows.Media.DialProtocol.DialDevicePicker)입니다.

## <a name="enumerate-a-snapshot-of-devices"></a>장치의 스냅숏 열거


일부 시나리오에서는 [**DevicePicker**](/uwp/api/Windows.Devices.Enumeration.DevicePicker) 이 요구 사항에 적합 하지 않으며 유연성이 더 필요 합니다. 사용자에 게 ui를 표시 하지 않고 사용자 고유의 UI를 빌드하거나 장치를 열거 해야 할 수 있습니다. 이러한 상황에서 장치의 스냅숏을 열거할 수 있습니다. 여기에는 현재 연결 되어 있거나 시스템에 연결 된 장치를 검색 하는 작업이 포함 됩니다. 그러나이 메서드는 사용 가능한 장치의 스냅숏을 보기만 하므로 목록 전체를 열거 한 후에 연결 하는 장치를 찾을 수 없습니다. 장치를 업데이트 하거나 제거 하는 경우에도 알림이 표시 되지 않습니다. 또 다른 잠재적인 단점은이 메서드가 전체 열거가 완료 될 때까지 모든 결과를 유지 한다는 것입니다. 따라서 네트워크 또는 무선 프로토콜을 통해 검색 되기 때문에 **Associationendpoint**, **associationendpointcontainer**또는 **associationendpointservice** 개체에 관심이 있는 경우에는이 메서드를 사용 하면 안 됩니다. 이 작업을 완료 하는 데 최대 30 초까지 걸릴 수 있습니다. 이 시나리오에서는 [**Devicewatcher**](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) 개체를 사용 하 여 가능한 장치를 열거 해야 합니다.

장치 스냅숏을 열거 하려면 [**FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) 메서드를 사용 합니다. 이 메서드는 전체 열거 프로세스가 완료 될 때까지 대기 하 고 모든 결과를 하나의 [**Deviceinformationcollection**](/uwp/api/windows.devices.enumeration.deviceinformationcollection) 개체로 반환 합니다. 또한이 메서드는 결과를 필터링 하 고 관심 있는 장치로 제한 하는 여러 옵션을 제공 하도록 오버 로드 됩니다. 이 작업은 [**Deviceclass**](/uwp/api/Windows.Devices.Enumeration.DeviceClass) 를 제공 하거나 장치 선택기를 전달 하 여 수행할 수 있습니다. 장치 선택기는 열거 하려는 장치를 지정 하는 AQS 문자열입니다. 자세한 내용은 [장치 선택기 빌드](build-a-device-selector.md)를 참조 하세요.

장치 열거 스냅숏의 예는 다음과 같습니다.



결과를 제한 하는 것 외에도 장치에 대해 검색 하려는 속성을 지정할 수 있습니다. 이렇게 하면 컬렉션에서 반환 되는 각 [**Deviceinformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 개체의 속성 모음에서 지정 된 속성을 사용할 수 있습니다. 모든 장치 종류에 대해 모든 속성을 사용할 수 있는 것은 아닙니다. 장치 종류에 사용할 수 있는 속성을 확인 하려면 [장치 정보 속성](device-information-properties.md)을 참조 하세요.



## <a name="enumerate-and-watch-devices"></a>장치 열거 및 시청


장치를 열거 하는 강력 하 고 유연한 방법은 [**Devicewatcher**](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)를 만드는 것입니다. 이 옵션은 장치를 열거 하는 경우 가장 뛰어난 유연성을 제공 합니다. 현재 있는 장치를 열거 하 고, 장치 선택기와 일치 하는 장치가 추가, 제거 또는 속성 변경 되는 경우에도 알림을 받을 수 있습니다. **Devicewatcher**를 만들 때 장치 선택기를 제공 합니다. 장치 선택기에 대 한 자세한 내용은 [장치 선택기 빌드](build-a-device-selector.md)를 참조 하세요. 감시자를 만든 후에는 제공 된 조건과 일치 하는 모든 장치에 대해 다음과 같은 알림을 받게 됩니다.

-   새 장치가 추가 되 면 알림을 추가 합니다.
-   관심이 있는 속성이 변경 되 면 업데이트 알림이 표시 됩니다.
-   장치를 더 이상 사용할 수 없거나 더 이상 필터와 일치 하지 않는 경우 알림을 제거 합니다.

[**Devicewatcher**](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)를 사용 하는 대부분의 경우 장치 목록을 유지 관리 하 고, 장치에 추가 하거나, 감시자가 시청 중인 장치에서 업데이트를 받을 때 항목을 업데이트 합니다. 업데이트 알림이 수신 되 면 업데이트 된 정보가 [**Deviceinformationupdate**](/uwp/api/windows.devices.enumeration.deviceinformationupdate) 개체로 제공 됩니다. 장치 목록을 업데이트 하려면 먼저 변경 된 적절 한 [**Deviceinformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 을 찾습니다. 그런 다음 해당 개체에 대 한 [**update**](/uwp/api/windows.devices.enumeration.deviceinformation.update) 메서드를 호출 하 여 **deviceinformationupdate** 개체를 제공 합니다. 이 함수는 **Deviceinformation** 개체를 자동으로 업데이트 하는 편리한 함수입니다.

[**Devicewatcher**](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) 는 장치가 도착할 때 알림을 보내고 변경 될 때이 메서드를 사용 해야 합니다 .이 메서드는 네트워킹 또는 무선 프로토콜을 통해 열거 되기 때문에 **associationendpoint**, **Associationendpointcontainer**또는 **associationendpointservice** 개체에 관심이 있을 때 사용 해야 합니다.

[**Devicewatcher**](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)를 만들려면 [**createwatcher**](/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher) 메서드 중 하나를 사용 합니다. 이러한 메서드는 관심 있는 장치를 지정할 수 있도록 오버 로드 됩니다. 이 작업은 [**Deviceclass**](/uwp/api/Windows.Devices.Enumeration.DeviceClass) 를 제공 하거나 장치 선택기를 전달 하 여 수행할 수 있습니다. 장치 선택기는 열거 하려는 장치를 지정 하는 AQS 문자열입니다. 자세한 내용은 [장치 선택기 빌드](build-a-device-selector.md)를 참조 하세요. 또한 장치에 대해 검색할 속성을 지정 하 고 관심이 있습니다. 이렇게 하면 컬렉션에서 반환 되는 각 [**Deviceinformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 개체의 속성 모음에서 지정 된 속성을 사용할 수 있습니다. 모든 장치 종류에 대해 모든 속성을 사용할 수 있는 것은 아닙니다. 장치 종류에 사용할 수 있는 속성을 확인 하려면 [장치 정보 속성](device-information-properties.md) 을 참조 하세요.

## <a name="watch-devices-as-a-background-task"></a>장치를 백그라운드 작업으로 시청


장치를 백그라운드 작업으로 감시 하는 작업은 위에서 설명한 대로 [**Devicewatcher**](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) 를 만드는 것과 매우 비슷합니다. 실제로 이전 섹션에서 설명한 대로 일반 **Devicewatcher** 개체를 먼저 만들어야 합니다. 만든 후에는 [**GetBackgroundTrigger**](/uwp/api/windows.devices.enumeration.devicewatcher.enumerationcompleted) 대신 devicewatcher를 호출 [**합니다.**](/uwp/api/windows.devices.enumeration.devicewatcher.start) **GetBackgroundTrigger**를 호출할 때 추가, 제거 또는 업데이트에 관심 있는 알림을 지정 해야 합니다. 추가를 요청 하지 않으면 업데이트 또는 제거를 요청할 수 없습니다. 트리거를 등록 하면 **Devicewatcher** 가 백그라운드에서 즉시 실행 되기 시작 합니다. 이 시점부터 사용자의 조건과 일치 하는 응용 프로그램에 대 한 새 알림을 받을 때마다 백그라운드 작업은 트리거되고 마지막으로 응용 프로그램을 트리거한 이후 변경 된 내용을 제공 합니다.

**중요**    [**DeviceWatcherTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceWatcherTrigger) 처음으로 응용 프로그램을 트리거하는 경우 감시자가 **enumerationcompleted** 상태에 도달할 때가 됩니다. 즉, 모든 초기 결과를 포함 하 게 됩니다. 이후에 응용 프로그램을 트리거하는 경우에는 마지막 트리거 이후에 발생 한 추가, 업데이트 및 제거 알림만 포함 됩니다. 이는 초기 결과가 한 번에 하나씩 나타나지 않으며 **Enumerationcompleted** 에 도달한 후 번들로만 전달 되기 때문에 포그라운드 [**devicewatcher**](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) 개체와 약간 다릅니다.

 

일부 무선 프로토콜은 백그라운드에서 검색 하는 경우 또는 백그라운드에서 검색을 지원 하지 않을 수 있는 경우 다르게 동작 합니다. 백그라운드 검색과 관련 하 여 세 가지 가능성이 있습니다. 다음 표에서는 응용 프로그램에 발생할 수 있는 가능성 및 영향을 보여 줍니다. 예를 들어 Bluetooth 및 Wi-fi Direct는 백그라운드 검색을 지원 하지 않으므로 확장을 통해 [**DeviceWatcherTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceWatcherTrigger)를 지원 하지 않습니다.

| 동작                                  | 영향                                                                                                                                  |
|-------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| 백그라운드에서 동일한 동작               | 없음                                                                                                                                    |
| 백그라운드 에서만 수동 검색을 수행할 수 있습니다. | 장치에서 수동 검색을 기다리는 동안 검색 하는 데 더 많은 시간이 걸릴 수 있습니다.                                                           |
| 백그라운드 검색은 지원 되지 않습니다.            | [**DeviceWatcherTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceWatcherTrigger)에서 장치를 검색할 수 없으며 업데이트는 보고 되지 않습니다. |

 

[**DeviceWatcherTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceWatcherTrigger) 가 백그라운드 작업으로의 검색을 지원 하지 않는 프로토콜을 포함 하는 경우 트리거는 계속 작동 합니다. 그러나 해당 프로토콜을 통해 업데이트나 결과를 얻을 수는 없습니다. 다른 프로토콜 또는 장치에 대 한 업데이트는 정상적으로 여전히 검색 됩니다.

## <a name="using-deviceinformationkind"></a>DeviceInformationKind 사용


대부분의 시나리오에서는 [**Deviceinformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 개체의 [**deviceinformationkind**](/uwp/api/windows.devices.enumeration.deviceinformationkind) 에 대해 걱정 하지 않아도 됩니다. 이는 사용 중인 장치 API에서 반환 하는 장치 선택 기가 API와 함께 사용할 올바른 종류의 장치 개체를 얻는 것을 보증 하기 때문입니다. 그러나 일부 시나리오에서는 장치에 대 한 **Deviceinformation** 을 가져올 것 이지만 장치 선택기를 제공 하는 해당 장치 API는 없습니다. 이러한 경우에는 고유한 선택기를 빌드해야 합니다. 예를 들어 장치에 있는 웹 서비스에는 전용 API가 없지만, 이러한 장치를 검색 하 여 해당 장치에 대 한 정보를 가져온 다음, [**이 api를**](/uwp/api/Windows.Devices.Enumeration) 사용 하 여 해당 장치에 대 한 정보를 가져올 수 있습니다.

장치 개체를 열거 하는 고유한 장치 선택기를 빌드하는 경우에는 [**Deviceinformationkind**](/uwp/api/windows.devices.enumeration.deviceinformationkind) 를 이해 하는 것이 중요 합니다. 가능한 모든 종류와 서로 관련 된 방법은 **Deviceinformationkind**의 참조 페이지에 설명 되어 있습니다. **Deviceinformationkind** 의 가장 일반적인 사용 중 하나는 장치 선택기와 함께 쿼리를 제출할 때 검색할 장치의 종류를 지정 하는 것입니다. 이렇게 하면 제공 된 **Deviceinformationkind**와 일치 하는 장치만 열거 합니다. 예를 들어 **Deviceinterface** 개체를 찾은 다음 쿼리를 실행 하 여 부모 **장치** 개체에 대 한 정보를 가져올 수 있습니다. 부모 개체에 추가 정보가 포함 될 수 있습니다.

[**Deviceinformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 개체의 속성 모음에서 사용할 수 있는 속성은 장치의 [**deviceinformationkind**](/uwp/api/windows.devices.enumeration.deviceinformationkind) 유형에 따라 달라 집니다. 특정 속성은 특정 종류의 경우에만 사용할 수 있습니다. 어떤 종류의 속성을 사용할 수 있는지에 대 한 자세한 내용은 [장치 정보 속성](device-information-properties.md)을 참조 하세요. 따라서 위의 예제에서 부모 **장치** 를 검색 하면 **deviceinterface** 장치 개체에서 사용할 수 없는 추가 정보에 액세스할 수 있습니다. 이로 인해 AQS 필터 문자열을 만들 때 열거 하는 **Deviceinformationkind** 개체에 대해 요청 된 속성을 사용할 수 있는지 확인 하는 것이 중요 합니다. 필터 작성에 대 한 자세한 내용은 [장치 선택기 빌드](build-a-device-selector.md)를 참조 하세요.

**Associationendpoint**, **associationendpointcontainer**또는 **associationendpointservice** 개체를 열거 하는 경우 무선 또는 네트워크 프로토콜을 통해 열거 합니다. 이러한 상황에서는 [**FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) 를 사용 하지 말고 [**createwatcher**](/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher)를 사용 하는 것이 좋습니다. 이는 네트워크를 통해 검색 하는 경우 [**Enumerationcompleted**](/uwp/api/windows.devices.enumeration.devicewatcher.enumerationcompleted)를 생성 하기 전에 시간 제한이 없는 검색 작업이 발생 하는 경우가 많기 때문입니다. **FindAllAsync** 는 **enumerationcompleted** 가 트리거될 때까지 작업을 완료 하지 않습니다. [**Devicewatcher**](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)를 사용 하는 경우에는 **enumerationcompleted** 가 호출 될 때에 관계 없이 실제 시간에 더 가깝게 결과를 얻을 수 있습니다.

## <a name="save-a-device-for-later-use"></a>나중에 사용 하기 위해 장치 저장


모든 [**deviceinformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 개체는 [**DeviceInformation.Id**](/uwp/api/windows.devices.enumeration.deviceinformation.id) 및 [**deviceinformation. Kind**](/uwp/api/windows.devices.enumeration.deviceinformation.kind)의 두 가지 정보를 조합 하 여 고유 하 게 식별 됩니다. 이러한 두 가지 정보를 유지 하는 경우 [**CreateFromIdAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.createfromidasync)에이 정보를 제공 하 여 **deviceinformation** 개체가 손실 된 후 다시 만들 수 있습니다. 이 작업을 수행 하는 경우 앱과 통합 되는 장치에 대 한 사용자 기본 설정을 저장할 수 있습니다.


 

 