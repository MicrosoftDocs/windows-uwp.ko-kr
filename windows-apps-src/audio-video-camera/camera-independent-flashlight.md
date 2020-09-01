---
ms.assetid: D20C8E01-4E78-4115-A2E8-07BB3E67DDDC
description: 이 문서에서는 디바이스 램프가 있는 경우 이러한 램프에 액세스하고 사용하는 방법을 보여 줍니다. 전구 기능은 장치의 카메라 및 카메라 플래시 기능에서 별도로 관리 됩니다.
title: 카메라 독립적 플래시
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5e682aabea061ad89cd36e135d5c6a83245c6cbb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161077"
---
# <a name="camera-independent-flashlight"></a>카메라 독립적 플래시



이 문서에서는 디바이스 램프가 있는 경우 이러한 램프에 액세스하고 사용하는 방법을 보여 줍니다. 전구 기능은 장치의 카메라 및 카메라 플래시 기능에서 별도로 관리 됩니다. 이 문서는 램프에 대 한 참조를 획득 하 고 설정을 조정 하는 것 외에도, 사용 하지 않을 때 램프 리소스를 적절 하 게 해제 하는 방법과 다른 앱에서 사용 하는 경우 램프의 가용성이 변경 되는 경우를 감지 하는 방법을 보여 줍니다.

## <a name="get-the-devices-default-lamp"></a>장치의 기본 램프 가져오기

장치의 기본 램프 장치를 얻으려면 [**GetDefaultAsync**](/uwp/api/windows.devices.lights.lamp.getdefaultasync)를 호출 합니다. 램프 Api는 [**Windows. Devices**](/uwp/api/Windows.Devices.Lights) 네임 스페이스에 있습니다. 이러한 Api에 액세스를 시도 하기 전에이 네임 스페이스에 대 한 using 지시문을 추가 해야 합니다.

