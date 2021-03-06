---
ms.assetid: 41E1B4F1-6CAF-4128-A61A-4E400B149011
title: 데이터 바인딩 심층 분석
description: UWP(유니버설 Windows 플랫폼) 애플리케이션의 데이터 바인딩을 사용하는 방법을 알아봅니다.
ms.date: 10/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: a4fc9244c84b955b925fef9c3527778bdb3ab2ea
ms.sourcegitcommit: 0c4bbaf1c119a84002748cdcf02e1449835559c3
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/16/2020
ms.locfileid: "92133066"
---
# <a name="data-binding-in-depth"></a>데이터 바인딩 심층 분석

**중요 API**

-   [ **{x:Bind} 태그 확장**](../xaml-platform/x-bind-markup-extension.md)
-   [**Binding 클래스**](/uwp/api/Windows.UI.Xaml.Data.Binding)
-   [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext)
-   [**INotifyPropertyChanged**](/uwp/api/Windows.UI.Xaml.Data.INotifyPropertyChanged)

> [!NOTE]
> 이 항목에서는 데이터 바인딩 기능에 대해 자세히 설명합니다. 간단하고 실용적인 소개는 [데이터 바인딩 개요](data-binding-quickstart.md)를 참조하세요.

이 토픽에서는 UWP(유니버설 Windows 플랫폼) 애플리케이션의 데이터 바인딩에 대해 설명합니다. 여기서 설명하는 API는 [**Windows.UI.Xaml.Data** 네임스페이스](/uwp/api/windows.ui.xaml.data)에 상주합니다.

데이터 바인딩은 앱의 UI에서 데이터를 표시하고 선택적으로 해당 데이터와 동기화된 상태를 유지하는 하나의 방법입니다. 데이터 바인딩은 데이터 문제를 UI 문제와 분리하여 개념 모델을 간소화하고 앱의 가독성, 테스트 용이성 및 유지 관리성을 향상시킬 수 있도록 해줍니다.

이러한 데이터 바인딩을 사용하여 UI가 처음 표시될 때 데이터 원본 값의 변경에 반응하지 않고 해당 값을 표시하기만 하도록 할 수 있습니다. 이것을 *일회성* 바인딩 모드라고 하며 런타임 중에 변경되지 않는 값에 대해 잘 작동합니다. 또는 값을 "관찰"하다가 값이 변경되면 UI를 업데이트하도록 선택할 수 있습니다. *단방향 바인딩*이라는 이 모드는 읽기 전용 데이터에 적합합니다. 마지막으로 사용자가 UI에서 값을 변경한 경우 해당 값이 데이터 원본에 자동으로 다시 적용되도록 관찰하고 업데이트할 수 있습니다. *양방향 바인딩*이라는 이 모드는 읽기-쓰기 데이터에 적합합니다. 예를 들면 다음과 같습니다.

-   일회성 바인딩 모드를 사용하여 [**Image**](/uwp/api/Windows.UI.Xaml.Controls.Image)를 현재 사용자의 사진에 바인딩할 수 있습니다.
-   단방향 바인딩 모드를 사용하여 [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView)를 신문 섹션별로 그룹화된 실시간 뉴스 기사 모음에 바인딩할 수 있습니다.
-   양방향 바인딩 모드를 사용하여 [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox)를 양식에 있는 고객의 이름에 바인딩할 수 있습니다.

모드에 따라 두 가지 종류의 바인딩이 있으며, 일반적으로 둘 다 UI 태그에 선언됩니다. [{x:Bind} 태그 확장](../xaml-platform/x-bind-markup-extension.md) 또는 [{Binding} 태그 확장](../xaml-platform/binding-markup-extension.md)을 사용하도록 선택할 수 있습니다. 동일한 앱에서 이 둘을 혼합하여 사용할 수도 있으며, 동일한 UI 요소에도 마찬가지입니다. {x:Bind}는 Windows 10의 새로운 기능으로, 향상된 성능을 제공합니다. 이 항목에 설명된 모든 세부 정보는 명시적으로 설명하지 않더라도 두 종류의 바인딩 모두에 적용됩니다.

**{x:Bind}를 보여주는 샘플 앱**

-   [{x:Bind} 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlBind)
-   [QuizGame](https://github.com/microsoft/Windows-appsample-networkhelper)
-   [XAML UI 기본 사항 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics)

**{Binding}을 보여주는 샘플 앱**

-   [Bookstore1](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore1Universal_10) 앱 다운로드
-   [Bookstore2](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore2Universal_10) 앱 다운로드

## <a name="every-binding-involves-these-pieces"></a>모든 바인딩에 포함된 요소

-   *바인딩 소스*. 바인딩할 데이터의 소스이며, 해당 값을 UI에 표시할 멤버가 있는 모든 클래스의 인스턴스일 수 있습니다.
-   *바인딩 대상*. 데이터를 표시하는 UI에서 [**FrameworkElement**](/uwp/api/Windows.UI.Xaml.FrameworkElement)의 [**DependencyProperty**](/uwp/api/Windows.UI.Xaml.DependencyProperty)입니다.
-   *바인딩 개체*. 소스에서 대상으로, 그리고 선택적으로 대상에서 다시 소스로 데이터 값을 전송하는 요소입니다. 바인딩 개체는 XAML 로드 시 [{x:Bind}](../xaml-platform/x-bind-markup-extension.md) 또는 [{Binding}](../xaml-platform/binding-markup-extension.md) 태그 확장에서 만들어집니다.

다음 섹션에서는 바인딩 소스, 바인딩 대상 및 바인딩 개체에 대해 자세히 알아봅니다. 또한 단추의 콘텐츠를 **HostViewModel**이라는 클래스에 속한 **NextButtonText**라는 문자열 속성에 바인딩하는 예제를 이러한 섹션과 함께 링크합니다.

### <a name="binding-source"></a>바인딩 소스

다음은 바인딩 소스로 사용할 수 있는 클래스의 매우 기본적인 구현입니다.

[C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md)를 사용하는 경우 프로젝트에 새 **Midl 파일(.idl)** 항목을 추가하고 아래의 C++/WinRT 코드 예제 목록과 같이 이름을 지정합니다. 새 파일의 내용을 목록에 표시된 [MIDL 3.0](/uwp/midl-3/intro) 코드로 바꾸고, 프로젝트를 빌드하여 `HostViewModel.h` 및 `.cpp`를 생성한 다음, 생성된 파일에 코드를 추가하여 목록과 일치시킵니다. 생성된 파일 및 해당 파일을 프로젝트에 복사하는 방법에 대한 자세한 내용은 [XAML 컨트롤, C++/WinRT 속성에 바인딩](../cpp-and-winrt-apis/binding-property.md)을 참조하세요.

```csharp
public class HostViewModel
{
    public HostViewModel()
    {
        this.NextButtonText = "Next";
    }

    public string NextButtonText { get; set; }
}
```

```cppwinrt
// HostViewModel.idl
namespace DataBindingInDepth
{
    runtimeclass HostViewModel
    {
        HostViewModel();
        String NextButtonText;
    }
}

// HostViewModel.h
// Implement the constructor like this, and add this field:
...
HostViewModel() : m_nextButtonText{ L"Next" } {}
...
private:
    std::wstring m_nextButtonText;
...

// HostViewModel.cpp
// Implement like this:
...
hstring HostViewModel::NextButtonText()
{
    return hstring{ m_nextButtonText };
}

void HostViewModel::NextButtonText(hstring const& value)
{
    m_nextButtonText = value;
}
...
```

이 구현의 **HostViewModel**과 해당 속성인 **NextButtonText**는 일회성 바인딩에만 적합합니다. 그러나 단방향 및 양방향 바인딩은 매우 일반적이므로 이러한 종류의 바인딩에서는 바인딩 소스의 데이터 값 변경에 응답하여 UI가 자동으로 업데이트됩니다. 이러한 종류의 바인딩이 제대로 작동하려면 바인딩 개체에서 바인딩 소스를 "관찰할 수 있도록" 확인해야 합니다. 따라서 이 예제에서는 **NextButtonText** 속성에 단방향 또는 양방향 바인딩하려는 경우 해당 속성 값에 대해 런타임에 발생하는 모든 변경 내용을 바인딩 개체에서 관찰할 수 있도록 해야 합니다.

한 가지 방법은 바인딩 소스를 나타내는 클래스를 [**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject)에서 파생하고 [**DependencyProperty**](/uwp/api/Windows.UI.Xaml.DependencyProperty)를 통해 데이터 값을 노출하는 것입니다. 그러면 [**FrameworkElement**](/uwp/api/Windows.UI.Xaml.FrameworkElement)가 관찰 가능하게 됩니다. **FrameworkElements**는 즉시 사용할 수 있는 유용한 바인딩 소스입니다.

클래스를 관찰 가능하게 하고 이미 기본 클래스가 있는 클래스에 필요한 클래스로 만드는 보다 간단한 방법은[**System.ComponentModel.INotifyPropertyChanged**](/dotnet/api/system.componentmodel.inotifypropertychanged)를 구현하는 것입니다. 여기에는 실제로 **PropertyChanged**라는 단일 이벤트의 구현이 포함됩니다. **HostViewModel**을 사용하는 예제는 아래와 같습니다.

```csharp
...
using System.ComponentModel;
using System.Runtime.CompilerServices;
...
public class HostViewModel : INotifyPropertyChanged
{
    private string nextButtonText;

    public event PropertyChangedEventHandler PropertyChanged = delegate { };

    public HostViewModel()
    {
        this.NextButtonText = "Next";
    }

    public string NextButtonText
    {
        get { return this.nextButtonText; }
        set
        {
            this.nextButtonText = value;
            this.OnPropertyChanged();
        }
    }

    public void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        // Raise the PropertyChanged event, passing the name of the property whose value has changed.
        this.PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

```cppwinrt
// HostViewModel.idl
namespace DataBindingInDepth
{
    runtimeclass HostViewModel : Windows.UI.Xaml.Data.INotifyPropertyChanged
    {
        HostViewModel();
        String NextButtonText;
    }
}

// HostViewModel.h
// Add this field:
...
    winrt::event_token PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& handler);
    void PropertyChanged(winrt::event_token const& token) noexcept;

