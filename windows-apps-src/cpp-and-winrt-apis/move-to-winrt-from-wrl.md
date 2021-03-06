---
description: 이 토픽에서는 WRL 코드를 C++/WinRT의 상응하는 코드로 포팅하는 방법을 보여줍니다.
title: WRL에서 C++/WinRT로 이동
ms.date: 05/30/2018
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, 포팅, 마이그레이션, WRL
ms.localizationpriority: medium
ms.openlocfilehash: f7e7000a70a71f8ad90fd38c74d6e29882b4cbaa
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157260"
---
# <a name="move-to-cwinrt-from-wrl"></a>WRL에서 C++/WinRT로 이동
[Windows 런타임 C++ 템플릿 라이브러리(WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) 코드를 [C++/WinRT](./intro-to-using-cpp-with-winrt.md)의 상응하는 코드로 포팅하는 방법을 보여줍니다.

C++/WinRT로 포팅하는 첫 번째 단계는 수동으로 C++/WinRT 지원을 프로젝트에 추가하는 것입니다([C++/WinRT에 대한 Visual Studio 지원](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) 참조). 이 작업을 수행하려면 [Microsoft.Windows.CppWinRT NuGet 패키지](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)를 프로젝트에 설치합니다. Visual Studio에서 프로젝트를 열고, **프로젝트** \> **NuGet 패키지 관리...** \> **찾아보기**를 차례로 클릭하고, 검색 상자에서 **Microsoft.Windows.CppWinRT**를 입력하거나 붙여넣고, 검색 결과에서 해당 항목을 선택한 다음, **설치**를 클릭하여 해당 프로젝트에 대한 패키지를 설치합니다. 해당 변경의 효과 중 하나는 프로젝트에서 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)에 대한 지원이 꺼진다는 것입니다. 프로젝트에서 C++/CX를 사용 중인 경우 지원을 끈 채로 유지하고 C++/CX 코드를 C++/WinRT에도 업데이트할 수 있습니다([C++/CX에서 C++/WinRT로 이동](move-to-winrt-from-cx.md) 참조). 또는 지원을 다시 켜고(프로젝트 속성에서 **C/C++** \> **일반** \> **Windows 런타임 확장 사용** \> **예(/ZW)** ) 먼저 WRL 코드 포팅에 집중할 수 있습니다. C++/CX 및 C++/WinRT 코드는 같은 프로젝트에 공존할 수 있으며, XAML 컴파일러 지원 및 Windows 런타임 구성 요소의 예외가 있습니다([C++/CX에서 C++/WinRT로 이동](move-to-winrt-from-cx.md) 참조).

프로젝트 속성 **일반** \> **대상 플랫폼 버전**을 10.0.17134.0(Windows 10 버전 1803) 이상으로 설정합니다.

컴파일된 헤더 파일에(일반적으로 `pch.h`) `winrt/base.h`를 포함합니다.

```cppwinrt
#include <winrt/base.h>
```

C++/WinRT 프로젝션된 Windows API 헤더를 포함하는 경우(예: `winrt/Windows.Foundation.h`) 자동으로 포함되기 때문에 명시적으로 이와 같은 `winrt/base.h`를 포함할 필요가 없습니다.

## <a name="porting-wrl-com-smart-pointers-microsoftwrlcomptr"></a>WRL COM 스마트 포인터 포팅([Microsoft: WRL::ComPtr](/cpp/windows/comptr-class))
**Microsoft::WRL::ComPtr\<T\>** 을 사용하는 코드를 포팅하여 [**winrt::com_ptr\<T\>** ](/uwp/cpp-ref-for-winrt/com-ptr)을 사용합니다. 다음은 코드 이전과 이후의 예제입니다. *이후* 버전에서 [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput-function) 멤버 함수는 기본 원시 포인터를 설정할 수 있도록 검색합니다.

```cpp
ComPtr<IDXGIAdapter1> previousDefaultAdapter;
DX::ThrowIfFailed(m_dxgiFactory->EnumAdapters1(0, &previousDefaultAdapter));
```

