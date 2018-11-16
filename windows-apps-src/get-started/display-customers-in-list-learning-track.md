---
author: QuinnRadich
title: 학습 트랙 - 목록에서 고객 표시
description: 목록에 있는 고객 개체 컬렉션을 표시하기 위해 수행해야 할 사항에 대해 알아봅니다.
ms.author: quradic
ms.date: 05/07/2018
ms.topic: article
keywords: 시작, uwp, windows 10, 학습 트랙, 데이터 바인딩, 목록
ms.localizationpriority: medium
ms.openlocfilehash: 4e58231d644616a088eb06f24a2465c240e10c16
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6978122"
---
# <a name="display-customers-in-a-list"></a>목록에서 고객 표시

UI에서 실제 데이터를 표시하고 조작하는 것은 매우 많은 앱의 기능에 중요합니다. 이 문서는 목록에 고객 개체 컬렉션을 표시하기 전에 알아야 할 사항을 보여 줍니다.

이 문서는 자습서가 아닙니다. 자습서를 원하는 경우 단계별 안내식 환경을 제공하는 우리의 [데이터 바인딩 자습서](../data-binding/xaml-basics-data-binding.md)를 참조하세요.

데이터 바인딩이 무엇인지와 작동 방식에 대해 빠르게 언급하는 것으로 시작하겠습니다. 그런 다음 UI에 **ListView**를 추가하고, 데이터 바인딩을 추가하고, 추가 기능으로 데이터 바인딩을 사용자 지정하겠습니다. 

## <a name="what-do-you-need-to-know"></a>알아야 할 사항?

데이터 바인딩은 UI에 앱의 데이터를 표시하는 방법입니다. 이렇게 하면 앱에서 *문제를 분리*하고 UI를 다른 코드와 개별적으로 유지할 수 있습니다. 보다 읽고 유지하기 쉬운 명확한 개념적 모델을 만듭니다.

모든 데이터 바인딩에는 두 부분이 있습니다.

* 바인딩할 데이터를 제공하는 원본입니다.
* 데이터가 표시되는 UI의 대상입니다.

데이터 바인딩을 구현하려면 바인딩에 데이터를 제공하는 소스에 코드를 추가해야 합니다. 또한 데이터 원본 속성을 지정하기 위해 XAML에 두 개의 태그 확장을 추가해야 합니다. 두 가지의 주요 차이는 다음과 같습니다.

* [**x:Bind**](../xaml-platform/x-bind-markup-extension.md)는 강력한 형식이며 성능 향상을 위해 컴파일 시 코드를 생성합니다. x:Bind은 기본적으로 변경되지 않는 읽기 전용 데이터의 빠른 디스플레이를 위해 최적화되는 일회성 바인딩입니다.
* [**바인딩**](../xaml-platform/binding-markup-extension.md)은 약한 형식이며 런타임 시 조립됩니다. 따라서 x:Bind보다 성능이 저하됩니다. 대부분의 경우 바인딩 대신 x:Bind를 사용해야 합니다. 그러나 이전 코드에서 바인딩이 발생할 수 있습니다. 바인딩은 기본적으로 원본에서 변경할 수 있는 읽기 전용 데이터를 최적화하는 단방향 데이터 전송입니다.

