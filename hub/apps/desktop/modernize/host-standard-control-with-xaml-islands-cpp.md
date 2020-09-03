---
description: 이 문서에서는 XAML 호스팅 API를 사용하여 C++ Win32 앱에서 표준 UWP 컨트롤을 호스트하는 방법을 보여 줍니다.
title: XAML Islands를 사용하여 C++ Win32 앱에서 표준 UWP 컨트롤 호스트
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10, uwp, cpp, win32, xaml islands, 래핑된 컨트롤, 표준 컨트롤
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 0842046419402bbfacc24331d0521efa9510153a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174197"
---
# <a name="host-a-standard-uwp-control-in-a-c-win32-app"></a>C++ Win32 앱에서 표준 UWP 컨트롤 호스트

이 문서에서는 [UWP XAML 호스팅 API](using-the-xaml-hosting-api.md)를 사용하여 표준 UWP 컨트롤(즉, Windows SDK에서 제공하는 컨트롤)을 새 C++ Win32 앱에 호스트하는 방법을 설명합니다. 이 코드는 [간단한 XAML Island 샘플](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Standalone_Samples/CppWinRT_Basic_Win32App)을 기준으로 하며, 이 섹션에서는 코드의 가장 중요한 부분에 대해 설명합니다. 기존 C++ Win32 앱 프로젝트가 있는 경우 프로젝트에 대해 이러한 단계 및 코드 예제를 적용할 수 있습니다.

> [!NOTE]
> 이 문서에서 설명하는 시나리오는 앱에서 호스트되는 UWP 컨트롤의 XAML 태그를 직접 편집하는 것을 지원하지 않습니다. 이 시나리오에서는 코드를 통해 호스트된 UWP 컨트롤의 모양과 동작을 수정하는 것으로 제한됩니다. UWP 컨트롤을 호스트할 때 XAML 태그를 직접 편집하는 방법에 대한 자세한 내용은 [C++ Win32 앱에서 사용자 지정 UWP 컨트롤 호스트](host-custom-control-with-xaml-islands-cpp.md)를 참조하세요.

## <a name="create-a-desktop-application-project"></a>데스크톱 애플리케이션 프로젝트 만들기

1. Windows 10 버전 1903 SDK(버전 10.0.18362) 이상 릴리스가 설치된 Visual Studio 2019에서 새 **Windows 데스크톱 애플리케이션** 프로젝트를 만들고 이름을 **MyDesktopWin32App**으로 지정합니다. 이 프로젝트 형식은 **C++** , **Windows**및 **데스크톱** 프로젝트 필터에서 사용할 수 있습니다.

2. **솔루션 탐색기**에서 솔루션 노드를 마우스 오른쪽 단추로 클릭하고, **솔루션 대상 변경**을 클릭하고, **10.0.18362.0** 이상의 SDK 릴리스를 선택한 후 **확인**을 클릭합니다.

