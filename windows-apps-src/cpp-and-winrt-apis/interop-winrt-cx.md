---
description: 이 항목에서는 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 개체와 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 개체 간에 변환하는 데 사용할 수 있는 두 가지 도우미 함수를 보여 줍니다.
title: C++/WinRT와 C++/CX 간의 Interop
ms.date: 10/09/2018
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 이식, 마이그레이션, Interop, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: c786256efb5488fff65a8e2bdb4c5d2ca0fa181c
ms.sourcegitcommit: a9f44bbb23f0bc3ceade3af7781d012b9d6e5c9a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88180798"
---
# <a name="interop-between-cwinrt-and-ccx"></a>C++/WinRT와 C++/CX 간의 Interop

이 항목을 읽기 전에 [C++/CX에서 C++/WinRT로 이동](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx) 항목의 정보가 필요합니다. 이 항목에서는 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 프로젝트를 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)로 이식하기 위한 두 가지 주요 전략 옵션을 소개합니다.

- 전체 프로젝트를 한 번에 이식합니다. 너무 크지 않은 프로젝트를 위한 가장 간단한 옵션입니다. Windows 런타임 구성 요소 프로젝트가 있으면 이 전략이 유일한 옵션입니다.
- 프로젝트를 점진적으로 이식합니다(코드베이스의 크기 또는 복잡성으로 인해 필요할 수 있음). 하지만 이 전략은 C++/CX와 C++/WinRT 코드가 동일한 프로젝트에 한동안 나란히 존재하는 이식 프로세스를 따라야 합니다. XAML 프로젝트의 경우, 언제라도, XAML 페이지 유형이 모두 C++/WinRT 또는 모두 C++/CX 중 하나여야 합니다. 

이 interop 항목은 프로젝트를 점진적으로 이식해야 하는 경우인 &mdash;두 번째 전략과 관련이 있습니다. 이 항목에서는 동일한 프로젝트 내에서 C++/CX 개체를 C++/WinRT 개체로(또는 그 반대로) 변환하는 데 사용할 수 있는 두 가지 도우미 함수를 보여줍니다.

이러한 도우미 함수는 코드를 C++/CX에서 C++/WinRT로 점진적으로 이식할 때 매우 유용합니다. 또는 이식 여부에 관계없이 동일한 프로젝트에서 C++/WinRT 및 C++/CX 언어 프로젝션을 모두 사용하도록 선택하고 도우미 함수를 사용하여 둘 사이를 상호 운용할 수 있습니다.

이 항목을 읽은 후 동일한 프로젝트에서 PPL 작업과 코루틴을 나란히 지원(예: 작업 체인에서 코루틴 호출)하는 방법을 보여주는 정보 및 코드 예제를 참고하려면 고급 항목 [비동시성 및 C++/WinRT와 C++/CX 간의 상호 운용성](/windows/uwp/cpp-and-winrt-apis/interop-winrt-cx-async)을 참조하세요.

## <a name="the-from_cx-and-to_cx-functions"></a>**from_cx** 및 **to_cx** 함수

다음은 변환 도우미 함수 두 개를 포함하는 `interop_helpers.h`라는 헤더 파일의 소스 코드 목록입니다. 다음 섹션에서는 함수에 대해 설명하고 프로젝트에서 헤더 파일을 만들고 사용하는 방법을 설명합니다.

```cppwinrt
// interop_helpers.h
#pragma once

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
```

### <a name="the-from_cx-function"></a>**from_cx** 함수

**from_cx** 도우미 함수는 C++/CX 개체를 동등한 C++/WinRT 개체로 변환합니다. 이 함수는 C++/CX 개체를 기본 [**IUnknown**](/windows/win32/api/unknwn/nn-unknwn-iunknown) 인터페이스 포인터로 캐스팅합니다. 그런 다음, 해당 포인터에 대한 [**QueryInterface**](/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))를 호출하여 C++/WinRT 개체의 기본 인터페이스에 대해 쿼리를 실행합니다. **QueryInterface**는 C++/CX `safe_cast` 확장에 해당하는 Windows 런타임 ABI(애플리케이션 이진 인터페이스)입니다. 그러면 [**winrt::put_abi**](/uwp/cpp-ref-for-winrt/put-abi) 함수가 C++/WinRT 개체의 기본 **IUnknown** 인터페이스 포인터 주소를 다른 값으로 설정할 수 있도록 가져옵니다.

### <a name="the-to_cx-function"></a>**to_cx** 함수

**to_cx** 함수는 C++/WinRT 개체를 동등한 C++/CX 개체로 변환합니다. [**winrt::get_abi**](/uwp/cpp-ref-for-winrt/get-abi) 함수는 포인터를 C++/WinRT 개체의 기본 **IUnknown** 인터페이스로 가져옵니다. 그런 다음, 해당 포인터를 C++/CX 개체로 캐스팅한 후 C++/CX `safe_cast` 확장을 사용해 요청된 C++/CX 형식에 대해 쿼리를 실행합니다.

