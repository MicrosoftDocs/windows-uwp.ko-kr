---
title: Win32에서 시각적 계층 사용
description: 시각적 계층을 사용하여 Win32 데스크톱 앱의 UI를 향상시킵니다.
template: detail.hbs
ms.date: 03/18/2019
ms.topic: article
keywords: UWP, 렌더링, 컴퍼지션, win32
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: c9b4ec38b0dd1f6eca3f43cfded74c6292c08100
ms.sourcegitcommit: d1c3e13de3da3f7dce878b3735ee53765d0df240
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66215192"
---
# <a name="using-the-visual-layer-with-win32"></a>Win32에서 시각적 계층 사용

Win32 앱에서 Windows 런타임 컴퍼지션 API([시각적 계층](/windows/uwp/composition/visual-layer)이라고도 함)를 사용하여 Windows 10 사용자를 위한 최신 환경을 만들 수 있습니다.

이 자습서의 전체 코드는 다음 GitHub에서 사용할 수 있습니다. [Win32 HelloComposition 샘플](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)

Ui 컴퍼지션을 정확하게 제어해야 하는 유니버설 Windows 애플리케이션은 UI를 구성하고 렌더링하는 방법을 세밀하게 제어하기 위해 [Windows.UI.Composition](/uwp/api/windows.ui.composition) 네임스페이스에 액세스합니다. 그러나 이 컴퍼지션 API는 UWP 앱으로 제한되지 않습니다. Win32 데스크톱 애플리케이션은 UWP 및 Windows 10의 최신 컴퍼지션 시스템을 활용할 수 있습니다.

## <a name="prerequisites"></a>필수 구성 요소

UWP 호스팅 API에는 다음과 같은 필수 구성 요소가 있습니다.

- 사용자가 Win32 및 UWP를 사용하는 앱 개발에 대해 잘 알고 있다고 가정합니다. 자세한 내용은 다음의 정보를 참조하세요.
  - [Win32 및 C++ 시작](/windows/desktop/learnwin32/learn-to-program-for-windows)
  - [Windows 10 앱 시작](/windows/uwp/get-started/)
  - [Windows 10용 데스크톱 애플리케이션 개선](/windows/uwp/porting/desktop-to-uwp-enhance)
- Windows 10 버전 1803 이상
- Windows 10 SDK 17134 이상

## <a name="how-to-use-composition-apis-from-a-win32-desktop-application"></a>Win32 데스크톱 애플리케이션에서 컴퍼지션 API를 사용하는 방법

이 자습서에서는 간단한 Win32 C++ 앱을 만들고 UWP 컴퍼지션 요소를 추가합니다. 여기서는 프로젝트를 올바르게 구성하고, interop 코드를 만들고, Windows 컴퍼지션 API를 사용하여 간단한 그리기를 수행하는 방법을 중점적으로 다룹니다. 완성된 앱은 다음과 같습니다.

![실행 중인 앱 UI](images/visual-layer-interop/win32-comp-interop-app-ui.png)

## <a name="create-a-c-win32-project-in-visual-studio"></a>Visual Studio에서 C++ Win32 프로젝트 만들기

첫 번째 단계는 Visual Studio에서 Win32 앱 프로젝트를 만드는 것입니다.

_HelloComposition_이라는 새 Win32 애플리케이션 프로젝트를 C++에서 만들려면 다음을 수행합니다.

1. Visual Studio를 열고 **파일** > **새로 만들기** > **프로젝트**를 선택합니다.

    **새 프로젝트** 대화 상자가 열립니다.
1. **설치됨** 범주 아래에서 **Visual C++** 노드를 확장하고 **Windows 데스크톱**을 선택합니다.
1. **Windows 데스크톱 애플리케이션** 템플릿을 선택합니다.
1. 이름 _HelloComposition_을 입력한 다음, **확인**를 클릭합니다.

    Visual Studio에서 프로젝트를 만들고 주 앱 파일에 대한 편집기를 엽니다.

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Windows 런타임 API를 사용하도록 프로젝트 구성

Win32 앱에서 WinRT(Windows 런타임) API를 사용하려면 C++/WinRT를 사용합니다. C++/WinRT 지원을 추가하도록 Visual Studio 프로젝트를 구성해야 합니다.

