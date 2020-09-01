---
description: 장치 자체 및 센서와 통합 되는 코드는 사용자의 입력 및 출력을 포함 합니다.
title: I/o, 장치 및 앱 모델을 위해 UWP에 Windows Phone Silverlight 포팅
ms.assetid: bf9f2c03-12c1-49e4-934b-e3fa98919c53
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 09b192f38a5bedaad491ade322df252d4b9e5971
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162187"
---
#  <a name="porting-windowsphone-silverlight-to-uwp-for-io-device-and-app-model"></a>I/o, 장치 및 앱 모델을 위해 UWP에 Windows Phone Silverlight 포팅


이전 항목에서는 [XAML 및 UI를 이식](wpsl-to-uwp-porting-xaml-and-ui.md)했습니다.

장치 자체 및 센서와 통합 되는 코드는 사용자의 입력 및 출력을 포함 합니다. 데이터 처리도 포함 될 수 있습니다. 그러나이 코드는 일반적으로 UI 계층 또는 데이터 계층으로 간주 되지 않습니다. 이 코드에는 진동 컨트롤러,가 속도계, 자이로스코프가, 마이크 및 스피커 (음성 인식 및 합성과 교차), (지역) 위치, 터치, 마우스, 키보드, 펜 등의 입력 소프트웨어나와의 통합이 포함 됩니다.

## <a name="application-lifecycle-process-lifetime-management"></a>응용 프로그램 수명 주기 (프로세스 수명 관리)

Windows Phone Silverlight 앱에는 삭제 표시를 지원 하 고 이후에 다시 활성화할 수 있도록 응용 프로그램 상태 및 해당 뷰 상태를 저장 하 고 복원 하는 코드가 포함 되어 있습니다. UWP (유니버설 Windows 플랫폼) 앱의 앱 수명 주기는 사용자가 언제 든 지 포그라운드에서 사용 하도록 선택한 앱에 사용할 수 있는 리소스를 최대화 하는 것과 동일한 목표로 사용 하도록 설계 되었으므로 Windows Phone Silverlight 앱과 매우 유사 합니다. 코드가 새 시스템에 쉽게 적용 되는 것을 알 수 있습니다.

**참고**    하드웨어 **뒤로** 단추를 누르면 Windows Phone Silverlight 앱이 자동으로 종료 됩니다. 모바일 장치에서 하드웨어 **뒤로** 단추를 누르면 UWP 앱이 자동으로 종료 *되지* 않습니다. 대신 일시 중단 되 고 종료 될 수 있습니다. 그러나 이러한 세부 정보는 응용 프로그램 수명 주기 이벤트에 적절 하 게 응답 하는 앱에는 투명 합니다.

"Debounce window"는 앱이 비활성 상태가 되는 기간과 일시 중단 이벤트를 발생 시키는 시스템 간의 시간입니다. UWP 앱의 경우에는 debounce 창이 없습니다. 일시 중단 이벤트는 앱이 비활성화 되는 즉시 발생 합니다.

자세한 내용은 [앱 수명 주기](../launch-resume/app-lifecycle.md)를 참조 하세요.

## <a name="camera"></a>카메라

Windows Phone Silverlight 카메라 캡처 코드는 **microsoft. devices**, **여 cameracapturetask** 또는 **클래스를 사용**합니다. 유니버설 Windows 플랫폼 (UWP)로 코드를 이식 하려면 [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) 클래스를 사용할 수 있습니다. [**CapturePhotoToStorageFileAsync**](/uwp/api/windows.media.capture.mediacapture.capturephototostoragefileasync) 항목에는 코드 예제가 있습니다. 이 방법을 사용 하면 저장소 파일에 사진을 캡처할 수 있으며, 앱 패키지 매니페스트에서 **마이크** 및 **웹캠** [**장치 기능**](/uwp/schemas/appxpackage/uapmanifestschema/element-devicecapability) 을 설정 해야 합니다.

또 다른 옵션은 **마이크** 및 **웹캠** [**장치 기능**](/uwp/schemas/appxpackage/uapmanifestschema/element-devicecapability)을 필요로 하는 [**CameraCaptureUI**](/uwp/api/Windows.Media.Capture.CameraCaptureUI) 클래스입니다.

UWP 앱에 대 한 렌즈 앱은 지원 되지 않습니다.

## <a name="detecting-the-platform-your-app-is-running-on"></a>앱이 실행 되 고 있는 플랫폼 검색

