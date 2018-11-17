---
author: mcleanbyron
description: 이 문서에는 데스크톱 응용 프로그램에서 UWP XAML UI를 호스트 하는 방법을 설명 합니다.
title: 호스팅 API는 데스크톱 응용 프로그램에서 UWP XAML을 사용 하 여
ms.author: mcleans
ms.date: 09/21/2018
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, win32
ms.localizationpriority: medium
ms.openlocfilehash: 69eb9f72d6b4cf01185f7e4886a7ed5c30a669df
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/16/2018
ms.locfileid: "7145343"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-desktop-application"></a>호스팅 API는 데스크톱 응용 프로그램에서 UWP XAML을 사용 하 여

> [!NOTE]
> UWP XAML 호스팅 API 및 XAML 제도 개발자 미리 보기를 현재 사용할 수 있습니다. 하지만 직접 사용해 프로토타입 작성 하는 코드에서 이제 새, 사용 하는 이러한 프로덕션 코드에서이 시간에 하지 않는 것이 좋습니다. 이러한 기능은 성숙 안정화 나중에 Windows 릴리스를 계속 됩니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다.
>
> API 및 XAML 제도 호스팅 XAML에 대 한 피드백을 있는 경우에 피드백을 보내 XamlIslandsFeedback@microsoft.com. 통찰력과 시나리오는 저희에 게 매우 중요 합니다.

Windows 10 Insider Preview SDK에서 17709 빌드할 비 UWP 데스크톱 응용 프로그램 (WPF, Windows Forms, 및 c + + Win32 응용 프로그램 포함) 창 핸들 (와 연결 된 모든 UI 요소에서 호스트 UWP 컨트롤에 *UWP XAML 호스팅 API를* 사용할 수 있습니다. HWND)입니다. 이 API를 사용 하면 비 UWP만 UWP 컨트롤을 통해 사용할 수 있는 최신 Windows 10 UI 기능을 사용 하 여 데스크톱 응용 프로그램. 예를 들어, 비 UWP 데스크톱 응용 프로그램은 [Windows 잉크](../design/input/pen-and-stylus-interactions.md)지원 및 [Fluent 디자인 시스템](../design/fluent-design-system/index.md) 을 사용 하는 호스트 UWP 컨트롤에이 API를 사용할 수 있습니다.

UWP XAML API를 호스팅 더 광범위 한 개발자가 데스크톱 응용 프로그램을 비 UWP Fluent UI를 가져올 수 있도록 하려면 제공 하는 컨트롤 집합에 대 한 기초를 제공 합니다. 이 시나리오는 *XAML 제도*라고도 합니다. 이 개발자 시나리오에 대 한 자세한 내용은 [데스크톱 응용 프로그램의 UWP 컨트롤](xaml-host-controls.md)을 참조 하세요.

## <a name="is-the-uwp-xaml-hosting-api-right-for-your-desktop-application"></a>UWP XAML 오른쪽 데스크톱 응용 프로그램에 대 한 API를 호스팅하는?

UWP XAML API를 호스팅 호스팅 데스크톱 응용 프로그램에서 UWP 컨트롤에 대 한 하위 수준 인프라를 제공 합니다. 일부 유형의 데스크톱 응용 프로그램 대체 하 고 더 편리 하 게 Api를 사용 하 여이 목표를 달성 하는 옵션이 있습니다.  

* C + + Win32 데스크톱 응용 프로그램이 있고 응용 프로그램의 호스트 UWP 컨트롤 하려는 경우 호스팅 API UWP XAML을 사용 해야 합니다. 이러한 유형의 응용 프로그램에 대 한 대안이 없습니다.