private:
    winrt::event<Windows::UI::Xaml::Data::PropertyChangedEventHandler> m_propertyChanged;
...

// HostViewModel.cpp
// Implement like this:
...
void HostViewModel::NextButtonText(hstring const& value)
{
    if (m_nextButtonText != value)
    {
        m_nextButtonText = value;
        m_propertyChanged(*this, Windows::UI::Xaml::Data::PropertyChangedEventArgs{ L"NextButtonText" });
    }
}

winrt::event_token HostViewModel::PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& handler)
{
    return m_propertyChanged.add(handler);
}

void HostViewModel::PropertyChanged(winrt::event_token const& token) noexcept
{
    m_propertyChanged.remove(token);
}
...
```

이제 **NextButtonText** 속성을 관찰할 수 있습니다. 이 속성에 대한 단방향 또는 양방향 바인딩을 작성할 경우(방법은 나중에 설명) 결과 바인딩 개체는 **PropertyChanged** 이벤트를 구독합니다. 이 이벤트가 발생하면 바인딩 개체의 처리기에 변경된 속성 이름이 포함된 인수가 수신됩니다. 이를 통해 바인딩 개체는 다시 읽어야 하는 속성 값을 인식할 수 있습니다.

C#을 사용하고 있다면 위에 표시된 패턴을 여러 번 구현할 필요가 없도록 [QuizGame](https://github.com/microsoft/Windows-appsample-networkhelper) 샘플("Common" 폴더)에 있는 **BindableBase** 기본 클래스에서 파생할 수 있습니다. 관련 예제는 다음과 같습니다.

```csharp
public class HostViewModel : BindableBase
{
    private string nextButtonText;

    public HostViewModel()
    {
        this.NextButtonText = "Next";
    }

    public string NextButtonText
    {
        get { return this.nextButtonText; }
        set { this.SetProperty(ref this.nextButtonText, value); }
    }
}
```

```cppwinrt
// Your BindableBase base class should itself derive from Windows::UI::Xaml::DependencyObject. Then, in HostViewModel.idl, derive from BindableBase instead of implementing INotifyPropertyChanged.
```

> [!NOTE]
> C++/WinRT의 경우 기본 클래스에서 파생된 런타임 클래스(애플리케이션에서 선언)를 *구성 가능* 클래스라고 합니다. 또한 구성 가능 클래스에 관련된 제약 조건이 있습니다. 애플리케이션이 Visual Studio 및 Microsoft Store에서 제출의 유효성 검사에 사용되는 [Windows 앱 인증 키트](../debug-test-perf/windows-app-certification-kit.md) 테스트를 통과하여 Microsoft Store로 성공적으로 수집되려면 구성 가능 클래스는 기본적으로 Windows 기본 클래스에서 파생되어야 합니다. 즉, 상속 계층 구조의 루트에 있는 클래스는 Windows.* 네임스페이스에서 시작되는 형식이어야 합니다. 기본 클래스에서 런타임 클래스를 파생시켜야 하는 경우(예를 들어 파생시킬 모든 보기 모델에 대한 **BindableBase** 클래스를 구현하려면) [**Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject)에서 파생시킬 수 있습니다.

[**String.Empty**](/dotnet/api/system.string.empty) 또는 **null** 인수를 사용하여 **PropertyChanged** 이벤트를 발생시킨 경우 이는 개체에 대한 모든 비인덱서 속성을 다시 읽어야 함을 나타냅니다. 특정 인덱서의 경우 "Item\[*indexer*\]"(여기서 *indexer*는 인덱스 값) 인수, 모든 인덱서의 경우 "Item\[\]" 값을 사용하여 개체에 대한 인덱서 속성이 변경되었음을 나타내는 이벤트를 발생시킬 수 있습니다.

바인딩 소스를 해당 속성에 데이터가 포함된 단일 개체 또는 개체 컬렉션으로 처리할 수 있습니다. C# 및 Visual Basic 코드에서는 런타임에 변경되지 않은 컬렉션을 표시하도록 [**List(Of T)** ](/dotnet/api/system.collections.generic.list-1)를 구현하는 개체에 일회성으로 바인딩할 수 있습니다. 관찰 가능한 컬렉션(컬렉션에서 항목이 추가 및 제거된 경우 관찰)의 경우에는 대신 [**ObservableCollection(Of T)** ](/dotnet/api/system.collections.objectmodel.observablecollection-1)에 단방향으로 바인딩합니다. C++/CX 코드에서는 관찰 가능한 컬렉션과 관찰 가능하지 않은 컬렉션 모두에 대해 [**Vector&lt;T&gt;** ](/cpp/cppcx/platform-collections-vector-class)에 바인딩할 수 있으며, C++/WinRT는 자체 형식이 있습니다. 사용자 컬렉션 클래스에 바인딩하려면 다음 표의 지침을 따르세요.

|시나리오|C# 및 VB(CLR)|C++/WinRT|C++/CX|
|-|-|-|-|
|개체에 바인딩합니다.|어떤 개체든 가능합니다.|어떤 개체든 가능합니다.|개체는 [**BindableAttribute**](/uwp/api/Windows.UI.Xaml.Data.BindableAttribute)을(를) 가지고 있거나 [**ICustomPropertyProvider**](/uwp/api/Windows.UI.Xaml.Data.ICustomPropertyProvider)을(를) 구현해야 합니다.|
|바운드 개체에서 속성 변경 알림을 가져옵니다.|개체는 [**INotifyPropertyChanged**](/dotnet/api/system.componentmodel.inotifypropertychanged)를 구현해야 합니다.| 개체는 [**INotifyPropertyChanged**](/uwp/api/Windows.UI.Xaml.Data.INotifyPropertyChanged)를 구현해야 합니다.|개체는 [**INotifyPropertyChanged**](/uwp/api/Windows.UI.Xaml.Data.INotifyPropertyChanged)를 구현해야 합니다.|
|컬렉션에 바인딩됩니다.| [**List(Of T)** ](/dotnet/api/system.collections.generic.list-1)|[**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)의 [**IVector**](/uwp/api/windows.foundation.collections.ivector_t_) 또는 [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)입니다. [XAML 항목 컨트롤, C++/WinRT 컬렉션 바인딩](../cpp-and-winrt-apis/binding-collection.md) 및 [C++/WinRT로 작성된 컬렉션](../cpp-and-winrt-apis/collections.md)을 참조하세요.| [**Vector&lt;T&gt;** ](/cpp/cppcx/platform-collections-vector-class)|
|바인딩된 컬렉션에서 컬렉션 변경 알림을 가져옵니다.|[**ObservableCollection(Of T)** ](/dotnet/api/system.collections.objectmodel.observablecollection-1)|[**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)의 [**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_)입니다. 예를 들어 [**winrt::single_threaded_observable_vector&lt;T&gt;** ](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)입니다.|[**IObservableVector&lt;T&gt;** ](/uwp/api/Windows.Foundation.Collections.IObservableVector_T_)입니다.  [**Vector&lt;T&gt;** ](/cpp/cppcx/platform-collections-vector-class)는 이 인터페이스를 구현합니다.|
|바인딩을 지원하는 컬렉션을 구현합니다.|[  **List(Of T)** ](/dotnet/api/system.collections.generic.list-1)를 확장하거나 [**IList**](/dotnet/api/system.collections.ilist), [**IList**](/dotnet/api/system.collections.generic.ilist-1)(Of [**Object**](/dotnet/api/system.object)), [**IEnumerable**](/dotnet/api/system.collections.ienumerable) 또는 [**IEnumerable**](/dotnet/api/system.collections.generic.ienumerable-1)(Of **Object**)를 구현합니다. 제네릭 **IList(Of T)** 및 **IEnumerable(Of T)** 에는 바인딩할 수 없습니다.|[**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)의 [**IVector**](/uwp/api/windows.foundation.collections.ivector_t_)을 구현합니다. [XAML 항목 컨트롤, C++/WinRT 컬렉션 바인딩](../cpp-and-winrt-apis/binding-collection.md) 및 [C++/WinRT로 작성된 컬렉션](../cpp-and-winrt-apis/collections.md)을 참조하세요.|[**IBindableVector**](/uwp/api/Windows.UI.Xaml.Interop.IBindableVector), [**IBindableIterable**](/uwp/api/Windows.UI.Xaml.Interop.IBindableIterable), [**IVector**](/uwp/api/Windows.Foundation.Collections.IVector_T_)&lt;[**Object**](/dotnet/api/system.object)^&gt;, [**IIterable**](/uwp/api/Windows.Foundation.Collections.IIterable_T_)&lt;**Object**^&gt;, **IVector**&lt;[**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)\*&gt; 또는 **IIterable**&lt;**IInspectable**\*&gt;을 구현합니다. 제네릭 **IVector&lt;T&gt;** 및 **IIterable&lt;T&gt;** 에는 바인딩할 수 없습니다.|
| 컬렉션 변경 알림을 지원하는 컬렉션을 구현합니다. | [  **ObservableCollection(Of T)** ](/dotnet/api/system.collections.objectmodel.observablecollection-1)을(를) 확장하거나 제네릭이 아닌 [**IList**](/dotnet/api/system.collections.ilist) 및 [**INotifyCollectionChanged**](/dotnet/api/system.collections.specialized.inotifycollectionchanged)을(를) 구현합니다.|[**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)의 [**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_) 또는 [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)를 구현합니다.|[  **IBindableVector**](/uwp/api/Windows.UI.Xaml.Interop.IBindableVector) 및 [**IBindableObservableVector**](/uwp/api/Windows.UI.Xaml.Interop.IBindableObservableVector)을(를) 구현합니다.|
|증분 로드를 지원하는 컬렉션을 구현합니다.|[  **ObservableCollection(Of T)** ](/dotnet/api/system.collections.objectmodel.observablecollection-1)을(를) 확장하거나 제네릭이 아닌 [**IList**](/dotnet/api/system.collections.ilist) 및 [**INotifyCollectionChanged**](/dotnet/api/system.collections.specialized.inotifycollectionchanged)을(를) 구현합니다. 또한 [**ISupportIncrementalLoading**](/uwp/api/Windows.UI.Xaml.Data.ISupportIncrementalLoading)을(를) 구현합니다.|[**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)의 [**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_) 또는 [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)를 구현합니다. 또한 [**ISupportIncrementalLoading**](/uwp/api/Windows.UI.Xaml.Data.ISupportIncrementalLoading)을 구현합니다.|[  **IBindableVector**](/uwp/api/Windows.UI.Xaml.Interop.IBindableVector), [**IBindableObservableVector**](/uwp/api/Windows.UI.Xaml.Interop.IBindableObservableVector) 및 [**ISupportIncrementalLoading**](/uwp/api/Windows.UI.Xaml.Data.ISupportIncrementalLoading)을 구현합니다.|

목록 컨트롤을 임의 크기의 데이터 원본에 바인딩하고 증분 로드를 사용하여 고성능을 유지할 수 있습니다. 예를 들어 한 번에 모든 결과를 로드할 필요 없이 목록 컨트롤을 Bing 이미지 쿼리 결과에 바인딩할 수 있습니다. 대신 일부 결과만 즉시 로드하고 필요에 따라 추가 결과를 로드합니다. 증분 로드를 지원하려면 컬렉션 변경 알림을 지원하는 데이터 원본에서 [**ISupportIncrementalLoading**](/uwp/api/Windows.UI.Xaml.Data.ISupportIncrementalLoading)을 구현해야 합니다. 데이터 바인딩 엔진이 더 많은 데이터를 요청할 경우 데이터 원본는 적절한 요청을 하고 결과를 통합한 다음 UI를 업데이트하기 위해 적절한 알림을 보내야 합니다.

### <a name="binding-target"></a>바인딩 대상

아래의 두 예제에서 **Button.Content** 속성은 바인딩 대상이고, 해당 값은 바인딩 개체를 선언하는 태그 확장으로 설정됩니다. 먼저 [{x:Bind}](../xaml-platform/x-bind-markup-extension.md)가 표시된 다음 [{Binding}](../xaml-platform/binding-markup-extension.md)이 표시됩니다. 태그에서 바인딩을 선언하는 것이 일반적입니다(편리하고 읽기 쉬우며 도구 사용이 간편함). 그러나 태그를 피하고 필요한 경우 명령을 통해(프로그래밍 방식으로) [**Binding**](/uwp/api/Windows.UI.Xaml.Data.Binding) 클래스의 인스턴스를 대신 만들 수 있습니다.

```xaml
<Button Content="{x:Bind ...}" ... />
```

```xaml
<Button Content="{Binding ...}" ... />
```

C++/WinRT 또는 Visual C++ 구성 요소 확장(C++/CX)을 사용하는 경우 [{Binding}](../xaml-platform/binding-markup-extension.md) 태그 확장에 사용하려는 런타임 클래스에 [**BindableAttribute**](/uwp/api/Windows.UI.Xaml.Data.BindableAttribute) 특성을 추가해야 합니다.

> [!IMPORTANT]
> [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md)를 사용하는 경우 Windows SDK 버전 10.0.17763.0(Windows 10 버전 1809) 이상을 설치했다면 [**BindableAttribute**](/uwp/api/Windows.UI.Xaml.Data.BindableAttribute) 특성을 사용할 수 있습니다. 해당 특성이 없을 경우 [{Binding}](../xaml-platform/binding-markup-extension.md) 태그 확장을 사용하려면 [ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider) 및 [ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty) 인터페이스를 구현해야 합니다.

### <a name="binding-object-declared-using-xbind"></a>{x:Bind}를 사용하여 선언된 바인딩 개체

[{x:Bind}](../xaml-platform/x-bind-markup-extension.md) 태그를 작성하기 전에 수행해야 하는 한 가지 단계가 있습니다. 태그 페이지를 나타내는 클래스에서 바인딩 소스 클래스를 노출해야 합니다. **MainPage** 페이지 클래스에 속성(이 예제의 경우 **HostViewModel** 형식)을 추가하면 됩니다.

```csharp
namespace DataBindingInDepth
{
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.ViewModel = new HostViewModel();
        }
    
        public HostViewModel ViewModel { get; set; }
    }
}
```

```cppwinrt
// MainPage.idl
import "HostViewModel.idl";