(자세한 내용은 [C++/WinRT 시작 - Windows 데스크톱 애플리케이션 프로젝트를 수정하여 C++/WinRT 지원 추가](/windows/uwp/cpp-and-winrt-apis/get-started.md#modify-a-windows-desktop-application-project-to-add-cwinrt-support)를 참조하세요.)

1. **프로젝트** 메뉴에서 프로젝트 속성(_HelloComposition 속성_)을 열고 다음 설정이 지정된 값으로 설정되었는지 확인합니다.

    - **구성**에 대해 _모든 구성_을 선택합니다. **플랫폼**에 대해 _모든 플랫폼_을 선택합니다.
    - **구성 속성** > **일반** > **Windows SDK 버전** = _10.0.17763.0_ 이상

    ![SDK 버전 설정](images/visual-layer-interop/sdk-version.png)

    - **C/C++**  > **언어** > **C++ 언어 표준** = _ISO C++ 17 표준(/stf:c++17)_

    ![언어 표준 설정](images/visual-layer-interop/language-standard.png)

    - **링커** > **입력** > **추가 종속성**에는 "_windowsapp.lib_"가 포함되어야 합니다. 목록에 포함되지 않은 경우 추가합니다.

    ![링커 종속성 추가](images/visual-layer-interop/linker-dependencies.png)

1. 미리 컴파일된 헤더 업데이트

    - `stdafx.h`와 `stdafx.cpp`를 각각 `pch.h` 및 `pch.cpp`로 바꿉니다.
    - 프로젝트 속성 **C/C++**  > **미리 컴파일된 헤더** > **미리 컴파일된 헤더 파일**을 *pch.h*로 설정합니다.
    - 모든 파일에서 `#include "stdafx.h"`를 찾아서 `#include "pch.h"`로 바꿉니다.

        (**편집** > **찾기 및 바꾸기** > **파일에서 찾기**)

        ![stdafx.h 찾기 및 바꾸기](images/visual-layer-interop/replace-stdafx.png)

    - `pch.h`에 `winrt/base.h` 및 `unknwn.h`를 포함합니다.

        ```cppwinrt
        // reference additional headers your program requires here
        #include <unknwn.h>
        #include <winrt/base.h>
        ```

이 시점에서 계속 진행하기 전에 프로젝트를 빌드하여 오류가 없는지 확인하는 것이 좋습니다.

## <a name="create-a-class-to-host-composition-elements"></a>컴퍼지션 요소를 호스트하는 클래스 만들기

시각적 계층으로 만든 콘텐츠를 호스트하려면 interop을 관리하고 컴퍼지션 요소를 만드는 클래스(_CompositionHost_)를 만듭니다. 여기서 다음을 포함하여 컴퍼지션 API 호스트를 위한 대부분의 구성을 수행합니다.

- [Compositor](/uwp/api/windows.ui.composition.compositor)를 가져와 [Windows.UI.Composition](/uwp/api/windows.ui.composition) 네임스페이스에서 개체를 만들고 관리합니다.
- [DispatcherQueueController](/uwp/api/windows.system.dispatcherqueuecontroller)/[DispatcherQueue](/uwp/api/windows.system.dispatcherqueue)를 만들어 WinRT API에 대한 작업을 관리합니다.
- [DesktopWindowTarget](/uwp/api/windows.ui.composition.desktop.desktopwindowtarget) 및 컴퍼지션 컨테이너를 만들어 컴퍼지션 개체를 표시합니다.

스레딩 문제를 방지하기 위해 이 클래스를 singleton으로 만듭니다. 예를 들어, 스레드별로 디스패처 큐를 하나만 만들 수 있으므로 동일한 스레드에서 CompositionHost의 두 번째 인스턴스를 인스턴스화하면 오류가 발생합니다.

> [!TIP]
> 필요한 경우 자습서의 끝 부분에 있는 전체 코드를 확인하여 자습서의 작업을 진행할 때 모든 코드가 올바른 위치에 있도록 합니다.

1. 새 클래스 파일을 프로젝트에 추가합니다.
    - **솔루션 탐색기**에서 _HelloComposition_ 프로젝트를 마우스 오른쪽 단추로 클릭합니다.
    - 상황에 맞는 메뉴에서 **추가** > **클래스...** 를 선택합니다.
    - **새 클래스** 대화 상자에서 클래스 이름을 _CompositionHost.cs_로 지정한 다음, **추가**를 클릭합니다.

1. 컴퍼지션 interop에 필요한 헤더 및 _using_을 포함합니다.
    - CompositionHost.h 파일 맨 위에 이러한 _include_를 추가합니다.

    ```cppwinrt
    #pragma once
    #include <winrt/Windows.UI.Composition.Desktop.h>
    #include <windows.ui.composition.interop.h>
    #include <DispatcherQueue.h>
    ```

    - CompositionHost.cpp 파일 맨 위, _include_ 다음에 이러한 _using_을 추가합니다.

    ```cppwinrt
    using namespace winrt;
    using namespace Windows::System;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Composition::Desktop;
    using namespace Windows::Foundation::Numerics;
    ```

1. singleton 패턴을 사용하도록 클래스를 편집합니다.
    - CompositionHost.h에서 생성자를 private으로 설정합니다.
    - 공용 정적 _GetInstance_ 메서드를 선언합니다.

    ```cppwinrt
    class CompositionHost
    {
    public:
        ~CompositionHost();
        static CompositionHost* GetInstance();

    private:
        CompositionHost();
    };
    ```

    - CompositionHost.cpp에서 _GetInstance_ 메서드의 정의를 추가합니다.

    ```cppwinrt
    CompositionHost* CompositionHost::GetInstance()
    {
        static CompositionHost instance;
        return &instance;
    }
    ```

1. CompositionHost.h에서 Compositor, DispatcherQueueController 및 DesktopWindowTarget에 대한 프라이빗 멤버 변수를 선언합니다.

    ```cppwinrt
    winrt::Windows::UI::Composition::Compositor m_compositor{ nullptr };
    winrt::Windows::System::DispatcherQueueController m_dispatcherQueueController{ nullptr };
    winrt::Windows::UI::Composition::Desktop::DesktopWindowTarget m_target{ nullptr };
    ```

1. 퍼블릭 메서드를 추가하여 컴퍼지션 interop 개체를 초기화합니다.
    > [!NOTE]
    > _Initialize_에서 _EnsureDispatcherQueue_, _CreateDesktopWindowTarget_ 및 _CreateCompositionRoot_ 메서드를 호출합니다. 다음 단계에서 이러한 메서드를 만듭니다.

    - CompositionHost.h에서 HWND를 인수로 사용하는 _Initialize_라는 퍼블릭 메서드를 선언합니다.

    ```cppwinrt
    void Initialize(HWND hwnd);
    ```

    - CompositionHost.cpp에서 _Initialize_ 메서드의 정의를 추가합니다.

    ```cppwinrt
    void CompositionHost::Initialize(HWND hwnd)
    {
        EnsureDispatcherQueue();
        if (m_dispatcherQueueController) m_compositor = Compositor();

        CreateDesktopWindowTarget(hwnd);
        CreateCompositionRoot();
    }
    ```

1. Windows 컴퍼지션을 사용하는 스레드에 디스패처 큐를 만듭니다.

    Compositor는 디스패처 큐가 있는 스레드에 만들어야 하므로 초기화하는 동안 이 메서드가 먼저 호출됩니다.

    - CompositionHost.h에서 _EnsureDispatcherQueue_라는 프라이빗 메서드를 선언합니다.

    ```cppwinrt
    void EnsureDispatcherQueue();
    ```

    - CompositionHost.cpp에서 _EnsureDispatcherQueue_ 메서드의 정의를 추가합니다.

    ```cppwinrt
    void CompositionHost::EnsureDispatcherQueue()
    {
        namespace abi = ABI::Windows::System;

        if (m_dispatcherQueueController == nullptr)
        {
            DispatcherQueueOptions options
            {
                sizeof(DispatcherQueueOptions), /* dwSize */
                DQTYPE_THREAD_CURRENT,          /* threadType */
                DQTAT_COM_ASTA                  /* apartmentType */
            };

            Windows::System::DispatcherQueueController controller{ nullptr };
            check_hresult(CreateDispatcherQueueController(options, reinterpret_cast<abi::IDispatcherQueueController**>(put_abi(controller))));
            m_dispatcherQueueController = controller;
        }
    }
    ```

1. 앱의 창을 컴퍼지션 대상으로 등록합니다.
    - CompositionHost.h에서 HWND를 인수로 사용하는 _CreateDesktopWindowTarget_라는 프라이빗 메서드를 선언합니다.

    ```cppwinrt
    void CreateDesktopWindowTarget(HWND window);
    ```

    - CompositionHost.cpp에서 _CreateDesktopWindowTarget_ 메서드의 정의를 추가합니다.

    ```cppwinrt
    void CompositionHost::CreateDesktopWindowTarget(HWND window)
    {
        namespace abi = ABI::Windows::UI::Composition::Desktop;

        auto interop = m_compositor.as<abi::ICompositorDesktopInterop>();
        DesktopWindowTarget target{ nullptr };
        check_hresult(interop->CreateDesktopWindowTarget(window, false, reinterpret_cast<abi::IDesktopWindowTarget**>(put_abi(target))));
        m_target = target;
    }
    ```

1. 시각적 개체를 저장할 루트 시각적 컨테이너를 만듭니다.
    - CompositionHost.h에서 _CreateCompositionRoot_라는 프라이빗 메서드를 선언합니다.

    ```cppwinrt
    void CreateCompositionRoot();
    ```

    - CompositionHost.cpp에서 _CreateCompositionRoot_ 메서드의 정의를 추가합니다.

    ```cppwinrt
    void CompositionHost::CreateCompositionRoot()
    {
        auto root = m_compositor.CreateContainerVisual();
        root.RelativeSizeAdjustment({ 1.0f, 1.0f });
        root.Offset({ 124, 12, 0 });
        m_target.Root(root);
    }
    ```

지금 프로젝트를 빌드하여 오류가 없는지 확인합니다.

이러한 메서드는 UWP 시각적 계층 및 Win32 API 간의 interop에 필요한 구성 요소를 설정합니다. 이제 앱에 콘텐츠를 추가할 수 있습니다.

### <a name="add-composition-elements"></a>컴퍼지션 요소 추가

인프라가 준비되었으므로 이제 표시하려는 컴퍼지션 콘텐츠를 생성할 수 있습니다.

이 예제에서는 짧은 지연 후에 삭제되도록 하는 애니메이션을 사용하여 임의 색상의 사각형 [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual)을 만드는 코드를 추가합니다.

1. 컴퍼지션 요소를 추가합니다.
    - CompositionHost.h에서 3개의 **float** 값을 인수로 사용하는 _AddElement_라는 퍼블릭 메서드를 선언합니다.

    ```cppwinrt
    void AddElement(float size, float x, float y);
    ```

    - CompositionHost.cpp에서 _AddElement_ 메서드의 정의를 추가합니다.

    ```cppwinrt
    void CompositionHost::AddElement(float size, float x, float y)
    {
        if (m_target.Root())
        {
            auto visuals = m_target.Root().as<ContainerVisual>().Children();
            auto visual = m_compositor.CreateSpriteVisual();

            auto element = m_compositor.CreateSpriteVisual();
            uint8_t r = (double)(double)(rand() % 255);;
            uint8_t g = (double)(double)(rand() % 255);;
            uint8_t b = (double)(double)(rand() % 255);;

            element.Brush(m_compositor.CreateColorBrush({ 255, r, g, b }));
            element.Size({ size, size });
            element.Offset({ x, y, 0.0f, });

            auto animation = m_compositor.CreateVector3KeyFrameAnimation();
            auto bottom = (float)600 - element.Size().y;
            animation.InsertKeyFrame(1, { element.Offset().x, bottom, 0 });

            using timeSpan = std::chrono::duration<int, std::ratio<1, 1>>;

            std::chrono::seconds duration(2);
            std::chrono::seconds delay(3);

            animation.Duration(timeSpan(duration));
            animation.DelayTime(timeSpan(delay));
            element.StartAnimation(L"Offset", animation);
            visuals.InsertAtTop(element);

            visuals.InsertAtTop(visual);
        }
    }
    ```

## <a name="create-and-show-the-window"></a>창 만들기 및 표시

이제 Win32 UI에 단추 및 UWP 컴퍼지션 콘텐츠를 추가할 수 있습니다.

1. HelloComposition.cpp 맨 위에 _CompositionHost.h_를 포함하고, BTN_ADD를 정의하고, CompositionHost의 인스턴스를 가져옵니다.

    ```cppwinrt
    #include "CompositionHost.h"

    // #define MAX_LOADSTRING 100 // This is already in the file.
    #define BTN_ADD 1000

    CompositionHost* compHost = CompositionHost::GetInstance();
    ```

1. `InitInstance` 메서드에서 만든 창의 크기를 변경합니다. (이 줄에서 `CW_USEDEFAULT, 0`을 `900, 672`로 변경합니다.)

    ```cppwinrt
    HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
        CW_USEDEFAULT, 0, 900, 672, nullptr, nullptr, hInstance, nullptr);
    ```

1. WndProc 함수에서 _message_ 스위치 블록에 `case WM_CREATE`를 추가합니다. 이 경우 CompositionHost를 초기화하고 단추를 만듭니다.

    ```cppwinrt
    case WM_CREATE:
    {
        compHost->Initialize(hWnd);
        srand(time(nullptr));

        CreateWindow(TEXT("button"), TEXT("Add element"),
            WS_VISIBLE | WS_CHILD | BS_PUSHBUTTON,
            12, 12, 100, 50,
            hWnd, (HMENU)BTN_ADD, nullptr, nullptr);
    }
    break;
    ```

1. 또한 WndProc 함수에서 단추 클릭을 처리하여 UI에 컴퍼지션 요소를 추가합니다. 

    WM_COMMAND 블록 내부의 _wmId_ 스위치 블록에 `case BTN_ADD`를 추가합니다.

    ```cppwinrt
    case BTN_ADD: // addButton click
    {
        double size = (double)(rand() % 150 + 50);
        double x = (double)(rand() % 600);
        double y = (double)(rand() % 200);
        compHost->AddElement(size, x, y);
        break;
    }
    ```

이제 앱을 빌드하고 실행합니다. 필요한 경우 자습서의 끝 부분에 있는 전체 코드를 확인하여 모든 코드가 올바른 위치에 있도록 합니다.

앱을 실행하고 단추를 클릭하면 UI에 애니메이션 효과가 있는 사각형이 추가된 것을 볼 수 있습니다.

## <a name="additional-resources"></a>추가 리소스

- [Win32 HelloComposition 샘플(GitHub)](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)
- [Win32 및 C++ 시작](/windows/desktop/learnwin32/learn-to-program-for-windows)
- [Windows 10 앱 시작](/windows/uwp/get-started/)(UWP)
- [Windows 10용 데스크톱 애플리케이션 개선](/windows/uwp/porting/desktop-to-uwp-enhance)(UWP)
- [Windows.UI.Composition 네임스페이스](/uwp/api/windows.ui.composition)(UWP)

## <a name="complete-code"></a>전체 코드

CompositionHost 클래스 및 InitInstance 메서드의 전체 코드는 다음과 같습니다.

### <a name="compositionhosth"></a>CompositionHost.h

```cppwinrt
#pragma once
#include <winrt/Windows.UI.Composition.Desktop.h>
#include <windows.ui.composition.interop.h>
#include <DispatcherQueue.h>

class CompositionHost
{
public:
    ~CompositionHost();
    static CompositionHost* GetInstance();

    void Initialize(HWND hwnd);
    void AddElement(float size, float x, float y);

private:
    CompositionHost();

    void CreateDesktopWindowTarget(HWND window);
    void EnsureDispatcherQueue();
    void CreateCompositionRoot();

    winrt::Windows::UI::Composition::Compositor m_compositor{ nullptr };
    winrt::Windows::UI::Composition::Desktop::DesktopWindowTarget m_target{ nullptr };
    winrt::Windows::System::DispatcherQueueController m_dispatcherQueueController{ nullptr };
};
```

### <a name="compositionhostcpp"></a>CompositionHost.cpp

```cppwinrt
#include "pch.h"
#include "CompositionHost.h"

using namespace winrt;
using namespace Windows::System;
using namespace Windows::UI;
using namespace Windows::UI::Composition;
using namespace Windows::UI::Composition::Desktop;
using namespace Windows::Foundation::Numerics;

CompositionHost::CompositionHost()
{
}

CompositionHost* CompositionHost::GetInstance()
{
    static CompositionHost instance;
    return &instance;
}

CompositionHost::~CompositionHost()
{
}

void CompositionHost::Initialize(HWND hwnd)
{
    EnsureDispatcherQueue();
    if (m_dispatcherQueueController) m_compositor = Compositor();

    if (m_compositor)
    {
        CreateDesktopWindowTarget(hwnd);
        CreateCompositionRoot();
    }
}

void CompositionHost::EnsureDispatcherQueue()
{
    namespace abi = ABI::Windows::System;

    if (m_dispatcherQueueController == nullptr)
    {
        DispatcherQueueOptions options
        {
            sizeof(DispatcherQueueOptions), /* dwSize */
            DQTYPE_THREAD_CURRENT,          /* threadType */
            DQTAT_COM_ASTA                  /* apartmentType */
        };

        Windows::System::DispatcherQueueController controller{ nullptr };
        check_hresult(CreateDispatcherQueueController(options, reinterpret_cast<abi::IDispatcherQueueController**>(put_abi(controller))));
        m_dispatcherQueueController = controller;
    }
}

void CompositionHost::CreateDesktopWindowTarget(HWND window)
{
    namespace abi = ABI::Windows::UI::Composition::Desktop;

    auto interop = m_compositor.as<abi::ICompositorDesktopInterop>();
    DesktopWindowTarget target{ nullptr };
    check_hresult(interop->CreateDesktopWindowTarget(window, false, reinterpret_cast<abi::IDesktopWindowTarget**>(put_abi(target))));
    m_target = target;
}

void CompositionHost::CreateCompositionRoot()
{
    auto root = m_compositor.CreateContainerVisual();
    root.RelativeSizeAdjustment({ 1.0f, 1.0f });
    root.Offset({ 124, 12, 0 });
    m_target.Root(root);
}

void CompositionHost::AddElement(float size, float x, float y)
{
    if (m_target.Root())
    {
        auto visuals = m_target.Root().as<ContainerVisual>().Children();
        auto visual = m_compositor.CreateSpriteVisual();

        auto element = m_compositor.CreateSpriteVisual();
        uint8_t r = (double)(double)(rand() % 255);;
        uint8_t g = (double)(double)(rand() % 255);;
        uint8_t b = (double)(double)(rand() % 255);;

        element.Brush(m_compositor.CreateColorBrush({ 255, r, g, b }));
        element.Size({ size, size });
        element.Offset({ x, y, 0.0f, });

        auto animation = m_compositor.CreateVector3KeyFrameAnimation();
        auto bottom = (float)600 - element.Size().y;
        animation.InsertKeyFrame(1, { element.Offset().x, bottom, 0 });

        using timeSpan = std::chrono::duration<int, std::ratio<1, 1>>;

        std::chrono::seconds duration(2);
        std::chrono::seconds delay(3);

        animation.Duration(timeSpan(duration));
        animation.DelayTime(timeSpan(delay));
        element.StartAnimation(L"Offset", animation);
        visuals.InsertAtTop(element);

        visuals.InsertAtTop(visual);
    }
}
```

### <a name="hellocompositioncpp-partial"></a>HelloComposition.cpp(부분)

```cppwinrt
#include "pch.h"
#include "HelloComposition.h"
#include "CompositionHost.h"

#define MAX_LOADSTRING 100
#define BTN_ADD 1000

CompositionHost* compHost = CompositionHost::GetInstance();

// Global Variables:

// ...
// ... code not shown ...
// ...

BOOL InitInstance(HINSTANCE hInstance, int nCmdShow)
{
   hInst = hInstance; // Store instance handle in our global variable

   HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
      CW_USEDEFAULT, 0, 900, 672, nullptr, nullptr, hInstance, nullptr);

// ...
// ... code not shown ...
// ...
}

// ...

LRESULT CALLBACK WndProc(HWND hWnd, UINT message, WPARAM wParam, LPARAM lParam)
{
    switch (message)
    {
// Add this...
    case WM_CREATE:
    {
        compHost->Initialize(hWnd);
        srand(time(nullptr));

        CreateWindow(TEXT("button"), TEXT("Add element"),
            WS_VISIBLE | WS_CHILD | BS_PUSHBUTTON,
            12, 12, 100, 50,
            hWnd, (HMENU)BTN_ADD, nullptr, nullptr);
    }
    break;
// ...
    case WM_COMMAND:
    {
        int wmId = LOWORD(wParam);
        // Parse the menu selections:
        switch (wmId)
        {
        case IDM_ABOUT:
            DialogBox(hInst, MAKEINTRESOURCE(IDD_ABOUTBOX), hWnd, About);
            break;
        case IDM_EXIT:
            DestroyWindow(hWnd);
            break;
// Add this...
        case BTN_ADD: // addButton click
        {
            double size = (double)(rand() % 150 + 50);
            double x = (double)(rand() % 600);
            double y = (double)(rand() % 200);
            compHost->AddElement(size, x, y);
            break;
        }
// ...
        default:
            return DefWindowProc(hWnd, message, wParam, lParam);
        }
    }
    break;
    case WM_PAINT:
    {
        PAINTSTRUCT ps;
        HDC hdc = BeginPaint(hWnd, &ps);
        // TODO: Add any drawing code that uses hdc here...
        EndPaint(hWnd, &ps);
    }
    break;
    case WM_DESTROY:
        PostQuitMessage(0);
        break;
    default:
        return DefWindowProc(hWnd, message, wParam, lParam);
    }
    return 0;
}

// ...
// ... code not shown ...
// ...
```