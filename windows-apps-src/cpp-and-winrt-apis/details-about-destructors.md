---
description: C++/WinRT 2.0을 사용하면 구현 유형의 소멸을 지연하고 소멸 중 안전하게 쿼리할 수 있습니다. 해당 기능과 이러한 기능을 사용하는 경우에 대해 설명합니다.
title: 소멸자에 대한 세부 정보
ms.date: 07/19/2019
ms.topic: article
keywords: Windows 10, UWP, 표준, C++, cpp, WinRT, 프로젝션, 지연된 소멸, 안전한 쿼리
ms.localizationpriority: medium
ms.openlocfilehash: 9806ea54665b24c246f2023714a14d94ec3bcc8e
ms.sourcegitcommit: 02cc7aaa408efe280b089ff27484e8bc879adf23
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68387800"
---
# <a name="details-about-destructors"></a>소멸자에 대한 세부 정보

C++/WinRT 2.0을 사용하면 구현 유형의 소멸을 지연하고 소멸 중 안전하게 쿼리할 수 있습니다. 해당 기능과 이러한 기능을 사용하는 경우에 대해 설명합니다.

## <a name="deferred-destruction"></a>지연된 소멸

[직접 할당 진단](/windows/uwp/cpp-and-winrt-apis/diag-direct-alloc) 항목에서 구현 형식에 프라이빗 소멸자가 있을 수 없다고 언급했습니다.

퍼블릭 소멸자를 사용하면 개체에 대한 최종 [**IUnknown::Release**](/windows/win32/api/unknwn/nf-unknwn-iunknown-release) 호출을 검색한 다음, 해당 개체의 소유권을 가져와서 소멸을 무기한 연기하는 기능인 지연된 소멸을 가능하게 한다는 이점이 있습니다.

클래식 COM 개체에서는 본질적으로 참조 수가 계산됩니다. 참조 수는 [**IUnknown::AddRef**](/windows/win32/api/unknwn/nf-unknwn-iunknown-addref) 및 **IUnknown::Release** 함수를 통해 관리됩니다. **Release**의 기존 구현에서 참조 수가 0에 도달하면 클래식 COM 개체의 C++ 소멸자가 호출됩니다.

```cppwinrt
uint32_t WINRT_CALL Release() noexcept
{
    uint32_t const remaining{ subtract_reference() };
 
    if (remaining == 0)
    {
        delete this;
    }
 
    return remaining;
}
```

`delete this;`는 개체에서 사용한 메모리를 해제하기 전에 개체의 소멸자를 호출합니다. 이는 소멸자에서 흥미로운 작업을 수행할 필요가 없는 경우에 충분히 효과가 있습니다.

```cppwinrt
using namespace winrt::Windows::Foundation;
... 
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    ~Sample() noexcept
    {
        // Too late to do anything interesting.
    }
};
```

*흥미롭다*는 것은 무엇을 의미할까요? 한 가지는 소멸자가 근본적으로 동기적이라는 것입니다. 다른 컨텍스트의 일부 스레드 특정 리소스를 소멸시킬 수 있는 스레드는 전환할 수 없습니다. 특정 리소스를 확보하기 위해 필요할 수 있는 다른 인터페이스에 대해 개체를 안정적으로 쿼리할 수 없습니다. 목록은 계속됩니다. 소멸이 중요한 경우에는 더 유연한 솔루션이 필요합니다. 여기서는 C++/WinRT의 **final_release** 함수가 제공됩니다.

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    static void final_release(std::unique_ptr<Sample> ptr) noexcept
    {
        // This is the first stop...
    }
 
    ~Sample() noexcept
    {
        // ...And this happens only when *unique_ptr* finally deletes the object.
    }
};
```

개체의 참조 수가 0으로 전환되면 바로 **final_release**를 호출하도록 **Release**의 C++/WinRT 구현을 업데이트했습니다. 이 상태에서 개체는 처리되지 않은 참조가 더 이상 없다고 확신할 수 있으며, 이제 개체 자체의 단독 소유권을 갖습니다. 이러한 이유로 자체 소유권을 정적 **final_release** 함수로 이전할 수 있습니다.

즉 개체는 공유 소유권을 지원하는 개체에서 단독으로 소유되는 개체로 변환되었습니다. **std::unique_ptr**은 개체에 대한 단독 소유권을 가지므로 **std:unique_ptr**이 범위를 벗어나면(이전에 다른 곳으로 이동하지 않는 경우) 개체를 해당 의미 체계의 일부로 자연스럽게 소멸시키며, 이에 따라 퍼블릭 소멸자가 필요합니다. 이것이 핵심입니다. **std::unique_ptr**에서 개체를 활성 상태로 유지하면 개체를 무기한으로 사용할 수 있습니다. 개체를 다른 곳으로 이동하는 방법에 대한 설명은 다음과 같습니다.

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    static void final_release(std::unique_ptr<Sample> ptr) noexcept
    {
        gc.push_back(std::move(ptr));
    }
};
```

