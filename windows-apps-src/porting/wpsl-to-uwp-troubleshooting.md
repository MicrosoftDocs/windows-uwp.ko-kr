---
description: 이 포팅 가이드의 끝 부분에 대 한 읽기를 권장 하지만, 프로젝트를 빌드하고 실행 하는 단계를 계속 해 서 확인 하는 것도 이해 하 고 있습니다.
title: UWP로 Windows Phone Silverlight 포팅 문제 해결
ms.assetid: d9a9a2a7-9401-4990-a992-4b13887f2661
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a2c1353882ade36b5c1b82d0b75967d010ae0dc7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174827"
---
#  <a name="troubleshooting-porting-windowsphone-silverlight-to-uwp"></a>UWP로 Windows Phone Silverlight 포팅 문제 해결


이전 항목에서는 [프로젝트를 포팅](wpsl-to-uwp-porting-to-a-uwp-project.md)했습니다.

이 포팅 가이드의 끝 부분에 대 한 읽기를 권장 하지만, 프로젝트를 빌드하고 실행 하는 단계를 계속 해 서 확인 하는 것도 이해 하 고 있습니다. 이렇게 하려면 중요 하지 않은 코드를 주석 처리 하거나 구체적인 다음을 반환 하 여 나중에 해당 부채를 지불 하는 방식으로 임시 진행률을 만들 수 있습니다. 이 항목의 문제 해결 및 해결 방법 표는이 단계에서 도움이 될 수 있지만 다음 몇 가지 항목을 읽는 것은 아닙니다. 이후 항목을 진행 하면서 언제 든 지 테이블을 다시 참조할 수 있습니다.

## <a name="tracking-down-issues"></a>문제 추적

XAML 구문 분석 예외는 특히 예외 내에 의미 없는 오류 메시지가 없는 경우 진단 하기 어려울 수 있습니다. 첫 번째 예외를 catch 하도록 디버거가 구성 되어 있는지 확인 합니다 (조기에 구문 분석 예외를 시도 하 고 catch 하기 위해). 디버거에서 예외 변수를 검사 하 여 HRESULT 또는 메시지에 유용한 정보가 있는지 여부를 확인할 수 있습니다. 또한 Visual Studio의 출력 창에서 XAML 파서에서 출력 하는 오류 메시지를 확인 합니다.

앱이 종료 되 고 모든 것이 XAML 태그 구문 분석 중에 처리 되지 않은 예외가 throw 된 경우 누락 된 리소스에 대 한 참조의 결과일 수 있습니다. 즉, Windows Phone Silverlight 앱에 대 한 키가 있지만 일부 시스템 **TextBlock** 스타일 키와 같은 Windows 10 앱에 대해서는 존재 하지 않는 리소스입니다. 또는 **UserControl**, 사용자 지정 컨트롤 또는 사용자 지정 레이아웃 패널 내에서 예외가 throw 될 수 있습니다.

마지막 수단은 이진 분할입니다. 페이지에서 태그의 절반을 제거 하 고 앱을 다시 실행 합니다. 그런 다음 오류가 제거 된 절반 (어떤 경우에는 어떤 경우에만 복원 해야 함) 내에 있는지 또는 제거 *하지* 않은 절반에 있는지를 확인 합니다. 오류를 포함 하는 절반을 분할 하 여 프로세스를 반복 합니다.

## <a name="targetplatformversion"></a>TargetPlatformVersion

이 섹션에서는 Visual Studio에서 Windows 10 프로젝트를 열 때 "Visual Studio 업데이트 필요" 메시지가 표시 되는 경우 수행할 작업에 대해 설명 합니다. 하나 이상의 프로젝트에 &lt; &gt; 설치 되지 않았거나 Visual Studio에 대 한 향후 업데이트의 일부로 포함 된 platform SDK 버전이 필요 합니다. "

-   먼저 설치한 Windows 10 용 SDK의 버전 번호를 확인 합니다. **C: \\ Program Files (x86) Windows 키트 10으로 이동 하 여 \\ \\ \\ \\ &lt; Versionfoldername &gt; 을 포함** 하 고, "주. 부. 빌드. 수정 버전"을 포함 하는 * &lt; versionfoldername &gt; *을 적어 둡니다.
-   편집을 위해 프로젝트 파일을 열고 `TargetPlatformVersion` 및 요소를 찾습니다 `TargetPlatformMinVersion` . 다음과 같이 편집 하 여 * &lt; versionfoldername &gt; * 을 디스크에 있는 쿼드 표기법 버전 번호로 바꿉니다.