```cppwinrt
winrt::com_ptr<IDXGIAdapter1> previousDefaultAdapter;
winrt::check_hresult(m_dxgiFactory->EnumAdapters1(0, previousDefaultAdapter.put()));
```

> [!IMPORTANT]
> 이미 배치된 [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr)이 있고(내부 원시 포인터에 이미 대상이 있음) 다른 개체를 가리키도록 다시 배치하려는 경우, 아래 코드 예제와 같이 `nullptr`을 먼저 할당해야 합니다. 할당하지 않으면, [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput-function) 또는 [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function)를 호출할 때 이미 배치된 **com_ptr**에서 내부 포인터가 Null이 아님을 어설션하여 문제가 부각됩니다.

```cppwinrt
winrt::com_ptr<IDXGISwapChain1> m_pDXGISwapChain1;
...
// We execute the code below each time the window size changes.
m_pDXGISwapChain1 = nullptr; // Important because we're about to re-seat 
winrt::check_hresult(
    m_pDxgiFactory->CreateSwapChainForHwnd(
        m_pCommandQueue.get(), // For Direct3D 12, this is a pointer to a direct command queue, and not to the device.
        m_hWnd,
        &swapChainDesc,
        nullptr,
        nullptr,
        m_pDXGISwapChain1.put())
);
```

다음 예제(*이후* 버전)에서는 [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function) 멤버 함수는 피할 포인터에 대한 포인터로 기본 원시 포인터를 검색합니다.

```cpp
ComPtr<ID3D12Debug> debugController;
if (SUCCEEDED(D3D12GetDebugInterface(IID_PPV_ARGS(&debugController))))
{
    debugController->EnableDebugLayer();
}
```

```cppwinrt
winrt::com_ptr<ID3D12Debug> debugController;
if (SUCCEEDED(D3D12GetDebugInterface(__uuidof(debugController), debugController.put_void())))
{
    debugController->EnableDebugLayer();
}
```

**ComPtr::Get**을 [**com_ptr::get**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrget-function)으로 바꿉니다.

```cpp
m_d3dDevice->CreateDepthStencilView(m_depthStencil.Get(), &dsvDesc, m_dsvHeap->GetCPUDescriptorHandleForHeapStart());
```

```cppwinrt
m_d3dDevice->CreateDepthStencilView(m_depthStencil.get(), &dsvDesc, m_dsvHeap->GetCPUDescriptorHandleForHeapStart());
```

**IUnknown** 포인터가 필요한 함수에 기본 원시 포인터를 전달하려면 다음 예제처럼 [**winrt::get_unknown**](/uwp/cpp-ref-for-winrt/get-unknown) 프리 함수를 사용합니다.

```cpp
ComPtr<IDXGISwapChain1> swapChain;
DX::ThrowIfFailed(
    m_dxgiFactory->CreateSwapChainForCoreWindow(
        m_commandQueue.Get(),
        reinterpret_cast<IUnknown*>(m_window.Get()),
        &swapChainDesc,
        nullptr,
        &swapChain
    )
);
```

```cppwinrt
winrt::agile_ref<winrt::Windows::UI::Core::CoreWindow> m_window; 
winrt::com_ptr<IDXGISwapChain1> swapChain;
winrt::check_hresult(
    m_dxgiFactory->CreateSwapChainForCoreWindow(
        m_commandQueue.get(),
        winrt::get_unknown(m_window.get()),
        &swapChainDesc,
        nullptr,
        swapChain.put()
    )
);
```

## <a name="porting-a-wrl-module-microsoftwrlmodule"></a>WRL 모듈 포팅(Microsoft: WRL::Module)
C++/WinRT 코드를 구성 요소를 구현하기 위해 WRL을 사용하는 기존 프로젝트에 서서히 추가할 수 있으며 기존 WRL 클래스는 계속 지원됩니다. 이 섹션에서는 그 방법을 보여줍니다.

