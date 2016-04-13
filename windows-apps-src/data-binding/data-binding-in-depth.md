---
ms.assetid: 41E1B4F1-6CAF-4128-A61A-4E400B149011
데이터 바인딩 심층 분석
데이터 바인딩은 앱의 UI에서 데이터를 표시하고 선택적으로 해당 데이터와 동기화된 상태를 유지하는 하나의 방법입니다.
---
# 데이터 바인딩 심층 분석

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


** 중요 API **

-   [**Binding 클래스**](https://msdn.microsoft.com/library/windows/apps/BR209820)
-   [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713)
-   [**INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/BR209899)

**참고** 이 항목에서는 데이터 바인딩 기능에 대해 자세히 설명합니다. 간단하고 실용적인 소개는 [데이터 바인딩 개요](data-binding-quickstart.md)를 참조하세요.

 

데이터 바인딩은 앱의 UI에서 데이터를 표시하고 선택적으로 해당 데이터와 동기화된 상태를 유지하는 하나의 방법입니다. 데이터 바인딩은 데이터 문제를 UI 문제와 분리하여 개념 모델을 간소화하고 앱의 가독성, 테스트 용이성 및 유지 관리성을 향상시킬 수 있도록 해줍니다.

이러한 데이터 바인딩을 사용하여 UI가 처음 표시될 때 데이터 원본 값의 변경에 반응하지 않고 해당 값을 표시하기만 하도록 할 수 있습니다. 일회성 바인딩이라는 이 기능은 런타임에 해당 값이 변경되지 않는 데이터에 적합합니다. 또한 값을 "관찰"하여 변경될 경우 UI를 업데이트할 수 있습니다. 단방향 바인딩이라는 이 기능은 읽기 전용 데이터에 적합합니다. 마지막으로 사용자가 UI에서 값을 변경한 경우 해당 값이 데이터 원본에 자동으로 다시 적용되도록 관찰하고 업데이트할 수 있습니다. 양방향 바인딩이라는 이 기능은 읽기-쓰기 데이터에 적합합니다. 예를 들면 다음과 같습니다.

-   일회성 바인딩을 사용하여 현재 사용자의 사진을 [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752)에 바인딩할 수 있습니다.
-   단방향 바인딩을 사용하여 신문 섹션별로 그룹화된 실시간 뉴스 기사 모음에 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878)를 바인딩할 수 있습니다.
-   양방향 바인딩을 사용하여 양식에 있는 고객의 이름에 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683)를 바인딩할 수 있습니다.

바인딩에는 두 종류가 있으며, 일반적으로 둘 다 UI 태그에 있습니다. [{x:Bind} 태그 확장](https://msdn.microsoft.com/library/windows/apps/Mt204783) 또는 [{Binding} 태그 확장](https://msdn.microsoft.com/library/windows/apps/Mt204782)을 사용하도록 선택할 수 있습니다. 동일한 앱에서 이 둘을 혼합하여 사용할 수도 있으며, 동일한 UI 요소에도 마찬가지입니다. {x:Bind}는 Windows 10의 새로운 기능으로, 향상된 성능을 제공합니다. {Binding}에는 추가 기능이 있습니다. 이 항목에 설명된 모든 세부 정보는 명시적으로 설명하지 않더라도 두 종류의 바인딩 모두에 적용됩니다.

**{x:Bind}를 보여 주는 샘플 앱**

-   [{x:Bind} 샘플](http://go.microsoft.com/fwlink/p/?linkid=619989)
-   [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame)
-   [XAML UI 기본 사항 샘플](http://go.microsoft.com/fwlink/p/?linkid=619992)

**{Binding}을 보여 주는 샘플 앱**

-   [Bookstore1](http://go.microsoft.com/fwlink/?linkid=532950) 앱 다운로드
-   [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952) 앱 다운로드

모든 바인딩에 포함된 요소
------------------------------------

-   *바인딩 소스*. 바인딩할 데이터의 소스이며, 해당 값을 UI에 표시할 멤버가 있는 모든 클래스의 인스턴스일 수 있습니다.
-   *바인딩 대상*. 데이터를 표시하는 UI에서 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/BR208706)의 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/BR242362)입니다.
-   *바인딩 개체*. 소스에서 대상으로, 그리고 선택적으로 대상에서 다시 소스로 데이터 값을 전송하는 요소입니다. 바인딩 개체는 XAML 로드 시 [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) 또는 [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) 태그 확장에서 만들어집니다.

다음 섹션에서는 바인딩 소스, 바인딩 대상 및 바인딩 개체에 대해 자세히 알아봅니다. 또한 단추의 콘텐츠를 **HostViewModel**이라는 클래스에 속한 **NextButtonText**라는 문자열 속성에 바인딩하는 예제를 이러한 섹션과 함께 링크합니다.

바인딩 소스
--------------

다음은 바인딩 소스로 사용할 수 있는 클래스의 매우 기본적인 구현입니다.

**참고** Visual C++ 구성 요소 확장(C++/CX)과 함께 [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782)을 사용하는 경우 바인딩 소스 클래스에 [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) 특성을 추가해야 합니다. [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783)를 사용하는 경우 해당 특성을 추가할 필요가 없습니다. 코드 조각은 [세부 정보 보기 추가](data-binding-quickstart.md#adding-a-details-view)를 참조하세요.

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

이 구현의 **HostViewModel**과 해당 속성인 **NextButtonText**는 일회성 바인딩에만 적합합니다. 그러나 단방향 및 양방향 바인딩은 매우 일반적이므로 이러한 종류의 바인딩에서는 바인딩 소스의 데이터 값 변경에 응답하여 UI가 자동으로 업데이트됩니다. 이러한 종류의 바인딩이 제대로 작동하려면 바인딩 개체에서 바인딩 소스를 "관찰할 수 있도록" 확인해야 합니다. 따라서 이 예제에서는 **NextButtonText** 속성에 단방향 또는 양방향 바인딩하려는 경우 해당 속성 값에 대해 런타임에 발생하는 모든 변경 내용을 바인딩 개체에서 관찰할 수 있도록 해야 합니다.

한 가지 방법은 바인딩 소스를 나타내는 클래스를 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/BR242356)에서 파생하고 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/BR242362)를 통해 데이터 값을 노출하는 것입니다. 그러면 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/BR208706)가 관찰 가능하게 됩니다. **FrameworkElements**는 즉시 사용할 수 있는 유용한 바인딩 소스입니다.

