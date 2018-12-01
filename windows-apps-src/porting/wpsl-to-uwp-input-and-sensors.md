---
description: 디바이스 및 센서와 통합되는 코드는 사용자의 입력과 사용자에 대한 출력을 포함합니다.
title: I/O, 디바이스 및 앱 모델에 대 한 WindowsPhone Silverlight를 UWP로 포팅 '
ms.assetid: bf9f2c03-12c1-49e4-934b-e3fa98919c53
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6ef1814443b3831e514eafb3f5a0c58b7703126b
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8346609"
---
#  <a name="porting-windowsphone-silverlight-to-uwp-for-io-device-and-app-model"></a>I/O, 디바이스 및 앱 모델에 대 한 WindowsPhone Silverlight를 UWP로 포팅


이전 항목에서는 [XAML 및 UI 포팅](wpsl-to-uwp-porting-xaml-and-ui.md)에 대해 살펴봤습니다.

디바이스 및 센서와 통합되는 코드는 사용자의 입력과 사용자에 대한 출력을 포함합니다. 데이터 처리를 포함할 수도 있습니다. 하지만 이 코드는 UI 계층 또는 데이터 계층으로 간주되지 않습니다. 이 코드는 진동 컨트롤러, 가속도계, 자이로스코프, 마이크와 스피커(음성 인식 및 음성 합성과 교차), 지리적 위치, 입력 형식(예제: 터치, 마우스, 키보드, 펜) 등과의 통합을 포함합니다.

## <a name="application-lifecycle-process-lifetime-management"></a>응용 프로그램 수명 주기(프로세스 수명 관리)

WindowsPhone Silverlight 앱에 저장 하 고 삭제 하 고 이후에 다시 활성화를 지원 하기 위해 응용 프로그램의 상태와 보기 상태를 복원 하는 코드가 들어 있습니다. 유니버설 Windows 플랫폼 (UWP) 앱의 응용 프로그램 수명 주기는 주기가 WindowsPhone Silverlight 앱과는 둘 다 사용할 수 있는 리소스를 최대화 한다는 동일한 목적으로 설계 되었기에 사용자가 선택한 모든 앱에는 언제 든 지 포그라운드 합니다. 코드는 새 시스템에 맞게 무리 없이 쉽게 조정됩니다.

**참고**  자동으로 **다시** 하드웨어 단추를 누르면 WindowsPhone Silverlight 앱을 종료 합니다. 모바일 디바이스에서 하드웨어 **뒤로** 단추를 눌러도 UWP 앱이 자동으로 종료되지는 *않습니다*. 대신 앱이 일시 중단된 후 종료될 수 있습니다. 하지만 이러한 세부 정보는 응용 프로그램 수명 주기 이벤트에 적절히 응답하도록 앱에 투명하게 적용됩니다.

"디바운스 기간"이란 비활성화되는 앱과 일시 중단 이벤트를 발생시키는 시스템 사이의 기간입니다. UWP 앱의 경우 디바운스 기간이 없습니다. 즉, 앱이 비활성화되는 즉시 일시 중단 이벤트가 발생합니다.

