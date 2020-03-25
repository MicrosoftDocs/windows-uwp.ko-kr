---
description: 이 문서에서는 Win32 앱에 대 한 고급 C++ XAML 고립 호스팅 시나리오를 설명 합니다.
title: Win32 앱의 C++ XAML 아일랜드에 대 한 고급 시나리오
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10, uwp, cpp, win32, xaml 아일랜드, 래핑된 컨트롤, 표준 컨트롤
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 50ee005fc0de52a3e0217a71fb3d391445c486db
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226237"
---
# <a name="advanced-scenarios-for-xaml-islands-in-c-win32-apps"></a>Win32 앱의 C++ XAML 아일랜드에 대 한 고급 시나리오

[표준 UWP 컨트롤을 호스트](host-standard-control-with-xaml-islands-cpp.md) 하 고 [사용자 지정 UWP 컨트롤을 호스트](host-custom-control-with-xaml-islands-cpp.md) 합니다. 문서에서는 C++ Win32 앱에서 XAML 아일랜드를 호스팅하기 위한 지침과 예제를 제공 합니다. 그러나이 문서의 코드 예제는 데스크톱 응용 프로그램에서 원활 하 게 사용자 환경을 제공 하기 위해 처리 해야 할 수 있는 많은 고급 시나리오를 처리 하지 않습니다. 이 문서에서는 이러한 시나리오 및 관련 코드 샘플에 대 한 포인터에 대 한 지침을 제공 합니다.

## <a name="keyboard-input"></a>키보드 입력

