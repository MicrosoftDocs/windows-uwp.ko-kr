---
description: 이 문서에서는 데스크톱 응용 프로그램에서 UWP XAML UI를 호스트 하는 방법을 설명 합니다.
title: 데스크톱 응용 프로그램에서 호스팅 API는 UWP XAML을 사용 하 여
ms.date: 04/19/2019
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, win32, xaml 제도
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 9cb18abec43a4439b6d4750df797be5a1620a3aa
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63812742"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-desktop-application"></a>데스크톱 응용 프로그램에서 호스팅 API는 UWP XAML을 사용 하 여

Windows 10, 버전 1903, 비 UWP 데스크톱 응용 프로그램 시작 (WPF, Windows Forms를 포함 하 여 및 C++ Win32 응용 프로그램) 사용할 수는 *호스팅 API는 UWP XAML* 연관 된 모든 UI 요소에서 UWP 컨트롤을 호스트 하는 창 핸들 (HWND)입니다. 이 API는 UWP 컨트롤을 통해 이용할 수 있는 최신 Windows 10 UI 기능을 사용 하려면 비 UWP 데스크톱 응용 프로그램 수 있습니다. 비 UWP 데스크톱 응용 프로그램 수를 사용 하는 호스트 UWP 컨트롤에이 API를 사용 하는 예를 들어 합니다 [Fluent Design System](../design/fluent-design-system/index.md) 지원과 [Windows 잉크](../design/input/pen-and-stylus-interactions.md)합니다.

호스팅 API는 UWP XAML 광범위 한 개발자가 데스크톱 응용 프로그램 uwp 비 Fluent UI를 가져올 수 있도록 제공 되는 컨트롤 집합을 위한 토대를 제공 합니다. 이 기능은 이라고 *XAML 제도*합니다. 이 기능의 개요를 보려면 [데스크톱 응용 프로그램에서 UWP 컨트롤](xaml-host-controls.md)합니다.

> [!NOTE]
> XAML 아일랜드에 대 한 의견이 있는 경우에서 새 문제를 작성 합니다 [Microsoft.Toolkit.Win32 리포지토리](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) 두고 의견 있습니다. 개인적으로 피드백을 제출 하려는 경우에 보낼 수 있습니다를 XamlIslandsFeedback@microsoft.com입니다. Insights 및 시나리오에는 우리에 게 매우 중요 합니다.

## <a name="should-you-use-the-uwp-xaml-hosting-api"></a>호스팅 API는 UWP XAML을 사용 해야 합니까?

호스팅 API는 UWP XAML 데스크톱 응용 프로그램에서 UWP 컨트롤을 호스팅하기 위한 하위 수준 인프라를 제공 합니다. 일부 유형의 데스크톱 응용 프로그램 대체 하 고 편리한 Api를 사용 하 여이 목표를 달성 하는 옵션이 있습니다.  

* 있는 경우는 C++ Win32 데스크톱 응용 프로그램을 하려면 호스트 UWP 컨트롤에 응용 프로그램에서 호스팅 API는 UWP XAML을 사용 해야 합니다. 이러한 유형의 응용 프로그램에 대 한 대안이 없습니다.

