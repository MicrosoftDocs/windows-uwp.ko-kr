---
description: 이 항목에서는 **winrt::implements** 기본 구조체를 직/간접적으로 사용하여 C++/WinRT API를 작성하는 방법에 대해서 설명합니다.
title: C++/WinRT를 통한 API 작성
ms.date: 01/10/2019
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션된, 프로젝션, 구현체, 구현, 런타임 클래스, 활성화
ms.localizationpriority: medium
ms.openlocfilehash: e4ca6946df327dbe6697a71d1050e6401ed531fe
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57626668"
---
# <a name="author-apis-with-cwinrt"></a>C++/WinRT를 통한 API 작성

이 항목에서는 작성 하는 방법을 보여 줍니다 [C + + /cli WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) Api를 사용 하 여 합니다 [ **winrt::implements** ](/uwp/cpp-ref-for-winrt/implements) 직접 또는 간접적으로 구조체를 기본입니다. 이번 문서의 맥락에 따라 *작성*이라는 표현은 *생성* 또는 *구현*과 동의어로 사용됩니다. 이번 항목에서는 다음 시나리오의 순서대로 C++/WinRT 형식으로 API를 구현하는 방법에 대해서 설명합니다.

- Windows 런타임 클래스(이하 런타임 클래스)를 *작성하지 않습니다*. 단지 앱에서 로컬 사용을 위해 Windows 런타임 인터페이스를 1개 이상 구현하려고 합니다. 이 경우에는 **winrt::implements**에서 직접 파생시켜 함수를 구현합니다.
- 런타임 클래스를 *작성합니다*. 앱에서 사용할 구성 요소를 작성할 수도 있습니다. 혹은 XAML 사용자 인터페이스(UI)에서 사용할 형식을 작성할 수도 있습니다. 이 경우에는 동일한 컴파일 단위 내에서 런타임 클래스를 구현하고 사용하게 됩니다. 어쨌든 두 경우 모두 도구를 사용해 **winrt::implements**에서 파생되는 클래스를 생성할 수 있습니다.

위의 두 시나리오에서 C++/WinRT API를 구현하는 형식을 *구현체 형식*이라고 부릅니다.

> [!IMPORTANT]
> 구현체 형식과 프로젝션된 형식의 개념을 서로 구분할 수 있어야 합니다. 프로젝션된 형식은 [C++/WinRT를 통한 API 사용](consume-apis.md)에 설명되어 있습니다.

## <a name="if-youre-not-authoring-a-runtime-class"></a>런타임 클래스를 *작성하지 않는* 경우
가장 간단한 시나리오는 로컬 사용을 목적으로 Windows 런타임 인터페이스를 구현하는 경우입니다. 이때는 런타임 클래스는 필요 없고 일반적인 C++ 클래스만 있으면 됩니다. 예를 들어, [**CoreApplication**](/uwp/api/windows.applicationmodel.core.coreapplication)을 기반으로 앱을 개발할 수도 있습니다.

> [!NOTE]
> 설치 및 C +를 사용 하는 방법에 대 한 정보에 대 한 + WinRT VSIX Visual Studio Extension () (지원을 제공 하는 프로젝트 템플릿)를 참조 하세요 [Visual Studio 지원 C + + /cli WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)합니다.

Visual Studio에서의 **Visual c + +** > **Windows 유니버설** > **Core 앱 (C + + /cli WinRT)** 프로젝트 템플릿에 보여줍니다**CoreApplication** 패턴입니다. 이 패턴은 [**Windows::ApplicationModel::Core::IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) 구현체를 [**CoreApplication::Run**](/uwp/api/windows.applicationmodel.core.coreapplication.run)으로 전달하는 것부터 시작됩니다.

```cppwinrt
using namespace Windows::ApplicationModel::Core;
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    IFrameworkViewSource source = ...
    CoreApplication::Run(source);
}
```

