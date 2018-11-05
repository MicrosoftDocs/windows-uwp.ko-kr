---
author: stevewhims
description: 이 포팅 가이드를 끝까지 읽어 보시고 프로젝트를 빌드하여 실행하는 단계를 신속하게 진행해 보세요.
title: UWP로 포팅 WindowsPhone Silverlight 문제 해결
ms.assetid: d9a9a2a7-9401-4990-a992-4b13887f2661
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3a5d3ab7b50721b969859006831b33e9b00e300f
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6039663"
---
#  <a name="troubleshooting-porting-windowsphone-silverlight-to-uwp"></a>UWP로 포팅 WindowsPhone Silverlight 문제 해결


이전 항목에서는 [프로젝트 포팅](wpsl-to-uwp-porting-to-a-uwp-project.md)에 대해 살펴봤습니다.

이 포팅 가이드를 끝까지 읽어 보시고 프로젝트를 빌드하여 실행하는 단계를 신속하게 진행해 보세요. 그렇게 하려면 필수가 아닌 코드에 주석이나 설명을 추가하고 나중에 돌아와서 해당 부채를 청산하여 일시적으로 진행할 수 있습니다. 이 항목의 문제 해결 증상 및 해결 방법 표는 다음 몇 개 항목을 대체하지는 않지만 이 단계에 유용한 정보를 제공합니다. 이후 항목을 진행하는 중에 언제든지 이 표를 다시 참조할 수 있습니다.

## <a name="tracking-down-issues"></a>문제 추적

예외 내에서 의미 있는 오류 메시지가 없는 경우에 특히 XAML 구문 분석 예외를 진단하는 것이 어려울 수 있습니다. 초기에 구문 분석 예외를 찾기 위해 첫째 예외를 catch하도록 디버거가 구성되어 있는지 확인하세요. 디버거에서 예외 변수를 검사하여 HRESULT 또는 메시지에 유용한 정보가 있는지 확인할 수 있습니다. XAML 파서로 오류 메시지 출력을 위한 Visual Studio 출력 창을 확인할 수도 있습니다.

앱을 종료 하는 경우 하였다 처리 되지 않은 예외가 XAML 태그 구문 분석 하는 동안 throw 된 다음 수 (즉, 리소스의 키가 Windows10 있지만 되지 WindowsPhone Silverlight 앱에 대 한 누락 된 리소스에 대 한 참조의 결과 앱, 예: 일부 시스템 **TextBlock** 스타일 키). 또는 **UserControl**, 사용자 지정 컨트롤 또는 사용자 지정 레이아웃 패널에서 예외가 발생했을 수 있습니다.

마지막으로 사용할 수 있는 방법은 이진 분할입니다. 페이지에서 태그의 약 1/2을 제거하고 앱을 다시 실행합니다. 그런 다음 오류가 제거된 부분에서 발생했는지 제거되지 *않은* 부분에서 발생했는지 여부를 확인합니다. 문제가 더 이상 발생하지 않을 때까지 오류가 포함된 부분을 계속해서 분할하면서 프로세스를 반복합니다.

## <a name="targetplatformversion"></a>TargetPlatformVersion

이 섹션에서는 Windows10 프로젝트를 Visual Studio에서 열었을 때 메시지가 표시 하는 경우 수행할 작업 "Visual Studio 업데이트 필요 설명. 하나 이상의 프로젝트에 설치되지 않았거나 Visual Studio의 향후 업데이트의 일부로 포함되는 플랫폼 SDK &lt;버전&gt;이(가) 필요합니다."라는 메시지가 표시될 경우 해야 할 일을 설명합니다.

-   먼저, 설치한 Windows10에 대 한 SDK의 버전 번호를 확인 합니다. **C:\\Program Files (x86)\\Windows Kits\\10\\Include\\&lt;versionfoldername&gt;** 으로 이동하고 *&lt;versionfoldername&gt;*(4중 표기의 "Major.Minor.Build.Revision"임)을 기록해 둡니다.
-   편집할 프로젝트 파일을 열고 `TargetPlatformVersion` 및 `TargetPlatformMinVersion` 요소를 찾습니다. 해당 요소를 다음과 같이 편집하고 *&lt;versionfoldername&gt;* 을 디스크에 있는 4중 표기 버전 번호로 바꿉니다.

```xml
   <TargetPlatformVersion><versionfoldername></TargetPlatformVersion>
   <TargetPlatformMinVersion><versionfoldername></TargetPlatformMinVersion>
```

## <a name="troubleshooting-symptoms-and-remedies"></a>문제 해결 증상 및 해결 방법

이 표의 해결 방법 정보는 차단 해제를 위한 충분한 정보를 제공하기 위한 것입니다. 뒤에 나오는 항목들을 참조하여 각 문제에 대한 세부 정보를 확인합니다.

| 증상 | 해결 방법 |
|---------|--------|
| XAML 파서 또는 컴파일러에서 "_이름 "&lt;typename&gt;"이(가) 네임스페이스 […]에 없습니다._"라는 오류를 발생합니다. | &lt;typename&gt;이 사용자 지정 형식인 경우 XAML 태그의 네임스페이스 접두사 선언에서 "clr-namespace"를 "using"으로 변경하고 어셈블리 토큰을 제거합니다. 플랫폼 형식의 경우 형식이 UWP(유니버설 Windows 플랫폼)에 적용되지 않으므로 해당 형식을 찾고 태그를 업데이트합니다. 즉시 발생할 수 있는 예로는 `phone:PhoneApplicationPage` 및 `shell:SystemTray.IsVisible`이 있습니다. | 
| XAML 파서 또는 컴파일러에서 "_구성원 "&lt;membername&gt;"을(를) 인식할 수 없거나 액세스할 수 없습니다._" 또는 "_"&lt;propertyname&gt;" 속성이 [...] 형식에 없습니다_."라는 오류를 발생합니다. | 이러한 오류는 루트 **Page**와 같은 일부 형식 이름을 포팅한 이후에 표시되기 시작합니다. 구성원 또는 속성이 UWP에 적용되지 않으므로 해당 형식을 찾고 태그를 업데이트합니다. 즉시 발생할 수 있는 예로는 `SupportedOrientations` 및 `Orientation`이 있습니다. |
| XAML 파서 또는 컴파일러에서 "_연결 가능한 속성 [...]이(가) [...]에 없습니다._" 또는 "_알 수 없는 연결 가능한 구성원 [...]이(가) 있습니다_."라는 오류를 발생합니다. | 연결된 속성이 아닌 형식이 원인일 수 있습니다. 이 경우 형식에 이미 오류가 있으므로 해당 오류를 해결하면 이 오류가 해결됩니다. 즉시 발생할 수 있는 예로는 `phone:PhoneApplicationPage.Resources` 및 `phone:PhoneApplicationPage.DataContext`이 있습니다. | 
|XAML 파서나 컴파일러 또는 런타임 예외에서 "_"&lt;resourcekey&gt;" 리소스를 확인할 수 없습니다._"라는 오류가 발생합니다. | 리소스 키는 UWP(유니버설 Windows 플랫폼) 앱에 적용되지 않습니다. 올바른 해당 리소스를 찾아서 태그를 업데이트합니다. 즉시 발생할 수 있는 예로는 시스템 **TextBlock** 스타일 키(예: `PhoneTextNormalStyle`)가 있습니다. |
| C# 컴파일러에서 "_[...]에 '&lt;이름&gt;' 형식 또는 네임스페이스 이름이 없습니다._" 또는 "_[...] 네임스페이스에 '&lt;이름&gt;’ 형식 또는 네임스페이스 이름이 없습니다._" 또는 "_'&lt;이름&gt;' 형식 또는 네임스페이스 이름이 현재 컨텍스트에 없습니다._"라는 오류를 발생합니다. | 컴파일러가 형식에 대해 올바른 UWP 네임스페이스를 아직 인식하지 못했기 때문일 수 있습니다. 이 문제를 해결하려면 Visual Studio의 **Resolve** 명령을 사용합니다. <br/>API가 유니버설 디바이스 패밀리로 알려진 API 집합에 없는 경우(즉, API가 확장 SDK에서 구현된 경우) [확장 SDK](wpsl-to-uwp-porting-to-a-uwp-project.md)를 사용합니다.<br/>포트가 덜 간단한 다른 경우도 있습니다. 즉시 발생할 수 있는 예로는 `DesignerProperties` 및 `BitmapImage`이 있습니다. | 
|장치에서 실행 될 때 앱이 종료, 했거나 Visual Studio에서 시작한 경우 "Windows 런타임 8.x 앱 [...]을 활성화할 수 없습니다. 활성화 요청이 실패했습니다. 오류: ‘Windows가 대상 응용 프로그램과 통신할 수 없습니다. 이는 일반적으로 대상 응용 프로그램의 프로세스가 중단되었음을 나타냅니다. […]”. | 초기화 중에 개발자 페이지 또는 바인딩된 속성 또는 다른 형식에서 실행 중인 명령적 코드에 문제가 있을 수 있습니다. 또는 앱이 종료될 때 표시하려는 XAML 파일을 구문 분석 하는 중에 발생할 수 있습니다. Visual Studio에서 시작할 경우에는 시작 페이지에서 발생할 수 있습니다. 잘못된 리소스 키를 찾아서 이 항목의 [문제 추적](#tracking-down-issues) 섹션의 몇 가지 지침을 참조하세요.|
| _XamlCompiler 오류 WMC0055: RectangleGeometry' 형식의 'Clip' 속성에 텍스트 값 '&lt;스트림 기하 도형&gt;'을(를) 할당할 수 없습니다._ | UWP에서는 [Microsoft DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) 및 XAML C++ UWP 앱의 형식입니다. |
| _XamlCompiler 오류 WMC0001: [...] XML 네임스페이스의 'RadialGradientBrush' 형식을 알 수 없습니다._ | UWP에는 **RadialGradientBrush** 형식이 없습니다. 태그에서 **RadialGradientBrush**를 제거하고 [Microsoft DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) 및 XAML C++ UWP 앱의 일부 다른 형식을 사용합니다. |
| _XamlCompiler 오류 WMC0011: '&lt;UIElement type&gt;' 요소의 'OpacityMask' 구성원을 알 수 없습니다._ | UWP  [Microsoft DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) 및 XAML C++ UWP 앱입니다. |
| _'System.Runtime.InteropServices.COMException' 형식의 첫째 예외가 SYSTEM.NI.DLL에서 발생했습니다. 추가 정보: 응용 프로그램이 다른 스레드를 위해 배열된 인터페이스를 호출했습니다. (예외가 발생한 HRESULT: 0x8001010E (RPC_E_WRONG_THREAD))._ | 수행 중인 작업을 UI 스레드에서 수행해야 합니다. [**CoreWindow.GetForCurrentThread**](https://msdn.microsoft.com/library/windows/apps/hh701589)를 호출합니다. |
| 애니메이션이 실행 중이지만 대상 속성에 영향을 주지 않습니다. | 독립 애니메이션으로 설정하거나 `EnableDependentAnimation="True"`를 설정합니다. [애니메이션](wpsl-to-uwp-porting-xaml-and-ui.md)을 참조하세요. |
| Windows10 프로젝트를 Visual Studio에서 열었을 때 "Visual Studio 업데이트 필요 표시. 하나 이상의 프로젝트에 설치되지 않았거나 Visual Studio의 향후 업데이트의 일부로 포함되는 플랫폼 SDK &lt;버전&gt;이(가) 필요합니다."라는 메시지가 표시될 경우 해야 할 일을 설명합니다. | 이 항목의 [TargetPlatformVersion](#targetplatformversion) 섹션을 참조하세요. |
| xaml.cs 파일에서 InitializeComponent가 호출되었을 때 System.InvalidCastException이 발생했습니다. | 이 예외는 동일한 xaml.cs 파일을 공유하는 둘 이상의 xaml 파일(이 중 하나 이상이 MRT 정규화됨)이 있고 요소가 두 xaml 파일 간에 일관되지 않은 x:Name 특성을 갖고 있는 경우 발생할 수 있습니다. 두 xaml 파일의 동일한 요소에 동일한 이름을 추가하거나 이름을 완전히 생략하세요. | 

다음 항목은 [XAML 및 UI 포팅](wpsl-to-uwp-porting-xaml-and-ui.md)입니다.