이를 좀 더 결정적인 가비지 수집기라고 생각하세요. 더 실질적이고 강력하게 **final_release** 함수를 코루틴으로 변환하고, 필요에 따라 스레드를 일시 중단 및 전환할 수 있는 동시에 최종 소멸을 한 곳에서 처리할 수 있습니다.

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    static winrt::fire_and_forget final_release(std::unique_ptr<Sample> ptr) noexcept
    {
        co_await winrt::resume_background(); // Unwind the calling thread.
 
        // Safely perform complex teardown here.
    }
};
```

일시 중단이 발생하면 원래 **IUnknown::Release** 함수 호출을 시작한 호출 스레드가 반환되고, 이에 따라 해당 인터페이스 포인터를 통해 한 번 보유한 개체를 더 이상 사용할 수 없음을 호출자에게 알립니다. UI 프레임워크는 원래 개체를 만든 특정 UI 스레드에서 개체가 소멸되었는지 확인해야 하는 경우가 많습니다. 소멸이 개체를 해제하는 것과 분리되어 있으므로 이 기능은 이러한 요구 사항을 간단하게 처리할 수 있습니다.

## <a name="safe-queries-during-destruction"></a>소멸 중 안전한 쿼리

지연된 소멸이라는 개념을 기반으로 하는 것은 소멸 중에 인터페이스를 안전하게 쿼리하는 기능입니다.

클래식 COM은 두 가지 핵심 개념을 기반으로 합니다. 첫 번째는 참조 수 계산이고, 두 번째는 인터페이스 쿼리입니다. **IUnknown** 인터페이스는 **AddRef** 및 **Release** 외에도 [**QueryInterface**](/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(refiid_void)를 제공합니다. 이 메서드는 XAML과 같은 특정 UI 프레임워크에서 구성 가능한 형식 시스템을 시뮬레이션할 때 XAML 계층 구조를 트래버스하는 데 많이 사용됩니다. 간단한 예제를 살펴보겠습니다.

```cppwinrt
struct MainPage : PageT<MainPage>
{
    ~MainPage()
    {
        DataContext(nullptr);
    }
};
```

이는 무해한 것처럼 *보일* 수 있습니다. 이 XAML 페이지는 해당 소멸자에서 해당 데이터 컨텍스트를 지우려고 합니다. 그러나 [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext)는 **FrameworkElement** 기본 클래스의 속성이며, 고유한 **IFrameworkElement** 인터페이스에 있습니다. 결과적으로 C++/WinRT는 **DataContext** 속성을 호출하기 전에 올바른 vtable을 조회하기 위해 **QueryInterface**에 대한 호출을 삽입해야 합니다. 그러나 소멸자에도 적용되는 이유는 참조 수가 0으로 전환되었기 때문입니다. 여기서 **QueryInterface**를 호출하면 해당 참조 수가 일시적으로 중단됩니다. 그리고 0으로 다시 반환되면 개체가 다시 소멸됩니다.

C++/WinRT 2.0은 이를 지원하도록 강화되었습니다. Release에 대한 간소화된 형식의 C++/WinRT 2.0 구현은 다음과 같습니다.

```cppwinrt
uint32_t Release() noexcept
{
    uint32_t const remaining{ subtract_reference() };
 
    if (remaining == 0)
    {
        m_references = 1; // Debouncing!
        T::final_release(...);
    }
 
    return remaining;
}
```

예측할 수 있듯이 먼저 참조 수를 줄인 다음, 처리되지 않은 참조가 없는 경우에만 작동합니다. 그러나 이 항목의 앞부분에서 설명한 정적 **final_release** 함수를 호출하기 전에 참조 수를 1로 설정하여 안정화합니다. 이를 *디바운싱*(debouncing, 전기 공학 용어를 인용함)이라고 합니다. 이는 최종 참조가 해제되지 않도록 방지하는 데 중요합니다. 이 경우 참조 수가 불안정하고 **QueryInterface**에 대한 호출을 안정적으로 지원할 수 없습니다.

최종 참조가 해제된 후에 **QueryInterface**를 호출하면 참조 수가 무기한으로 증가할 수 있으므로 위험합니다. 개체의 수명을 연장시키지 않는 알려진 코드 경로만 호출하는 것은 사용자의 책임입니다. C++/WinRT는 이러한 **QueryInterface** 호출을 안정적으로 수행할 수 있도록 하여 중도에서 목표를 달성합니다.

이는 참조 수를 안정화하여 수행합니다. 최종 참조가 해제되면 실제 참조 수는 0이거나 일부 무분별한 예측할 수 없는 값입니다. 약한 참조가 관련되면 두 번째 경우가 발생할 수 있습니다. 어느 쪽이든 **QueryInterface**에 대한 후속 호출이 발생하면 반드시 참조 수가 일시적으로 증가하므로 이를 지속할 수 없습니다. 이에 따라 참조가 디바운싱됩니다. 1로 설정하면 이 개체에서 **Release**에 대한 최종 호출이 다시는 발생하지 않습니다. 이제 **std::unique_ptr**에서 개체를 소유하지만 **QueryInterface**/**Release** 쌍에 대한 바인딩된 호출이 안전하므로 이것이 정확히 원하는 것입니다.

좀 더 흥미로운 예제를 살펴보겠습니다.

```cppwinrt
struct MainPage : PageT<MainPage>
{
    ~MainPage()
    {
        DataContext(nullptr);
    }

