---
ms.assetid: 25B18BA5-E584-4537-9F19-BB2C8C52DFE1
title: 앱 접근 권한 값 선언
description: '사진, 음악 또는 디바이스(예: 카메라 또는 마이크)와 같은 특정 API 또는 리소스에 액세스하려면 Windows 앱의 패키지 매니페스트에서 접근 권한 값을 선언해야 합니다.'
ms.date: 02/10/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: ed06779b2f8a8a38320a7292bea17bf1b64a43d6
ms.sourcegitcommit: 4df27104a9e346d6b9fb43184812441fe5ea3437
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96025189"
---
# <a name="app-capability-declarations"></a>앱 접근 권한 값 선언

사진, 음악 또는 디바이스(예: 카메라 또는 마이크)와 같은 특정 Windows 10 API 또는 리소스에 액세스하려면 Windows 앱의 [패키지 매니페스트](/uwp/schemas/appxpackage/appx-package-manifest)에서 접근 권한 값을 선언해야 합니다. 접근 권한 값은 UWP 앱과 Windows 10용 MSIX 또는 AppX 패키지에 패키징된 기타 유형의 데스크톱 앱에 사용됩니다.

앱의 [패키지 매니페스트](/uwp/schemas/appxpackage/appx-package-manifest)에서 접근 권한 값을 선언하여 특정 리소스 또는 API에 대한 액세스를 요청합니다. Visual Studio에서 [매니페스트 디자이너](/windows/msix/package/packaging-uwp-apps#configure-an-app-package)를 사용하여 일반 접근 권한 값을 선언하거나 수동으로 추가할 수 있습니다. 자세한 내용은 [패키지 매니페스트에 접근 권한 값을 지정하는 방법](/uwp/schemas/appxpackage/how-to-specify-capabilities-in-a-package-manifest)을 참조하세요. 고객은 스토어에서 앱 구입 시 앱에서 선언한 모든 접근 권한 값에 관한 알림을 받아야 합니다. 앱에 필요하지 않은 접근 권한 값은 선언하지 마세요.

일부 접근 권한 값은 앱에 *중요한 리소스* 에 대한 액세스 권한을 제공합니다. 이러한 리소스는 사용자의 개인 데이터에 액세스하거나 사용자의 비용이 들 수 있으므로 중요한 것으로 간주됩니다. 설정 앱에서 관리되는 개인 정보 설정을 통해 사용자는 중요한 리소스에 대한 액세스를 동적으로 제어할 수 있습니다. 따라서 앱에서는 중요한 리소스를 항상 사용 가능한 것으로 가정하지 않는 것이 중요합니다. 중요한 리소스에 액세스하는 방법에 대한 자세한 내용은 [개인정보 인식 앱에 대한 지침](../security/index.md)을 참조하세요. 앱에 *중요한 리소스* 에 대한 액세스 권한을 제공하는 기능은 기능 시나리오 옆에 별표(\*)로 주석 처리됩니다.

다음과 같은 여러 가지 유형의 접근 권한 값이 있습니다.

- 대부분의 공통적인 앱 시나리오에 적용되는 [범용 접근 권한 값](#general-use-capabilities)입니다.
- [디바이스 접근 권한 값](#device-capabilities)은 앱이 주변 디바이스 및 내부 디바이스에 액세스할 수 있게 합니다.
- Microsoft Store에 제출하려면 승인을 받아야 하고/하거나 일반적으로 Microsoft와 특정 파트너에게만 제공되는 [제한된 접근 권한 값](#restricted-capabilities)입니다.
- [사용자 지정 접근 권한 값](#custom-capabilities)입니다.

## <a name="general-use-capabilities"></a>범용 접근 권한 값

범용 접근 권한 값은 앱 패키지 매니페스트의 **Capability** 요소를 사용하여 지정합니다. 이러한 접근 권한 값은 대부분의 공통적인 앱 시나리오에 적용됩니다.

> [!NOTE]
> 모든 **Capability** 요소는 패키지 매니페스트의 **Capabilities** 노드 아래에 있는 [CustomCapability](#custom-capabilities) 및 [DeviceCapability](#device-capabilities) 요소 앞에 있어야 합니다.

| 접근 권한 값 시나리오 | 접근 권한 값 사용 |
|---------------------|------------------|
| **음악**\* | **musicLibrary** 접근 권한 값은 사용자의 음악 라이브러리에 대한 프로그래밍 방식 액세스를 제공하여 애플리케이션이 사용자 개입 없이 라이브러리의 모든 파일을 열거하고 액세스할 수 있도록 합니다. 이 접근 권한 값은 주로 전체 음악 라이브러리를 사용해야 하는 주크박스 앱에서 사용됩니다.<br /><br />[  **file picker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker)는 사용자가 앱에서 사용할 파일을 열 수 있는 강력한 UI 메커니즘을 제공합니다. 앱의 시나리오에서 프로그래밍 방식 액세스가 필요하고 **file picker** 를 사용하여 이 시나리오를 실현할 수 없는 경우에만 **musicLibrary** 접근 권한 값을 선언하세요.<br /><br /> **musicLibrary** 접근 권한 값은 아래 표시된 대로 앱의 패키지 매니페스트에서 선언할 때 **uap** 네임스페이스를 포함해야 합니다. <br /><br />```<Capabilities><uap:Capability Name="musicLibrary"/></Capabilities>```
| **그림**\* | **picturesLibrary** 접근 권한 값은 사용자의 그림 라이브러리에 대한 프로그래밍 방식 액세스를 제공하여 애플리케이션이 사용자 개입 없이 라이브러리의 모든 파일을 열거하고 액세스할 수 있도록 합니다. 이 접근 권한 값은 주로 전체 사진 라이브러리를 사용해야 하는 사진 앱에서 사용됩니다.<br /><br />[  **file picker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker)는 사용자가 앱에서 사용할 파일을 열 수 있는 강력한 UI 메커니즘을 제공합니다. 앱의 시나리오에서 프로그래밍 방식 액세스가 필요하고 **file picker** 를 사용하여 이 시나리오를 실현할 수 없는 경우에만 **picturesLibrary** 접근 권한 값을 선언하세요.<br /><br />**picturesLibrary** 접근 권한 값은 아래 표시된 대로 앱의 패키지 매니페스트에서 선언할 때 **uap** 네임스페이스를 포함해야 합니다.<br /><br />```<Capabilities><uap:Capability Name="picturesLibrary"/></Capabilities>```
| **동영상**\* | **videosLibrary** 접근 권한 값을 사용하면 사용자의 동영상에 프로그래밍 방식으로 액세스할 수 있으므로 앱이 사용자 조작 없이 라이브러리의 모든 파일을 열거하고 액세스할 수 있습니다. 이 접근 권한 값은 주로 전체 동영상 라이브러리를 사용해야 하는 동영상 재생 앱에서 사용됩니다.<br /><br />[  **file picker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker)는 사용자가 앱에서 사용할 파일을 열 수 있는 강력한 UI 메커니즘을 제공합니다. 앱의 시나리오에서 프로그래밍 방식 액세스가 필요하고 **file picker** 를 사용하여 이 시나리오를 실현할 수 없는 경우에만 **videosLibrary** 접근 권한 값을 선언하세요.<br /><br />**videosLibrary** 접근 권한 값은 아래 표시된 대로 앱의 패키지 매니페스트에서 선언할 때 **uap** 네임스페이스를 포함해야 합니다.<br /><br />```<Capabilities><uap:Capability Name="videosLibrary"/></Capabilities>```
| **이동식 스토리지** | **removableStorage** 접근 권한 값은 패키지 매니페스트에 정의된 파일 형식 연결로 필터링하여 USB 키 및 외장형 하드 드라이브 등, 이동식 저장소의 파일에 대한 프로그래밍 방식의 액세스를 제공합니다. 예를 들어 문서 뷰어 앱이 .doc 파일 형식 연결을 선언했다면 이동식 저장 장치에서 .doc 파일은 열 수 있지만 다른 형식의 파일은 열 수 없습니다. 사용자가 이동식 저장 장치에 다양한 정보를 포함할 수 있고 앱에서 선언된 형식의 모든 파일에 이동식 스토리지의 올바른 프로그래밍 방식 액세스에 대한 유효한 사유를 제공할 것으로 기대하므로 이 접근 권한 값을 선언할 때 주의해야 합니다.<br /><br />사용자는 앱에서 선언하는 모든 파일 연결을 처리할 것으로 기대합니다. 따라서 앱이 처리할 수 있다고 확신할 수 없는 파일 연결은 선언하지 마세요. [  **file picker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker)는 사용자가 앱에서 사용할 파일을 열 수 있는 강력한 UI 메커니즘을 제공합니다.<br /><br />앱의 시나리오에서 프로그래밍 방식 액세스가 필요하고 [**file picker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker)를 사용하여 이 시나리오를 실현할 수 없는 경우에만 **removableStorage** 접근 권한 값을 선언하세요.<br /><br />**removableStorage** 접근 권한 값은 아래 표시된 대로 앱의 패키지 매니페스트에서 선언할 때 **uap** 네임스페이스를 포함해야 합니다.<br /><br />```<Capabilities><uap:Capability Name="removableStorage"/></Capabilities>```
| **인터넷 및 공용 네트워크**\* | 인터넷 및 공용 네트워크에 대해 다양한 수준의 액세스를 제공하는 두 가지 접근 권한 값이 있습니다.<br /><br />**internetClient** 접근 권한 값은 앱이 인터넷에서 들어오는 데이터를 받을 수 있음을 나타냅니다. 서버 역할을 수행할 수 없습니다. 로컬 네트워크 액세스 권한이 없습니다.<br />**internetClientServer** 접근 권한 값은 앱이 인터넷에서 들어오는 데이터를 받을 수 있음을 나타냅니다. 서버 역할을 수행할 수 있습니다. 로컬 네트워크 액세스 권한이 없습니다.<br /><br />웹 서비스 구성 요소가 있는 대부분의 앱은 **internetClient** 를 사용합니다. 앱이 들어오는 네트워크 연결을 수신 대기해야 하는 P2P(피어 투 피어) 시나리오를 사용하는 앱은 **internetClientServer** 를 사용해야 합니다. **internetClientServer** 접근 권한 값에는 **internetClient** 접근 권한 값에서 제공하는 액세스가 포함되므로 **internetClientServer** 를 지정할 경우 **internetClient** 를 지정할 필요가 없습니다.
| **가정 및 회사 네트워크**\* | **privateNetworkClientServer** 접근 권한 값은 방화벽을 통해 가정 및 회사 네트워크에 대한 인바운드 및 아웃바운드 액세스를 제공합니다. 이 접근 권한 값은 주로 LAN(Local Area Network)을 통해 통신하는 게임 및 다양한 로컬 디바이스를 통해 데이터를 공유하는 앱에 사용됩니다. 앱에서 **musicLibrary**, **picturesLibrary** 또는 **videosLibrary** 를 지정하는 경우 홈 그룹의 해당 라이브러리에 액세스하기 위해 이 접근 권한 값을 사용하지 않아도 됩니다. Windows에서는 이 접근 권한 값을 통해 인터넷에 액세스할 수 없습니다.
| **약속** | **appointments** 접근 권한 값은 사용자의 약속 저장소에 액세스할 수 있게 합니다. 이 접근 권한 값은 동기화된 네트워크 계정에서 획득한 약속 및 약속 저장소에 기록하는 다른 앱에 대해 읽기 권한으로 액세스할 수 있도록 합니다. 이 접근 권한 값을 사용하면 앱이 새 일정을 만들고 만든 일정에 약속을 쓸 수 있습니다.<br /><br />**appointments** 접근 권한 값은 아래 표시된 대로 앱의 패키지 매니페스트에서 선언할 때 **uap** 네임스페이스를 포함해야 합니다.<br /><br />```<Capabilities><uap:Capability Name="appointments"/></Capabilities>```
| **연락처**\* | **contacts** 접근 권한 값은 다양한 연락처 저장소의 연락처를 집계한 단일 보기에 액세스할 수 있게 합니다. 이 접근 권한 값을 통해 앱은 다양한 네트워크 및 로컬 연락처 저장소에서 동기화된 연락처에 제한적으로 액세스할 수 있습니다(네트워크 허용 규칙 적용).<br /><br />**contacts** 접근 권한 값은 아래 표시된 대로 앱의 패키지 매니페스트에서 선언할 때 **uap** 네임스페이스를 포함해야 합니다.<br /><br />```<Capabilities><uap:Capability Name="contacts"/></Capabilities>```
| **코드 생성** | 앱에서 **codeGeneration** 접근 권한 값을 통해 앱에 JIT 접근 권한 값을 제공하는 다음 기능에 액세스할 수 있습니다.<br /><br />[**VirtualProtectFromApp**](/windows/desktop/api/memoryapi/nf-memoryapi-virtualprotectfromapp)<br />[**CreateFileMappingFromApp**](/windows/desktop/api/memoryapi/nf-memoryapi-createfilemappingfromapp)<br />[**OpenFileMappingFromApp**](/windows/desktop/api/memoryapi/nf-memoryapi-openfilemappingfromapp)<br />[**MapViewOfFileFromApp**](/windows/desktop/api/memoryapi/nf-memoryapi-mapviewoffilefromapp)
| **AllJoyn** | **allJoyn** 접근 권한 값을 통해 네트워크의 AllJoyn 사용 앱 및 디바이스가 서로를 검색하고 상호 작용할 수 있습니다.<br /><br />[  **Windows.Devices.AllJoyn**](/uwp/api/Windows.Devices.AllJoyn) 네임스페이스의 API에 액세스하는 모든 앱은 이 접근 권한 값을 사용해야 합니다.
| **전화 통화** | **phoneCall** 접근 권한 값을 통해 앱은 디바이스의 모든 전화 회선에 액세스하고 다음 기능을 수행할 수 있습니다.<ul><li>사용자에게 메시지를 표시하지 않고 전화 회선에 전화를 걸고 시스템 다이얼러를 표시합니다.</li><li>회선 관련 메타데이터에 액세스합니다.</li><li>회선 관련 트리거에 액세스합니다.</li><li>사용자가 선택한 스팸 필터 앱이 차단 목록 및 통화 발신 정보를 설정하고 확인할 수 있도록 합니다.</li></ul>**phoneCall** 접근 권한 값은 아래 표시된 대로 앱의 패키지 매니페스트에서 선언할 때 **uap** 네임스페이스를 포함해야 합니다.<br /><br />```<Capabilities><uap:Capability Name="phoneCall"/></Capabilities>```<br /><br />앱에서 <b>phoneCallHistoryPublic</b> 접근 권한 값을 통해 디바이스의 셀룰러 및 일부 VoIP 통화 기록 정보를 읽을 수 있습니다. 또한 앱에서 이 접근 권한 값을 통해 VoIP 통화 기록 항목을 쓸 수도 있습니다. <a href="/uwp/api/Windows.ApplicationModel.Calls.PhoneCallHistoryStore">  <b>PhoneCallHistoryStore</b></a> 클래스의 모든 구성원에 액세스하려면 이 접근 권한 값이 필요합니다.
| **녹음된 통화 폴더**\* | **recordedCallsFolder** 디바이스 접근 권한 값을 통해 앱은 녹음된 통화 폴더에 액세스할 수 있습니다.<br /><br />**recordedCallsFolder** 접근 권한 값은 아래 표시된 대로 앱의 패키지 매니페스트에서 선언할 때 **mobile** 네임스페이스를 포함해야 합니다.<br /><br />```<Capabilities><mobile:Capability Name="recordedCallsFolder"/></Capabilities>```
| **사용자 계정 정보**\* | **userAccountInformation** 접근 권한 값을 통해 앱은 사용자의 이름 및 사진에 액세스할 수 있습니다.<br /><br />[  **Windows.System.UserProfile**](/uwp/api/Windows.System.UserProfile) 네임스페이스에서 일부 API에 액세스하려면 이 접근 권한 값이 필요합니다.<br /><br />**userAccountInformation** 접근 권한 값은 아래 표시된 대로 앱의 패키지 매니페스트에서 선언할 때 **uap** 네임스페이스를 포함해야 합니다.<br /><br />```<Capabilities><uap:Capability Name="userAccountInformation"/></Capabilities>```
| **VoIP 호출** | **voipCall** 접근 권한 값을 통해 앱은 [**Windows.ApplicationModel.Calls**](/uwp/api/Windows.ApplicationModel.Calls) 네임스페이스의 VoIP 호출 API에 액세스할 수 있습니다.<br /><br />**voipCall** 접근 권한 값은 아래 표시된 대로 앱의 패키지 매니페스트에서 선언할 때 **uap** 네임스페이스를 포함해야 합니다.<br /><br />```<Capabilities><uap:Capability Name="voipCall"/></Capabilities>```
| **3D 개체** | 앱에서는 **objects3D** 접근 권한 값을 통해 3D 개체 파일에 프로그래밍 방식으로 액세스할 수 있습니다. 이 접근 권한 값은 주로 전체 3D 개체 라이브러리에 액세스해야 하는 3D 앱과 게임에서 사용됩니다.<br /><br />이 접근 권한 값은 [**Windows.Storage**](/uwp/api/Windows.Storage) 네임스페이스에서 API를 사용하여 3D 개체를 포함하는 폴더에 액세스하는 데 필요합니다.<br /><br />**objects3D** 접근 권한 값은 아래 표시된 대로 앱의 패키지 매니페스트에서 선언할 때 **uap** 네임스페이스를 포함해야 합니다.<br /><br />```<Capabilities><uap:Capability Name="objects3D"/></Capabilities>```
| **읽기 차단된 메시지**\* | **blockedChatMessages** 접근 권한 값을 통해 앱은 스팸 필터 앱에 의해 차단된 SMS 및 MMS 메시지를 읽을 수 있습니다.<br /><br />이 접근 권한 값은 [**Windows.ApplicationModel.Chat**](/uwp/api/Windows.ApplicationModel.Chat) 네임스페이스에서 API를 사용하여 차단된 메시지에 액세스하는 데 필요합니다.<br /><br />**blockedChatMessages** 접근 권한 값은 아래 표시된 대로 앱의 패키지 매니페스트에서 선언할 때 **uap** 네임스페이스를 포함해야 합니다.<br /><br />```<Capabilities><uap:Capability Name="blockedChatMessages"/></Capabilities>```
| **사용자 지정 디바이스** | **lowLevelDevices** 접근 권한 값을 사용하면 많은 추가 요구 사항이 충족될 때 앱이 사용자 지정 디바이스에 액세스할 수 있습니다. 이 접근 권한 값은 GPIO, I2C, SPI 및 PWM 디바이스에 대한 액세스를 허용하는 **lowLevel** 디바이스 접근 권한 값과 혼동해서는 안 됩니다.<br /><br /> [디바이스 인터페이스](/windows-hardware/drivers/install/device-interface-classes)를 공개하는 사용자 지정 드라이버를 개발하고, 이 디바이스에 대한 핸들을 열어 IOCTL을 보내려면 다음을 수행해야 합니다. <ul><li>애플리케이션 매니페스트에서 **lowLevelDevices** 접근 권한 값 ```<Capabilities><iot:Capability Name="lowLevelDevices"/></Capabilities>```를 사용하도록 설정</li><li>[포함된 모드](/windows/iot-core/develop-your-app/EmbeddedMode)를 사용하도록 설정</li><li>[INF](/previous-versions/windows/desktop/deviceaccess/register-the-device-interface-class-as-privileged)에서 또는 드라이버에서 [WdfDeviceAssignInterfaceProperty()](/windows-hardware/drivers/ddi/content/wdfdevice/nf-wdfdevice-wdfdeviceassigninterfaceproperty)를 호출하여 디바이스 인터페이스를 [제한됨](/windows-hardware/drivers/install/devpkey-deviceinterface-restricted)으로 표시합니다.</ul>그런 다음, [**Windows.Devices.Custom.CustomDevice**](/uwp/api/Windows.Devices.Custom.CustomDevice)를 사용하여 디바이스의 핸들을 열 수 있습니다. 자세한 내용은 [내부 디바이스용 UWP 디바이스 앱](/windows-hardware/drivers/devapps/uwp-device-apps-for-specialized-devices)을 참조하세요.
| **IoT 시스템 관리** | **systemManagement** 접근 권한 값을 통해 앱은 종료 또는 다시 부팅, 로캘 및 표준 시간대와 같은 기본 시스템 관리 권한을 가질 수 있습니다.<br /><br />[  **Windows.System**](/uwp/api/Windows.System) 네임스페이스에서 일부 API에 액세스하려면 이 접근 권한 값이 필요합니다.<br /><br />**systemManagement** 접근 권한 값은 아래 표시된 대로 앱의 패키지 매니페스트에서 선언할 때 **iot** 네임스페이스를 포함해야 합니다.<br /><br />```<Capabilities><iot:Capability Name="systemManagement"/></Capabilities>```
| **백그라운드 미디어 재생** | **backgroundMediaPlayback** 접근 권한 값은 [**MediaPlayer**](/uwp/api/windows.media.playback.mediaplayer) 및 [**AudioGraph**](/uwp/api/windows.media.audio.audiograph) 클래스 같은 미디어 관련 API의 동작을 변경하여 앱이 백그라운드에 있는 동안 미디어 재생을 사용하도록 설정합니다. 모든 활성 오디오 스트림은 더 이상 음소거되지 않고 앱이 백그라운드로 전환할 때 계속 들을 수 있습니다. 또한 재생 중 자동으로 앱 수명이 확장됩니다.
| **원격 시스템** | **remoteSystem** 접근 권한 값을 통해 앱이 사용자의 Microsoft 계정과 연결된 디바이스 목록에 액세스할 수 있습니다. 디바이스 목록에 대한 액세스는 디바이스 간에 유지되는 모든 작업을 수행하는 데 필요합니다. 다음의 모든 멤버에 액세스하려면 이 접근 권한 값이 필요합니다.<ul><li>[Windows.System.RemoteSystems](/uwp/api/windows.system.remotesystems) 네임스페이스</li><li>[Windows.System.RemoteLauncher](/uwp/api/Windows.System.RemoteLauncher) 클래스</li><li>[AppServiceConnection.OpenRemoteAsync](/uwp/api/windows.applicationmodel.appservice.appserviceconnection.openremoteasync) 메서드</li></ul> |
| **공간 인식** | **spatialPerception** 접근 권한 값은 공간 매핑 데이터에 대한 프로그램 방식의 액세스를 제공하고, 사용자 근처의 공간 애플리케이션에서 지정한 지역의 표면에 대한 혼합 현실 앱 정보를 제공합니다.  사용자의 앱에서 명시적으로 표면 메시를 사용하는 경우에만 spatialPerception 접근 권한 값을 선언합니다. 이 접근 권한 값은 혼합 현실 앱에서 사용자의 머리 자세에 따라 홀로그램 렌더링을 수행하는 데 필요하지 않기 때문입니다. |
| **글로벌 미디어 컨트롤** | **globalMediaControl** 접근 권한 값을 사용하면 [**SystemMediaTransportControls**](/uwp/api/Windows.Media.SystemMediaTransportControls)와 통합된 시스템 전체의 재생 세션에 앱이 액세스하여 재생 정보를 제공하고 원격 제어를 허용할 수 있습니다. 이 접근 권한 값은 [**Windows.Media.Control**](/uwp/api/windows.media.control) 네임스페이스의 일부 API를 사용하는 데 필요합니다. 이 접근 권한 값은 [uap7:Capability](/uwp/schemas/appxpackage/uapmanifestschema/element-uap7-capability) 요소에 정의되어 있습니다.  |

## <a name="device-capabilities"></a>장치 접근 권한 값

디바이스 접근 권한 값은 앱이 주변 디바이스 및 내부 디바이스에 액세스할 수 있게 합니다. 디바이스 접근 권한 값은 앱 패키지 매니페스트의 **DeviceCapability** 요소를 사용하여 지정합니다. 이 요소에는 추가 자식 요소가 필요할 수 있으며 일부 디바이스 접근 권한 값을 패키지 매니페스트에 수동으로 추가해야 합니다. 자세한 내용은 [패키지 매니페스트에서 장치 접근 권한 값을 지정하는 방법](/uwp/schemas/appxpackage/how-to-specify-device-capabilities-in-a-package-manifest) 및 [**DeviceCapability Schema reference**](/uwp/schemas/appxpackage/uapmanifestschema/element-devicecapability)를 참조하세요.

> [!NOTE]
> 패키지 매니페스트의 **Capabilities** 요소 아래에 **DeviceCapability** 요소가 여러 개 있을 수 있습니다. 모든 **DeviceCapability** 요소는 **Capability** 및 [CustomCapability](#custom-capabilities) 요소 뒤에 나와야 합니다.

| 접근 권한 값 시나리오 | 접근 권한 값 사용 |
|---------------------|------------------|
| **위치**\* | **location** 접근 권한 값을 사용하면 PC에 있는 GPS 센서와 같은 전용 하드웨어에서 검색되거나 사용 가능한 네트워크 정보에서 파생된 위치 기능에 액세스할 수 있습니다. 앱은 사용자가 **설정** 참 메뉴에서 위치 서비스를 사용하지 않도록 설정한 경우를 해결해야 합니다. |
| **마이크** | **microphone** 접근 권한 값을 사용하면 마이크의 오디오 피드에 액세스할 수 있으므로 앱이 연결된 마이크로 오디오를 녹음할 수 있습니다. 앱은 사용자가 **설정** 참 메뉴에서 마이크를 사용하지 않도록 설정한 경우를 해결해야 합니다. |
| **근접** | **proximity** 접근 권한 값을 사용하면 근접한 여러 장치가 서로 통신할 수 있습니다. 이 접근 권한 값은 주로 캐주얼 멀티 플레이어 게임 및 정보를 교환하는 앱에서 사용됩니다. 장치에서는 Bluetooth, Wi-Fi, 인터넷을 비롯하여 최적의 가용 연결을 제공하는 통신 기술을 사용하려고 합니다. 이 접근 권한 값은 디바이스 간에 통신을 시작하는 데만 사용됩니다. |
| **웹캠** | **webcam** 접근 권한 값은 기본 제공 카메라나 외부 webcam의 화상 대화에 대한 액세스 권한을 제공하여 앱에서 사진 및 동영상을 캡처할 수 있도록 합니다. Windows에서 앱은 사용자가 **설정** 참 메뉴에서 카메라를 사용하지 않도록 설정한 경우를 처리해야 합니다.<br/>**webcam** 접근 권한 값은 비디오 스트림에 대한 액세스 권한만 부여합니다. 오디오 스트림에 대한 액세스 권한도 부여하려면 **microphone** 접근 권한 값을 추가해야 합니다. |
| **USB** | **USB** 디바이스 기능은 [USB 디바이스용 앱 매니페스트 패키지 업데이트](/windows-hardware/drivers/usbcon/)에서 API 액세스를 가능하게 합니다. |
| **HID(휴먼 인터페이스 디바이스)** | **humaninterfacedevice** 디바이스 기능은 [HID용 디바이스 기능을 지정하는 방법](/uwp/schemas/appxpackage/how-to-specify-device-capabilities-for-hid)에서 API 액세스가 가능하게 합니다. |
| **POS(Point of Service)** | **pointOfService** 장치 접근 권한 값은 [**Windows.Devices.PointOfService**](/uwp/api/Windows.Devices.PointOfService) 네임스페이스에서 API 액세스를 가능하게 합니다. 이 네임스페이스는 앱이 POS(서비스 시점) 바코드 스캐너 및 자기 띠 판독기에 액세스할 수 있게 합니다. 이 네임스페이스는 UWP Microsoft Store 앱에서 다양한 제조업체의 POS 디바이스에 액세스할 수 있는 공급업체 중립적 인터페이스를 제공합니다. |
| **Bluetooth** | **bluetooth** 장치 접근 권한 값은 앱이 GATT(일반 특성) 또는 클래식 기본 속도(RFCOMM) 프로토콜을 통해 이미 연결된 Bluetooth 장치와 통신할 수 있도록 합니다.<br/>이 접근 권한 값은 [**Windows.Devices.Bluetooth**](/uwp/api/Windows.Devices.Bluetooth) 네임스페이스의 일부 API를 사용하는 데 필요합니다. |
| **Wi-Fi 네트워킹** | **wiFiControl** 장치 접근 권한 값을 통해 앱은 Wi-Fi 네트워크를 검색하고 Wi-Fi 네트워크에 연결할 수 있습니다.<br/>이 접근 권한 값은 [**Windows.Devices.WiFi**](/uwp/api/Windows.Devices.WiFi) 네임스페이스의 일부 API를 사용하는 데 필요합니다. |
| **송수신 장치 상태** | **radios** 장치 접근 권한 값을 통해 앱은 Wi-Fi 송수신 장치와 Bluetooth 송수신 장치를 전환할 수 있습니다.<br/>이 접근 권한 값은 [**Windows.Devices.Radios**](/uwp/api/Windows.Devices.Radios) 네임스페이스의 API를 사용하는 데 필요합니다.  |
| **광 디스크** | **optical** 장치 기능을 통해 앱은 CD, DVD 및 블루레이와 같은 광 디스크 드라이브의 기능에 액세스할 수 있습니다.<br/>이 접근 권한 값은 [**Windows.Devices.Custom**](/uwp/api/Windows.Devices.Custom) 네임스페이스의 일부 API를 사용하는 데 필요합니다. |
| **동작 활동** | **activity** 장치 기능을 통해 앱은 장치의 현재 동작을 감지할 수 있습니다.<br/>이 접근 권한 값은 [**Windows.Devices.Sensors**](/uwp/api/Windows.Devices.Sensors) 네임스페이스의 일부 API를 사용하는 데 필요합니다. |
| **직렬 통신** | **serialcommunication** 디바이스 접근 권한 값은 Windows 앱이 직렬 포트나 추상 직렬 포트를 지원하는 디바이스와 통신할 수 있는 Windows.Devices.SerialCommunication 네임스페이스의 API에 대한 액세스를 제공합니다. [**Windows.Devices.SerialCommnication**](/uwp/api/windows.devices.serialcommunication) 네임스페이스의 API를 사용하기 위해 필요한 접근 권한 값입니다. |
| **시선 추적기** | 호환되는 시선 추적 디바이스가 연결되어 있거나 시선 추적을 지원하는 Mixed Reality 디바이스인 경우 앱에서 **gazeInput** 접근 권한 값을 통해 애플리케이션 범위 내에서 사용자가 보는 위치를 탐지할 수 있습니다. 이 접근 권한 값은 [**Windows.Devices.Input.Preview**](/uwp/api/windows.devices.input.preview) 네임스페이스의 일부 API를 사용하는 데 필요합니다. Mixed Reality 디바이스의 경우 이 접근 권한 값은 [**Windows.Perception.People.EyesPose**](/uwp/api/windows.perception.people.eyespose)의 API에 필요합니다. |
| **GPIO, I2C, SPI 및 PWM** | **lowLevel** 디바이스 접근 권한 값은 GPIO, I2C, SPI 및 PWM 디바이스에 대한 액세스를 제공합니다. 이 접근 권한 값은 다음 네임스페이스의 API를 사용하는 데 필요합니다. [**Windows.Devices.Gpio**](/uwp/api/windows.devices.gpio), [**Windows.Devices.I2c**](/uwp/api/windows.devices.i2c), [**Windows.Devices.Spi**](/uwp/api/windows.devices.spi),[**Windows.Devices.Pwm**](/uwp/api/windows.devices.pwm).<br /><br />```<Capabilities><DeviceCapability Name="lowLevel"/></Capabilities>``` |


<span id="special-and-restricted-capabilities" />

## <a name="restricted-capabilities"></a>제한된 접근 권한 값

앱에서 제한된 접근 권한 값을 선언하는 경우 앱을 Microsoft Store에 게시하도록 승인을 받으려면 [앱 제출 프로세스](../publish/app-submissions.md) 중에 정보를 제공해야 합니다. 제출의 [제출 옵션](../publish/manage-submission-options.md#restricted-capabilities) 페이지에서 이 정보를 제공하고, 앱에서 선언하는 각각의 제한된 접근 권한 값을 사용하는 방식을 설명해야 합니다.

> [!IMPORTANT]
> 제한된 접근 권한 값은 매우 한정적인 시나리오에 사용됩니다. 이 접근 권한 값은 사용이 엄격히 제한되며 추가 스토어 등록 정책 및 검토가 적용됩니다. 승인을 받을 필요 없이 제한된 접근 권한 값을 선언하는 앱을 테스트용으로 로드할 수 있습니다. 승인은 이러한 앱을 Microsoft Store에 제출하는 경우에만 필요합니다.

앱에서 제한된 접근 권한 값이 꼭 필요한 경우 외에는 접근 권한 값을 선언하지 마세요. 예를 들어 사용자가 자신의 ID를 확인하는 디지털 인증서와 함께 스마트 카드를 제공하는 2단계 인증을 사용하는 뱅킹과 같이 이러한 접근 권한 값이 필요하고 적합한 경우가 있습니다. 다른 예로는 기본적으로 기업 고객용으로 디자인되고 사용자의 도메인 자격 증명 없이는 액세스할 수 없는 회사 리소스에 액세스해야 하는 앱이 있습니다.

제한된 접근 권한 값을 선언하려면 [앱 패키지 매니페스트](/uwp/schemas/appxpackage/appx-package-manifest) 원본 파일(`Package.appxmanifest`)을 수정합니다. **xmlns:rescap** XML 네임스페이스 선언을 추가하고, 제한된 접근 권한 값을 선언할 때 **rescap** 접두사를 사용합니다. 예를 들어 **appCaptureSettings** 접근 권한 값을 선언하는 방법은 다음과 같습니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package
    ...
    xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    IgnorableNamespaces="... rescap">
...
<Capabilities>
    <rescap:Capability Name="appCaptureSettings"/>
</Capabilities>
</Package>
```

> [!NOTE]
> 모든 제한된 접근 권한 값 요소는 패키지 매니페스트의 **Capabilities** 노드 아래에 있는 [CustomCapability](#custom-capabilities) 및 [DeviceCapability](#device-capabilities) 요소 앞에 있어야 합니다.

### <a name="restricted-capability-approval-process"></a>제한된 접근 권한 값 승인 프로세스

이전에는 접근 권한 값을 사용하기 위해 승인을 받으려면 지원 팀에 문의해야 했습니다. 이제는 [제출 프로세스](../publish/app-submissions.md) 중에 [개발자 센터](https://partner.microsoft.com/dashboard)에서 이 정보를 제공할 수 있습니다.

제출용 패키지를 업로드할 때 제한된 접근 권한 값이 선언되었는지 확인합니다. 제한된 접근 권한 값이 선언된 것으로 확인되면 [제출 옵션](../publish/manage-submission-options.md#restricted-capabilities) 페이지에서 제품이 각 접근 권한 값을 사용하는 방식에 대한 세부 정보를 제공해야 합니다. 제품에서 접근 권한 값을 선언해야 하는 이유를 이해할 수 있도록 최대한 자세한 정보를 제공해야 합니다. 인증 프로세스를 완료하는 데 시간이 추가로 소요될 수 있습니다.

인증 과정에서 Microsoft의 테스터는 개발자가 제공한 정보를 검토하여 제출이 접근 권한 값을 사용할 수 있도록 승인되었는지 확인합니다. 인증 프로세스를 완료하는 데 시간이 추가로 소요될 수 있습니다. 접근 권한 값의 사용이 승인되면 앱은 나머지 인증 프로세스를 진행합니다. 앱에 대한 업데이트를 제출하는 경우 추가 접근 권한 값을 선언하지 않는 이상 일반적으로 접근 권한 값 승인 프로세스를 반복하지 않아도 됩니다.

접근 권한 값 사용이 승인되지 않는 경우 제출은 인증에 실패하며 인증 보고서에 피드백이 제공됩니다. 그러면 접근 권한 값을 선언하지 않는 새 제출을 만들고 패키지를 업로드하거나, 또는 해당되는 경우 접근 권한 값 사용과 관련된 모든 문제를 처리하고 새 제출에서 승인을 요청할 수 있습니다.

> [!NOTE]
> 파트너 센터에서 개발 샌드박스를 사용하여 제출하는 경우(예: Xbox Live와 통합되는 게임) **제출 옵션** 페이지에서 정보를 제공하는 것이 아니라 미리 승인을 요청해야 합니다. 이렇게 하려면 [Windows 개발자 지원 페이지](https://developer.microsoft.com/windows/support)를 방문하세요. 개발자 지원 토픽 **대시보드 문제**, 문제 유형 **앱 제출**, 하위 범주 **기타** 를 선택합니다. 그리고 접근 권한 값을 어떻게 사용할 것인지, 제품에 접근 권한 값이 필요한 이유가 무엇인지 설명합니다. 필요한 모든 정보를 제공하지 않으면 요청이 거부됩니다. 자세한 정보를 제공해야 할 수도 있습니다. 이 프로세스는 일반적으로 영업일 기준으로 5일 이상 걸리므로 미리 요청을 제출하세요.
>
> 또한 제출을 시작하기 전에 제한된 접근 권한 값을 사용하도록 승인되었는지 확인하고 싶다면 개발 샌드박스의 사용 여부에 관계 없이 승인을 요청하는 이 방법(제출 과정에서 이 정보를 제공하는 대신)을 사용할 수 있습니다.

<span id="restricted-and-special-use-capability-list" />

### <a name="restricted-capability-list"></a>제한된 접근 권한 값 목록

다음 표에는 제한된 접근 권한 값이 나열되어 있습니다. 위에 설명된 프로세스를 수행하여 Microsoft Store에 제출한 앱에서 이러한 접근 권한 값에 대한 승인을 요청할 수 있습니다.

> [!IMPORTANT]
> 이러한 제한된 접근 권한 값 중 일부는 매우 한정적이고 제한된 경우를 제외하고 Microsoft Store에 제출되는 앱에 대해 거의 승인되지 않습니다. 이러한 접근 권한 값은 아래 표에 설명되어 있습니다. Microsoft Store를 통해 배포할 계획인 경우 앱에서 이러한 접근 권한 값을 선언하지 않는 것이 좋습니다.

| 접근 권한 값 시나리오 | 접근 권한 값 사용 |
|---------------------|------------------|
| **엔터프라이즈** | Windows 도메인 자격 증명은 사용자가 자신의 자격 증명을 사용하여 원격 리소스에 로그인할 수 있도록 하며, 마치 사용자가 사용자 이름과 암호를 제공한 것처럼 작동합니다. **enterpriseAuthentication** 접근 권한 값은 엔터프라이즈 내의 서버에 연결하는 기간 업무 앱에서 일반적으로 사용됩니다. <br /><br />인터넷을 통한 일반적인 통신에는 이 기능이 필요하지 않습니다.<br /><br />**enterpriseAuthentication** 접근 권한 값은 일반적인 기간 업무 앱을 지원하기 위한 것입니다. 회사 리소스에 액세스할 필요가 없는 앱에서는 이 접근 권한 값을 선언하지 마세요. [  **파일 선택기**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker)는 사용자가 앱에서 사용할 네트워크 공유의 파일을 열 수 있는 강력한 UI 메커니즘을 제공합니다. 앱의 시나리오에서 프로그래밍 방식 액세스가 필요한데 **파일 선택기** 를 사용하여 이 시나리오를 구현할 수 없는 경우에만 **enterpriseAuthentication** 접근 권한 값을 선언하세요.<br /><br />**enterpriseAuthentication** 접근 권한 값은 아래 표시된 대로 앱의 패키지 매니페스트에서 선언할 때 **uap** 네임스페이스를 포함해야 합니다.<br /><br />```<Capabilities><uap:Capability Name="enterpriseAuthentication"/></Capabilities>```<br /><br />**enterpriseDataPolicy** 접근 권한 값을 사용하면 앱이 Windows Information Protection 정책(예: 모바일 디바이스 관리 및 모바일 애플리케이션 관리 시스템)을 통해 관리될 때 앱에서 엔터프라이즈 데이터를 별도로 안전하게 처리할 수 있습니다.  아래와 같이 제한된 접근 권한 값을 선언합니다. <br /><br />```<Capabilities><rescap:Capability Name="enterpriseDataPolicy"/></Capabilities>```<br /><br />다음 클래스의 모든 구성원을 사용하려면 이 접근 권한 값이 필요합니다.<ul><li><a href="/uwp/api/Windows.Security.EnterpriseData.FileProtectionManager">FileProtectionManager</a></li><li><a href="/uwp/api/Windows.Security.EnterpriseData.DataProtectionManager">DataProtectionManager</a></li><li><a href="/uwp/api/Windows.Security.EnterpriseData.ProtectionPolicyManager">ProtectionPolicyManager</a></li></ul> |
| **공유 사용자 인증서** | **sharedUserCertificates** 접근 권한 값을 사용하면 앱에서 스마트 카드에 저장된 인증서와 같이 공유 사용자 저장소의 소프트웨어 및 하드웨어 기반 인증서를 추가하고 액세스할 수 있습니다. 이 접근 권한 값은 주로 스마트 카드로 인증하는 금융 또는 엔터프라이즈 앱에 사용됩니다.<br /><br />**sharedUserCertificates** 접근 권한 값은 아래 표시된 대로 앱의 패키지 매니페스트에서 선언할 때 **uap** 네임스페이스를 포함해야 합니다.<br /><br />```<Capabilities><uap:Capability Name="sharedUserCertificates"/></Capabilities>``` |
|**문서**\* | **documentsLibrary** 접근 권한 값은 패키지 매니페스트에 선언된 파일 형식 연결로 필터링하여 사용자의 [문서] 라이브러리에 대한 프로그래밍 방식 액세스를 제공합니다. 예를 들어 워드 프로세싱 앱에서 .doc 파일 형식 연결을 선언한 경우 사용자의 [문서] 라이브러리에 있는 .doc 파일을 열 수 있습니다. <br /><br />**documentsLibrary** 기능은 애플리케이션에서 사용자 개입 *없이* 문서 라이브러리에 프로그래밍 방식으로 액세스하는 *경우에만* 필요합니다. 사용자가 선택기 API를 사용하여 문서 라이브러리를 선택하는 경우 **documentsLibrary** 기능이 <b>없어도</b> 애플리케이션에서 문서 라이브러리에 액세스할 수 있습니다. 일반적으로 다음 선택기 API 중 하나를 사용하여 앱에서 사용자가 파일 위치를 선택할 수 있도록 허용해야 합니다. <ul><li>기존 파일을 여는 [FileOpenPicker](/uwp/api/Windows.Storage.Pickers.FileOpenPicker)</li><li>새 파일을 저장하는 [FileSavePicker](/uwp/api/windows.storage.pickers.filesavepicker)</li><li>추가 파일을 여는/저장하는 폴더를 선택하는 [FolderPicker](/uwp/api/windows.storage.pickers.folderpicker)</li></ul>이러한 API를 사용하면 사용자는 클라우드 동기화 계정(예: OneDrive)처럼 가장 적합한 위치를 선택할 수 있습니다. 사용자가 이러한 API를 사용하여 파일 또는 폴더를 선택한 후에는 앱에서 [FutureAccessList](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) API를 사용하여 해당 위치에 지속적으로 액세스할 수 있습니다. 이 API를 사용하면 나중에 사용자에게 위치를 다시 선택하도록 요청하지 않아도 앱에서 파일 또는 폴더에 액세스할 수 있습니다.<br /><br />기존 워크플로에서 파일이 [문서] 라이브러리에 있는 것으로 가정하거나(예: 기존 데스크톱 애플리케이션을 사용하는 interop) 사용자가 위치를 선택할 필요가 없도록 하려는 경우에는 애플리케이션의 **documentsLibrary** 접근 권한 값을 선언하면 됩니다. 애플리케이션에 **documentsLibrary** 접근 권한 값을 사용할 때는 사용자가 위치를 수동으로 선택할 수 있게 하는 것이 좋습니다.<br /><br />**documentsLibrary** 접근 권한 값은 아래 표시된 대로 앱의 패키지 매니페스트에서 선언할 때 **uap** 네임스페이스를 포함해야 합니다.<br /><br />```<Capabilities><uap:Capability Name="documentsLibrary"/></Capabilities>``` |
| **게임 DVR 설정** | **appCaptureSettings** 제한된 접근 권한 값을 통해 앱은 게임 DVR에 대한 사용자 설정을 제어할 수 있습니다.<br /><br />이 접근 권한 값은 [**Windows.Media.Capture**](/uwp/api/Windows.Media.Capture) 네임스페이스의 일부 API를 사용하는 데 필요합니다. <br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다.  |
| **셀룰러** | **cellularDeviceControl** 제한된 접근 권한 값을 통해 앱은 셀룰러 장치를 제어할 수 있습니다.<br /><br />**cellularDeviceIdentity** 접근 권한 값을 통해 앱은 셀룰러 ID 데이터에 액세스할 수 있습니다.<br /><br />**cellularMessaging** 접근 권한 값을 통해 앱은 SMS 및 RCS를 사용할 수 있습니다.<br /><br />이러한 기능은 [**Windows.Devices.Sms**](/uwp/api/Windows.Devices.Sms) 네임스페이스의 일부 API를 사용하는 데 필요합니다.  |
| **디바이스 잠금 해제** | **deviceUnlock** 제한된 접근 권한 값을 통해 앱은 개발자 및 엔터프라이즈 테스트용 로드 시나리오에 대해 장치의 잠금을 해제할 수 있습니다.<br /><br /> Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
| **듀얼 SIM 타일** | **dualSimTiles** 제한된 접근 권한 값을 통해 앱은 SIM이 여러 개인 장치에서 추가 앱 목록 항목을 만들 수 있습니다.<br /><br />이 접근 권한 값은 [**Windows.UI.StartScreen**](/uwp/api/Windows.UI.StartScreen) 네임스페이스의 일부 API를 사용하는 데 필요합니다. |
| **엔터프라이즈 공유 스토리지** | **enterpriseDeviceLockdown** 제한된 접근 권한 값을 통해 앱은 장치 잠금 API를 사용하고 엔터프라이즈 공유 저장소 폴더에 액세스할 수 있습니다. <br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
| **시스템 입력 주입** | **inputInjectionBrokered** 제한된 접근 권한 값을 통해 앱은 HID, 터치, 펜, 키보드 또는 마우스와 같은 다양한 형식의 입력을 프로그래밍 방식으로 시스템에 주입할 수 있습니다. 이 접근 권한 값은 일반적으로 시스템을 제어할 수 있는 협업 앱에 사용됩니다.<br /><br />PC의 경우 이 기능이 있는 앱의 입력 주입은 동일한 앱 컨테이너의 프로세스에서만 수신됩니다.<br /><br />```<Capabilities><rescap:Capability Name="inputInjectionBrokered" /></Capabilities>``` |
| **입력 관찰**\* | **inputObservation** 제한된 접근 권한 값을 통해 앱은 최종 대상과 관계없이 시스템에서 수신되는 HID, 터치, 펜, 키보드 또는 마우스와 같은 다양한 형식의 원시 입력을 관찰할 수 있습니다.<br /><br />이 접근 권한 값 및 관련 API는 엄선된 Microsoft 파트너만 사용할 수 있습니다.  |
| **입력 표시 안 함** | **inputSuppression** 제한된 접근 권한 값을 통해 앱은 HID, 터치, 펜, 키보드 또는 마우스와 같은 다양한 형식의 원시 입력을 시스템에서 받지 않도록 할 수 있습니다.<br /><br />이 접근 권한 값 및 관련 API는 엄선된 Microsoft 파트너만 사용할 수 있습니다. |
| **VPN 앱** | **networkingVpnProvider** 제한된 접근 권한 값을 통해 앱은 연결을 관리하고 VPN 플러그 인 기능을 제공하는 기능을 비롯한 VPN 기능에 대한 모든 권한을 가질 수 있습니다.<br /><br />이 접근 권한 값은 [**Windows.Networking.Vpn**](/uwp/api/Windows.Networking.Vpn) 네임스페이스의 일부 API를 사용하는 데 필요합니다. |
| **기타 앱 관리** | **packageManagement** 제한된 접근 권한 값을 통해 앱은 다른 앱을 직접 관리할 수 있습니다.<br /><br />**packageQuery** 장치 기능을 통해 앱은 다른 앱에 대한 정보를 수집할 수 있습니다.<br /><br />이러한 접근 권한 값은 [**PackageManager**](/uwp/api/Windows.Management.Deployment.PackageManager) 클래스의 일부 메서드 및 속성에 액세스하는 데 필요합니다. |
| **화면 프로젝션** | **screenDuplication** 제한된 접근 권한 값을 통해 앱은 다른 장치의 화면에 표시할 수 있습니다.<br /><br />이 접근 권한 값은 DirectX 네임스페이스의 API를 사용하는 데 필요합니다. <br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
| **사용자 계정 이름** | **userPrincipalName** 제한된 접근 권한 값을 사용하면 앱이 현재 사용자의 UPN(사용자 계정 이름)에 액세스할 수 있습니다.<br /><br />이 접근 권한 값은 [**GetUserNameEx**](/windows/desktop/api/secext/nf-secext-getusernameexa) 함수를 호출하는 데 필요합니다. <br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
| **전자지갑** | **walletSystem** 제한된 접근 권한 값을 통해 앱은 저장된 전자지갑 카드에 대한 모든 권한을 가질 수 있습니다.<br /><br />이 접근 권한 값은 [**Windows.ApplicationModel.Wallet.System**](/uwp/api/Windows.ApplicationModel.Wallet.System) 네임스페이스의 API를 사용하는 데 필요합니다. <br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
| **위치 기록** | **locationHistory** 제한된 접근 권한 값을 통해 앱은 장치의 위치 기록에 액세스할 수 있습니다.<br /><br />이 접근 권한 값은 [**Windows.Devices.Geolocation**](/uwp/api/Windows.Devices.Geolocation) 네임스페이스의 API를 사용하는 데 필요합니다.
| **앱 닫기 확인** | **confirmAppClose** 제한된 접근 권한 값을 통해 앱은 앱 자체 및 해당 창을 닫고 앱의 닫기를 지연할 수 있습니다.<br /><br />앱에서 이 접근 권한 값에 대해 Windows 10 버전 1703(빌드 10.0.15063) 이상을 요청할 수 있습니다. Windows 10 이전 버전에서 이 기능은 비공개이며 "요청한 기능이 이 애플리케이션에 대해 인증되지 않습니다"라는 오류 메시지와 함께 앱 설치가 실패합니다. |
| **통화 기록**\* | **phoneCallHistory** 제한된 접근 권한 값을 통해 앱은 통화 기록을 읽고 기록에서 항목을 삭제할 수 있습니다.<br /><br />이 접근 권한 값은 [**Windows.ApplicationModel.Chat**](/uwp/api/Windows.ApplicationModel.Chat) 네임스페이스의 API를 사용하는 데 필요합니다. <br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
| **시스템 수준 약속 액세스** | **appointmentsSystem** 제한된 접근 권한 값을 통해 앱은 사용자 일정의 모든 약속을 읽고 수정할 수 있습니다.<br /><br />이 접근 권한 값은 [**Windows.ApplicationModel.Appointment**](/uwp/api/Windows.ApplicationModel.Appointments) 네임스페이스의 API를 사용하는 데 필요합니다. <br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
| **시스템 수준 채팅 메시지 액세스**\* | **chatSystem** 제한된 접근 권한 값을 통해 앱은 모든 SMS 및 MMS 메시지를 읽고 쓸 수 있습니다.<br />이 접근 권한 값은 [**Windows.ApplicationModel.Chat**](/uwp/api/Windows.ApplicationModel.Chat) 네임스페이스의 API를 사용하는 데 필요합니다. <br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
| **시스템 수준 연락처 액세스** | **contactsSystem** 제한된 접근 권한 값을 통해 앱은 제한되거나 중요하다고 지정된 연락처 정보를 읽고 기존 연락처 정보를 수정할 수 있습니다.<br /><br />이 접근 권한 값은 [**Windows.ApplicationModel.Chat**](/uwp/api/Windows.ApplicationModel.Chat) 네임스페이스의 API를 사용하는 데 필요합니다. <br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
| **이메일 액세스** | **email** 제한된 접근 권한 값을 통해 앱은 사용자 메일을 읽고, 심사하고, 보낼 수 있습니다.<br /><br />이 접근 권한 값은 [**Windows.ApplicationModel.Email**](/uwp/api/Windows.ApplicationModel.Email) 네임스페이스의 API를 사용하는 데 필요합니다. <br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
| **시스템 수준 이메일 액세스**| **emailSystem** 제한된 접근 권한 값을 통해 앱은 사용자가 제한하거나 중요한 메일을 읽고, 심사하고, 보낼 수 있습니다. <br /><br />이 접근 권한 값은 [**Windows.ApplicationModel.Email**](/uwp/api/Windows.ApplicationModel.Email) 네임스페이스의 API를 사용하는 데 필요합니다. <br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
| **시스템 수준 통화 기록 액세스** | **phoneCallHistorySystem** 제한된 접근 권한 값을 통해 앱은 기존 항목을 변경하고 새 항목을 작성하여 통화 기록을 완전히 수정할 수 있습니다.<br /><br />이 접근 권한 값은 [**Windows.ApplicationModel.Calls**](/uwp/api/Windows.ApplicationModel.Calls) 네임스페이스의 API를 사용하는 데 필요합니다. <br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
| **문자 메시지 보내기**\* | **smsSend** 제한된 접근 권한 값을 통해 앱은 SMS 및 MMS 메시지를 보낼 수 있습니다.<br /><br />이 접근 권한 값은 [**Windows.ApplicationModel.Chat**](/uwp/api/Windows.ApplicationModel.Chat) 네임스페이스의 API를 사용하는 데 필요합니다. |
| **모든 사용자 데이터에 대한 시스템 수준 액세스** | **userDataSystem** 제한된 접근 권한 값을 통해 앱은 사용자 데이터 시스템 데이터 저장소에 액세스할 수 있습니다. <br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
| **Microsoft Store 미리 보기 기능** | **previewStore** 제한된 접근 권한 값을 통해 앱은 앱에서 바로 구매 제품의 SKU를 검색하고 구매할 수 있습니다.<br /><br />이 접근 권한 값은 [**Windows.ApplicationModel.Store.Preview**](/uwp/api/Windows.ApplicationModel.Store.Preview) 네임스페이스의 특정 API를 사용하는 데 필요합니다. |
| **최초 로그인 설정** | **firstSignInSettings** 제한된 접근 권한 값을 통해 앱은 사용자가 장치에 처음 로그인할 때 설정된 사용자 설정에 액세스할 수 있습니다. |
| **Windows 팀 환경** | **teamEditionExperience** 제한된 접근 권한 값을 통해 앱은 Windows 팀 세션의 많은 환경적 측면을 제어하는 내부 API에 액세스할 수 있습니다. Windows 팀 세션은 Microsoft Surface Hub와 같은 팀 디바이스에서 실행될 수 있습니다. <br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
| **원격 잠금 해제** | **remotePassportAuthentication** 제한된 접근 권한 값을 통해 앱은 원격 PC의 잠금을 해제하는 데 사용될 수 있는 자격 증명에 액세스할 수 있습니다. <br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
| **미리 보기 컴퍼지션** | **previewUiComposition** 제한된 접근 권한 값을 통해 앱은 완료되기 전에 API에 대한 피드백을 제공할 수 있도록 사용자 인터페이스에 대한 [**Windows.UI.Composition**](/uwp/api/Windows.UI.Composition) 네임스페이스를 미리 볼 수 있습니다. 자세한 내용은 wincomposition@microsoft.com으로 문의하세요. |
| **보안 평가 잠금** | **secureAssessment** 제한된 접근 권한 값을 통해 앱은 보안 평가를 위해 Windows를 단일 앱 모드로 잠글 수 있습니다. <br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
| **연결 관리자 프로비저닝** | **networkConnectionManagerProvisioning** 제한된 접근 권한 값을 통해 앱은 장치를 WWAN 및 WLAN 인터페이스에 연결하는 정책을 정의할 수 있습니다. 이 접근 권한 값을 사용하는 앱은 통신사가 해당 모바일 네트워크에 연결하는 디바이스를 제어하기 위해 만듭니다. |
| **데이터 요금제 프로비저닝** | **networkDataPlanProvisioning** 제한된 접근 권한 값을 통해 앱은 장치에서 데이터 요금제에 대한 정보를 수집하고 네트워크 사용량을 읽을 수 있습니다. 이 접근 권한 값을 사용하는 앱은 통신사가 고객의 실제 데이터 사용량을 OS 데이터 사용량 설정에 통합하기 위해 만듭니다. |
| **소프트웨어 라이선스** | **slapiQueryLicenseValue** 제한된 접근 권한 값을 통해 앱은 소프트웨어 라이선싱 정책을 쿼리할 수 있습니다. <br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
| **확장 실행** | **extendedBackgroundTaskTime** 제한된 접근 권한 값을 통해 실행 시간 제한으로 인한 백그라운드 작업 취소 또는 종료를 막을 수 있습니다. 이들은 여전히 다른 모든 메모리 및 에너지 사용량 제한을 따릅니다. 이 접근 권한 값은 배터리 사용 또는 프라이버시 백그라운드 앱 설정을 사용하여 제한할 수 있습니다. 소비자 및 관리자는 그룹 정책 설정을 통해 여전히 백그라운드 작업을 제어할 수 있습니다.<br /><br />**extendedExecutionBackgroundAudio** 제한된 접근 권한 값을 통해 앱은 프로그라운드에 없을 때 오디오를 재생할 수 있습니다.<br /><br />**extendedExecutionCritical** 제한된 접근 권한 값을 통해 앱은 중요한 확장 실행 세션을 시작할 수 있습니다.<br /><br />**extendedExecutionUnconstrained** 제한된 접근 권한 값을 통해 앱은 제약이 없는 확장 실행 세션을 시작할 수 있습니다. <br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다.<br /><br />앱이 중단되었을 때 확장된 실행을 사용하여 연기하는 방법에 대한 자세한 내용은 [확장된 실행으로 앱 일시 중단 연기](../launch-resume/run-minimized-with-extended-execution.md)를 참조하세요. |
| **모바일 디바이스 관리** | **deviceManagementDmAccount** 제한된 접근 권한 값을 통해 앱은 MO OMA-DM(Mobile Operator Open Mobile Alliance - Device Management) 계정을 프로비전 및 구성할 수 있습니다.<br /><br />**deviceManagementFoundation** 제한된 접근 권한 값을 통해 앱은 장치에서 MDM(모바일 장치 관리) CSP(구성 서비스 공급자) 인프라에 대한 기본 액세스 권한을 가질 수 있습니다. 특정 CSP에 액세스하려면 다른 접근 권한 값이 필요합니다.<br /><br />**deviceManagementWapSecurityPolicies** 제한된 접근 권한 값을 통해 앱은 MM, SI/SL(서비스 표시/서비스 로드), OMA-CP(Open Mobile Alliance - Client Provisioning) 등 WAP(Wireless Application Protocol) 기반 서비스를 구성할 수 있습니다.<br /><br />**deviceManagementEmailAccount** 제한된 접근 권한 값을 통해 통신사는 사용자에게 프로비전하는 장치에서 메일 계정을 추가하고 관리하기 위한 앱을 만들 수 있습니다. |
| **패키지 정책 제어** | **packagePolicySystem** 제한된 접근 권한 값을 통해 앱은 장치에 설치된 앱과 관련된 시스템 정책을 제어할 수 있습니다.<br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
| **게임 목록** | **gameList** 제한된 접근 권한 값을 통해 앱은 시스템에 설치된 알려진 게임 목록을 가져올 수 있습니다.<br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
| **Xbox 액세서리** | **xboxAccessoryManagement** 제한된 접근 권한 값을 통해 앱은 Xbox 하드웨어 사양을 준수하는 Xbox 장치를 직접 관리할 수 있습니다.<br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
| **액세서리에 대한 음성 인식** | **cortanaSpeechAccessory** 제한된 접근 권한 값을 통해 앱은 명령을 호출하고 Cortana에 전달할 수 있습니다.<br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
| **액세서리 관리** | **accessoryManager** 제한된 접근 권한 값을 통해 앱은 액세서리로 전달되고 사용자에게 표시될 수 있도록 액세서리 앱으로 등록하고 특정 앱 알림에 옵트인(opt-in)할 수 있습니다. |
| **드라이버 액세스** | **interopServices** 제한된 접근 권한 값을 통해 앱은 드라이버를 직접 조작할 수 있습니다. <br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
| **전경 관찰** | **inputForegroundObservation** 제한된 접근 권한 값을 통해 전경의 앱은 키보드 입력을 가로채고 앱이 아닌 모든 키보드 입력 처리를 바이패스할 수 있습니다. SAS 조합은 이 접근 권한 값으로 가로챌 수 없습니다. 이 접근 권한 값은 [**KeyboardDeliveryInterceptor**](/uwp/api/Windows.UI.Input.KeyboardDeliveryInterceptor) 클래스의 구성원에 액세스하는 데 필요합니다.
| **OEM 및 MO 파트너 앱** | **oemDeployment** 제한된 접근 권한 값을 통해 Microsoft 파트너가 만든 앱은 장치에 새 앱을 설치하고 현재 설치된 앱을 쿼리할 수 있습니다.<br /><br />**oemPublicDirectory** 제한된 접근 권한 값을 통해 Microsoft 파트너가 만든 앱은 공유 앱 폴더에 액세스할 수 있습니다. Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
| **앱 라이선싱** | **appLicensing** 제한된 접근 권한 값을 통해 라이선스 없이 앱을 실행할 수 있습니다. 매니페스트에서 이 접근 권한 값을 선언하는 경우 스토어에 앱을 제출할 수 없습니다. <br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
| **위치 시스템**| **locationSystem** 제한된 접근 권한 값을 통해 앱은 디바이스의 기본 위치 설정과 같은 특정 권한 위치 구성을 수행할 수 있습니다. <br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
| **사용자 데이터 계정 공급자**| **userDataAccountsProvider** 제한된 접근 권한 값을 통해 앱은 메일, 일정 및 연락처 계정 전체를 관리할 수 있습니다. |
| **펜 작업 영역** | **previewPenWorkspace** 접근 권한 값을 통해 앱이 Windows.ApplicationModel.Preview.Notes 네임스페이스에 액세스하여 펜 작업 영역 내에 작업 기억 처리기로 호스트될 수 있습니다. |
| **보조 인증 요소** | **secondaryAuthenticationFactor** 접근 권한 값을 통해 앱이 근처 도우미 인증 디바이스의 암호 저장소를 전달하여 PC의 잠금을 해제할 수 있습니다. 예를 들어 도우미 피트니스 밴드를 사용하여 PC의 잠금을 해제할 수 있습니다. 이 접근 권한 값은 Windows.Security.Authentication.Identity.Provider 네임스페이스에서 API에 액세스하는 데 필요합니다.<br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
| **Microsoft Store 라이선스 관리**| **storeLicenseManagement** 접근 권한 값을 통해 Microsoft 파트너 허브-앱은 디바이스에서 스토어 라이선스를 관리할 수 있습니다. 이 접근 권한 값은 Windows.ApplicationModel.Store.LicenseManagement 네임스페이스에서 API에 액세스하는 데 필요합니다. |
| **사용자 시스템 ID**| **userSystemId** 접근 권한 값을 통해 앱이 사용자에게 특정한 시스템 식별자를 가져올 수 있습니다. 이 식별자는 특정 시스템에서 현재 사용자를 고유하게 식별하며 앱에서 정보를 상호 연결하는 데 사용할 수 있습니다. 이 접근 권한 값은 Windows.System.Profile.SystemIdentification 클래스에서 GetUserSpecificSystemId API에 액세스하는 데 필요합니다. |
| **대상이 지정된 콘텐츠**| **targetedContent** 제한된 접근 권한 값을 통해 애플리케이션은 [**Windows.Services.TargetedContent**](/uwp/api/windows.services.targetedcontent)가 제공하는 대상이 지정된 구독 콘텐츠를 검색 및 사용할 수 있습니다.<br /><br />이 접근 권한 값은 **Windows.System.Profile.SystemIdentification** 네임스페이스의 일부 API를 사용하는 데 필요합니다. |
| **UI 자동화**| **uiAutomation** 접근 권한 값을 통해 내레이터 같은 UI 자동화 클라이언트는 UI 자동화 서버 또는 공급자에 연결할 수 있습니다.<br /><br />이 접근 권한 값은 **Windows.Xbox.Media.Capture.Broadcaster** 네임스페이스의 일부 API를 사용하는 데 필요합니다. |
|**게임 바 서비스**| **gameBarServices** 는 자사 스토어의 업데이트 가능한 수신함 UWA로 제한됩니다.<br /><br />이 접근 권한 값은 [**Windows.Media.Capture.GameBarsSrvices**](/uwp/api/windows.media.capture.gamebarservices) 클래스를 사용하는 데 필요합니다.<br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
|**앱 캡처 서비스**| **appCaptureServices** 용량은 Microsoft와 계약상 관계를 체결한 당사자로 제한됩니다. 이러한 관계는 Xbox 서비스 및 bizdev의 도움으로 진행 중에 있는 파트너 계약에 따라 부여됩니다.<br /><br />이 접근 권한 값은 [**Windows.Media.Capture.AppCaptureServices**](/uwp/api/windows.media.capture.appcaptureservices) 클래스를 사용하는 데 필요합니다. |
|**앱 브로드캐스트 서비스**| **appBroadcastServices** 접근 권한 값은 Microsoft와 계약상 관계를 체결한 당사자로 제한됩니다. 이러한 관계는 Xbox 서비스 및 bizdev의 도움으로 진행 중에 있는 파트너 계약에 따라 부여됩니다.<br /> <br />이 접근 권한 값은 [**Windows.Media.capture.AppBroadcastServices**](/uwp/api/windows.media.capture.appbroadcastservices) 클래스를 사용하는 데 필요합니다.<br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
|**오디오 디바이스 구성**| **audioDeviceConfiguration** 이 접근 권한 값을 통해 애플리케이션은 오디오 드라이버에 노출된 오디오 효과를 쿼리, 구성, 활성화 및 비활성화할 수 있습니다. <br /> <br />이 접근 권한 값은 [**Windows.Media.Devices.AudioDeviceModulesManager**](/uwp/api/windows.media.devices.audiodevicemodulesmanager) 클래스를 사용하는 데 필요합니다.<br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. **AudioDeviceModulesManager** 를 통해 애플리케이션이 해당 시스템의 모든 오디오 효과에 액세스할 수 있기 때문입니다. 오디오 효과를 설정하면 디바이스의 오디오 성능에 부정적인 영향을 미칠 가능성이 있습니다. |
| **백그라운드 미디어 녹화** | **backgroundMediaRecording** 접근 권한 값은 [**MediaCapture**](/uwp/api/windows.media.capture.mediacapture) 및 [**AudioGraph**](/uwp/api/windows.media.audio.audiograph) 클래스 같은 미디어 관련 API의 동작을 변경하여 앱이 백그라운드에 있는 동안 미디어 녹화를 사용하도록 설정합니다. |
|**미리 보기 잉크 작업 영역**| **previewInkWorkspace** 접근 권한 값을 통해 앱은 잉크 작업 영역 내에 호스트된 미리 보기 잉크 네임스페이스에 액세스할 수 있습니다. 일반적으로 OEM이 디바이스의 화이트보드 애플리케이션을 교체할 때 사용됩니다.<br /> <br />이 접근 권한 값은 [**Windows.ApplicationModel.Preview.InkWorkspace**](/uwp/api/windows.applicationmodel.preview.inkworkspace) 네임스페이스의 API에 필요합니다. |
|**시작 화면 관리**| **startScreenManagement** 접근 권한 값을 통해 앱은 자동으로 타일을 시작 화면으로 고정합니다. 앱이 백그라운드에서도 고정할 수 있습니다. **startScreenManagement** 접근 권한 값이 없으면 어떤 API도 차단되지 않으며, **startScreenManagement** 를 사용하면 앱이 Pin API를 사용할 때 셸에 어떤 UI도 표시되지 않습니다. |
|**Cortana 사용 권한**| **cortanaPermissions** 접근 권한 값을 통해 앱은 사용자가 디바이스에서 Cortana에 부여한 사용 권한을 열거합니다. 이 접근 권한 값을 통해 앱은 디바이스에서 Cortana 사용 권한을 부여 및 호출할 수 있습니다. **cortanaPermissions** 를 사용하려면 사용 권한을 부여하기 전에 디바이스에서 법적 텍스트를 표시해야 합니다. 따라서 사용 권한을 수정하여 발생하는 모든 법적 결과를 사용자에게 알리는 것은 앱의 책임입니다.<br /> <br /><br />이 접근 권한 값은 **HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Search** 레지스트리 설정에 대한 읽기 권한을 얻기 위해 필요합니다.<br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
|**모든 앱 Mod**| **allAppMods** 접근 권한 값을 통해 앱은 모든 앱에서 AppMods 폴더에 액세스할 수 있습니다. Mod 관리 유틸리티는 **allAppMods** 를 사용하여 이를 사용하는 게임 또는 앱 외부의 Mod를 관리합니다. |
|**확장된 리소스**| **expandedResources** 접근 권한 값을 통해 앱은 게임 모드 리소스에 액세스할 수 있습니다. 조건을 충족하는 Xbox와 PC에서 게임 모드 리소스는 앱의 단독 사용을 위해 예약된 가용 CPU 코어의 일부를 나타냅니다. 또한 Xbox에서 앱은 최소 4GB의 메모리 파티션을 단독으로 사용합니다.<br /><br />이 접근 권한 값은 위에 정의된 대로 CPU 및 메모리 리소스의 단독 사용권을 얻는 데 필요합니다. |
|**보호되는 앱**| **protectedApp** 접근 권한 값을 통해 Microsoft Store에서 앱을 보호된 프로세스로 로드할 수 있습니다. 앱이 Microsoft Store에 수집되면 Microsoft Store가 실행 파일에 BLOB을 추가합니다. 또한 Microsoft Store는 Microsoft 키를 통해 실행 파일에 로그인합니다. BLOB에 Microsoft 서명이 필요하기 때문에 프로세스 로더는 보호된 프로세스를 적용하기 위해 접근 권한 값 대신 이 BLOB을 확인합니다.<br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
|**게임 모니터**| **gameMonitor** 접근 권한 값을 통해 시스템은 활성 모니터링을 통해 앱의 게임 부정 행위를 감지할 수 있습니다.<br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
|**앱 진단**| **appDiagnostics** 권한 제한 값을 통해 앱은 실행 중인 다른 UWP 앱에 대한 진단 정보(패키지 정보, 메모리 사용량, 계정 이름)를 얻을 수 있습니다. 반환되는 정보에는 앱이 실행 중인 도메인/머신 계정 이름이 포함되어 있습니다. 호출 앱이 관리자 권한으로 실행이 되는 경우에는 앱이 머신의 모든 계정에서 실행 중인 모든 앱의 목록을 검색할 수 있습니다. <br /><br />이 접근 권한 값은 [**Windows.System.AppDiagnosticInfo**](/uwp/api/windows.system.appdiagnosticinfo), **Windows.System.AppDiagnosticInfo.RequestAppDiagnosticInfoAsync** 및 [**Windows.ApplicationModel.AppInfo**](/uwp/api/windows.applicationmodel.appinfo) 클래스를 사용하는 데 필요합니다. |
| **장치 포털 공급자** | **devicePortalProvider** 접근 권한 값은 앱이 **Windows.System.Diagnostics.DevicePortal** API를 호출할 수 있도록 하고, 개발자 모드에 있는 동안 진단 도구에서 [웹 서버](../debug-test-perf/device-portal-plugin.md) 역할을 합니다.<br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
| **기업 클라우드 Single Sign-On** | **enterpriseCloudSSO** 접근 권한 값을 통해 앱이 호스팅된 웹 보기 컨트롤 내의 AAD(Azure Active Directory) 리소스를 사용하여 Single Sign-On할 수 있습니다. |
| **자동으로 VoIP 통화 수락** | **backgroundVoIP** 접근 권한 값을 사용하면 사용자에게 명시적으로 통화를 수락하도록 요구하지 않고도 들어오는 VoIP 통화를 자동으로 수신하고 수락할 수 있습니다. 이 접근 권한 값을 활용하는 앱은 카메라 및 마이크에 대한 모든 권한을 가지며 백그라운드에서 이 리소스를 사용할 수 있습니다.<br /><br />Microsoft Store에 제출되는 앱에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분 개발자는 이 접근 권한 값을 사용할 수 있도록 승인되지 않습니다. |
| **VoIP 통화 리소스 예약** | **oneProcessVoIP** 접근 권한 값을 사용하면 단일 프로세스 애플리케이션에서 VoIP 통화에 필요한 CPU 및 메모리 리소스를 예약할 수 있습니다.<br /><br />Microsoft Store에 제출되는 앱에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분 개발자는 이 접근 권한 값을 사용할 수 있도록 승인되지 않습니다. |
| **개발 모드 네트워크** | **developmentModeNetwork** 접근 권한 값을 사용하면 C++/CX UWP 앱 또는 C++ Windows 런타임 구성 요소의 OpenFile Win32 API를 호출할 때 앱이 로그인한 사용자의 자격 증명을 사용하여 네트워크 경로에 액세스할 수 있습니다. <br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
| **다양한 파일 시스템 액세스** | **broadFileSystemAccess** 접근 권한 값을 사용하면 앱이 런타임 동안 어떠한 추가 파일 선택기 스타일 프롬프트 없이 현재 앱을 실행하는 사용자와 같은 파일 시스템에 액세스할 수 있습니다. 사용자가 이미 FilePicker 또는 FolderPicker를 사용하여 선택한 파일에 액세스할 때는 이 접근 권한 값이 필요 없다는 점을 기억해야 합니다.<br/><br/>이 접근 권한 값은 [Windows.Storage](/uwp/api/windows.storage) API에 대해 작동합니다. 사용자가 언제든지 [설정]에서 권한을 부여 또는 거부할 수 있으므로 앱은 이러한 변경에 탄력적으로 대응할 수 있어야 합니다. 2018년 4월 업데이트에서 사용 권한의 기본값은 켜짐입니다. 2018년 10월 업데이트에서 기본값은 꺼짐입니다. 또한 이 접근 권한 값을 사용하여 **문서**, **사진**, **동영상** 등의 특수 폴더 접근 권한 값을 선언하면 안 됩니다. 매니페스트에 **broadFileSystemAccess** 를 추가하여 앱에서 이 접근 권한 값을 사용하도록 설정할 수 있습니다. 예제는 [파일 액세스 권한](../files/file-access-permissions.md) 문서를 참조하세요.<br/><br/>**참고:** 이 기능은 Xbox에서 지원되지 않습니다. |
| **시스템 펌웨어 및 BIOS** | **smbios** 접근 권한 값을 통해 앱은 bios 데이터 및 시스템 펌웨어 데이터에 액세스할 수 있습니다. |
| **완전 신뢰 권한 수준** | **runFullTrust** 접근 권한 값을 통해 앱을 사용자 머신의 완전 신뢰 권한 수준에서 실행할 수 있습니다. 이 접근 권한 값은 [FullTrustProcessLauncher](/uwp/api/windows.applicationmodel.fulltrustprocesslauncher) API를 사용하는 데 필요합니다.<br /><br />이 접근 권한 값은 appx 또는 msix 패키지로 제공되는 데스크톱 애플리케이션에도 필요하며([데스크톱 브리지](https://developer.microsoft.com/windows/bridges/desktop)와 마찬가지), DAC(데스크톱 앱 변환기) 또는 Visual Studio를 사용하여 이러한 앱을 패키징할 때 자동으로 매니페스트에 표시됩니다. |
| **권한 상승** | **allowElevation** 제한된 접근 권한 값을 사용하면 Microsoft 파트너와 기업에서 만든 앱이 시작 시 또는 앱의 수명 주기 동안 자동 권한 상승이 필요한 기존 데스크톱 기능을 유지할 수 있습니다.<br/><br/>Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. 기업에서 비즈니스용 Microsoft Store를 통해 개인 저장소에 배포하는 기간 업무 앱에만 승인됩니다.  |
| **Windows 팀 디바이스 자격 증명** | **teamEditionDeviceCredential** 제한된 접근 권한 값을 통해 앱은 Windows 10 버전 1703 이상을 실행하는 Surface Hub 디바이스에서 디바이스 계정 자격 증명을 요청하는 API에 액세스할 수 있습니다.<br/><br/>Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
| **Windows 팀 애플리케이션 보기** | **teamEditionView** 제한된 접근 권한 값을 통해 앱은 Windows 10 버전 1703 이상을 실행하는 Surface Hub 디바이스에서 애플리케이션을 호스팅하는 API에 액세스할 수 있습니다.<br/><br/>Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
| **카메라 처리 확장 프로그램** | **cameraProcessingExtension** 제한된 접근 권한 값을 통해 앱은 직접 카메라 컨트롤 없이 카메라에서 캡처된 이미지를 처리할 수 있습니다.<br /><br />이 접근 권한 값은 [Windows.Devices.PointOfService.Provider](/uwp/api/windows.devices.pointofservice.provider) 네임스페이스의 API를 호출하는 데 필요합니다.<br /><br />스토어 제출에 대해 모든 사용자가 이 접근 권한 값에 대한 액세스 권한을 요청할 수 있습니다. |
| **데이터 사용 관리** | **networkDataUsageManagement** 제한된 접근 권한 값을 통해 앱은 네트워크 데이터 사용 정보를 수집할 수 있습니다.<br /><br />이 접근 권한 값은 [GetAttributedNetworkUsageAsync](/uwp/api/windows.networking.connectivity.connectionprofile.getattributednetworkusageasync)를 호출하는 데 필요합니다.<br /><br />스토어 제출에 대해 모든 사용자가 이 접근 권한 값에 대한 액세스 권한을 요청할 수 있습니다. |
| **전화 회선 연결 관리** | **phoneLineTransportManagement** 접근 권한 값을 통해 앱은 전화 회선 연결을 담당하는 시스템 디바이스를 관리할 수 있습니다.<br /><br />이 접근 권한 값은 [Windows.ApplicationModel.Calls](/uwp/api/windows.applicationmodel.calls) 네임스페이스의 PhoneLineTransportDevice API를 사용하는 데 필요합니다. |
| **가상화되지 않은 리소스** | **unvirtualizedResources** 제한된 접근 권한 값을 통해 애플리케이션은 패키지 매니페스트에서 [RegistryWriteVirtualization](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop6-registrywritevirtualization) 및 [FileSystemWriteVirtualization](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop6-filesystemwritevirtualization) 요소를 선언하여 레지스트리 및 파일 시스템에 가상화를 사용하지 않도록 설정할 수 있습니다. 이렇게 선언하면 시스템이 HKEY_CURRENT_USER 또는 사용자의 AppData 폴더에 대한 쓰기를 가상화하지 못합니다. 다른 애플리케이션이 내 애플리케이션과 동일한 레지스트리 또는 파일 시스템 항목을 읽거나 써야 하는 시나리오에 유용합니다.<br /><br />이 접근 권한 값은 Microsoft와 파트너가 게시하는 특정 유형의 데스크톱 PC 게임을 위해 설계되었습니다. 시스템의 완전 제거 기능을 손상시킬 수 있기 때문에 다른 시나리오에는 사용할 수 없습니다. |
| **수정 가능한 앱** | **modifiableApp** 제한된 접근 권한 값을 통해 애플리케이션은 패키지 매니페스트에서 [windows.mutablePackageDirectories](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop6-package-extension) 확장을 선언할 수 있습니다. 이렇게 하면 애플리케이션에서 수정된 또는 추가된 파일을 배치할 폴더 이름을 제공할 수 있습니다. OS는 이 폴더를 만들고 애플리케이션에서 원래 설치된 파일 대신(또는 해당 파일과 함께) 이 폴더의 파일을 사용하게 합니다.<br /><br />이 접근 권한 값은 Microsoft와 파트너가 게시하는 특정 유형의 데스크톱 PC 게임을 위해 설계되었습니다. 서명되지 않은 코드가 실행될 수 있으므로 다른 시나리오에는 승인되지 않습니다. |
| **패키지 쓰기 리디렉션 호환성 Shim** | **packageWriteRedirectionCompatibilityShim** 제한된 접근 권한 값은 모든 새 파일을 사용자별 위치에 만들도록 애플리케이션을 구성합니다. 쓰기를 위해 열려 있는 모든 기존 파일이 사용자별 위치에 복사된 후 해당 위치의 파일이 수정됩니다. 이 접근 권한 값은 설치 폴더에 파일을 만들거나 설치 폴더의 파일을 수정하는 애플리케이션에 유용합니다.<br /><br />이 접근 권한 값은 Microsoft와 파트너가 게시하는 특정 유형의 데스크톱 PC 게임을 위해 설계되었습니다. 그러나 경우에 따라 다른 앱에도 적용될 수 있습니다. |
| **사용자 지정 설치 작업** | **customInstallActions** 제한된 접근 권한 값을 사용하면 애플리케이션이 패키지 매니페스트에서 [windows.customInstall](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop6-package-extension) 확장을 선언하여 애플리케이션과 함께 실행되는 하나 이상의 추가 설치 관리자 파일(.exe 또는 .msi)을 지정할 수 있습니다. 이렇게 하면 표준 배포 시나리오(설치, 업데이트, 복구 또는 제거)에 대한 사용자 지정 작업을 지정할 수 있습니다. 예를 들어 타사 재배포 가능 구성 요소를 번들로 묶는 애플리케이션에 유용합니다.<br /><br />이 접근 권한 값은 Microsoft와 파트너가 게시하는 특정 유형의 데스크톱 PC 게임을 위해 설계되었습니다. 다른 시나리오에는 승인되지 않습니다. |
| **패키지 서비스** | **packagedServices** 제한된 접근 권한 값을 사용하면 Microsoft 파트너와 기업에서 만든 애플리케이션은 패키지 매니페스트에서 [windows.service](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop6-extension) 확장을 선언하여 앱과 함께 하나 이상의 서비스를 설치할 수 있습니다. 이러한 서비스는 로컬 서비스, 네트워크 서비스 또는 로컬 시스템 계정에서 실행되도록 구성할 수 있습니다. 로컬 서비스 및 네트워크 서비스에는 **packagedServices** 접근 권한 값만 필요합니다. 로컬 시스템 서비스에는 **packagedServices** 및 **localSystemServices** 접근 권한 값이 모두 필요합니다.<br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다.  |
| **로컬 시스템 서비스** | **localSystemServices** 제한된 접근 권한 값을 사용하면 Microsoft 파트너와 기업에서 만든 애플리케이션은 앱과 함께 하나 이상의 로컬 시스템 서비스를 설치할 수 있습니다. 즉, 애플리케이션에서 LocalSystem이 될 서비스의 StartAccount를 선언할 수 있습니다. 이 시나리오에는 **packagesServices** 접근 권한 값도 필요합니다. <br /><br />Microsoft Store에 제출하는 애플리케이션에서는 이 접근 권한 값을 선언하지 않는 것이 좋습니다. 대부분의 경우 이 접근 권한 값을 사용하면 앱이 승인되지 않습니다. |
| **백그라운드 공간 인식** | **backgroundSpatialPerception** 제한된 접근 권한 값을 통해 애플리케이션은 사용자의 머리, 손, 동작 컨트롤러, 앱이 백그라운드에서 실행되는 동안 추적되는 기타 개체의 움직임에 액세스할 수 있습니다. |

## <a name="custom-capabilities"></a>사용자 지정 접근 권한 값

위의 [제한된 접근 권한 값](#restricted-capabilities) 섹션에서는 사용자 지정 접근 권한 값을 사용하기 위한 승인을 요청하는 데 사용할 수 있는 동일한 접근 권한 값 승인 프로세스에 대해 설명합니다. [포함된 SIM](/uwp/api/windows.networking.networkoperators.esim) API는 사용자 지정 접근 권한 값이 필요한 API의 예입니다. 개발자 모드에서 로컬로만 애플리케이션을 실행하려는 경우에는 사용자 지정 접근 권한 값이 필요 없습니다. 그러나 앱을 Microsoft Store에 게시하거나 개발자 모드 외부에서 실행하려면 필요합니다.

Windows TAM(기술 담당 관리자)이 있는 경우 TAM과 협력하여 액세스를 요청할 수 있습니다. 자세한 내용은 [Microsoft TAM에게 문의](/windows-hardware/drivers/mobilebroadband/testing-your-desktop-cosa-apn-database-submission#contact-your-microsoft-tam)에서 찾을 수 있습니다.

사용자 지정 접근 권한 값을 선언하려면 [앱 패키지 매니페스트](/uwp/schemas/appxpackage/appx-package-manifest) 원본 파일(`Package.appxmanifest`)을 수정합니다. **xmlns:uap4** XML 네임스페이스 선언을 추가하고, 사용자 지정 접근 권한 값을 선언할 때 **uap4** 접두사를 사용합니다. 예를 들면 다음과 같습니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package
    ...
    xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4">
...
<Capabilities>
    <uap4:CustomCapability Name="CompanyName.customCapabilityName_PublisherID"/>
</Capabilities>
</Package>
```

> [!NOTE]
> 모든 **CustomCapability** 요소는 패키지 매니페스트의 **Capabilities** 노드 아래에 있는 **Capability** 요소의 뒤, 그리고 [DeviceCapability](#device-capabilities) 요소의 앞에 있어야 합니다.

## <a name="related-topics"></a>관련 항목

* [제출 옵션](../publish/manage-submission-options.md)
* [패키지 매니페스트에서 접근 권한 값을 지정하는 방법](/uwp/schemas/appxpackage/how-to-specify-capabilities-in-a-package-manifest)
* [패키지 매니페스트에서 디바이스 접근 권한 값을 지정하는 방법](/uwp/schemas/appxpackage/how-to-specify-device-capabilities-in-a-package-manifest)
