---
title: 시각적 계층을 사용 하 여 Win32를 사용 하 여
description: Win32 데스크톱 앱의 UI를 향상 시키기 위해 시각적 계층을 사용 합니다.
template: detail.hbs
ms.date: 03/18/2019
ms.topic: article
keywords: UWP, 렌더링, 컴퍼지션, win32
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: c9b4ec38b0dd1f6eca3f43cfded74c6292c08100
ms.sourcegitcommit: d1c3e13de3da3f7dce878b3735ee53765d0df240
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66215192"
---
# <a name="using-the-visual-layer-with-win32"></a>시각적 계층을 사용 하 여 Win32를 사용 하 여

Windows 런타임 구성 Api를 사용할 수 있습니다 (라고도 합니다 [시각적 계층](/windows/uwp/composition/visual-layer))는 Windows 10 사용자에 대해 간단한 최신 환경을 만들기 위해 Win32 앱에서.

이 자습서에 대 한 전체 코드는 GitHub에서 사용할 수 있습니다. [Win32 HelloComposition 샘플](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)합니다.

해당 UI 컴퍼지션 정밀 하 게 제어 해야 하는 유니버설 Windows 응용 프로그램에 액세스할 수 합니다 [Windows.UI.Composition](/uwp/api/windows.ui.composition) 네임 스페이스를 미세 조정 된 해당 UI는 구성 하 고 렌더링 하는 방법을 제어 합니다. 그러나이 컴퍼지션 API는 UWP 앱에 제한이 없습니다. Win32 데스크톱 응용 프로그램의 UWP 및 Windows 10의 최신 컴퍼지션 시스템을 사용할 수 있습니다.

## <a name="prerequisites"></a>사전 요구 사항

호스팅 API는 UWP 이러한 필수 조건이 필요 합니다.

- Win32와 UWP를 사용 하 여 앱 개발 경험이 있다고 가정 합니다. 자세한 내용은 다음의 정보를 참조하세요.
  - [Win32를 사용 하 여 시작 하 고C++](/windows/desktop/learnwin32/learn-to-program-for-windows)
  - [Windows 10 앱 시작](/windows/uwp/get-started/)
  - [Windows 10 용 데스크톱 응용 프로그램을 향상합니다](/windows/uwp/porting/desktop-to-uwp-enhance)
- Windows 10 버전 1803 이상이
- Windows 10 SDK 17134 이상

## <a name="how-to-use-composition-apis-from-a-win32-desktop-application"></a>Win32 데스크톱 응용 프로그램에서 컴퍼지션 Api를 사용 하는 방법

간단한 Win32를 만들면이 자습서에서는 C++ 앱을 UWP 구성 요소를 추가 합니다. 포커스를 올바르게 프로젝트 구성, interop 코드를 만들고 Windows 컴퍼지션 Api를 사용 하 여 간단한 것 그리기 켜져 있습니다. 완성된 된 앱을 다음과 같이 표시 됩니다.

![실행 중인 앱 UI](images/visual-layer-interop/win32-comp-interop-app-ui.png)

## <a name="create-a-c-win32-project-in-visual-studio"></a>만들기는 C++ Visual Studio에서 Win32 프로젝트

먼저 Visual Studio에서 Win32 응용 프로그램 프로젝트를 만드는 것입니다.

새 Win32 응용 프로그램 프로젝트를 만들려면 C++ 이라는 _HelloComposition_:

1. Visual Studio를 열고 선택 **파일** > **새로 만들기** > **프로젝트**합니다.

    합니다 **새 프로젝트** 대화 상자가 열립니다.
1. 아래는 **설치 됨** 범주를 확장 합니다 **Visual C++**  노드를 선택한 후 **Windows 데스크톱**.
1. 선택 된 **Windows 데스크톱 응용 프로그램** 템플릿.
1. 이름을 입력 _HelloComposition_, 클릭 **확인**합니다.

    Visual Studio 프로젝트를 만들고 기본 응용 프로그램 파일에 대 한 편집기를 엽니다.

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Windows 런타임 Api를 사용 하도록 프로젝트 구성

Windows Runtime (WinRT) Win32 앱에서 Api 사용 하려면 사용 하 여 C++/WinRT 합니다. 추가할 Visual Studio 프로젝트를 구성 해야 C++/WinRT 지원 합니다.