### <a name="the-interop_helpersh-header-file"></a>`interop_helpers.h` 헤더 파일

프로젝트에서 두 도우미 함수를 사용하려면 다음 단계를 수행합니다.

- 프로젝트에 새 **헤더 파일(.h)** 항목을 추가하고 `interop_helpers.h`라고 이름을 지정합니다.
- `interop_helpers.h`의 콘텐츠를 위의 코드 목록으로 바꿉니다.
- 이러한 포함을 `pch.h`에 추가합니다.

```cppwinrt
// pch.h
...
#include <unknwn.h>
// Include C++/WinRT projected Windows API headers here.
...
#include <interop_helpers.h>
```

## <a name="taking-a-ccx-project-and-adding-cwinrt-support"></a>C++/CX 프로젝트를 가져와서 C++/WinRT 지원 추가

이 섹션에서는 기존 C++/CX 프로젝트를 가져와서 C++/WinRT 지원을 추가한 다음, 이식 작업을 수행하기로 결정한 경우 수행할 작업을 설명합니다. 또한, [Visual Studio의 C++/WinRT 지원](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)을 참조하세요.

C++/CX 프로젝트에서 C++/CX와 C++/WinRT를 혼합하려면&mdash;프로젝트에서 **from_cx** 및 **to_cx** 도우미 함수 사용 포함&mdash; 프로젝트에 C++/WinRT 지원을 수동으로 추가해야 합니다.

먼저 Visual Studio에서 C++/CX 프로젝트를 열고 프로젝트 속성의 **일반** \> **대상 플랫폼 버전**이 10.0.17134.0(Windows 10 버전 1803) 이상으로 설정되어 있는지 확인합니다.

### <a name="install-the-cwinrt-nuget-package"></a>C++/WinRT NuGet 패키지 설치

[Microsoft.Windows.CppWinRT NuGet 패키지](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)는 C++/WinRT 빌드 지원(MSBuild 속성 및 대상)을 제공합니다. 설치하려면 메뉴 항목, **속성** \> **NuGet 패키지 관리...** \> **찾아보기**를 차례로 클릭하고, 검색 상자에서 **Microsoft.Windows.CppWinRT**를 입력하거나 붙여넣고, 검색 결과에서 해당 항목을 선택한 다음, **설치**를 클릭하여 해당 프로젝트에 대한 패키지를 설치합니다.

> [!IMPORTANT]
> C++/WinRT NuGet 패키지를 설치하면 C++/CX에 대한 지원이 프로젝트에서 해제됩니다. 한 번에 이식하려는 경우에는 지원이 해제된 상태를 유지하는 것이 좋습니다. 그러면 빌드 메시지가 C++/CX의 모든 종속성을 찾고 이식하는 데 도움이 됩니다(결국, 순수한 C++/CX 프로젝트를 순수한 C++/WinRT 프로젝트로 전환). 하지만 다시 켜는 방법에 대한 정보는 다음 섹션을 참조하세요.

### <a name="turn-ccx-support-back-on"></a>C++/CX 지원 다시 켜기

한 번에 이식하려는 경우에는 이렇게 할 필요가 없습니다. 하지만 점진적으로 이식해야 하는 경우에는 이 시점에 프로젝트에서 C++/CX 지원을 다시 켜야 합니다. 프로젝트 속성에서 **C/C++** \>**일반** \>**Windows 런타임 확장 사용** \>**예(/ZW)** 를 선택합니다.

또는(XAML 프로젝트의 경우) Visual Studio에서 C++/WinRT 프로젝트 속성 페이지를 사용하여 C++/CX 지원을 추가할 수 있습니다. 프로젝트 속성에서 **공용 속성** \> **C++/WinRT** \> **프로젝트 언어** \> **C++/CX**를 선택합니다. 이렇게 하면 `.vcxproj` 파일에 다음 속성이 추가됩니다.

```xml
<syntaxhighlight lang="xml">
  <PropertyGroup Label="Globals">
    <CppWinRTProjectLanguage>C++/CX</CppWinRTProjectLanguage>
  </PropertyGroup>
</syntaxhighlight>
```

> [!IMPORTANT]
> **Midl 파일(.idl)** 의 콘텐츠를 스텁 파일로 처리해야 할 때마다 **프로젝트 언어**를 다시 **C++/WinRT**로 변경해야 합니다. 빌드가 스텁을 생성한 후 **프로젝트 언어**를 다시 **C++/CX**로 변경합니다.