Windows 10의 앱 대상 변경에 대해 생각 하는 방법입니다. 새 개념적 모델은 앱이 UWP (유니버설 Windows 플랫폼)를 대상으로 하 고 모든 Windows 장치에서 실행 되는 것입니다. 그런 다음 특정 장치 제품군에만 적용 되는 기능을 선택할 수 있습니다. 필요한 경우 앱은 하나 이상의 장치 패밀리를 대상으로 하는 것을 제한 하는 옵션도 있습니다. 장치 제품군에 대 한 자세한 내용 및 대상으로 할 장치 패밀리를 결정 하는 방법에 대 한 자세한 내용은 [UWP 앱에 대 한 가이드를](../get-started/universal-application-platform-guide.md)참조 하세요.

**참고**    운영 체제 또는 장치 패밀리를 사용 하 여 기능이 있는지 검색 하지 않는 것이 좋습니다. 현재 운영 체제 또는 장치 제품군을 식별 하는 것은 일반적으로 특정 운영 체제 또는 장치 패밀리 기능이 있는지 여부를 확인 하는 가장 좋은 방법이 아닙니다. 운영 체제 또는 장치 패밀리 (및 버전 번호)를 검색 하는 대신 기능 자체의 존재 여부를 테스트 합니다 ( [조건부 컴파일 및 적응 코드](wpsl-to-uwp-porting-to-a-uwp-project.md)참조). 특정 운영 체제 또는 장치 패밀리가 필요한 경우 해당 버전에 대 한 테스트를 설계 하는 대신 지원 되는 최소 버전으로 사용 해야 합니다.

앱의 UI를 다른 장치에 맞게 조정 하기 위해 권장 되는 몇 가지 기술이 있습니다. 자동 크기 조정 요소와 동적 레이아웃 패널은 항상 그대로 사용 합니다. XAML 태그에서 유효 픽셀(이전의 보기 픽셀)로 크기를 사용하므로 다른 해상도 및 배율에 맞게 UI를 조정할 수 있습니다([보기/유효 픽셀, 가시거리 및 배율 인수](wpsl-to-uwp-porting-xaml-and-ui.md) 참조). 그리고 시각적 상태 관리자의 적응 트리거와 setter를 사용 하 여 UI를 창 크기로 조정 합니다 ( [UWP 앱에 대 한 가이드](../get-started/universal-application-platform-guide.md)참조).

그러나 장치 패밀리를 피할 수 없는 시나리오가 있는 경우이 작업을 수행할 수 있습니다. 이 예제에서는 [**AnalyticsVersionInfo**](/uwp/api/Windows.System.Profile.AnalyticsVersionInfo) 클래스를 사용 하 여 해당 하는 경우 모바일 장치 제품군에 맞게 조정 된 페이지로 이동 하 고, 그렇지 않은 경우 기본 페이지로 대체 해야 합니다.

```csharp
   if (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Mobile")
        rootFrame.Navigate(typeof(MainPageMobile), e.Arguments);
    else
        rootFrame.Navigate(typeof(MainPage), e.Arguments);
```

앱은 적용 되는 리소스 선택 요소에서 실행 중인 장치 제품군을 확인할 수도 있습니다. 아래 예제에서는이 작업을 수행 하는 방법을 보여 줍니다. [**QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.qualifiervalues) 항목에서는 장치 패밀리 요소를 기준으로 장치 패밀리 관련 리소스를 로드 하는 클래스에 대 한 일반적인 사용 사례에 대해 설명 합니다.

```csharp
var qualifiers = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues;
string deviceFamilyName;
bool isDeviceFamilyNameKnown = qualifiers.TryGetValue("DeviceFamily", out deviceFamilyName);
```

또한 [조건부 컴파일 및 적응 코드](wpsl-to-uwp-porting-to-a-uwp-project.md)를 참조 하세요.

## <a name="device-status"></a>디바이스 상태

Windows Phone Silverlight 앱은 **DeviceStatus** 클래스를 사용 하 여 앱이 실행 되는 장치에 대 한 정보를 가져올 수 있습니다. **Microsoft.Phone.Info** 네임 스페이스에 대 한 직접 uwp와는 관계가 없지만, **DeviceStatus** 클래스의 멤버에 대 한 호출 대신 UWP 앱에서 사용할 수 있는 몇 가지 속성 및 이벤트는 다음과 같습니다.