* WPF 및 Windows Forms 응용 프로그램에 대 한 좋습니다를 사용 하는 합니다 [래핑된 컨트롤](xaml-host-controls.md#wrapped-controls) 및 [컨트롤을 호스팅할](xaml-host-controls.md#host-controls) 호스팅 API를 직접 UWP XAML을 사용 하는 대신 Windows 커뮤니티 도구 키트의 합니다. 이러한 컨트롤 호스팅 API에 내부적으로 UWP XAML을 사용 하 고 그렇지 않은 경우 호스팅 API를 직접 키보드 탐색 및 레이아웃 변경 내용을 포함 하 여 UWP XAML을 사용 하는 경우를 직접 처리 해야 하는 동작을 모두를 구현 합니다. 그러나 원하는 경우 이러한 유형의 응용 프로그램에서 직접 API를 호스트 하는 UWP XAML을 사용할 수 있습니다.

이 문서에서는 Windows 커뮤니티 도구 키트에서 제공 하는 컨트롤 대신 응용 프로그램에서 직접 API를 호스트 하는 UWP XAML을 사용 하는 방법을 설명 합니다.

## <a name="prerequisites"></a>사전 요구 사항

호스팅 API는 UWP XAML에는 이러한 필수 조건이 있습니다.

* Windows 10, 버전 1903 (또는 이상) 및 해당 Windows SDK의 빌드합니다.
* Windows 런타임 Api를 사용 하 고 수행 하 여 XAML 제도 사용 하도록 프로젝트를 구성 [이러한 지침](xaml-host-controls.md#requirements)합니다.

## <a name="architecture-of-xaml-islands"></a>XAML 아일랜드의 아키텍처

호스팅 API는 UWP XAML이 기본 Windows 런타임 형식 및 COM 인터페이스를 포함 합니다.

* [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager). 이 클래스는 UWP XAML 프레임 워크를 나타냅니다. 이 클래스는 단일 정적 제공 [ **InitializeForCurrentThread** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) 데스크톱 응용 프로그램에서 현재 스레드에서 UWP XAML 프레임 워크를 초기화 하는 메서드입니다.

* [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource). 이 클래스는 데스크톱 응용 프로그램에서 호스트 하는 UWP XAML 콘텐츠의 인스턴스를 나타냅니다. 이 클래스의 가장 중요 한 멤버를 [ **콘텐츠** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) 속성입니다. 이 속성을 할당 하는 [ **Windows.UI.Xaml.UIElement** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 호스트 하려는 합니다. 이 클래스에는 다음과 같은 XAML 제도 들어오고 라우팅 키보드 포커스 탐색을 위한 다른 멤버가 있습니다.

* **IDesktopWindowXamlSourceNative** 하 고 **IDesktopWindowXamlSourceNative2** COM 인터페이스입니다. **IDesktopWindowXamlSourceNative** 제공 합니다 **AttachToWindow** 메서드를 사용 하 여 부모 UI 요소에 응용 프로그램에서 XAML 아일랜드를 연결 합니다. **IDesktopWindowXamlSourceNative2** 제공 합니다 **PreTranslateMessage** 메서드를 특정 Windows 메시지를 올바르게 처리 하기 위해 UWP XAML 프레임 워크를 사용 하도록 설정 합니다.
    > [!NOTE]
    > 이러한 인터페이스에서 선언 되는 **windows.ui.xaml.hosting.desktopwindowxamlsource.h** Windows SDK의 헤더 파일입니다. 기본적으로이 파일은 %programfiles (x86) %\Windows Kits\10\Include\\< 빌드 번호\>\um 합니다. 에 C++ Win32 프로젝트에서이 헤더 파일을 직접 참조할 수 있습니다. WPF 또는 Windows Forms 프로젝트에서 사용 하 여 응용 프로그램 코드의 인터페이스를 선언 해야 합니다 [ **ComImport** ](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.comimportattribute) 특성입니다. 인터페이스 선언에는 정확히의 선언과 일치 해야 **windows.ui.xaml.hosting.desktopwindowxamlsource.h**합니다.

다음 다이어그램에서는 데스크톱 응용 프로그램에서 호스트 되는 XAML 섬에 있는 개체의 계층 구조를 보여 줍니다.

![DesktopWindowXamlSource 아키텍처](images/xaml-hosting-api-rev2.png)

* 기본 수준에서 XAML 아일랜드를 호스트 하려는 응용 프로그램에서 UI 요소를입니다. 이 UI 요소에 대 한 창 핸들 (HWND) 있어야 합니다. XAML 아일랜드를 호스트할 수 있습니다 하는 UI 요소의 예로 [ **System.Windows.Interop.HwndHost** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) WPF 응용 프로그램에 대 한 [ **System.Windows.Forms.Control** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) Windows Forms 응용 프로그램용으로 [창](https://docs.microsoft.com/windows/desktop/winmsg/about-windows) 에 대 한 C++ Win32 응용 프로그램입니다.

* 다음 수준은 **DesktopWindowXamlSource** 개체입니다. 이 개체는 XAML 아일랜드를 호스팅하기 위한 인프라를 제공 합니다. 코드는이 개체를 만들고 부모 UI 요소에 연결 하는 일을 담당 합니다.

* 만들 때를 **DesktopWindowXamlSource**,이 개체는 자동으로 UWP 컨트롤을 호스트 하는 네이티브 자식 창을 만듭니다. 이 네이티브 자식 창에는 대부분의 손을 벗어나 추상화 코드 하지만 해당 핸들 (HWND) 필요한 경우에 액세스할 수 있습니다.

* 마지막으로, 최상위 수준에서 데스크톱 응용 프로그램에서 호스트 하려는 UWP 컨트롤이입니다. 파생 되는 모든 UWP 개체 수 [ **Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), 사용자 지정 사용자 컨트롤 뿐 아니라 모든 Windows SDK에서 제공 하는 UWP 컨트롤을 포함 합니다.

## <a name="related-samples"></a>관련 샘플

호스팅 API 코드에서 UWP XAML을 사용 하는 방식의 응용 프로그램 형식, 응용 프로그램 및 기타 요인의 디자인에 따라 달라 집니다. 완전 한 응용 프로그램의 컨텍스트에서이 API를 사용 하는 방법에 설명 하이 문서는 다음 샘플에서 코드를 참조 합니다.

### <a name="c-win32"></a>C++Win32

[C++Win32 샘플](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island)합니다. 이 샘플에는 패키지 되지 않은 UWP 사용자 정의 컨트롤 호스팅의 전체 구현을 보여 줍니다. C++ Win32 응용 프로그램 (즉, 응용 프로그램을 MSIX 패키지로 포함 되어 있지).

### <a name="wpf-and-windows-forms"></a>WPF 및 Windows Forms

합니다 [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) Windows 커뮤니티 도구 키트 컨트롤 UWP WPF 및 Windows Forms 응용 프로그램에서 호스팅 API를 사용 하는 참조 샘플 역할도 합니다. 소스 코드는 다음 위치에서 제공 됩니다.

  * 컨트롤의 WPF 버전에 대 한 [여기](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost)합니다. WPF 버전에서 파생 [ **System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)합니다.

  * 컨트롤의 Windows Forms 버전용 [여기](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost)합니다. 파생 되는 Windows Forms 버전 [ **System.Windows.Forms.Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)합니다.

## <a name="host-uwp-xaml-controls"></a>호스트 UWP XAML 컨트롤

응용 프로그램에서 UWP 컨트롤을 호스트 하려면 호스팅 API는 UWP XAML을 사용 하는 기본 단계는 다음과 같습니다.

1. 응용 프로그램 중 하나를 만들기 전에 현재 스레드에 대 한 UWP XAML 프레임 워크를 초기화 합니다 [ **Windows.UI.Xaml.UIElement** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 에서 호스트 하는 개체를 [  **DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)합니다.

    * 응용 프로그램을 만드는 경우 합니다 **DesktopWindowXamlSource** 중 하나를 만들기 전에 개체를 **Windows.UI.Xaml.UIElement** 개체를 인스턴스화할 때이 프레임 워크를 초기화 됩니다는 **DesktopWindowXamlSource** 개체입니다. 이 시나리오에서는 프레임 워크를 초기화를 사용자 고유의 코드를 추가할 필요가 없습니다.

    * 그러나 응용 프로그램을 만드는 경우 합니다 **Windows.UI.Xaml.UIElement** 개체를 만들기 전에 **DesktopWindowXamlSource** 개체를 호스트 하는 응용 프로그램의 정적 를호출해야합니다[ **WindowsXamlManager.InitializeForCurrentThread** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) 메서드를 명시적으로 전에 UWP XAML 프레임 워크를 초기화 합니다 **Windows.UI.Xaml.UIElement** 개체는 인스턴스화됩니다. 응용 프로그램은 부모 UI 요소를 호스트 하는 경우에 일반적으로이 메서드를 호출 합니다 **DesktopWindowXamlSource** 인스턴스화됩니다.

    ```cppwinrt
    Windows::UI::Xaml::Hosting::WindowsXamlManager windowsXamlManager =
        Windows::UI::Xaml::Hosting::WindowsXamlManager::InitializeForCurrentThread();
    ```

    ```csharp
    global::Windows.UI.Xaml.Hosting.WindowsXamlManager windowsXamlManager =
        global::Windows.UI.Xaml.Hosting.WindowsXamlManager.InitializeForCurrentThread();
    ```

    > [!NOTE]
    > 이 메서드는 반환 된 [ **WindowsXamlManager** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) UWP XAML 프레임 워크에 대 한 참조를 포함 하는 개체입니다. 만큼 만들 수 있습니다 **WindowsXamlManager** 은 지정 된 스레드에 대해 원하는 개체입니다. 그러나 UWP XAML 프레임 워크에 대 한 참조를 유지 하는 각 개체에 없으므로 XAML 리소스가 결국 해제 되도록 개체는 삭제 해야 합니다.

2. 만들기는 [ **DesktopWindowXamlSource** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) 개체와 연결 된 창 핸들을 응용 프로그램에서 부모 UI 요소에 연결 합니다.

    이렇게 하려면 다음이 단계를 수행 해야 합니다.

    1. 만들기는 **DesktopWindowXamlSource** 개체를 캐스트 합니다 **IDesktopWindowXamlSourceNative** 또는 **IDesktopWindowXamlSourceNative2** COM 인터페이스입니다.

    2. 호출을 **AttachToWindow** 메서드는 **IDesktopWindowXamlSourceNative** 또는 **IDesktopWindowXamlSourceNative2** 인터페이스를 한 창 핸들을 전달 합니다 응용 프로그램에서 UI 요소를 부모입니다.

    3. 에 포함 된 내부 자식 창의 초기 크기를 설정 합니다 **DesktopWindowXamlSource**합니다. 기본적으로이 내부 자식 창의 너비와 높이가 0 설정 됩니다. 창에 추가한 모든 UWP 컨트롤의 크기를 설정 하지 않으면 합니다 **DesktopWindowXamlSource** 표시 되지 것입니다. 내부 자식 창에 액세스 하려면 합니다 **DesktopWindowXamlSource**를 사용 하 여를 **WindowHandle** 의 속성을 **IDesktopWindowXamlSourceNative** 또는 **IDesktopWindowXamlSourceNative2** 인터페이스입니다. 다음 예에서는 합니다 [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) 창의 크기를 설정 하는 함수입니다.

    이 프로세스를 보여 주는 코드 예제는 다음과 같습니다.

    ```cppwinrt
    // This example assumes you already have an HWND variable named 'parentHwnd' that
    // contains the handle of the parent window.
    Windows::UI::Xaml::Hosting::DesktopWindowXamlSource desktopWindowXamlSource;
    auto interop = desktopWindowXamlSource.as<IDesktopWindowXamlSourceNative>();
    check_hresult(interop->AttachToWindow(parentHwnd));

    HWND childInteropHwnd = nullptr;
    interop->get_WindowHandle(&childInteropHwnd);

    SetWindowPos(childInteropHwnd, 0, 0, 0, 300, 300, SWP_SHOWWINDOW);
    ```

    ```csharp
    // This WPF example assumes you already have an HwndHost named 'parentHwndHost'
    // that will act as the parent UI element for your XAML Island. It also assumes
    // you have used the DllImport attribute to import SetWindowPos from user32.dll
    // as a static method into a class named NativeMethods.
    Windows.UI.Xaml.Hosting.DesktopWindowXamlSource desktopWindowXamlSource =
        new Windows.UI.Xaml.Hosting.DesktopWindowXamlSource();

    IntPtr iUnknownPtr = System.Runtime.InteropServices.Marshal.GetIUnknownForObject(
        desktopWindowXamlSource);
    IDesktopWindowXamlSourceNative desktopWindowXamlSourceNative =
        System.Runtime.InteropServices.Marshal.Marshal.GetTypedObjectForIUnknown(
            iUnknownPtr, typeof(IDesktopWindowXamlSourceNative))
            as IDesktopWindowXamlSourceNative;

    desktopWindowXamlSourceNative.AttachToWindow(parentHwndHost.Handle);

    var childInteropHwnd = desktopWindowXamlSourceNative.WindowHandle;
    NativeMethods.SetWindowPos(childInteropHwnd, HWND_TOP, 0, 0, 300, 300, SWP_SHOWWINDOW);
    ```

3. 설정 합니다 **Windows.UI.Xaml.UIElement** 를 호스트 하려는 [ **콘텐츠** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) 속성에 **DesktopWindowXamlSource** 개체입니다. 다음 예제에서는 [ **Windows.UI.Xaml.Controls.Grid** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) 라는 ```myGrid``` 하는 **콘텐츠** 속성입니다.

   ```cppwinrt
   desktopWindowXamlSource.Content(myGrid);
   ```

   ```csharp
   desktopWindowXamlSource.Content = myGrid;
   ```

샘플 응용 프로그램의 컨텍스트에서 이러한 작업을 보여 주는 전체 예제를 다음 코드 파일을 참조 하세요.

  * **C++Win32:** 참조를 [XamlBridge.cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/XamlBridge.cpp) 파일을 [ C++ Win32 샘플](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island)합니다.

  * **WPF:** 참조를 [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs) 하 고 [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) Windows 커뮤니티 도구 키트의 파일입니다.  

  * **Windows Forms:** 참조를 [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs) 하 고 [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) Windows 커뮤니티 도구 키트의 파일입니다.

## <a name="handle-keyboard-input-and-focus-navigation"></a>키보드 입력 및 포커스 탐색 처리

몇 가지 방법으로 앱을 XAML 제도 호스팅하는 경우 키보드 입력 및 포커스 탐색을 올바르게 처리 하기 위해 수행 해야 합니다.

### <a name="keyboard-input"></a>키보드 입력

각 XAML 섬에 대 한 키보드 입력을 제대로 처리 하려면 응용 프로그램 해야 모든 Windows 메시지 전달 UWP XAML 프레임 워크 특정 메시지를 올바르게 처리할 수 있도록 합니다. 메시지 루프에 액세스할 수 있는 응용 프로그램의 특정 위치에서이 위해 캐스팅 합니다 **DesktopWindowXamlSource** 각 XAML 아일랜드에 대 한 개체를 **IDesktopWindowXamlSourceNative2** COM 인터페이스입니다. 차례로 호출 합니다 **PreTranslateMessage** 메서드의이 인터페이스와 현재 메시지를 전달 합니다.

  * C++ Win32 응용 프로그램에서 호출할 수 있습니다 **PreTranslateMessage** 주 메시지 루프에서 직접. 예제를 참조 하세요. 합니다 [SampleApp.cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/SampleApp.cpp#L61) 코드 파일을 [ C++ Win32 샘플](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island)합니다.

  * WPF 응용 프로그램을 호출할 수 있습니다 **PreTranslateMessage** 에 대 한 이벤트 처리기에서 합니다 [ **ComponentDispatcher.ThreadFilterMessage** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage?view=netframework-4.7.2) 이벤트입니다. 예를 들어 참조를 [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177) Windows 커뮤니티 도구 키트의 파일입니다.

  * Windows Forms 응용 프로그램을 호출할 수 있습니다 **PreTranslateMessage** 에 대 한 재정의에서 합니다 [ **Control.PreprocessMessage** ](https://docs.microsoft.com/en-us/dotnet/api/system.windows.forms.control.preprocessmessage?view=netframework-4.7.2) 메서드. 예를 들어 참조를 [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100) Windows 커뮤니티 도구 키트의 파일입니다.

### <a name="keyboard-focus-navigation"></a>키보드 포커스 탐색

사용자가 키보드를 사용 하 여 응용 프로그램에서 UI 요소를 통해 탐색 하는 경우 (예를 들어, 키를 눌러 **탭** 또는 방향/화살표 키), 내부 / 외부로 프로그래밍 방식으로 포커스를 이동 해야 합니다  **DesktopWindowXamlSource** 개체입니다. 사용자의 키보드 탐색에 도달 하면 합니다 **DesktopWindowXamlSource**, 첫 번째에 포커스를 이동 [ **Windows.UI.Xaml.UIElement** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 탐색 순서에서 개체 다음으로 포커스를 이동할 계속 하면 UI에 대 한 **Windows.UI.Xaml.UIElement** 사용자로 개체 요소를 순환 하 고 포커스를 이동할의 다시는 **DesktopWindowXamlSource**부모 UI 요소에 있습니다.  

호스팅 API는 UWP XAML 여러 형식 및 멤버 이러한 작업을 수행할 수 있도록 제공 합니다.

* 키보드 탐색 들어가면 프로그램 **DesktopWindowXamlSource**서 [ **GotFocus** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) 이벤트가 발생 합니다. 이 이벤트를 처리 하 고 프로그래밍 방식으로 포커스를 첫 번째 이동 호스팅된 **Windows.UI.Xaml.UIElement** 사용 하 여 합니다 [ **NavigateFocus** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus) 메서드.

* 마지막 요소로 포커스에 사용자가 있는 경우에 **DesktopWindowXamlSource** 전화를 받고 합니다 **탭** 키 또는 화살표 키를 합니다 [ **TakeFocusRequested** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested) 이벤트가 발생 합니다. 이 이벤트를 처리 하 고 프로그래밍 방식으로 호스트 응용 프로그램의 다음 포커스 가능 요소에 포커스를 이동 합니다. WPF 응용 프로그램의 예를 들어 여기서는 **DesktopWindowXamlSource** 에서 호스트 되는 [ **System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost), 사용할 수 있습니다를 [  **MoveFocus** ](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) 호스트 응용 프로그램의 다음 포커스 가능 요소에 포커스를 전송 하는 방법입니다.

샘플 응용 프로그램의 컨텍스트에서이 작업을 수행 하는 방법을 보여주는 예제를 다음 코드 파일을 참조 하세요.

  * **WPF:** 참조 된 [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) Windows 커뮤니티 도구 키트의 파일입니다.  

  * **Windows Forms:** 참조 된 [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) Windows 커뮤니티 도구 키트의 파일입니다.

## <a name="handle-layout-changes"></a>레이아웃 변경 내용을 처리합니다

사용자 부모 UI 요소의 크기를 변경 하는 경우 필요한 레이아웃 변경 되도록 예상 대로 표시 UWP 컨트롤을 처리 해야 합니다. 다음은 몇 가지 중요 한 시나리오를 고려해 야 합니다.

* 에 C++ Win32 응용 프로그램, 응용 프로그램에서 사용 하 여 호스트 된 XAML 섬을 재배치할 수 WM_SIZE 메시지를 처리할 때 합니다 [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) 함수입니다. 예제를 참조 하세요. 합니다 [SampleApp.cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/SampleApp.cpp#L191) 코드 파일을 [ C++ Win32 샘플](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island)합니다.

* 부모 UI 요소를 필요에 맞게 사각형 영역의 크기를 가져올 해야 하는 경우를 **Windows.UI.Xaml.UIElement** 에서 호스트 하는 합니다 **DesktopWindowXamlSource**를 호출 합니다 [ **측정값** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) 메서드를 **Windows.UI.Xaml.UIElement**합니다. 예를 들어 다음과 같은 가치를 제공해야 합니다.

    * WPF 응용 프로그램에서에서이 수행할 수 있습니다는 [ **MeasureOverride** ](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride) 메서드를 [ **HwndHost** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) 호스팅하는  **DesktopWindowXamlSource**합니다. 예를 들어 참조를 [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) Windows 커뮤니티 도구 키트의 파일입니다.

    * Windows Forms 응용 프로그램에서에서이 수행할 수 있습니다는 [ **GetPreferredSize** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize) 메서드를 [ **컨트롤** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) 를 호스팅하는 **DesktopWindowXamlSource**합니다. 예를 들어 참조를 [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) Windows 커뮤니티 도구 키트의 파일입니다.

* 부모 UI 요소의 크기가 변경 되 면 호출을 [ **정렬** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) 루트 메서드의 **Windows.UI.Xaml.UIElement** 에서 호스트 하는  **DesktopWindowXamlSource**합니다. 예를 들어 다음과 같은 가치를 제공해야 합니다.

    * WPF 응용 프로그램에서에서이 수행할 수 있습니다 합니다 [ **ArrangeOverride** ](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride) 메서드를 [ **HwndHost** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) 를호스트하는개체**DesktopWindowXamlSource**합니다. 예를 들어 참조를 [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) Windows 커뮤니티 도구 키트의 파일입니다.

    * Windows Forms 응용 프로그램에서 이렇게 할 수 있습니다에 대 한 처리기에서는 [ **SizeChanged** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged) 의 이벤트를 [ **컨트롤** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) 호스팅하는 **DesktopWindowXamlSource**합니다. 예를 들어 참조를 [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) Windows 커뮤니티 도구 키트의 파일입니다.

## <a name="handle-dpi-changes"></a>DPI 변경 내용을 처리합니다

UWP XAML 프레임 워크 (예를 들어 경우 사용자가 다른 화면 DPI 사용 하 여 모니터 간에 창을 끌) 호스 티 드 UWP 컨트롤에 대 한 DPI 변경 내용을 자동으로 처리 합니다. 최상의 환경을 위해 좋습니다에 Windows Forms, WPF 또는 C++ Win32 응용 프로그램이 모니터별 DPI를 인식 하 게 되도록 구성 되어 있습니다.

모니터별 DPI 인식 되도록 응용 프로그램을 구성 하려면 추가 된 [side-by-side-어셈블리 매니페스트에](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) 를 설정 하 고 프로젝트 ```<dpiAwareness>``` 요소를 ```PerMonitorV2```입니다. 이 값에 대 한 자세한 내용은 참조에 대 한 설명을 [ **DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2**](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context)합니다.

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

## <a name="host-custom-uwp-xaml-controls"></a>사용자 지정 UWP XAML 컨트롤 호스트

> [!IMPORTANT]
> 사용자 지정 UWP XAML 컨트롤을 호스트 하려면 응용 프로그램에 대해 컴파일할 수 있도록 컨트롤에 대 한 소스 코드를 있어야 합니다.

설명한 프로세스 외에도 다음과 같은 추가 작업을 수행 해야 합니다 (직접 정의한 컨트롤 또는 타사에서 제공 하는 컨트롤) 사용자 지정 UWP XAML 컨트롤을 호스트 하려는 경우는 [이전 섹션](#host-uwp-xaml-controls)합니다.

1. 파생 되는 사용자 지정 형식 정의 [ **Windows.UI.Xaml.Application** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application) 구현 하 고 [ **IXamlMetadataProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlmetadataprovider)합니다. 이 형식은 응용 프로그램의 현재 디렉터리의 어셈블리에 사용자 지정 UWP XAML 형식에 대 한 메타 데이터를 로드 하는 것에 대 한 루트 메타 데이터 공급자로 작동 합니다.

2. 호출 된 [ **GetXamlType** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlmetadataprovider.getxamltype) UWP XAML 컨트롤의 형식 이름을 할당 되 면 루트 메타 데이터 공급자의 메서드 (이 런타임 시 코드에서 할당 될 수 또는이를 사용 하도록 선택할 수 있습니다 할당 된 Visual Studio 속성 창에서).

    예를 들어 다음 코드 파일을 참조 하세요.
      * **C++Win32:** 참조를 [XamlApplication.cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/Microsoft.UI.Xaml.Markup/XamlApplication.cpp) 코드 파일을 [ C++ Win32 샘플](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island)합니다.

      * **WPF 및 Windows Forms**: 참조를 [XamlApplication.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Win32.UI.XamlHost/XamlApplication.cs) 하 고 [UWPTypeFactory.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Win32.UI.XamlHost/UWPTypeFactory.cs) 코드 Windows 커뮤니티 도구 키트의 파일입니다. 이러한 파일은 공유 구현의 일부를 **WindowsXamlHost** WPF와 이러한 유형의 앱에서 API를 호스트 하는 UWP XAML을 사용 하는 방법을 설명 하는 데 도움이 되는 Windows Forms에 대 한 클래스입니다.

3. 호스트 응용 프로그램 솔루션에 사용자 지정 UWP XAML 컨트롤에 대 한 소스 코드를 통합 하 고 사용자 지정 컨트롤을 빌드하고 응용 프로그램에서 사용 합니다. WPF 또는 Windows Forms 응용 프로그램에 대 한 지침은 [이러한 지침](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost#add-a-custom-uwp-control)합니다. 에 대 한 예는 C++ Win32 응용 프로그램 참조를 [Microsoft.UI.Xaml.Markup](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island/Microsoft.UI.Xaml.Markup) 및 [MyApp](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island/MyApp) 에서 프로젝트를 [ C++ Win32 샘플](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island).

## <a name="troubleshooting"></a>문제 해결

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>UWP 앱에서 API를 호스트 하는 UWP XAML을 사용 하 여 오류

| 문제점 | 해결 방법 |
|-------|------------|
| 앱이 수신 된 **COMException** 다음 메시지와 함께 합니다. "DesktopWindowXamlSource를 활성화할 수 없습니다. 이 형식을 사용할 수 없습니다 UWP 앱에서. " 또는 "WindowsXamlManager를 활성화할 수 없습니다. 이 형식을 사용할 수 없습니다 UWP 앱에서. " | 이 오류는 호스팅 API는 UWP XAML을 사용 하려는 나타냅니다 (특히 인스턴스화할 하려고 하는 [ **DesktopWindowXamlSource** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) 하거나 [  **WindowsXamlManager** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) 형식) UWP 앱에서. Windows Forms, WPF 등의 비 UWP 데스크톱 응용 프로그램에서 사용할 호스팅 API는 UWP XAML 것만 하 고 C++ Win32 응용 프로그램입니다. |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>다른 스레드에서 창에 연결 하는 오류

| 문제점 | 해결 방법 |
|-------|------------|
| 앱이 수신 된 **COMException** 다음 메시지와 함께 합니다. "AttachToWindow 메서드 다른 스레드에서 지정된 된 HWND를 만들었기 때문에 실패 합니다." | 이 오류는 응용 프로그램 호출을 나타냅니다 합니다 **IDesktopWindowXamlSourceNative::AttachToWindow** 메서드 다른 스레드에서 생성 된 창의 HWND를 전달 합니다. 메서드를 호출 하는 코드와 동일한 스레드에서 생성 된 창의 HWND이이 메서드를 전달 해야 합니다. |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>다른 최상위 창에서 창에 연결 하는 오류

| 문제점 | 해결 방법 |
|-------|------------|
| 앱이 수신 된 **COMException** 다음 메시지와 함께 합니다. "AttachToWindow 메서드 같은 스레드에서 AttachToWindow에 이전에 전달 된 HWND 보다 다른 최상위 창에서 지정한 HWND를 상속 하기 때문에 실패 합니다." | 이 오류는 응용 프로그램 호출을 나타냅니다 합니다 **IDesktopWindowXamlSourceNative::AttachToWindow** 메서드에 지정 된 창 보다 다른 최상위 창에서 상속 된 창의 HWND를 전달 하 고는 동일한 스레드에서이 메서드에 대 한 이전 호출 합니다.</p></p>응용 프로그램 호출 후 **IDesktopWindowXamlSourceNative::AttachToWindow** 특정 스레드에서 다른 모든 [ **DesktopWindowXamlSource** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) 동일한 개체 스레드 창 첫 번째 호출에서 전달 된 동일한 최상위 창의 하위 항목에 연결할 수 있습니다 **IDesktopWindowXamlSourceNative::AttachToWindow**합니다. 때 모든 합니다 **DesktopWindowXamlSource** 개체는 특정 스레드를 다음에 대 한 닫혀 **DesktopWindowXamlSource** 모든 창에 다시 연결할 무료 이면 합니다.</p></p>이 문제를 해결 하려면 하나를 모두 닫습니다 **DesktopWindowXamlSource** 개체를이 대 한 새 스레드를 만들거나이 스레드에서 다른 최상위 창에 바인딩되어 **DesktopWindowXamlSource**합니다. |

## <a name="related-topics"></a>관련 항목

* [데스크톱 응용 프로그램에서 UWP 컨트롤](xaml-host-controls.md)
* [C++Win32 XAML 제도 샘플](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island)