namespace DataBindingInDepth
{
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        HostViewModel ViewModel{ get; };
    }
}

// MainPage.h
// Include a header, and add this field:
...
#include "HostViewModel.h"
...
    DataBindingInDepth::HostViewModel ViewModel();

private:
    DataBindingInDepth::HostViewModel m_viewModel{ nullptr };
...

// MainPage.cpp
// Implement like this:
...
MainPage::MainPage()
{
    InitializeComponent();

}

DataBindingInDepth::HostViewModel MainPage::ViewModel()
{
    return m_viewModel;
}
...
```

이제 바인딩 개체를 선언하는 태그에 대해 좀 더 자세히 알아보겠습니다. 아래 예제에서는 위의 "바인딩 대상" 섹션에서 사용한 것과 동일한 **Button.Content** 바인딩 대상을 사용하여 이 대상이 **HostViewModel.NextButtonText** 속성에 바인딩됨을 보여 줍니다.

```xaml
<!-- MainPage.xaml -->
<Page x:Class="DataBindingInDepth.Mainpage" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

**Path**에 대해 지정한 값에 유의하세요. 이 값은 페이지 자체의 컨텍스트에서 해석되며, 이 예제에서는 **MainPage** 페이지에 방금 추가한 **ViewModel** 속성을 참조하여 경로가 시작됩니다. 이 속성은 **HostViewModel** 인스턴스를 반환하므로 해당 개체에 점을 찍어 **HostViewModel.NextButtonText** 속성에 액세스할 수 있습니다. 또한 **Mode**를 지정하여 [{x:Bind}](../xaml-platform/x-bind-markup-extension.md) 기본값 일회성을 재정의합니다.

[  **Path**](/uwp/api/windows.ui.xaml.data.binding.path) 속성은 중첩된 속성, 연결된 속성, 정수 및 문자열 인덱서 등에 바인딩하는 데 필요한 다양한 구문 옵션을 지원합니다. 자세한 내용은 [속성 경로 구문](../xaml-platform/property-path-syntax.md)을 참조하세요. 문자열 인덱서에 바인딩하면 [**ICustomPropertyProvider**](/uwp/api/Windows.UI.Xaml.Data.ICustomPropertyProvider)을(를) 구현할 필요 없이 동적 속성에 바인딩하는 효과가 있습니다. 다른 설정은 [{x:Bind} 태그 확장](../xaml-platform/x-bind-markup-extension.md)을 참조하세요.

**HostViewModel.NextButtonText** 속성이 실제로 관찰 가능하다는 것을 보여주기 위해 **클릭** 이벤트 처리기를 단추에 추가하고, **HostViewModel.NextButtonText** 값을 업데이트합니다. 단추를 빌드하고, 실행하고, 클릭하여 단추의 **콘텐츠** 업데이트 값을 확인합니다.

```csharp
// MainPage.xaml.cs
private void Button_Click(object sender, RoutedEventArgs e)
{
    this.ViewModel.NextButtonText = "Updated Next button text";
}
```

```cppwinrt
// MainPage.cpp
void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
{
    ViewModel().NextButtonText(L"Updated Next button text");
}
```

> [!NOTE]
> [**TextBox.Text**](/uwp/api/windows.ui.xaml.controls.textbox.text)에 대한 변경 내용은 모든 사용자 키 입력 후가 아니라 [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox)가 포커스를 잃을 때 양방향 바인딩 소스로 전송됩니다.

**DataTemplate 및 x:DataType**

[  **DataTemplate**](/uwp/api/Windows.UI.Xaml.DataTemplate)(항목 템플릿, 콘텐츠 템플릿 또는 헤더 템플릿 중 무엇으로 사용되든지 관계없음) 내에서 **Path** 값은 페이지의 컨텍스트가 아니라 템플릿을 기반으로 만들 데이터 개체의 컨텍스트에서 해석됩니다. 데이터 템플릿에서 {x:Bind}를 사용할 때 컴파일 시간에 해당 바인딩의 유효성을 검사(그리고 해당 바인딩에 대해 효율적인 코드를 생성)할 수 있도록 **DataTemplate**에서 **x:DataType**을 사용하여 데이터 개체 형식을 선언해야 합니다. 아래 제공된 예제는 **SampleDataGroup** 개체의 컬렉션에 바인딩된 항목 컨트롤의 **ItemTemplate**으로 사용할 수 있습니다.

