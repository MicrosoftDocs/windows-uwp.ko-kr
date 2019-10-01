---
description: '**winrt::implements** 기본 구조체를 직접 또는 간접적으로 사용하여 C++/WinRT API를 작성하는 방법을 보여 줍니다.'
title: C++/WinRT를 사용하여 API 작성
ms.date: 07/08/2019
ms.topic: article
keywords: windows 10, uwp, 표준, c++, cpp, winrt, 프로젝션된, 프로젝션, 구현체, 구현, 런타임 클래스, 활성화
ms.localizationpriority: medium
ms.openlocfilehash: eba0e6312bc22153d8cb62eb97d32635184f0fdc
ms.sourcegitcommit: f34deba1d4460d85ed08fe9648999fe03ff6a3dd
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317118"
---
# <a name="author-apis-with-cwinrt"></a>C++/WinRT를 사용하여 API 작성

[**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) 기본 구조체를 직접 또는 간접적으로 사용하여 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) API를 작성하는 방법을 보여줍니다. 이 맥락에서 사용되는 *작성*이라는 표현은 *생성* 또는 *구현*과 동의어입니다. 이 토픽에서는 다음 시나리오의 순서대로 C++/WinRT 형식으로 API를 구현하는 방법을 설명합니다.

> [!NOTE]
> 이 항목에서는 Windows 런타임 구성 요소에 대해 다루지만 C++/WinRT 컨텍스트 내에서만 설명합니다. 모든 Windows 런타임 언어를 포함하는 Windows 런타임 구성 요소에 대한 콘텐츠를 원하는 경우 [Windows 런타임 구성 요소](/windows/uwp/winrt-components/)를 참조하세요.

- Windows 런타임 클래스(이하 런타임 클래스)를 작성하지 *않습니다*. 단지 앱 내에서 로컬로 사용할 수 있도록 Windows 런타임 인터페이스를 하나 이상 구현할 것입니다. 이 예제의 **winrt::implements**에서 직접 파생시켜 함수를 구현합니다.
- 런타임 클래스를 *작성*할 것입니다. 앱에서 사용할 구성 요소를 작성할 수도 있습니다. 혹은 XAML UI(사용자 인터페이스)에서 사용할 형식을 작성할 수도 있습니다. 이 경우에는 동일한 컴파일 단위 내에서 런타임 클래스를 구현하고 사용하게 됩니다. 어쨌든 두 경우 모두 도구를 사용해 **winrt::implements**에서 파생되는 클래스를 생성할 수 있습니다.

위의 두 시나리오에서 C++/WinRT API를 구현하는 형식을 *구현 형식*이라고 부릅니다.

> [!IMPORTANT]
> 구현 형식과 프로젝션된 형식의 개념을 구분할 수 있어야 합니다. 프로젝션된 형식은 [C++/WinRT를 통한 API 사용](consume-apis.md)에 설명되어 있습니다.

## <a name="if-youre-not-authoring-a-runtime-class"></a>런타임 클래스를 작성하지 *않는* 경우

가장 간단한 시나리오는 로컬에서 사용할 목적으로 Windows 런타임 인터페이스를 구현하는 경우입니다. 이 경우 런타임 클래스는 필요 없고 일반적인 C++ 클래스만 있으면 됩니다. 예를 들어 [**CoreApplication**](/uwp/api/windows.applicationmodel.core.coreapplication)을 기반으로 앱을 개발할 수 있습니다.

> [!NOTE]
> 프로젝트 템플릿 및 빌드 지원을 함께 제공하는 C++/WinRT VSIX(Visual Studio Extension) 및 NuGet 패키지를 설치하고 사용하는 방법에 대한 자세한 내용은 [Visual Studio의 C++/WinRT 지원](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)을 참조하세요.

Visual Studio에서 **Visual C++**  > **Windows 유니버설** > **Core App(C++/WinRT)** 프로젝트 템플릿은 **CoreApplication** 패턴을 보여줍니다. 이 패턴은 [**Windows::ApplicationModel::Core::IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) 구현을 [**CoreApplication::Run**](/uwp/api/windows.applicationmodel.core.coreapplication.run)으로 전달하는 것부터 시작됩니다.

```cppwinrt
using namespace Windows::ApplicationModel::Core;
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    IFrameworkViewSource source = ...
    CoreApplication::Run(source);
}
```

**CoreApplication**은 인터페이스를 사용하여 앱의 첫 번째 보기를 만듭니다. 개념적으로 **IFrameworkViewSource**는 다음과 같이 표시됩니다.

```cppwinrt
struct IFrameworkViewSource : IInspectable
{
    IFrameworkView CreateView();
};
```

역시 개념적으로 **CoreApplication::Run** 구현은 다음과 같이 표시됩니다.

```cppwinrt
void Run(IFrameworkViewSource viewSource) const
{
    IFrameworkView view = viewSource.CreateView();
    ...
}
```

따라서 개발자는 **IFrameworkViewSource** 인터페이스를 구현합니다. C++/WinRT에는 기본 구조체 템플릿인 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements)가 있기 때문에 COM 스타일 프로그래밍을 이용하지 않고도 하나 또는 여러 인터페이스를 쉽게 구현할 수 있습니다. 간단하게 **implements**에서 형식을 파생시킨 후 인터페이스의 함수를 구현하면 됩니다. 다음과 같이 하세요.

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

**IFrameworkViewSource**는 처리되었습니다. 다음 단계는 **IFrameworkView** 인터페이스를 구현하는 개체를 반환하는 것입니다. 마찬가지로 **App**에서 인터페이스를 구현하도록 선택할 수 있습니다. 다음 코드 예제는 적어도 데스크톱에서 앱을 실행할 최소한의 앱을 나타냅니다.

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

**App** 형식*이* **IFrameworkViewSource**이므로 하나만 **Run**으로 전달할 수 있습니다.

```cppwinrt
using namespace Windows::ApplicationModel::Core;
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    CoreApplication::Run(winrt::make<App>());
}
```

## <a name="if-youre-authoring-a-runtime-class-in-a-windows-runtime-component"></a>Windows 런타임 구성 요소에서 런타임 클래스를 작성하는 경우

