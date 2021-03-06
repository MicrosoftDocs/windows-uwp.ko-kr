---
description: Agile 개체는 모든 스레드에서 액세스할 수 있는 개체입니다. C++/WinRT 형식은 기본적으로 Agile이지만 옵트아웃할 수 있습니다.
title: C++/WinRT의 Agile 개체
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, agile, 개체, agility, IAgileObject
ms.localizationpriority: medium
ms.openlocfilehash: 71800de1d209a0164ab5a7e90bbc191c0f9bebe9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154637"
---
# <a name="agile-objects-in-cwinrt"></a>C++/WinRT의 Agile 개체

대부분 경우 표준 C++ 개체 같은 Windows 런타임 클래스 인스턴스는(대부분의 표준 C++ 개체와 마찬가지로) 모든 스레드에서 액세스할 수 있습니다. 이러한 Windows 런타임 클래스는 *Agile*입니다. Windows와 함께 제공되는 소수의 Windows 런타임 클래스는 Agile이 아닙니다. 하지만 이러한 클래스를 사용할 때는 스레딩 모델과 마샬링 동작을 고려해야 합니다. 여기에서 마샬링이란 아파트 경계에서 데이터를 전달하는 것을 말합니다. 모든 Windows 런타임 개체가 Agile이도록 설정하면 기본적으로 사용자 고유의 [C++/WinRT](./intro-to-using-cpp-with-winrt.md) 형식도 Agile이기 때문에 매우 바람직합니다.

하지만 옵트아웃으로 Agile을 선택하지 않을 수도 있습니다. 예를 들어, 단일 스레드 아파트처럼 경우에 따라 형식 개체가 상주해야 하는 이유가 존재하기 때문입니다. 이는 일반적으로 다시 표시 요구 사항과 관련이 있습니다. 하지만 점차 UI(사용자 인터페이스) API 조차도 Agile 개체를 제공하고 있습니다. 일반적으로 Agility는 가장 간단하면서 성능이 뛰어난 옵션입니다. 또한 활성화 팩터리를 구현할 때 해당하는 런타임 클래스가 Agile하지 않더라도 Agile을 설정해야 합니다.

> [!NOTE]
> Windows 런타임은 COM을 기반으로 합니다. COM 용어로 Agile 클래스는 `ThreadingModel` = *Both*로 등록됩니다. COM 스레딩 모델 및 아파트에 대한 자세한 내용은 [COM 스레딩 모델의 이해와 사용](/previous-versions/ms809971(v=msdn.10))을 참조하세요.

## <a name="code-examples"></a>코드 예제

C++/WinRT가 Agility를 어떻게 지원하는지 살펴보기 위해 아래와 같은 런타임 클래스의 예제 구현을 사용합니다.

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