3. [Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) NuGet 패키지를 설치하여 프로젝트에서 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis)에 대한 지원을 사용하도록 설정합니다.

    1. **솔루션 탐색기**에서 프로젝트를 마우스 오른쪽 단추로 클릭하고 **NuGet 패키지 관리**를 선택합니다.
    2. **찾아보기** 탭을 선택하고 [Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) 패키지를 검색한 후 이 패키지의 최신 버전을 설치합니다.

    > [!NOTE]
    > 새 프로젝트인 경우 [C++/WinRT Visual Studio Extension(VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264)을 설치하고 해당 확장에 포함된 C++/WinRT 프로젝트 템플릿 중 하나를 사용할 수 있습니다. 자세한 내용은 [이 문서](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)를 참조하세요.

4. [Microsoft.Toolkit.Win32.UI.SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) NuGet 패키지를 설치합니다.

    1. **NuGet 패키지 관리자** 창에서 **시험판 포함**이 포함되어 있는지 확인합니다.
    2. **찾아보기** 탭을 선택하고 **Microsoft.Toolkit.Win32.UI.SDK** 패키지를 검색한 후 이 패키지의 버전 v6.0.0 이상 버전을 설치합니다. 이 패키지는 XAML Islands가 앱에서 작동할 수 있도록 하는 몇 가지 빌드 및 런타임 자산을 제공합니다.

5. 애플리케이션이 Windows 10 버전 1903 이상과 호환되도록 지정하려면 [애플리케이션 매니페스트](/windows/desktop/SbsCs/application-manifests)에서 `maxVersionTested` 값을 설정합니다.

    1. 프로젝트에 애플리케이션 매니페스트가 아직 없는 경우 프로젝트에 새 XML 파일을 추가하고 이름을 **app.manifest**로 지정합니다.
    2. 애플리케이션 매니페스트에서 다음 예제에 표시된 **compatibility** 요소와 자식 요소를 포함합니다. **maxVersionTested** 요소의 **Id** 특성을 대상으로 하는 Windows 10의 버전 번호로 바꿉니다(Windows 10, 버전 1903 이상 릴리스여야 함).

        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
            <compatibility xmlns="urn:schemas-microsoft-com:compatibility.v1">
                <application>
                    <!-- Windows 10 -->
                    <maxversiontested Id="10.0.18362.0"/>
                    <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
                </application>
            </compatibility>
        </assembly>
        ```

## <a name="use-the-xaml-hosting-api-to-host-a-uwp-control"></a>XAML 호스팅 API를 사용하여 UWP 컨트롤 호스트

XAML 호스팅 API를 사용하여 UWP 컨트롤을 호스트하는 기본 프로세스는 다음과 같은 일반적인 단계를 따릅니다.

1. 앱이 호스트할 [Windows.UI.Xaml.UIElement](/uwp/api/windows.ui.xaml.uielement) 개체를 만들기 전에 현재 스레드에 대한 UWP XAML 프레임워크를 초기화합니다. 이 작업을 수행하는 방법에는 여러 가지가 있으며, 컨트롤을 호스트하는 [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) 개체를 만들려는 시기에 따라 달라집니다.

    * 애플리케이션에서 호스트할 **Windows.UI.Xaml.UIElement** 개체를 만들기 전에 **DesktopWindowXamlSource** 개체를 만드는 경우 이 프레임워크는 **DesktopWindowXamlSource** 개체를 인스턴스화할 때 초기화됩니다. 이 시나리오에서는 프레임워크를 초기화하는 사용자 고유의 코드를 추가할 필요가 없습니다.

    * 그러나 애플리케이션에서 호스트할 **DesktopWindowXamlSource** 개체를 만들기 전에 **Windows.UI.Xaml.UIElement** 개체를 만드는 경우 애플리케이션에서 정적 [WindowsXamlManager.InitializeForCurrentThread](/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) 메서드를 호출하여 **Windows.UI.Xaml.UIElement** 개체가 인스턴스화되기 전에 UWP XAML 프레임워크를 명시적으로 초기화해야 합니다. 애플리케이션은 일반적으로 **DesktopWindowXamlSource**를 호스트하는 부모 UI 요소가 인스턴스화될 때 이 메서드를 호출해야 합니다.

    > [!NOTE]
    > 이 메서드는 UWP XAML 프레임워크에 대한 참조를 포함하는 [WindowsXamlManager](/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) 개체를 반환합니다. 지정된 스레드에서 원하는 수만큼 **WindowsXamlManager** 개체를 만들 수 있습니다. 그러나 각 개체는 UWP XAML 프레임워크에 대한 참조를 포함하기 때문에 개체를 삭제하여 XAML 리소스가 최종적으로 해제되도록 해야 합니다.

2. [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) 개체를 만들고 창 핸들과 연결된 애플리케이션의 부모 UI 요소에 연결합니다.

    이렇게 하려면 다음 단계를 수행해야 합니다.

    1. **DesktopWindowXamlSource** 개체를 만들고 **IDesktopWindowXamlSourceNative** 또는 **IDesktopWindowXamlSourceNative2** COM 인터페이스로 캐스트합니다.
        > [!NOTE]
        > 이러한 인터페이스는 Windows SDK에서 **windows.ui.xaml.hosting.desktopwindowxamlsource.h** 헤더 파일에 선언됩니다. 기본적으로 이 파일은 %programfiles(x86)%\Windows Kits\10\Include\\<빌드 번호\>\um에 있습니다.

    2. **IDesktopWindowXamlSourceNative** 또는 **IDesktopWindowXamlSourceNative2** 인터페이스의 **AttachToWindow** 메서드를 호출하고 애플리케이션에서 부모 UI 요소의 창 핸들을 전달합니다.

    3. **DesktopWindowXamlSource**에 포함된 내부 자식 창의 초기 크기를 설정합니다. 기본적으로 이 내부 자식 창의 너비와 높이는 0으로 설정되어 있습니다. 창의 크기를 설정하지 않으면 **DesktopWindowXamlSource**에 추가하는 UWP 컨트롤이 표시되지 않습니다. **DesktopWindowXamlSource**의 내부 자식 창에 액세스하려면 **IDesktopWindowXamlSourceNative** 또는 **IDesktopWindowXamlSourceNative2** 인터페이스의 **WindowHandle** 속성을 사용합니다.

3. 마지막으로, 호스트할 **Windows.UI.Xaml.UIElement**을 **DesktopWindowXamlSource** 개체의 [콘텐츠](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) 속성에 할당합니다.

다음 단계 및 코드 예제에서는 위의 프로세스를 구현하는 방법을 보여 줍니다.

1. 프로젝트의 **소스 파일** 폴더에서 기본 **MyDesktopWin32App.cpp** 파일을 엽니다. 파일의 전체 내용을 삭제하고 다음 `include` 및 `using` 문을 추가합니다. 이러한 문에는 표준 C++ 및 UWP 헤더와 네임스페이스 외에도 XAML Islands와 관련된 몇 가지 항목이 포함되어 있습니다.

    ```cppwinrt
    #include <windows.h>
    #include <stdlib.h>
    #include <string.h>

    #include <winrt/Windows.Foundation.Collections.h>
    #include <winrt/Windows.system.h>
    #include <winrt/windows.ui.xaml.hosting.h>
    #include <windows.ui.xaml.hosting.desktopwindowxamlsource.h>
    #include <winrt/windows.ui.xaml.controls.h>
    #include <winrt/Windows.ui.xaml.media.h>

    using namespace winrt;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Xaml::Hosting;
    using namespace Windows::Foundation::Numerics;
    ```

3. 이전 섹션 뒤에 다음 코드를 복사합니다. 이 코드는 앱에 대한 **WinMain** 함수를 정의합니다. 이 함수는 기본 창을 초기화하고 XAML 호스팅 API를 사용하여 창에서 간단한 UWP **TextBlock** 컨트롤을 호스트합니다.

    ```cppwinrt
    LRESULT CALLBACK WindowProc(HWND, UINT, WPARAM, LPARAM);

    HWND _hWnd;
    HWND _childhWnd;
    HINSTANCE _hInstance;

    int CALLBACK WinMain(_In_ HINSTANCE hInstance, _In_opt_ HINSTANCE hPrevInstance, _In_ LPSTR lpCmdLine, _In_ int nCmdShow)
    {
        _hInstance = hInstance;

        // The main window class name.
        const wchar_t szWindowClass[] = L"Win32DesktopApp";
        WNDCLASSEX windowClass = { };

        windowClass.cbSize = sizeof(WNDCLASSEX);
        windowClass.lpfnWndProc = WindowProc;
        windowClass.hInstance = hInstance;
        windowClass.lpszClassName = szWindowClass;
        windowClass.hbrBackground = (HBRUSH)(COLOR_WINDOW + 1);

        windowClass.hIconSm = LoadIcon(windowClass.hInstance, IDI_APPLICATION);

        if (RegisterClassEx(&windowClass) == NULL)
        {
            MessageBox(NULL, L"Windows registration failed!", L"Error", NULL);
            return 0;
        }

        _hWnd = CreateWindow(
            szWindowClass,
            L"Windows c++ Win32 Desktop App",
            WS_OVERLAPPEDWINDOW | WS_VISIBLE,
            CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT,
            NULL,
            NULL,
            hInstance,
            NULL
        );
        if (_hWnd == NULL)
        {
            MessageBox(NULL, L"Call to CreateWindow failed!", L"Error", NULL);
            return 0;
        }


        // Begin XAML Island section.

        // The call to winrt::init_apartment initializes COM; by default, in a multithreaded apartment.
        winrt::init_apartment(apartment_type::single_threaded);

        // Initialize the XAML framework's core window for the current thread.
        WindowsXamlManager winxamlmanager = WindowsXamlManager::InitializeForCurrentThread();

        // This DesktopWindowXamlSource is the object that enables a non-UWP desktop application 
        // to host UWP controls in any UI element that is associated with a window handle (HWND).
        DesktopWindowXamlSource desktopSource;

        // Get handle to the core window.
        auto interop = desktopSource.as<IDesktopWindowXamlSourceNative>();

        // Parent the DesktopWindowXamlSource object to the current window.
        check_hresult(interop->AttachToWindow(_hWnd));

        // This HWND will be the window handler for the XAML Island: A child window that contains XAML.  
        HWND hWndXamlIsland = nullptr;

        // Get the new child window's HWND. 
        interop->get_WindowHandle(&hWndXamlIsland);

        // Update the XAML Island window size because initially it is 0,0.
        SetWindowPos(hWndXamlIsland, 0, 200, 100, 800, 200, SWP_SHOWWINDOW);

        // Create the XAML content.
        Windows::UI::Xaml::Controls::StackPanel xamlContainer;
        xamlContainer.Background(Windows::UI::Xaml::Media::SolidColorBrush{ Windows::UI::Colors::LightGray() });

        Windows::UI::Xaml::Controls::TextBlock tb;
        tb.Text(L"Hello World from Xaml Islands!");
        tb.VerticalAlignment(Windows::UI::Xaml::VerticalAlignment::Center);
        tb.HorizontalAlignment(Windows::UI::Xaml::HorizontalAlignment::Center);
        tb.FontSize(48);

        xamlContainer.Children().Append(tb);
        xamlContainer.UpdateLayout();
        desktopSource.Content(xamlContainer);

        // End XAML Island section.

        ShowWindow(_hWnd, nCmdShow);
        UpdateWindow(_hWnd);

        //Message loop:
        MSG msg = { };
        while (GetMessage(&msg, NULL, 0, 0))
        {
            TranslateMessage(&msg);
            DispatchMessage(&msg);
        }

        return 0;
    }
    ```

4. 이전 섹션 뒤에 다음 코드를 복사합니다. 이 코드는 창에 대한 [창 프로시저](/windows/win32/learnwin32/writing-the-window-procedure)를 정의합니다.

    ```cppwinrt
    LRESULT CALLBACK WindowProc(HWND hWnd, UINT messageCode, WPARAM wParam, LPARAM lParam)
    {
        PAINTSTRUCT ps;
        HDC hdc;
        wchar_t greeting[] = L"Hello World in Win32!";
        RECT rcClient;

        switch (messageCode)
        {
            case WM_PAINT:
                if (hWnd == _hWnd)
                {
                    hdc = BeginPaint(hWnd, &ps);
                    TextOut(hdc, 300, 5, greeting, wcslen(greeting));
                    EndPaint(hWnd, &ps);
                }
                break;
            case WM_DESTROY:
                PostQuitMessage(0);
                break;

            // Create main window
            case WM_CREATE:
                _childhWnd = CreateWindowEx(0, L"ChildWClass", NULL, WS_CHILD | WS_BORDER, 0, 0, 0, 0, hWnd, NULL, _hInstance, NULL);
                return 0;

            // Main window changed size
            case WM_SIZE:
                // Get the dimensions of the main window's client
                // area, and enumerate the child windows. Pass the
                // dimensions to the child windows during enumeration.
                GetClientRect(hWnd, &rcClient);
                MoveWindow(_childhWnd, 200, 200, 400, 500, TRUE);
                ShowWindow(_childhWnd, SW_SHOW);

                return 0;

                // Process other messages.

            default:
                return DefWindowProc(hWnd, messageCode, wParam, lParam);
                break;
        }

        return 0;
    }
    ```

5. 코드 파일을 저장하고 앱을 빌드한 후 실행합니다. 앱 창에서 UWP **TextBlock** 컨트롤이 표시되는지 확인합니다.
    > [!NOTE]
    > `warning C4002:  too many arguments for function-like macro invocation 'GetCurrentTime'` 및 `manifest authoring warning 81010002: Unrecognized Element "maxversiontested" in namespace "urn:schemas-microsoft-com:compatibility.v1"`을 비롯한 몇 가지 빌드 경고가 표시될 수 있습니다. 이러한 경고는 현재 도구 및 NuGet 패키지와 관련된 알려진 문제로, 무시해도 됩니다.

이러한 작업을 보여 주는 전체 예제는 다음 코드 파일을 참조하세요.

* **C++ Win32:**
  * [HelloWindowsDesktop](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Standalone_Samples/CppWinRT_Basic_Win32App/Win32DesktopApp/HelloWindowsDesktop.cpp) 파일을 참조하세요.
  * [XamlBridge](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp) 파일을 참조하세요.
* **WPF:** Windows 커뮤니티 도구 키트의 [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs) 및 [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) 파일을 참조하세요.  
* **Windows Forms:** Windows 커뮤니티 도구 키트의 [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs) 및 [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) 파일을 참조하세요.

## <a name="package-the-app"></a>앱 패키지

필요에 따라 배포를 위해 [MSIX 패키지](/windows/msix)에 앱을 패키지할 수 있습니다. MSIX는 Windows용 최신 앱 패키징 기술로, MSI, .appx, App-V 및 ClickOnce 설치 기술의 조합을 기준으로 합니다.

다음 지침에서는 Visual Studio 2019의 [Windows 애플리케이션 패키징 프로젝트](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)를 사용하여 솔루션에 있는 모든 구성 요소를 MSIX 패키지에 패키지하는 방법을 보여 줍니다. 이러한 단계는 MSIX 패키지에서 앱을 패키지하는 경우에만 필요합니다.

> [!NOTE]
> 배포를 위해 [MSIX 패키지](/windows/msix)에 애플리케이션을 패키지하지 않도록 선택하는 경우 앱을 실행하는 컴퓨터에 [Visual C++ Runtime](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads)이 설치되어 있어야 합니다.

1. 새 [Windows 애플리케이션 패키징 프로젝트](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)를 솔루션에 추가합니다. 프로젝트를 만들 때 **대상 버전**과 **최소 버전**을 모두**Windows 10 버전 1903(10.0; 빌드 18362)** 으로 선택합니다.

2. 패키징 프로젝트에서 **애플리케이션** 노드를 마우스 오른쪽 단추로 클릭하고 **참조 추가**를 선택합니다. 프로젝트 목록에서 솔루션의 C++/Win32 데스크톱 애플리케이션 프로젝트를 선택하고 **확인**을 클릭합니다.

3. 패키징 프로젝트를 빌드하고 실행합니다. 앱이 실행되고 UWP 컨트롤이 예상대로 표시되는지 확인합니다.

## <a name="next-steps"></a>다음 단계

이 문서의 코드 예제에서는 C++ Win32 앱에서 표준 UWP 컨트롤을 호스트하는 기본적인 시나리오를 시작합니다. 다음 섹션에서는 애플리케이션에서 지원해야 하는 추가 시나리오를 소개합니다.

### <a name="host-a-custom-uwp-control"></a>사용자 지정 UWP 컨트롤 호스트

여러 시나리오에서 함께 작동하는 여러 개별 컨트롤이 포함된 사용자 지정 UWP XAML 컨트롤을 호스트해야 할 수 있습니다. C++ Win32 앱에 사용자 지정 UWP 컨트롤(사용자가 직접 정의하는 컨트롤 또는 타사에서 제공하는 컨트롤)을 호스트하는 프로세스는 표준 컨트롤을 호스트하는 것보다 복잡하며 추가 코드가 필요합니다.

전체 연습에 대해서는 [XAML 호스팅 API를 사용하여 C++ Win32 앱에서 사용자 지정 UWP 컨트롤 호스트](host-custom-control-with-xaml-islands-cpp.md)를 참조하세요.

### <a name="advanced-scenarios"></a>고급 시나리오

XAML Islands를 호스트하는 많은 데스크톱 애플리케이션은 원활한 사용자 환경을 제공하기 위해 추가 시나리오를 처리해야 합니다. 예를 들어, 데스크톱 애플리케이션은 XAML Islands의 키보드 입력, XAML Islands와 기타 UI 요소 사이에 포커스 탐색 및 레이아웃 변경 내용을 처리해야 할 수 있습니다.

이러한 시나리오 및 관련 코드 샘플에 대한 포인터를 처리하는 방법에 대한 자세한 내용은 [C++ Win32 앱의 XAML Islands에 대한 고급 시나리오](advanced-scenarios-xaml-islands-cpp.md)를 참조하세요.

## <a name="related-topics"></a>관련 항목

* [데스크톱 앱에서 UWP XAML 컨트롤 호스트(XAML Islands)](xaml-islands.md)
* [C++ Win32 앱에서 UWP XAML 호스팅 API 사용](using-the-xaml-hosting-api.md)
* [C++ Win32 앱에서 사용자 지정 UWP 컨트롤 호스트](host-custom-control-with-xaml-islands-cpp.md)
* [C++ Win32 앱의 XAML Islands에 대한 고급 시나리오](advanced-scenarios-xaml-islands-cpp.md)
* [XAML Islands 코드 샘플](https://github.com/microsoft/Xaml-Islands-Samples)