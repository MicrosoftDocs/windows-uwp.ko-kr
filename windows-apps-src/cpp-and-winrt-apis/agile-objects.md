---
description: Agile 개체란 어떤 스레드에서든지 액세스할 수 있는 개체를 말합니다. C++/WinRT 형식은 기본적으로 Agile이지만 옵트아웃으로 선택하지 않을 수도 있습니다.
title: C++/WinRT의 Agile 개체
ms.date: 10/20/2018
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션, agile, 개체, agility, IAgileObject
ms.localizationpriority: medium
ms.openlocfilehash: 0b390161a4eb2c4f38fed9bce226c5a5e92c5ad8
ms.sourcegitcommit: 82edc63a5b3623abce1d5e70d8e200a58dec673c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58291782"
---
# <a name="agile-objects-in-cwinrt"></a>Agile 개체의 C++/WinRT

대부분의 경우 Windows 런타임 클래스 인스턴스의 모든 스레드에서 액세스할 수 있습니다 (대부분의 표준와 마찬가지로 C++ 개체 수)입니다. 이러한 Windows 런타임 클래스는 *agile*합니다. 소수의 Windows와 함께 제공 되는 Windows 런타임 클래스는 agile, 하지만 해당 스레딩 모델 및 마샬링 동작을 고려해 야 필요가 하를 사용 하는 경우에 (마샬링은 데이터 전달 아파트 경계를 넘어). Agile, 되도록 모든 Windows 런타임 개체에 대 한 좋은 기본 하므로 고유한 [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 형식은 기본적으로 agile 합니다.

하지만 옵트아웃할 수 있습니다. 예를 들어, 지정 된 단일 스레드 아파트에 상주할 형식의 개체를 요구 하는 것이 유리가 있을 수 있습니다. 이는 일반적으로 다시 표시 요구 사항과 관련이 있습니다. 하지만 점차 사용자 인터페이스(UI) API 조차도 Agile 개체를 제공하고 있습니다. 일반적으로 Agility는 가장 간단하면서 성능이 뛰어난 옵션입니다. 또한 활성화 팩터리를 구현할 때 해당하는 런타임 클래스가 Agile하지 않더라도 Agile을 설정해야 합니다.

> [!NOTE]
> Windows 런타임은 COM을 기반으로 합니다. COM 용어로 Agile 클래스는 `ThreadingModel` = *Both*로 등록됩니다. COM 스레딩 모델 및 아파트에 대 한 자세한 내용은 참조 하세요. [이해와 Using COM Threading Models](https://msdn.microsoft.com/library/ms809971)합니다.

## <a name="code-examples"></a>코드 예제

보겠습니다 런타임 클래스의 예제 구현을 설명 하기 위해 사용 하는 방법 C++/WinRT 민첩성을 지원 합니다.

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

struct MyType : winrt::implements<MyType, IStringable>
{
    winrt::hstring ToString(){ ... }
};
```

옵트아웃을 설정하지 않았기 때문에 이 구현체는 Agile입니다. [  **winrt::implements**](/uwp/cpp-ref-for-winrt/implements) 기본 구조체는 [**IAgileObject**](https://msdn.microsoft.com/library/windows/desktop/hh802476)와 [**IMarshal**](/windows/desktop/api/objidl/nn-objidl-imarshal)을 구현합니다. **IAgileObject**를 모르는 레거시 코드일 때는 **IMarshal** 구현체가 **CoCreateFreeThreadedMarshaler**를 사용하여 정확하게 처리합니다.

이 코드는 개체의 Agility 여부를 확인합니다. [  **IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) 호출은 `myimpl`이 Agile이 아닐 경우 예외를 발생시킵니다.

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

[  **winrt::non_agile**](/uwp/cpp-ref-for-winrt/non-agile) 마커 구조체를 템플릿 인수로 기본 클래스에 전달하여 명시적으로 Agile 개체 지원을 사용하지 않도록 옵트아웃을 선택할 수 있습니다.

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

민첩성을 옵트아웃 하는 여부를 구현할 수 있습니다 **IMarshal** 직접. 예를 들어 사용할 수 있습니다 합니다 **winrt::non_agile** 기본 민첩성 구현을 방지 하 고 구현 하는 표식 **IMarshal** 직접&mdash;를 값으로 마샬링 의미 체계를 지원 합니다.

## <a name="agile-references-winrtagileref"></a>Agile 참조(winrt::agile_ref)

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

[  **agile_ref::get**](/uwp/cpp-ref-for-winrt/agile-ref#agile_refget-function) 호출은 **get**이 호출되는 스레드 컨텍스트에서 안전하게 사용될 수 있도록 프록시를 반환합니다.

## <a name="important-apis"></a>중요 API

* [IAgileObject 인터페이스](https://msdn.microsoft.com/library/windows/desktop/hh802476)
* [IMarshal 인터페이스](/windows/desktop/api/objidl/nn-objidl-imarshal)
* [winrt::agile_ref 구조체 템플릿](/uwp/cpp-ref-for-winrt/agile-ref)
* [winrt::implements 구조체 템플릿](/uwp/cpp-ref-for-winrt/implements)
* [winrt::make_agile 함수 템플릿](/uwp/cpp-ref-for-winrt/make-agile)
* [winrt::non_agile 표식 구조체](/uwp/cpp-ref-for-winrt/non-agile)
* [winrt::Windows::Foundation::IUnknown:: 함수](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)
* [winrt::Windows::Foundation::IUnknown::try_as function](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)

## <a name="related-topics"></a>관련 항목

* [이해 하 고 COM 스레딩 모델을 사용 하 여](https://msdn.microsoft.com/library/ms809971)