**CoreApplication**은 인터페이스를 사용하여 앱의 첫 번째 보기를 만듭니다. 개념적으로 **IFrameworkViewSource**가 다음과 같이 표시됩니다.

```cppwinrt
struct IFrameworkViewSource : IInspectable
{
    IFrameworkView CreateView();
};
```

한 번 더 개념적으로 **CoreApplication::Run** 구현체가 다음과 같이 표시됩니다.

```cppwinrt
void Run(IFrameworkViewSource viewSource) const
{
    IFrameworkView view = viewSource.CreateView();
    ...
}
```

따라서 개발자는 **IFrameworkViewSource** 인터페이스를 구현합니다. C++/WinRT는 기본 구조체 템플릿인 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements)가 있기 때문에 COM 스타일 프로그래밍을 이용하지 않고도 인터페이스(들)를 쉽게 구현할 수 있습니다. 간단하게 **implements**에서 형식을 파생시킨 후 인터페이스의 함수를 구현하면 됩니다. 다음과 같이 하세요.

```cppwinrt
// App.cpp
...
struct App : implements<App, IFrameworkViewSource>
{
    IFrameworkView CreateView()
    {
        return ...
    }
}
...
```

**IFrameworkViewSource**를 이용하면 됩니다. 다음은 **IFrameworkView** 인터페이스를 구현할 개체를 반환하는 단계입니다. 마찬가지로 **App**에서 인터페이스를 구현하도록 선택할 수 있습니다. 아래 코드 예제는 데스크톱에서 실행 중인 창을 가져오는 최소 앱을 나타냅니다.

```cppwinrt
// App.cpp
...
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    IFrameworkView CreateView()
    {
        return *this;
    }

    void Initialize(CoreApplicationView const &) {}

    void Load(hstring const&) {}

    void Run()
    {
        CoreWindow window = CoreWindow::GetForCurrentThread();
        window.Activate();

        CoreDispatcher dispatcher = window.Dispatcher();
        dispatcher.ProcessEvents(CoreProcessEventsOption::ProcessUntilQuit);
    }

    void SetWindow(CoreWindow const & window)
    {
        // Prepare app visuals here
    }

    void Uninitialize() {}
};
...
```

때문에 **앱** 형식 *되는* **IFrameworkViewSource**, 하나를 전달 하면 **실행**.

```cppwinrt
using namespace Windows::ApplicationModel::Core;
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    CoreApplication::Run(App{});
}
```

## <a name="if-youre-authoring-a-runtime-class-in-a-windows-runtime-component"></a>Windows 런타임 구성 요소에서 런타임 클래스를 작성하는 경우
형식이 응용 프로그램에서 사용할 수 있도록 Windows 런타임 구성 요소에 패키지 형태로 존재하는 경우에는 런타임 클래스 형식이어야 합니다.

> [!TIP]
> IDL 파일을 편집하는 경우 빌드 성능을 최적화하기 위해, 그리고 생성된 소스 코드 파일에 대한 IDL 파일 논리 대응을 위해 자신의 IDL(인터페이스 정의 언어)(.idl) 파일에서 각 런타임 클래스를 선언하는 것이 좋습니다. Visual Studio는 모든 결과 `.winmd` 파일을 루트 네임스페이스가 같은 단일 파일로 병합합니다. 구성 요소 소비자가 해당 최종 `.winmd` 파일을 참조할 것입니다.

예를 들면 다음과 같습니다.

```idl
// MyRuntimeClass.idl
namespace MyProject
{
    runtimeclass MyRuntimeClass
    {
        // Declaring a constructor (or constructors) in the IDL causes the runtime class to be
        // activatable from outside the compilation unit.
        MyRuntimeClass();
        String Name;
    }
}
```