각 XAML 아일랜드에 대 한 키보드 입력을 올바르게 처리 하려면 특정 메시지가 올바르게 처리 될 수 있도록 응용 프로그램에서 모든 Windows 메시지를 UWP XAML 프레임 워크로 전달 해야 합니다. 이렇게 하려면 메시지 루프에 액세스할 수 있는 응용 프로그램의 일부에서 각 XAML 아일랜드의 **Desktopwindowxamlsource** 개체를 **IDesktopWindowXamlSourceNative2** COM 인터페이스로 캐스팅 합니다. 그런 다음이 인터페이스의 **PreTranslateMessage** 메서드를 호출 하 고 현재 메시지를 전달 합니다.

  * Win32:: 앱이 주 메시지 루프에서 직접 **PreTranslateMessage** 를 호출할 수 있습니다. **C++** 예제를 보려면 [Xamlbridge .cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp#L16) 파일을 참조 하세요.

  * **WPF:** 앱은 [Componentdispatcher. ThreadFilterMessage](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage) 이벤트에 대 한 이벤트 처리기에서 **PreTranslateMessage** 를 호출할 수 있습니다. 예를 들어 Windows 커뮤니티 도구 키트의 [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177) 파일을 참조 하세요.

  * **Windows Forms:** 앱은 **PreTranslateMessage** 를 호출할 수 있습니다 [. preprocessmessage](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.preprocessmessage) 메서드. 예를 들어 Windows 커뮤니티 도구 키트의 [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100) 파일을 참조 하세요.

## <a name="keyboard-focus-navigation"></a>키보드 포커스 탐색

사용자가 키보드를 사용 하 여 응용 프로그램의 UI 요소를 탐색 하는 경우 (예: **tab** 키 또는 방향/화살표 키 누르기), 프로그래밍 방식으로 **Desktopwindowxamlsource** 개체와 외부로 포커스를 이동 해야 합니다. 사용자의 키보드 탐색이 **Desktopwindowxamlsource**에 도달 하면 UI의 탐색 순서에서 첫 번째 [windows](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) . uiui&gt 개체에 포커스를 이동 하 고, 사용자가 요소를 순환할 때 포커스를 다음의 **창** 으로 이동 하 고, 사용자가 요소를 순환 **하 여 부모** UI 요소로 포커스를 이동 합니다.  

UWP XAML 호스팅 API는 이러한 작업을 수행 하는 데 도움이 되는 몇 가지 형식과 멤버를 제공 합니다.

* 키보드 탐색이 **Desktopwindowxamlsource**에 들어가면 [GotFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) 이벤트가 발생 합니다. 이 이벤트를 처리 하 고 [NavigateFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus) 메서드를 사용 하 여 프로그래밍 방식으로 포커스를 첫 번째 호스팅된 **창** 으로 이동 합니다.

* 사용자가 **Desktopwindowxamlsource** 에서 가장 포커스를 받을 수 있는 요소에 있고 **Tab** 키 또는 화살표 키를 누르면 [TakeFocusRequested](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested) 이벤트가 발생 합니다. 이 이벤트를 처리 하 고 호스트 응용 프로그램에서 포커스를 받을 수 있는 다음 요소로 포커스를 프로그래밍 방식으로 이동 합니다. 예를 들어 **Desktopwindowxamlsource** [system.windows.interop.hwndhost>](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)에서 호스트 되는 WPF 응용 프로그램에서 [movefocus](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) 메서드를 사용 하 여 호스트 응용 프로그램에서 포커스를 받을 수 있는 다음 요소로 포커스를 이동할 수 있습니다.

작업 중인 샘플 응용 프로그램의 컨텍스트에서이 작업을 수행 하는 방법을 보여 주는 예제는 다음 코드 파일을 참조 하세요.

  * /Win32: [xamlbridge .cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp) 파일을 참조 하세요.  **C++**

  * **WPF:** Windows 커뮤니티 도구 키트의 [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) 파일을 참조 하세요.  

  * **Windows Forms:** Windows 커뮤니티 도구 키트의 [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) 파일을 참조 하세요.

## <a name="handle-layout-changes"></a>레이아웃 변경 내용 처리

사용자가 부모 UI 요소의 크기를 변경 하는 경우 UWP 컨트롤이 원하는 대로 표시 되도록 필요한 레이아웃 변경 내용을 처리 해야 합니다. 고려해 야 할 몇 가지 중요 한 시나리오는 다음과 같습니다.

* C++ Win32 응용 프로그램에서 응용 프로그램이 WM_SIZE 메시지를 처리할 때 [setwindowpos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) 함수를 사용 하 여 호스팅된 XAML 아일랜드의 위치를 변경할 수 있습니다. 예제를 보려면 [sampleapp.exe](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/SampleApp.cpp#L170) 코드 파일을 참조 하세요.

* 부모 UI 요소가 **Desktopwindowxamlsource**에서 **호스팅하는 데** 필요한 사각형 영역의 크기를 표시 해야 하는 경우에는 **windows.** x x. i x. i x. i a. [Measure](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) 예를 들면 다음과 같습니다.

    * WPF 응용 프로그램에서는 **Desktopwindowxamlsource**를 호스트 하는 [system.windows.interop.hwndhost>](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) 의 [system.windows.frameworkelement.measureoverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride) 메서드에서이 작업을 수행할 수 있습니다. 예를 들어 Windows 커뮤니티 도구 키트의 [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) 파일을 참조 하세요.

    * Windows Forms 응용 프로그램에서는 **Desktopwindowxamlsource**를 호스트 하는 [컨트롤](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) 의 [GetPreferredSize](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize) 메서드에서이 작업을 수행할 수 있습니다. 예를 들어 Windows 커뮤니티 도구 키트의 [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) 파일을 참조 하세요.

* 부모 UI 요소의 크기가 변경 되 면 **Desktopwindowxamlsource**에서 호스트 하는 루트 **Windows. .xaml. UIElement** 의 [정렬](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) 메서드를 호출 합니다. 예를 들면 다음과 같습니다.

    * WPF 응용 프로그램에서는 **Desktopwindowxamlsource**를 호스팅하는 [System.windows.interop.hwndhost>](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) 개체의 [system.windows.frameworkelement.arrangeoverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride) 메서드에서이 작업을 수행할 수 있습니다. 예를 들어 Windows 커뮤니티 도구 키트의 [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) 파일을 참조 하세요.

    * Windows Forms 응용 프로그램에서는 **Desktopwindowxamlsource**를 호스트 하는 [컨트롤](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) 의 [SizeChanged](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged) 이벤트에 대 한 처리기에서이 작업을 수행할 수 있습니다. 예를 들어 Windows 커뮤니티 도구 키트의 [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) 파일을 참조 하세요.

## <a name="handle-dpi-changes"></a>DPI 변경 내용 처리

UWP XAML 프레임 워크는 호스트 된 UWP 컨트롤에 대 한 DPI 변경을 자동으로 처리 합니다 (예: 사용자가 다른 화면 DPI를 사용 하는 모니터 사이에서 창을 끌 때). 최상의 환경을 위해 Windows Forms, WPF 또는 C++ Win32 응용 프로그램이 모니터 당 DPI를 인식 하도록 구성 하는 것이 좋습니다.

모니터 별로 응용 프로그램 DPI를 인식 하도록 구성 하려면 프로젝트에 [side-by-side 어셈블리 매니페스트](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) 를 추가 하 고 **\<\>** 요소를 **PerMonitorV2**로 설정 합니다. 이 값에 대 한 자세한 내용은 [DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context)에 대 한 설명을 참조 하십시오.

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

* [데스크톱 앱에서 UWP XAML 컨트롤 호스트 (XAML 제도)](xaml-islands.md)
* [C++ Win32 앱에서 UWP XAML 호스팅 API 사용](using-the-xaml-hosting-api.md)
* [C++ Win32 앱에서 표준 UWP 컨트롤 호스팅](host-standard-control-with-xaml-islands-cpp.md)
* [C++ Win32 앱에서 사용자 지정 UWP 컨트롤 호스트](host-custom-control-with-xaml-islands-cpp.md)
* [XAML 아일랜드 코드 샘플](https://github.com/microsoft/Xaml-Islands-Samples)
* [C++Win32 XAML 아일랜드 샘플](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32/SampleCppApp)