```xaml
<DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```

**경로의 약한 형식의 개체**

Title이라는 문자열 속성을 구현하는 SampleDataGroup이라는 형식을 예로 들어보겠습니다. 또한 형식 개체이지만 실제로는 SampleDataGroup의 인스턴스를 반환하는 MainPage.SampleDataGroupAsObject라는 속성이 있습니다. `<TextBlock Text="{x:Bind SampleDataGroupAsObject.Title}"/>` 바인딩은 형식 개체에서 Title 속성을 찾을 수 없으므로 컴파일 오류를 발생시킵니다. 이 문제의 해결 방법은 다음과 같이 경로 구문에 캐스트를 추가하는 것입니다. `<TextBlock Text="{x:Bind ((data:SampleDataGroup)SampleDataGroupAsObject).Title}"/>`. 다음은 Element가 개체로 선언되지만 실제로는 TextBlock인 다른 예입니다. `<TextBlock Text="{x:Bind Element.Text}"/>`. 그리고 캐스트는 이 문제를 해결합니다. `<TextBlock Text="{x:Bind ((TextBlock)Element).Text}"/>`.

**데이터가 비동기적으로 로드되는 경우**

**{x:Bind}** 를 지원하는 코드는 컴파일 시간에 페이지의 partial 클래스에서 생성됩니다. 이러한 파일은 `obj` 폴더(C#의 경우 이름이 `<view name>.g.cs`와 같음)에서 찾을 수 있습니다. 생성된 코드에는 페이지의 [**Loading**](/uwp/api/Windows.UI.Xaml.FrameworkElement) 이벤트에 대한 처리기가 포함되며, 해당 처리기는 페이지의 바인딩을 나타내는 생성된 클래스의 **Initialize** 메서드를 호출합니다. 그러면 **Initialize**는 차례로 **Update**를 호출하여 바인딩 소스와 바인딩 대상 간에 데이터를 이동하기 시작합니다. **Loading**은 페이지 또는 사용자 정의 컨트롤의 첫 번째 측정 단계 바로 전에 발생합니다. 따라서 데이터가 비동기적으로 로드되는 경우 **Initialize**가 호출될 때까지 데이터가 준비되지 않을 수 있습니다. 그러므로 데이터를 로드한 후 `this.Bindings.Update();`을 호출하여 일회성 바인딩이 강제로 초기화되도록 할 수 있습니다. 비동기적으로 로드된 데이터에 대해서만 일회성 바인딩이 필요한 경우 이 방법으로 데이터를 초기화하는 것이 단방향 바인딩을 사용하고 변경 내용을 수신 대기하는 것보다 훨씬 저렴합니다. 데이터가 세분화된 변경을 거치지 않고 특정 작업의 일부로 업데이트될 가능성이 큰 경우 바인딩을 일회성으로 만들고 언제든지 **Update**를 호출하여 강제로 수동 업데이트를 수행할 수 있습니다.

> [!NOTE]
> **{x:Bind}** 는 JSON 개체의 사전 구조 탐색처럼 런타임에 바인딩되는 속성 시나리오와 덕 타이핑(duck typing)에 적합하지 않습니다. "덕 타이핑"은 속성 이름의 어휘 일치를 기반으로 하는 약한 입력 형식입니다("오리처럼 걷고 헤엄치고 꽥꽥거리면 오리"). 덕 타이핑을 사용할 경우 **Age** 속성에 대한 바인딩은 **Person** 또는 **Wine** 개체도 동일하게 만족합니다(각 형식에 **Age** 속성이 있다고 가정). 이러한 시나리오에는 **{Binding}** 태그 확장을 사용합니다.

### <a name="binding-object-declared-using-binding"></a>{Binding}을 사용하여 선언된 바인딩 개체

C++/WinRT 또는 Visual C++ 구성 요소 확장(C++/CX)을 사용하는 경우 [{Binding}](../xaml-platform/binding-markup-extension.md) 태그 확장을 사용하려면 바인딩할 런타임 클래스에 [**BindableAttribute**](/uwp/api/Windows.UI.Xaml.Data.BindableAttribute) 특성을 추가해야 합니다. [{x:Bind}](../xaml-platform/x-bind-markup-extension.md)를 사용하려는 경우에는 해당 특성이 필요하지 않습니다.

```cppwinrt
// HostViewModel.idl
// Add this attribute:
[Windows.UI.Xaml.Data.Bindable]
runtimeclass HostViewModel : Windows.UI.Xaml.Data.INotifyPropertyChanged
...
```

> [!IMPORTANT]
> [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md)를 사용하는 경우, Windows SDK 버전 10.0.17763.0(Windows 10, 버전 1809) 이상을 설치했다면 [**BindableAttribute**](/uwp/api/Windows.UI.Xaml.Data.BindableAttribute) 특성을 사용할 수 있습니다. 해당 특성이 없을 경우 [{Binding}](../xaml-platform/binding-markup-extension.md) 태그 확장을 사용하려면 [ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider) 및 [ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty) 인터페이스를 구현해야 합니다.

[{Binding}](../xaml-platform/binding-markup-extension.md)은 기본적으로 태그 페이지의 [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext)에 바인딩하는 것으로 가정합니다. 따라서 페이지의 **DataContext**를 바인딩 소스 클래스(이 예제의 경우 **HostViewModel** 형식)의 인스턴스로 설정합니다. 아래 예제에서는 바인딩 개체를 선언하는 태그를 보여 줍니다. 위의 "바인딩 대상" 섹션에서 사용한 것과 동일한 **Button.Content** 바인딩 대상을 사용하고 **HostViewModel.NextButtonText** 속성에 바인딩합니다.

```xaml
<Page xmlns:viewmodel="using:DataBindingInDepth" ... >
    <Page.DataContext>
        <viewmodel:HostViewModel x:Name="viewModelInDataContext"/>
    </Page.DataContext>
    ...
    <Button Content="{Binding Path=NextButtonText}" ... />
</Page>
```

```csharp
// MainPage.xaml.cs
private void Button_Click(object sender, RoutedEventArgs e)
{
    this.viewModelInDataContext.NextButtonText = "Updated Next button text";
}
```

```cppwinrt
// MainPage.cpp
void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
{
    viewModelInDataContext().NextButtonText(L"Updated Next button text");
}
```

**Path**에 대해 지정한 값에 유의하세요. 이 값은 페이지의 [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext) 컨텍스트(이 예제에서는 **HostViewModel**의 인스턴스로 설정됨)에서 해석됩니다. 경로는 **HostViewModel.NextButtonText** 속성을 참조합니다. 여기에서는 [{Binding}](../xaml-platform/binding-markup-extension.md)의 기본값 단방향이 적용되므로 **Mode**를 생략할 수 있습니다.

UI 요소에 대한 [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext)의 기본값은 해당 부모에서 상속된 값입니다. 물론 기본적으로 자식에서 상속되는 **DataContext**를 명시적으로 설정하여 기본값을 재정의할 수 있습니다. 요소에서 **DataContext**를 명시적으로 설정하는 것은 여러 바인딩에서 동일한 소스를 사용하려는 경우에 유용합니다.

바인딩 개체에는 기본적으로 바인딩이 선언된 UI 요소의 [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext)로 설정되는 **Source** 속성이 있습니다. 바인딩에서 **Source**, **RelativeSource** 또는 **ElementName**을 명시적으로 설정하여 이 기본값을 재정의할 수 있습니다(자세한 내용은 [{Binding}](../xaml-platform/binding-markup-extension.md) 참조).

[**DataTemplate**](/uwp/api/Windows.UI.Xaml.DataTemplate) 내에서 [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext)는 템플릿을 기반으로 만들 데이터 개체로 자동 설정됩니다. 아래 제공된 예제는 **Title** 및 **Description**이라는 문자열 속성이 있는 모든 형식의 컬렉션에 바인딩된 항목 컨트롤의 **ItemTemplate**으로 사용할 수 있습니다.

```xaml
<DataTemplate x:Key="SimpleItemTemplate">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{Binding Title}"/>
      <TextBlock Text="{Binding Description"/>
    </StackPanel>
  </DataTemplate>
```

> [!NOTE]
> 기본적으로 [**TextBox.Text**](/uwp/api/windows.ui.xaml.controls.textbox.text)에 대한 변경 내용은 [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox)가 포커스를 잃을 때 양방향 바인딩 소스로 전송됩니다. 변경 내용이 모든 사용자 키 입력 후 전송되도록 하려면 태그의 바인딩에서 **UpdateSourceTrigger**를 **PropertyChanged**로 설정합니다. **UpdateSourceTrigger**를 **Explicit**로 설정하여 변경 내용이 소스로 전송되는 경우를 완전히 제어할 수도 있습니다. 그런 다음 텍스트 상자(일반적으로 [**TextBox.TextChanged**](/uwp/api/Windows.UI.Xaml.Controls.TextBox))에서 이벤트를 처리하고, 대상에서 [**GetBindingExpression**](/uwp/api/windows.ui.xaml.frameworkelement.getbindingexpression)을 호출하여 [**BindingExpression**](/uwp/api/windows.ui.xaml.data.bindingexpression) 개체를 가져온 다음 마지막으로 [**BindingExpression.UpdateSource**](/uwp/api/windows.ui.xaml.data.bindingexpression.updatesource)를 호출하여 데이터 원본을 프로그래밍 방식으로 업데이트합니다.

[  **Path**](/uwp/api/windows.ui.xaml.data.binding.path) 속성은 중첩된 속성, 연결된 속성, 정수 및 문자열 인덱서 등에 바인딩하는 데 필요한 다양한 구문 옵션을 지원합니다. 자세한 내용은 [속성 경로 구문](../xaml-platform/property-path-syntax.md)을 참조하세요. 문자열 인덱서에 바인딩하면 [**ICustomPropertyProvider**](/uwp/api/Windows.UI.Xaml.Data.ICustomPropertyProvider)을(를) 구현할 필요 없이 동적 속성에 바인딩하는 효과가 있습니다. [  **ElementName**](/uwp/api/windows.ui.xaml.data.binding.elementname) 속성은 요소 간 바인딩에 유용합니다. [  **RelativeSource**](/uwp/api/windows.ui.xaml.data.binding.relativesource) 속성은 다양하게 사용되며, 그중 하나로 [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) 내에서 템플릿 바인딩을 대신하여 더 유용하게 사용됩니다. 다른 설정은 [{Binding} 태그 확장](../xaml-platform/binding-markup-extension.md) 및 [**Binding**](/uwp/api/Windows.UI.Xaml.Data.Binding) 클래스를 참조하세요.

## <a name="what-if-the-source-and-the-target-are-not-the-same-type"></a>소스와 대상의 형식이 서로 다른 경우

부울 속성 값을 기반으로 UI 요소의 가시성을 제어하려는 경우, 숫자 값의 범위나 추세를 보여 주는 색상을 사용하여 UI 요소를 렌더링하려는 경우 또는 문자열이 필요한 UI 요소 속성에 날짜 및/또는 시간 값을 표시하려는 경우 형식 간에 값을 변환해야 합니다. 바인딩 소스 클래스에서 올바른 형식의 다른 속성을 노출하고 변환 논리를 캡슐화되고 테스트 가능하도록 유지하는 것이 올바른 해결 방법인 경우가 있습니다. 하지만 이 방법은 소스 및 대상 속성의 개수 또는 조합이 많은 경우 유연하지 않고 확장할 수도 없습니다. 이 경우 다음 두 가지 옵션이 있습니다.

* {x:Bind}를 사용할 경우에는 함수에 직접 바인딩하여 해당 변환을 수행할 수 있습니다.
* 또는 변환을 수행하도록 설계된 개체인 값 변환기를 지정할 수 있습니다. 

## <a name="value-converters"></a>값 변환기

다음은 [**DateTime**](/dotnet/api/system.datetime) 값을 월이 포함된 문자열 값으로 변환하는 일회성 또는 단방향 바인딩에 적합한 값 변환기입니다. 이 클래스는 [**IValueConverter**](/uwp/api/Windows.UI.Xaml.Data.IValueConverter)를 구현합니다.

```csharp
public class DateToStringConverter : IValueConverter
{
    // Define the Convert method to convert a DateTime value to 
    // a month string.
    public object Convert(object value, Type targetType, 
        object parameter, string language)
    {
        // value is the data from the source object.
        DateTime thisdate = (DateTime)value;
        int monthnum = thisdate.Month;
        string month;
        switch (monthnum)
        {
            case 1:
                month = "January";
                break;
            case 2:
                month = "February";
                break;
            default:
                month = "Month not found";
                break;
        }
        // Return the value to pass to the target.
        return month;
    }

    // ConvertBack is not implemented for a OneWay binding.
    public object ConvertBack(object value, Type targetType, 
        object parameter, string language)
    {
        throw new NotImplementedException();
    }
}
```

```cppwinrt
// See the "Formatting or converting data values for display" section in the "Data binding overview" topic.
```

그리고 바인딩 개체 태그에서 해당 값 변환기를 사용할 방법을 다음과 같습니다.

```xaml
<UserControl.Resources>
  <local:DateToStringConverter x:Key="Converter1"/>
</UserControl.Resources>
...
<TextBlock Grid.Column="0" 
  Text="{x:Bind ViewModel.Month, Converter={StaticResource Converter1}}"/>
<TextBlock Grid.Column="0" 
  Text="{Binding Month, Converter={StaticResource Converter1}}"/>
```

바인딩에 대해 [**Converter**](/uwp/api/windows.ui.xaml.data.binding.converter) 매개 변수가 정의되어 있는 경우 바인딩 엔진은 [**Convert**](/uwp/api/windows.ui.xaml.data.ivalueconverter.convert) 및 [**ConvertBack**](/uwp/api/windows.ui.xaml.data.ivalueconverter.convertback) 메서드를 호출합니다. 소스에서 데이터가 전달되면 바인딩 엔진은 **Convert**를 호출하고 반환되는 데이터를 대상에 전달합니다. 대상에서 데이터가 전달되면(양방향 바인딩의 경우) 바인딩 엔진은 **ConvertBack**을 호출하고 반환되는 데이터를 소스에 전달합니다.

또한 변환기는 다음과 같은 선택적 매개 변수를 사용합니다. [**ConverterLanguage**](/uwp/api/windows.ui.xaml.data.binding.converterlanguage) 매개 변수는 변환에 사용할 언어를 지정하는 데 사용되고, [**ConverterParameter**](/uwp/api/windows.ui.xaml.data.binding.converterparameter) 매개 변수는 변환 논리에 대한 매개 변수를 전달하는 데 사용됩니다. 변환기 매개 변수에 대한 사용 예제는 [**IValueConverter**](/uwp/api/Windows.UI.Xaml.Data.IValueConverter)를 참조하세요.

> [!NOTE]
> 변환 중에 오류가 발생하더라도 예외가 throw되지 않습니다. 대신 [**DependencyProperty.UnsetValue**](/uwp/api/windows.ui.xaml.dependencyproperty.unsetvalue)을(를) 반환하고 데이터 전송을 중지합니다.

바인딩 소스를 확인할 수 없을 때마다 사용할 기본값을 표시하려면 태그에서 바인딩 개체에 대해 **FallbackValue** 속성을 설정합니다. 이 방법은 변환 및 형식 지정 오류를 처리하는 데 유용합니다. 또한 형식이 다른 바인딩된 컬렉션의 일부 개체에 존재하지 않을 수 있는 소스 속성에 바인딩하는 데에도 유용합니다.

텍스트 컨트롤을 문자열이 아닌 값에 바인딩하면 데이터 바인딩 엔진이 값을 문자열로 변환합니다. 값이 참조 형식이면 데이터 바인딩 엔진은 [**ICustomPropertyProvider.GetStringRepresentation**](/uwp/api/windows.ui.xaml.data.icustompropertyprovider.getstringrepresentation) 또는 [**IStringable.ToString**](/windows/desktop/api/windows.foundation/nf-windows-foundation-istringable-tostring)을 호출하여(있는 경우) 문자열 값을 검색하고, 그렇지 않으면 [**Object.ToString**](/dotnet/api/system.object.tostring#System_Object_ToString)을 호출합니다. 그러나 바인딩 엔진은 기본 클래스 구현을 숨기는 **ToString** 구현을 무시합니다. 대신 하위 클래스 구현은 기본 클래스 **ToString** 메서드를 재정의해야 합니다. 마찬가지로, 네이티브 언어에서는 모든 관리되는 개체가 [**ICustomPropertyProvider**](/uwp/api/Windows.UI.Xaml.Data.ICustomPropertyProvider) 및 [**IStringable**](/windows/desktop/api/windows.foundation/nn-windows-foundation-istringable)을 구현하는 것처럼 보입니다. 그러나 **GetStringRepresentation** 및 **IStringable.ToString**에 대한 모든 호출은 **Object.ToString** 또는 해당 메서드의 재정의로 라우트되며, 기본 클래스 구현을 숨기는 새로운 **ToString** 구현으로는 라우트되지 않습니다.

> [!NOTE]
> Windows 10 버전 1607부터 XAML 프레임워크는 기본 제공 부울-표시 변환기를 제공합니다. 변환기는 **Visible** 열거형 값에 **true**를, **Collapsed**에 **false**를 매핑하므로 변환기를 만들지 않고 Visibility 속성을 부울에 바인딩할 수 있습니다. 기본 제공 변환기를 사용하려면 앱의 최소 대상 SDK 버전이 14393 이상이어야 합니다. 앱이 이전 버전의 Windows 10을 대상으로 하는 경우 기본 제공 변환기를 사용할 수 없습니다. 대상 버전에 대한 자세한 내용은 [버전 적응 코드](../debug-test-perf/version-adaptive-code.md)를 참조하세요.

## <a name="function-binding-in-xbind"></a>{x:Bind}에서 함수 바인딩

{x:Bind}를 사용하면 바인딩 경로의 마지막 단계가 함수가 됩니다. 이는 변환을 수행하고 둘 이상의 속성에 종속된 바인딩을 수행하는 데 사용할 수 있습니다. [**X:bind 함수**](function-bindings.md)를 참조하세요.

<span id="resource-dictionaries-with-x-bind"/>

## <a name="element-to-element-binding"></a>요소 간 바인딩

한 XAML 요소의 속성을 다른 XAML 요소의 속성에 바인딩할 수 있습니다. 태그에서는 다음과 같이 표시됩니다.

```xaml
<TextBox x:Name="myTextBox" />
<TextBlock Text="{x:Bind myTextBox.Text, Mode=OneWay}" />
```

> [!IMPORTANT]
> C++/WinRT를 사용하는 요소 간 바인딩에 필요한 워크플로는 [요소 간 바인딩](../cpp-and-winrt-apis/binding-property.md#element-to-element-binding)을 참조하세요.

## <a name="resource-dictionaries-with-xbind"></a>{x:Bind}를 사용하는 리소스 사전

[{x:Bind} 태그 확장](../xaml-platform/x-bind-markup-extension.md)은 코드 생성에 종속되므로 **InitializeComponent**를 호출하여 생성된 코드를 초기화하는 생성자가 포함된 코드 숨김 파일이 필요합니다. 파일 이름을 참조하는 대신 해당 형식을 인스턴스화(**InitializeComponent**가 호출되도록)하여 리소스 사전을 다시 사용합니다. 다음은 기존 리소스 사전이 있고 해당 사전에서 {x:Bind}를 사용하려는 경우에 수행할 작업에 대한 예제입니다.

TemplatesResourceDictionary.xaml

```xaml
<ResourceDictionary
    x:Class="ExampleNamespace.TemplatesResourceDictionary"
    .....
    xmlns:examplenamespace="using:ExampleNamespace">
    
    <DataTemplate x:Key="EmployeeTemplate" x:DataType="examplenamespace:IEmployee">
        <Grid>
            <TextBlock Text="{x:Bind Name}"/>
        </Grid>
    </DataTemplate>
</ResourceDictionary>
```

TemplatesResourceDictionary.xaml.cs

```csharp
using Windows.UI.Xaml.Data;
 
namespace ExampleNamespace
{
    public partial class TemplatesResourceDictionary
    {
        public TemplatesResourceDictionary()
        {
            InitializeComponent();
        }
    }
}
```

MainPage.xaml

```xaml
<Page x:Class="ExampleNamespace.MainPage"
    ....
    xmlns:examplenamespace="using:ExampleNamespace">

    <Page.Resources>
        <ResourceDictionary>
            .... 
            <ResourceDictionary.MergedDictionaries>
                <examplenamespace:TemplatesResourceDictionary/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Page.Resources>
</Page>
```

## <a name="event-binding-and-icommand"></a>이벤트 바인딩 및 ICommand

[{x:Bind}](../xaml-platform/x-bind-markup-extension.md)는 이벤트 바인딩이라는 기능을 지원합니다. 이 기능을 사용하면 바인딩을 사용하여 이벤트에 대한 처리기를 지정할 수 있습니다. 이는 코드 숨김 파일의 메서드를 사용하여 이벤트를 처리하는 것 외의 추가 옵션입니다. **MainPage** 클래스에 **RootFrame** 속성이 있다고 가정해 보겠습니다.

```csharp
public sealed partial class MainPage : Page
{
    ...
    public Frame RootFrame { get { return Window.Current.Content as Frame; } }
}
```

이와 같은 **RootFrame** 속성에서 반환되는 **Frame** 개체의 메서드에 단추의 **Click** 이벤트를 바인딩할 수 있습니다. 또한 단추의 **IsEnabled** 속성을 동일한 **Frame**의 다른 멤버에 바인딩할 수 있습니다.

```xaml
<AppBarButton Icon="Forward" IsCompact="True"
IsEnabled="{x:Bind RootFrame.CanGoForward, Mode=OneWay}"
Click="{x:Bind RootFrame.GoForward}"/>
```

오버로드된 메서드는 이 기술을 사용하여 이벤트를 처리하는 데 사용할 수 없습니다. 또한 이벤트를 처리하는 메서드에 매개 변수가 있는 경우 각각 모든 이벤트 매개 변수의 형식에서 할당 가능해야 합니다. 이 경우 [**Frame.GoForward**](/uwp/api/windows.ui.xaml.controls.frame.goforward)는 오버로드되지 않고 매개 변수가 없습니다(그러나 두 개의 **object** 매개 변수를 사용한 경우에도 유효함). [**Frame.GoBack**](/uwp/api/windows.ui.xaml.controls.frame.goback)은 오버로드되므로 이 기술에서는 이 메서드를 사용할 수 없습니다.

이벤트 바인딩 기술은 명령(명령은 [**ICommand**](/uwp/api/windows.ui.xaml.input.icommand) 인터페이스를 구현하는 개체를 반환하는 속성)을 구현하고 사용하는 것과 유사합니다. [{x:Bind}](../xaml-platform/x-bind-markup-extension.md)와 [{Binding}](../xaml-platform/binding-markup-extension.md) 둘 다 명령과 함께 작동합니다. 명령 패턴을 여러 번 구현할 필요가 없도록 [QuizGame](https://github.com/microsoft/Windows-appsample-networkhelper) 샘플("Common" 폴더)에 있는 **DelegateCommand** 도우미 클래스를 사용할 수 있습니다.

## <a name="binding-to-a-collection-of-folders-or-files"></a>폴더 또는 파일 컬렉션에 바인딩

[  **Windows.Storage**](/uwp/api/Windows.Storage) 네임스페이스의 API를 사용하여 폴더 및 파일 데이터를 검색할 수 있습니다. 그러나 다양한 **GetFilesAsync**, **GetFoldersAsync** 및 **GetItemsAsync** 메서드는 목록 컨트롤에 바인딩하기에 적합한 값을 반환하지 않습니다. 대신 [**FileInformationFactory**](/uwp/api/Windows.Storage.BulkAccess.FileInformationFactory) 클래스의 [**GetVirtualizedFilesVector**](/uwp/api/windows.storage.bulkaccess.fileinformationfactory.getvirtualizedfilesvector), [**GetVirtualizedFoldersVector**](/uwp/api/windows.storage.bulkaccess.fileinformationfactory.getvirtualizedfoldersvector) 및 [**GetVirtualizedItemsVector**](/uwp/api/windows.storage.bulkaccess.fileinformationfactory.getvirtualizeditemsvector) 메서드의 반환 값에 바인딩해야 합니다. [StorageDataSource 및 GetVirtualizedFilesVector 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/master/Official%20Windows%20Platform%20Sample/Windows%208.1%20Store%20app%20samples/99866-Windows%208.1%20Store%20app%20samples/StorageDataSource%20and%20GetVirtualizedFilesVector%20sample)의 다음 코드 예제는 일반적인 사용 패턴을 보여 줍니다. 앱 패키지 매니페스트에서 **picturesLibrary** 기능을 선언하고 그림 라이브러리 폴더에 그림이 있는지 확인해야 합니다.

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    var library = Windows.Storage.KnownFolders.PicturesLibrary;
    var queryOptions = new Windows.Storage.Search.QueryOptions();
    queryOptions.FolderDepth = Windows.Storage.Search.FolderDepth.Deep;
    queryOptions.IndexerOption = Windows.Storage.Search.IndexerOption.UseIndexerWhenAvailable;

    var fileQuery = library.CreateFileQueryWithOptions(queryOptions);

    var fif = new Windows.Storage.BulkAccess.FileInformationFactory(
        fileQuery,
        Windows.Storage.FileProperties.ThumbnailMode.PicturesView,
        190,
        Windows.Storage.FileProperties.ThumbnailOptions.UseCurrentScale,
        false
        );

    var dataSource = fif.GetVirtualizedFilesVector();
    this.PicturesListView.ItemsSource = dataSource;
}
```

일반적으로 이 접근 방식을 사용하여 파일 및 폴더 정보의 읽기 전용 보기를 만듭니다. 예를 들어 사용자가 음악 보기에서 노래를 평가할 수 있도록 파일 및 폴더 속성에 대한 양방향 바인딩을 만들 수 있습니다. 그러나 적절한 **SavePropertiesAsync** 메서드(예: [**MusicProperties.SavePropertiesAsync**](/uwp/api/windows.storage.fileproperties.musicproperties.savepropertiesasync))를 호출할 때까지 변경 사항은 지속되지 않습니다. 항목이 초점을 잃으면 선택 초기화가 트리거되므로 변경 사항을 적용해야 합니다.

이 기술을 사용한 양방향 바인딩은 음악과 같이 인덱싱된 위치에서만 작동합니다. [  **FolderInformation.GetIndexedStateAsync**](/uwp/api/windows.storage.bulkaccess.folderinformation.getindexedstateasync) 메서드를 호출하여 위치가 인덱싱되었는지 여부를 확인할 수 있습니다.

가상화된 벡터는 해당 값을 채우기 전에 일부 항목에 대해 **null**을 반환할 수도 있습니다. 예를 들어 가상화된 벡터에 바인딩된 목록 컨트롤의 [**SelectedItem**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem) 값을 사용하기 전에 **null**을 확인하거나 [**SelectedIndex**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex)를 대신 사용해야 합니다.

## <a name="binding-to-data-grouped-by-a-key"></a>키별로 그룹화된 데이터에 바인딩

항목의 단순 컬렉션(예를 들어 **BookSku** 클래스로 표현되는 책)을 가져와 공용 속성(예를 들어 **BookSku.AuthorName** 속성)을 키로 사용하여 항목을 그룹화한 경우 그룹화된 데이터가 호출됩니다. 데이터를 그룹화한 경우 해당 데이터는 더 이상 단순 컬렉션이 아닙니다. 그룹화된 데이터는 그룹 개체의 컬렉션으로, 각 그룹 개체에는 다음이 있습니다.

- 키
- 속성이 해당 키와 일치하는 항목의 컬렉션

책 예제를 다시 사용하기 위해 저자 이름을 기준으로 책을 그룹화하면 저자 이름 그룹 컬렉션이 생성되고 각 그룹에는 다음이 있습니다.

- 저자 이름인 키
- **AuthorName** 속성이 그룹의 키와 일치하는 **BookSku** 컬렉션입니다.

일반적으로 컬렉션을 표시하려면 항목 컨트롤의 [**ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)(예: [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) 또는 [**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView))를 컬렉션을 반환하는 속성에 직접 바인딩합니다. 항목의 단순 컬렉션이 없는 경우에는 특별히 수행해야 하는 작업이 없습니다. 그러나 그룹화된 데이터에 바인딩하는 경우처럼 그룹 개체의 컬렉션인 경우 항목 컨트롤과 바인딩 소스 사이에 있는 [**CollectionViewSource**](/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource)라는 중간 개체의 서비스가 필요합니다. **CollectionViewSource**를 그룹화된 데이터를 반환하는 속성에 바인딩하고, 항목 컨트롤을 **CollectionViewSource**에 바인딩합니다. **CollectionViewSource**의 추가적인 부가 가치는 현재 항목을 추적하므로 둘 이상의 항목 컨트롤을 모두 동일한 **CollectionViewSource**에 바인딩하여 동기화된 상태로 유지할 수 있다는 점입니다. 또한 [**CollectionViewSource.View**](/uwp/api/windows.ui.xaml.data.collectionviewsource.view) 속성에서 반환되는 개체의 [**ICollectionView.CurrentItem**](/uwp/api/windows.ui.xaml.data.icollectionview.currentitem) 속성을 통해 현재 항목에 프로그래밍 방식으로 액세스할 수 있습니다.

[  **CollectionViewSource**](/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource)의 그룹화 기능을 활성화하려면 [**IsSourceGrouped**](/uwp/api/windows.ui.xaml.data.collectionviewsource.issourcegrouped)를 **true**로 설정합니다. [  **ItemsPath**](/uwp/api/windows.ui.xaml.data.collectionviewsource.itemspath) 속성을 설정해야 하는지 여부는 정확히 그룹 개체를 작성하는 방법에 따라 결정됩니다. 그룹 개체를 작성하는 방법에는 "is-a-group" 패턴과 "has-a-group" 패턴의 두 가지 방법이 있습니다. "is-a-group" 패턴에서는 그룹 개체가 컬렉션 형식(예: **List&lt;T&gt;** )에서 파생되므로 실제로 그룹 개체는 항목 그룹 자체입니다. 이 패턴을 사용하면 **ItemsPath**를 설정하지 않아도 됩니다. "has-a-group" 패턴에서는 그룹 개체가 하나 이상의 컬렉션 형식(예: **List&lt;T&gt;** ) 속성을 가지므로 그룹에 속성 형식의 항목 그룹 "하나가 있습니다"(또는 여러 속성 형식의 여러 항목 그룹). 이 패턴을 사용하면 **ItemsPath**를 항목 그룹을 포함하는 속성 이름으로 설정해야 합니다.

아래 예제에서는 "has-a-group" 패턴을 보여 줍니다. 페이지 클래스에서 뷰 모델의 인스턴스를 반환하는 [**ViewModel**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext)이라는 속성이 있습니다. [  **CollectionViewSource**](/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource)는 뷰 모델의 **Authors** 속성(**Authors**는 그룹 개체의 컬렉션)에 바인딩되며, 그룹화된 항목을 포함하는 **Author.BookSkus** 속성임을 지정합니다. 마지막으로 [**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView)는 **CollectionViewSource**에 바인딩되며, 그룹의 항목을 렌더링할 수 있도록 그룹 스타일이 정의되어 있습니다.

```xaml
<Page.Resources>
    <CollectionViewSource
    x:Name="AuthorHasACollectionOfBookSku"
    Source="{x:Bind ViewModel.Authors}"
    IsSourceGrouped="true"
    ItemsPath="BookSkus"/>
</Page.Resources>
...
<GridView
ItemsSource="{x:Bind AuthorHasACollectionOfBookSku}" ...>
    <GridView.GroupStyle>
        <GroupStyle
            HeaderTemplate="{StaticResource AuthorGroupHeaderTemplateWide}" ... />
    </GridView.GroupStyle>
</GridView>
```

"is-a-group" 패턴을 구현하는 방법에는 두 가지 방법이 있습니다. 한 가지 방법은 사용자 고유의 그룹 클래스를 작성하는 것입니다. **List&lt;T&gt;** (여기서 *T*는 항목의 형식)에서 클래스를 파생합니다. 정의합니다(예: `public class Author : List<BookSku>`). 두 번째 방법은 [LINQ](/previous-versions/bb397926(v=vs.140)) 식을 사용하여 **BookSku** 항목의 유사한 속성 값에서 그룹 개체(및 그룹 클래스)를 동적으로 만드는 것입니다. 이 방법(항목의 단순 목록만 유지하고 즉석에서 그룹화)은 클라우드 서비스에서 데이터에 액세스하는 앱에 일반적입니다. **Author** 및 **Genre**와 같이 특정 그룹 클래스가 필요 없는 책을 저자 또는 장르 등으로 유연하게 그룹화할 수 있게 됩니다.

아래 예제에서는 [LINQ](/previous-versions/bb397926(v=vs.140))를 사용하는 "is-a-group" 패턴을 보여 줍니다. 여기에서는 장르별로 책을 그룹화합니다. 따라서 그룹 머리글에 장르 이름으로 책이 표시됩니다. 이는 그룹 [**Key**](/dotnet/api/system.linq.igrouping-2.key#System_Linq_IGrouping_2_Key) 값에 대한 "Key" 속성 경로로 표시됩니다.

```csharp
using System.Linq;
...
private IOrderedEnumerable<IGrouping<string, BookSku>> genres;

public IOrderedEnumerable<IGrouping<string, BookSku>> Genres
{
    get
    {
        if (this.genres == null)
        {
            this.genres = from book in this.bookSkus
                          group book by book.genre into grp
                          orderby grp.Key
                          select grp;
        }
        return this.genres;
    }
}
```

데이터 템플릿에서 [{x:Bind}](../xaml-platform/x-bind-markup-extension.md)를 사용할 때는 **x:DataType** 값을 설정하여 바인딩할 형식을 나타내야 합니다. 형식이 제네릭인 경우 태그로 표현할 수 없으므로 대신 그룹 스타일 헤더 템플릿에서 [{Binding}](../xaml-platform/binding-markup-extension.md)을 사용해야 합니다.

```xaml
    <Grid.Resources>
        <CollectionViewSource x:Name="GenreIsACollectionOfBookSku"
        Source="{x:Bind Genres}"
        IsSourceGrouped="true"/>
    </Grid.Resources>
    <GridView ItemsSource="{x:Bind GenreIsACollectionOfBookSku}">
        <GridView.ItemTemplate x:DataType="local:BookTemplate">
            <DataTemplate>
                <TextBlock Text="{x:Bind Title}"/>
            </DataTemplate>
        </GridView.ItemTemplate>
        <GridView.GroupStyle>
            <GroupStyle>
                <GroupStyle.HeaderTemplate>
                    <DataTemplate>
                        <TextBlock Text="{Binding Key}"/>
                    </DataTemplate>
                </GroupStyle.HeaderTemplate>
            </GroupStyle>
        </GridView.GroupStyle>
    </GridView>
```

[  **SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 컨트롤은 사용자가 그룹화된 데이터를 보고 탐색하는 데 유용한 방법입니다. [Bookstore2](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore2Universal_10) 샘플 앱은 **SemanticZoom**을 사용하는 방법을 보여 줍니다. 이 앱에서는 저자별로 그룹화된 책 목록을 보거나(확대 보기), 축소하여 저자의 점프 목록(축소 보기)을 볼 수 있습니다. 점프 목록은 책 목록을 스크롤할 때보다 훨씬 더 빠른 탐색이 가능케 합니다. 확대 및 축소 보기는 실제로 동일한 **CollectionViewSource**에 바인딩된 **ListView** 또는 **GridView** 컨트롤입니다.

![SemanticZoom의 그림](images/sezo.png)

계층적 데이터(예: 범주 내의 하위 범주)에 바인딩하는 경우 UI에서 일련의 항목 컨트롤이 포함된 계층 수준을 표시할 수 있습니다. 하나의 항목 컨트롤에서 선택한 사항에 따라 이후 항목 컨트롤의 콘텐츠가 결정됩니다. 각 목록을 자체 [**CollectionViewSource**](/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) 및 **CollectionViewSource** 인스턴스에 체인으로 함께 바인딩하여 목록을 동기화 상태로 유지할 수 있습니다. 이를 마스터/세부 정보(또는 목록/세부 정보) 보기라고 합니다. 자세한 내용은 [계층적 데이터에 바인딩하고 마스터/세부 정보 보기를 만드는 방법](how-to-bind-to-hierarchical-data-and-create-a-master-details-view.md)을 참조하세요.

<span id="debugging"/>

## <a name="diagnosing-and-debugging-data-binding-problems"></a>데이터 바인딩 문제 진단 및 디버깅

바인딩 태그는 속성(C#의 경우 때로 필드와 메서드도 포함됨) 이름을 포함합니다. 따라서 속성 이름을 바꾼 경우 해당 속성을 참조하는 바인딩도 변경해야 합니다. 그러지 않으면 일반적인 데이터 바인딩 버그가 발생하며 앱이 컴파일되지 않거나 제대로 작동하지 않습니다.

[{x:Bind}](../xaml-platform/x-bind-markup-extension.md) 및 [{Binding}](../xaml-platform/binding-markup-extension.md)에서 만든 바인딩 개체는 기능적으로 거의 동일합니다. 그러나 {x:Bind}는 바인딩 소스에 대한 형식 정보가 있으며, 컴파일 타임에 소스 코드를 생성합니다. {x:Bind}를 사용하면 코드의 나머지 부분에서 발생한 것과 동일한 종류의 문제를 검색할 수 있습니다. 여기에는 바인딩 식에 대한 컴파일 타임 유효성 검사와 페이지의 부분 클래스로 생성된 소스 코드에 중단점을 설정하는 방식의 디버깅이 포함됩니다. 이러한 클래스는 `obj` 폴더의 파일(C#의 경우 이름이 `<view name>.g.cs`와 같음)에서 찾을 수 있습니다. 바인딩에 문제가 있는 경우 Microsoft Visual Studio 디버거에서 **처리되지 않은 예외 발생 시 중단**을 설정합니다. 디버거가 이 시점에 실행을 중단하므로 잘못된 것을 디버그할 수 있습니다. {x:Bind}에 의해 생성된 코드는 바인딩 소스 코드 그래프의 각 부분에 대해 동일한 패턴을 따르며, **호출 스택** 창의 정보를 사용하여 문제에 이르는 호출 시퀀스를 결정할 수 있습니다.

[{Binding}](../xaml-platform/binding-markup-extension.md)에는 바인딩 소스에 대한 형식 정보가 없습니다. 그러나 디버거를 연결한 상태로 앱을 실행하면 Visual Studio의 **출력** 창에 모든 바인딩 오류가 표시됩니다.

## <a name="creating-bindings-in-code"></a>코드에서 바인딩 만들기

**참고**  이 섹션은 [{Binding}](../xaml-platform/binding-markup-extension.md)에만 적용됩니다. 코드에서 [{x:Bind}](../xaml-platform/x-bind-markup-extension.md) 바인딩을 만들 수 없기 때문입니다. 그러나 모든 종속성 속성에 대한 변경 알림을 등록할 수 있는 [**DependencyObject.RegisterPropertyChangedCallback**](/uwp/api/windows.ui.xaml.dependencyobject.registerpropertychangedcallback)을 사용해서도 {x:Bind}를 사용할 때와 동일한 이점을 얻을 수 있습니다.

XAML 대신 절차 코드를 사용하여 UI 요소를 데이터에 연결할 수도 있습니다. 이렇게 하려면 새로운 [**Binding**](/uwp/api/Windows.UI.Xaml.Data.Binding) 개체를 만들고, 적절한 속성을 설정하고, [**FrameworkElement.SetBinding**](/uwp/api/windows.ui.xaml.frameworkelement.setbinding) 또는 [**BindingOperations.SetBinding**](/uwp/api/windows.ui.xaml.data.bindingoperations.setbinding)을 호출합니다. 런타임에 바인딩 속성 값을 선택하거나 여러 컨트롤 간에 단일 바인딩을 공유하려는 경우 프로그래밍 방식으로 바인딩을 만드는 것이 유용합니다. 그러나 **SetBinding**을 호출한 후에는 바인딩 속성 값을 변경할 수 없습니다.

다음 예제는 코드에서 바인딩을 구현하는 방법을 보여 줍니다.

```xaml
<TextBox x:Name="MyTextBox" Text="Text"/>
```

```csharp
// Create an instance of the MyColors class 
// that implements INotifyPropertyChanged.
MyColors textcolor = new MyColors();

// Brush1 is set to be a SolidColorBrush with the value Red.
textcolor.Brush1 = new SolidColorBrush(Colors.Red);

// Set the DataContext of the TextBox MyTextBox.
MyTextBox.DataContext = textcolor;

// Create the binding and associate it with the text box.
Binding binding = new Binding() { Path = new PropertyPath("Brush1") };
MyTextBox.SetBinding(TextBox.ForegroundProperty, binding);
```

```vbnet
' Create an instance of the MyColors class 
' that implements INotifyPropertyChanged. 
Dim textcolor As New MyColors()

' Brush1 is set to be a SolidColorBrush with the value Red. 
textcolor.Brush1 = New SolidColorBrush(Colors.Red)

' Set the DataContext of the TextBox MyTextBox. 
MyTextBox.DataContext = textcolor

' Create the binding and associate it with the text box.
Dim binding As New Binding() With {.Path = New PropertyPath("Brush1")}
MyTextBox.SetBinding(TextBox.ForegroundProperty, binding)
```

## <a name="xbind-and-binding-feature-comparison"></a>{x:Bind}와 {Binding}의 기능 비교

| 기능 | {x:Bind} | {Binding} | 참고 |
|---------|----------|-----------|-------|
| 경로가 기본 속성 | `{x:Bind a.b.c}` | `{Binding a.b.c}` | | 
| 속성 경로 | `{x:Bind Path=a.b.c}` | `{Binding Path=a.b.c}` | x:Bind에서는 경로의 루트가 기본적으로 DataContext가 아니라 Page에서 지정됩니다. | 
| 인덱서 | `{x:Bind Groups[2].Title}` | `{Binding Groups[2].Title}` | 컬렉션의 지정된 항목에 바인딩됩니다. 정수 기반 인덱스만 지원됩니다. | 
| 연결된 속성 | `{x:Bind Button22.(Grid.Row)}` | `{Binding Button22.(Grid.Row)}` | 연결된 속성은 괄호 안에 지정됩니다. XAML 네임스페이스에서 속성이 선언되지 않은 경우 문서 헤드의 코드 네임스페이스에 매핑되어야 하는 xml 네임스페이스를 접두사로 사용합니다. | 
| 캐스팅 | `{x:Bind groups[0].(data:SampleDataGroup.Title)}` | 필요 없습니다. | 캐스트는 괄호 안에 지정됩니다. XAML 네임스페이스에서 속성이 선언되지 않은 경우 문서 헤드의 코드 네임스페이스에 매핑되어야 하는 xml 네임스페이스를 접두사로 사용합니다. | 
| 변환기 | `{x:Bind IsShown, Converter={StaticResource BoolToVisibility}}` | `{Binding IsShown, Converter={StaticResource BoolToVisibility}}` | 변환기는 Page/ResourceDictionary 또는 App.xaml에서 선언해야 합니다. | 
| ConverterParameter, ConverterLanguage | `{x:Bind IsShown, Converter={StaticResource BoolToVisibility}, ConverterParameter=One, ConverterLanguage=fr-fr}` | `{Binding IsShown, Converter={StaticResource BoolToVisibility}, ConverterParameter=One, ConverterLanguage=fr-fr}` | 변환기는 Page/ResourceDictionary 또는 App.xaml에서 선언해야 합니다. | 
| TargetNullValue | `{x:Bind Name, TargetNullValue=0}` | `{Binding Name, TargetNullValue=0}` | 바인딩 식의 리프가 null일 때 사용됩니다. 문자열 값에 작은따옴표를 사용합니다. | 
| FallbackValue | `{x:Bind Name, FallbackValue='empty'}` | `{Binding Name, FallbackValue='empty'}` | 바인딩에 대한 경로의 일부(리프 제외)가 null일 때 사용됩니다. | 
| ElementName | `{x:Bind slider1.Value}` | `{Binding Value, ElementName=slider1}` | {x:Bind}를 사용하면 필드에 바인딩됩니다. 경로의 루트가 기본적으로 Page에서 지정되므로 해당 필드를 통해 모든 명명된 요소에 액세스할 수 있습니다. | 
| RelativeSource: Self (자체) | `<Rectangle x:Name="rect1" Width="200" Height="{x:Bind rect1.Width}" ... />` | `<Rectangle Width="200" Height="{Binding Width, RelativeSource={RelativeSource Self}}" ... />` | {x:Bind}를 사용하는 경우 요소의 이름을 지정하고 경로에서 해당 이름을 사용합니다. | 
| RelativeSource: TemplatedParent | 필요하지 않음 | `{Binding <path>, RelativeSource={RelativeSource TemplatedParent}}` | ControlTemplate에 대한 {x:Bind} TargetType은 템플릿 부모에 대한 바인딩을 나타냅니다. {Binding}의 경우 일반 템플릿 바인딩은 대부분 컨트롤 템플릿에서 사용할 수 있습니다. 하지만 변환기 또는 양방향 바인딩을 사용해야 하는 경우 TemplatedParent를 사용합니다.&lt; | 
| 원본 | 필요하지 않음 | `<ListView ItemsSource="{Binding Orders, Source={StaticResource MyData}}"/>` | {X:Bind}의 경우 명명된 요소를 직접 사용할 수 있으며, 속성 또는 정적 경로를 사용할 수 있습니다. | 
| 모드 | `{x:Bind Name, Mode=OneWay}` | `{Binding Name, Mode=TwoWay}` | 모드는 OneTime, OneWay 또는 TwoWay일 수 있습니다. {x:Bind} defaults to OneTime; {Binding} defaults to OneWay. | 
| UpdateSourceTrigger | `{x:Bind Name, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}` | `{Binding UpdateSourceTrigger=PropertyChanged}` | UpdateSourceTrigger는 Default, LostFocus, 또는 PropertyChanged가 될 수 있습니다. {x:Bind}는 UpdateSourceTrigger=Explicit를 지원하지 않습니다. {x:Bind}에서는 LostFocus 동작을 사용하는 TextBox.Text를 제외한 모든 경우에 PropertyChanged 동작을 사용합니다. |