비슷한 사용자 지정 옵션(`cppwinrt.exe` 도구의 동작을 미세 조정) 목록은 Microsoft.Windows.CppWinRT NuGet 패키지 [추가 정보](https://github.com/microsoft/cppwinrt/blob/master/nuget/readme.md#customizing)를 참조하세요.

### <a name="include-cwinrt-header-files"></a>C++/WinRT 헤더 파일 포함

미리 컴파일된 헤더 파일(일반적으로 `pch.h`)에 최소한 수행해야 하는 작업은 아래와 같이 `winrt/base.h`를 포함하는 것입니다.

```cppwinrt
// pch.h
...
#include <winrt/base.h>
...
```

하지만 **winrt::Windows::Foundation** 네임스페이스에 유형이 거의 확실하게 필요합니다. 그리고, 필요한 네임스페이스를 이미 알고 있을 수도 있습니다. 따라서, 다음과 같이 네임스페이스에 해당하는 C++/WinRT 프로젝션된 Windows API 헤더를 포함합니다. (`winrt/base.h`는 자동으로 포함되기 때문에 지금 명시적으로 포함할 필요가 없습니다.)

```cppwinrt
// pch.h
...
#include <winrt/Windows.Foundation.h>
// Include any other C++/WinRT projected Windows API headers here.
...
```

네임스페이스 별칭 `namespace cx` 및 `namespace winrt`를 사용하는 기법에 대해서는 다음 섹션(*C++/WinRT 프로젝트를 가져와서 C++/CX 지원 추가*)에서 코드 예제를 참조하세요. 이 기법을 사용하면 C++/WinRT 프로젝션과 C++/CX 프로젝션 간의 잠재적인 네임스페이스 충돌을 처리할 수 있습니다.

### <a name="add-interop_helpersh-to-the-project"></a>프로젝트에 `interop_helpers.h` 추가

이제 **from_cx**와 **to_cx** 함수를 C++/CX 프로젝트에 추가할 수 있습니다. 이 작업을 수행하는 방법에 대한 지침은 위의 [**from_cx** 및 **to_cx** 함수](#the-from_cx-and-to_cx-functions) 섹션을 참조하세요.

## <a name="taking-a-cwinrt-project-and-adding-ccx-support"></a>C++/WinRT 프로젝트를 가져와서 C++/CX 지원 추가

이 섹션에서는 새 C++/WinRT 프로젝트를 만들고 여기에서 이식 작업을 수행하기로 결정한 경우 수행할 작업을 설명합니다.

C++/WinRT 프로젝트에서 C++/WinRT와 C++/CX를 혼합하려면&mdash;프로젝트에서 **from_cx** 및 **to_cx** 도우미 함수 사용 포함&mdash; 프로젝트에 C++/CX 지원을 수동으로 추가해야 합니다.

- C++/WinRT 프로젝트 템플릿 중 하나를 사용하여 Visual Studio에서 새 C++/WinRT 프로젝트를 만듭니다([Visual Studio의 C++/WinRT 지원](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) 참조).
- C++/CX에 대한 프로젝트 지원을 켭니다. 프로젝트 속성에서 **C/C++** \> **일반** \> **Windows 런타임 확장 사용** \> **예(/ZW)** 를 선택합니다.

### <a name="an-example-cwinrt-project-showing-the-two-helper-functions-in-use"></a>사용 중인 두 개의 도우미 함수를 보여 주는 예제 C++/WinRT 프로젝트

이 섹션에서는 **from_cx** 및 **to_cx** 사용 방법을 보여주는 예제 C++/WinRT 프로젝트를 만들 수 있습니다. 또한 C++/WinRT 프로젝션 및 C++/CX 프로젝션 간의 잠재적인 네임스페이스 충돌을 다루기 위해 코드의 다른 격리 영역에 대한 네임스페이스 별칭을 사용하는 방법을 보여 줍니다.

- a **Visual C++** \> **Windows Universal** \> **Core App(C++/WinRT)** 프로젝트를 만듭니다.
- 프로젝트 속성에서 **C/C++** \> **일반** \> **Windows 런타임 확장 사용** \> **예(/ZW)** 를 선택합니다.
- 프로젝트에 `interop_helpers.h`을 추가합니다. 이 작업을 수행하는 방법에 대한 지침은 위의 [**from_cx** 및 **to_cx** 함수](#the-from_cx-and-to_cx-functions) 섹션을 참조하세요.
- `App.cpp`의 콘텐츠를 아래 코드 목록으로 바꾸고, 빌드하고, 실행합니다.

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
* [IUnknown 인터페이스](/windows/win32/api/unknwn/nn-unknwn-iunknown)
* [QueryInterface 함수](/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))
* [winrt::get_abi 함수](/uwp/cpp-ref-for-winrt/get-abi)
* [winrt::put_abi 함수](/uwp/cpp-ref-for-winrt/put-abi)

## <a name="related-topics"></a>관련 항목
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [C++/CX에서 C++/WinRT로 이동](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx)
* [비동시성 및 C++/WinRT와 C++/CX 간의 상호 운용성](/windows/uwp/cpp-and-winrt-apis/interop-winrt-cx-async)
