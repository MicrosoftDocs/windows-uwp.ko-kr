---
ms.assetid: D20C8E01-4E78-4115-A2E8-07BB3E67DDDC
description: 이 문서에서는 디바이스 램프가 있는 경우 이러한 램프에 액세스하고 사용하는 방법을 보여 줍니다. 램프 기능은 디바이스의 카메라 및 카메라 플래시 기능과는 별도로 관리됩니다.
title: 카메라 독립적 플래시
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5fd69900995a51af806c99b25aae43149d6e30a7
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7971078"
---
# <a name="camera-independent-flashlight"></a>카메라 독립적 플래시



이 문서에서는 디바이스 램프가 있는 경우 이러한 램프에 액세스하고 사용하는 방법을 보여 줍니다. 램프 기능은 디바이스의 카메라 및 카메라 플래시 기능과는 별도로 관리됩니다. 이 문서에서는 램프에 대한 참조를 획득하고 해당 설정을 조정하는 것 외에, 사용하지 않을 때 램프 리소스를 적절히 해제하는 방법과 다른 앱에서 사용되고 있을 때 램프 가용성이 달라지는 경우를 감지하는 방법도 설명합니다.

## <a name="get-the-devices-default-lamp"></a>디바이스의 기본 램프 가져오기

디바이스의 기본 램프 디바이스를 가져오려면 [**Lamp.GetDefaultAsync**](https://msdn.microsoft.com/library/windows/apps/dn894327)를 호출합니다. 램프 API는 [**Windows.Devices.Lights**](https://msdn.microsoft.com/library/windows/apps/dn894331) 네임스페이스에 있습니다. 이러한 API에 액세스하려고 하기 전에 이 네임스페이스에 대한 using 지시문을 추가해야 합니다.

[!code-cs[LightsNamespace](./code/Lamp/cs/MainPage.xaml.cs#SnippetLightsNamespace)]


[!code-cs[DeclareLamp](./code/Lamp/cs/MainPage.xaml.cs#SnippetDeclareLamp)]


[!code-cs[GetDefaultLamp](./code/Lamp/cs/MainPage.xaml.cs#SnippetGetDefaultLamp)]

반환된 개체가 **null**이면 **Lamp** API는 디바이스에서 지원되지 않습니다. 일부 디바이스는 실제로 램프가 있더라도 **Lamp** API를 지원하지 않을 수 있습니다.

## <a name="get-a-specific-lamp-using-the-lamp-selector-string"></a>램프 선택기 문자열을 사용하여 특정 램프 가져오기

일부 디바이스에는 둘 이상의 램프가 있을 수 있습니다. 디바이스에서 사용할 수 있는 램프 목록을 보려면 [**GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/dn894328)를 호출하여 디바이스 선택기 문자열을 가져옵니다. 그런 후에 이 선택기 문자열을 [**DeviceInformation.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432)에 전달할 수 있습니다. 이 메서드는 다양한 종류의 디바이스를 열거하는 데 사용되며 선택기 문자열은 해당 메서드에 램프가 있는 디바이스만 반환하도록 알립니다. **FindAllAsync**에서 반환된 [**DeviceInformationCollection**](https://msdn.microsoft.com/library/windows/apps/br225395) 개체는 디바이스에서 사용할 수 있는 램프를 나타내는 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393) 개체 컬렉션입니다. 목록에서 개체 중 하나를 선택한 다음 [**Id**](https://msdn.microsoft.com/library/windows/apps/br225437) 속성을 [**Lamp.FromIdAsync**](https://msdn.microsoft.com/library/windows/apps/dn894326)에 전달하여 요청된 램프에 대한 참조를 가져옵니다. 이 예제에서는 **System.Linq** 네임스페이스의 **GetFirstOrDefault** 확장 메서드를 사용하여 [**EnclosureLocation.Panel**](https://msdn.microsoft.com/library/windows/apps/br229906) 속성 값이 **Back**인 **DeviceInformation** 개체를 선택합니다. 이 속성 값을 지정하면 디바이스 인클로저 뒷면에 있는 램프(있는 경우)가 선택됩니다.

[**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393) API는 [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/br225459) 네임스페이스에 있습니다.

[!code-cs[EnumerationNamespace](./code/Lamp/cs/MainPage.xaml.cs#SnippetEnumerationNamespace)]

[!code-cs[GetLampWithSelectionString](./code/Lamp/cs/MainPage.xaml.cs#SnippetGetLampWithSelectionString)]

## <a name="adjust-lamp-settings"></a>램프 설정 조정

[**Lamp**](https://msdn.microsoft.com/library/windows/apps/dn894310) 클래스의 인스턴스가 있으면 [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/dn894330) 속성을 **true**로 설정하여 램프를 켭니다.

[!code-cs[LampSettingsOn](./code/Lamp/cs/MainPage.xaml.cs#SnippetLampSettingsOn)]

[**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/dn894330) 속성을 **false**로 설정하여 램프를 끕니다.

[!code-cs[LampSettingsOff](./code/Lamp/cs/MainPage.xaml.cs#SnippetLampSettingsOff)]

일부 디바이스에는 색 값을 지원하는 램프가 있습니다. [**IsColorSettable**](https://msdn.microsoft.com/library/windows/apps/dn894329) 속성을 확인하여 램프가 색을 지원하는지 확인합니다. 이 값이 **true**이면 [**Color**](https://msdn.microsoft.com/library/windows/apps/dn894322) 속성을 사용하여 램프 색을 설정할 수 있습니다.

[!code-cs[LampSettingsColor](./code/Lamp/cs/MainPage.xaml.cs#SnippetLampSettingsColor)]

## <a name="register-to-be-notified-if-the-lamp-availability-changes"></a>램프 가용성이 변경되면 알림을 받도록 등록

액세스를 요청하는 가장 최근의 앱에 램프 액세스가 허용됩니다. 따라서 다른 앱이 시작되고 앱이 현재 사용 중인 램프 리소스를 요청하면 현재 사용 중인 앱은 다른 앱이 리소스를 해제할 때까지 램프를 제어할 수 없게 됩니다. 램프의 가용성이 변경될 때 알림을 받으려면 [**Lamp.AvailabilityChanged**](https://msdn.microsoft.com/library/windows/apps/dn894317) 이벤트에 대한 처리기를 등록합니다.

[!code-cs[AvailabilityChanged](./code/Lamp/cs/MainPage.xaml.cs#SnippetAvailabilityChanged)]

이벤트 처리기에서 [**LampAvailabilityChanged.IsAvailable**](https://msdn.microsoft.com/library/windows/apps/dn894315) 속성을 확인하여 램프를 사용할 수 있는지 확인합니다. 이 예제에서는 램프 가용성에 따라 램프를 켜고 끄는 토글 스위치를 사용하거나 사용하지 않도록 설정합니다.

[!code-cs[AvailabilityChangedHandler](./code/Lamp/cs/MainPage.xaml.cs#SnippetAvailabilityChangedHandler)]

## <a name="properly-dispose-of-the-lamp-resource-when-not-in-use"></a>사용되지 않는 램프 리소스를 적절히 해제

램프를 더 이상 사용하지 않을 경우 사용하지 않도록 설정하고 [**Lamp.Close**](https://msdn.microsoft.com/library/windows/apps/dn894320)를 호출하여 리소스를 해제하고 다른 앱이 램프에 액세스할 수 있도록 해야 합니다. C#을 사용하는 경우 이 속성은 **Dispose** 메서드에 매핑됩니다. [**AvailabilityChanged**](https://msdn.microsoft.com/library/windows/apps/dn894317)에 등록한 경우 램프 리소스를 해제할 때 처리기를 등록 취소해야 합니다. 램프 리소스를 해제하기 위한 코드의 오른쪽 부분은 앱에 따라 다릅니다. 램프 액세스 범위를 단일 페이지로 지정하려면 [**OnNavigatingFrom**](https://msdn.microsoft.com/library/windows/apps/br227509) 이벤트의 리소스를 사용 가능하게 해제합니다.

[!code-cs[DisposeLamp](./code/Lamp/cs/MainPage.xaml.cs#SnippetDisposeLamp)]

## <a name="related-topics"></a>관련 항목
- [미디어 재생](media-playback.md)

 