위의 IDL은 Windows 런타임(이하 런타임) 클래스를 선언합니다. 런타임 클래스란 일반적으로 실행 가능한 경계에서 최신 COM 인터페이스를 통해 활성화 및 사용되는 형식을 말합니다. IDL 파일을 프로젝트에 추가하여 빌드할 때는 C++/WinRT 도구 체인(`midl.exe` 및 `cppwinrt.exe`)이 사용자를 대신하여 구현체 형식을 생성합니다. 위의 IDL 예제를 보면 이름이 `\MyProject\MyProject\Generated Files\sources\MyRuntimeClass.h`와 `MyRuntimeClass.cpp`인 소스 코드 파일에서 **winrt::MyProject::implementation::MyRuntimeClass**라는 이름의 C++ 구조체 스텁이 구현체 형식에 해당합니다.

구현체 형식은 다음과 같은 모습입니다.

```cppwinrt
// MyRuntimeClass.h
...
namespace winrt::MyProject::implementation
{
    struct MyRuntimeClass : MyRuntimeClassT<MyRuntimeClass>
    {
        MyRuntimeClass() = default;

        hstring Name();
        void Name(hstring const& value);
    };
}

// winrt::MyProject::factory_implementation::MyRuntimeClass is here, too.
```

사용되는 F-바인딩 다형성 패턴에 주의하세요(**MyRuntimeClass**는 자신을 기본 클래스인 **MyRuntimeClassT**에 대한 템플릿 인수로 사용합니다). 이 패턴을 묘하게 되풀이되는 템플릿 패턴(CRTP)이라고도 부릅니다. 상향식 상속 체인을 따르는 경우에는 **MyRuntimeClass_base**에 이르게 됩니다.

```cppwinrt
template <typename D, typename... I>
struct MyRuntimeClass_base : implements<D, MyProject::IMyRuntimeClass, I...>
```

따라서 이번 시나리오에서는 상속 계층의 루트에 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) 기본 구조체 템플릿이 다시 한 번 위치합니다.

Windows 런타임 구성 요소의 API 작성에 대한 자세한 내용과 코드, 그리고 연습은 [C++/WinRT의 이벤트 작성](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component)을 참조하세요.

## <a name="if-youre-authoring-a-runtime-class-to-be-referenced-in-your-xaml-ui"></a>XAML UI에서 참조할 런타임 클래스르 작성하는 경우
형식이 XAML UI에서 참조되는 경우에는 XAML과 동일한 프로젝트에 위치하더라도 런타임 클래스 형식이어야 합니다. 일반적으로 실행 가능한 경계에서 활성화되기는 하지만 대신에 구현되는 컴파일 단위 내에서 런타임 클래스를 사용할 수도 있습니다.