struct MyType : winrt::implements<MyType, IStringable>
{
    winrt::hstring ToString(){ ... }
};
```

옵트아웃을 설정하지 않았기 때문에 이 구현은 Agile입니다. [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) 기본 구조체는 [**IAgileObject**](/windows/desktop/api/objidl/nn-objidl-iagileobject)와 [**IMarshal**](/windows/desktop/api/objidl/nn-objidl-imarshal)을 구현합니다. **IMarshal** 구현은 **CoCreateFreeThreadedMarshaler**를 사용하여 **IAgileObject**에 대해 알지 못하는 레거시 코드에 올바른 작업을 수행합니다.

이 코드는 개체의 Agility 여부를 확인합니다. [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) 호출은 `myimpl`이 Agile이 아닐 경우 예외를 발생시킵니다.

```cppwinrt
winrt::com_ptr<MyType> myimpl{ winrt::make_self<MyType>() };
winrt::com_ptr<IAgileObject> iagileobject{ myimpl.as<IAgileObject>() };
```

따라서 예외를 처리하지 않으려면 [**IUnknown::try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)를 호출하는 것이 좋습니다.

```cppwinrt
winrt::com_ptr<IAgileObject> iagileobject{ myimpl.try_as<IAgileObject>() };
if (iagileobject) { /* myimpl is agile. */ }
```

**IAgileObject**는 자체적인 메서드가 없기 때문에 사용자가 많은 작업을 할 수 없습니다. 이때는 아래 변형을 사용하는 것이 더욱 일반적입니다.

```cppwinrt
if (myimpl.try_as<IAgileObject>()) { /* myimpl is agile. */ }
```

**IAgileObject**는 *마커 인터페이스*입니다. **IAgileObject**에 대한 쿼리의 성공 또는 실패 여부가 여기에서 얻을 수 있는 정보 및 유용성의 전부입니다.

## <a name="opting-out-of-agile-object-support"></a>Agile 개체 지원의 옵트아웃

[**winrt::non_agile**](/uwp/cpp-ref-for-winrt/non-agile) 마커 구조체를 템플릿 인수로 기본 클래스에 전달하여 명시적으로 Agile 개체 지원을 사용하지 않도록 옵트아웃을 선택할 수 있습니다.

**winrt::implements**에서 직접 파생되는 경우

```cppwinrt
struct MyImplementation: implements<MyImplementation, IStringable, winrt::non_agile>
{
    ...
}
```

런타임 클래스를 작성하는 경우

```cppwinrt
struct MyRuntimeClass: MyRuntimeClassT<MyRuntimeClass, winrt::non_agile>
{
    ...
}
```

variadic 매개 변수 팩에서 마커 구조체가 표시되는 경우는 중요하지 않습니다.

사용자가 Agility를 옵트아웃하든, 혹은 하지 않든 **IMarshal**을 직접 구현할 수 있습니다. 예를 들어, **winrt::non_agile** 마커를 사용하여 기본 Agility 구현을 방지하거나, 혹은&mdash;아마도 값에 따른 마샬링(marshal-by-value) 의미 체계를 지원할 목적으로 직접 **IMarshal**을 구현할 수도 있습니다.

## <a name="agile-references-winrtagile_ref"></a>Agile 참조(winrt::agile_ref)

Agile이 아닌 개체를 사용하지만 일부 Agile 가능성이 있는 컨텍스트일 때 개체를 전달해야 한다면 한 가지 옵션으로 [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) 구조체 템플릿을 사용하여 Agile 참조를 non-Agile 형식 인스턴스로, 혹은 non-Agile 개체의 인터페이스로 가져오는 방법이 있습니다.

```cppwinrt
NonAgileType nonagile_obj;
winrt::agile_ref<NonAgileType> agile{ nonagile_obj };
```

그 밖에 [**winrt::make_agile**](/uwp/cpp-ref-for-winrt/make-agile) 도우미 함수를 사용하는 방법도 있습니다.

```cppwinrt
NonAgileType nonagile_obj;
auto agile{ winrt::make_agile(nonagile_obj) };
```

어떤 경우가 되었든 `agile`을 자유롭게 다른 아파트의 스레드로 전달하여 사용할 수 있습니다.

```cppwinrt
co_await resume_background();
NonAgileType nonagile_obj_again{ agile.get() };
winrt::hstring message{ nonagile_obj_again.Message() };
```

[**agile_ref::get**](/uwp/cpp-ref-for-winrt/agile-ref#agile_refget-function) 호출은 **get**이 호출되는 스레드 컨텍스트에서 안전하게 사용될 수 있도록 프록시를 반환합니다.

## <a name="important-apis"></a>중요 API

* [IAgileObject 인터페이스](/windows/desktop/api/objidl/nn-objidl-iagileobject)
* [IMarshal 인터페이스](/windows/desktop/api/objidl/nn-objidl-imarshal)
* [winrt::agile_ref 구조체 템플릿](/uwp/cpp-ref-for-winrt/agile-ref)
* [winrt::implements 구조체 템플릿](/uwp/cpp-ref-for-winrt/implements)
* [winrt::make_agile 함수 템플릿](/uwp/cpp-ref-for-winrt/make-agile)
* [winrt::non_agile 마커 구조체](/uwp/cpp-ref-for-winrt/non-agile)
* [winrt::Windows::Foundation::IUnknown::as 함수](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)
* [winrt::Windows::Foundation::IUnknown::try_as 함수](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)

## <a name="related-topics"></a>관련 항목

* [COM 스레딩 모델의 이해와 사용](/previous-versions/ms809971(v=msdn.10))