자세한 내용은 [앱 수명 주기](https://msdn.microsoft.com/library/windows/apps/mt243287)를 참조하세요.

## <a name="camera"></a>Camera

WindowsPhone Silverlight 카메라 캡처 코드 **Microsoft.Devices.Camera**, **Microsoft.Devices.PhotoCamera**또는 **Microsoft.Phone.Tasks.CameraCaptureTask** 클래스를 사용 합니다. 해당 코드를 UWP(유니버설 Windows 플랫폼)로 포팅하기 위해 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) 클래스를 사용할 수 있습니다. 코드 예제는 [**CapturePhotoToStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh700836) 항목을 참조하세요. 해당 메서드를 사용 하는 사진을 저장소 파일에 캡처할 수 있습니다 하며[**장치 기능**](https://msdn.microsoft.com/library/windows/apps/dn934747) **마이크** 및 **웹캠**을 앱 패키지 매니페스트에서 설정할 수 있습니다.

다른 옵션은 [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) 클래스, **마이크** 및 **웹캠**을[**장치 접근 권한 값**](https://msdn.microsoft.com/library/windows/apps/dn934747)도 필요 합니다.

필터 앱은 UWP 앱에 대해 지원되지 않습니다.

## <a name="detecting-the-platform-your-app-is-running-on"></a>앱이 실행되고 있는 플랫폼 검색

Windows10 사용 하 여 앱을 대상으로 변경에 대 한 생각 하는 방법은 합니다. 새로운 개념적 모델에서는 앱이 UWP(유니버설 Windows 플랫폼)를 대상으로 하고 모든 Windows 디바이스에서 실행됩니다. 그런 다음 특정 디바이스 패밀리에 독점적으로 사용되는 기능을 돋보이도록 선택할 수 있습니다. 또한 필요한 경우 앱에는 특별히 하나 이상의 디바이스 패밀리를 대상으로 하도록 자체적으로 제한하는 옵션도 있습니다. 디바이스 패밀리와 대상으로 할 디바이스 패밀리를 결정하는 방법에 대한 자세한 내용은 [UWP 앱 지침](https://msdn.microsoft.com/library/windows/apps/dn894631)을 참조하세요.

**참고**  를 사용 하면 운영 체제 또는 디바이스 패밀리 기능이 있는지 검색 하는 것이 좋습니다. 일반적으로 현재 운영 체제 또는 디바이스 패밀리를 식별하는 방법이 특정 운영 체제 또는 디바이스 패밀리 기능이 있는지 확인하는 가장 좋은 방법은 아닙니다. 운영 체제 또는 디바이스 패밀리(및 버전 번호)를 검색하는 대신 기능 자체의 존재에 대해 테스트합니다([조건부 컴파일 및 적응 코드](wpsl-to-uwp-porting-to-a-uwp-project.md) 참조). 특정 운영 체제 또는 디바이스 패밀리가 필요한 경우 해당 버전에 대한 테스트를 디자인하지 않고 해당 버전을 지원되는 최소 버전으로 사용해야 합니다.

앱의 UI를 서로 다른 디바이스에 맞게 구성하는 데 권장되는 여러 기술이 있습니다. 기존과 마찬가지로 자동 크기 조정 요소 및 동적 레이아웃 패널을 계속 사용할 수 있습니다. XAML 태그에서 유효 픽셀(이전의 보기 픽셀)로 크기를 사용하므로 다른 해상도 및 배율에 맞게 UI를 조정할 수 있습니다([보기/유효 픽셀, 가시거리 및 배율 인수](wpsl-to-uwp-porting-xaml-and-ui.md) 참조). Visual State Manager의 적응 트리거 및 setter를 사용하여 창 크기에 맞게 UI를 조정할 수 있습니다([UWP 앱 지침](https://msdn.microsoft.com/library/windows/apps/dn894631) 참조).

그러나 디바이스 패밀리 검색이 반드시 필요한 경우에는 검색을 수행할 수 있습니다. 이 예제에서는 [**AnalyticsVersionInfo**](https://msdn.microsoft.com/library/windows/apps/dn960165) 클래스를 사용하여 적절한 경우 디바이스 패밀리에 맞게 조정된 페이지를 탐색하고 그렇지 않은 경우 다시 기본 페이지를 사용할 수 있는지 확인합니다.

```csharp
   if (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Mobile")
        rootFrame.Navigate(typeof(MainPageMobile), e.Arguments);
    else
        rootFrame.Navigate(typeof(MainPage), e.Arguments);
```

또한 앱이 적용되는 리소스 선택 요소에서 실행되고 있는 디바이스 패밀리를 결정할 수도 있습니다. 아래 예제는 명령을 통해 이를 수행하는 방법을 보여 주며, [**ResourceContext.QualifierValues**](https://msdn.microsoft.com/library/windows/apps/br206071) 항목에서는 디바이스 패밀리 요소에 따라 디바이스 패밀리별 리소스를 로드하는 데 있어 클래스의 일반적인 사용 사례에 관해 설명합니다.

```csharp
var qualifiers = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues;
string deviceFamilyName;
bool isDeviceFamilyNameKnown = qualifiers.TryGetValue("DeviceFamily", out deviceFamilyName);
```

또한 [조건부 컴파일 및 적응 코드](wpsl-to-uwp-porting-to-a-uwp-project.md)를 참조하세요.

## <a name="device-status"></a>디바이스 상태

WindowsPhone Silverlight 앱은 앱이 실행 중인 장치에 대 한 정보를 가져오는 **Microsoft.Phone.Info.DeviceStatus** 클래스를 사용할 수 있습니다. UWP에는 **Microsoft.Phone.Info** 네임스페이스에 해당하는 직접적인 항목이 없지만 UWP 앱에서 **DeviceStatus** 클래스의 구성원을 호출하는 대신 사용할 수 있는 몇 가지 속성과 이벤트가 있습니다.

| Windows Phone Silverlight                                                               | UWP                                                                                                                                                                                                                                                                                                                                |
|-----------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ApplicationCurrentMemoryUsage** 및 **ApplicationCurrentMemoryUsageLimit** 속성 | [**MemoryManager.AppMemoryUsage**](https://msdn.microsoft.com/library/windows/apps/dn633832) 및 [**AppMemoryUsageLimit**](https://msdn.microsoft.com/library/windows/apps/dn633836) 속성                                                                                                                                    |
| **ApplicationPeakMemoryUsage** 속성                                                 | Visual Studio에서 메모리 프로파일링 도구를 사용합니다. 자세한 내용은 [메모리 사용 분석](http://msdn.microsoft.com/library/windows/apps/dn645469.aspx)을 참조하세요.                                                                                                                                                                          |
| **DeviceFirmwareVersion** 속성                                                      | [**EasClientDeviceInformation.SystemFirmwareVersion**](https://msdn.microsoft.com/library/windows/apps/dn608144) 속성 (데스크톱 디바이스 패밀리에만 해당)                                                                                                                                                                             |
| **DeviceHardwareVersion** 속성                                                      | [**EasClientDeviceInformation.SystemHardwareVersion**](https://msdn.microsoft.com/library/windows/apps/dn608145) 속성 (데스크톱 디바이스 패밀리에만 해당)                                                                                                                                                                             |
| **DeviceManufacturer** 속성                                                         | [**EasClientDeviceInformation.SystemManufacturer**](https://msdn.microsoft.com/library/windows/apps/hh701398) 속성 (데스크톱 디바이스 패밀리에만 해당)                                                                                                                                                                                |
| **DeviceName** 속성                                                                 | [**EasClientDeviceInformation.SystemProductName**](https://msdn.microsoft.com/library/windows/apps/hh701401) 속성 (데스크톱 디바이스 패밀리에만 해당)                                                                                                                                                                                 |
| **DeviceTotalMemory** 속성                                                          | 해당 속성 없음                                                                                                                                                                                                                                                                                                                      |
| **IsKeyboardDeployed** 속성                                                         | 해당하는 속성이 없습니다. 이 속성은 일반적으로 사용되지 않는 모바일 디바이스의 하드웨어 키보드에 대한 정보를 제공합니다.                                                                                                                                                                                                        |
| **IsKeyboardPresent** 속성                                                          | 해당하는 속성이 없습니다. 이 속성은 일반적으로 사용되지 않는 모바일 디바이스의 하드웨어 키보드에 대한 정보를 제공합니다.                                                                                                                                                                                                        |
| **KeyboardDeployedChanged** 이벤트                                                       | 해당하는 속성이 없습니다. 이 속성은 일반적으로 사용되지 않는 모바일 디바이스의 하드웨어 키보드에 대한 정보를 제공합니다.                                                                                                                                                                                                        |
| **PowerSource** 속성                                                                | 해당 속성 없음                                                                                                                                                                                                                                                                                                                      |
| **PowerSourceChanged** 이벤트                                                            | [**RemainingChargePercentChanged**](https://msdn.microsoft.com/library/windows/apps/jj207240) 이벤트를 처리합니다(모바일 디바이스 패밀리에만 해당). 이 이벤트는 [**RemainingChargePercent**](https://msdn.microsoft.com/library/windows/apps/jj207239) 속성(모바일 디바이스 패밀리에만 해당) 값이 1%씩 감소할 때 발생합니다. |

## <a name="location"></a>위치

Windows10에서 해당 앱 패키지 매니페스트에서 위치 기능을 선언 하는 응용 프로그램 실행 되 면 최종 사용자의 동의에 메시지가 나타납니다. 따라서 앱에서 고유한 사용자 지정 동의 확인 프롬프트가 표시되거나 켜기-끄기 토글이 제공되면 최종 사용자에게 한 번만 묻도록 제거할 수 있습니다.

## <a name="orientation"></a>방향

UWP 앱에서 **PhoneApplicationPage.SupportedOrientations** 및 **Orientation** 속성은 앱 패키지 매니페스트의 [**uap:InitialRotationPreference**](https://msdn.microsoft.com/library/windows/apps/dn934798) 요소에 해당합니다. 아직 선택하지 않은 경우 **응용 프로그램** 탭을 선택하고 **지원되는 회전**에서 하나 이상의 확인란을 선택하여 기본 설정을 기록하세요.

디바이스 방향과 화면 크기와 상관없이 UWP 앱의 UI를 보기 좋게 디자인하는 것이 좋습니다. 자세한 내용은 다음 항목인 [폼 팩터 및 사용자 환경에 대한 포팅](wpsl-to-uwp-form-factors-and-ux.md)을 참조하세요.

다음 항목은 [비즈니스 및 데이터 계층 포팅](wpsl-to-uwp-business-and-data.md)입니다.

