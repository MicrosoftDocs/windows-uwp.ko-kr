---
description: 이 문서에서는 C++ Win32 앱에 대한 고급 XAML Island 호스팅 시나리오에 대해 설명합니다.
title: C++ Win32 앱의 XAML Islands에 대한 고급 시나리오
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10, uwp, cpp, win32, xaml islands, 래핑된 컨트롤, 표준 컨트롤
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 50ee005fc0de52a3e0217a71fb3d391445c486db
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80226237"
---
# <a name="advanced-scenarios-for-xaml-islands-in-c-win32-apps"></a>C++ Win32 앱의 XAML Islands에 대한 고급 시나리오

[표준 UWP 컨트롤 호스트](host-standard-control-with-xaml-islands-cpp.md) 및 [사용자 지정 UWP 컨트롤 호스트](host-custom-control-with-xaml-islands-cpp.md) 문서에서는 C++ Win32 앱에서 XAML Islands를 호스트하기 위한 지침과 예제를 제공합니다. 그러나 이 문서의 코드 예제로 데스크톱 애플리케이션에서 원활한 사용자 환경을 제공하기 위해 처리해야 할 수 있는 많은 고급 시나리오를 처리할 수 있는 것은 아닙니다. 이 문서에서는 이러한 시나리오에 대한 지침과 관련 코드 샘플의 핵심 사항을 제공합니다.

## <a name="keyboard-input"></a>키보드 입력