[!code-cs[LightsNamespace](./code/Lamp/cs/MainPage.xaml.cs#SnippetLightsNamespace)]


[!code-cs[DeclareLamp](./code/Lamp/cs/MainPage.xaml.cs#SnippetDeclareLamp)]


[!code-cs[GetDefaultLamp](./code/Lamp/cs/MainPage.xaml.cs#SnippetGetDefaultLamp)]

반환 된 개체가 **null**이면 장치에서 **램프** API가 지원 되지 않습니다. 장치에 실제로 장치가 있는 경우에도 일부 장치는 **램프** API를 지원 하지 않을 수 있습니다.

## <a name="get-a-specific-lamp-using-the-lamp-selector-string"></a>램프 선택기 문자열을 사용 하 여 특정 램프 가져오기

일부 장치에는 두 개 이상의 램프가 있을 수 있습니다. 장치에서 사용 가능한 램프 목록을 가져오려면 [**Getdeviceselector**](/uwp/api/windows.devices.lights.lamp.getdeviceselector)를 호출 하 여 장치 선택기 문자열을 가져옵니다. 그런 다음이 선택기 문자열을 [**Deviceinformation**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync)에 전달할 수 있습니다. 이 메서드는 다양 한 종류의 장치를 열거 하는 데 사용 되 고 선택기 문자열을 사용 하 여 램프 장치만 반환 하도록 메서드를 지정할 수 있습니다. **FindAllAsync** 에서 반환 된 [**deviceinformationcollection**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationCollection) 개체는 장치에서 사용 가능한 램프를 나타내는 [**deviceinformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 개체의 컬렉션입니다. 목록에서 개체 중 하나를 선택 하 고 [**Id**](/uwp/api/windows.devices.enumeration.deviceinformation.id) 속성을 [**FromIdAsync**](/uwp/api/windows.devices.lights.lamp.fromidasync) 에 전달 하 여 요청 된 램프에 대 한 참조를 가져옵니다. 이 예제에서는 GetFirstOrDefault **네임 스페이스의** **GetFirstOrDefault** 확장 메서드를 사용 하 여 [**EnclosureLocation**](/uwp/api/windows.devices.enumeration.enclosurelocation.panel) 속성의 값이 **back**인 **deviceinformation** 개체를 선택 합니다 .이 값은 장치의 인클로저 후면에 있는 램프 (있는 경우)를 선택 합니다.

[**Deviceinformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) Api는 [**Windows. Devices**](/uwp/api/Windows.Devices.Enumeration) 네임 스페이스에 있습니다.

[!code-cs[EnumerationNamespace](./code/Lamp/cs/MainPage.xaml.cs#SnippetEnumerationNamespace)]

[!code-cs[GetLampWithSelectionString](./code/Lamp/cs/MainPage.xaml.cs#SnippetGetLampWithSelectionString)]

## <a name="adjust-lamp-settings"></a>램프 설정 조정

[**램프**](/uwp/api/Windows.Devices.Lights.Lamp) 클래스의 인스턴스를 만든 후에는 [**IsEnabled**](/uwp/api/windows.devices.lights.lamp.isenabled) 속성을 **true**로 설정 하 여 램프를 켜 세요.

[!code-cs[LampSettingsOn](./code/Lamp/cs/MainPage.xaml.cs#SnippetLampSettingsOn)]

[**IsEnabled**](/uwp/api/windows.devices.lights.lamp.isenabled) 속성을 **false**로 설정 하 여 램프를 끕니다.

[!code-cs[LampSettingsOff](./code/Lamp/cs/MainPage.xaml.cs#SnippetLampSettingsOff)]

일부 장치에는 색 값을 지 원하는 전구가 있습니다. [**Iscolorsettable**](/uwp/api/windows.devices.lights.lamp.iscolorsettable) 수 있는 속성을 확인 하 여 램프에서 색을 지원 하는지 확인 합니다. 이 값이 **true**이면 [**색**](/uwp/api/windows.devices.lights.lamp.color) 속성을 사용 하 여 램프 색을 설정할 수 있습니다.

[!code-cs[LampSettingsColor](./code/Lamp/cs/MainPage.xaml.cs#SnippetLampSettingsColor)]

## <a name="register-to-be-notified-if-the-lamp-availability-changes"></a>램프 가용성이 변경 될 경우 알리도록 등록

액세스를 요청 하기 위해 최신 앱에 램프 액세스가 부여 됩니다. 따라서 다른 앱이 시작 되 고 앱에서 현재 사용 중인 램프 리소스를 요청 하는 경우 앱은 다른 앱이 리소스를 해제할 때까지 더 이상 램프를 제어할 수 없습니다. 램프 가용성이 변경 될 때 알림을 받으려면 [**AvailabilityChanged**](/uwp/api/windows.devices.lights.lamp.availabilitychanged) 이벤트에 대 한 처리기를 등록 합니다.

[!code-cs[AvailabilityChanged](./code/Lamp/cs/MainPage.xaml.cs#SnippetAvailabilityChanged)]

이벤트에 대 한 처리기에서 [**LampAvailabilityChanged**](/uwp/api/windows.devices.lights.lampavailabilitychangedeventargs.isavailable) 속성을 확인 하 여 램프를 사용할 수 있는지 확인 합니다. 이 예제에서는 램프를 켜고 끌 수 있는 토글 스위치를 램프 가용성에 따라 사용 하거나 사용 하지 않도록 설정 합니다.

[!code-cs[AvailabilityChangedHandler](./code/Lamp/cs/MainPage.xaml.cs#SnippetAvailabilityChangedHandler)]

## <a name="properly-dispose-of-the-lamp-resource-when-not-in-use"></a>사용 중이 아닌 경우 램프 리소스를 적절 하 게 삭제 합니다.

더 이상 램프를 사용 하지 않는 경우 사용 하지 않도록 설정 하 고 램프를 호출 해야 [**합니다.**](/uwp/api/windows.devices.lights.lamp.close) 리소스를 해제 하 고 다른 앱이 램프에 액세스 하도록 허용 합니다. C #을 사용 하는 경우이 속성은 **Dispose** 메서드에 매핑됩니다. [**AvailabilityChanged**](/uwp/api/windows.devices.lights.lamp.availabilitychanged)에 등록 한 경우 램프 리소스를 삭제할 때 처리기를 등록 취소 해야 합니다. 코드에서 램프 리소스를 삭제 하는 올바른 장소는 앱에 따라 달라 집니다. 램프에서 단일 페이지에 대 한 액세스 범위를 만들려면 [**OnNavigatingFrom**](/uwp/api/windows.ui.xaml.controls.page.onnavigatingfrom) 이벤트에서 리소스를 해제 합니다.

[!code-cs[DisposeLamp](./code/Lamp/cs/MainPage.xaml.cs#SnippetDisposeLamp)]

## <a name="related-topics"></a>관련 항목
- [미디어 재생](media-playback.md)

 