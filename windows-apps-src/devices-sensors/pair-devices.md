---
ms.assetid: F8A741B4-7A6A-4160-8C5D-6B92E267E6EA
title: 디바이스 페어링
description: 일부 장치는 페어링된 후에 사용 해야 사용할 수 있습니다. Windows. Enumeration 네임 스페이스는 장치를 페어링 하는 세 가지 다른 방법을 지원 합니다.
ms.date: 04/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: d3fff5a8868cfe29c944336a33c8b6554b74ebf4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175447"
---
# <a name="pair-devices"></a>디바이스 페어링



**중요 API**

- [**Windows.Devices.Enumeration**](/uwp/api/Windows.Devices.Enumeration)

일부 장치는 페어링된 후에 사용 해야 사용할 수 있습니다. [**Windows. Enumeration**](/uwp/api/Windows.Devices.Enumeration) 네임 스페이스는 장치를 페어링 하는 세 가지 다른 방법을 지원 합니다.

-   자동 페어링
-   기본 페어링
-   사용자 지정 페어링

**팁**    일부 장치는 사용 하기 위해 페어링된 필요가 없습니다. 자동 페어링에 대 한 섹션에서 설명 합니다.

 

## <a name="automatic-pairing"></a>자동 페어링


응용 프로그램에서 장치를 사용 하려고 하지만 장치가 페어링 되었는지 여부를 고려 하지 않는 경우가 있습니다. 장치와 연결 된 기능을 사용할 수 있습니다. 예를 들어 앱이 웹캠에서 이미지를 캡처하려는 경우에는 이미지 캡처만 장치 자체에 관심이 있는 것은 아닙니다. 관심이 있는 장치에 사용할 수 있는 장치 Api가 있는 경우이 시나리오는 자동 페어링 아래에 있습니다.

이 경우에는 장치와 연결 된 Api를 사용 하 여 필요에 따라 호출 하 고 시스템을 신뢰 하 여 필요할 수 있는 모든 페어링을 처리할 수 있습니다. 일부 장치는 해당 기능을 사용 하기 위해 페어링된 필요가 없습니다. 장치를 쌍으로 연결 해야 하는 경우 장치 Api가 백그라운드에서 페어링 작업을 처리 하므로 해당 기능을 앱에 통합할 필요가 없습니다. 앱은 지정 된 장치가 페어링 되었는지 아니면 반드시 필요한 지에 대 한 정보를 얻을 수 없지만 장치에 액세스 하 여 해당 기능을 사용할 수는 있습니다.

## <a name="basic-pairing"></a>기본 페어링


기본 페어링은 응용 프로그램에서 장치를 페어링 하기 위해 [**Windows.**](/uwp/api/Windows.Devices.Enumeration) i. i. i a api를 사용 하는 경우입니다. 이 시나리오에서는 Windows에서 페어링 프로세스를 시도 하 여 처리할 수 있습니다. 사용자 상호 작용이 필요한 경우 Windows에서 처리 됩니다. 장치와 페어링 해야 하 고 자동 연결을 시도 하는 관련 장치 API가 없는 경우에는 기본 페어링을 사용 합니다. 장치를 사용 하 여 먼저 장치를 쌍으로 연결 해야 합니다.

