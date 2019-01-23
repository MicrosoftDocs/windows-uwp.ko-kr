---
description: 이번 항목에서는 C++/CX 개체와 C++/WinRT 개체를 서로 변환하는 데 사용할 수 있는 두 가지 도우미 함수에 대해서 설명합니다.
title: C++/WinRT와 C++/CX 사이의 상호 운용성
ms.date: 10/09/2018
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 이식, 마이그레이션, 상호 운용성, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: e1e4570320e9d48351ccb01052fc77d35ae03642
ms.sourcegitcommit: 4a359aecafb73d73b5a8e78f7907e565a2a43c41
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/22/2019
ms.locfileid: "9024572"
---
# <a name="interop-between-cwinrt-and-ccx"></a>C++/WinRT와 C++/CX 사이의 상호 운용성

점진적으로의 코드를 포팅 전략에 [C + + CX](/cpp/cppcx/visual-c-language-reference-c-cx) 프로젝트 [C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 에 대해서는 설명 [이동 C + +에서 C + + CX](move-to-winrt-from-cx.md)합니다.

이 항목에서는 C + 간에 변환 하는 데 사용할 수 있는 두 가지 도우미 함수 + CX 및 C + + /winrt 개체는 같은 프로젝트 내 합니다. 두 언어 프로젝션을 사용 하는 코드 사이의 상호 운용성 사용 하거나 C +에서 코드를 포팅 대로 함수를 사용할 수 + /CX C + + WinRT 합니다.

## <a name="fromcx-and-tocx-functions"></a>from_cx 함수와 to_cx 함수
아래 도우미 함수는 C++/CX 개체를 상응하는 C++/WinRT 개체로 변환합니다. 이 함수는 C++/CX 개체를 기본 [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509) 인터페이스 포인터로 캐스팅합니다. 그런 다음 해당 포인터에 대한 [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521)를 호출하여 C++/WinRT 개체의 기본 인터페이스에 대해 쿼리를 실행합니다. **QueryInterface**는 C++/CX safe_cast 확장에 상응하는 Windows 런타임 응용 프로그램 이진 인터페이스(ABI)입니다. 그러면 [**winrt::put_abi**](/uwp/cpp-ref-for-winrt/put-abi) 함수가 C++/WinRT 개체의 기본 **IUnknown** 인터페이스 포인터 주소를 다른 값으로 설정할 수 있도록 가져옵니다.

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

아래 도우미 함수는 C++/WinRT 개체를 상응하는 C++/CX 개체로 변환합니다. [**winrt::get_abi**](/uwp/cpp-ref-for-winrt/get-abi) 함수는 포인터를 C++/WinRT 개체의 기본 **IUnknown** 인터페이스로 가져옵니다. 그런 다음 포인터를 C++/CX 개체로 캐스팅한 후 C++/CX safe_cast 확장을 사용해 요청된 C++/CX 형식에 대해 쿼리를 실행합니다.

```cppwinrt
template <typename T>
T^ to_cx(winrt::Windows::Foundation::IUnknown const& from)
{
    return safe_cast<T^>(reinterpret_cast<Platform::Object^>(winrt::get_abi(from)));
}
```

## <a name="example-project-showing-the-two-helper-functions-in-use"></a>사용 중인 두 가지 도우미 함수를 보여 주는 예제 프로젝트

간단한 방식으로 점진적으로 C +의 코드를 포팅하는 시나리오를 재현 하려면 + CX 프로젝트를 C + + /winrt는 C + 중 하나를 사용 하 여 Visual Studio에서 새 프로젝트를 만들어서 시작할 수 있습니다 + /winrt 프로젝트 템플릿과 (C +에 대 한 Visual Studio 지원 [참조 + WinRT 및 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-and-the-vsix)).

이 예제에서는 프로젝트 C + 간의 잠재적인 네임 스페이스 충돌을 처리 하기 위해 코드의 다른 제도 대 한 네임 스페이스 별칭을 사용 하는 방법을 보여 + WinRT 프로젝션 및 C + + /CX 프로젝션 합니다.

- **Visual c + +** 만들기 \> **Windows 유니버설** > **Core App (C + + WinRT)** 프로젝트.
- **C/c + +** 프로젝트 속성에서 \> **일반** \> **Windows 런타임 확장** \> **예 (/ZW)**. 이 설정 프로젝트 지원과 C + + CX 합니다.
- 내용을 `App.cpp` 아래 코드 목록을 사용 합니다.

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
* [IUnknown 인터페이스](https://msdn.microsoft.com/library/windows/desktop/ms680509)
* [QueryInterface 함수](https://msdn.microsoft.com/library/windows/desktop/ms682521)
* [winrt::get_abi 함수](/uwp/cpp-ref-for-winrt/get-abi)
* [winrt::put_abi 함수](/uwp/cpp-ref-for-winrt/put-abi)

## <a name="related-topics"></a>관련 항목
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [C++/CX에서 C++/WinRT로 이동](move-to-winrt-from-cx.md)
