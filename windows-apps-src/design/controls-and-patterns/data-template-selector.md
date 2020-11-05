---
description: 데이터 템플릿 선택을 사용하여 항목 속성을 기준으로 항목의 스타일을 사용자 지정합니다.
title: 데이터 템플릿 선택
label: Data template selection
template: detail.hbs
ms.date: 10/18/2019
ms.topic: article
keywords: windows 10, uwp
pm-contact: anawish
ms.openlocfilehash: 5b10afc03a1936c033977a53bd12effdae1c2ead
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032336"
---
# <a name="data-template-selection-styling-items-based-on-their-properties"></a>데이터 템플릿 선택: 속성을 기준으로 항목 스타일 지정

컬렉션 제어의 사용자 지정된 디자인은 [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate)에서 관리됩니다. 데이터 템플릿은 각 항목의 레이아웃 및 스타일 지정 방식을 정의하며, 해당 태그는 컬렉션의 모든 항목에 적용됩니다. 이 문서에서는 [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector)를 사용하여 다양한 데이터 템플릿을 컬렉션에 적용하고, 특정 항목 속성 또는 사용자가 선택한 값을 기준으로 사용할 데이터 템플릿을 선택하는 방법을 설명합니다.

> **중요 API** : [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector), [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate)

[DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector)는 사용자 지정 템플릿 선택 논리를 가능하게 하는 클래스입니다. 이를 통해 컬렉션의 특정 항목에 사용할 데이터 템플릿을 지정하는 규칙을 정의할 수 있습니다. 이 논리를 구현하려면 코드 숨김에서 DataTemplateSelector의 하위 클래스를 생성하고 어떤 항목의 카테고리에 어떤 데이터 템플릿을 사용할지 결정하는 논리를 정의합니다(예를 들어 특정 유형의 항목 또는 특정 속성 값을 갖는 항목 등). 사용할 데이터 템플릿의 정의와 함께 XAML 파일의 리소스 섹션에서 이 클래스의 인스턴스를 선언합니다. 이러한 리소스를 XAML에서 참조할 수 있는 `x:Key` 값으로 식별합니다.

## <a name="prerequisites"></a>필수 구성 요소