(자세한 내용은 참조 하세요 [시작 C++/WinRT-추가할 Windows 데스크톱 응용 프로그램 프로젝트를 수정 C++/WinRT 지원](/windows/uwp/cpp-and-winrt-apis/get-started.md#modify-a-windows-desktop-application-project-to-add-cwinrt-support)).

1. **프로젝트** 메뉴에서 프로젝트 속성을 엽니다 (_HelloComposition 속성_) 지정된 된 값으로 설정 되어 다음 설정을 확인 합니다.

    - 에 대 한 **Configuration**를 선택 _모든 구성_합니다. 에 대 한 **플랫폼**를 선택 _모든 플랫폼_합니다.
    - **구성 속성** > **일반** > **Windows SDK 버전** = _10.0.17763.0_ 이상

    ![SDK 버전 설정](images/visual-layer-interop/sdk-version.png)

    - **C /C++** > **언어**  >   **C++ 언어 표준** = _ISO C++ 17 표준 (/ stf:c + + 17)_

    ![표준 언어 설정](images/visual-layer-interop/language-standard.png)

    - **링커** > **입력** > **추가 종속성** 포함 해야 합니다 "_windowsapp.lib_"입니다. 목록에 포함 되지 않으면,이 추가 합니다.

    ![링커 종속성 추가](images/visual-layer-interop/linker-dependencies.png)

1. 미리 컴파일된 헤더를 업데이트 합니다.

    - 이름 바꾸기 `stdafx.h` 하 고 `stdafx.cpp` 하 `pch.h` 및 `pch.cpp`, 각각.
    - 프로젝트 속성 설정 **C /C++** > **미리 컴파일된 헤더** > **미리 컴파일된 헤더 파일** 하 *pch.h*.
    - 찾기 및 바꾸기 `#include "stdafx.h"` 사용 하 여 `#include "pch.h"` 모든 파일에 있습니다.

        (**편집할** > **찾기 및 바꾸기** > **파일에서 찾기**)

        ![찾기 및 바꾸기 stdafx.h](images/visual-layer-interop/replace-stdafx.png)

    - `pch.h`을 포함할 `winrt/base.h` 및 `unknwn.h`합니다.

        ```cppwinrt
        // reference additional headers your program requires here
        #include <unknwn.h>
        #include <winrt/base.h>
        ```

진행 하기 전에 오류가 없는지 확인 하는 시점에서 프로젝트를 빌드하는 것이 좋습니다.

## <a name="create-a-class-to-host-composition-elements"></a>호스트 구성 요소 클래스 만들기

콘텐츠를 호스트 하면 시각적 계층을 사용 하 여 만들기에 클래스를 만듭니다 (_CompositionHost_) 컴퍼지션 요소를 만들고 interop을 관리 합니다. 다음은 대부분의 컴퍼지션 Api를 포함 하 여 호스트에 대 한 구성 수행할 수입니다.

- 가져오기는 [Compositor](/uwp/api/windows.ui.composition.compositor), 생성 되 고에서 개체를 관리 합니다 [Windows.UI.Composition](/uwp/api/windows.ui.composition) 네임 스페이스입니다.
- 만들기는 [DispatcherQueueController](/uwp/api/windows.system.dispatcherqueuecontroller)/[DispatcherQueue](/uwp/api/windows.system.dispatcherqueue) WinRT Api에 대 한 작업을 관리할 수 있습니다.
- 만들기는 [DesktopWindowTarget](/uwp/api/windows.ui.composition.desktop.desktopwindowtarget) 및 컴퍼지션 컨테이너 컴퍼지션 개체를 표시 합니다.

단일 스레딩 문제를 방지 하려면 클래스 있도록 합니다. 예를 들어, 오류를 발생 시키는 동일한 스레드에서 CompositionHost의 두 번째 인스턴스 인스턴스화 하므로 스레드당 하나의 디스패처 큐를 만들만 있습니다.

> [!TIP]
> 해야 할 경우 자습서를 진행 하면서 모든 코드를 올바른 위치에 있는지 확인 하려면 자습서의 끝에서 전체 코드를 확인 합니다.

1. 새 클래스 파일을 프로젝트에 추가합니다.
    - **솔루션 탐색기**를 마우스 오른쪽 단추로 클릭 합니다 _HelloComposition_ 프로젝트입니다.
    - 상황에 맞는 메뉴에서 선택 **추가** > **클래스...** .
    - 에 **클래스 추가** 대화 상자에서 클래스의 이름 _CompositionHost.cs_, 클릭 **추가**합니다.

1. 헤더를 포함 하 고 _using_ 컴퍼지션 interop에 대 한 필요 합니다.
    - 이러한 CompositionHost.h, 추가 _포함_ 파일의 맨 위에 있는 합니다.

    ```cppwinrt
    #pragma once
    #include <winrt/Windows.UI.Composition.Desktop.h>
    #include <windows.ui.composition.interop.h>
    #include <DispatcherQueue.h>
    ```

    - 이러한 CompositionHost.cpp, 추가 _using_ 후 파일의 맨 위에 있는 _포함_합니다.

    ```cppwinrt
    using namespace winrt;
    using namespace Windows::System;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Composition::Desktop;
    using namespace Windows::Foundation::Numerics;
    ```

1. 단일 항목 패턴을 사용 하는 클래스를 편집 합니다.
    - CompositionHost.h에서 생성자를 비공개로 설정 합니다.
    - 공용 정적 선언 _GetInstance_ 메서드.

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

    - CompositionHost.cpp의 정의 추가 합니다 _GetInstance_ 메서드.

    ```cppwinrt
    CompositionHost* CompositionHost::GetInstance()
    {
        static CompositionHost instance;
        return &instance;
    }
    ```

1. CompositionHost.h, private 멤버 변수를 선언할 작성자, DispatcherQueueController, 및 DesktopWindowTarget 합니다.

    ```cppwinrt
    winrt::Windows::UI::Composition::Compositor m_compositor{ nullptr };
    winrt::Windows::System::DispatcherQueueController m_dispatcherQueueController{ nullptr };
    winrt::Windows::UI::Composition::Desktop::DesktopWindowTarget m_target{ nullptr };
    ```

1. 컴퍼지션 interop 개체를 초기화 하는 공용 메서드를 추가 합니다.
    > [!NOTE]
    > _초기화_를 호출 하는 _EnsureDispatcherQueue_를 _CreateDesktopWindowTarget_, 및 _CreateCompositionRoot_ 메서드. 다음 단계에서 이러한 메서드를 만듭니다.

    - CompositionHost.h, 라는 public 메서드를 선언할 _초기화_ 인수로 HWND를 사용 합니다.

    ```cppwinrt
    void Initialize(HWND hwnd);
    ```

    - CompositionHost.cpp의 정의 추가 합니다 _초기화_ 메서드.

    ```cppwinrt
    void CompositionHost::Initialize(HWND hwnd)
    {
        EnsureDispatcherQueue();
        if (m_dispatcherQueueController) m_compositor = Compositor();

        CreateDesktopWindowTarget(hwnd);
        CreateCompositionRoot();
    }
    ```

1. Windows 컴퍼지션을 사용 하는 스레드에서 디스패처 큐를 만듭니다.

    이 메서드는 첫 번째 초기화 하는 동안 디스패처 큐에 있는 스레드에서 Compositor는 만들어야 합니다.

    - CompositionHost.h, 라는 private 메서드를 선언할 _EnsureDispatcherQueue_합니다.

    ```cppwinrt
    void EnsureDispatcherQueue();
    ```

    - CompositionHost.cpp의 정의 추가 합니다 _EnsureDispatcherQueue_ 메서드.

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

1. 앱의 창 컴퍼지션 대상으로 등록 합니다.
    - CompositionHost.h, 라는 private 메서드를 선언할 _CreateDesktopWindowTarget_ 인수로 HWND를 사용 합니다.

    ```cppwinrt
    void CreateDesktopWindowTarget(HWND window);
    ```

    - CompositionHost.cpp의 정의 추가 합니다 _CreateDesktopWindowTarget_ 메서드.

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

1. 시각적 개체를 보유 하는 루트 시각적 컨테이너를 만듭니다.
    - CompositionHost.h, 라는 private 메서드를 선언할 _CreateCompositionRoot_합니다.

    ```cppwinrt
    void CreateCompositionRoot();
    ```

    - CompositionHost.cpp의 정의 추가 합니다 _CreateCompositionRoot_ 메서드.

    ```cppwinrt
    void CompositionHost::CreateCompositionRoot()
    {
        auto root = m_compositor.CreateContainerVisual();
        root.RelativeSizeAdjustment({ 1.0f, 1.0f });
        root.Offset({ 124, 12, 0 });
        m_target.Root(root);
    }
    ```

오류가 없는지 확인 하려면 지금 프로젝트를 빌드하십시오.

이러한 메서드는 UWP 시각적 계층 및 Win32 Api 간의 상호 운용성에 필요한 구성 요소를 설정 합니다. 이제 앱에 콘텐츠를 추가할 수 있습니다.

### <a name="add-composition-elements"></a>컴퍼지션 요소를 추가 합니다.

준비에서 인프라를 사용 하 여 이제 표시 하려는 컴퍼지션 콘텐츠를 생성할 수 있습니다.

예를 들어 임의로 색 사각형을 만드는 코드를 추가한 [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual) 짧은 지연 후 삭제 하는 애니메이션을 사용 하 여 합니다.

1. 컴퍼지션 요소를 추가 합니다.
    - CompositionHost.h, 라는 public 메서드를 선언할 _AddElement_ 3를 사용 하는 **float** 인수 값입니다.

    ```cppwinrt
    void AddElement(float size, float x, float y);
    ```

    - CompositionHost.cpp의 정의 추가 합니다 _AddElement_ 메서드.

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

## <a name="create-and-show-the-window"></a>만들고 창 표시

이제 Win32 ui 단추 및 UWP 컴퍼지션 콘텐츠를 추가할 수 있습니다.

1. 포함 파일의 맨 위에 있는 HelloComposition.cpp _CompositionHost.h_, BTN_ADD를 정의 하 고 CompositionHost 인스턴스를 가져옵니다.

    ```cppwinrt
    #include "CompositionHost.h"

    // #define MAX_LOADSTRING 100 // This is already in the file.
    #define BTN_ADD 1000

    CompositionHost* compHost = CompositionHost::GetInstance();
    ```

1. 에 `InitInstance` 메서드를 만든 창의 크기를 변경 합니다. (이 줄에서 변경할 `CW_USEDEFAULT, 0` 에 `900, 672`.)

    ```cppwinrt
    HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
        CW_USEDEFAULT, 0, 900, 672, nullptr, nullptr, hInstance, nullptr);
    ```

1. WndProc 함수에서 추가 `case WM_CREATE` 에 _메시지_ 스위치 블록입니다. 이 경우에 CompositionHost 초기화 하 고 단추를 만듭니다.

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

1. 또한 WndProc 함수에서 ui 컴퍼지션 요소를 추가 하는 단추 클릭을 처리 합니다. 

    추가 `case BTN_ADD` 에 _wmId_ WM_COMMAND 블록 내에서 스위치 블록입니다.

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

이제 작성 하 고 앱을 실행할 수 있습니다. 해야 할 경우 모든 코드를 올바른 위치 인지 확인 하려면 자습서의 끝에서 전체 코드를 확인 합니다.

앱을 실행 하 고 단추를 클릭 하는 경우 UI에 추가 하는 애니메이션된 사각형이 표시 됩니다.

## <a name="additional-resources"></a>추가 자료

- [Win32 HelloComposition 샘플 (GitHub)](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)
- [Win32를 사용 하 여 시작 하 고C++](/windows/desktop/learnwin32/learn-to-program-for-windows)
- [Windows 10 앱 시작](/windows/uwp/get-started/) (UWP)
- [Windows 10 용 데스크톱 응용 프로그램을 향상](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Windows.UI.Composition 네임 스페이스](/uwp/api/windows.ui.composition) (UWP)

## <a name="complete-code"></a>전체 코드

InitInstance 방법과 CompositionHost 클래스에 대 한 전체 코드는 다음과 같습니다.

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

### <a name="hellocompositioncpp-partial"></a>HelloComposition.cpp (부분)

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