가능한 때마다 **x:Bind**를 사용하는 것이 좋으며 이 문서의 조각에서 이를 설명하겠습니다. 차이점에 대한 자세한 내용은 [{x:Bind} 및 {Binding} 기능 비교](../data-binding/data-binding-in-depth.md#xbind-and-binding-feature-comparison)를 참조하세요.

## <a name="create-a-data-source"></a>데이터 원본 만들기

첫째, 고객 데이터를 나타내는 클래스가 필요합니다. 참조 지점을 제공하기 위해 핵심 예시에서 이 프로세스를 설명하겠습니다.

```csharp
public class Customer
{
    public string Name { get; set; }
}
```

## <a name="create-a-list"></a>목록 만들기

모든 고객을 표시하려면 먼저 고객을 고정할 목록을 만들어야 합니다. [목록 보기](../design/controls-and-patterns/listview-and-gridview.md)는 이 작업에 이상적인 기본 XAML 컨트롤입니다. 사용자 ListView는 현재 페이지에서 위치가 필요하며 곧 해당 **ItemSource** 속성에 대한 값이 필요합니다.

```xaml
<ListView ItemsSource=""
    HorizontalAlignment="Center"
    VerticalAlignment="Center"/>
```

ListView에 데이터를 바인딩한 후 설명서로 돌아가서 모양 및 레이아웃을 요구에 가장 잘 맞도록 사용자 지정해 볼 수 있습니다.

## <a name="bind-data-to-your-list"></a>목록에 데이터 바인딩

이제 바인딩을 고정하기 위한 기본 UI를 만들었으므로 바인딩을 제공하기 위해 원본을 구성해야 합니다. 이를 수행하는 방법의 예시는 다음과 같습니다.

```csharp
public sealed partial class MainPage : Page
{
    public ObservableCollection<Customer> Customers { get; }
        = new ObservableCollection<Customer>();

    public MainPage()
    {
        this.InitializeComponent();
          // Add some customers
        this.Customers.Add(new Customer() { Name = "NAME1"});
        this.Customers.Add(new Customer() { Name = "NAME2"});
        this.Customers.Add(new Customer() { Name = "NAME3"});
    }
}
```
```xaml
<ListView ItemsSource="{x:Bind Customers}"
    HorizontalAlignment="Center"
    VerticalAlignment="Center">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:Customer">
            <TextBlock Text="{x:Bind Name}"/>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

[데이터 바인딩 개요](../data-binding/data-binding-quickstart.md#binding-to-a-collection-of-items)는 항목의 컬렉션에 대한 바인딩 섹션에서 유사한 문제를 안내합니다. 여기 예에서 중요한 다음 단계를 보여 줍니다.

* UI의 코드 뒤에서 **ObservableCollection<T>** 형식의 속성을 만들어 고객 개체를 고정합니다.
* ListView의 **ItemSource** 해당 속성에 바인딩합니다.
* 각 항목이 목록에 표시되는 방식을 구성하는 ListView에 대한 기본 **ItemTemplate**을 제공합니다.

문서 레이아웃을 사용자 지정하거나 선택 항목을 추가하거나 방금 만든 **DataTemplate**을 조정하려면 자유롭게 [목록 보기](../design/controls-and-patterns/listview-and-gridview.md) 문서를 다시 살펴보세요. 하지만 고객을 편집하려는 경우에는 어떻게 하나요?

## <a name="edit-your-customers-through-the-ui"></a>UI를 통해 고객 편집

목록에 고객을 표시했지만 데이터 B=바인딩으로 더 많은 것을 수행할 수 있습니다. UI에서 어떻게 바로 데이터를 편집할 수 있나요? 이렇게 하려면 먼저에 데이터 바인딩의 세 가지 모드에 대해 살펴보겠습니다.

* *일회성*: 이 데이터 바인딩은 한 번만 활성화되며 변경에 대응하지 않습니다.
* *단방향*: 이 데이터 바인딩은 데이터 원본의 변경 내용으로 UI를 업데이트합니다.
* *양방향*: 이 데이터 바인딩은 데이터 원본의 변경 내용으로 UI를 업데이트하고 UI 내의 변경 사항으로 데이터를 업데이트합니다.

앞에서 코드 조각을 따랐다면 귀하가 만든 바인딩을 x:Bind를 사용하며 모드를 지정하지 않고 이를 일회성 바인딩으로 만듭니다. UI에서 바로 고객을 편집하려는 경우 데이터 변경이 고객 개체에도 다시 전달되도록 하기 위해 양방향 바인딩으로 변경해야 합니다. [데이터 바인딩 심층 분석](../data-binding/data-binding-in-depth.md)에 자세한 내용이 나와 있습니다.

데이터 원본이 변경되는 경우에도 양방향 바인딩 또한 UI를 업데이트합니다. 이를 수행하기 위해 원본에서 [**INotifyPropertyChanged**](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged(d=robot).aspx)를 구현하고 해당 속성 setter가 **PropertyChanged** 이벤트를 발생시키는지 확인합니다. 아래와 같이 **OnPropertyChanged** 메서드와 같은 도우미 메서드를 호출하도록 하는 것이 일반적인 다른 방식입니다.

```csharp
public class Customer : INotifyPropertyChanged
{
    private string _name;
    public string Name
    {
        get => _name;
        set
        {
            if (_name != value)
                {
                    _name = value;
                    this.OnPropertyChanged();
                }
        }
    }
    public event PropertyChangedEventHandler PropertyChanged;

    public void OnPropertyChanged([CallerMemberName] string propertyName = null) =>
        this.PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
}
```
**TextBlock** 대신 **TextBox**를 사용하여 ListView의 텍스트를 편집할 수 있게 만들고, 데이터 바인딩의 **모드**를 **TwoWay**로 설정했는지 확인합니다.

```xaml
<ListView ItemsSource="{x:Bind Customers}"
    HorizontalAlignment="Center"
    VerticalAlignment="Center">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:Customer">
            <TextBox Text="{x:Bind Name, Mode=TwoWay}"/>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

이것이 작동하는지 확인하는 빠른 방법은 TextBox 컨트롤 및 OneWay 바인딩에 두 번째 ListView를 추가하는 것입니다. 두 번째 목록의 값은 첫 번째 값을 편집할 때 자동으로 변경됩니다.

> [!NOTE]
> ListView 내부에서 직접 편집하는 것은 작동 중인 양방향 바인딩을 표시하는 간단한 방법이지만 유용성 문제가 발생할 수 있습니다. 앱을 더 활용하기 전에 [다른 XAML 컨트롤](../design/controls-and-patterns/controls-and-events-intro.md)을 사용하여 데이터를 편집하고 ListView를 표시 전용으로 유지하는 것을 고려해 보세요.

## <a name="going-further"></a>더 나아가기

양방향 바인딩의 고객 목록을 만들었으므로 이제 자유롭게 제공한 링크를 통해 문서를 왔다 갔다 하고 체험해 보세요. 또한 .기본 및 고급 바인딩의 단계별 실습을 원하는 경우 우리의 [데이터 바인딩 자습서](../data-binding/xaml-basics-data-binding.md)를 확인하거나 보다 강력한 UI를 만들기 위해 [마스터/세부 정보 패턴](../design/controls-and-patterns/master-details.md) 같은 컨트롤을 조사할 수 있습니다.

## <a name="useful-apis-and-docs"></a>유용한 API 및 문서

여기에 데이터 바인딩을 시작하는 데 도움이 되는 API의 빠른 요약 및 다른 유용한 문서가 제공됩니다.

### <a name="useful-apis"></a>유용한 API

| API | 설명 |
|------|---------------|
| [데이터 템플릿](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate) | UI의 특정 요소를 표시하도록 데이터 개체의 시각적 구조에 대해 설명합니다. |
| [x:Bind](../xaml-platform/x-bind-markup-extension.md) | 권장된 x:Bind 태그 확장에 대한 설명서. |
| [바인딩](../xaml-platform/binding-markup-extension.md) | 이전 바인딩 태그 확장에 대한 설명서. |
| [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) | 세로 스택에 데이터 항목을 표시하는 UI 컨트롤입니다. |
| [TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) | UI에서 편집할 수 있는 텍스트 데이터를 표시하기 위한 기본 텍스트 컨트롤입니다. |
| [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged(d=robot).aspx) | 데이터를 관찰할 수 있도록 만들어 데이터 바인딩에 제공하는 인터페이스입니다. |
| [ItemsControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) | 이 클래스의 **ItemsSource** 속성으로 ListView를 데이터 원본에 바인딩할 수 있습니다. |

### <a name="useful-docs"></a>유용한 문서

| 항목 | 설명 |
|-------|----------------|
| [데이터 바인딩 심층 분석](../data-binding/data-binding-in-depth.md) | 데이터 바인딩 원칙의 기본 개요 |
| [데이터 바인딩 개요](../data-binding/data-binding-quickstart.md) | 데이터 바인딩에 대한 자세한 개념 정보. |
| [목록 보기](../design/controls-and-patterns/listview-and-gridview.md) | **DataTemplate**의 구현을 포함하여 ListView를 만들고 구성하는 정보 |

## <a name="useful-code-samples"></a>유용한 코드 샘플

| 코드 샘플 | 설명 |
|-----------------|---------------|
| [데이터 바인딩 자습서](../data-binding/xaml-basics-data-binding.md) | 데이터 바인딩의 기본 사항을 단계별로 안내하는 환경입니다. |
| [ListView 및 GridView](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView) | 데이터 바인딩으로 더욱 정교한 ListView를 탐색해 보세요. |
| [QuizGame](https://github.com/Microsoft/Windows-appsample-networkhelper) | **INotifyPropertyChanged**의 표준 구현에 대한 **BindableBase** 클래스("일반" 폴더에 있음)를 포함하여 작동 중인 데이터 바인딩을 확인합니다. |