각 XAML Island에 대한 키보드 입력을 올바르게 처리하려면 특정 메시지가 올바르게 처리될 수 있도록 애플리케이션에서 모든 Windows 메시지를 UWP XAML 프레임워크로 전달해야 합니다. 이렇게 하려면 메시지 루프에 액세스할 수 있는 애플리케이션의 일부에서 각 XAML Island에 대한 **DesktopWindowXamlSource** 개체를 **IDesktopWindowXamlSourceNative2** COM 인터페이스로 캐스트합니다. 그런 다음 이 인터페이스의 **PreTranslateMessage** 메서드를 호출하고 현재 메시지를 전달합니다.

  * **C++ Win32:** : 앱은 기본 메시지 루프에서 직접 **PreTranslateMessage**를 호출할 수 있습니다. 예제는 [XamlBridge.cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp#L16) 파일을 참조하세요.

  * **WPF:** 앱은 [ComponentDispatcher. ThreadFilterMessage](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage) 이벤트에 대한 이벤트 처리기에서 **PreTranslateMessage**를 호출할 수 있습니다. 예제는 Windows 커뮤니티 도구 키트의 [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177) 파일을 참조하세요.

  * **Windows Forms:** 앱은 [Control.PreprocessMessage](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.preprocessmessage) 메서드에 대한 재정의에서 **PreTranslateMessage**를 호출할 수 있습니다. 예제는 Windows 커뮤니티 도구 키트의 [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100) 파일을 참조하세요.

## <a name="keyboard-focus-navigation"></a>키보드 포커스 탐색

사용자가 키보드를 사용하여 애플리케이션의 UI 요소를 탐색하는 경우(예를 들어 **Tab** 또는 방향/화살표 키를 누름) 프로그래밍 방식으로 **DesktopWindowXamlSource** 개체로 또는 개체 외부로 포커스를 이동해야 합니다. 사용자의 키보드 탐색이 **DesktopWindowXamlSource**에 도달 하면 UI의 탐색 순서에서 첫 번째 [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 개체에 포커스를 이동하고, 사용자가 요소를 순환할 때 다음 **Windows.UI.Xaml.UIElement** 개체로 포커스를 계속 이동한 후 **DesktopWindowXamlSource** 외부에서 부모 UI 요소로 다시 포커스를 이동합니다.  

UWP XAML 호스팅 API는 이러한 작업을 수행하는 데 도움이 되는 몇 가지 형식과 멤버를 제공합니다.

* 키보드 탐색이 **DesktopWindowXamlSource**로 이동하면 [GotFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) 이벤트가 발생합니다. 이 이벤트를 처리하고 [NavigateFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus) 메서드를 사용하여 프로그래밍 방식으로 호스트된 첫 번째 **Windows.UI.Xaml.UIElement**로 포커스를 이동합니다.

* 사용자가 **DesktopWindowXamlSource**의 포커스 가능한 마지막 요소에 있는 경우 **Tab** 키 또는 화살표 키를 누르면 [TakeFocusRequested](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested) 이벤트가 발생합니다. 이 이벤트를 처리하고 프로그래밍 방식으로 호스트 애플리케이션의 포커스 가능한 다음 요소로 포커스를 이동합니다. 예를 들어 **DesktopWindowXamlSource**가 [System.windows.interop.hwndhost>](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)에서 호스트되는 WPF 애플리케이션에서 [MoveFocus](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) 메서드를 사용하여 호스트 애플리케이션에서 포커스 가능한 다음 요소로 포커스를 이동할 수 있습니다.

작업 중인 샘플 애플리케이션의 컨텍스트에서 이 작업을 수행하는 방법을 보여 주는 예제는 다음 코드 파일을 참조하세요.

  * **C++/Win32**: [XamlBridge](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp) 파일을 참조하세요.

  * **WPF:** Windows 커뮤니티 도구 키트의 [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) 파일을 참조하세요.  

  * **Windows Forms:** Windows 커뮤니티 도구 키트의 [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) 파일을 참조하세요.

## <a name="handle-layout-changes"></a>레이아웃 변경 처리

사용자가 부모 UI 요소의 크기를 변경하는 경우 UWP 컨트롤이 원하는 대로 표시되도록 필요한 레이아웃 변경 내용을 처리해야 합니다. 고려해야 할 몇 가지 중요한 시나리오는 다음과 같습니다.

* C++ Win32 애플리케이션에서 애플리케이션이 WM_SIZE 메시지를 처리할 때 [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) 함수를 사용하여 호스트된 XAML Island의 위치를 변경할 수 있습니다. 예제에 대해서는 [SampleApp.cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/SampleApp.cpp#L170) 코드 파일을 참조하세요.

* 부모 UI 요소가 **DesktopWindowXamlSource**에서 호스트하는 **Windows.UI.Xaml.UIElement**를 맞추는 데 필요한 사각형 영역의 크기를 가져와야 하는 경우 **Windows.UI.Xaml.UIElement**의 [Measure](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) 메서드를 호출합니다. 예:

    * WPF 애플리케이션에서 **DesktopWindowXamlSource**를 호스트하는 [HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)의 [MeasureOverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride) 메서드에서 이 작업을 수행할 수 있습니다. 예제는 Windows 커뮤니티 도구 키트의 [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) 파일을 참조하세요.

    * Windows Forms 애플리케이션에서 **DesktopWindowXamlSource**를 호스트하는 [Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)의 [GetPreferredSize](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize) 메서드에서 이 작업을 수행할 수 있습니다. 예제는 Windows 커뮤니티 도구 키트의 [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) 파일을 참조하세요.

* 부모 UI 요소의 크기가 변경되면 **DesktopWindowXamlSource**에서 호스트하는 루트 **Windows.UI.Xaml.UIElement**의 [Arrange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) 메서드를 호출합니다. 예:

    * WPF 애플리케이션에서 **DesktopWindowXamlSource**를 호스트하는 [HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) 개체의 [ArrangeOverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride) 메서드에서 이 작업을 수행할 수 있습니다. 예제는 Windows 커뮤니티 도구 키트의 [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) 파일을 참조하세요.

    * Windows Forms 애플리케이션에서 **DesktopWindowXamlSource**를 호스트하는 [Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)의 [SizeChanged](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged) 이벤트에 대한 처리기에서 이 작업을 수행할 수 있습니다. 예제는 Windows 커뮤니티 도구 키트의 [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) 파일을 참조하세요.

## <a name="handle-dpi-changes"></a>DPI 변경 내용 처리

UWP XAML 프레임워크는 호스트된 UWP 컨트롤에 대한 DPI 변경을 자동으로 처리합니다(예: 사용자가 다른 화면 DPI를 사용하는 모니터 사이에서 창을 끌 때). 최상의 환경을 위해 Windows Forms, WPF 또는 C++ Win32 애플리케이션이 모니터별 DPI를 인식하도록 구성하는 것이 좋습니다.

애플리케이션을 모니터별 DPI를 인식하도록 구성하려면 [side-by-side 어셈블리 매니페스트](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests)를 프로젝트에 추가하고 **\<dpiAwareness\>** 요소를 **PerMonitorV2**로 설정합니다. 이 값에 대한 자세한 내용은 [DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context)에 대한 설명을 참조하세요.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
        <windowsSettings>
            <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2</dpiAwareness>
        </windowsSettings>
    </application>
</assembly>
```

## <a name="related-topics"></a>관련 항목

* [데스크톱 앱에서 UWP XAML 컨트롤 호스트(XAML Islands)](xaml-islands.md)
* [C++ Win32 앱에서 UWP XAML 호스팅 API 사용](using-the-xaml-hosting-api.md)
* [C++ Win32 앱에서 표준 UWP 컨트롤 호스트](host-standard-control-with-xaml-islands-cpp.md)
* [C++ Win32 앱에서 사용자 지정 UWP 컨트롤 호스트](host-custom-control-with-xaml-islands-cpp.md)
* [XAML Islands 코드 샘플](https://github.com/microsoft/Xaml-Islands-Samples)
* [C++Win32 XAML Islands 샘플](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32/SampleCppApp)