기본 페어링을 시도 하려면 먼저 관심 있는 장치에 대 한 [**Deviceinformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 개체를 가져와야 합니다. 해당 개체를 받은 후에는 [**Deviceinformation페어링**](/uwp/api/windows.devices.enumeration.deviceinformation.pairing) 개체 인 [**deviceinformation. 페어링**](/uwp/api/windows.devices.enumeration.deviceinformation.pairing) 속성을 조작 합니다. 페어링을 시도 하려면 [**PairAsync**](/uwp/api/windows.devices.enumeration.deviceinformationpairing.pairasync)를 호출 하면 됩니다. 앱 시간을 연결 하 여 페어링 작업을 완료할 수 있도록 하려면 **결과를 대기** 해야 합니다. 페어링 작업의 결과가 반환 되 고 오류가 반환 되지 않으면 장치가 페어링 됩니다.

기본 페어링을 사용 하는 경우 장치의 페어링 상태에 대 한 추가 정보에 액세스할 수도 있습니다. 예를 들어 페어링 상태 ([**IsPaired**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationPairing.IsPaired))와 장치가 쌍을 연결할 수 있는지 여부 ([**canpair**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationPairing.CanPair))를 알고 있습니다. 둘 다 [**Deviceinformationpairing**](/uwp/api/windows.devices.enumeration.deviceinformation.pairing) 개체의 속성입니다. 자동 페어링을 사용 하는 경우 관련 [**deviceinformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 개체를 가져오지 않으면이 정보에 액세스할 수 없습니다.

## <a name="custom-pairing"></a>사용자 지정 페어링


사용자 지정 페어링 기능을 사용 하면 앱이 페어링 프로세스에 참여할 수 있습니다. 이를 통해 앱은 페어링 프로세스에 대해 지원 되는 [**Devicep락 Ing종류**](/uwp/api/Windows.Devices.Enumeration.DevicePairingKinds) 를 지정할 수 있습니다. 또한 필요에 따라 사용자와 상호 작용 하는 사용자 인터페이스를 만들 수 있습니다. 앱이 페어링 프로세스를 진행 하는 방법에 좀 더 영향을 주거나 사용자의 페어링 사용자 인터페이스를 표시 하려면 사용자 지정 페어링을 사용 합니다.

사용자 지정 페어링을 구현 하기 위해 기본 페어링과 마찬가지로 관심이 있는 장치에 대 한 [**Deviceinformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 개체를 가져와야 합니다. 그러나에 관심이 있는 특정 속성은 [**Deviceinformation입니다. 페어링. 사용자 지정**](/uwp/api/windows.devices.enumeration.deviceinformationpairing.custom). 그러면 [**Deviceinformationcustompairing**](/uwp/api/windows.devices.enumeration.deviceinformationcustompairing) 개체가 제공 됩니다. 모든 [**Deviceinformationcustompairing**](/uwp/api/windows.devices.enumeration.deviceinformationcustompairing.pairasync) 메서드는 [**Devicep ing종류**](/uwp/api/Windows.Devices.Enumeration.DevicePairingKinds) 매개 변수를 포함 해야 합니다. 사용자가 장치를 페어링 하기 위해 수행 해야 하는 작업을 나타냅니다. 다양 한 종류와 사용자가 수행 해야 하는 작업에 대 한 자세한 내용은 **Devicep락 Ing종류** 참조 페이지를 참조 하세요. 기본 페어링과 마찬가지로 연결 작업을 완료 하기 위해 앱 시간을 제공 하기 위해 **결과를 대기** 해야 합니다. 페어링 작업의 결과가 반환 되 고 오류가 반환 되지 않으면 장치가 페어링 됩니다.

사용자 지정 페어링을 지원 하려면 [**PairingRequested**](/uwp/api/windows.devices.enumeration.deviceinformationcustompairing.pairingrequested) 이벤트에 대 한 처리기를 만들어야 합니다. 이 처리기는 사용자 지정 페어링 시나리오에 사용 될 수 있는 다양 한 모든 [**Devicep락 지정 종류**](/uwp/api/Windows.Devices.Enumeration.DevicePairingKinds) 를 고려해 야 합니다. 수행할 적절 한 작업은 이벤트 인수의 일부로 제공 된 **Devicep락 설정 종류** 에 따라 달라 집니다.

사용자 지정 페어링이 항상 시스템 수준 작업이라는 것을 인식해야 합니다. 이로 인해 데스크톱 또는 Windows Phone에서 작업 하는 경우 페어링이 발생 하면 시스템 대화 상자가 항상 사용자에 게 표시 됩니다. 이러한 플랫폼은 사용자 동의가 필요한 사용자 경험을 posses 때문입니다. 해당 대화 상자를 자동으로 생성 하므로 이러한 플랫폼에서 작업 하는 경우에 **만** 이 대화 [**상자를 직접**](/uwp/api/Windows.Devices.Enumeration.DevicePairingKinds) 만들 필요는 없습니다. 다른 **devicep락 설정 종류**의 경우 특정 **devicep락 ing종류** 값에 따라 몇 가지 특수 한 처리를 수행 해야 합니다. 다른 **Devicep락 Ing종류별로** 사용자 지정 쌍을 처리 하는 방법에 대 한 예제는 샘플을 참조 하세요.

Windows 10 버전 1903부터 새 **DevicepProvidePasswordCredential Ing종류가** 지원 됩니다 ( **ProvidePasswordCredential**). 이 값은 연결 된 장치로 인증 하기 위해 앱에서 사용자의 사용자 이름과 암호를 요청 해야 함을 의미 합니다. 이 경우를 처리 하려면 **PairingRequested** 이벤트 처리기의 이벤트 인수에 대 한 [**Acceptwithpasswordcredential**](/uwp/api/windows.devices.enumeration.devicepairingrequestedeventargs.acceptwithpasswordcredential?branch=release-19h1#Windows_Devices_Enumeration_DevicePairingRequestedEventArgs_AcceptWithPasswordCredential_Windows_Security_Credentials_PasswordCredential_) 메서드를 호출 하 여 페어링을 적용 합니다. 사용자 이름 및 암호를 매개 변수로 캡슐화 하는 [**Passwordcredential**](/uwp/api/windows.security.credentials.passwordcredential) 개체를 전달 합니다. 원격 장치에 대 한 사용자 이름 및 암호는 로컬에 로그인 한 사용자에 대 한 자격 증명과는 다르며 종종 동일 하지 않습니다.

## <a name="unpairing"></a>연결 해제


장치를 페어링 해제 하는 것은 위에서 설명한 기본 또는 사용자 지정 페어링 시나리오 에서만 관련이 있습니다. 자동 페어링을 사용 하는 경우 앱은 장치의 페어링 상태에 명확한 상태로 유지 되며 연결 해제 필요가 없습니다. 장치를 연결 해제 하도록 선택 하는 경우 기본 또는 사용자 지정 페어링을 구현 하는지 여부에 관계 없이 프로세스는 동일 합니다. 이는 추가 정보를 제공 하거나 연결 해제 프로세스에서 상호 작용할 필요가 없기 때문입니다.

장치를 페어링 해제 하는 첫 번째 단계는 연결 해제 장치에 대 한 [**Deviceinformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 개체를 가져오는 것입니다. 그런 다음 Deviceinformation을 검색 해야 [**합니다. 페어링**](/uwp/api/windows.devices.enumeration.deviceinformation.pairing) 속성 및 [**UnpairAsync**](/uwp/api/windows.devices.enumeration.deviceinformationpairing.unpairasync)를 호출 합니다. 페어링과 마찬가지로 **결과를 기다리는** 것이 좋습니다. 연결 해제 작업의 결과가 반환 되 고 오류가 반환 되지 않으면 장치는 페어링되지 않습니다.

## <a name="sample"></a>예제


[**Windows.**](/uwp/api/Windows.Devices.Enumeration) i a. i a api를 사용 하는 방법을 보여 주는 샘플을 다운로드 하려면 [여기](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing)를 클릭 하세요.

 

 