* WPF 및 Windows Forms 응용 프로그램에 대 한 권장 [래핑된 컨트롤](xaml-host-controls.md#wrapped-controls) 및 [호스트 컨트롤](xaml-host-controls.md#host-controls) 에 UWP XAML 대신 Windows 커뮤니티 도구 키트 사용 호스팅 API입니다. 이러한 컨트롤 호스팅 API 내부적으로 UWP XAML을 사용 하 고 간단한 개발 환경을 제공 합니다. 그러나 선택 하는 경우 이러한 유형의 응용 프로그램에서 직접 API 호스팅 UWP XAML을 사용할 수 있습니다.

## <a name="related-samples"></a>관련 샘플

UWP XAML 코드에서 호스팅 API를 사용 하는 방식에 응용 프로그램 유형, 응용 프로그램 및 기타 요인의 디자인에 따라 다릅니다. 완전 한 응용 프로그램의 컨텍스트에서이 API를 사용 하는 방법을 돕기 위해이 문서는 다음 샘플에서 코드를 참조 합니다.

### <a name="c-win32"></a>C + + Win32

호스팅 API는 c + + Win32 응용 프로그램에서 UWP XAML을 사용 하는 방법을 보여 주는 샘플 GitHub의 여러 가지가 있습니다.

  * [XamlHostingSample](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting)합니다. 이 샘플에는 UWP [InkCanvas](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas), [InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)및 [MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) 컨트롤 c + + Win32 응용 프로그램을 추가 하는 방법을 보여 줍니다.
  * [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32)합니다. 이 샘플에는 몇 가지 기본 UWP 컨트롤 c + + Win32 응용 프로그램을 추가 하 고 DPI 변경을 처리 하는 방법을 보여 줍니다.

### <a name="wpf-and-windows-forms"></a>WPF 및 Windows Forms

Windows 커뮤니티 도구 키트에서 [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 컨트롤 호스팅 API WPF 및 Windows Forms 응용 프로그램의 UWP를 사용 하 여에 대 한 참조 샘플 역할을 합니다. 소스 코드는 다음 위치에서 사용할 수 있습니다.

  * [여기로 이동](https://github.com/Microsoft/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Wpf.UI.XamlHost)하는 컨트롤의 WPF 버전입니다. WPF 버전 [**System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)에서 파생 됩니다.
  * [여기로 이동](https://github.com/Microsoft/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Forms.UI.XamlHost)하는 컨트롤의 Windows Forms 버전입니다. Windows Forms 버전 [**System.Windows.Forms.Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)에서 파생 됩니다.

## <a name="prerequisites"></a>사전 요구 사항

호스팅 API UWP XAML에 이러한 필수 조건이 있습니다.

* Windows 10 Insider Preview 빌드 17709 (또는 이후 빌드) 및 Windows SDK의 해당 Insider Preview 빌드입니다. 진화 기능 이기 때문에 최신 버전을 사용 하는 것이 좋습니다 최상의 환경을 위해 빌드하십시오.

* 호스팅 API 데스크톱 응용 프로그램에서 UWP XAML을 사용 하려면 프로젝트를 UWP Api를 호출할 수 있도록 구성 해야 합니다.

    * **C + + Win32:** 사용 하도록 프로젝트를 구성 하는 것이 좋습니다 [C + + WinRT](../cpp-and-winrt-apis/index.md)합니다. 다운로드 및 설치 합니다 [C + + WinRT Visual Studio Extension (VSIX)](https://aka.ms/cppwinrt/vsix) Visual Studio Marketplace에서 다음을 추가 합니다 ```<CppWinRTEnabled>true</CppWinRTEnabled>``` 속성으로 설명 [여기](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).vcxproj 파일을 합니다.

    * **Windows Forms 및 WPF:** [이러한 지침](../porting/desktop-to-uwp-enhance.md)을 따릅니다.

## <a name="architecture-of-xaml-islands"></a>XAML 제도의 아키텍처

UWP XAML API를 호스팅 [**Windows.UI.Xaml.Hosting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting) 네임 스페이스의 [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource), [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)및 기타 관련된 형식을 포함 합니다. 데스크톱 응용 프로그램은 UWP 컨트롤을 렌더링 하 고 키보드 포커스 탐색 요소 들어오고 경로를이 API를 사용할 수 있습니다. 또한 데스크톱 응용 프로그램 크기를 조정 하 고 원하는 대로 UWP 컨트롤을 배치 수 있습니다.

데스크톱 응용 프로그램에서 API를 호스팅하는 XAML을 사용 하 여 XAML 섬을 만들 때 개체의 다음 계층 구조를 포함 됩니다.

* 기본 수준에서 XAML 섬 호스트 하려는 응용 프로그램의 UI 요소를입니다. 이 UI 요소에는 창 핸들 (HWND)가 있어야 합니다. WPF 응용 프로그램, Windows Forms 응용 프로그램에 대 한 [**System.Windows.Forms.Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) 및 c + + Win32 응용 프로그램에 대 한 [창](https://docs.microsoft.com/windows/desktop/winmsg/about-windows) 에 대 한 [**System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) 를 포함 하는 UI 요소는 XAML 섬 호스팅할 수 있습니다.

* 다음 수준은 **DesktopWindowXamlSource** 개체입니다. 이 개체는 XAML 섬 호스팅을 위한 인프라를 제공 합니다. 코드는이 개체를 만들고 부모 UI 요소에 연결 해야 합니다.

* **DesktopWindowXamlSource**만들 때이 개체는 자동으로 호스트 UWP 컨트롤에 기본 자식 창을 만듭니다. 코드에서이 기본 자식 창을 추상화 대부분은 하지만 해당 핸들 (HWND) 필요한 경우에 액세스할 수 있습니다.

* 마지막으로, 최상위 수준에서 데스크톱 응용 프로그램에서 호스트 하려는 UWP 컨트롤이입니다. 이 [**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), 사용자 지정 사용자 컨트롤 뿐만 아니라 Windows SDK에서 제공 하는 모든 UWP 컨트롤을 포함 하 여에서 파생 되는 모든 UWP 개체 수 있습니다.

다음 다이어그램은 XAML 섬에 있는 개체의 계층 구조를 보여 줍니다.

![DesktopWindowXamlSource 아키텍처](images/xaml-hosting-api-rev2.png)

## <a name="how-to-host-uwp-xaml-controls"></a>UWP XAML 컨트롤을 호스트 하는 방법

UWP XAML 호스팅 API를 사용 하 여 호스트 응용 프로그램에서 UWP 컨트롤에 대 한 주요 단계는 다음과 같습니다.

1. 응용 프로그램에서 [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)에서 호스팅하는 [**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 개체를 만들기 전에 현재 스레드에 대 한 UWP XAML 프레임 워크를 초기화 합니다.

    * **DesktopWindowXamlSource** 개체를 인스턴스화할 때이 프레임 워크를 초기화는 응용 프로그램을 만드는 경우 **DesktopWindowXamlSource** 개체 **Windows.UI.Xaml.UIElement** 개체 중 하나를 만들기 전에 . 이 시나리오에서는 프레임 워크를 초기화 하려면 자신만의 코드를 추가할 필요가 없습니다.

    * 그러나 응용 프로그램을 호스팅할 **DesktopWindowXamlSource** 개체를 만들기 전에 **Windows.UI.Xaml.UIElement** 개체를 만들고, 응용 프로그램 호출 해야 합니다 [**정적 WindowsXamlManager.InitializeForCurrentThread**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) **Windows.UI.Xaml.UIElement** 개체가 인스턴스화됩니다 전에 UWP XAML 프레임 워크를 명시적으로 초기화 하는 방법. 일반적으로 응용 프로그램 **DesktopWindowXamlSource** 를 호스팅하는 부모 UI 요소를 인스턴스화할 때이 메서드를 호출 해야 합니다.

    ```cppwinrt
    Windows::UI::Xaml::Hosting::WindowsXamlManager windowsXamlManager =
        Windows::UI::Xaml::Hosting::WindowsXamlManager::InitializeForCurrentThread();
    ```

    ```csharp
    global::Windows.UI.Xaml.Hosting.WindowsXamlManager windowsXamlManager =
        global::Windows.UI.Xaml.Hosting.WindowsXamlManager.InitializeForCurrentThread();
    ```

    > [!NOTE]
    > 이 메서드는 UWP XAML 프레임 워크에 대 한 참조를 포함 하는 [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) 개체를 반환 합니다. 지정 된 스레드에서 원하는 만큼 **WindowsXamlManager** 개체를 만들 수 있습니다. 그러나 UWP XAML 프레임 워크에 대 한 참조를 유지 하는 각 개체에 없으므로 XAML 리소스는 결국 해제 되도록 개체를 삭제 해야 합니다.

2. [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) 개체를 만들고 창 핸들와 연결 된 응용 프로그램에서 부모 UI 요소에 연결 합니다.

    이렇게 하려면 다음이 단계를 수행 해야 합니다.

    1. **DesktopWindowXamlSource** 개체를 만들고 **IDesktopWindowXamlSourceNative** COM 인터페이스로 캐스팅 합니다. 이 인터페이스에서 선언 되는 ```windows.ui.xaml.hosting.desktopwindowxamlsource.h``` Windows SDK의 헤더 파일입니다. C + + Win32 프로젝트에이 헤더 파일을 직접 참조할 수 있습니다. Windows Forms 또는 WPF 프로젝트에서 [**ComImport**](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.comimportattribute) 특성을 사용 하 여 응용 프로그램 코드에서이 인터페이스를 선언 해야 합니다. 인터페이스 선언에서 인터페이스 선언을 정확 하 게 일치 하는지 확인 ```windows.ui.xaml.hosting.desktopwindowxamlsource.h```.

    2. **IDesktopWindowXamlSourceNative** 인터페이스의 **AttachToWindow** 메서드를 호출 하 고 응용 프로그램에서 부모 UI 요소의 창 핸들을 전달 합니다.

    3. 초기 **DesktopWindowXamlSource**에 포함 된 내부 자식 창 크기를 설정 합니다. 기본적으로이 내부 자식 창 너비와 높이 0에 설정 됩니다. 창 크기를 설정 하지 않으면 **DesktopWindowXamlSource** 에 추가 하는 모든 UWP 컨트롤은 표시 됩니다. **DesktopWindowXamlSource**의 내부 자식 창에 액세스 하려면 **IDesktopWindowXamlSourceNative** 인터페이스의 **WindowHandle** 속성을 사용 합니다. 다음 예제에서는 창 크기를 설정 하려면 [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) 함수를 사용 합니다.

    이 프로세스를 보여 주는 일부 코드 예제는 다음과 같습니다.

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
    // that will act as the parent UI elemnt for your XAML island. It also assumes
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

3. **DesktopWindowXamlSource** 개체의 [**콘텐츠**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) 속성을 호스트 하려면 **Windows.UI.Xaml.UIElement** 를 설정 합니다. 다음 예제에서는 설정 라는 [**Windows.UI.Xaml.Controls.Grid**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) ```myGrid``` **콘텐츠** 속성에 있습니다.

   ```cppwinrt
   desktopWindowXamlSource.Content(myGrid);
   ```

   ```csharp
   desktopWindowXamlSource.Content = myGrid;
   ```

작업 샘플 응용 프로그램의 컨텍스트에서 이러한 작업을 보여 주는 전체 예제는 다음 코드 파일을 참조 하세요.

  * **C + + Win32:** [XamlHostingSample](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting) 샘플에서 [Main.cpp](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting/blob/master/XamlHostingSample/Main.cpp) 파일 또는 [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32) 샘플에서 [Desktop.cpp](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/Desktop.cpp) 파일을 참조 하세요.
  * **WPF:** Windows 커뮤니티 도구 키트에서 [WindowsXamlHostBase.cs](https://github.com/Microsoft/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs) 및 [WindowsXamlHost.cs](https://github.com/Microsoft/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) 파일을 참조 하세요.  
  * **Windows Forms:** Windows 커뮤니티 도구 키트에서 [WindowsXamlHostBase.cs](https://github.com/Microsoft/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs) 및 [WindowsXamlHost.cs](https://github.com/Microsoft/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) 파일을 참조 하세요.


## <a name="how-to-host-custom-uwp-xaml-controls"></a>사용자 지정 하는 호스트에 UWP XAML 컨트롤 방법을

> [!IMPORTANT]
> 현재 C# WPF 및 Windows Forms 응용 프로그램의 사용자 지정 UWP XAML 컨트롤에서 타사만 지원 됩니다. 응용 프로그램에서이 대해 컴파일할 수 있도록 컨트롤에 대 한 소스 코드가 있어야 합니다.

호스트 (직접 정의 하는 컨트롤 또는 타사에서 제공 하는 컨트롤)는 UWP XAML 컨트롤 사용자 지정 하려는 경우 [이전 섹션](#how-to-host-uwp-xaml-controls)에 설명 된 프로세스 외에도 다음과 같은 추가 작업을 수행 해야 합니다.

1. [**Windows.UI.Xaml.Application**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application) 에서 파생 되 고 [**IXamlMetadataProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlmetadataprovider)을 구현 하는 사용자 지정 형식을 정의 합니다. 이 이와 같은 역할을 루트 메타 데이터 공급자 응용 프로그램의 현재 디렉터리에 어셈블리에서 사용자 지정 XAML UWP 형식에 대 한 메타 데이터를 로드 합니다.

    이 작업을 수행 하는 방법을 보여 주는 예제를 Windows 커뮤니티 도구 키트의 [XamlApplication.cs](https://github.com/Microsoft/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Win32/Microsoft.Windows.Interop.WindowsXamlHost.Shared/XamlApplication.cs) 코드 파일을 참조 하세요. 이 파일은 WPF 및 Windows Forms, UWP XAML 호스팅 이러한 유형의 앱에서 API를 사용 하는 방법을 설명 하는 데 도움이 되는 **WindowsXamlHost** 클래스 구현의 공유 일부입니다.

2. 루트 메타 데이터 공급자의 [**GetXamlType**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlmetadataprovider.getxamltype) 메서드를 호출 하 여 UWP XAML 컨트롤의 형식 이름 할당 될 때 (이 런타임 시 코드에서 할당 될 수 또는 Visual Studio 속성 창에서 할당 하려면이 옵션을 사용 하도록 선택할 수 있습니다).

    이 작업을 수행 하는 방법을 보여 주는 예제를 Windows 커뮤니티 도구 키트의 [UWPTypeFactory.cs](https://github.com/Microsoft/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Win32/Microsoft.Windows.Interop.WindowsXamlHost.Shared/UWPTypeFactory.cs) 코드 파일을 참조 하세요. 이 파일에는 WPF 및 Windows Forms에 대 한 공유 구현의 **WindowsXamlHost** 클래스의 일부입니다.

3. 호스트 응용 프로그램 솔루션에 UWP XAML 사용자 지정 컨트롤에 대 한 소스 코드를 통합 하 고 사용자 지정 컨트롤을 빌드하고 [이 지침](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost#add-a-custom-uwp-control)을 다음 응용 프로그램에서 사용 합니다.

## <a name="how-to-handle-keyboard-focus-navigation"></a>키보드 포커스 탐색을 처리 하는 방법

사용자가 키보드를 사용 하 여 (예를 들어 키를 눌러 **Tab** 또는 화살표 방향을 /) 응용 프로그램의 UI 요소를 통해, **DesktopWindowXamlSource** 개체 들어오고 포커스를 프로그래밍 방식으로 이동 해야 합니다. 사용자의 키보드 탐색은 **DesktopWindowXamlSource**, UI에 대 한 탐색 순서에서 첫 번째 [**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 개체에 포커스를 이동에 도달 하면 다음에 포커스를 이동 하려고 계속 ** Windows.UI.Xaml.UIElement** 개체는 사용자 순환 요소 및 다음 다시 **DesktopWindowXamlSource** 및 부모 UI 요소에 포커스를 이동 합니다.  

UWP XAML API를 호스팅 몇 가지 형식 및 멤버 이러한 작업을 수행 하는 데 제공 합니다.

1. 키보드 탐색에 **DesktopWindowXamlSource**들어가면 [**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) 이벤트는 발생 합니다. 이 이벤트를 처리 하 고 첫 번째 호스팅된 **Windows.UI.Xaml.UIElement** 으로 [**NavigateFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus) 메서드를 사용 하 여 포커스를 프로그래밍 방식으로 이동 합니다.

2. 사용자가 프로그램 **DesktopWindowXamlSource** 의 마지막 포커스 가능 요소에 **Tab** 키 또는 화살표 키를 누를 때 [**TakeFocusRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested) 이벤트는 발생 합니다. 이 이벤트를 처리 하 고 호스트 응용 프로그램에서 다음 포커스 가능 요소로 포커스를 프로그래밍 방식으로 이동 합니다. 예를 들어 **DesktopWindowXamlSource** [**System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)에서 호스팅되는 WPF 응용 프로그램에서 호스트 응용 프로그램에서 다음 포커스 가능 요소로 포커스를 전송할 [**MoveFocus**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) 메서드를 사용할 수 있습니다.

작업 샘플 응용 프로그램의 컨텍스트에서이 작업을 수행 하는 방법을 보여 주는 예제, 다음 코드 파일을 참조 하세요.
  * **WPF:** Windows 커뮤니티 도구 키트에서 [WindowsXamlHostBase.Focus.cs](https://github.com/Microsoft/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) 파일을 참조 하세요.  
  * **Windows Forms:** Windows 커뮤니티 도구 키트에서 [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/Microsoft/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) 파일을 참조 하세요.

## <a name="how-to-handle-layout-changes"></a>레이아웃 변경을 처리 하는 방법

사용자가 부모 UI 요소의 크기를 변경 하는 경우 예상 대로 표시 UWP 컨트롤 하는지 필요한 레이아웃 변경을 처리 해야 합니다. 다음은 몇 가지 중요 한 시나리오를 고려해 야 합니다.

1. **Windows.UI.Xaml.UIElement의 [**측정**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) 메서드를 호출 하는 부모 UI 요소를 표시 하는 **Windows.UI.Xaml.UIElement** **DesktopWindowXamlSource**에서 호스팅하는 데 필요한 사각형 영역의 크기를 가져오려면 해야 하는 경우 **. 예를 들면 다음과 같습니다.
    * WPF 응용 프로그램에서의 호스트 **DesktopWindowXamlSource** [**HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) [**MeasureOverride**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride) 메서드에서이 수행할 수 있습니다.
    * Windows Forms 응용 프로그램에서 **DesktopWindowXamlSource**를 호스트 하는 [**컨트롤**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) 의 [**GetPreferredSize**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize) 메서드에서이 수행할 수 있습니다.

2. 부모 UI 요소 변경, 크기 호출 **Windows.UI.Xaml.UIElement** 루트의 [**Arrange**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) 메서드는 **DesktopWindowXamlSource**에서 호스팅하는 합니다. 예를 들면 다음과 같습니다.
    * WPF 응용 프로그램에서 **DesktopWindowXamlSource**호스팅하는 [**HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) 개체의 [**ArrangeOverride**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride) 메서드에서 이렇게 할 수 있습니다.
    * Windows Forms 응용 프로그램에서 수행할 수 있습니다이 [**컨트롤**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) 의 [**SizeChanged**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged) 이벤트 처리기에서 해당 호스트 **DesktopWindowXamlSource**.

작업 샘플 응용 프로그램의 컨텍스트에서이 작업을 수행 하는 방법을 보여 주는 예제, 다음 코드 파일을 참조 하세요.
  * **WPF:** Windows 커뮤니티 도구 키트에서 [WindowsXamlHost.Layout.cs](https://github.com/Microsoft/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) 파일을 참조 하세요.  
  * **Windows Forms:** Windows 커뮤니티 도구 키트에서 [WindowsXamlHost.Layout.cs](https://github.com/Microsoft/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) 파일을 참조 하세요.

## <a name="how-to-handle-dpi-changes"></a>DPI 변경을 처리 하는 방법

렌더링 변환을 사용 하 여 UWP 컨트롤을 구성 하려면 앱에 DPI 변경을 수신 대기할 해야 호스트 UWP 컨트롤 (예를 들어 사용자가 다양 한 화면 DPI 사용 하 여 모니터 간에 창을) 하는 창에 DPI 변경의 처리 하려는 경우 창 위치를 업데이트 및 DPI 변경에 대 한 응답으로 UWP 컨트롤의 변형 렌더링 합니다.

다음 단계에서는 c + + Win32 응용 프로그램의 컨텍스트에서이 프로세스를 처리 하는 방법을 설명 합니다. 완전 한 예제 github [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32) 샘플에서 [Desktop.cpp](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/Desktop.cpp) 및 [Desktop.h](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/Desktop.h) 코드 파일을 참조 하세요.

1. 앱에서 [**ScaleTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform) 개체를 유지 관리 하 고 UWP 컨트롤의 [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform) 메서드를 할당 합니다. 다음 예제에서는 c + + Win32 응용 프로그램에서 [**Windows.UI.Xaml.Controls.Grid**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) 컨트롤에 대 한이 수행 합니다.

    ```cppwinrt
    // Private fields maintained by your app, such as in a window class you have defined.
    Windows::UI::Xaml::Media::ScaleTransform m_scale;
    Windows::UI::Xaml::Controls::Grid m_rootGrid;

    // Code that runs during initialization, such as the constructor for a window class you have defined.
    m_rootGrid.RenderTransform(m_scale);
    ```

2. [**WindowProc**](https://msdn.microsoft.com/library/windows/desktop/ms633573.aspx) 기능에서 [**WM_DPICHANGED**](https://docs.microsoft.com/windows/desktop/hidpi/wm-dpichanged) 메시지를 기다리고 있습니다. 이 메시지에 대 한 응답 합니다.
    * [**SetWindowPos**](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) 함수를 사용 하 여 메시지를 전달 하는 사각형에 UWP 컨트롤이 포함 된 창 크기를 조정 합니다.
    * 새로운 DPI 값에 따라 **ScaleTransform** 개체의 x 축과 y 배율 인수를 업데이트 합니다.
    * 필요한 대로 모양 및 UWP 컨트롤의 레이아웃을 수정 합니다. 다음 코드 예제는 [**안쪽 여백**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid.padding) DPI 변경에 대 한 응답으로 호스팅된 **Windows.UI.Xaml.Controls.Grid** 컨트롤의 조정합니다.

    ```cppwinrt
    LRESULT HandleDpiChange(HWND hWnd, WPARAM wParam, LPARAM lParam)
    {
        HWND hWndStatic = GetWindow(hWnd, GW_CHILD);
        if (hWndStatic != nullptr)
        {
            UINT uDpi = HIWORD(wParam);

            // Resize the window
            auto lprcNewScale = reinterpret_cast<RECT*>(lParam);

            SetWindowPos(hWnd, nullptr, lprcNewScale->left, lprcNewScale->top,
                lprcNewScale->right - lprcNewScale->left, lprcNewScale->bottom - lprcNewScale->top,
                SWP_NOZORDER | SWP_NOACTIVATE);

            NewScale(uDpi);
          }
          return 0;
    }

    void NewScale(UINT dpi) {

        auto scaleFactor = (float)dpi / 100;

        if (m_scale != nullptr) {
            m_scale.ScaleX(scaleFactor);
            m_scale.ScaleY(scaleFactor);
        }

        ApplyCorrection(scaleFactor);
    }

    void ApplyCorrection(float scaleFactor) {
        float rightCorrection = (m_rootGrid.Width() * scaleFactor - m_rootGrid.Width()) / scaleFactor;
        float bottomCorrection = (m_rootGrid.Height() * scaleFactor - m_rootGrid.Height()) / scaleFactor;

        m_rootGrid.Padding(Windows::UI::Xaml::ThicknessHelper::FromLengths(0, 0, rightCorrection, bottomCorrection));
    }
    ```

2. 모니터 별 DPI 주의 해야 할 응용 프로그램을 구성 하려면 프로젝트에 [어셈블리 나란히 매니페스트](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) 를 추가 하 고 설정 ```<dpiAwareness>``` 요소의 ```PerMonitorV2```합니다. 이 값에 대 한 자세한 내용은 [**DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2**](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context)에 대 한 설명을 참조 하세요.

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

    완전 한 예제-나란히 어셈블리 매니페스트 github [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32) 샘플에서 [XamlIslandsWin32.exe.manifest](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/XamlIslandsWin32.exe.manifest) 파일을 참조 하세요.

## <a name="limitations"></a>제한 사항

XAML 호스팅 API는 Windows 10에 대 한 다른 모든 유형의 XAML 호스트 컨트롤 동일한 제한이 적용을 공유 합니다. 자세한 목록이 [XAML 호스트 컨트롤 한계](xaml-host-controls.md#limitations)를 참조 하세요.

## <a name="troubleshooting"></a>문제 해결

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>UWP XAML UWP 앱에서 호스팅 API를 사용 하 여 오류

| 문제 | 해결 방법 |
|-------|------------|
| 앱에서 다음 메시지와 함께 **COMException** : "DesktopWindowXamlSource 활성화할 수 없습니다. 이 유형을 사용할 수 없습니다 UWP 앱에서. " 또는 "WindowsXamlManager 활성화할 수 없습니다. 이 유형을 사용할 수 없습니다 UWP 앱에서. " | 이 오류는 호스팅 API UWP XAML을 사용 하려는 나타냅니다 (특히 하려는 [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) 또는 [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) 형식은 인스턴스화할) UWP 앱에서. 호스팅 API UWP XAML은 WPF, Windows Forms, 및 c + + Win32 응용 프로그램 등의 비 UWP 데스크톱 앱에서 사용할 에서만 합니다. |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>다른 스레드에서 창에 연결 하는 오류

| 문제 | 해결 방법 |
|-------|------------|
| 앱에서 다음 메시지와 함께 **COMException** : "AttachToWindow 메서드는 지정 된 HWND 다른 스레드에서 만들어졌으므로 못했습니다." | 이 오류는 응용 프로그램이 **IDesktopWindowXamlSourceNative.AttachToWindow** 메서드를 호출 하 고 다른 스레드에서 만든 창의 HWND 전달 나타냅니다. 이 메서드 메서드를 호출 하는 코드와 동일한 스레드에서 만든 창의 HWND를 통과 해야 합니다. |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>다른 최상위 창에서 창에 연결 하는 오류

| 문제 | 해결 방법 |
|-------|------------|
| 앱에서 다음 메시지와 함께 **COMException** : "AttachToWindow 메서드 동일한 스레드에서 AttachToWindow에 이전에 전달 된 HWND 보다 다른 최상위 창에서 지정된 된 HWND 상속 때문에 실패 합니다." | 이 오류는 응용 프로그램이 **IDesktopWindowXamlSourceNative.AttachToWindow** 메서드를 호출 하 고이 메서드에 대 한 이전 호출에 지정 된 창 보다 다른 최상위 창에서 상속 된 창의 HWND 전달를 나타냅니다. 동일한 스레드에서 합니다.</p></p>응용 프로그램 **IDesktopWindowXamlSourceNative.AttachToWindow** 특정 스레드에서 호출 후 동일한 스레드에서 다른 모든 [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) 개체 같은 최상위 창에 아래에 있는 windows에만 연결할 수 있습니다. 첫 번째 **IDesktopWindowXamlSourceNative.AttachToWindow**호출에 전달 된 합니다. 특정 스레드에 대 한 모든 **DesktopWindowXamlSource** 개체 닫히면 다음 **DesktopWindowXamlSource** 창에 다시 연결할 무료 됩니다.</p></p>이 문제를 해결 하려면 중 하나는 **DesktopWindowXamlSource**이 대 한 새 스레드를 만들거나이 스레드에서 다른 최상위 창에 바인딩된 **DesktopWindowXamlSource** 개체를 모두 닫습니다. |

## <a name="related-topics"></a>관련 항목

* [데스크톱 응용 프로그램의 UWP 컨트롤](xaml-host-controls.md)