    static winrt::fire_and_forget final_release(std::unique_ptr<MainPage> ptr)
    {
        co_await 5s;
        co_await winrt::resume_foreground(ptr->Dispatcher());
        ptr = nullptr;
    }
};
```

먼저 **final_release** 함수가 호출되어 이제 정리할 시간임을 구현에 알립니다. 여기서 **final_release**가 코루틴이 됩니다. 첫 번째 일시 중단 지점을 시뮬레이션하기 위해 스레드 풀에서 몇 초 동안 기다리는 것으로 시작합니다. 그런 다음, 페이지의 디스패처 스레드에서 다시 시작합니다. [**Dispatcher**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher)가 **DependencyObject** 기본 클래스의 속성이므로 이 마지막 단계에는 쿼리가 필요합니다. 마지막으로 `nullptr`을 **std::unique_ptr**에 할당하여 페이지가 실제로 삭제됩니다. 그러면 페이지의 소멸자가 호출됩니다.

소멸자 내에서 데이터 컨텍스트를 지웁니다. 알고 있듯이 이 컨텍스트에는 **FrameworkElement** 기본 클래스에 대한 쿼리가 필요합니다.

이 모든 것은 C++/WinRT 2.0에서 제공하는 참조 수 디바운싱(또는 참조 수 안정화)으로 인해 가능합니다.