```xml
   <TargetPlatformVersion><versionfoldername></TargetPlatformVersion>
   <TargetPlatformMinVersion><versionfoldername></TargetPlatformMinVersion>
```

## <a name="troubleshooting-symptoms-and-remedies"></a>증상 및 해결 방법

테이블의 보상 정보는 자신을 차단 해제 하는 데 충분 한 정보를 제공 하기 위한 것입니다. 이러한 각 문제에 대 한 자세한 내용은 이후 항목을 참조 하세요.

| 증상 | 해결책 |
|---------|--------|
| XAML 파서 또는 컴파일러가 "_이름" &lt; typename &gt; "이 네임 스페이스 [...]에 없습니다._" 라는 오류를 제공 합니다. | &lt;Typename &gt; 이 사용자 지정 형식이 면 XAML 태그의 네임 스페이스 접두사 선언에서 "clr-namespace"를 "using"으로 변경 하 고 모든 어셈블리 토큰을 제거 합니다. 플랫폼 형식의 경우 형식이 UWP (유니버설 Windows 플랫폼)에 적용 되지 않으므로 해당 항목을 찾아 태그를 업데이트 합니다. 바로 발생할 수 있는 예는 `phone:PhoneApplicationPage` 및 `shell:SystemTray.IsVisible` 입니다. | 
| XAML 파서 또는 컴파일러는 "_" &lt; membername "멤버를 인식할 수 &gt; 없거나 액세스할 수 없습니다._" 라는 오류를 제공 합니다. 또는 "_속성" &lt; propertyname &gt; "이 형식 [...]."에_없습니다. | 이러한 오류는 루트 **페이지**와 같은 일부 형식 이름을 이식 하 고 나면 표시 되기 시작 합니다. 멤버 또는 속성이 UWP에 적용 되지 않으므로 해당 항목을 찾아 태그를 업데이트 합니다. 바로 발생할 수 있는 예는 `SupportedOrientations` 및 `Orientation` 입니다. |
| XAML 파서 또는 컴파일러가 "연결_가능한 속성 [...]의 오류를 제공 합니다. [...]를 찾을 수 없습니다._ 또는 "_알 수 없는 연결 가능한 멤버 [...]._"입니다. | 이는 연결 된 속성이 아닌 형식에 의해 발생할 수 있습니다. 이 경우 형식에 대해 오류가 발생 하 고이 오류가 해결 되 면이 오류가 사라집니다. 바로 발생할 수 있는 예는 `phone:PhoneApplicationPage.Resources` 및 `phone:PhoneApplicationPage.DataContext` 입니다. | 
|XAML 파서 또는 컴파일러 또는 런타임 예외는 "_리소스" &lt; Resourcekey &gt; "를 확인할_수 없습니다." 라는 오류를 제공 합니다. | 리소스 키는 유니버설 Windows 플랫폼 (UWP) 앱에 적용 되지 않습니다. 올바른 해당 리소스를 찾고 태그를 업데이트 합니다. 바로 발생할 수 있는 예는와 같은 시스템 **TextBlock** 스타일 키 `PhoneTextNormalStyle` 입니다. |
| C # 컴파일러는 "_' name ' 형식 또는 네임 스페이스 이름을 &lt; &gt; 찾을 수 없습니다. [...]_" 또는 "' 이름 '_형식 또는 네임 스페이스 이름이 &lt; &gt; _현재 컨텍스트에 없습니다." 또는 "형식 또는 네임 스페이스_이름이 &lt; &gt; 현재 컨텍스트에_없습니다." 라는 오류를 제공 합니다. | 이는 컴파일러가 특정 형식의 올바른 UWP 네임 스페이스를 아직 인식 하지 못하는 것일 수 있습니다. Visual Studio의 **Resolve** 명령을 사용 하 여 문제를 해결 합니다. <br/>API가 유니버설 장치 패밀리로 알려진 api 집합에 없는 경우 (즉, API는 확장 SDK에서 구현 됨) [확장 sdk](wpsl-to-uwp-porting-to-a-uwp-project.md)를 사용 합니다.<br/>포트가 간단 하지 않은 경우도 있습니다. 바로 발생할 수 있는 예는 `DesignerProperties` 및 `BitmapImage` 입니다. | 
|장치에서 실행 되는 경우 앱이 종료 되거나 Visual Studio에서 시작 될 때 "Windows 런타임 .x 앱을 활성화할 수 없습니다. [...] 오류가 표시 됩니다. ' Windows가 대상 응용 프로그램과 통신할 수 없습니다. ' 오류로 인해 활성화 요청이 실패 했습니다. 이는 일반적으로 대상 응용 프로그램의 프로세스가 중단 되었음을 나타냅니다. […]”. | 문제는 초기화 하는 동안 사용자의 페이지 또는 바인딩된 속성 (또는 기타 형식)에서 실행 되는 명령적 코드 일 수 있습니다. 또는 앱이 종료 될 때 표시 될 XAML 파일을 구문 분석 하는 동안 발생할 수 있습니다 (Visual Studio에서 시작 하는 경우 시작 페이지가 됨). 잘못 된 리소스 키를 확인 하 고이 항목의 [문제 추적](#tracking-down-issues) 섹션에 있는 지침 중 일부를 시도 합니다.|
| _XamlCompiler 오류 WMC0055: &lt; &gt; ' RectangleGeometry ' 형식의 ' Clip ' 속성에 텍스트 값 ' stream geometry '를 할당할 수 없습니다._ | UWP에서 [Microsoft DirectX](/windows/desktop/directx) 및 XAML c + + UWP 앱의 형식입니다. |
| _XamlCompiler 오류 WMC0001: XML 네임 스페이스 [...]에 알 수 없는 ' RadialGradientBrush ' 형식이 있습니다._ | UWP에 **RadialGradientBrush** 형식이 없습니다. 태그에서 **RadialGradientBrush** 을 제거 하 고 다른 형식의 [MICROSOFT DirectX](/windows/desktop/directx) 및 XAML c + + UWP 앱을 사용 합니다. |
| _XamlCompiler 오류 WMC0011: ' &lt; UIElement type ' 요소에 알 수 없는 ' OpacityMask ' 멤버가 있습니다. &gt;_ | UWP [Microsoft DirectX](/windows/desktop/directx) 및 XAML c + + UWP 앱입니다. |
| _SYSTEM.NI.DLL에서 ' T e m. COMException ' 형식의 첫 번째 예외가 발생 했습니다. 추가 정보: 응용 프로그램이 다른 스레드에 대해 마샬링된 인터페이스를 호출 했습니다. (HRESULT: 0x8001010E (RPC_E_WRONG_THREAD))에서 예외가 발생 했습니다._ | 사용자가 수행 하는 작업은 UI 스레드에서 수행 해야 합니다. [**CoreWindow**](/uwp/api/windows.ui.core.corewindow.getforcurrentthread)를 호출 합니다. |
| 애니메이션은 실행 중이지만 대상 속성에는 영향을 주지 않습니다. | 애니메이션을 독립적으로 만들거나 설정 `EnableDependentAnimation="True"` 합니다. [애니메이션](wpsl-to-uwp-porting-xaml-and-ui.md)을 참조하세요. |
| Visual Studio에서 Windows 10 프로젝트를 열면 "Visual Studio 업데이트 필요" 메시지가 표시 됩니다. 하나 이상의 프로젝트에 &lt; &gt; 설치 되지 않았거나 Visual Studio에 대 한 향후 업데이트의 일부로 포함 된 platform SDK 버전이 필요 합니다. " | 이 항목의 [Targetplatformversion](#targetplatformversion) 섹션을 참조 하세요. |
| Xaml.cs 파일에서 InitializeComponent을 호출 하면 InvalidCastException이 throw 됩니다. | 이 문제는 동일한 xaml.cs 파일을 공유 하는 하나 이상의 xaml 파일이 있는 경우, 그리고 요소에 두 xaml 파일 간에 일치 하지 않는 x:Name 특성이 있는 경우에 발생할 수 있습니다. 두 xaml 파일의 동일한 요소에 동일한 이름을 추가 하거나 이름을 모두 생략 합니다. | 

다음 항목에서는 [XAML 및 UI 포팅](wpsl-to-uwp-porting-xaml-and-ui.md)에 대해 설명 합니다.