클래스를 관찰 가능하게 하고 이미 기본 클래스가 있는 클래스에 필요한 클래스로 만드는 보다 간단한 방법은[**System.ComponentModel.INotifyPropertyChanged**](T:System.ComponentModel.INotifyPropertyChanged)를 구현하는 것입니다. 여기에는 실제로 **PropertyChanged**라는 단일 이벤트의 구현이 포함됩니다. **HostViewModel**을 사용하는 예제는 아래와 같습니다.

**참고** C++/CX의 경우 [**Windows::UI::Xaml::Data::INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/BR209899)를 구현하며, 바인딩 소스 클래스에 [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872)가 있거나 [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878)를 구현해야 합니다.

``` csharp
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
        this.PropertyChanged(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

이제 **NextButtonText** 속성을 관찰할 수 있습니다. 이 속성에 대한 단방향 또는 양방향 바인딩을 작성할 경우(방법은 나중에 설명) 결과 바인딩 개체는 **PropertyChanged** 이벤트를 구독합니다. 이 이벤트가 발생하면 바인딩 개체의 처리기에 변경된 속성 이름이 포함된 인수가 수신됩니다. 이를 통해 바인딩 개체는 다시 읽어야 하는 속성 값을 인식할 수 있습니다.

위에 표시된 패턴을 여러 번 구현할 필요가 없도록 [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame) 샘플("Common" 폴더)에 있는 **BindableBase** 기본 클래스에서 파생할 수 있습니다. 관련 예제는 다음과 같습니다.

``` csharp
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

[
            **String.Empty**](T:System.String) 또는 **null** 인수를 사용하여 **PropertyChanged** 이벤트를 발생시킨 경우 이는 개체에 대한 모든 비인덱서 속성을 다시 읽어야 함을 나타냅니다. 특정 인덱서의 경우 "Item\[*indexer*\]"(여기서 *indexer*는 인덱스 값) 인수, 모든 인덱서의 경우 "Item\[\]" 값을 사용하여 개체에 대한 인덱서 속성이 변경되었음을 나타내는 이벤트를 발생시킬 수 있습니다.

바인딩 소스를 해당 속성에 데이터가 포함된 단일 개체 또는 개체 컬렉션으로 처리할 수 있습니다. C# 및 Visual Basic 코드에서는 런타임에 변경되지 않은 컬렉션을 표시하도록 [**List(Of T)**](T:System.Collections.Generic.List%601)를 구현하는 개체에 일회성으로 바인딩할 수 있습니다. 관찰 가능한 컬렉션(컬렉션에서 항목이 추가 및 제거된 경우 관찰)의 경우에는 대신 [**ObservableCollection(Of T)**](T:System.Collections.ObjectModel.ObservableCollection%601)에 단방향으로 바인딩합니다. C++ 코드에서는 관찰 가능한 컬렉션과 관찰 가능하지 않은 컬렉션 모두에 대해 [**Vector&lt;T&gt;**](https://msdn.microsoft.com/en-us/library/dn858385.aspx)에 바인딩할 수 있습니다. 사용자 컬렉션 클래스에 바인딩하려면 다음 표의 지침을 따르세요.

