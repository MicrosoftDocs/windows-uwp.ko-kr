---
description: 이 항목에서는 C++/CX 개체와 C++/WinRT 개체 간에 변환하는 데 사용할 수 있는 두 가지 도우미 함수를 보여 줍니다.
title: C++/WinRT와 C++/CX 간의 Interop
ms.date: 10/09/2018
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 이식, 마이그레이션, Interop, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: a6b57627cbf9021732a8a66818250ffc1fca915f
ms.sourcegitcommit: 7585bf66405b307d7ed7788d49003dc4ddba65e6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67660120"
---
# <a name="interop-between-cwinrt-and-ccx"></a>C++/WinRT와 C++/CX 간의 Interop

[C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 프로젝트의 코드를 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)로 점진적으로 이식하는 전략은 [C++/CX에서 C++/WinRT로 이동](move-to-winrt-from-cx.md)에서 설명합니다.

이 항목에서는 동일한 프로젝트 내에서 C++/CX 개체와 C++/WinRT 개체 간에 변환하는 데 사용할 수 있는 두 가지 도우미 함수를 보여 줍니다. 두 언어 프로젝션을 사용하는 코드 사이의 interop에 도우미 함수를 사용하거나 코드를 C++/CX에서 C++/WinRT로 이식할 때 함수를 사용할 수 있습니다.

## <a name="fromcx-and-tocx-functions"></a>from_cx 함수와 to_cx 함수
아래 도우미 함수는 C++/CX 개체를 상응하는 C++/WinRT 개체로 변환합니다. 이 함수는 C++/CX 개체를 기본 [**IUnknown**](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown) 인터페이스 포인터로 캐스팅합니다. 그런 다음, 해당 포인터에 대한 [**QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))를 호출하여 C++/WinRT 개체의 기본 인터페이스에 대해 쿼리를 실행합니다. **QueryInterface**는 C++/CX safe_cast 확장에 상응하는 Windows 런타임 ABI(Application Binary Interface)입니다. 그러면 [**winrt::put_abi**](/uwp/cpp-ref-for-winrt/put-abi) 함수가 C++/WinRT 개체의 기본 **IUnknown** 인터페이스 포인터 주소를 다른 값으로 설정할 수 있도록 가져옵니다.

```cppwinrt
template <typename T>
T from_cx(Platform::Object^ from)
{
    T to{ nullptr };

    winrt::check_hresult(reinterpret_cast<::IUnknown*>(from)
        ->QueryInterface(winrt::guid_of<T>(),
            reinterpret_cast<void**>(winrt::put_abi(to))));

    return to;
}
```

아래 도우미 함수는 C++/WinRT 개체를 상응하는 C++/CX 개체로 변환합니다. [**winrt::get_abi**](/uwp/cpp-ref-for-winrt/get-abi) 함수는 포인터를 C++/WinRT 개체의 기본 **IUnknown** 인터페이스로 가져옵니다. 그런 다음, 해당 포인터를 C++/CX 개체로 캐스팅한 후 C++/CX safe_cast 확장을 사용해 요청된 C++/CX 형식에 대해 쿼리를 실행합니다.

```cppwinrt
template <typename T>
T^ to_cx(winrt::Windows::Foundation::IUnknown const& from)
{
    return safe_cast<T^>(reinterpret_cast<Platform::Object^>(winrt::get_abi(from)));
}
```

## <a name="example-project-showing-the-two-helper-functions-in-use"></a>사용 중인 두 개의 도우미 함수를 보여 주는 예제 프로젝트

간단한 방법으로 C++/CX 프로젝트에서 C++/WinRT로 코드를 점진적으로 이식하는 시나리오를 재현하려면 먼저 C++/WinRT 프로젝트 템플릿 중 하나를 사용하여 Visual Studio에서 새 프로젝트를 만듭니다([Visual Studio의 C++/WinRT 지원](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) 참조).

또한 이 예제 프로젝트에서는 C++/WinRT 프로젝션 및 C++/CX 프로젝션 간의 잠재적인 네임스페이스 충돌을 다루기 위해 코드의 다른 격리 영역에 대한 네임스페이스 별칭을 사용하는 방법을 보여 줍니다.

- a **Visual C++** \> **Windows Universal** > **Core App(C++/WinRT)** 프로젝트를 만듭니다.
- 프로젝트 속성에서 **C/C++** \> **일반** \> **Windows 런타임 확장 사용** \> **예(/ZW)** 를 선택합니다. 이렇게 하면 C++/CX에 대한 프로젝트 지원이 켜집니다.
- `App.cpp`의 콘텐츠를 아래 코드 목록으로 바꿉니다.

`WINRT_ASSERT`는 매크로 정의이며 [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros)로 확장됩니다.