이번 시나리오에서는 API를 작성하고 사용합니다. 런타임 클래스를 구현하는 절차는 기본적으로 Windows 런타임 구성 요소의 절차와 동일합니다. 따라서 이전 섹션인 [Windows 런타임 구성 요소에서 런타임 클래스를 작성하는 경우](#if-youre-authoring-a-runtime-class-in-a-windows-runtime-component)를 참조하세요. 유일하게 다른 점이라고 한다면 C++/WinRT 도구 체인이 IDL에서 구현체 형식 뿐만 아니라 프로젝션된 형식도 생성한다는 것입니다. 이번 시나리오에서 "**MyRuntimeClass**"만 언급하여 혼란스러울 수 있겠지만 이는 종류가 다르면서 이름만 같은 엔터티가 다수 있기 때문입니다.

- **MyRuntimeClass**는 런타임 클래스의 이름입니다. 하지만 이는 IDL에서 선언되고 일부 프로그래밍 언어에 구현되는 추상화입니다.
- **MyRuntimeClass**는 C++ 구조체이자 런타임 클래스의 C++/WinRT 구현체이기도 한 **winrt::MyProject::implementation::MyRuntimeClass**의 이름입니다. 앞에서 설명한 것처럼 구현하는 프로젝트와 사용하는 프로젝트가 따로 있다면 이 구조체는 구현하는 프로젝트에만 존재합니다. 이는 *구현체 형식*, 즉 *구현체*입니다. 이 형식은 `\MyProject\MyProject\Generated Files\sources\MyRuntimeClass.h` 파일과 `MyRuntimeClass.cpp` 파일에서 `cppwinrt.exe` 도구를 통해 생성됩니다.
- **MyRuntimeClass**는 C++ 구조체 **winrt::MyProject::MyRuntimeClass**의 형태로 프로젝션된 형식의 이름입니다. 구현하는 프로젝트와 사용하는 프로젝트가 따로 있다면 이 구조체는 사용하는 프로젝트에만 존재합니다. 이는 *프로젝션된 형식*, 즉 *프로젝션*입니다. 이 형식은 `\MyProject\MyProject\Generated Files\winrt\impl\MyProject.2.h` 파일에서 `cppwinrt.exe`를 통해 생성됩니다.

![프로젝션된 형식 및 구현 형식](images/myruntimeclass.png)

다음은 이번 항목과 관련하여 프로젝션된 형식의 일부입니다.

```cppwinrt
// MyProject.2.h
...
namespace winrt::MyProject
{
    struct MyRuntimeClass : MyProject::IMyRuntimeClass
    {
        MyRuntimeClass(std::nullptr_t) noexcept {}
        MyRuntimeClass();
    };
}
```

런타임 클래스에서 **INotifyPropertyChanged** 인터페이스를 구현하는 연습 예제는 [XAML 컨트롤, C++/WinRT 속성 바인딩](binding-property.md)을 참조하세요.

이번 시나리오에서 런타임 클래스를 사용하는 절차는 [C++/WinRT를 통한 API 사용](consume-apis.md#if-the-api-is-implemented-in-the-consuming-project)에 설명되어 있습니다.

## <a name="runtime-class-constructors"></a>런타임 클래스 생성자
다음은 위에서 언급한 내용 외에 주의해야 할 몇 가지 사항입니다.

- IDL에서 각 생성자를 생성하면 생성자가 구현체 형식과 프로젝션된 형식으로 생성됩니다. IDL에서 선언하는 생성자는 *다른* 컴파일 단위에서 런타임 클래스를 이용하는 데 사용됩니다.
- IDL에서 생성자를 선언하든 선언하지 않든 상관없이 `nullptr_t`을 가져오는 생성자 오버로드는 프로젝션된 형식으로 생성됩니다. `nullptr_t` 생성자의 호출은 *동일한* 컴파일 단위에서 런타임 클래스를 사용하는 *두 단계 중 첫 번째 단계*입니다. 자세한 내용과 코드 예제는 [C++/WinRT를 통한 API 사용](consume-apis.md#if-the-api-is-implemented-in-the-consuming-project)을 참조하세요.
- *동일한* 컴파일 단위에서 런타임 클래스를 사용하는 경우에는 비기본 생성자를 직접 구현체 형식(`MyRuntimeClass.h`)으로 구현할 수도 있습니다.

> [!NOTE]
> 런타임 클래스를 다른 컴파일 단위에서 사용하려면(일반적인 경우) 생성자(들)를 IDL에 추가하세요(기본 생성자는 1개 이상). 이렇게 하면 구현체 형식과 함께 팩터리 구현체도 가져오게 됩니다.
> 
> 동일한 컴파일 단위에서만 런타임 클래스를 작성하고 사용하려면 IDL에서 생성자를 선언하지 마세요. 팩터리 구현체도 필요하지 않기 때문에 생성되지 않습니다. 구현체 형식의 기본 생성자가 삭제되지만 간단히 편집하여 기본 생성자로 대신 사용할 수 있습니다.
> 
> 동일한 컴파일 단위에서만 런타임 클래스를 작성하고 사용하는 동시에 생성자 매개 변수까지 필요하다면 직접 구현체 형식으로 필요한 생성자(들)를 작성하세요.

## <a name="instantiating-and-returning-implementation-types-and-interfaces"></a>구현체 형식 및 인터페이스의 인스턴스화와 반환
이번 섹션에서는 [**IStringable**](/uwp/api/windows.foundation.istringable) 및 [**IClosable**](/uwp/api/windows.foundation.iclosable) 인터페이스를 구현하는 **MyType**이라는 이름의 구현체 형식을 예로 들겠습니다.

**MyType**은 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements)에서 직접 파생시킬 수 있습니다(런타임 클래스 아님).

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

struct MyType : implements<MyType, IStringable, IClosable>
{
    winrt::hstring ToString(){ ... }
    void Close(){}
};
```

혹은 IDL에서 생성하는 것도 가능합니다(런타임 클래스).

```idl
// MyType.idl
namespace MyProject
{
    runtimeclass MyType: Windows.Foundation.IStringable, Windows.Foundation.IClosable
    {
        MyType();
    }    
}
```

**MyType**에서 프로젝션 과정 중 사용하거나 반환할 수 있는 **IStringable** 또는 **IClosable** 개체로 이동하려면 [**winrt::make**](/uwp/cpp-ref-for-winrt/make) 함수 템플릿을 호출할 수 있습니다. **확인** 구현 형식의 기본 인터페이스를 반환 합니다.

```cppwinrt
IStringable istringable = winrt::make<MyType>();
```

> [!NOTE]
> 하지만 XAML UI에서 형식을 참조하는 경우에는 구현체 형식과 프로젝션된 형식이 모두 동일한 프로젝트에 위치합니다. 이 경우 **있도록** 프로젝션 형식의 인스턴스를 반환 합니다. 해당 시나리오의 코드 예제는 [XAML 컨트롤, C++/WinRT 속성 바인딩](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)을 참조하세요.

**IStringable** 인터페이스의 멤버를 호출할 때는 `istringable`(위의 코드 예제에서)만 사용할 수 있습니다. 하지만 C++/WinRT 인터페이스(프로젝션된 인터페이스)는 [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)에서 파생됩니다. 따라서 호출할 수 있습니다 [ **IUnknown::as** ](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) (또는 [ **IUnknown::try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function)) 다른 프로젝션 된 형식 또는 수 있는 인터페이스를 쿼리 하는 데 또한 사용 하거나 반환 합니다.

```cppwinrt
istringable.ToString();
IClosable iclosable = istringable.as<IClosable>();
iclosable.Close();
```

구현체의 모든 멤버에 액세스한 후 나중에 인터페이스를 호출자에게 반환해야 한다면 [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self) 함수 템플릿을 사용합니다. **make_self**는 구현체 형식을 래핑하는 [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr)을 반환합니다. 화살표 연산자를 사용하여 모든 인터페이스의 멤버에 액세스하거나, 호출자에게 있는 그대로 반환하거나, 혹은 **as**를 호출하여 그에 따른 인터페이스 개체를 호출자에게 반환할 수도 있습니다.

```cppwinrt
winrt::com_ptr<MyType> myimpl = winrt::make_self<MyType>();
myimpl->ToString();
myimpl->Close();
IClosable iclosable = myimpl.as<IClosable>();
iclosable.Close();
```

**MyType** 클래스는 구현체이기 때문에 프로젝션에 포함되지 않습니다. 하지만 이런 식으로 가상 함수를 호출하는 오버헤드 없이 구현체 메서드를 직접 호출할 수 있습니다. 위의 예제에서는 **MyType::ToString**이 **IStringable**에서 프로젝션된 메서드와 동일한 서명을 사용하지만 응용 프로그램 이진 인터페이스(ABI)를 거치지 않고 비가상 메서드를 직접 호출합니다. **com_ptr**은 포인터를 **MyType** 구조체에 고정시키기 때문에 사용자가 `myimpl` 변수 및 화살표 연산자를 통해 **MyType**의 다른 내부 세부 정보에 액세스할 수 있습니다.

인터페이스 개체를 하면 오류가 발생 하는 인터페이스를 구현에는 것을 사용 하 여 구현을 다시 가져올 수 있습니다 아는 경우에는 [ **winrt::get_self** ](/uwp/cpp-ref-for-winrt/get-self) 함수 템플릿입니다. 다시 말하지만 이는 가상 함수 호출을 피하여 구현체에 직접 이를 수 있는 기법입니다.

> [!NOTE]
> Windows SDK (Windows 10, 버전 1809) 10.0.17763.0 버전을 설치 하지 않은 경우 나중에 다음 호출 해야 [ **winrt::from_abi** ](/uwp/cpp-ref-for-winrt/from-abi) 대신 [ **winrt::get_ self**](/uwp/cpp-ref-for-winrt/get-self)합니다.

예를 들면 다음과 같습니다. 또 다른 예로 [구현 합니다 **BgLabelControl** 사용자 지정 컨트롤 클래스](xaml-cust-ctrl.md#implement-the-bglabelcontrol-custom-control-class)합니다.

```cppwinrt
void ImplFromIClosable(IClosable const& from)
{
    MyType* myimpl = winrt::get_self<MyType>(from);
    myimpl->ToString();
    myimpl->Close();
}
```

하지만 원래 인터페이스 개체만 참조를 계속 유지합니다. *사용자*가 참조를 계속 유지하려면 [**com_ptr::copy_from**](/uwp/cpp-ref-for-winrt/com-ptr#comptrcopyfrom-function)을 호출하면 됩니다.

```cppwinrt
winrt::com_ptr<MyType> impl;
impl.copy_from(winrt::get_self<MyType>(from));
// com_ptr::copy_from ensures that AddRef is called.
```

구현체 형식 자체는 **winrt::Windows::Foundation::IUnknown**에서 파생되지 않기 때문에 **as** 함수가 없습니다. 그렇더라도 하나를 인스턴스화하여 모든 인터페이스의 멤버에 액세스할 수는 있습니다. 단, 이 경우 원시 구현체 형식 인스턴스를 호출자에게 반환하지 마세요. 대신 위에서 언급한 기법 중 한 가지를 사용하여 프로젝션된 인터페이스, 즉 **com_ptr**을 반환하세요.

```cppwinrt
MyType myimpl;
myimpl.ToString();
myimpl.Close();
IClosable ic1 = myimpl.as<IClosable>(); // error
```

구현체 형식 인스턴스가 있을 때 이 인스턴스를 해당하는, 프로젝션된 형식이 필요한 함수에게 전달해야 한다면 그렇게 할 수 있습니다. 변환 연산자 구현 형식에 있는 (구현 형식에 의해 생성 된 것임을 `cppwinrt.exe` 도구)는 이러한 모든 효과 합니다. 프로젝션 된 형식에 해당 값을 예상 하는 메서드를 직접 구현 형식 값을 전달할 수 있습니다. 구현 형식 멤버 함수를 전달할 수 있습니다 `*this` 프로젝션 된 형식에 해당 값을 필요로 하는 방법입니다.

```cppwinrt
// MyProject::MyType is the projected type; the implementation type would be MyProject::implementation::MyType.

void MyOtherType::DoWork(MyProject::MyType const&){ ... }

...

void FreeFunction(MyProject::MyOtherType const& ot)
{
    MyType myimpl;
    ot.DoWork(myimpl);
}

...

void MyType::MemberFunction(MyProject::MyOtherType const& ot)
{
    ot.DoWork(*this);
}
```

## <a name="deriving-from-a-type-that-has-a-non-default-constructor"></a>기본이 아닌 생성자가 있는 형식에서 파생
[**ToggleButtonAutomationPeer::ToggleButtonAutomationPeer(ToggleButton)** ](/uwp/api/windows.ui.xaml.automation.peers.togglebuttonautomationpeer.-ctor#Windows_UI_Xaml_Automation_Peers_ToggleButtonAutomationPeer__ctor_Windows_UI_Xaml_Controls_Primitives_ToggleButton_) 은 기본값이 아닌 생성자의 예입니다. 기본 생성자가 없기 때문에 **ToggleButtonAutomationPeer**를 생성하려면 *소유자*를 전달해야 합니다. 결과적으로 **ToggleButtonAutomationPeer**에서 파생시키는 경우에는 *소유자*를 가져와 기본 클래스로 전달하는 생성자를 입력해야 합니다. 실제로 표시되는 모습은 다음과 같습니다.

```idl
// MySpecializedToggleButton.idl
namespace MyNamespace
{
    runtimeclass MySpecializedToggleButton :
        Windows.UI.Xaml.Controls.Primitives.ToggleButton
    {
        ...
    };
}
```

```idl
// MySpecializedToggleButtonAutomationPeer.idl
namespace MyNamespace
{
    runtimeclass MySpecializedToggleButtonAutomationPeer :
        Windows.UI.Xaml.Automation.Peers.ToggleButtonAutomationPeer
    {
        MySpecializedToggleButtonAutomationPeer(MySpecializedToggleButton owner);
    };
}
```

구현체 형식일 때 생성되는 생성자는 다음과 같습니다.

```cppwinrt
// MySpecializedToggleButtonAutomationPeer.cpp
...
MySpecializedToggleButtonAutomationPeer::MySpecializedToggleButtonAutomationPeer
    (MyNamespace::MySpecializedToggleButton const& owner)
{
    ...
}
...
```

여기에서 유일하게 빠진 것은 해당하는 생성자 매개 변수를 기본 클래스로 전달해야 하는 점입니다. 위에서 언급한 F-바인딩 다형성 패턴을 기억하고 있습니까? 일단 C++/WinRT에서 사용하는 해당 패턴의 세부 정보에 익숙해지면 기본 클래스의 이름을 알아볼 수 있습니다(혹은 구현체 클래스의 헤더 파일만 살펴봐도 됩니다). 다음은 이 경우에 기본 클래스 생성자를 호출하는 방법입니다.

```cppwinrt
// MySpecializedToggleButtonAutomationPeer.cpp
...
MySpecializedToggleButtonAutomationPeer::MySpecializedToggleButtonAutomationPeer
    (MyNamespace::MySpecializedToggleButton const& owner) : 
    MySpecializedToggleButtonAutomationPeerT<MySpecializedToggleButtonAutomationPeer>(owner)
{
    ...
}
...
```

기본 클래스 생성자는 **ToggleButton**이 필요합니다. 및 **MySpecializedToggleButton** *되는* **ToggleButton**합니다.

위에서 설명한 대로(생성자 매개 변수를 기본 클래스에게 전달) 편집할 때까지 컴파일러가 생성자를 플래그 처리하고 **MySpecializedToggleButtonAutomationPeer_base&lt;MySpecializedToggleButtonAutomationPeer&gt;** 라는 이름의 형식에 사용할 수 있는 기본 생성자가 없다고 알립니다. 하지만 실제로는 이 클래스가 구현체 형식의 기본 클래스입니다.

## <a name="important-apis"></a>중요 API
* [winrt::com_ptr 구조체 템플릿](/uwp/cpp-ref-for-winrt/com-ptr)
* [winrt::com_ptr::copy_from function](/uwp/cpp-ref-for-winrt/com-ptr#comptrcopyfrom-function)
* [winrt::from_abi 함수 템플릿](/uwp/cpp-ref-for-winrt/from-abi)
* [winrt::get_self 함수 템플릿](/uwp/cpp-ref-for-winrt/get-self)
* [winrt::implements 구조체 템플릿](/uwp/cpp-ref-for-winrt/implements)
* [winrt::make 함수 템플릿](/uwp/cpp-ref-for-winrt/make)
* [winrt::make_self 함수 템플릿](/uwp/cpp-ref-for-winrt/make-self)
* [winrt::Windows::Foundation::IUnknown:: 함수](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)

## <a name="related-topics"></a>관련 항목
* [사용 Api을 사용 하 여 C + + /cli WinRT](consume-apis.md)
* [XAML 컨트롤입니다. 바인딩할 C + + /cli WinRT 속성](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)