| 시나리오                                                        | C# 및 VB(CLR)                                                                                                                                                                                                                                                                                                                                                                                                                   | C++/CX                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|-----------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 개체에 바인딩합니다.                                              | 어떤 개체든 가능합니다.                                                                                                                                                                                                                                                                                                                                                                                                                 | 개체는 [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872)을(를) 가지고 있거나 [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878)을(를) 구현해야 합니다.                                                                                                                                                                                                                                                                                                             |
| 바운드 개체에서 속성 변경 업데이트를 가져옵니다.                | 개체는 [**System.ComponentModel. INotifyPropertyChanged**](T:System.ComponentModel.INotifyPropertyChanged)을(를) 구현해야 합니다.                                                                                                                                                                                                                                                                                                         | 개체는 [**Windows.UI.Xaml.Data. INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/BR209899)을(를) 구현해야 합니다.                                                                                                                                                                                                                                                                                                                                                           |
| 컬렉션에 바인딩됩니다.                                           | [**List(Of T)**](T:System.Collections.Generic.List%601)                                                                                                                                                                                                                                                                                                                                                                            | [**Platform::Collections::Vector&lt;T&gt;**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh441570.aspx)                                                                                                                                                                                                                                                                                                                                                                                         |
| 바운드 컬렉션에서 컬렉션 변경 업데이트를 가져옵니다.          | [**ObservableCollection(Of T)**](T:System.Collections.ObjectModel.ObservableCollection%601)                                                                                                                                                                                                                                                                                                                                        | [**Platform::Collections::Vector&lt;T&gt;**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh441570.aspx)                                                                                                                                                                                                                                                                                                                                                                                         |
| 바인딩을 지원하는 컬렉션을 구현합니다.                   | [
            **List(Of T)**](T:System.Collections.Generic.List%601)를 확장하거나 [**IList**](T:System.Collections.IList), [**IList**](T:System.Collections.Generic.IList%601)(Of [**Object**](T:System.Object)), [**IEnumerable**](T:System.Collections.IEnumerable) 또는 [**IEnumerable**](T:System.Collections.Generic.IEnumerable%601)(Of **Object**)를 구현합니다. 제네릭 **IList(Of T)** 및 **IEnumerable(Of T)**에는 바인딩할 수 없습니다. | [
            **IBindableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701979), [**IBindableIterable**](https://msdn.microsoft.com/library/windows/apps/Hh701957), [**IVector**](https://msdn.microsoft.com/library/windows/apps/BR206631)&lt;[**Object**](T:System.Object)^&gt;, [**IIterable**](https://msdn.microsoft.com/library/windows/apps/BR226024)&lt;**Object**^&gt;, **IVector**&lt;[**IInspectable**](https://msdn.microsoft.com/library/BR205821)\*&gt; 또는 **IIterable**&lt;**IInspectable**\*&gt;을 구현합니다. 제네릭 **IVector&lt;T&gt;** 및 **IIterable&lt;T&gt;**에는 바인딩할 수 없습니다. |
| 컬렉션 변경 업데이트를 지원하는 컬렉션을 구현합니다. | [
            **ObservableCollection(Of T)**](T:System.Collections.ObjectModel.ObservableCollection%601)을(를) 확장하거나 제네릭이 아닌 [**IList**](T:System.Collections.IList) 및 [**INotifyCollectionChanged**](T:System.Collections.Specialized.INotifyCollectionChanged)을(를) 구현합니다.                                                                                                                                                               | [
            **IBindableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701979) 및 [**IBindableObservableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701974)을(를) 구현합니다.                                                                                                                                                                                                                                                                                                                       |
| 증분 로드를 지원하는 컬렉션을 구현합니다.       | [
            **ObservableCollection(Of T)**](T:System.Collections.ObjectModel.ObservableCollection%601)을(를) 확장하거나 제네릭이 아닌 [**IList**](T:System.Collections.IList) 및 [**INotifyCollectionChanged**](T:System.Collections.Specialized.INotifyCollectionChanged)을(를) 구현합니다. 또한 [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916)을(를) 구현합니다.                                                          | [
            **IBindableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701979), [**IBindableObservableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701974) 및 [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916)을 구현합니다.                                                                                                                                                                                                                                         |

 
목록 컨트롤을 임의 크기의 데이터 원본에 바인딩하고 증분 로드를 사용하여 고성능을 유지할 수 있습니다. 예를 들어 한 번에 모든 결과를 로드할 필요 없이 목록 컨트롤을 Bing 이미지 쿼리 결과에 바인딩할 수 있습니다. 대신 일부 결과만 즉시 로드하고 필요에 따라 추가 결과를 로드합니다. 증분 로드를 지원하려면 컬렉션 변경 알림을 지원하는 데이터 원본에서 [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916)을(를) 구현해야 합니다. 데이터 바인딩 엔진이 더 많은 데이터를 요청할 경우 데이터 원본는 적절한 요청을 하고 결과를 통합한 다음 UI를 업데이트하기 위해 적절한 알림을 보내야 합니다.

바인딩 대상
--------------

아래 두 예제에서 **Button.Content** 속성은 바인딩 대상이고 해당 값은 바인딩 개체를 선언하는 태그 확장으로 설정됩니다. 먼저 [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783)가 표시된 다음 [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782)이 표시됩니다. 태그에서 바인딩을 선언하는 것이 일반적입니다(편리하고 읽기 쉬우며 도구 사용이 간편함). 그러나 태그를 피하고 필요한 경우 명령을 통해(프로그래밍 방식으로) [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820) 클래스의 인스턴스를 대신 만들 수 있습니다.

<!-- XAML lang specifier not yet supported in OP. Using XML for now. -->
``` xml
<Button Content="{x:Bind ...}" ... /> 
```

``` xml
<Button Content="{Binding ...}" ... /> 
```

Binding object declared using {x:Bind}
--------------------------------------

There's one step we need to do before we author our [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) markup. We need to expose our binding source class from the class that represents our page of markup. We do that by adding a property (of type **HostViewModel** in this case) to our **HostView** page class.

``` csharp
namespace QuizGame.View
{
    public sealed partial class HostView : Page
    {
        public HostView()
        {
            this.InitializeComponent();
            this.ViewModel = new HostViewModel();
        }
    
        public HostViewModel ViewModel { get; set; }
    }
}
``` 이제 바인딩 개체를 선언하는 태그에 대해 좀 더 자세히 알아보겠습니다. 아래 예제에서는 위의 "바인딩 대상" 섹션에서 사용한 것과 동일한 **Button.Content** 바인딩 대상을 사용하여 이 대상이 **HostViewModel.NextButtonText** 속성에 바인딩됨을 보여 줍니다.

``` xml
<Page x:Class="QuizGame.View.HostView" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

Notice the value that we specify for **Path**. This value is interpreted in the context of the page itself, and in this case the path begins by referencing the **ViewModel** property that we just added to the **HostView** page. That property returns a **HostViewModel** instance, and so we can dot into that object to access the **HostViewModel.NextButtonText** property. And we specify **Mode**, to override the [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) default of one-time.

The [**Path**](https://msdn.microsoft.com/library/windows/apps/BR209820-path) property supports a variety of syntax options for binding to nested properties, attached properties, and integer and string indexers. For more info, see [Property-path syntax](https://msdn.microsoft.com/library/windows/apps/Mt185586). Binding to string indexers gives you the effect of binding to dynamic properties without having to implement [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878). For other settings, see [{x:Bind} markup extension](https://msdn.microsoft.com/library/windows/apps/Mt204783).

**Note**  Changes to [**TextBox.Text**](https://msdn.microsoft.com/library/windows/apps/BR209683-text) are sent to a two-way bound source when the [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) loses focus, and not after every user keystroke.

**DataTemplate and x:DataType**

Inside a [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242348) (whether used as an item template, a content template, or a header template), the value of **Path** is not interpreted in the context of the page, but in the context of the data object being templated. So that its bindings can be validated (and efficient code generated for them) at compile-time, a **DataTemplate** needs to declare the type of its data object using **x:DataType**. The example given below could be used as the **ItemTemplate** of an items control bound to a collection of **SampleDataGroup** objects.

``` xml
<DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```

**Weakly-typed objects in your Path**

Consider for example that you have a type named SampleDataGroup, which implements a string property named Title. And you have a property MainPage.SampleDataGroupAsObject, which is of type object but which actually returns an instance of SampleDataGroup. The binding `<TextBlock Text="{x:Bind SampleDataGroupAsObject.Title}"/>` will result in a compile error because the Title property is not found on the type object. The remedy for this is to add a cast to your Path syntax like this: `<TextBlock Text="{x:Bind SampleDataGroupAsObject.(data:SampleDataGroup.Title)}"/>`. Here's another example where Element is declared as object but is actually a TextBlock: `<TextBlock Text="{x:Bind Element.Text}"/>`. And a cast remedies the issue: `<TextBlock Text="{x:Bind Element.(TextBlock.Text)}"/>`.

**데이터가 비동기적으로 로드되는 경우** **{x:Bind}**를 지원하는 코드는 컴파일 시간에 페이지의 partial 클래스에서 생성됩니다. 이러한 파일은 `obj` 폴더(C#의 경우 이름이<view name>`.g.cs`. The generated code includes a handler for your page's [**Loading**](https://msdn.microsoft.com/library/windows/apps/BR208706-loading) event, and that handler calls the **Initialize** method on a generated class that represent's your page's bindings. **Initialize** in turn calls **Update** to begin moving data between the binding source and the target. **Loading** is raised just before the first measure pass of the page or user control. So if your data is loaded asynchronously it may not be ready by the time **Initialize** is called. So, after you've loaded data, you can force one-time bindings to be initialized by calling `this->Bindings->Update();`와 같음)에서 찾을 수 있습니다. 비동기적으로 로드된 데이터에 대해서만 일회성 바인딩이 필요한 경우 이 방법으로 데이터를 초기화하는 것이 단방향 바인딩을 사용하고 변경 내용을 수신 대기하는 것보다 훨씬 비용이 저렴합니다. 데이터가 세분화된 변경을 거치지 않고 특정 작업의 일부로 업데이트될 가능성이 큰 경우 바인딩을 일회성으로 만들고 언제든지 **Update**를 호출하여 강제로 수동 업데이트를 수행할 수 있습니다.

**제한 사항** 

**{x:Bind}**는 JSON 개체의 사전 구조를 탐색하는 것과 같은 런타임에 바인딩되는 시나리오에는 적합하지 않을 뿐만 아니라, 속성 이름에 대한 어휘 일치를 기반으로 한 약한 형식의 입력인 덕 타이핑(duck typing)("오리처럼 걷고 헤엄치고 꽥꽥거리면 그것은 오리입니다.")에도 적합하지 않습니다. 덕 타이핑(duck typing)을 사용할 경우 Age 속성에 대한 바인딩은 Person 또는 Wine 개체도 동일하게 만족합니다. 이러한 시나리오의 경우 **{Binding}**을 사용하세요.

{Binding} ---------------------------------------

[{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782)을 사용하여 선언된 개체를 바인딩하는 것은 기본적으로 태그 페이지의 [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713)에 바인딩하는 것으로 가정합니다. 따라서 페이지의 **DataContext**를 바인딩 소스 클래스(이 예제의 경우 **HostViewModel** 형식)의 인스턴스로 설정합니다. 아래 예제에서는 바인딩 개체를 선언하는 태그를 보여 줍니다. 위의 "바인딩 대상" 섹션에서 사용한 것과 동일한 **Button.Content** 바인딩 대상을 사용하고 **HostViewModel.NextButtonText** 속성에 바인딩합니다.

``` xml
<Page xmlns:viewmodel="using:QuizGame.ViewModel" ... >
    <Page.DataContext>
        <viewmodel:HostViewModel/>
    </Page.DataContext>
    ...
    <Button Content="{Binding Path=NextButtonText}" ... />
</Page>
```

Notice the value that we specify for **Path**. This value is interpreted in the context of the page's [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713), which in this example is set to an instance of **HostViewModel**. The path references the **HostViewModel.NextButtonText** property. We can omit **Mode**, because the [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) default of one-way works here.

The default value of [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) for a UI element is the inherited value of its parent. You can of course override that default by setting **DataContext** explicitly, which is in turn inherited by children by default. Setting **DataContext** explicitly on an element is useful when you want to have multiple bindings that use the same source.

A binding object has a **Source** property, which defaults to the [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) of the UI element on which the binding is declared. You can override this default by setting **Source**, **RelativeSource**, or **ElementName** explicitly on the binding (see [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) for details).

Inside a [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242348), the [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) is set to the data object being templated. The example given below could be used as the **ItemTemplate** of an items control bound to a collection of any type that has string properties named **Title** and **Description**.

``` xml
<DataTemplate x:Key="SimpleItemTemplate">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{Binding Title}"/>
      <TextBlock Text="{Binding Description"/>
    </StackPanel>
  </DataTemplate>
```

**Note**  By default, changes to [**TextBox.Text**](https://msdn.microsoft.com/library/windows/apps/BR209683-text) are sent to a two-way bound source when the [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) loses focus. To cause changes to be sent after every user keystroke, set **UpdateSourceTrigger** to **PropertyChanged** on the binding in markup. You can also completely take control of when changes are sent to the source by setting **UpdateSourceTrigger** to **Explicit**. You then handle events on the text box (typically [**TextBox.TextChanged**](https://msdn.microsoft.com/library/windows/apps/BR209683-textchanged)), call [**GetBindingExpression**](https://msdn.microsoft.com/library/windows/apps/BR208706-getbindingexpression) on the target to get a [**BindingExpression**](https://msdn.microsoft.com/library/windows/apps/BR209820expression) object, and finally call [**BindingExpression.UpdateSource**](https://msdn.microsoft.com/library/windows/apps/BR209820expression-updatesource) to programmatically update the data source.

The [**Path**](https://msdn.microsoft.com/library/windows/apps/BR209820-path) property supports a variety of syntax options for binding to nested properties, attached properties, and integer and string indexers. For more info, see [Property-path syntax](https://msdn.microsoft.com/library/windows/apps/Mt185586). Binding to string indexers gives you the effect of binding to dynamic properties without having to implement [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878). The [**ElementName**](https://msdn.microsoft.com/library/windows/apps/BR209820-elementname) property is useful for element-to-element binding. The [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/BR209820-relativesource) property has several uses, one of which is as a more powerful alternative to template binding inside a [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/BR209391). For other settings, see [{Binding} markup extension](https://msdn.microsoft.com/library/windows/apps/Mt204782) and the [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820) class.

What if the source and the target are not the same type?
--------------------------------------------------------

If you want to control the visibility of a UI element based on the value of a boolean property, or if you want to render a UI element with a color that's a function of a numeric value's range or trend, or if you want to display a date and/or time value in a UI element property that expects a string, then you'll need to convert values from one type to another. There will be cases where the right solution is to expose another property of the right type from your binding source class, and keep the conversion logic encapsulated and testable there. But that isn't flexible nor scalable when you have large numbers, or large combinations, of source and target properties. In that case you'll want to use something known as a value converter. This section describes how to implement and consume a value converter.

Here's a value converter, suitable for a one-time or a one-way binding, that converts a [**DateTime**](T:System.DateTime) value to a string value containing the month. The class implements [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/BR209903).

``` csharp
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

``` vbnet
Public Class DateToStringConverter
    Implements IValueConverter

    ' Define the Convert method to change a DateTime object to
    ' a month string.
    Public Function Convert(ByVal value As Object, -
        ByVal targetType As Type, ByVal parameter As Object, -
        ByVal language As String) As Object -
        Implements IValueConverter.Convert

        ' value is the data from the source object.
        Dim thisdate As DateTime = CType(value, DateTime)
        Dim monthnum As Integer = thisdate.Month
        Dim month As String
        Select Case (monthnum)
            Case 1
                month = "January"
            Case 2
                month = "February"
            Case Else
                month = "Month not found"
        End Select
        ' Return the value to pass to the target.
        Return month

    End Function

    ' ConvertBack is not implemented for a OneWay binding.
    Public Function ConvertBack(ByVal value As Object, -
        ByVal targetType As Type, ByVal parameter As Object, -
        ByVal language As String) As Object -
        Implements IValueConverter.ConvertBack

        Throw New NotImplementedException

    End Function
End Class
``` 그리고 바인딩 개체 태그에서 해당 값 변환기를 사용할 방법을 다음과 같습니다.

``` xml
<UserControl.Resources>
  <local:DateToStringConverter x:Key="Converter1"/>
</UserControl.Resources>

...

<TextBlock Grid.Column="0" 
  Text="{x:Bind ViewModel.Month, Converter={StaticResource Converter1}}"/>

<TextBlock Grid.Column="0" 
  Text="{Binding Month, Converter={StaticResource Converter1}}"/>
```

The binding engine calls the [**Convert**](https://msdn.microsoft.com/library/windows/apps/BR209903-convert) and [**ConvertBack**](https://msdn.microsoft.com/library/windows/apps/BR209903-convertback) methods if the [**Converter**](https://msdn.microsoft.com/library/windows/apps/BR209820-converter) parameter is defined for the binding. When data is passed from the source, the binding engine calls **Convert** and passes the returned data to the target. When data is passed from the target (for a two-way binding), the binding engine calls **ConvertBack** and passes the returned data to the source.

The converter also has optional parameters: [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/BR209820-converterlanguage), which allows specifying the language to be used in the conversion, and [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/BR209820-converterparameter), which allows passing a parameter for the conversion logic. For an example that uses a converter parameter, see [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/BR209903).

**Note**  If there is an error in the conversion, do not throw an exception. Instead, return [**DependencyProperty.UnsetValue**](https://msdn.microsoft.com/library/windows/apps/BR242362-unsetvalue), which will stop the data transfer.

To display a default value to use whenever the binding source cannot be resolved, set the **FallbackValue** property on the binding object in markup. This is useful to handle conversion and formatting errors. It is also useful to bind to source properties that might not exist on all objects in a bound collection of heterogeneous types.

If you bind a text control to a value that is not a string, the data binding engine will convert the value to a string. If the value is a reference type, the data binding engine will retrieve the string value by calling [**ICustomPropertyProvider.GetStringRepresentation**](https://msdn.microsoft.com/library/windows/apps/BR209878-getstringrepresentation) or [**IStringable.ToString**](https://msdn.microsoft.com/library/Dn302136) if available, and will otherwise call [**Object.ToString**](M:System.Object.ToString). Note, however, that the binding engine will ignore any **ToString** implementation that hides the base-class implementation. Subclass implementations should override the base class **ToString** method instead. Similarly, in native languages, all managed objects appear to implement [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878) and [**IStringable**](https://msdn.microsoft.com/library/Dn302135). However, all calls to **GetStringRepresentation** and **IStringable.ToString** are routed to **Object.ToString** or an override of that method, and never to a new **ToString** implementation that hides the base-class implementation.

Resource dictionaries with {x:Bind}
-----------------------------------

The [{x:Bind} markup extension](https://msdn.microsoft.com/library/windows/apps/Mt204783) depends on code generation, so it needs a code-behind file containing a constructor that calls **InitializeComponent** (to initialize the generated code). You re-use the resource dictionary by instantiating its type (so that **InitializeComponent** is called) instead of referencing its filename. Here's an example of what to do if you have an existing resource dictionary and you want to use {x:Bind} in it.

TemplatesResourceDictionary.xaml

``` xml
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

``` csharp
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
``` MainPage.xaml ``` xml
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

Event binding and ICommand
--------------------------

[{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) supports a feature called event binding. With this feature, you can specify the handler for an event using a binding, which is an additional option on top of handling events with a method on the code-behind file. Let's say you have a **RootFrame** property on your **MainPage** class.

``` csharp
    public sealed partial class MainPage : Page
    {
        ....    
        public Frame RootFrame { get { return Window.Current.Content as Frame; } }
    }
``` 이와 같은 **RootFrame** 속성에서 반환되는 **Frame** 개체의 메서드에 단추의 **Click** 이벤트를 바인딩할 수 있습니다. 또한 단추의 **IsEnabled** 속성을 동일한 **Frame**의 다른 멤버에 바인딩할 수 있습니다.

``` xml
    <AppBarButton Icon="Forward" IsCompact="True"
    IsEnabled="{x:Bind RootFrame.CanGoForward, Mode=OneWay}"
    Click="{x:Bind RootFrame.GoForward}"/>
```

Overloaded methods cannot be used to handle an event with this technique. Also, if the method that handles the event has parameters then they must all be assignable from the types of all of the event's parameters, respectively. In this case, [**Frame.GoForward**](https://msdn.microsoft.com/library/windows/apps/BR242693) is not overloaded and it has no parameters (but it would still be valid even if it took two **object** parameters). [**Frame.GoBack**](https://msdn.microsoft.com/library/windows/apps/Dn996568) is overloaded, though, so we can't use that method with this technique.

The event binding technique is similar to implementing and consuming commands (a command is a property that returns an object that implements the [**ICommand**](T:System.Windows.Input.ICommand) interface). Both [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) and [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) work with commands. So that you don't have to implement the command pattern multiple times, you can use the **DelegateCommand** helper class that you'll find in the [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame) sample (in the "Common" folder).

## Binding to a collection of folders or files

You can use the APIs in the [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346) namespace to retrieve folder and file data. However, the various **GetFilesAsync**, **GetFoldersAsync**, and **GetItemsAsync** methods do not return values that are suitable for binding to list controls. Instead, you must bind to the return values of the [**GetVirtualizedFilesVector**](https://msdn.microsoft.com/library/windows/apps/Hh701422), [**GetVirtualizedFoldersVector**](https://msdn.microsoft.com/library/windows/apps/Hh701428), and [**GetVirtualizedItemsVector**](https://msdn.microsoft.com/library/windows/apps/Hh701430) methods of the [**FileInformationFactory**](https://msdn.microsoft.com/library/windows/apps/BR207501) class. The following code example from the [StorageDataSource and GetVirtualizedFilesVector sample](http://go.microsoft.com/fwlink/p/?linkid=228621) shows the typical usage pattern. Remember to declare the **picturesLibrary** capability in your app package manifest, and confirm that there are pictures in your Pictures library folder.

``` csharp
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
``` 일반적으로 이 접근 방식을 사용하여 파일 및 폴더 정보의 읽기 전용 보기를 만듭니다. 예를 들어 사용자가 음악 보기에서 노래를 평가할 수 있도록 파일 및 폴더 속성에 대한 양방향 바인딩을 만들 수 있습니다. 그러나 적절한 **SavePropertiesAsync** 메서드(예: [**MusicProperties.SavePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/BR207760))를 호출할 때까지 변경 사항은 지속되지 않습니다. 항목이 초점을 잃으면 선택 초기화가 트리거되므로 변경 사항을 적용해야 합니다.

이 기술을 사용한 양방향 바인딩은 음악과 같이 인덱싱된 위치에서만 작동합니다. [
            **FolderInformation.GetIndexedStateAsync**](https://msdn.microsoft.com/library/windows/apps/BR207627) 메서드를 호출하여 위치가 인덱싱되었는지 여부를 확인할 수 있습니다.

가상화된 벡터는 해당 값을 채우기 전에 일부 항목에 대해 **null**을 반환할 수도 있습니다. 예를 들어 가상화된 벡터에 바인딩된 목록 컨트롤의 [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/BR209770) 값을 사용하기 전에 **null**을 확인하거나 [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/BR209768)를 대신 사용해야 합니다.

키로 그룹화한 데이터에 바인딩 -------------------------------- 항목의 단순 컬렉션(예: **BookSku** 클래스로 표현되는 책) 및 공용 속성을 사용한 항목 그룹화(예: **BookSku.AuthorName** 속성)을 가져오는 경우 그룹화된 데이터가 결과로 호출됩니다. 데이터를 그룹화한 경우 해당 데이터는 더 이상 단순 컬렉션이 아닙니다. 그룹화된 데이터는 각 그룹 개체에 a) 키 및 b) 속성이 해당 키와 일치하는 항목의 컬렉션이 있는 그룹 개체의 컬렉션입니다. 책을 다시 예로 들어 저자 이름별로 책을 그룹화하면 각 그룹에 a) 저자 이름인 키 및 b) **AuthorName** 속성이 그룹의 키와 일치하는 **BookSku** 컬렉션이 있는 저자 이름 그룹 컬렉션이 생성됩니다.

일반적으로 컬렉션을 표시하려면 항목 컨트롤의 [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/BR242828)(예: [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 또는 [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705))를 컬렉션을 반환하는 속성에 직접 바인딩합니다. 항목의 단순 컬렉션이 없는 경우에는 특별히 수행해야 하는 작업이 없습니다. 그러나 그룹화된 데이터에 바인딩하는 경우처럼 그룹 개체의 컬렉션인 경우 항목 컨트롤과 바인딩 소스 사이에 있는 [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833)라는 중간 개체의 서비스가 필요합니다. **CollectionViewSource**를 그룹화된 데이터를 반환하는 속성에 바인딩하고, 항목 컨트롤을 **CollectionViewSource**에 바인딩합니다. **CollectionViewSource**의 추가적인 부가 가치는 현재 항목을 추적하므로 둘 이상의 항목 컨트롤을 모두 동일한 **CollectionViewSource**에 바인딩하여 동기화된 상태로 유지할 수 있다는 점입니다. 또한 [**CollectionViewSource.View**](https://msdn.microsoft.com/library/windows/apps/BR209833-view) 속성에서 반환되는 개체의 [**ICollectionView.CurrentItem**](https://msdn.microsoft.com/library/windows/apps/BR209857) 속성을 통해 현재 항목에 프로그래밍 방식으로 액세스할 수 있습니다.

[
            **CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833)의 그룹화 기능을 활성화하려면 [**IsSourceGrouped**](https://msdn.microsoft.com/library/windows/apps/BR209833-issourcegrouped)를 **true**로 설정합니다. [
            **ItemsPath**](https://msdn.microsoft.com/library/windows/apps/BR209833-itemspath) 속성을 설정해야 하는지 여부는 정확히 그룹 개체를 작성하는 방법에 따라 결정됩니다. 그룹 개체를 작성하는 방법에는 "is-a-group" 패턴과 "has-a-group" 패턴의 두 가지 방법이 있습니다. "is-a-group" 패턴에서는 그룹 개체가 컬렉션 형식(예: **List&lt;T&gt;**)에서 파생되므로 실제로 그룹 개체는 항목 그룹 자체입니다. 이 패턴을 사용하면 **ItemsPath**를 설정하지 않아도 됩니다. "has-a-group" 패턴에서는 그룹 개체가 하나 이상의 컬렉션 형식(예: **List&lt;T&gt;**) 속성을 가지므로 그룹에 속성 형식의 항목 그룹 "하나가 있습니다"(또는 여러 속성 형식의 여러 항목 그룹). 이 패턴을 사용하면 **ItemsPath**를 항목 그룹을 포함하는 속성 이름으로 설정해야 합니다.

아래 예제에서는 "has-a-group" 패턴을 보여 줍니다. 페이지 클래스에서 뷰 모델의 인스턴스를 반환하는 [**ViewModel**](https://msdn.microsoft.com/library/windows/apps/BR208713)이라는 속성이 있습니다. [
            **CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833)는 뷰 모델의 **Authors** 속성(**Authors**는 그룹 개체의 컬렉션)에 바인딩되며, 그룹화된 항목을 포함하는 **Author.BookSkus** 속성임을 지정합니다. 마지막으로 [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705)는 **CollectionViewSource**에 바인딩되며, 그룹의 항목을 렌더링할 수 있도록 그룹 스타일이 정의되어 있습니다.

``` csharp
    <Page.Resources>
        <CollectionViewSource
        x:Name="AuthorHasACollectionOfBookSku"
        Source="{x:Bind ViewModel.Authors}"
        IsSourceGrouped="true"
        ItemsPath="BookSkus"/>
    </Page.Resources>
    ...

    <GridView
    ItemsSource="{Binding Source={StaticResource AuthorHasACollectionOfBookSku}}" ...>
        <GridView.GroupStyle>
            <GroupStyle
                HeaderTemplate="{StaticResource AuthorGroupHeaderTemplateWide}" ... />
        </GridView.GroupStyle>
    </GridView>
```

Note that the [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/BR242828) must use [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) (and not [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783)) because it needs to set the **Source** property to a resource. To see the above example in the context of the complete app, download the [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952) sample app. Unlike the markup shown above, [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952) uses {Binding} exclusively.

You can implement the "is-a-group" pattern in one of two ways. One way is to author your own group class. Derive the class from **List&lt;T&gt;** (where *T* is the type of the items). For example, `public class Author : List<BookSku>`. 두 번째 방법은 [LINQ](http://msdn.microsoft.com/library/bb397926.aspx) 식을 사용하여 **BookSku** 항목의 유사한 속성 값에서 그룹 개체(및 그룹 클래스)를 동적으로 만드는 것입니다. 이 방법(항목의 단순 목록만 유지하고 즉석에서 그룹화)은 클라우드 서비스에서 데이터에 액세스하는 앱에 일반적입니다. **Author** 및 **Genre**와 같이 특정 그룹 클래스가 필요 없는 책을 저자 또는 장르 등으로 유연하게 그룹화할 수 있게 됩니다.

아래 예제에서는 [LINQ](http://msdn.microsoft.com/library/bb397926.aspx)를 사용하는 "is-a-group" 패턴을 보여 줍니다. 여기에서는 장르별로 책을 그룹화합니다. 따라서 그룹 머리글에 장르 이름으로 책이 표시됩니다. 이는 그룹 [**Key**](P:System.Linq.IGrouping%602.Key) 값에 대한 "Key" 속성 경로로 표시됩니다.

``` csharp
    using System.Linq;

    ...

    private IOrderedEnumerable<IGrouping<string, BookSku>> 장르; public IOrderedEnumerable<IGrouping<string, BookSku>> 장르
    {
        get
        {
            if (this.genres == null)
            {
                this.genres = from book in this.bookSkus
                group book by book.genre into grp
                orderby grp.Key select grp;
            }
            return this.genres;
        }
    } ```

Remember that when using [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) with data templates we need to indicate the type being bound to by setting an **x:DataType** value. If the type is generic then we can't express that in markup so we need to use [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) instead in the group style header template.

``` xml
    <Grid.Resources>
        <CollectionViewSource x:Name="GenreIsACollectionOfBookSku"
        Source="{Binding Genres}"
        IsSourceGrouped="true"/>
    </Grid.Resources>
    <GridView ItemsSource="{Binding Source={StaticResource GenreIsACollectionOfBookSku}}">
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

A [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/Hh702601) control is a great way for your users to view and navigate grouped data. The [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952) sample app illustrates how to use the **SemanticZoom**. In that app, you can view a list of books grouped by author (the zoomed-in view) or you can zoom out to see a jump list of authors (the zoomed-out view). The jump list affords much quicker navigation than scrolling through the list of books. The zoomed-in and zoomed-out views are actually **ListView** or **GridView** controls bound to the same **CollectionViewSource**.

![An illustration of a SemanticZoom](images/sezo.png)

When you bind to hierarchical data—such as subcategories within categories—you can choose to display the hierarchical levels in your UI with a series of items controls. A selection in one items control determines the contents of subsequent items controls. You can keep the lists synchronized by binding each list to its own [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) and binding the **CollectionViewSource** instances together in a chain. This is called a master/details (or list/details) view. For more info, see [How to bind to hierarchical data and create a master/details view](how-to-bind-to-hierarchical-data-and-create-a-master-details-view.md).

Diagnosing and debugging data binding problems
-----------------------------------------------

Your binding markup contains the names of properties (and, for C#, sometimes fields and methods). So when you rename a property, you'll also need to change any binding that references it. Forgetting to do that leads to a typical example of a data binding bug, and your app either won't compile or won't run correctly.

The binding objects created by [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) and [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) are largely functionally equivalent. But {x:Bind} has type information for the binding source, and it generates source code at compile-time. With {x:Bind} you get the same kind of problem detection that you get with the rest of your code. That includes compile-time validation of your binding expressions, and debugging by setting breakpoints in the source code generated as the partial class for your page. These classes can be found in the files in your `obj` folder, with names like (for C#) `<view name>.g.cs`). 바인딩에 문제가 있는 경우 Microsoft Visual Studio 디버거에서 **처리되지 않은 예외 발생 시 중단**을 설정합니다. 디버거가 이 시점에 실행을 중단하므로 잘못된 것을 디버그할 수 있습니다. {x:Bind}에 의해 생성된 코드는 바인딩 소스 코드 그래프의 각 부분에 대해 동일한 패턴을 따르며, **호출 스택** 창의 정보를 사용하여 문제에 이르는 호출 시퀀스를 결정할 수 있습니다.

[
            {Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782)에는 바인딩 소스에 대한 형식 정보가 없습니다. 그러나 디버거를 연결한 상태로 앱을 실행하면 Visual Studio의 **출력** 창에 모든 바인딩 오류가 표시됩니다.

코드에서 바인딩 만들기 -------------------------

**참고** 이 섹션은 [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782)에만 적용됩니다. 코드에서 [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) 바인딩을 만들 수 없기 때문입니다. 그러나 모든 종속성 속성에 대한 변경 알림을 등록할 수 있는 [**DependencyProperty.RegisterPropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/BR242356-registerpropertychangedcallback)을 사용해서도 {x:Bind}를 사용할 때와 동일한 이점을 얻을 수 있습니다.

XAML 대신 절차 코드를 사용하여 UI 요소를 데이터에 연결할 수도 있습니다. 이렇게 하려면 새로운 [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820) 개체를 만들고, 적절한 속성을 설정하고, [**FrameworkElement.SetBinding**](https://msdn.microsoft.com/library/windows/apps/BR208706-setbinding) 또는 [**BindingOperations.SetBinding**](https://msdn.microsoft.com/library/windows/apps/BR209820operations-setbinding)을 호출합니다. 런타임에 바인딩 속성 값을 선택하거나 여러 컨트롤 간에 단일 바인딩을 공유하려는 경우 프로그래밍 방식으로 바인딩을 만드는 것이 유용합니다. 그러나 **SetBinding**을 호출한 후에는 바인딩 속성 값을 변경할 수 없습니다.

다음 예제는 코드에서 바인딩을 구현하는 방법을 보여 줍니다.

``` xml
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

``` vbnet
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
``` {x: Bind} 및 {Binding} 기능 비교 ------------------------------------------ | 기능 | {x:Bind} | {Binding} | 참고 사항 | |---------|----------|-----------|-------| | 경로가 기본 속성임 | `{x:Bind a.b.c}` | `{Binding a.b.c}` | | 
| 경로 속성 | `{x:Bind Path=a.b.c}` | `{Binding Path=a.b.c}` | x:Bind에서는 경로의 루트가 기본적으로 DataContext가 아니라 Page에서 지정됩니다. | | 인덱서 | `{x:Bind Groups[2].Title}` | `{Binding Groups[2].Title}` | 컬렉션의 지정된 항목에 바인딩됩니다. 정수 기반 인덱스만 지원됩니다. | | 연결된 속성 | `{x:Bind Button22.(Grid.Row)}` | `{Binding Button22.(Grid.Row)}` | 연결된 속성은 괄호 안에 지정됩니다. XAML 네임스페이스에서 속성이 선언되지 않은 경우 문서 헤드의 코드 네임스페이스에 매핑되어야 하는 xml 네임스페이스를 접두사로 사용합니다. | | 캐스팅 | `{x:Bind groups[0].(data:SampleDataGroup.Title)}` | 필요하지 않음< | 캐스트는 괄호 안에 지정됩니다. XAML 네임스페이스에서 속성이 선언되지 않은 경우 문서 헤드의 코드 네임스페이스에 매핑되어야 하는 xml 네임스페이스를 접두사로 사용합니다. | 
| 변환기 | `{x:Bind IsShown, Converter={StaticResource BoolToVisibility}}` | `{Binding IsShown, Converter={StaticResource BoolToVisibility}}` | 변환기는 Page/ResourceDictionary의 루트 또는 App.xaml에서 선언해야 합니다. | 
| ConverterParameter, ConverterLanguage | `{x:Bind IsShown, Converter={StaticResource BoolToVisibility}, ConverterParameter=One, ConverterLanguage=fr-fr}` | `{Binding IsShown, Converter={StaticResource BoolToVisibility}, ConverterParameter=One, ConverterLanguage=fr-fr}` | 변환기는 Page/ResourceDictionary 또는 App.xaml에서 선언해야 합니다. | 
| TargetNullValue | `{x:Bind Name, TargetNullValue=0}` | `{Binding Name, TargetNullValue=0}` | 바인딩 식의 리프가 null일 때 사용됩니다. 문자열 값에 작은따옴표를 사용합니다. | 
| FallbackValue | `{x:Bind Name, FallbackValue='empty'}` | `{Binding Name, FallbackValue='empty'}` | 바인딩에 대한 경로의 일부(리프 제외)가 null일 때 사용됩니다. | 
| ElementName | `{x:Bind slider1.Value}` | `{Binding Value, ElementName=slider1}` | {x:Bind}를 사용하면 필드에 바인딩됩니다. 경로의 루트가 기본적으로 Page에서 지정되므로 해당 필드를 통해 모든 명명된 요소에 액세스할 수 있습니다. | 
| RelativeSource: Self | `<Rectangle x:Name="rect1" Width="200" Height="{x:Bind rect1.Width}" ... />` | `<Rectangle Width="200" Height="{Binding Width, RelativeSource={RelativeSource Self}}" ... />` | With {x:Bind}, name the element and use its name in Path. | 
| RelativeSource: TemplatedParent | Not supported | `{Binding <path>RelativeSource = {RelativeSource TemplatedParent}}' | 일반 템플릿 바인딩은 대부분 컨트롤 템플릿에서 사용할 수 있습니다. 하지만 변환기 또는 양방향 바인딩을 사용해야 하는 경우 TemplatedParent를 사용합니다.< | 
| 소스 | 지원되지 않음 | `<ListView ItemsSource="{Binding Orders, Source={StaticResource MyData}}"/>` | For {x:Bind} use a property or a static path instead. | 
| Mode | `{x:Bind Name, Mode=OneWay}` | `{Binding Name, Mode=TwoWay}` | Mode can be OneTime, OneWay, or TwoWay. {x:Bind} defaults to OneTime; {Binding} defaults to OneWay. | 
| UpdateSourceTrigger | Not supported | `<Binding UpdateSourceTrigger="[Default | PropertyChanged | Explicit]"/>` | {x:Bind}에서는 손실된 포커스에서 소스를 업데이트할 때까지 기다리는 TextBox.Text를 제외하고 모든 경우에 PropertyChanged 동작을 사용합니다. | 




<!--HONumber=Mar16_HO1-->