Visual Studio에서 새 **Windows 런타임 구성 요소(C++/WinRT)** 프로젝트 형식을 만드는 경우 `Generated Files\module.g.cpp` 파일이 생성됩니다. 해당 파일은 프로젝트에 복사하고 추가할 수 있는 두 가지 유용한 C++/WinRT 함수(아래에 나열)의 정의를 포함합니다. 이러한 함수는 **WINRT_CanUnloadNow** 및 **WINRT_GetActivationFactory**이며, 보시다시피 조건에 따라 사용자가 포팅하는 스테이지를 지원하기 위한 순서로 WRL을 호출합니다.

```cppwinrt
HRESULT WINRT_CALL WINRT_CanUnloadNow()
{
#ifdef _WRL_MODULE_H_
    if (!::Microsoft::WRL::Module<::Microsoft::WRL::InProc>::GetModule().Terminate())
    {
        return S_FALSE;
    }
#endif

    if (winrt::get_module_lock())
    {
        return S_FALSE;
    }

    winrt::clear_factory_cache();
    return S_OK;
}

HRESULT WINRT_CALL WINRT_GetActivationFactory(HSTRING classId, void** factory)
{
    try
    {
        *factory = nullptr;
        wchar_t const* const name = WINRT_WindowsGetStringRawBuffer(classId, nullptr);

        if (0 == wcscmp(name, L"MoveFromWRLTest.Class"))
        {
            *factory = winrt::detach_abi(winrt::make<winrt::MoveFromWRLTest::factory_implementation::Class>());
            return S_OK;
        }

#ifdef _WRL_MODULE_H_
        return ::Microsoft::WRL::Module<::Microsoft::WRL::InProc>::GetModule().GetActivationFactory(classId, reinterpret_cast<::IActivationFactory**>(factory));
#else
        return winrt::hresult_class_not_available().to_abi();
#endif
    }
    catch (...) { return winrt::to_hresult(); }
}
```

이러한 기능을 프로젝트에 가져왔으면 [**Module::GetActivationFactory**](/cpp/windows/module-getactivationfactory-method)를 직접 호출하는 대신 **WINRT_GetActivationFactory**(내부적으로 WRL 함수를 호출)를 호출합니다. 다음은 코드 이전과 이후의 예제입니다.

```cpp
HRESULT WINAPI DllGetActivationFactory(_In_ HSTRING activatableClassId, _Out_ ::IActivationFactory **factory)
{
    auto & module = Microsoft::WRL::Module<Microsoft::WRL::InProc>::GetModule();
    return module.GetActivationFactory(activatableClassId, factory);
}
```

```cppwinrt
HRESULT __stdcall WINRT_GetActivationFactory(HSTRING activatableClassId, void** factory);
HRESULT WINAPI DllGetActivationFactory(_In_ HSTRING activatableClassId, _Out_ ::IActivationFactory **factory)
{
    return WINRT_GetActivationFactory(activatableClassId, reinterpret_cast<void**>(factory));
}
```

[**Module::Terminate**](/cpp/windows/module-terminate-method)를 직접 호출하는 대신 **WINRT_CanUnloadNow**(내부적으로 WRL 함수를 호출)를 호출합니다. 다음은 코드 이전과 이후의 예제입니다.

```cpp
HRESULT __stdcall DllCanUnloadNow(void)
{
    auto &module = Microsoft::WRL::Module<Microsoft::WRL::InProc>::GetModule();
    HRESULT hr = (module.Terminate() ? S_OK : S_FALSE);
    if (hr == S_OK)
    {
        hr = ...
    }
    return hr;
}
```

```cppwinrt
HRESULT __stdcall WINRT_CanUnloadNow();
HRESULT __stdcall DllCanUnloadNow(void)
{
    HRESULT hr = WINRT_CanUnloadNow();
    if (hr == S_OK)
    {
        hr = ...
    }
    return hr;
}
```

## <a name="important-apis"></a>중요 API
* [winrt::com_ptr 구조체 템플릿](/uwp/cpp-ref-for-winrt/com-ptr)
* [winrt::Windows::Foundation::IUnknown 구조체](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)

## <a name="related-topics"></a>관련 항목
* [C++/WinRT 소개](intro-to-using-cpp-with-winrt.md)
* [C++/CX에서 C++/WinRT로 이동](move-to-winrt-from-cx.md)
* [Windows 런타임 C++ 템플릿 라이브러리(WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl)