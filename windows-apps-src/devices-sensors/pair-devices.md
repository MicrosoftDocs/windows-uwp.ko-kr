---
author: DBirtolo
ms.assetid: F8A741B4-7A6A-4160-8C5D-6B92E267E6EA
title: 장치 페어링
description: 일부 디바이스는 페어링해야 사용할 수 있습니다. Windows.Devices.Enumeration 네임스페이스는 세 가지 방법으로 디바이스를 페어링하도록 지원합니다.
---
# 디바이스 페어링

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


** 중요 API **

-   [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459)

일부 디바이스는 페어링해야 사용할 수 있습니다. [
            **Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) 네임스페이스는 세 가지 방법으로 장치를 페어링하도록 지원합니다.

-   자동 페어링
-   기본 페어링
-   사용자 지정 페어링

**팁** 일부 디바이스는 사용하기 위해 페어링하지 않아도 됩니다. 이는 자동 페어링에 대한 섹션에서 설명합니다.

 

## 자동 페어링


때로 응용 프로그램에서 장치를 사용하려 하지만 장치를 페어링할지 여부는 신경 쓰지 않습니다. 그저 장치와 연결된 기능을 사용할 수 있기를 원합니다. 예를 들어 앱을 통해 단순히 웹캠에서 이미지를 캡처하려는 경우 장치 자체에는 관심을 두지 않고 이미지 캡처에만 관심을 두면 됩니다. 관심 있는 장치에 사용할 수 있는 장치 API가 있는 경우 이 시나리오는 자동 페어링에 해당됩니다.

이 경우 단순히 장치와 관련된 API를 사용하여 필요에 따라 호출을 만들고 필요한 페어링을 처리하기 위해 시스템을 신뢰합니다. 일부 장치는 사용자가 해당 기능을 사용하기 위해 페어링하지 않아도 됩니다. 장치를 페어링해야 하는 경우 장치 API가 백그라운드에서 페어링 작업을 처리하므로 사용자가 해당 기능을 앱으로 통합할 필요가 없습니다. 앱이 지정된 장치와 페어링했는지 또는 페어링해야 할지 여부를 알지 못해도 여전히 장치에 액세스하고 해당 기능을 사용할 수 있습니다.

## 기본 페어링


기본 페어링은 응용 프로그램에서 장치를 쌍으로 연결하기 위해 [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) API를 사용하는 것입니다. 이 시나리오에서는 Windows에서 페어링 프로세스를 시도하고 처리하도록 합니다. 사용자 작업이 필요한 경우에는 Windows에서 처리됩니다. 장치와 쌍으로 연결해야 하지만 자동 페어링을 시도할 관련 장치 API가 없는 경우 기본 페어링을 사용합니다. 장치를 사용할 수 있어야 하며 먼저 장치와 쌍으로 연결해야 합니다.

기본 페어링을 시도하려면 먼저 관심 있는 디바이스에 대한 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 개체를 가져와야 합니다. 해당 개체를 받으면 [**DeviceInformation.Pairing**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.deviceinformation.pairing.aspx) 속성을 조작합니다. 이제 [**DeviceInformationPairing**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.deviceinformation.pairing.aspx) 개체입니다. 페어링을 시도하려면 [**DeviceInformationPairing.PairAsync**](https://msdn.microsoft.com/library/windows/apps/mt608800)를 호출하면 됩니다. 페어링 작업을 완료할 시간을 앱에 제공하려면 결과를 **await**해야 합니다. 페어링 작업의 결과가 반환되며, 오류가 반환되지 않는 한 디바이스가 페어링됩니다.

기본 페어링을 사용하는 경우 장치의 페어링 상태에 대한 추가 정보에도 액세스할 수 있습니다. 예를 들어 페어링 상태([**IsPaired**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.deviceinformation.pairing.aspx_ispaired)) 및 디바이스를 페어링할 수 있는지 여부([**CanPair**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.deviceinformation.pairing.aspx_canpair))를 알 수 있습니다. 이러한 두 가지는 모두 [**DeviceInformationPairing**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.deviceinformation.pairing.aspx) 개체의 속성입니다. 자동 페어링을 사용하는 경우 관련 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 개체를 가져오지 않는 한 이 정보에 액세스하지 못할 수도 있습니다.

## 사용자 지정 페어링


사용자 지정 페어링을 통해 앱에서 페어링 프로세스에 참여할 수 있습니다. 이 경우 앱에서 페어링 프로세스에 대해 지원되는 [**DevicePairingKinds**](https://msdn.microsoft.com/library/windows/apps/Mt608808)를 지정할 수 있습니다. 필요에 따라 사용자와 상호 작용하기 위한 고유 사용자 인터페이스를 만들어야 할 수도 있습니다. 앱에서 페어링 프로세스가 진행되는 방식에 좀 더 많은 영향을 주려는 경우 또는 고유한 페어링 사용자 인터페이스를 표시하려는 경우 사용자 지정 페어링을 사용합니다.

사용자 지정 페어링을 구현하려면 기본 페어링과 마찬가지로 관심 있는 디바이스에 대한 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 개체를 가져와야 합니다. 그러나 관심 있는 특정 속성은 [**DeviceInformation.Pairing.Custom**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.deviceinformation.pairing.aspx_custom)입니다. 그러면 [**DeviceInformationCustomPairing**](https://msdn.microsoft.com/library/windows/apps/BR225393custompairing) 개체가 제공됩니다. 모든 [**DeviceInformationCustomPairing.PairAsync**](https://msdn.microsoft.com/library/windows/apps/BR225393custompairing_pairasync) 메서드에는 [**DevicePairingKinds**](https://msdn.microsoft.com/library/windows/apps/Mt608808) 매개 변수가 포함되어야 합니다. 이는 사용자가 디바이스를 페어링하기 위해 수행해야 하는 작업을 나타냅니다. 여러 종류 및 사용자가 수행해야 하는 작업에 대한 자세한 내용은 **DevicePairingKinds** 참조 페이지를 참조하세요. 기본 페어링과 마찬가지로 페어링 작업을 완료할 시간을 앱에 제공하려면 결과를 **await**해야 합니다. 페어링 작업의 결과가 반환되며, 오류가 반환되지 않는 한 디바이스가 페어링됩니다.

사용자 지정 페어링을 지원하려면 [**PairingRequested**](https://msdn.microsoft.com/library/windows/apps/BR225393custompairing_pairingrequested) 이벤트에 대한 처리기를 만들어야 합니다. 이 처리기가 사용자 지정 페어링 시나리오에서 사용할 수 있는 여러 가지의 모든 [**DevicePairingKinds**](https://msdn.microsoft.com/library/windows/apps/Mt608808)를 고려하도록 확인합니다. 수행할 적절한 작업은 이벤트 인수의 일부로 제공되는 **DevicePairingKinds**에 따라 달라집니다.

사용자 지정 페어링이 항상 시스템 수준 작업이라는 것을 인식해야 합니다. 이 때문에 데스크톱 또는 Windows Phone에서 작업할 때 페어링이 발생할 경우 시스템 대화 상자가 사용자에게 항상 표시됩니다. 이는 해당하는 두 플랫폼이 모두 사용자 동의가 필요한 사용자 환경을 보유하기 때문입니다. 대화 상자가 자동으로 생성되므로 이러한 플랫폼에서 작업하는 경우 **ConfirmOnly**의 [**DevicePairingKinds**](https://msdn.microsoft.com/library/windows/apps/Mt608808)를 선택할 때 고유한 대화 상자를 만들지 않아도 됩니다. 다른 **DevicePairingKinds**의 경우 특정 **DevicePairingKinds** 값에 따라 어떤 특별한 처리를 수행해야 합니다. 다른 **DevicePairingKinds** 값에 대해 사용자 지정 페어링을 처리하는 방법의 예제는 샘플을 참조하세요.

## 언페어링


장치 언페어링은 위에서 설명한 기본 페어링 또는 사용자 지정 페어링 시나리오에만 관련이 있습니다. 자동 페어링을 사용하는 경우 앱이 여전히 장치의 페어링 상태를 인식하지 못하므로 언페어링할 필요가 없습니다. 장치를 언페어링하기로 선택한 경우 기본 페어링과 사용자 지정 페어링 모두에 대해 프로세스가 동일합니다. 이는 언페어링 프로세스에 추가 정보를 제공하거나 조작할 필요가 없기 때문입니다.

디바이스를 언페어링하는 첫 번째 단계는 언페어링하려는 디바이스의 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 개체를 가져오는 것입니다. 그런 다음 [**DeviceInformation.Pairing**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.deviceinformation.pairing.aspx) 속성을 검색하고 [**DeviceInformationPairing.UnpairAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationpairing.unpairasync)를 호출해야 합니다. 페어링과 마찬가지로 결과를 **await**합니다. 언페어링 작업의 결과가 반환되며, 오류가 반환되지 않는 한 디바이스가 언페어링됩니다.

## 샘플


[
            **Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) API를 사용하는 방법을 보여 주는 샘플을 다운로드하려면 [여기](http://go.microsoft.com/fwlink/?LinkID=620536)를 클릭하세요.

 

 






<!--HONumber=May16_HO2-->


