---
ms.assetid: 4311D293-94F0-4BBD-A22D-F007382B4DB8
title: 장치 열거
description: 열거형 네임스페이스를 사용하면 시스템에 내부에서 연결되거나, 외부에서 연결되거나, 무선 또는 네트워킹 프로토콜을 통해 검색 가능한 디바이스를 찾을 수 있습니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f6348cc713d4fb93dfed9310eea9d3fd1025a2de
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8332843"
---
# <a name="enumerate-devices"></a>디바이스 열거


## <a name="samples"></a>샘플

모든 사용 가능한 디바이스를 열거하는 가장 간단한 방법은 [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.findallasync.aspx) 명령을 사용하여 스냅숏을 만드는 것입니다(아래 섹션에서 자세히 설명함).

```CSharp
async void enumerateSnapshot(){
  DeviceInformationCollection collection = await DeviceInformation.FindAllAsync();
}
```

[**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) API의 고급 사용법을 보여 주는 샘플을 다운로드하려면 [여기](http://go.microsoft.com/fwlink/?LinkID=620536)를 클릭하세요.

## <a name="enumeration-apis"></a>열거형 API

열거형 네임스페이스를 사용하면 시스템에 내부에서 연결되거나, 외부에서 연결되거나, 무선 또는 네트워킹 프로토콜을 통해 검색 가능한 디바이스를 찾을 수 있습니다. 가능한 장치를 열거하는 데 사용되는 API는 [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) 네임스페이스입니다. 이러한 API를 사용하는 몇 가지 이유는 다음과 같습니다.

-   응용 프로그램에 연결할 장치 찾기
-   시스템에 연결되거나 시스템에서 검색할 수 있는 장치에 대한 정보 가져오기
-   장치가 추가되거나, 연결되거나, 연결이 끊어지거나, 온라인 상태가 변경되거나, 기타 속성이 변경된 경우 앱에 알림 제공
-   장치가 연결되거나, 연결이 끊어지거나, 온라인 상태가 변경되거나, 기타 속성이 변경된 경우 앱에 백그라운드 트리거 제공

이러한 API는 앱을 실행 중인 개별 장치 및 시스템에서 해당 기술을 지원하는 경우 다음 프로토콜 및 버스를 통해 장치를 열거할 수 있습니다. 이는 전체 목록이 아니며, 특정 장치에서 다른 프로토콜을 지원할 수도 있습니다.

-   물리적으로 연결된 버스. PCI 및 USB가 여기에 포함됩니다. 예를 들면 **장치 관리자**에서 볼 수 있는 모든 항목입니다.
-   [UPnP](https://msdn.microsoft.com/library/windows/desktop/Aa382303)
-   DLNA(Digital Living Network Alliance)
-   [**DIAL(검색 및 시작)**](https://msdn.microsoft.com/library/windows/apps/Dn946818)
-   [**DNS-SD(DNS Service Discovery)**](https://msdn.microsoft.com/library/windows/apps/Dn895183)
-   [WSD(Web Services on Devices)](https://msdn.microsoft.com/library/windows/desktop/Aa826001)
-   [Bluetooth](https://msdn.microsoft.com/library/windows/desktop/Aa362932)
-   [**Wi-Fi Direct**](https://msdn.microsoft.com/library/windows/apps/Dn297687)
-   WiGig
-   [**서비스 지점**](https://msdn.microsoft.com/library/windows/apps/Dn298071)

대부분의 경우 열거형 API 사용에 대해 걱정할 필요가 없습니다. 장치를 사용하는 많은 API에서 적절한 기본 장치를 자동으로 선택하거나 더 간소화된 연결형 API를 제공하기 때문입니다. 예를 들어 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/BR242926)는 기본 오디오 렌더러 장치를 자동으로 사용합니다. 앱에서 기본 장치를 사용하는 한 응용 프로그램에서 열거형 API를 사용할 필요가 없습니다. 열거형 API는 사용 가능한 장치를 검색하고 연결할 수 있는 일반적이고 유연한 방법을 제공합니다. 이 항목에서는 디바이스 열거에 대한 정보를 제공하고 장치를 열거하는 네 가지 일반적인 방법을 설명합니다.

-   [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841) UI 사용
-   시스템에서 현재 검색 가능한 장치의 스냅숏 열거
-   현재 검색 가능한 디바이스 열거 및 변경 내용 감시
-   현재 검색 가능한 디바이스 열거 및 백그라운드 작업의 변경 내용 감시

## <a name="deviceinformation-objects"></a>DeviceInformation 개체


열거형 API로 작업할 때 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 개체를 사용해야 하는 경우가 자주 있습니다. 이러한 개체는 사용 가능한 장치 정보의 대부분을 포함하고 있습니다. 다음 표에서는 관심을 가질 만한 **DeviceInformation** 속성 중 일부를 설명합니다. 전체 목록은 **DeviceInformation**에 대한 참조 페이지를 참조하세요.

| 속성                         | 설명                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **DeviceInformation.Id**         | 장치의 고유 식별자이며, 문자열 변수로 제공됩니다. 대부분의 경우 메서드 간에 전달하는 것만으로 관심 있는 특정 장치를 나타낼 수 있는 불투명 값입니다. 앱을 닫았다가 다시 연 후에도 이 속성 및 **DeviceInformation.Kind** 속성을 사용할 수 있습니다. 그러면 동일한 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 개체를 복구하고 다시 사용할 수 있습니다. |
| **DeviceInformation.Kind**       | [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 개체로 표시되는 장치 개체의 종류를 나타냅니다. 장치 범주나 장치 유형이 아닙니다. 단일 장치를 다른 종류의 여러 **DeviceInformation** 개체로 나타낼 수 있습니다. 이 속성에 사용할 수 있는 값 및 값 간의 관계가 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationkind.aspx)에 나와 있습니다.                           |
| **DeviceInformation.Properties** | 이 속성 모음은 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 개체에 대해 요청된 정보를 포함합니다. 가장 일반적인 속성은 **DeviceInformation** 개체의 속성(예: [**DeviceInformation.Name**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.name))으로 쉽게 참조됩니다. 자세한 내용은 [장치 정보 속성](device-information-properties.md)을 참조하세요.                                                                |

 

## <a name="devicepicker-ui"></a>DevicePicker UI


[**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841)는 Windows에서 제공하는 컨트롤이며, 사용자가 목록에서 장치를 선택할 수 있는 작은 UI를 만듭니다. 몇 가지 방법으로 **DevicePicker** 창을 사용자 지정할 수 있습니다.

-   [**SupportedDeviceSelectors**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicepickerfilter.supporteddeviceselectors.aspx)와 [**SupportedDeviceClasses**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicepickerfilter.supporteddeviceclasses.aspx) 중 하나 또는 둘 다를 [**DevicePicker.Filter**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicepicker.filter)에 추가하여 UI에 표시되는 장치를 제어할 수 있습니다. 대부분의 경우에서 하나의 선택기나 클래스만 추가하면 되지만 여러 개가 필요한 경우 여러 개를 추가할 수 있습니다. 여러 선택기 또는 클래스를 추가하는 경우 OR 논리 함수를 사용하여 결합합니다.
-   장치를 검색할 속성을 지정할 수 있습니다. [**DevicePicker.RequestedProperties**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicepicker.requestedproperties)에 속성을 추가하면 됩니다.
-   [**Appearance**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicepicker.appearance)를 사용하여 [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841)의 모양을 변경할 수 있습니다.
-   [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841)를 표시할 때 크기와 위치를 지정할 수 있습니다.

[**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841)가 표시되어 있는 동안 장치가 추가, 제거 또는 업데이트되면 UI의 내용이 자동으로 업데이트됩니다.

**참고** [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationkind.aspx) [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841)를 사용 하 여 지정할 수 없습니다. 특정 **DeviceInformationKind**의 디바이스를 사용하려면 [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446)를 빌드하고 고유한 UI를 제공해야 합니다.

 

미디어 콘텐츠 캐스팅 및 DIAL도 사용하려는 경우 각각 고유한 선택기를 제공합니다. 고유한 선택기는 각각 [**CastingDevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn972525) 및 [**DialDevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn946783)입니다.

## <a name="enumerate-a-snapshot-of-devices"></a>장치의 스냅숏 열거


일부 시나리오에서는 [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841)가 요구에 적합하지 않으며 더 유연한 클래스가 필요합니다. 아마도 고유한 UI를 빌드하거나 사용자에게 UI를 표시하지 않고 장치를 열거해야 할 수 있습니다. 이러한 경우 장치의 스냅숏을 열거할 수 있습니다. 여기에는 경우 현재 시스템에 연결되거나 페어링된 장치를 확인하는 작업이 포함됩니다. 그러나 이 방법은 사용 가능한 장치의 스냅숏만 찾으므로 목록을 열거한 후 연결되는 장치를 찾을 수는 없습니다. 또한 장치가 업데이트 또는 제거되는 경우 알려주지 않습니다. 또 다른 잠재적 단점은 전체 열거형이 완료될 때까지 아무 결과도 표시되지 않는다는 점입니다. 그렇기 때문에 **AssociationEndpoint** **AssociationEndpointContainer** 또는 **AssociationEndpointService** 개체에 관심이 있는 경우에는 이 방법을 사용해서는 안 됩니다. 이러한 개체는 네트워크 또는 무선 프로토콜을 통해 검색됩니다. 이 방법은 완료하는 데 최대 30초가 걸릴 수 있습니다. 해당 시나리오에서는 [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446) 개체를 만들어 가능한 장치를 열거해야 합니다.

장치의 스냅숏을 열거하려면 [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.findallasync.aspx) 메서드를 사용합니다. 이 메서드는 전체 열거 프로세스가 완료될 때까지 대기하고 모든 결과를 하나의 [**DeviceInformationCollection**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationcollection.aspx) 개체로 반환합니다. 또한 이 메서드는 결과를 필터링하고 관심 있는 장치로 제한할 수 있는 여러 옵션을 제공하기 위해 오버로드됩니다. 이를 위해 [**DeviceClass**](https://msdn.microsoft.com/library/windows/apps/BR225381)를 제공하거나 장치 선택기를 전달할 수 있습니다. 장치 선택기는 열거할 장치를 지정하는 AQS 문자열입니다. 자세한 내용은 [장치 선택기 빌드](build-a-device-selector.md)를 참조하세요.

디바이스 열거형 스냅숏의 예는 다음과 같습니다.



결과를 제한하는 것 외에, 장치를 검색할 속성을 지정할 수도 있습니다. 이렇게 하면 컬렉션으로 반환된 각 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 개체의 속성 모음에서 지정한 속성을 사용할 수 있습니다. 모든 장치 종류에 모든 속성을 사용할 수 있는 것은 아닙니다. 장치 종류에 따라 사용할 수 있는 속성을 확인하려면 [장치 정보 속성](device-information-properties.md)을 참조하세요.



## <a name="enumerate-and-watch-devices"></a>디바이스 열거 및 감시


장치를 열거하는 더 강력하고 유연한 방법은 [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446)를 만드는 것입니다. 이 옵션을 장치를 열거할 때 최고의 유연성을 제공합니다. 현재 있는 장치를 열거할 수 있을 뿐 아니라 장치 선택기와 일치하는 장치가 추가 또는 제거되거나 속성이 변경되는 경우 알림을 받을 수도 있습니다. **DeviceWatcher**를 만들 때는 장치 선택기를 제공합니다. 장치 선택기에 대한 자세한 내용은 [장치 선택기 빌드](build-a-device-selector.md) 참조하세요. 감시자를 만든 후에 제공한 조건과 일치하는 장치가 있으면 다음 알림이 표시됩니다.

-   새 장치 추가되는 경우 추가 알림
-   관심 있는 속성이 변경되는 경우 업데이트 알림
-   장치를 더 이상 사용할 수 있거나 장치가 필터와 더 이상 일치하지 않는 경우 제거 알림

[**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446)를 사용하는 경우 대부분 장치 목록을 유지 관리하고 목록에 장치를 추가하거나, 목록에서 항목을 제거하거나, 감시자가 감시 중인 장치에서 업데이트를 받을 때 항목을 업데이트할 수 있습니다. 업데이트 알림을 받는 경우 업데이트된 정보를 [**DeviceInformationUpdate**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationupdate.aspx) 개체로 사용할 수 있습니다. 장치 목록을 업데이트하려면 먼저 변경된 적절 한 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393)를 찾습니다. 그런 다음 **DeviceInformationUpdate** 개체를 제공하여 해당 개체의 [**Update**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.update) 메서드를 호출합니다. 이 편리한 기능은 **DeviceInformation** 개체를 자동으로 업데이트합니다.

[**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446)는 디바이스가 도착할 때와 변경될 때 알림을 보내므로 네트워킹 또는 무선 프로토콜을 통해 열거되는 **AssociationEndpoint**, **AssociationEndpointContainer** 또는 **AssociationEndpointService** 개체에 관심이 있는 경우 이 방법을 사용하여 디바이스를 열거해야 합니다.

[**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446)를 만들려면 [**CreateWatcher**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.createwatcher.aspx) 메서드 중 하나를 사용합니다. 이러한 메서드는 관심 있는 장치를 지정할 수 있도록 오버로드됩니다. 이를 위해 [**DeviceClass**](https://msdn.microsoft.com/library/windows/apps/BR225381)를 제공하거나 장치 선택기를 전달할 수 있습니다. 장치 선택기는 열거할 장치를 지정하는 AQS 문자열입니다. 자세한 내용은 [장치 선택기 빌드](build-a-device-selector.md)를 참조하세요. 또한 장치에 대해 검색할 관심 있는 속성도 지정할 수 있습니다. 이렇게 하면 컬렉션으로 반환된 각 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 개체의 속성 모음에서 지정한 속성을 사용할 수 있습니다. 모든 장치 종류에 모든 속성을 사용할 수 있는 것은 아닙니다. 장치 종류에 따라 사용 가능한 속성을 확인하려면 [장치 정보 속성](device-information-properties.md)을 참조하세요.

## <a name="watch-devices-as-a-background-task"></a>백그라운드 작업으로 장치 감시


백그라운드 작업으로 장치를 감시하는 작업은 위에서 설명한 [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446) 만들기와 유사합니다. 실제로 이전 섹션에 설명된 대로 먼저 일반 **DeviceWatcher** 개체를 만들어야 합니다. 이 개체를 만들었으면 [**DeviceWatcher.Start**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicewatcher.start) 대신 [**GetBackgroundTrigger**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicewatcher.enumerationcompleted.aspx)를 호출합니다. **GetBackgroundTrigger**를 호출할 때 받고 싶은 알림의 종류 즉, 추가, 제거 또는 업데이트를 지정해야 합니다. 업데이트 또는 제거를 요청하려면 추가도 요청해야 합니다. 트리거를 등록하면 **DeviceWatcher**가 백그라운드에서 즉시 실행되기 시작합니다. 이때부터 기준에 일치하는 응용 프로그램에 대한 새로운 알림을 받을 때마다 백그라운드 작업이 트리거되고 응용 프로그램을 마지막으로 트리거한 이후의 최신 변경 내용을 제공합니다.

**중요 한** [**DeviceWatcherTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn913838) 응용 프로그램을 트리거하는 처음으로 감시자 **EnumerationCompleted** 상태에 도달 하면 됩니다. 따라서 초기 결과가 모두 포함됩니다. 이후 응용 프로그램을 트리거할 때는 마지막 트리거 이후 발생한 추가, 업데이트 및 제거 알림만 포함됩니다. 이와 약간 다르게 포그라운드 [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446) 개체의 경우 초기 결과가 한번에 하나씩 제공되지 않고 **EnumerationCompleted**에 도달한 후 번들로만 제공됩니다.

 

일부 무선 프로토콜은 포그라운드와 백그라운드에서 검색할 때 다르게 작동하거나 백그라운드 검색을 전혀 지원하지 않을 수 있습니다. 백그라운드 검색과 관련하여 세 가지 가능성이 있습니다. 다음 표에는 이 가능성과 응용 프로그램에 미칠 수 있는 영향이 나와 있습니다. 예를 들어 Bluetooth 및 Wi-Fi Direct는 백그라운드 검색을 지원하지 않으므로, 확장명으로 [**DeviceWatcherTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn913838)를 지원하지 않습니다.

| 동작                                  | 영향                                                                                                                                  |
|-------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| 백그라운드에서 동일하게 동작               | 없음                                                                                                                                    |
| 백그라운드에서 수동 검색만 가능 | 수동 검색이 수행될 때까지 기다리느라 장치 검색이 더 오래 걸릴 수 있습니다.                                                           |
| 백그라운드 검색이 지원되지 않음            | [**DeviceWatcherTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn913838)에서 장치를 검색할 수 없고 업데이트가 보고되지 않습니다. |

 

[**DeviceWatcherTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn913838)에 백그라운드 작업으로 검색을 지원하지 않는 프로토콜이 포함되어 있어도 트리거는 계속 작동합니다. 그러나 해당 프로토콜을 통해 업데이트 또는 결과 가져올 수 없습니다. 다른 프로토콜 또는 장치에 대한 업데이트는 계속 정상적으로 검색됩니다.

## <a name="using-deviceinformationkind"></a>DeviceInformationKind 사용


대부분의 시나리오에서는 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 개체의 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationkind.aspx)는 신경 쓰지 않아도 됩니다. 사용 중인 장치 API에서 반환되는 장치 선택기에서는 해당 API와 함께 사용할 적절한 장치 종류를 가져오도록 보장하는 경우가 많기 때문입니다. 그러나 일부 시나리오에서는 장치에 대한 **DeviceInformation**를 가져오려고 하지만 장치 선택기를 제공하는 해당하는 장치 API가 없을 수 있습니다. 이러한 경우 선택기를 직접 빌드해야 합니다. 예를 들어 Web Services on Devices는 전용 API 없지만 [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) API를 사용하여 해당 장치를 검색하고 장치에 대한 정보를 가져온 다음 소켓 API를 사용하여 장치를 사용할 수 있습니다.

장치 개체를 열거하는 장치 선택기를 직접 빌드하는 경우 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationkind.aspx)에 대해 이해해야 합니다. 가능한 모든 종류 및 다른 종류와의 관계는 **DeviceInformationKind**의 참조 페이지에 설명되어 있습니다. **DeviceInformationKind**의 가장 일반적인 용도 중 하나는 장치 선택기와 함께 쿼리를 제출할 때 검색할 장치 종류를 지정하는 것입니다. 이렇게 하면 제공된 **DeviceInformationKind**와 일치하는 장치만 열거할 수 있습니다. 예를 들어 **DeviceInterface** 개체를 찾은 다음 부모 **Device** 개체에 대한 정보를 가져오는 쿼리를 실행할 수 있습니다. 해당 부모 개체는 추가 정보를 포함할 수 있습니다.

[**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 개체의 속성 모음에서 사용할 수 있는 속성은 장치의 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationkind.aspx)에 따라 달라집니다. 일부 속성은 특정 종류에서만 사용할 수 있습니다. 어떤 속성을 어떤 종류에 사용할 수 있는지에 대한 자세한 내용은 [장치 정보 속성](device-information-properties.md)을 참조하세요. 따라서 위의 예제에서 부모 **Device**를 검색하면 **DeviceInterface** 장치 개체에서 사용할 수 없던 추가 정보에 액세스할 수 있습니다. 이 때문에 AQS 필터 문자열을 만들 때는 열거하는 **DeviceInformationKind** 개체에 대해 요청한 속성을 사용할 수 있는지 확인하는 일이 중요합니다. 필터를 빌드하는 방법에 대한 자세한 내용은 [장치 선택기 빌드](build-a-device-selector.md)를 참조하세요.

**AssociationEndpoint**, **AssociationEndpointContainer** 또는 **AssociationEndpointService** 개체를 열거할 때는 무선 또는 네트워크 프로토콜을 통해 열거합니다. 이러한 상황에서는 [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.findallasync.aspx)를 사용하지 말고 대신 [**CreateWatcher**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.createwatcher.aspx)를 사용하는 것이 좋습니다. 네트워크를 검색할 때는 [**EnumerationCompleted**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicewatcher.enumerationcompleted.aspx)를 생성하기 전에 10초 이상 동안 검색 작업이 시간 초과되지 않습니다. **FindAllAsync**는 **EnumerationCompleted**가 트리거될 때까지 작업을 완료하지 않습니다. [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446)를 사용하면 **EnumerationCompleted**가 호출된 시점에 상관없이 실시간에 보다 가깝게 결과를 얻을 수 있습니다.

## <a name="save-a-device-for-later-use"></a>나중에 사용하기 위해 장치 저장


모든 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 개체는 [**DeviceInformation.Id**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.id)와 [**DeviceInformation.Kind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.kind.aspx)라는 두 가지 정보의 조합으로 고유하게 식별됩니다. 이 두 가지 정보를 유지하면 **DeviceInformation** 개체가 손실된 후 [**CreateFromIdAsync**](https://msdn.microsoft.com/library/windows/apps/br225425.aspx)에 이 정보를 제공하여 개체를 다시 만들 수 있습니다. 이 작업을 수행하는 경우 앱과 통합되는 장치에 대한 사용자 기본 설정을 저장할 수 있습니다.


 

 