형식이 애플리케이션에서 사용할 수 있도록 Windows 런타임 구성 요소에 패키지 형태로 존재하는 경우에는 런타임 클래스 형식이어야 합니다. Microsoft IDL(인터페이스 정의 언어)(.idl) 파일에서 런타임 클래스를 선언합니다([런타임 클래스를 Midl 파일(.idl)로 팩터링](#factoring-runtime-classes-into-midl-files-idl) 참조).

각 IDL 파일에 따라 `.winmd` 파일이 만들어지고, Visual Studio는 이러한 모든 파일을 루트 네임스페이스와 동일한 이름의 단일 파일로 병합합니다. 이 최종 `.winmd` 파일을 구성 요소 소비자가 참조할 것입니다.

다음은 IDL 파일에서 런타임 클래스를 선언하는 예제입니다.

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

이 IDL은 Windows 런타임(이하 런타임) 클래스를 선언합니다. 런타임 클래스는 일반적으로 실행 가능한 경계를 넘어서 최신 COM 인터페이스를 통해 활성화 및 사용할 수 있는 형식입니다. IDL 파일을 프로젝트에 추가하여 빌드할 때 C++/WinRT 도구 체인(`midl.exe` 및 `cppwinrt.exe`)이 사용자를 대신하여 구현 형식을 생성합니다. IDL 파일 워크플로 작업 예제는 [XAML 컨트롤, C++/WinRT 속성에 바인딩](binding-property.md)을 참조하세요.

위의 IDL 예제를 보면 이름이 `\MyProject\MyProject\Generated Files\sources\MyRuntimeClass.h`와 `MyRuntimeClass.cpp`인 소스 코드 파일에서 **winrt::MyProject::implementation::MyRuntimeClass**라는 이름의 C++ 구조체 스텁이 구현 형식에 해당합니다.

구현 형식은 다음과 같은 모습입니다.

```cppwinrt
// MyRuntimeClass.h
...
namespace winrt::MyProject::implementation
{
    struct MyRuntimeClass : MyRuntimeClassT<MyRuntimeClass>
    {
        MyRuntimeClass() = default;

        winrt::hstring Name();
        void Name(winrt::hstring const& value);
    };
}

// winrt::MyProject::factory_implementation::MyRuntimeClass is here, too.
```

사용되는 F-바인딩 다형성 패턴을 잘 보세요. **MyRuntimeClass**는 자신을 기본 클래스인 **MyRuntimeClassT**에 대한 템플릿 인수로 사용합니다. 이 패턴을 CRTP(Curiously Recurring Template Pattern)이라고도 합니다. 상향식 상속 체인을 따르는 경우에는 **MyRuntimeClass_base**에 이르게 됩니다.

```cppwinrt
template <typename D, typename... I>
struct MyRuntimeClass_base : implements<D, MyProject::IMyRuntimeClass, I...>
```

따라서 이 시나리오에서는 상속 계층의 루트에 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) 기본 구조체 템플릿이 다시 한 번 위치합니다.

Windows 런타임 구성 요소의 API 작성에 대한 자세한 내용과 코드 및 연습은 [C++/WinRT의 이벤트 작성](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component)을 참조하세요.

## <a name="if-youre-authoring-a-runtime-class-to-be-referenced-in-your-xaml-ui"></a>XAML UI에서 참조할 런타임 클래스를 작성하는 경우

형식을 XAML UI에서 참조하는 경우에는 XAML과 동일한 프로젝트에 위치하더라도 런타임 클래스 형식이어야 합니다. 일반적으로 실행 가능한 경계에서 활성화되기는 하지만, 대신에 구현되는 컴파일 단위 내에서 런타임 클래스를 사용할 수도 있습니다.

이 시나리오에서는 API를 작성*하고* 사용합니다. 런타임 클래스를 구현하는 절차는 기본적으로 Windows 런타임 구성 요소의 절차와 동일합니다. 따라서 이전 섹션&mdash;[Windows 런타임 구성 요소에서 런타임 클래스를 작성하는 경우](#if-youre-authoring-a-runtime-class-in-a-windows-runtime-component)를 참조하세요. 유일하게 다른 점이라고 한다면 C++/WinRT 도구 체인이 IDL에서 구현 형식뿐 아니라 프로젝션된 형식도 생성한다는 것입니다. 이 시나리오에서 "**MyRuntimeClass**"만 언급하여 혼란스러울 수 있겠지만, 이는 종류가 다르면서 이름만 같은 엔터티가 다수 있기 때문입니다.

- **MyRuntimeClass**는 런타임 클래스의 이름입니다. 하지만 이는 IDL에서 선언되고 일부 프로그래밍 언어에서 구현되는 추상화입니다.
- **MyRuntimeClass**는 C++ 구조체이자 런타임 클래스의 C++/WinRT 구현이기도 한 **winrt::MyProject::implementation::MyRuntimeClass**의 이름입니다. 앞서 설명했듯이, 구현하는 프로젝트와 사용하는 프로젝트가 따로 있다면 이 구조체는 구현하는 프로젝트에만 존재합니다. 이것이 *구현 형식* 또는 *구현*입니다. 이 형식은 `\MyProject\MyProject\Generated Files\sources\MyRuntimeClass.h` 파일과 `MyRuntimeClass.cpp` 파일에서 `cppwinrt.exe` 도구를 통해 생성됩니다.
- **MyRuntimeClass**는 C++ 구조체 **winrt::MyProject::MyRuntimeClass**의 형태로 프로젝션된 형식의 이름입니다. 구현하는 프로젝트와 사용하는 프로젝트가 따로 있다면 이 구조체는 사용하는 프로젝트에만 존재합니다. 이것이 *프로젝션된 형식* 또는 *프로젝션*입니다. 이 형식은 `\MyProject\MyProject\Generated Files\winrt\impl\MyProject.2.h` 파일에서 `cppwinrt.exe`를 통해 생성됩니다.

![프로젝션된 형식 및 구현 형식](images/myruntimeclass.png)

다음은 이 토픽과 관련된 프로젝션된 형식의 일부입니다.

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

이 시나리오에서 런타임 클래스를 사용하는 절차는 [C++/WinRT를 통한 API 사용](consume-apis.md#if-the-api-is-implemented-in-the-consuming-project)에 설명되어 있습니다.

## <a name="factoring-runtime-classes-into-midl-files-idl"></a>런타임 클래스를 Midl 파일(.idl)로 팩터링

Visual Studio 프로젝트 및 항목 템플릿은 각 런타임 클래스에 대해 별도의 IDL 파일을 생성합니다. 그러면 IDL 파일과 생성된 해당 소스 코드 파일이 논리적으로 일치합니다.

그러나 프로젝트의 모든 런타임 클래스를 단일 IDL 파일로 통합하면 빌드 시간이 크게 향상될 수 있습니다. 그렇지 않고 이러한 클래스 간에 복잡한(또는 순환) `import` 종속성이 있는 경우 실제로 통합이 필요할 수 있습니다. 또한 런타임 클래스가 함께 있으면 이를 작성하고 검토하는 것이 더 쉬울 수 있습니다.

## <a name="runtime-class-constructors"></a>런타임 클래스 생성자

다음은 위에서 언급한 내용 외에 주의해야 할 몇 가지 사항입니다.

- IDL에서 각 생성자를 생성하면 생성자가 구현 형식과 프로젝션된 형식으로 생성됩니다. IDL에서 선언하는 생성자는 *다른* 컴파일 단위에서 런타임 클래스를 이용하는 데 사용됩니다.
- IDL에서 생성자를 선언하든 선언하지 않든 상관없이, **std::nullptr_t**를 가져오는 생성자 오버로드는 프로젝션된 형식으로 생성됩니다. **std::nullptr_t** 생성자 호출은 *동일한* 컴파일 단위의 런타임 클래스를 사용하기 위한 *두 단계 중 첫 번째 단계*입니다. 자세한 내용과 코드 예제는 [C++/WinRT를 통한 API 사용](consume-apis.md#if-the-api-is-implemented-in-the-consuming-project)을 참조하세요.
- *동일한* 컴파일 단위에서 런타임 클래스를 사용하는 경우에는 비 기본 생성자를 직접 구현 형식(`MyRuntimeClass.h`)으로 구현할 수도 있습니다.

> [!NOTE]
> 런타임 클래스가 주로 다른 컴파일 단위에서 사용될 것으로 예상되면 생성자(최소한 기본 생성자)를 IDL에 포함하세요. 이렇게 하면 구현 형식과 함께 팩터리 구현도 가져오게 됩니다.
> 
> 동일한 컴파일 단위에서만 런타임 클래스를 작성하고 사용하려면 IDL에서 생성자를 선언하지 마세요. 팩터리 구현도 필요하지 않기 때문에 생성되지 않습니다. 구현 형식의 기본 생성자가 삭제되지만, 간단하게 편집하여 기본 생성자로 대신 사용할 수 있습니다.
> 
> 동일한 컴파일 단위에서만 런타임 클래스를 작성하여 사용할 계획이며, 생성자 매개 변수가 필요한 경우에는 직접 구현 형식으로 필요한 생성자를 작성하세요.

## <a name="runtime-class-methods-properties-and-events"></a>런타임 클래스 메서드, 속성 및 이벤트

IDL을 사용하여 런타임 클래스 및 해당 멤버를 선언하는 워크플로를 살펴보았으며, 그 다음은 도구에서 프로토타입 및 스텁 구현을 자동으로 생성합니다. 런타임 클래스의 멤버에 대해 자동 생성된 이러한 프로토타입의 경우 IDL에서 선언된 형식과 다른 형식을 전달할 수 있도록 편집*할 수 있습니다*. 하지만 그렇게 할 수 있는 것은 IDL에서 선언된 형식을 구현된 버전에서 선언된 형식으로 전달할 수 있을 때만 가능합니다.

예를 들면 다음과 같습니다.

- 매개 변수 형식을 완화할 수 있습니다. 예를 들어 IDL에서 메서드가 **SomeClass**를 가져오는 경우 구현에서 이를 **IInspectable**로 변경하도록 선택할 수 있습니다. 이는 모든 **SomeClass**를 **IInspectable**로 전달할 수 있기 때문에 가능합니다(물론 반대로는 작동하지 않습니다).
- 참조 대신 값으로 복사 가능한 매개 변수를 허용할 수 있습니다. 예를 들어 `SomeClass const&`를 `SomeClass const&`로 변경할 수 있습니다. 코루틴에 대한 참조를 캡처하지 못하도록 해야 할 때 필요합니다([매개 변수 전달](/windows/uwp/cpp-and-winrt-apis/concurrency#parameter-passing) 참조).
- 반환 값을 완화할 수 있습니다. 예를 들어 **void**를 [**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget)으로 변경할 수 있습니다.

마지막 둘은 비동기 이벤트 처리기를 작성할 때 매우 유용합니다.

## <a name="instantiating-and-returning-implementation-types-and-interfaces"></a>구현 형식과 인터페이스의 인스턴스화 및 반환

이 섹션에서는 [**IStringable**](/uwp/api/windows.foundation.istringable) 및 [**IClosable**](/uwp/api/windows.foundation.iclosable) 인터페이스를 구현하는 **MyType**이라는 이름의 구현 형식을 예로 들겠습니다.

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

또는 IDL에서 생성하는 것도 가능합니다(이것은 런타임 클래스).

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

**MyType**에서 프로젝션 과정 중 사용하거나 반환할 수 있는 **IStringable** 또는 **IClosable** 개체로 이동하려면 [**winrt::make**](/uwp/cpp-ref-for-winrt/make) 함수 템플릿을 호출해야 합니다. **make**는 구현 형식의 기본 인터페이스를 반환합니다.

```cppwinrt
IStringable istringable = winrt::make<MyType>();
```

> [!NOTE]
> 하지만 XAML UI에서 형식을 참조하는 경우에는 구현 형식과 프로젝션된 형식이 모두 동일한 프로젝트에 위치합니다. 이때는 **make**가 프로젝션된 형식 인스턴스를 반환합니다. 해당 시나리오의 코드 예제는 [XAML 컨트롤, C++/WinRT 속성 바인딩](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)을 참조하세요.

**IStringable** 인터페이스의 멤버를 호출할 때는 위 코드 예제의 `istringable`만 사용할 수 있습니다. 하지만 C++/WinRT 인터페이스(프로젝션된 인터페이스)는 [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)에서 파생됩니다. 따라서 [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)(또는 [**IUnknown::try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function))를 호출하여 다른 프로젝션된 형식 또는 인터페이스를 쿼리할 수 있으며, 이렇게 하여 얻은 결과를 사용하거나 반환할 수 있습니다.

```cppwinrt
istringable.ToString();
IClosable iclosable = istringable.as<IClosable>();
iclosable.Close();
```

구현의 모든 멤버에 액세스한 후 나중에 인터페이스를 호출자에게 반환해야 한다면 [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self) 함수 템플릿을 사용합니다. **make_self**는 구현 형식을 래핑하는 [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr)을 반환합니다. 화살표 연산자를 사용하여 모든 인터페이스의 멤버에 액세스하거나, 호출자에게 있는 그대로 반환하거나, **as**를 호출하여 그에 따른 인터페이스 개체를 호출자에게 반환할 수 있습니다.

```cppwinrt
winrt::com_ptr<MyType> myimpl = winrt::make_self<MyType>();
myimpl->ToString();
myimpl->Close();
IClosable iclosable = myimpl.as<IClosable>();
iclosable.Close();
```

**MyType** 클래스는 구현이므로 프로젝션에 포함되지 않습니다. 하지만 이런 식으로 가상 함수를 호출하는 오버헤드 없이 구현 메서드를 직접 호출할 수 있습니다. 위의 예제에서는 **MyType::ToString**이 **IStringable**에서 프로젝션된 메서드와 동일한 서명을 사용하지만, ABI(애플리케이션 이진 인터페이스)를 거치지 않고 비가상 메서드를 직접 호출합니다. **com_ptr**은 포인터를 **MyType** 구조체에 고정하기 때문에 사용자가 `myimpl` 변수 및 화살표 연산자를 통해 **MyType**의 다른 내부 세부 정보에 액세스할 수 있습니다.

인터페이스 개체가 있고, 이 개체가 구현의 인터페이스라는 사실을 알고 있는 경우에는 [**from_abi**](/uwp/cpp-ref-for-winrt/get-self) 함수 템플릿을 사용해 구현으로 돌아갈 수 있습니다. 다시 말하지만, 이는 가상 함수 호출을 피하여 구현에 직접 이를 수 있는 기법입니다.

> [!NOTE]
> Windows SDK 버전 10.0.17763.0(Windows 10 버전 1809) 이상을 설치하지 않은 경우 [**winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self) 대신 [**winrt::from_abi**](/uwp/cpp-ref-for-winrt/from-abi)를 호출해야 합니다.

예를 들면 다음과 같습니다. [**BgLabelControl** 사용자 지정 컨트롤 클래스 구현](xaml-cust-ctrl.md#implement-the-bglabelcontrol-custom-control-class)에 또 다른 예제가 있습니다.

```cppwinrt
void ImplFromIClosable(IClosable const& from)
{
    MyType* myimpl = winrt::get_self<MyType>(from);
    myimpl->ToString();
    myimpl->Close();
}
```

하지만 원래 인터페이스 개체만 참조를 계속 유지합니다. 참조를 계속 *유지*하려면 [**com_ptr::copy_from**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrcopy_from-function)을 호출하면 됩니다.

```cppwinrt
winrt::com_ptr<MyType> impl;
impl.copy_from(winrt::get_self<MyType>(from));
// com_ptr::copy_from ensures that AddRef is called.
```

구현 형식 자체는 **winrt::Windows::Foundation::IUnknown**에서 파생되지 않기 때문에 **as** 함수가 없습니다. 그렇더라도 하나를 인스턴스화하여 모든 인터페이스의 멤버에 액세스할 수 있습니다. 단, 이렇게 할 경우 원시 구현 형식 인스턴스를 호출자에게 반환하지 마세요. 대신 위에서 언급한 기법 중 한 가지를 사용하여 프로젝션된 인터페이스, 즉 **com_ptr**을 반환하세요.

```cppwinrt
MyType myimpl;
myimpl.ToString();
myimpl.Close();
IClosable ic1 = myimpl.as<IClosable>(); // error
```

구현 형식 인스턴스가 있고 이 인스턴스를 해당하는 프로젝션된 형식이 필요한 함수에 전달해야 하는 경우 그렇게 할 수 있습니다. 구현 형식에는 이것을 가능하게 하는 변환 연산자가 존재하기 때문입니다(단, 구현 형식이 `cppwinrt.exe` 도구에서 생성된 경우에 한해). 해당하는 프로젝션된 형식의 값이 필요한 메서드에 직접 구현 형식 값을 전달할 수 있습니다. 구현 형식 멤버 함수에서, 해당하는 프로젝션된 형식의 값이 필요한 메서드에 `*this`를 전달할 수 있습니다.

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

## <a name="deriving-from-a-type-that-has-a-non-default-constructor"></a>기본이 아닌 생성자를 갖고 있는 형식에서 파생

[**ToggleButtonAutomationPeer::ToggleButtonAutomationPeer(ToggleButton)** ](/uwp/api/windows.ui.xaml.automation.peers.togglebuttonautomationpeer.-ctor#Windows_UI_Xaml_Automation_Peers_ToggleButtonAutomationPeer__ctor_Windows_UI_Xaml_Controls_Primitives_ToggleButton_)는 기본이 아닌 생성자의 예입니다. 기본 생성자가 없기 때문에 **ToggleButtonAutomationPeer**를 생성하려면 *소유자*를 전달해야 합니다. 결과적으로 **ToggleButtonAutomationPeer**에서 파생시키는 경우에는 *소유자*를 가져와 기본 클래스로 전달하는 생성자를 입력해야 합니다. 실제로 표시되는 모습은 다음과 같습니다.

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

구현 형식일 때 생성되는 생성자는 다음과 같습니다.

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

여기서 유일하게 빠진 것은 해당하는 생성자 매개 변수를 기본 클래스로 전달해야 한다는 점입니다. 위에서 언급한 F-바인딩 다형성 패턴을 기억하시나요? 일단 C++/WinRT에서 사용하는 해당 패턴의 세부 정보에 익숙해지면 기본 클래스의 이름을 알아볼 수 있습니다. 또는 혹은 구현 클래스의 헤더 파일만 살펴봐도 됩니다. 다음은 이 상황에서 기본 클래스 생성자를 호출하는 방법입니다.

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

기본 클래스 생성자는 **ToggleButton**이 필요합니다. 그리고 **MySpecializedToggleButton** *은* **ToggleButton**입니다.

위에서 설명한 대로(생성자 매개 변수를 기본 클래스에게 전달) 편집할 때까지 컴파일러는 생성자를 플래그 처리하고 **MySpecializedToggleButtonAutomationPeer_base&lt;MySpecializedToggleButtonAutomationPeer&gt;** 라는 이름의 형식에 사용할 수 있는 기본 생성자가 없다고 알립니다. 하지만 실제로는 이 클래스가 구현 형식의 기본 클래스입니다.

## <a name="namespaces-projected-types-implementation-types-and-factories"></a>네임스페이스: 프로젝션된 형식, 구현 형식 및 팩터리

이 항목에서 이전에 살펴본 대로 C++/WinRT 런타임 클래스는 둘 이상의 네임스페이스에 둘 이상의 C++ 클래스 형태로 존재합니다. 따라서 **MyRuntimeClass**라는 이름은 **winrt::MyProject** 네임스페이스와 **winrt::MyProject::implementation** 네임스페이스에서 의미가 서로 다릅니다. 현재 컨텍스트에서 사용 중인 네임스페이스를 확인한 다음, 다른 네임스페이스의 이름이 필요하면 네임스페이스 접두사를 사용합니다. 해당 네임스페이스에 대해 좀 더 자세히 살펴보겠습니다.

- **winrt::MyProject**. 이 네임스페이스는 프로젝션된 형식을 포함합니다. 프로젝션된 형식의 개체는 프록시로, 특히 지원 개체를 사용자 프로젝트에서 구현하거나 다른 컴파일 단위에서 구현할 수 있는 경우 해당 지원 개체에 대한 스마트 포인터입니다.
- **winrt::MyProject::implementation**. 이 네임스페이스는 구현 형식을 포함합니다. 구현 형식의 개체는 포인터가 아닌 값(전체 C++ 스택 개체)입니다. 구현 형식을 직접 생성하지 마세요. 대신, [**winrt::make**](/uwp/cpp-ref-for-winrt/make)를 호출하여 구현 형식을 템플릿 매개 변수로 전달하세요. 이 항목에서 이전에 **winrt::make** 작업 예제를 살펴보았으며 다른 예제는 [XAML 컨트롤, C++/WinRT 속성에 바인딩](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)에서 찾을 수 있습니다. [직접 할당 진단](/windows/uwp/cpp-and-winrt-apis/diag-direct-alloc)도 참조하세요.
- **winrt::MyProject::factory_implementation**. 이 네임스페이스는 팩터리를 포함합니다. 이 네임스페이스의 개체가 [**IActivationFactory**](/windows/win32/api/activation/nn-activation-iactivationfactory)를 지원합니다.

다음 표에서는 다른 컨텍스트에서 사용하는 데 필요한 최소 네임스페이스 자격을 보여 줍니다.

|컨텍스트의 네임스페이스|프로젝션된 형식 지정|구현 형식 지정|
|-|-|-|
|**winrt::MyProject**|`MyRuntimeClass`|`implementation::MyRuntimeClass`|
|**winrt::MyProject::implementation**|`MyProject::MyRuntimeClass`|`MyRuntimeClass`|

> [!IMPORTANT]
> 프로젝션된 형식을 구현에서 반환하려는 경우 `MyRuntimeClass myRuntimeClass;`를 작성하여 구현 형식을 인스턴스화하지 않도록 주의해야 합니다. 해당 시나리오에 대한 올바른 기술 및 코드는 이 항목의 [구현 형식과 인터페이스의 인스턴스화 및 반환](#instantiating-and-returning-implementation-types-and-interfaces) 섹션에 나와 있습니다.
>
> 해당 시나리오에서 `MyRuntimeClass myRuntimeClass;` 관련 문제는 스택에 **winrt::MyProject::implementation::MyRuntimeClass** 개체가 생성된다는 것입니다. 이 개체(구현 형식)는 몇 가지 측면에서 프로젝션된 형식처럼 동작하므로 동일한 방식으로 메서드를 호출할 수 있으며 프로젝션된 형식으로 변환됩니다. 하지만 범위를 벗어나면 표준 C++ 규칙에 따라 개체가 소멸합니다. 따라서 해당 개체에 프로젝션된 형식(스마트 포인터)을 반환한 경우 해당 포인터는 이제 현수 포인터입니다.
>
> 이러한 메모리 손상 버그 유형은 진단하기 어렵습니다. 따라서 디버그 빌드에서 C++WinRT 어설션을 사용하면 스택 감지기를 사용하여 이러한 실수를 잡아내는 데 도움이 됩니다. 하지만 코루틴이 힙에 할당되므로 코루틴 내에서 실수가 발생하면 잡아낼 수 없습니다. 자세한 내용은 [직접 할당 진단](/windows/uwp/cpp-and-winrt-apis/diag-direct-alloc)을 참조하세요.

## <a name="using-projected-types-and-implementation-types-with-various-cwinrt-features"></a>다양한 C++/WinRT 기능에서 프로젝션된 형식 및 구현 형식 사용

C++/WinRT 기능에서 형식을 사용하는 다양한 위치 및 필요한 형식의 종류(프로젝션된 형식, 구현 형식 또는 둘 다)는 다음과 같습니다.

|기능|허용|참고|
|-|-|-|
|`T`(스마트 포인터를 나타냄)|프로젝션|실수로 구현 형식을 사용한 경우에 대해서는 [네임스페이스: 프로젝션된 형식, 구현 형식 및 팩터리](#namespaces-projected-types-implementation-types-and-factories)의 주의 사항을 참조하세요.|
|`agile_ref<T>`|둘 다|구현 형식을 사용할 경우 생성자 인수는 `com_ptr<T>`여야 합니다.|
|`com_ptr<T>`|구현|프로젝션된 형식을 사용하면 `'Release' is not a member of 'T'` 오류가 생성됩니다.|
|`default_interface<T>`|둘 다|구현 형식을 사용하면 첫 번째 구현된 인터페이스가 반환됩니다.|
|`get_self<T>`|구현|프로젝션된 형식을 사용하면 `'_abi_TrustLevel': is not a member of 'T'` 오류가 생성됩니다.|
|`guid_of<T>()`|둘 다|기본 인터페이스의 GUID를 반환합니다.|
|`IWinRTTemplateInterface<T>`<br>|프로젝션|실수로 구현 형식을 사용하여 컴파일한 경우 [네임스페이스: 프로젝션된 형식, 구현 형식 및 팩터리](#namespaces-projected-types-implementation-types-and-factories)의 주의 사항을 참조하세요.|
|`make<T>`|구현|프로젝션된 형식을 사용하면 `'implements_type': is not a member of any direct or indirect base class of 'T'` 오류가 생성됩니다.|
| `make_agile(T const&amp;)`|둘 다|구현 형식을 사용할 경우 인수는 `com_ptr<T>`여야 합니다.|
| `make_self<T>`|구현|프로젝션된 형식을 사용하면 `'Release': is not a member of any direct or indirect base class of 'T'` 오류가 생성됩니다.|
| `name_of<T>`|프로젝션|구현 형식을 사용하는 경우 기본 인터페이스의 stringified GUID를 가져올 수 있습니다.|
| `weak_ref<T>`|둘 다|구현 형식을 사용할 경우 생성자 인수는 `com_ptr<T>`여야 합니다.|

## <a name="opt-in-to-uniform-construction-and-direct-implementation-access"></a>균일한 생성 및 직접 구현 액세스 옵트인

이 섹션에서는 새 프로젝트에서 기본적으로 사용하도록 설정되어 있지만 옵트인할 수 있는 C++/WinRT 2.0 기능에 대해 설명합니다. 기존 프로젝트의 경우 `cppwinrt.exe` 도구를 구성하여 옵트인해야 합니다. Visual Studio에서 프로젝트 속성 **공용 속성** > **C++/WinRT** > **최적화됨**을 *예*로 설정합니다. 이렇게 하면 `<CppWinRTOptimized>true</CppWinRTOptimized>`를 프로젝트 파일에 추가하는 효과가 있습니다. 그리고 명령줄에서 `cppwinrt.exe`를 호출할 때 스위치를 추가하는 것과 동일한 효과가 있습니다.

`-opt[imize]` 스위치를 사용하면 *균일한 생성*이 가능한 경우가 많습니다. 균일한(또는 *통합된*) 생성에서는 C++/WinRT 언어 프로젝션 자체를 사용하여 로더 문제 없이 효율적으로 구현 형식(애플리케이션에서 사용하도록 구성 요소에서 구현한 형식)을 만들고 사용합니다.

기능을 설명하기 전에 먼저 균일한 생성을 사용하지 *않는* 상황을 살펴보겠습니다. 설명하기 위해 다음 Windows 런타임 클래스 예제에서 시작하겠습니다.

```idl
// MyClass.idl
namespace MyProject
{
    runtimeclass MyClass
    {
        MyClass();
        void Method();
        static void StaticMethod();
    }
}
```

C++/WinRT 라이브러리 사용에 익숙한 C++ 개발자는 다음과 같은 클래스를 사용할 수도 있습니다.

```cppwinrt
using namespace winrt::MyProject;

MyClass c;
c.Method();
MyClass::StaticMethod();
```

그리고 표시된 사용 코드가 이 클래스를 구현하는 동일한 구성 요소 내에 있지 않으면 이 방법이 완벽하게 합리적입니다. 언어 프로젝션인 C++/WinRT는 ABI(Windows 런타임에서 정의하는 COM 기반 애플리케이션 이진 인터페이스) 개발자를 보호합니다. C++/WinRT는 구현에 직접 호출하지 않고 ABI를 통해 이동합니다.

따라서 **MyClass** 개체(`MyClass c;`)를 생성하는 코드 줄에서 C++/WinRT 프로젝션은 [**RoGetActivationFactory**](/windows/win32/api/roapi/nf-roapi-rogetactivationfactory)를 호출하여 클래스 또는 활성화 팩터리를 검색한 다음, 해당 팩터리를 사용하여 개체를 만듭니다. 마지막 줄도 마찬가지로 팩터리를 사용하여 정적 메서드 호출로 표시되는 항목을 만듭니다. 이 모든 작업을 수행하려면 클래스를 등록하고 모듈에서 [**DllGetActivationFactory**](/previous-versions/br205771(v=vs.85)) 진입점을 구현해야 합니다. C++/WinRT는 매우 빠른 팩터리 캐시를 사용하므로 이 구성 요소를 사용하는 애플리케이션에 문제가 발생하지 않습니다. 문제는 구성 요소 내에서 약간의 문제만 발생한다는 것입니다.

첫째, C++/WinRT 팩터리 캐시가 아무리 빠르더라도 **RoGetActivationFactory**(또는 팩터리 캐시를 통한 후속 호출)를 통해 호출하는 것은 구현에 직접 호출하는 것보다 항상 느립니다. **RoGetActivationFactory**, [**IActivationFactory::ActivateInstance**](/windows/win32/api/activation/nf-activation-iactivationfactory-activateinstance), [**QueryInterface**](/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(refiid_void))를 순서대로 호출하는 것은 분명히 로컬로 정의된 형식에 대해 C++ `new` 식을 사용하는 것만큼 효율적이지 않습니다. 결과적으로 숙련된 C++/WinRT 개발자는 개체를 구성 요소 내에 만들 때 [**winrt::make**](/uwp/cpp-ref-for-winrt/make) 또는 [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self) 도우미 함수를 사용하는 데 익숙합니다.

```cppwinrt
// MyClass c;
MyProject::MyClass c{ winrt::make<implementation::MyClass>() };
```

그러나 볼 수 있듯이 이 방법은 거의 편리하거나 간결하지도 않습니다. 도우미 함수를 사용하여 개체를 만들어야 하며, 구현 형식과 프로젝션된 형식을 명확히 구분해야 합니다.

둘째, 프로젝션을 사용하여 클래스를 만들면 해당 활성화 팩터리가 캐시됩니다. 일반적으로 이 작업을 수행하는 것이 좋습니다. 그러나 팩터리가 호출하는 동일한 모듈(DLL)에 있으면 DLL을 효과적으로 고정하여 언로딩을 방지했습니다. 대부분의 경우 이는 중요하지 않습니다. 그러나 일부 시스템 구성 요소는 언로딩을 *지원해야* 합니다.

이 경우 *균일한 생성*이라는 용어가 나타납니다. 만들기 코드가 클래스만 사용하는 프로젝트에 있는지 또는 클래스를 실제로 *구현*하는 프로젝트에 있는지 여부에 관계없이 자유롭게 동일한 구문을 사용하여 개체를 만들 수 있습니다.

```cppwinrt
// MyProject::MyClass c{ winrt::make<implementation::MyClass>() };
MyClass c;
```

`-opt[imize]` 스위치를 사용하여 구성 요소 프로젝트를 빌드하면 언어 프로젝션을 통한 호출이 구현 형식을 직접 만드는 **winrt::make** 함수에 대한 동일한 효율적인 호출로 압축됩니다. 이렇게 하면 구문을 간단하고 예측 가능하게 만들 수 있으므로 팩터리를 통해 호출할 때 성능 저하를 방지할 수 있고 프로세스에서 구성 요소를 고정할 필요가 없습니다. 또한 이는 구성 요소 프로젝트 외에도 XAML 애플리케이션에도 유용합니다. 동일한 애플리케이션에서 구현된 클래스에 대한 **RoGetActivationFactory**를 무시하면 구성 요소 외부에 있을 때와 동일한 방식으로(등록할 필요 없이) 해당 클래스를 생성할 수 있습니다.

균일한 생성은 팩터리 내부에서 제공하는 *모든* 호출에 적용됩니다. 실제로 이는 최적화에서 생성자와 정적 구성 요소를 모두 사용한다는 것을 의미합니다. 원래 예제는 다음과 같습니다.

```cppwinrt
MyClass c;
c.Method();
MyClass::StaticMethod();
```

`-opt[imize]`를 사용하지 않으면 첫 번째 및 마지막 명령문에서 팩터리 개체를 통해 호출해야 합니다. `-opt[imize]`를 *사용하면* 둘 모두에서 이 작업을 수행하지 않습니다. 그리고 이러한 호출은 구현에 대해 직접 컴파일되며, 심지어 인라인될 수도 있습니다. 이는 `-opt[imize]`, 즉 *직접 구현* 액세스에 대해 언급할 때 자주 사용되는 다른 용어와 관련이 있습니다.

언어 프로젝션이 편리하지만, 구현에 직접 액세스할 수 있는 경우 이를 활용하여 가장 효율적인 코드를 만들 수 있습니다. C++/WinRT를 사용하면 프로젝션의 안전성과 생산성을 그대로 유지하도록 강요하지 않고 이 작업을 수행할 수 있습니다.

이는 언어 프로젝션에서 해당 구현 형식에 연결하여 직접 액세스할 수 있도록 구성 요소를 상호 운용해야 하므로 주요 변경 내용입니다. C++/WinRT는 헤더 전용 라이브러리이므로 내부를 조사하여 진행 상황을 확인할 수 있습니다. `-opt[imize]`를 사용하지 않으면 다음과 같은 프로젝션에서 **MyClass** 생성자 및  **StaticMethod** 멤버를 정의합니다.

```cppwinrt
namespace winrt::MyProject
{
    inline MyClass::MyClass() :
        MyClass(impl::call_factory<MyClass>([](auto&& f){
            return f.template ActivateInstance<MyClass>(); }))
    {
    }
    inline void MyClass::StaticMethod()
    {
        impl::call_factory<MyClass, MyProject::IClassStatics>([&](auto&& f) {
            return f.StaticMethod(); });
    }
}
```

위의 모든 작업을 반드시 수행할 필요는 없습니다. 이는 두 호출에서 모두 **call_factory**라는 함수에 대한 호출이 필요하다는 것을 보여 주기 위한 것입니다. 이러한 호출은 팩터리 캐시를 포함하고, 구현에 직접 액세스하는 것이 아닙니다. `-opt[imize]`를 *사용하면* 이러한 동일한 함수가 전혀 정의되지 않습니다. 대신, 이러한 함수는 프로젝션에서 선언하고, 해당 정의는 구성 요소에 남아 있습니다.

그러면 구성 요소에서 구현에 직접 호출하는 정의를 제공할 수 있습니다. 이제 주요 변경 내용에 도달했습니다. 이러한 정의는 `-component` 및 `-opt[imize]`를 모두 사용할 때 생성되며 `Type.g.cpp`라는 파일에 나타납니다. 여기서 *Type*은 구현되는 런타임 클래스의 이름입니다. 따라서 기존 프로젝트에서 `-opt[imize]`를 사용하도록 처음 설정하는 경우에는 다양한 링커 오류가 발생할 수 있습니다. 작업을 수행할 수 있도록 생성된 파일을 구현에 포함시켜야 합니다.

예제에서는 `MyClass.h`가 `-opt[imize]`의 사용 여부에 관계없이 다음과 같을 수 있습니다.

```cppwinrt
// MyClass.h
#pragma once
#include "MyClass.g.h"
 
namespace winrt::MyProject::implementation
{
    struct MyClass : ClassT<MyClass>
    {
        MyClass() = default;
 
        static void StaticMethod();
        void Method();
    };
}
namespace winrt::MyProject::factory_implementation
{
    struct MyClass : ClassT<MyClass, implementation::MyClass>
    {
    };
}
```

`MyClass.cpp`에는 모든 것이 함께 제공됩니다.

```cppwinrt
#include "pch.h"
#include "MyClass.h"
#include "MyClass.g.cpp" // !!It's important that you add this line!!
 
namespace winrt::MyProject::implementation
{
    void MyClass::StaticMethod()
    {
    }
 
    void MyClass::Method()
    {
    }
}
```

따라서 기존 프로젝트에서 균일한 생성을 사용하려면 구현 클래스에 대한 포함(및 정의) 뒤에 `#include <Sub/Namespace/Type.g.cpp>`가 나오도록 각 구현의 `.cpp` 파일을 편집해야 합니다. 이 파일은 프로젝션이 정의되지 않은 상태로 있는 함수에 대한 정의를 제공합니다. `MyClass.g.cpp` 파일 내에서 이러한 정의는 다음과 같습니다.

```cppwinrt
namespace winrt::MyProject
{
    MyClass::MyClass() :
        MyClass(make<MyProject::implementation::MyClass>())
    {
    }
    void MyClass::StaticMethod()
    {
        return MyProject::implementation::MyClass::StaticMethod();
    }
}
```

그리고 이를 통해 구현에 대한 효율적인 직접 호출로 프로젝션을 완벽하게 완료하고, 팩터리 캐시에 대한 호출을 방지하고, 링커를 만족시킵니다.

`-opt[imize]`에서 수행하는 마지막 작업은 C++/WinRT 1.0에 필요한 강력한 형식 결합을 제거하여 증분 빌드가 훨씬 더 빠르게 수행되는 방식으로 프로젝트의 `module.g.cpp`(DLL의 **DllGetActivationFactory** 및 **DllCanUnloadNow** 내보내기를 구현하는 데 유용한 파일) 구현을 변경하는 것입니다. 이를 종종 *형식이 지워진 팩터리*라고 합니다. `-opt[imize]`를 사용하지 않으면 구성 요소에 대해 생성된 `module.g.cpp` 파일은 모든 구현 클래스(이 예제에서는 `MyClass.h`)의 정의를 포함하여 시작됩니다. 그러면 다음과 같이 각 클래스에 대한 구현 팩터리를 직접 만듭니다.

```cppwinrt
if (requal(name, L"MyProject.MyClass"))
{
    return winrt::detach_abi(winrt::make<winrt::MyProject::factory_implementation::MyClass>());
}
```

다시 말하지만, 모든 세부 정보를 따를 필요는 없습니다. 여기서 확인할 수 있는 유용한 것은 구성 요소에서 구현한 모든 클래스에 대한 전체 정의가 필요하다는 것입니다. 이 경우 단일 구현을 변경하면 `module.g.cpp`가 다시 컴파일되므로 내부 루프에 큰 영향을 줄 수 있습니다. `-opt[imize]`에서는 더 이상 그렇지 않습니다. 대신 생성된 `module.g.cpp` 파일에 두 가지 상황이 발생합니다. 첫 번째는 구현 클래스를 더 이상 포함하지 않는다는 것입니다. 다음 예제에서는 `MyClass.h`를 전혀 포함하지 않습니다. 대신 해당 구현에 대한 지식 없이 구현 팩터리를 만듭니다.

```cppwinrt
void* winrt_make_MyProject_MyClass();
 
if (requal(name, L"MyProject.MyClass"))
{
    return winrt_make_MyProject_MyClass();
}
```

분명히 이러한 정의를 포함할 필요가 없으며, 링커를 통해 **winrt_make_Component_Class** 함수의 정의를 확인합니다. 물론, 사용자를 위해 생성된 `MyClass.g.cpp` 파일(및 이전에 균일한 생성을 지원하기 위해 포함된 파일)도 이 함수를 정의하므로 이에 대해 생각할 필요가 없습니다. 이 예제에서 생성된 `MyClass.g.cpp` 파일의 전체 내용은 다음과 같습니다.

```cppwinrt
void* winrt_make_MyProject_MyClass()
{
    return winrt::detach_abi(winrt::make<winrt::MyProject::factory_implementation::MyClass>());
}
namespace winrt::MyProject
{
    MyClass::MyClass() :
        MyClass(make<MyProject::implementation::MyClass>())
    {
    }
    void MyClass::StaticMethod()
    {
        return MyProject::implementation::MyClass::StaticMethod();
    }
}
```

여기서 볼 수 있듯이 **winrt_make_MyProject_MyClass** 함수는 구현의 팩터리를 직접 만듭니다. 이렇게 하면 지정된 구현을 모두 만족스럽게 변경할 수 있으며, `module.g.cpp`를 다시 컴파일할 필요가 전혀 없습니다. Windows 런타임 클래스를 추가하거나 제거하는 경우에만 `module.g.cpp`를 업데이트하고 다시 컴파일해야 합니다.

## <a name="overriding-base-class-virtual-methods"></a>기본 클래스의 가상 메서드 재정의

기본 클래스와 파생 클래스는 모두 앱 정의 클래스이지만 가상 메서드가 최상위 Windows 런타임 클래스에 정의되는 경우 파생 클래스에 가상 메서드와 관련된 문제가 있을 수 있습니다. 실제로 XAML 클래스에서 파생되면 이 문제가 발생합니다. 이 섹션의 나머지 부분은 [Derived 클래스](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx#derived-classes)의 예제에서 계속 진행됩니다.

```cppwinrt
namespace winrt::MyNamespace::implementation
{
    struct BasePage : BasePageT<BasePage>
    {
        void OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
    };

    struct DerivedPage : DerivedPageT<DerivedPage>
    {
        void OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
    };
}
```

계층 구조는 [**Windows::UI::Xaml::Controls::Page**](/uwp/api/windows.ui.xaml.controls.page) \<- **BasePage** \<- **DerivedPage**입니다. **BasePage::OnNavigatedFrom** 메서드는 [**Page::OnNavigatedFrom**](/uwp/api/windows.ui.xaml.controls.page.onnavigatedfrom)을 올바르게 재정의하지만, **DerivedPage::OnNavigatedFrom**은 **BasePage::OnNavigatedFrom**을 재정의하지 않습니다.

여기서 **DerivedPage**는 **BasePage**에서 **IPageOverrides** vtable을 다시 사용합니다. 즉, **IPageOverrides::OnNavigatedFrom** 메서드를 재정의하지 못합니다. 하나의 잠재적인 해결 방법으로, **BasePage** 자체가 템플릿 클래스가 되고 헤더 파일에 해당 구현이 전적으로 포함되어야 합니다. 그러나 이는 받아들일 수 없을 정도로 복잡합니다.

이 문제를 해결하려면 기본 클래스에서 **OnNavigatedFrom** 메서드를 명시적으로 가상 메서드로 선언합니다. 이렇게 하면 **DerivedPage::IPageOverrides::OnNavigatedFrom**에 대한 vtable 항목에서 **BasePage::IPageOverrides::OnNavigatedFrom**을 호출할 때 생산자는 **BasePage::OnNavigatedFrom**을 호출하며, 이 경우 가상성(virtualness)으로 인해 결국에는 **DerivedPage::OnNavigatedFrom**을 호출합니다.

```cppwinrt
namespace winrt::MyNamespace::implementation
{
    struct BasePage : BasePageT<BasePage>
    {
        // Note the `virtual` keyword here.
        virtual void OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
    };

    struct DerivedPage : DerivedPageT<DerivedPage>
    {
        void OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
    };
}
```

이렇게 하려면 클래스 계층 구조의 모든 멤버가 **OnNavigatedFrom** 메서드의 반환 값 및 매개 변수 형식에 동의해야 합니다. 동의하지 않는 경우 위의 버전을 가상 메서드로 사용하고 대체 항목을 래핑해야 합니다.

## <a name="important-apis"></a>중요 API
* [winrt::com_ptr 구조체 템플릿](/uwp/cpp-ref-for-winrt/com-ptr)
* [winrt::com_ptr::copy_from 함수](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrcopy_from-function)
* [winrt::from_abi 함수 템플릿](/uwp/cpp-ref-for-winrt/from-abi)
* [winrt::get_self 함수 템플릿](/uwp/cpp-ref-for-winrt/get-self)
* [winrt::implements 구조체 템플릿](/uwp/cpp-ref-for-winrt/implements)
* [winrt::make 함수 템플릿](/uwp/cpp-ref-for-winrt/make)
* [winrt::make_self 함수 템플릿](/uwp/cpp-ref-for-winrt/make-self)
* [winrt::Windows::Foundation::IUnknown::as 함수](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)

## <a name="related-topics"></a>관련 항목
* [C++/WinRT를 통한 API 사용](consume-apis.md)
* [XAML 컨트롤(C++/WinRT 속성에 바인딩)](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)