```cppwinrt
// App.cpp
#include "pch.h"
#include <sstream>

namespace cx
{
    using namespace Windows::Foundation;
}

namespace winrt
{
    using namespace Windows;
    using namespace Windows::ApplicationModel::Core;
    using namespace Windows::Foundation;
    using namespace Windows::Foundation::Numerics;
    using namespace Windows::UI;
    using namespace Windows::UI::Core;
    using namespace Windows::UI::Composition;
}

template <typename T>
T from_cx(Platform::Object^ from)
{
    T to{ nullptr };

    winrt::check_hresult(reinterpret_cast<::IUnknown*>(from)
        ->QueryInterface(winrt::guid_of<T>(),
            reinterpret_cast<void**>(winrt::put_abi(to))));

    return to;
}

template <typename T>
T^ to_cx(winrt::Windows::Foundation::IUnknown const& from)
{
    return safe_cast<T^>(reinterpret_cast<Platform::Object^>(winrt::get_abi(from)));
}

struct App : winrt::implements<App, winrt::IFrameworkViewSource, winrt::IFrameworkView>
{
    winrt::CompositionTarget m_target{ nullptr };
    winrt::VisualCollection m_visuals{ nullptr };
    winrt::Visual m_selected{ nullptr };
    winrt::float2 m_offset{};

    winrt::IFrameworkView CreateView()
    {
        return *this;
    }

    void Initialize(winrt::CoreApplicationView const &)
    {
    }

    void Load(winrt::hstring const&)
    {
    }

    void Uninitialize()
    {
    }

    void Run()
    {
        winrt::CoreWindow window = winrt::CoreWindow::GetForCurrentThread();
        window.Activate();

        winrt::CoreDispatcher dispatcher = window.Dispatcher();
        dispatcher.ProcessEvents(winrt::CoreProcessEventsOption::ProcessUntilQuit);
    }

    void SetWindow(winrt::CoreWindow const & window)
    {
        winrt::Compositor compositor;
        winrt::ContainerVisual root = compositor.CreateContainerVisual();
        m_target = compositor.CreateTargetForCurrentView();
        m_target.Root(root);
        m_visuals = root.Children();

        window.PointerPressed({ this, &App::OnPointerPressed });
        window.PointerMoved({ this, &App::OnPointerMoved });

        window.PointerReleased([&](auto && ...)
        {
            m_selected = nullptr;
        });
    }

    void OnPointerPressed(IInspectable const &, winrt::PointerEventArgs const & args)
    {
        winrt::float2 const point = args.CurrentPoint().Position();

        for (winrt::Visual visual : m_visuals)
        {
            winrt::float3 const offset = visual.Offset();
            winrt::float2 const size = visual.Size();

            if (point.x >= offset.x &&
                point.x < offset.x + size.x &&
                point.y >= offset.y &&
                point.y < offset.y + size.y)
            {
                m_selected = visual;
                m_offset.x = offset.x - point.x;
                m_offset.y = offset.y - point.y;
            }
        }

        if (m_selected)
        {
            m_visuals.Remove(m_selected);
            m_visuals.InsertAtTop(m_selected);
        }
        else
        {
            AddVisual(point);
        }
    }

    void OnPointerMoved(IInspectable const &, winrt::PointerEventArgs const & args)
    {
        if (m_selected)
        {
            winrt::float2 const point = args.CurrentPoint().Position();

            m_selected.Offset(
            {
                point.x + m_offset.x,
                point.y + m_offset.y,
                0.0f
            });
        }
    }

    void AddVisual(winrt::float2 const point)
    {
        winrt::Compositor compositor = m_visuals.Compositor();
        winrt::SpriteVisual visual = compositor.CreateSpriteVisual();

        static winrt::Color colors[] =
        {
            { 0xDC, 0x5B, 0x9B, 0xD5 },
            { 0xDC, 0xED, 0x7D, 0x31 },
            { 0xDC, 0x70, 0xAD, 0x47 },
            { 0xDC, 0xFF, 0xC0, 0x00 }
        };

        static unsigned last = 0;
        unsigned const next = ++last % _countof(colors);
        visual.Brush(compositor.CreateColorBrush(colors[next]));

        float const BlockSize = 100.0f;

        visual.Size(
        {
            BlockSize,
            BlockSize
        });

        visual.Offset(
        {
            point.x - BlockSize / 2.0f,
            point.y - BlockSize / 2.0f,
            0.0f,
        });

        m_visuals.InsertAtTop(visual);

        m_selected = visual;
        m_offset.x = -BlockSize / 2.0f;
        m_offset.y = -BlockSize / 2.0f;
    }
};

int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    winrt::init_apartment();

    winrt::Uri uri(L"http://aka.ms/cppwinrt");
    std::wstringstream wstringstream;
    wstringstream << L"C++/WinRT: " << uri.Domain().c_str() << std::endl;

    // Convert from a C++/WinRT type to a C++/CX type.
    cx::Uri^ cx = to_cx<cx::Uri>(uri);
    wstringstream << L"C++/CX: " << cx->Domain->Data() << std::endl;
    ::OutputDebugString(wstringstream.str().c_str());

    // Convert from a C++/CX type to a C++/WinRT type.
    winrt::Uri uri_from_cx = from_cx<winrt::Uri>(cx);
    WINRT_ASSERT(uri.Domain() == uri_from_cx.Domain());
    WINRT_ASSERT(uri == uri_from_cx);

    winrt::CoreApplication::Run(winrt::make<App>());
}
```

## <a name="important-apis"></a>중요 API
* [IUnknown 인터페이스](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown)
* [QueryInterface 함수](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))
* [winrt::get_abi 함수](/uwp/cpp-ref-for-winrt/get-abi)
* [winrt::put_abi 함수](/uwp/cpp-ref-for-winrt/put-abi)

## <a name="related-topics"></a>관련 항목
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [C++/CX에서 C++/WinRT로 이동](move-to-winrt-from-cx.md)