- [ListView 또는 GridView와 같은 컬렉션 제어를 사용 및 만드는](listview-and-gridview.md) 방법.
- [DataTemplate을 사용하여 항목의 모양을 사용자 지정하는](item-containers-templates.md#data-template) 방법.

## <a name="when-not-to-use-a-datatemplateselector"></a>DataTemplateSelector를 사용하지 않는 경우

일반적으로 ListView 또는 GridView의 모든 항목에 완전히 다른 레이아웃/스타일을 적용해서는 안 됩니다. 이는 DataTemplateSelector를 잘못 사용하는 경우이며 성능에 부정적인 영향을 미칩니다.

목록 항목의 시각적 디스플레이의 특정 요소는 특정 속성을 바인딩함으로써 하나의 데이터 템플릿만 사용하여 제어할 수 있습니다. 예를 들어, 항목은 각각 데이터 템플릿의 아이콘 소스 속성에 바인딩하고 각 항목에 해당 아이콘 소스 속성에 대한 다른 값을 부여함으로써 서로 다른 아이콘을 가질 수 있습니다. 이렇게 하면 DataTemplateSelector를 사용하는 것보다 더 나은 성능을 얻을 수 있습니다.

## <a name="when-to-use-a-datatemplateselector"></a>DataTemplateSelector를 사용하는 경우

하나의 컬렉션 제어에서 여러 데이터 템플릿을 사용하려는 경우 [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector)를 생성해야 합니다. `DataTemplateSelector`는 특정 항목을 돋보이게 하면서도 동시에 유사한 레이아웃을 유지할 수 있는 유연성을 제공합니다. DataTemplateSelector가 유용한 많은 사용 사례와 함께, 사용 중인 제어와 전략을 다시 고려하는 것이 더 나은 몇 가지 시나리오가 있습니다.

컬렉션 제어는 일반적으로 모두 하나의 유형인 항목 컬렉션으로 바인딩됩니다. 그러나 항목들이 동일한 유형이어도, 특정 속성에 대한 값이 다르거나 다른 의미를 나타낼 수도 있습니다. 특정 항목이 다른 항목보다 더 중요할 수도 있고, 한 항목이 특히 중요하거나 다를 수 있으며, 시각적으로 두드러질 필요가 있을 수도 있습니다. DataTemplateSelector는 이러한 경우에 매우 유용합니다.

또한 다른 유형의 항목을 포함하는 컬렉션에 바인딩할 수 있습니다. 바인딩된 컬렉션에는 문자열, 정수, 사용자 지정 클래스 개체 등이 혼합되어 있을 수 있습니다. 이는 DataTemplateSelector가 항목의 개체 유형에 따라 다른 데이터 템플릿을 할당할 수 있기 때문에 특히 유용합니다.

다음은 데이터 템플릿 선택을 사용할 수 있는 경우의 예입니다.

- ListView 내에서 다양한 수준의 직원을 표시하기 - 직원의 각 유형/수준은 쉽게 구별할 수 있도록 다른 색상 배경이 필요할 수 있습니다.
- GridView를 사용하여 제품 갤러리에서 할인 항목을 표시하기 - 할인 항목이 정가 항목과 구별되도록 하기 위해 빨간색 배경 또는 다른 색 글꼴이 필요할 수 있습니다.
- FlipView를 사용하여 사진 갤러리에서 우승자/상위 사진 표시하기.
- ListView에서 음수/양수를 다르게 표시하거나 짧은 문자열/긴 문자열을 표시해야 하는 경우.

## <a name="create-a-datatemplateselector"></a>DataTemplateSelector 만들기

데이터 템플릿 선택을 만드는 경우 코드에서 템플릿 선택 논리를 정의하고 XAML에서 데이터 템플릿을 정의합니다.

### <a name="code-behind-component"></a>코드 숨김 구성 요소

데이터 템플릿 선택을 사용하려면 먼저 코드 숨김에서 [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector)의 하위 클래스를 만듭니다(DataTemplateSelector에서 파생된 클래스). 클래스에서 각 템플릿을 클래스의 속성으로 선언합니다. 그런 다음, 사용자 고유의 템플릿 선택 논리를 포함하도록 [SelectTemplateCore](/uwp/api/windows.ui.xaml.controls.datatemplateselector.selecttemplatecore) 메서드를 재정의합니다.

다음은 `MyDataTemplateSelector`라고 하는 간단한 `DataTemplateSelector` 하위 클래스의 예제입니다.

```csharp
public class MyDataTemplateSelector : DataTemplateSelector
{
    public DataTemplate Normal { get; set; }
    public DataTemplate Accent { get; set; }

    protected override DataTemplate SelectTemplateCore(object item)
    {
        if ((int)item % 2 == 0)
        {
            return Normal;
        }
        else
        {
            return Accent;
        }
    }
}
```

`MyDataTemplateSelector` 클래스는 `DataTemplateSelector` 클래스에서 파생되었으며, 먼저 두 개의 [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) 개체인 `Normal` 및 `Accent`를 정의합니다. 현재로서는 빈 선언이지만 XAML 파일의 태그와 함께 “채우기”가 될 것입니다.

`SelectTemplateCore` 메서드는 항목 개체(즉, 각 컬렉션 항목)를 포함하며, 어떤 상황에서 반환할 `DataTemplate`에 대한 규칙으로 재정의됩니다. 이 경우에서 항목이 홀수면 `Accent` 데이터 템플릿을 수신하며, 짝수면 `Normal` 데이터 템플릿을 수신합니다.

### <a name="xaml-component"></a>XAML 구성 요소

두 번째로, XAML 파일의 리소스 섹션에서 이 새로운 `MyDataTemplateSelector` 클래스의 인스턴스를 만들어야 합니다. 모든 리소스에는 컬렉션 제어의 `ItemTemplateSelector` 속성에 바인딩하는 데 사용하는 `x:Key`가 필요합니다(이후 단계에서). 또한 `DataTemplate` 개체의 두 가지 인스턴스를 만들고 리소스 섹션에서 해당 레이아웃을 정의합니다. 이러한 데이터 템플릿을 `MyDataTemplateSelector` 클래스에서 선언한 `Accent` 및 `Normal` 속성에 할당합니다.

다음은 필요한 XAML 리소스 및 태그의 예제입니다.

```xaml
<Page.Resources>

<DataTemplate x:Key="NormalItemTemplate" x:DataType="x:Int32">
    <Button HorizontalAlignment="Stretch" VerticalAlignment="Stretch" Background="{ThemeResource SystemChromeLowColor}">
        <TextBlock Text="{x:Bind}" />
    </Button>
</DataTemplate>

<DataTemplate x:Key="AccentItemTemplate" x:DataType="x:Int32">
    <Button HorizontalAlignment="Stretch" VerticalAlignment="Stretch" Background="{ThemeResource SystemAccentColor}">
        <TextBlock Text="{x:Bind}" />
    </Button>
</DataTemplate>

<l:MyDataTemplateSelector x:Key="MyDataTemplateSelector"
    Normal="{StaticResource NormalItemTemplate}"
    Accent="{StaticResource AccentItemTemplate}"/>

</Page.Resources>
```

위에서 볼 수 있듯이 두 개의 데이터 템플릿 `Normal` 및 `Accent`가 정의되어 있습니다. 두 템플릿 모두 항목을 단추로 표시합니다. 단, `Accent` 데이터 템플릿은 배경에는 테마 컬러 브러시를 사용하는 반면, `Normal` 데이터 템플릿은 회색 컬러 브러시(`SystemChromeLowColor`)를 사용합니다. 그런 다음, 이러한 두 데이터 템플릿은 C# 코드 숨김에서 만들었던 MyDataTemplateSelector 클래스의 특성인 `Normal` 및 `Accent` DataTemplate 개체에 할당됩니다.

마지막 단계는 사용자의 `DataTemplateSelector`를 컬렉션 제어의 `ItemTemplateSelector` 속성에 바인딩하는 것입니다(이 경우 ListView). 이것은 `ItemTemplate` 속성에 값을 할당해야 하는 필요성을 대체합니다. 

```xaml
<ListView x:Name = "TestListView"
          ItemsSource = "{x:Bind NumbersList}"
          ItemTemplateSelector = "{StaticResource MyDataTemplateSelector}">
</ListView>
```

코드가 컴파일되고 나면, 각 컬렉션 항목은 `MyDataTemplateSelector`의 재정의된 `SelectTemplateCore` 메서드를 통해 실행되며 적절한 DataTemplate으로 렌더링됩니다.

> [!IMPORTANT]
> [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater?view=winui-2.2)와 함께 `DataTemplateSelector`를 사용하는 경우 `DataTemplateSelector`를 `ItemTemplate` 속성에 바인딩합니다. `ItemsRepeater`에는 `ItemTemplateSelector` 속성이 없습니다.

## <a name="datatemplateselector-performance-considerations"></a>DataTemplateSelector 성능 고려 사항

대규모 데이터 컬렉션과 함께 ListView 또는 GridView를 사용하는 경우 스크롤 및 패닝 성능이 문제가 될 수 있습니다. 대규모 컬렉션의 성능을 잘 유지하기 위해 데이터 템플릿의 성능을 향상시키는 몇 가지 단계를 수행할 수 있습니다. 이러한 내용은 [ListView 및 GridView UI 최적화](../../debug-test-perf/optimize-gridview-and-listview.md)에 자세히 설명되어 있습니다.

- _항목당 요소 감소_ - 데이터 템플릿의 UI 요소 수를 적절한 최솟값으로 유지합니다.
- 다른 유형의 컬렉션에서 컨테이너 재생
  - _ChoosingItemContainer 이벤트_ 사용 - 이 이벤트는 다른 항목에 대해 다른 데이터 템플릿을 사용하는 성능이 우수한 방법입니다. 최상의 성능을 얻으려면 특정 데이터에 대한 데이터 템플릿 선택 및 캐싱을 최적화해야 합니다.
  - _항목 템플릿 선택_ 사용 - 항목 템플릿 선택(`DataTemplateSelector`)은 성능에 영향을 미치기 때문에 피해야 합니다.