| Windows Phone Silverlight                                                               | UWP                                                                                                                                                                                                                                                                                                                                |
|-----------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ApplicationCurrentMemoryUsage** 및 **ApplicationCurrentMemoryUsageLimit** 속성 | [**Memorymanager. AppMemoryUsage**](/uwp/api/windows.system.memorymanager.appmemoryusage) 및 [**AppMemoryUsageLimit**](/uwp/api/windows.system.memorymanager.appmemoryusagelimit) 속성                                                                                                                                    |
| **ApplicationPeakMemoryUsage** 속성                                                 | Visual Studio의 메모리 프로 파일링 도구를 사용 합니다. 자세한 내용은 [메모리 사용 분석](/visualstudio/welcome-to-visual-studio-2015?view=vs-2015)을 참조 하세요.                                                                                                                                                                          |
| **DeviceFirmwareVersion** 속성                                                      | [**EasClientDeviceInformation.SystemFirmwareVersion**](/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation.systemfirmwareversion) 속성 (데스크톱 장치 제품군에만 해당)                                                                                                                                                                             |
| **DeviceHardwareVersion** 속성                                                      | [**EasClientDeviceInformation.SystemHardwareVersion**](/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation.systemhardwareversion) 속성 (데스크톱 장치 제품군에만 해당)                                                                                                                                                                             |
| **Device.devicemanufacturer** 속성                                                         | [**EasClientDeviceInformation.SystemManufacturer**](/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation.systemmanufacturer) 속성 (데스크톱 장치 제품군에만 해당)                                                                                                                                                                                |
| **장치** 이름 속성                                                                 | [**EasClientDeviceInformation.SystemProductName**](/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation.systemproductname) 속성 (데스크톱 장치 제품군에만 해당)                                                                                                                                                                                 |
| **Devicet이상 Almemory** 속성                                                          | 동일한 요소 없음                                                                                                                                                                                                                                                                                                                      |
| **IsKeyboardDeployed** 속성                                                         | 해당하는 값이 없습니다. 이 속성은 일반적으로 사용 되지 않는 모바일 장치에 대 한 하드웨어 키보드에 대 한 정보를 제공 합니다.                                                                                                                                                                                                        |
| **IsKeyboardPresent** 속성                                                          | 해당하는 값이 없습니다. 이 속성은 일반적으로 사용 되지 않는 모바일 장치에 대 한 하드웨어 키보드에 대 한 정보를 제공 합니다.                                                                                                                                                                                                        |
| **KeyboardDeployedChanged** 이벤트                                                       | 해당하는 값이 없습니다. 이 속성은 일반적으로 사용 되지 않는 모바일 장치에 대 한 하드웨어 키보드에 대 한 정보를 제공 합니다.                                                                                                                                                                                                        |
| **Powersource** 속성                                                                | 동일한 요소 없음                                                                                                                                                                                                                                                                                                                      |
| **PowerSourceChanged** 이벤트                                                            | [**RemainingChargePercentChanged**](/uwp/api/windows.phone.devices.power.battery.remainingchargepercentchanged) 이벤트를 처리 합니다 (모바일 장치 제품군에만 해당). [**RemainingChargePercent**](/uwp/api/windows.phone.devices.power.battery.remainingchargepercent) 속성 값 (모바일 장치 제품군에만 해당)이 1% 감소 하면 이벤트가 발생 합니다. |

## <a name="location"></a>위치

앱 패키지 매니페스트의 위치 기능을 선언 하는 앱이 Windows 10에서 실행 되는 경우 시스템은 최종 사용자에 게 동의 여부를 묻는 메시지를 표시 합니다. 따라서 앱에서 고유한 사용자 지정 동의 확인 프롬프트를 표시 하거나, 설정/해제 기능을 제공 하는 경우 최종 사용자에 게 한 번만 표시 되도록 해당 정보를 제거 하는 것이 좋습니다.

## <a name="orientation"></a>방향

**PhoneApplicationPage** 및 **Orientation** 속성에 해당 하는 UWP 앱은 앱 패키지 매니페스트의 [**uap: InitialRotationPreference**](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-splashscreen) 요소입니다. **응용 프로그램** 탭이 선택 되어 있지 않은 경우 선택 하 고 **지원 되는 회전** 아래에서 하나 이상의 확인란을 선택 하 여 기본 설정을 기록 합니다.

그러나 장치 방향과 화면 크기에 관계 없이 UWP 앱의 UI를 디자인 하는 것이 좋습니다. [폼 팩터 및 사용자 경험을 위해 포팅](wpsl-to-uwp-form-factors-and-ux.md)하는 방법에 대 한 자세한 내용은 다음에 나오는 항목입니다.

다음 항목은 [비즈니스 및 데이터 계층을 이식](wpsl-to-uwp-business-and-data.md)하는 것입니다.