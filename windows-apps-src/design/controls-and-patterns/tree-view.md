---
description: ItemsSource를 계층적 데이터 원본에 바인딩하여 확장 가능한 트리 보기를 만들 수도 있고, TreeViewNode 개체를 직접 만들어서 관리할 수도 있습니다.
title: 트리 보기
label: Tree view
template: detail.hbs
ms.date: 06/14/2019
ms.topic: article
ms.localizationpriority: medium
pm-contact: predavid
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
dev_langs:
- csharp
- vb
ms.custom: RS5, 19H1
ms.openlocfilehash: 4c29e8f2f88469dfbf260268682cf18e0399e327
ms.sourcegitcommit: 81cb0b597bedfed8a54ac8b7e84089ef057fa9e3
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68514127"
---
# <a name="treeview"></a>TreeView

XAML [TreeView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview) 컨트롤은 중첩된 항목이 포함된 노드를 확장 및 축소하는 계층적 목록을 지원합니다. 이 컨트롤은 UI의 폴더 구조 또는 중첩된 관계를 나타내는 데 사용할 수 있습니다.

**TreeView** API는 다음과 같은 기능을 지원합니다.

- N 수준의 중첩
- 단일 또는 다중 노드 중에서 선택
- **TreeView** 및 [TreeViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeviewitem)의 **ItemsSource** 속성에 데이터 바인딩
- **TreeView** 항목 템플릿의 루트인 **TreeViewItem**
- **TreeViewItem**의 임의 형식 콘텐츠
- 트리 보기 간에 끌어서 놓기

| **Windows UI 라이브러리 가져오기** |
| - |
| 이 컨트롤은 UWP 앱용 새 컨트롤과 UI 기능을 포함하는 NuGet 패키지인 Windows UI 라이브러리의 일부로 포함되었습니다. 설치 지침을 비롯한 자세한 내용은 [Windows UI 라이브러리 개요](https://docs.microsoft.com/uwp/toolkits/winui/)를 참조하세요. |

| **플랫폼 API** | **Windows UI 라이브러리 API** |
| - | - |
| [TreeView 클래스](/uwp/api/windows.ui.xaml.controls.treeview), [TreeViewNode 클래스](/uwp/api/windows.ui.xaml.controls.treeviewnode), [TreeView.ItemsSource 속성](/uwp/api/windows.ui.xaml.controls.treeview.itemssource) | [TreeView 클래스](/uwp/api/microsoft.ui.xaml.controls.treeview), [TreeViewNode 클래스](/uwp/api/microsoft.ui.xaml.controls.treeviewnode), [TreeView.ItemsSource 속성](/uwp/api/microsoft.ui.xaml.controls.treeview.itemssource) |

이 문서 전체에서 XAML의 **muxc** 별칭을 사용하여 프로젝트에 포함된 Windows UI 라이브러리 API를 나타냅니다. 다음을 [Page](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page) 요소에 추가했습니다.

```xaml
xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
```

코드 숨김에서는 C#의 **muxc** 별칭을 사용하여 프로젝트에 포함된 Windows UI 라이브러리 API를 나타냅니다. 다음 **using** 문을 파일 맨 위에 추가했습니다.

```csharp
using muxc = Microsoft.UI.Xaml.Controls;
```

```vb
Imports muxc = Microsoft.UI.Xaml.Controls
```

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

- 항목에 중첩된 목록 항목이 있고 해당 피어 및 노드에 대한 항목의 계층적 관계를 설명해야 하는 경우에 **TreeView**를 사용합니다.

- 우선 순위가 아닌 항목의 중첩된 관계를 강조해야 하는 경우에는 **TreeView**를 사용하지 않습니다. 대부분의 반복 연습 시나리오에서는 일반적인 목록 보기가 적절합니다.

## <a name="examples"></a>예

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/TreeView">앱을 열고 실제로 작동하는 TreeView를 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="treeview-ui"></a>TreeView UI

트리 보기는 들여쓰기와 아이콘의 조합을 사용하여 부모 노드와 자식 노드 간의 중첩 관계를 나타냅니다. 축소된 노드는 오른쪽을 가리키는 펼침 단추를 사용하고, 확장된 노드는 아래쪽을 가리키는 펼침 단추를 사용합니다.

![TreeView의 펼침 단추 아이콘](images/treeview-simple.png)

트리 보기의 항목 데이터 템플릿에 아이콘을 포함시켜 노드를 나타낼 수 있습니다. 예를 들어 파일 시스템 계층을 표시하는 경우 부모 노드와 리프 노드의 파일 아이콘에 폴더 아이콘을 사용할 수 있습니다.

![TreeView에서 펼침 단추와 폴더 아이콘을 함께 사용](images/treeview-icons.png)

## <a name="create-a-tree-view"></a>트리 보기 만들기

[ItemsSource](/uwp/api/windows.ui.xaml.controls.treeview.itemssource)를 계층적 데이터 원본에 바인딩하여 트리 보기를 만들 수도 있고, **TreeViewNode** 개체를 직접 만들어서 관리할 수도 있습니다.

트리 보기를 만들려면 [TreeView](/uwp/api/windows.ui.xaml.controls.treeview) 컨트롤과 [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode) 개체의 계층을 사용합니다. 하나 이상의 루트 노드를 **TreeView** 컨트롤의 [RootNodes](/uwp/api/windows.ui.xaml.controls.treeview.rootnodes) 컬렉션에 추가하여 노드 계층을 만듭니다. 이제 각 **TreeViewNode**에서 [Children](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeviewnode.children) 컬렉션에 더 많은 노드를 추가할 수 있습니다. 트리 보기 노드를 필요한 깊이만큼 중첩할 수 있습니다.

[ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview)의 **ItemsSource**로 했던 것처럼, 계층적 데이터 원본을 [ItemsSource](/uwp/api/windows.ui.xaml.controls.treeview.itemssource) 속성에 바인딩하여 트리 보기 콘텐츠를 제공할 수 있습니다. 마찬가지로, [ItemTemplate](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate)(및 선택적 [ItemTemplateSelector](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate))을 사용하여 항목을 렌더링하는 [DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate)을 제공합니다.

> [!IMPORTANT]
> **ItemsSource** 및 관련 API에는 Windows 10 버전 1809([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 이상 또는 [Windows UI 라이브러리](https://docs.microsoft.com/uwp/toolkits/winui/)가 필요합니다.
>
> **ItemsSource**는 **TreeView.RootNodes**를 대신하여 **TreeView** 컨트롤에 콘텐츠를 삽입하는 대안 메커니즘입니다. **ItemsSource**와 **RootNodes**를 동시에 설정할 수 없습니다. **ItemsSource**를 사용하는 경우 노드가 자동으로 생성되므로 **TreeView.RootNodes** 속성에서 노드에 액세스할 수 있습니다.

다음은 XAML에 선언된 간단한 트리 보기의 예입니다. 사용자는 일반적으로 코드에 노드를 추가하지만, 여기서는 XAML 계층을 보여주고자 합니다. 왜냐하면 노드의 계층이 생성되는 방법을 시각화하는 데 도움이 될 수 있기 때문입니다.

```xaml
<muxc:TreeView>
    <muxc:TreeView.RootNodes>
        <muxc:TreeViewNode Content="Flavors"
                           IsExpanded="True">
            <muxc:TreeViewNode.Children>
                <muxc:TreeViewNode Content="Vanilla"/>
                <muxc:TreeViewNode Content="Strawberry"/>
                <muxc:TreeViewNode Content="Chocolate"/>
            </muxc:TreeViewNode.Children>
        </muxc:TreeViewNode>
    </muxc:TreeView.RootNodes>
</muxc:TreeView>
```

대부분의 경우 트리 보기는 데이터 원본의 데이터를 표시하므로, 개발자는 일반적으로 XAML에서 루트 **TreeView** 컨트롤을 선언하되, 코드로 또는 데이터 바인딩을 사용하여 **TreeViewNode** 개체를 추가합니다.

### <a name="bind-to-a-hierarchical-data-source"></a>계층적 데이터 원본에 바인딩

데이터 바인딩을 사용하여 트리 보기를 만들려면 계층적 컬렉션을 **TreeView.ItemsSource** 속성으로 설정합니다. 그런 다음, **ItemTemplate**에서 자식 항목 컬렉션을 **TreeViewItem.ItemsSource** 속성으로 설정합니다.

```xaml
<muxc:TreeView ItemsSource="{x:Bind DataSource}">
    <muxc:TreeView.ItemTemplate>
        <DataTemplate x:DataType="local:Item">
            <muxc:TreeViewItem ItemsSource="{x:Bind Children}"
                               Content="{x:Bind Name}"/>
        </DataTemplate>
    </muxc:TreeView.ItemTemplate>
</muxc:TreeView>
```

전체 코드는 [데이터 바인딩을 사용하는 트리 보기](#tree-view-using-data-binding)를 참조하세요.

#### <a name="items-and-item-containers"></a>항목 및 항목 컨테이너

**TreeView.ItemsSource**를 사용하는 경우 다음 API를 사용하여 컨테이너에서 노드 또는 데이터 항목을 가져오거나 그 반대로 가져올 수 있습니다.

| **[TreeViewItem](/uwp/api/windows.ui.xaml.controls.treeviewitem)** | |
| - | - |
| [TreeView.ItemFromContainer](/uwp/api/windows.ui.xaml.controls.treeview.itemfromcontainer) | 지정된 **TreeViewItem** 컨테이너에 대한 데이터 항목을 가져옵니다. |
| [TreeView.ContainerFromItem](/uwp/api/windows.ui.xaml.controls.treeview.containerfromitem) | 지정된 데이터 항목에 대한 **TreeViewItem** 컨테이너를 가져옵니다. |

| **[TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode)** | |
| - | - |
| [TreeView.NodeFromContainer](/uwp/api/windows.ui.xaml.controls.treeview.nodefromcontainer) | 지정된 **TreeViewItem** 컨테이너에 대한 **TreeViewNode**를 가져옵니다. |
| [TreeView.ContainerFromNode](/uwp/api/windows.ui.xaml.controls.treeview.containerfromnode) | 지정된 **TreeViewNode**에 대한 **TreeViewItem** 컨테이너를 가져옵니다. |

### <a name="manage-tree-view-nodes"></a>트리 보기 노드 관리

이 트리 보기는 앞서 XAML로 생성한 보기와 동일하지만, 코드에서 노드가 생성됩니다.

```xaml
<muxc:TreeView x:Name="sampleTreeView"/>
```

```csharp
private void InitializeTreeView()
{
    muxc.TreeViewNode rootNode = new muxc.TreeViewNode() { Content = "Flavors" };
    rootNode.IsExpanded = true;
    rootNode.Children.Add(new muxc.TreeViewNode() { Content = "Vanilla" });
    rootNode.Children.Add(new muxc.TreeViewNode() { Content = "Strawberry" });
    rootNode.Children.Add(new muxc.TreeViewNode() { Content = "Chocolate" });

    sampleTreeView.RootNodes.Add(rootNode);
}
```

```vb
Private Sub InitializeTreeView()
    Dim rootNode As New muxc.TreeViewNode With {.Content = "Flavors", .IsExpanded = True}
    With rootNode.Children
        .Add(New muxc.TreeViewNode With {.Content = "Vanilla"})
        .Add(New muxc.TreeViewNode With {.Content = "Strawberry"})
        .Add(New muxc.TreeViewNode With {.Content = "Chocolate"})
    End With
    sampleTreeView.RootNodes.Add(rootNode)
End Sub
```

다음 API는 트리 보기의 데이터 계층을 관리하는 데 사용할 수 있습니다.

| **[TreeView](/uwp/api/windows.ui.xaml.controls.treeview)** | |
| - | - |
| [RootNodes](/uwp/api/windows.ui.xaml.controls.treeview.rootnodes) | 트리 보기에는 하나 이상의 루트 노드가 포함될 수 있습니다. **TreeViewNode** 개체를 **RootNodes** 컬렉션에 추가하여 루트 노드를 생성합니다. 루트 노드의 **부모**는 항상 **null**입니다. 루트 노드의 **깊이**는 0입니다. |

| **[TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode)** | |
| - | - |
| [Children](/uwp/api/windows.ui.xaml.controls.treeviewnode.children) | **TreeViewNode** 개체를 부모 노드의 **Children** 컬렉션에 추가하여 노드 계층을 만듭니다. 한 노드는 **자식** 컬렉션에 있는 모든 노드의 **부모**가 됩니다. |
| [HasChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.haschildren) | 노드가 자식으로 구현된 경우 이 값은 **true**입니다. **false**는 빈 폴더 또는 항목을 나타냅니다. |
| [HasUnrealizedChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.hasunrealizedchildren) | 확장에 따라 노드를 채우고 있는 경우에는 이 속성을 사용합니다. 이 문서 뒷부분의 [확장 시 노드 채우기](#fill-a-node-when-its-expanding)를 참조하세요. |
| [Depth](/uwp/api/windows.ui.xaml.controls.treeviewnode.depth) | 자식 노드가 루트 노드에서 얼마나 멀리 있는지 나타냅니다. |
| [Parent](/uwp/api/windows.ui.xaml.controls.treeviewnode.parent) | 이 노드가 포함된 **Children** 컬렉션을 소유하고 있는 **TreeViewNode**를 가져옵니다. |

트리 보기는 **HasChildren** 및 **HasUnrealizedChildren** 속성을 사용하여 확장/축소 아이콘의 표시 여부를 결정합니다. 속성 중 하나가 **true**이면 아이콘이 표시되고, 그렇지 않으면 표시되지 않습니다.

## <a name="tree-view-node-content"></a>트리 보기 노드 콘텐츠

트리 보기 노드가 [콘텐츠](/uwp/api/windows.ui.xaml.controls.treeviewnode.content) 속성에 표시하는 데이터 항목을 저장할 수 있습니다.

위 예제에서는 콘텐츠가 간단한 문자열 값이었습니다. 여기 나온 트리 보기 노드는 사용자의 **Pictures** 폴더를 표시하고 있기 때문에 사진 라이브러리 [StorageFolder](/uwp/api/windows.storage.storagefolder)가 노드의 **Content** 속성에 할당됩니다.

```csharp
StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
muxc.TreeViewNode pictureNode = new muxc.TreeViewNode();
pictureNode.Content = picturesFolder;
```

```vb
Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
Dim pictureNode As New muxc.TreeViewNode With {.Content = picturesFolder}
```

> [!NOTE]
> **Pictures** 폴더에 액세스하려면 앱 매니페스트에서 **사진 라이브러리** 기능을 지정해야 합니다. 자세한 내용은 [앱 기능 선언](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations)을 참조하세요.

트리 보기에 데이터 항목이 표시되는 방법을 지정하기 위해 [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate)을 제공할 수 있습니다.

> [!NOTE]
> Windows 10 버전 1803에서는 콘텐츠가 문자열이 아닌 경우 **TreeView** 컨트롤의 템플릿을 다시 작성하고 사용자 지정 **ItemTemplate**을 지정해야 합니다. 이후 버전에서는 **ItemTemplate** 속성을 설정합니다. 자세한 내용은 [TreeView.ItemTemplate](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate)을 참조하세요.

### <a name="item-container-style"></a>항목 컨테이너 스타일

**ItemsSource** 또는 **RootNodes** 중에 무엇을 사용하든, "컨테이너"라고 부르는 각 노드를 표시하는 데 사용되는 실제 요소는 [TreeViewItem](/uwp/api/windows.ui.xaml.controls.treeviewitem) 개체입니다. **TreeView**의 [ItemContainerStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview.itemcontainerstyle) 또는 [ItemContainerStyleSelector](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview.itemcontainerstyleselector) 속성을 사용하여 컨테이너의 스타일을 지정하도록 **TreeViewItem** 속성을 수정할 수 있습니다.

다음 예제에서는 확장/축소된 문자 모양을 주황색 +/- 기호로 변경하는 방법을 보여 줍니다. 기본 **TreeViewItem** 템플릿에서 문자 모양은 `Segoe MDL2 Assets` 글꼴을 사용하도록 설정됩니다. **Setter.Value** 속성은 유니코드 문자 값을 XAML에서 사용하는 형식(예: `Value="&#xE948;"`)으로 제공하여 설정할 수 있습니다.

```xaml
<muxc:TreeView>
    <muxc:TreeView.ItemContainerStyle>
        <Style TargetType="muxc:TreeViewItem">
            <Setter Property="CollapsedGlyph" Value="&#xE948;"/>
            <Setter Property="ExpandedGlyph" Value="&#xE949;"/>
            <Setter Property="GlyphBrush" Value="DarkOrange"/>
        </Style>
    </muxc:TreeView.ItemContainerStyle>
    <muxc:TreeView.RootNodes>
        <muxc:TreeViewNode Content="Flavors"
               IsExpanded="True">
            <muxc:TreeViewNode.Children>
                <muxc:TreeViewNode Content="Vanilla"/>
                <muxc:TreeViewNode Content="Strawberry"/>
                <muxc:TreeViewNode Content="Chocolate"/>
            </muxc:TreeViewNode.Children>
        </muxc:TreeViewNode>
    </muxc:TreeView.RootNodes>
</muxc:TreeView>
```

### <a name="item-template-selectors"></a>항목 템플릿 선택기

기본적으로 [TreeView](/uwp/api/windows.ui.xaml.controls.treeview)는 각 노드에 대한 데이터 항목의 문자열 표현을 표시합니다. 모든 노드에 대해 표시되는 내용을 변경하도록 [ItemTemplate](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate) 속성을 설정할 수 있습니다. 또는 [ItemTemplateSelector](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplateselector)를 사용하여 항목 형식 또는 지정한 다른 조건에 따라 트리 보기 항목에 대해 다른 [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate)을 선택할 수 있습니다.

예를 들어 파일 탐색기 앱에서 폴더에 한 데이터 템플릿을 사용하고, 파일에는 다른 폴더를 사용할 수 있습니다.

![서로 다른 데이터 템플릿을 사용하는 파일과 폴더](images/treeview-icons.png)

다음은 항목 템플릿 선택기를 만들어서 사용하는 방법의 예입니다.  자세한 내용은 [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector) 클래스를 참조하세요.

> [!NOTE]
> 이 코드는 더 큰 예제의 일부이며, 자체적으로 작동하지 않습니다. `ExplorerItem`을 정의하는 코드를 포함하여 전체 예제를 보려면 GitHub에서 [Xaml-Controls-Gallery 리포지토리](https://github.com/microsoft/Xaml-Controls-Gallery)를 확인하세요. [TreeViewPage.xaml](https://github.com/microsoft/Xaml-Controls-Gallery/blob/1ecd85c908a8a1cb9a8201e548f58db379801e69/XamlControlsGallery/ControlPages/TreeViewPage.xaml) 및 [TreeViewPage.xaml.cs](https://github.com/Microsoft/Xaml-Controls-Gallery/blob/1ecd85c908a8a1cb9a8201e548f58db379801e69/XamlControlsGallery/ControlPages/TreeViewPage.xaml.cs)에 관련 코드가 포함되어 있습니다.

```xaml
<Page.Resources>
    <DataTemplate x:Key="FolderTemplate" x:DataType="local:ExplorerItem">
        <muxc:TreeViewItem ItemsSource="{x:Bind Children}">
            <StackPanel Orientation="Horizontal">
                <Image Width="20" Source="Assets/folder.png"/>
                <TextBlock Text="{x:Bind Name}" />
            </StackPanel>
        </muxc:TreeViewItem>
    </DataTemplate>

    <DataTemplate x:Key="FileTemplate" x:DataType="local:ExplorerItem">
        <muxc:TreeViewItem>
            <StackPanel Orientation="Horizontal">
                <Image Width="20" Source="Assets/file.png"/>
                <TextBlock Text="{Binding Name}"/>
            </StackPanel>
        </muxc:TreeViewItem>
    </DataTemplate>

    <local:ExplorerItemTemplateSelector
            x:Key="ExplorerItemTemplateSelector"
            FolderTemplate="{StaticResource FolderTemplate}"
            FileTemplate="{StaticResource FileTemplate}" />
</Page.Resources>

<Grid>
    <muxc:TreeView
        ItemsSource="{x:Bind DataSource}"
        ItemTemplateSelector="{StaticResource ExplorerItemTemplateSelector}"/>
</Grid>
```

```csharp
public class ExplorerItemTemplateSelector : DataTemplateSelector
{
    public DataTemplate FolderTemplate { get; set; }
    public DataTemplate FileTemplate { get; set; }

    protected override DataTemplate SelectTemplateCore(object item)
    {
        var explorerItem = (ExplorerItem)item;
        if (explorerItem.Type == ExplorerItem.ExplorerItemType.Folder) return FolderTemplate;

        return FileTemplate;
    }
}
```

[SelectTemplateCore](/uwp/api/windows.ui.xaml.controls.datatemplateselector.selecttemplatecore) 메서드에 전달된 개체 형식은 **ItemsSource** 속성을 설정하거나 **TreeViewNode** 개체를 직접 만들고 관리하여 트리 보기를 만드는지 여부에 따라 달라집니다.

- **ItemsSource**가 설정되는 경우 개체는 데이터 항목의 모든 형식 중 하나가 됩니다. 이전 예제에서 개체는 `ExplorerItem`이었으므로 `ExplorerItem`에 대한 간단한 캐스트(`var explorerItem = (ExplorerItem)item;`) 뒤에 사용할 수 있습니다.
- **ItemsSource**가 설정되지 않고 트리 보기 노드를 직접 관리하는 경우 **SelectTemplateCore**에 전달되는 개체는 **TreeViewNode**입니다. 이 경우 데이터 항목은 [TreeViewNode.Content](/uwp/api/windows.ui.xaml.controls.treeviewnode.content) 속성에서 가져올 수 있습니다.

다음은 뒤에 나오는 [사진 및 음악 라이브러리 트리 보기](#pictures-and-music-library-tree-view) 예제의 데이터 템플릿 선택기입니다. **SelectTemplateCore** 메서드는 [StorageFolder](/uwp/api/windows.storage.storagefolder) 또는 [StorageFile](/uwp/api/windows.storage.storagefile)이 해당 콘텐츠로 포함될 수 있는 **TreeViewNode**를 받습니다. 콘텐츠에 따라 음악 폴더, 사진 폴더, 음악 파일 또는 사진 파일에 대한 기본 템플릿 또는 특정 템플릿을 반환할 수 있습니다.

```csharp
protected override DataTemplate SelectTemplateCore(object item)
{
    var node = (TreeViewNode)item;
    if (node.Content is StorageFolder)
    {
        var content = node.Content as StorageFolder;
        if (content.DisplayName.StartsWith("Pictures")) return PictureFolderTemplate;
        if (content.DisplayName.StartsWith("Music")) return MusicFolderTemplate;
    }
    else if (node.Content is StorageFile)
    {
        var content = node.Content as StorageFile;
        if (content.ContentType.StartsWith("image")) return PictureItemTemplate;
        if (content.ContentType.StartsWith("audio")) return MusicItemTemplate;
    }
    return DefaultTemplate;
}
```

```vb
Protected Overrides Function SelectTemplateCore(ByVal item As Object) As DataTemplate
    Dim node = CType(item, muxc.TreeViewNode)

    If TypeOf node.Content Is StorageFolder Then
        Dim content = TryCast(node.Content, StorageFolder)
        If content.DisplayName.StartsWith("Pictures") Then Return PictureFolderTemplate
        If content.DisplayName.StartsWith("Music") Then Return MusicFolderTemplate
    ElseIf TypeOf node.Content Is StorageFile Then
        Dim content = TryCast(node.Content, StorageFile)
        If content.ContentType.StartsWith("image") Then Return PictureItemTemplate
        If content.ContentType.StartsWith("audio") Then Return MusicItemTemplate
    End If

    Return DefaultTemplate
End Function
```

## <a name="interacting-with-a-tree-view"></a>트리 보기와의 상호 작용

여러 가지 방법으로 사용자가 상호 작용하도록 트리 보기를 구성할 수 있습니다.

- 노드 확장 또는 축소
- 항목 단일 또는 다중 선택
- 항목 호출을 위한 클릭

### <a name="expandcollapse"></a>확장/축소

자식 노드가 있는 트리 보기는 확장/축소 문자 모양을 클릭하여 언제든지 확장 또는 축소가 가능합니다. 또한 프로그래밍 방식으로 노드를 확장 또는 축소하고 노드 상태가 변경될 때 응답할 수 있습니다.

#### <a name="expandcollapse-a-node-programmatically"></a>프로그래밍 방식으로 노드 확장/축소

코드로 트리 보기 노드를 확장 또는 축소할 수 있는 2가지 방법이 있습니다.

- [TreeView](/uwp/api/windows.ui.xaml.controls.treeview) 클래스에는 [Collapse](/uwp/api/windows.ui.xaml.controls.treeview.collapse) 및 [Expand](/uwp/api/windows.ui.xaml.controls.treeview.expand) 메서드가 포함되어 있습니다. 이러한 메서드를 호출할 때 확장 또는 축소하려는 **TreeViewNode**를 전달합니다.

- 각 [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode)에는 [IsExpanded](/uwp/api/windows.ui.xaml.controls.treeviewnode.isexpanded) 속성이 포함되어 있습니다. 이 속성을 사용하여 노드의 상태를 확인하거나 상태를 변경하도록 속성을 설정할 수 있습니다. 또한 XAML에서 이 속성을 설정하여 노드의 초기 상태를 설정할 수도 있습니다.

### <a name="fill-a-node-when-its-expanding"></a>확장 시 노드 채우기

트리 보기에 다수의 노드를 표시해야 하는 경우도 있고, 얼마나 많은 노드가 필요한지 사전에 알 수 없는 경우도 있습니다. **TreeView** 컨트롤은 가상화되지 않으므로 확장 시에는 각 노드를 채우고 축소 시에는 자식 노드를 제거하는 방식으로 리소스를 관리할 수 있습니다.

[Expanding](/uwp/api/windows.ui.xaml.controls.treeview.expand) 이벤트를 처리하고 [HasUnrealizedChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.hasunrealizedchildren) 속성을 사용하여 확장 시 노드에 자식 노드를 추가합니다. **HasUnrealizedChildren** 속성은 노드를 채워야 하는지 여부나 **Children** 컬렉션이 이미 채워져 있는지 여부를 나타냅니다. **TreeViewNode**는 이 값을 설정하지 않는다는 것을 기억하고 앱 코드에서 이를 관리하는 것이 중요합니다.

다음은 이러한 API를 사용하는 예입니다. **FillTreeNode** 구현을 포함하여 컨텍스트에 대해 이 문서의 마지막 부분에 나와 있는 전체 예제 코드를 참조하세요.

```csharp
private void SampleTreeView_Expanding(muxc.TreeView sender, muxc.TreeViewExpandingEventArgs args)
{
    if (args.Node.HasUnrealizedChildren)
    {
        FillTreeNode(args.Node);
    }
}
```

```vb
Private Sub SampleTreeView_Expanding(sender As muxc.TreeView, args As muxc.TreeViewExpandingEventArgs)
    If args.Node.HasUnrealizedChildren Then
        FillTreeNode(args.Node)
    End If
End Sub
```

필수 요건은 아니지만, [Collapsed](/uwp/api/windows.ui.xaml.controls.treeview.collapsed) 이벤트를 처리하고 부모 노드가 닫힐 때 자식 노드를 제거하기를 원할 수도 있습니다. 트리 보기에 다수의 노드가 있거나 노드 데이터가 다양한 리소스를 사용하는 경우에는 중요할 수 있습니다. 닫힌 노드에 자식 노드를 남겨두는 것과 비교해 열릴 때마다 노드를 채우는 것이 성능에 미치는 영향을 고려해야 합니다. 최상의 옵션은 앱에 따라 달라집니다.

다음은 **Collapsed** 이벤트 처리기의 예입니다.

```csharp
private void SampleTreeView_Collapsed(muxc.TreeView sender, muxc.TreeViewCollapsedEventArgs args)
{
    args.Node.Children.Clear();
    args.Node.HasUnrealizedChildren = true;
}
```

```vb
Private Sub SampleTreeView_Collapsed(sender As muxc.TreeView, args As muxc.TreeViewCollapsedEventArgs)
    args.Node.Children.Clear()
    args.Node.HasUnrealizedChildren = True
End Sub
```

### <a name="invoking-an-item"></a>항목 호출

사용자가 해당 항목을 선택하는 대신 작업을 호출할 수 있습니다(단추처럼 항목 취급). [ItemInvoked](/uwp/api/windows.ui.xaml.controls.treeview.iteminvoked) 이벤트를 처리하여 이러한 사용자 상호 작용에 응답합니다.

> [!NOTE]
> [IsItemClickEnabled](/uwp/api/windows.ui.xaml.controls.listviewbase.isitemclickenabled) 속성을 가지고 있는 **ListView**와 달리, 항목 호출은 트리 보기에서 항상 활성화되어 있습니다. 여전히 이벤트 처리 여부를 선택할 수 있습니다.

**[TreeViewItemInvokedEventArgs](/uwp/api/windows.ui.xaml.controls.treeviewiteminvokedeventargs) 클래스**

**ItemInvoked** 이벤트 인수는 호출된 항목에 대한 액세스를 제공합니다. [InvokedItem](/uwp/api/windows.ui.xaml.controls.treeviewiteminvokedeventargs.invokeditem) 속성은 호출된 노드를 가지고 있습니다. 이 속성을 **TreeViewNode**로 캐스팅하고 **TreeViewNode.Content** 속성에서 데이터 항목을 가져올 수 있습니다.

다음은 **ItemInvoked** 이벤트 처리기의 예입니다. 데이터 항목은 [IStorageItem](/uwp/api/windows.storage.istorageitem)이고, 이 예제에는 파일 및 트리에 대한 몇 가지 정보만 나와 있습니다. 또한 노드가 폴더 노드이면 노드를 동시에 확장 또는 축소할 수 있습니다. 그렇지 않으면 펼침 단추를 클릭할 때만 노드가 확장 또는 축소됩니다.

```csharp
private void SampleTreeView_ItemInvoked(muxc.TreeView sender, muxc.TreeViewItemInvokedEventArgs args)
{
    var node = args.InvokedItem as muxc.TreeViewNode;
    if (node.Content is IStorageItem item)
    {
        FileNameTextBlock.Text = item.Name;
        FilePathTextBlock.Text = item.Path;
        TreeDepthTextBlock.Text = node.Depth.ToString();

        if (node.Content is StorageFolder)
        {
            node.IsExpanded = !node.IsExpanded;
        }
    }
}
```

```vb
Private Sub SampleTreeView_ItemInvoked(sender As muxc.TreeView, args As muxc.TreeViewItemInvokedEventArgs)
    Dim node = TryCast(args.InvokedItem, muxc.TreeViewNode)
    Dim item = TryCast(node.Content, IStorageItem)
    If item IsNot Nothing Then
        FileNameTextBlock.Text = item.Name
        FilePathTextBlock.Text = item.Path
        TreeDepthTextBlock.Text = node.Depth.ToString()
        If TypeOf node.Content Is StorageFolder Then
            node.IsExpanded = Not node.IsExpanded
        End If
    End If
End Sub
```

### <a name="item-selection"></a>항목 선택

**TreeView** 컨트롤은 단일 선택 및 다중 선택을 모두 지원합니다. 기본적으로 노드 선택 기능은 꺼져 있지만, 노드 선택이 가능하도록 [TreeView.SelectionMode](/uwp/api/windows.ui.xaml.controls.treeview.selectionmode) 속성을 설정할 수 있습니다. [TreeViewSelectionMode](/uwp/api/windows.ui.xaml.controls.treeviewselectionmode) 값은 **없음**, **단일** 및 **다중**입니다.

#### <a name="multiple-selection"></a>다중 선택

다중 선택이 활성화되면 각 트리 보기 노드 옆에 확인란이 표시되고 선택된 항목이 강조 표시됩니다. 사용자는 확인란을 사용하여 항목을 선택 또는 선택 취소할 수 있습니다. 해당 항목을 클릭해도 여전히 항목이 호출됩니다.

부모 노드를 선택하거나 선택 취소하면 해당 노드의 모든 자식 노드가 선택 또는 선택 취소됩니다. 모두 그런 것은 아니지만, 부모 노드 아래에 있는 일부 자식 노드의 경우 부모 노드의 확인란이 (검은색 상자로 채워진) 미확정으로 표시됩니다.

![트리 보기에서 다중 선택](images/treeview-selection.png)

선택된 노드는 트리 보기의 [SelectedNodes](/uwp/api/windows.ui.xaml.controls.treeview.selectednodes) 컬렉션에 추가됩니다. [SelectAll](/uwp/api/windows.ui.xaml.controls.treeview.selectall) 메서드를 호출하여 트리 보기의 모든 노드를 선택할 수 있습니다.

> [!NOTE]
> **SelectAll**을 호출하면 **SelectionMode**에 관계없이 실현된 모든 노드가 선택됩니다. 일관된 사용자 환경을 제공하려면 **SelectionMode**가 **Multiple**인 경우 **SelectAll**만 호출해야 합니다.

#### <a name="selection-and-realizedunrealized-nodes"></a>선택 및 실현/미실현 노드

트리 보기에 미실현 노드가 있으면 선택 시 이들을 고려하지 않습니다. 다음은 미실현 노드가 있을 때 선택 시 주의해야 하는 사항입니다.

- 사용자가 부모 노드를 선택하면 해당 부모 노드에서 실현된 모든 자식 노드가 선택됩니다. 마찬가지로, 모든 자녀 노드를 선택하면 부모 노드도 선택됩니다.
- **SelectAll** 메서드는 **SelectedNodes** 컬렉션에 실현된 노드만 추가합니다.
- 미실현 자식 노드가 있는 부모 노드를 선택하면 실현되었을 때 자식 노드가 선택됩니다.

## <a name="code-examples"></a>코드 예제

다음 코드 예제에서는 트리 보기 컨트롤의 다양한 기능을 보여 줍니다.

### <a name="tree-view-using-xaml"></a>XAML을 사용하는 트리 보기

이 예제에서는 XAML에서 간단한 트리 보기 구조를 만드는 방법을 보여줍니다. 이 트리 보기는 사용자가 선택할 수 있는 범주화된 아이스크림 맛과 토핑을 보여줍니다. 다중 선택이 활성화되어 있는 상태에서 사용자가 단추를 클릭하면 선택된 항목이 기본 앱 UI에 표시됩니다.

```xaml
<Page
    x:Class="TreeViewTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
          Padding="100">
        <SplitView IsPaneOpen="True"
               DisplayMode="Inline"
               OpenPaneLength="296">
            <SplitView.Pane>
                <muxc:TreeView x:Name="DessertTree" SelectionMode="Multiple">
                    <muxc:TreeView.RootNodes>
                        <muxc:TreeViewNode Content="Flavors" IsExpanded="True">
                            <muxc:TreeViewNode.Children>
                                <muxc:TreeViewNode Content="Vanilla"/>
                                <muxc:TreeViewNode Content="Strawberry"/>
                                <muxc:TreeViewNode Content="Chocolate"/>
                            </muxc:TreeViewNode.Children>
                        </muxc:TreeViewNode>

                        <muxc:TreeViewNode Content="Toppings">
                            <muxc:TreeViewNode.Children>
                                <muxc:TreeViewNode Content="Candy">
                                    <muxc:TreeViewNode.Children>
                                        <muxc:TreeViewNode Content="Chocolate"/>
                                        <muxc:TreeViewNode Content="Mint"/>
                                        <muxc:TreeViewNode Content="Sprinkles"/>
                                    </muxc:TreeViewNode.Children>
                                </muxc:TreeViewNode>
                                <muxc:TreeViewNode Content="Fruits">
                                    <muxc:TreeViewNode.Children>
                                        <muxc:TreeViewNode Content="Mango"/>
                                        <muxc:TreeViewNode Content="Peach"/>
                                        <muxc:TreeViewNode Content="Kiwi"/>
                                    </muxc:TreeViewNode.Children>
                                </muxc:TreeViewNode>
                                <muxc:TreeViewNode Content="Berries">
                                    <muxc:TreeViewNode.Children>
                                        <muxc:TreeViewNode Content="Strawberry"/>
                                        <muxc:TreeViewNode Content="Blueberry"/>
                                        <muxc:TreeViewNode Content="Blackberry"/>
                                    </muxc:TreeViewNode.Children>
                                </muxc:TreeViewNode>
                            </muxc:TreeViewNode.Children>
                        </muxc:TreeViewNode>
                    </muxc:TreeView.RootNodes>
                </muxc:TreeView>
            </SplitView.Pane>

            <StackPanel Grid.Column="1" Margin="12,0">
                <Button Content="Select all" Click="SelectAllButton_Click"/>
                <Button Content="Create order" Click="OrderButton_Click" Margin="0,12"/>
                <TextBlock Text="Your flavor selections:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FlavorList" Margin="0,0,0,12"/>
                <TextBlock Text="Your topping selections:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="ToppingList"/>
            </StackPanel>
        </SplitView>
    </Grid>
</Page>
```

```csharp
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using muxc = Microsoft.UI.Xaml.Controls;

namespace TreeViewTest
{
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
        }

        private void OrderButton_Click(object sender, RoutedEventArgs e)
        {
            FlavorList.Text = string.Empty;
            ToppingList.Text = string.Empty;

            foreach (muxc.TreeViewNode node in DessertTree.SelectedNodes)
            {
                if (node.Parent.Content?.ToString() == "Flavors")
                {
                    FlavorList.Text += node.Content + "; ";
                }
                else if (node.HasChildren == false)
                {
                    ToppingList.Text += node.Content + "; ";
                }
            }
        }

        private void SelectAllButton_Click(object sender, RoutedEventArgs e)
        {
            if (DessertTree.SelectionMode == muxc.TreeViewSelectionMode.Multiple)
            {
                DessertTree.SelectAll();
            }
        }
    }
}
```

```vb
Private Sub OrderButton_Click(sender As Object, e As RoutedEventArgs)
    FlavorList.Text = String.Empty
    ToppingList.Text = String.Empty
    For Each node As muxc.TreeViewNode In DessertTree.SelectedNodes
        If node.Parent.Content?.ToString() = "Flavors" Then
            FlavorList.Text += node.Content & "; "
        ElseIf node.HasChildren = False Then
            ToppingList.Text += node.Content & "; "
        End If
    Next
End Sub

Private Sub SelectAllButton_Click(sender As Object, e As RoutedEventArgs)
    If DessertTree.SelectionMode = muxc.TreeViewSelectionMode.Multiple Then
        DessertTree.SelectAll()
    End If
End Sub
```

### <a name="tree-view-using-data-binding"></a>데이터 바인딩을 사용하는 트리 보기

이 예제에서는 이전 예제와 동일한 트리 보기를 만드는 방법을 보여줍니다. 그러나 XAML에서 데이터 계층 구조를 만드는 대신, 코드로 데이터를 만들어서 트리 보기의 **ItemsSource** 속성에 바인딩합니다. (이전 예제의 단추 이벤트 처리기는 이 예제에도 적용됩니다.)

```xaml
<Page
    x:Class="TreeViewTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
    xmlns:local="using:TreeViewTest"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
          Padding="100">
        <SplitView IsPaneOpen="True"
                   DisplayMode="Inline"
                   OpenPaneLength="296">
            <SplitView.Pane>
                <muxc:TreeView Name="DessertTree"
                                      SelectionMode="Multiple"
                                      ItemsSource="{x:Bind DataSource}">
                    <muxc:TreeView.ItemTemplate>
                        <DataTemplate x:DataType="local:Item">
                            <muxc:TreeViewItem
                                ItemsSource="{x:Bind Children}"
                                Content="{x:Bind Name}"/>
                        </DataTemplate>
                    </muxc:TreeView.ItemTemplate>
                </muxc:TreeView>
            </SplitView.Pane>

            <StackPanel Grid.Column="1" Margin="12,0">
                <Button Content="Select all"
                        Click="SelectAllButton_Click"/>
                <Button Content="Create order"
                        Click="OrderButton_Click"
                        Margin="0,12"/>
                <TextBlock Text="Your flavor selections:"
                           Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FlavorList" Margin="0,0,0,12"/>
                <TextBlock Text="Your topping selections:"
                           Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="ToppingList"/>
            </StackPanel>
        </SplitView>
    </Grid>

</Page>
```

```csharp
using System.Collections.ObjectModel;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using muxc = Microsoft.UI.Xaml.Controls;

namespace TreeViewTest
{
    public sealed partial class MainPage : Page
    {
        private ObservableCollection<Item> DataSource = new ObservableCollection<Item>();

        public MainPage()
        {
            this.InitializeComponent();
            DataSource = GetDessertData();
        }

        private ObservableCollection<Item> GetDessertData()
        {
            var list = new ObservableCollection<Item>();

            Item flavorsCategory = new Item()
            {
                Name = "Flavors",
                Children =
                {
                    new Item() { Name = "Vanilla" },
                    new Item() { Name = "Strawberry" },
                    new Item() { Name = "Chocolate" }
                }
            };

            Item toppingsCategory = new Item()
            {
                Name = "Toppings",
                Children =
                {
                    new Item()
                    {
                        Name = "Candy",
                        Children =
                        {
                            new Item() { Name = "Chocolate" },
                            new Item() { Name = "Mint" },
                            new Item() { Name = "Sprinkles" }
                        }
                    },
                    new Item()
                    {
                        Name = "Fruits",
                        Children =
                        {
                            new Item() { Name = "Mango" },
                            new Item() { Name = "Peach" },
                            new Item() { Name = "Kiwi" }
                        }
                    },
                    new Item()
                    {
                        Name = "Berries",
                        Children =
                        {
                            new Item() { Name = "Strawberry" },
                            new Item() { Name = "Blueberry" },
                            new Item() { Name = "Blackberry" }
                        }
                    }
                }
            };

            list.Add(flavorsCategory);
            list.Add(toppingsCategory);
            return list;
        }

        private void OrderButton_Click(object sender, RoutedEventArgs e)
        {
            FlavorList.Text = string.Empty;
            ToppingList.Text = string.Empty;

            foreach (muxc.TreeViewNode node in DessertTree.SelectedNodes)
            {
                if (node.Parent.Content?.ToString() == "Flavors")
                {
                    FlavorList.Text += node.Content + "; ";
                }
                else if (node.HasChildren == false)
                {
                    ToppingList.Text += node.Content + "; ";
                }
            }
        }

        private void SelectAllButton_Click(object sender, RoutedEventArgs e)
        {
            if (DessertTree.SelectionMode == muxc.TreeViewSelectionMode.Multiple)
            {
                DessertTree.SelectAll();
            }
        }
    }

    public class Item
    {
        public string Name { get; set; }
        public ObservableCollection<Item> Children { get; set; } = new ObservableCollection<Item>();

        public override string ToString()
        {
            return Name;
        }
    }
}

```

### <a name="pictures-and-music-library-tree-view"></a>사진 및 음악 라이브러리 트리 보기

이 예제에서는 사용자 **사진** 및 **음악** 라이브러리의 콘텐츠 및 구조를 표시하는 트리 보기를 만드는 방법을 보여줍니다. 항목 수를 미리 알 수 없으므로 확장 시에는 각 노드를 채우고 축소 시에는 노드를 비웁니다.

사용자 지정 항목 템플릿은 형식이 [IStorageItem](/uwp/api/windows.storage.istorageitem)인 데이터 항목을 표시하는 데 사용됩니다.

> [!IMPORTANT]
> 이 예제의 코드에서는 **picturesLibrary** 및 **musicLibrary** 기능이 필요합니다. 파일 액세스에 대한 자세한 내용은 [파일 액세스 권한](../../files/file-access-permissions.md), [열거 및 쿼리 파일/폴더](../../files/quickstart-listing-files-and-folders.md) 및 [음악, 사진 및 동영상 라이브러리의 파일 및 폴더](../../files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)를 참조하세요.

```xaml
<Page
    x:Class="TreeViewTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:TreeViewTest"
    xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">
    <Page.Resources>
        <DataTemplate x:Key="TreeViewItemDataTemplate">
            <Grid Height="44">
                <TextBlock Text="{Binding Content.DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </Grid>
        </DataTemplate>

        <DataTemplate x:Key="MusicItemDataTemplate">
            <StackPanel Height="44" Orientation="Horizontal">
                <SymbolIcon Symbol="Audio" Margin="0,0,4,0"/>
                <TextBlock Text="{Binding Content.DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </StackPanel>
        </DataTemplate>

        <DataTemplate x:Key="PictureItemDataTemplate">
            <StackPanel Height="44" Orientation="Horizontal">
                <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xEB9F;"
                          Margin="0,0,4,0"/>
                <TextBlock Text="{Binding Content.DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </StackPanel>
        </DataTemplate>

        <DataTemplate x:Key="MusicFolderDataTemplate">
            <StackPanel Height="44" Orientation="Horizontal">
                <SymbolIcon Symbol="MusicInfo" Margin="0,0,4,0"/>
                <TextBlock Text="{Binding Content.DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </StackPanel>
        </DataTemplate>

        <DataTemplate x:Key="PictureFolderDataTemplate">
            <StackPanel Height="44" Orientation="Horizontal">
                <SymbolIcon Symbol="Pictures" Margin="0,0,4,0"/>
                <TextBlock Text="{Binding Content.DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </StackPanel>
        </DataTemplate>

        <local:ExplorerItemTemplateSelector
            x:Key="ExplorerItemTemplateSelector"
            DefaultTemplate="{StaticResource TreeViewItemDataTemplate}"
            MusicItemTemplate="{StaticResource MusicItemDataTemplate}"
            MusicFolderTemplate="{StaticResource MusicFolderDataTemplate}"
            PictureItemTemplate="{StaticResource PictureItemDataTemplate}"
            PictureFolderTemplate="{StaticResource PictureFolderDataTemplate}"/>
    </Page.Resources>

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <SplitView IsPaneOpen="True"
                   DisplayMode="Inline"
                   OpenPaneLength="296">
            <SplitView.Pane>
                <Grid>
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition/>
                    </Grid.RowDefinitions>
                    <Button Content="Refresh tree" Click="RefreshButton_Click" Margin="24,12"/>
                    <muxc:TreeView x:Name="sampleTreeView" Grid.Row="1" SelectionMode="Single"
                              ItemTemplateSelector="{StaticResource ExplorerItemTemplateSelector}"
                              Expanding="SampleTreeView_Expanding"
                              Collapsed="SampleTreeView_Collapsed"
                              ItemInvoked="SampleTreeView_ItemInvoked"/>
                </Grid>
            </SplitView.Pane>

            <StackPanel Grid.Column="1" Margin="12,72">
                <TextBlock Text="File name:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FileNameTextBlock" Margin="0,0,0,12"/>

                <TextBlock Text="File path:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FilePathTextBlock" Margin="0,0,0,12"/>

                <TextBlock Text="Tree depth:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="TreeDepthTextBlock" Margin="0,0,0,12"/>
            </StackPanel>
        </SplitView>
    </Grid>
</Page>
```

```csharp
using System;
using System.Collections.Generic;
using Windows.Storage;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using muxc = Microsoft.UI.Xaml.Controls;

namespace TreeViewTest
{
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            InitializeTreeView();
        }

        private void InitializeTreeView()
        {
            // A TreeView can have more than 1 root node. The Pictures library
            // and the Music library will each be a root node in the tree.
            // Get Pictures library.
            StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
            muxc.TreeViewNode pictureNode = new muxc.TreeViewNode();
            pictureNode.Content = picturesFolder;
            pictureNode.IsExpanded = true;
            pictureNode.HasUnrealizedChildren = true;
            sampleTreeView.RootNodes.Add(pictureNode);
            FillTreeNode(pictureNode);

            // Get Music library.
            StorageFolder musicFolder = KnownFolders.MusicLibrary;
            muxc.TreeViewNode musicNode = new muxc.TreeViewNode();
            musicNode.Content = musicFolder;
            musicNode.IsExpanded = true;
            musicNode.HasUnrealizedChildren = true;
            sampleTreeView.RootNodes.Add(musicNode);
            FillTreeNode(musicNode);
        }

        private async void FillTreeNode(muxc.TreeViewNode node)
        {
            // Get the contents of the folder represented by the current tree node.
            // Add each item as a new child node of the node that's being expanded.

            // Only process the node if it's a folder and has unrealized children.
            StorageFolder folder = null;

            if (node.Content is StorageFolder && node.HasUnrealizedChildren == true)
            {
                folder = node.Content as StorageFolder;
            }
            else
            {
                // The node isn't a folder, or it's already been filled.
                return;
            }

            IReadOnlyList<IStorageItem> itemsList = await folder.GetItemsAsync();

            if (itemsList.Count == 0)
            {
                // The item is a folder, but it's empty. Leave HasUnrealizedChildren = true so
                // that the chevron appears, but don't try to process children that aren't there.
                return;
            }

            foreach (var item in itemsList)
            {
                var newNode = new muxc.TreeViewNode();
                newNode.Content = item;

                if (item is StorageFolder)
                {
                    // If the item is a folder, set HasUnrealizedChildren to true.
                    // This makes the collapsed chevron show up.
                    newNode.HasUnrealizedChildren = true;
                }
                else
                {
                    // Item is StorageFile. No processing needed for this scenario.
                }

                node.Children.Add(newNode);
            }

            // Children were just added to this node, so set HasUnrealizedChildren to false.
            node.HasUnrealizedChildren = false;
        }

        private void SampleTreeView_Expanding(muxc.TreeView sender, muxc.TreeViewExpandingEventArgs args)
        {
            if (args.Node.HasUnrealizedChildren)
            {
                FillTreeNode(args.Node);
            }
        }

        private void SampleTreeView_Collapsed(muxc.TreeView sender, muxc.TreeViewCollapsedEventArgs args)
        {
            args.Node.Children.Clear();
            args.Node.HasUnrealizedChildren = true;
        }

        private void SampleTreeView_ItemInvoked(muxc.TreeView sender, muxc.TreeViewItemInvokedEventArgs args)
        {
            var node = args.InvokedItem as muxc.TreeViewNode;

            if (node.Content is IStorageItem item)
            {
                FileNameTextBlock.Text = item.Name;
                FilePathTextBlock.Text = item.Path;
                TreeDepthTextBlock.Text = node.Depth.ToString();

                if (node.Content is StorageFolder)
                {
                    node.IsExpanded = !node.IsExpanded;
                }
            }
        }

        private void RefreshButton_Click(object sender, RoutedEventArgs e)
        {
            sampleTreeView.RootNodes.Clear();
            InitializeTreeView();
        }
    }

    public class ExplorerItemTemplateSelector : DataTemplateSelector
    {
        public DataTemplate DefaultTemplate { get; set; }
        public DataTemplate MusicItemTemplate { get; set; }
        public DataTemplate PictureItemTemplate { get; set; }
        public DataTemplate MusicFolderTemplate { get; set; }
        public DataTemplate PictureFolderTemplate { get; set; }

        protected override DataTemplate SelectTemplateCore(object item)
        {
            var node = (muxc.TreeViewNode)item;

            if (node.Content is StorageFolder)
            {
                var content = node.Content as StorageFolder;
                if (content.DisplayName.StartsWith("Pictures")) return PictureFolderTemplate;
                if (content.DisplayName.StartsWith("Music")) return MusicFolderTemplate;
            }
            else if (node.Content is StorageFile)
            {
                var content = node.Content as StorageFile;
                if (content.ContentType.StartsWith("image")) return PictureItemTemplate;
                if (content.ContentType.StartsWith("audio")) return MusicItemTemplate;

            }
            return DefaultTemplate;
        }
    }
}
```

```vb
Public NotInheritable Class MainPage
    Inherits Page

    Public Sub New()
        InitializeComponent()
        InitializeTreeView()
    End Sub

    Private Sub InitializeTreeView()
        ' A TreeView can have more than 1 root node. The Pictures library
        ' and the Music library will each be a root node in the tree.
        ' Get Pictures library.
        Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
        Dim pictureNode As New muxc.TreeViewNode With {
        .Content = picturesFolder,
        .IsExpanded = True,
        .HasUnrealizedChildren = True
    }
        sampleTreeView.RootNodes.Add(pictureNode)
        FillTreeNode(pictureNode)

        ' Get Music library.
        Dim musicFolder As StorageFolder = KnownFolders.MusicLibrary
        Dim musicNode As New muxc.TreeViewNode With {
        .Content = musicFolder,
        .IsExpanded = True,
        .HasUnrealizedChildren = True
    }
        sampleTreeView.RootNodes.Add(musicNode)
        FillTreeNode(musicNode)
    End Sub

    Private Async Sub FillTreeNode(node As muxc.TreeViewNode)
        ' Get the contents of the folder represented by the current tree node.
        ' Add each item as a new child node of the node that's being expanded.

        ' Only process the node if it's a folder and has unrealized children.
        Dim folder As StorageFolder = Nothing
        If TypeOf node.Content Is StorageFolder AndAlso node.HasUnrealizedChildren Then
            folder = TryCast(node.Content, StorageFolder)
        Else
            ' The node isn't a folder, or it's already been filled.
            Return
        End If

        Dim itemsList As IReadOnlyList(Of IStorageItem) = Await folder.GetItemsAsync()
        If itemsList.Count = 0 Then
            ' The item is a folder, but it's empty. Leave HasUnrealizedChildren = true so
            ' that the chevron appears, but don't try to process children that aren't there.
            Return
        End If

        For Each item In itemsList
            Dim newNode As New muxc.TreeViewNode With {
            .Content = item
        }
            If TypeOf item Is StorageFolder Then
                ' If the item is a folder, set HasUnrealizedChildren to True.
                ' This makes the collapsed chevron show up.
                newNode.HasUnrealizedChildren = True
            Else
                ' Item is StorageFile. No processing needed for this scenario.
            End If
            node.Children.Add(newNode)
        Next

        ' Children were just added to this node, so set HasUnrealizedChildren to False.
        node.HasUnrealizedChildren = False
    End Sub

    Private Sub SampleTreeView_Expanding(sender As muxc.TreeView, args As muxc.TreeViewExpandingEventArgs)
        If args.Node.HasUnrealizedChildren Then
            FillTreeNode(args.Node)
        End If
    End Sub

    Private Sub SampleTreeView_Collapsed(sender As muxc.TreeView, args As muxc.TreeViewCollapsedEventArgs)
        args.Node.Children.Clear()
        args.Node.HasUnrealizedChildren = True
    End Sub

    Private Sub SampleTreeView_ItemInvoked(sender As muxc.TreeView, args As muxc.TreeViewItemInvokedEventArgs)
        Dim node = TryCast(args.InvokedItem, muxc.TreeViewNode)
        Dim item = TryCast(node.Content, IStorageItem)
        If item IsNot Nothing Then
            FileNameTextBlock.Text = item.Name
            FilePathTextBlock.Text = item.Path
            TreeDepthTextBlock.Text = node.Depth.ToString()
            If TypeOf node.Content Is StorageFolder Then
                node.IsExpanded = Not node.IsExpanded
            End If
        End If
    End Sub

    Private Sub RefreshButton_Click(sender As Object, e As RoutedEventArgs)
        sampleTreeView.RootNodes.Clear()
        InitializeTreeView()
    End Sub

End Class

Public Class ExplorerItemTemplateSelector
    Inherits DataTemplateSelector

    Public Property DefaultTemplate As DataTemplate
    Public Property MusicItemTemplate As DataTemplate
    Public Property PictureItemTemplate As DataTemplate
    Public Property MusicFolderTemplate As DataTemplate
    Public Property PictureFolderTemplate As DataTemplate

    Protected Overrides Function SelectTemplateCore(ByVal item As Object) As DataTemplate
        Dim node = CType(item, muxc.TreeViewNode)

        If TypeOf node.Content Is StorageFolder Then
            Dim content = TryCast(node.Content, StorageFolder)
            If content.DisplayName.StartsWith("Pictures") Then Return PictureFolderTemplate
            If content.DisplayName.StartsWith("Music") Then Return MusicFolderTemplate
        ElseIf TypeOf node.Content Is StorageFile Then
            Dim content = TryCast(node.Content, StorageFile)
            If content.ContentType.StartsWith("image") Then Return PictureItemTemplate
            If content.ContentType.StartsWith("audio") Then Return MusicItemTemplate
        End If

        Return DefaultTemplate
    End Function
End Class
```

### <a name="drag-and-drop-items-between-tree-views"></a>트리 보기 간에 항목 끌어서 놓기

다음 예제에서는 서로 간에 항목을 끌어서 놓을 수 있는 두 트리 보기를 만드는 방법을 설명합니다. 항목을 다른 트리 보기로 끌어서 놓으면 목록의 끝에 추가됩니다. 그러나 트리 보기 내에서 항목의 순서를 다시 지정할 수 있습니다. 이 예제에서는 하나의 루트 노드가 있는 트리 보기만 고려합니다.

```xaml
<Page
    x:Class="TreeViewTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>

        <TreeView x:Name="treeView1"
                  AllowDrop="True"
                  CanDragItems="True"
                  CanReorderItems="True"
                  DragOver="TreeView_DragOver"
                  Drop="TreeView_Drop"
                  DragItemsStarting="TreeView_DragItemsStarting"
                  DragItemsCompleted="TreeView_DragItemsCompleted"/>
        <TreeView x:Name="treeView2"
                  AllowDrop="True"
                  Grid.Column="1"
                  CanDragItems="True"
                  CanReorderItems="True"
                  DragOver="TreeView_DragOver"
                  Drop="TreeView_Drop"
                  DragItemsStarting="TreeView_DragItemsStarting"
                  DragItemsCompleted="TreeView_DragItemsCompleted"/>

    </Grid>

</Page>
```

```csharp
using System;
using Windows.ApplicationModel.DataTransfer;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;

namespace TreeViewTest
{
    public sealed partial class MainPage : Page
    {
        private TreeViewNode deletedItem;
        private TreeView sourceTreeView;

        public MainPage()
        {
            this.InitializeComponent();
            InitializeTreeView();
        }

        private void InitializeTreeView()
        {
            TreeViewNode parentNode1 = new TreeViewNode() { Content = "tv1" };
            TreeViewNode parentNode2 = new TreeViewNode() { Content = "tv2" };

            parentNode1.Children.Add(new TreeViewNode() { Content = "tv1FirstChild" });
            parentNode1.Children.Add(new TreeViewNode() { Content = "tv1SecondChild" });
            parentNode1.Children.Add(new TreeViewNode() { Content = "tv1ThirdChild" });
            parentNode1.Children.Add(new TreeViewNode() { Content = "tv1FourthChild" });
            parentNode1.IsExpanded = true;
            treeView1.RootNodes.Add(parentNode1);

            parentNode2.Children.Add(new TreeViewNode() { Content = "tv2FirstChild" });
            parentNode2.Children.Add(new TreeViewNode() { Content = "tv2SecondChild" });
            parentNode2.IsExpanded = true;
            treeView2.RootNodes.Add(parentNode2);
        }

        private void TreeView_DragOver(object sender, DragEventArgs e)
        {
            if (e.DataView.Contains(StandardDataFormats.Text))
            {
                e.AcceptedOperation = DataPackageOperation.Move;
            }
        }

        private async void TreeView_Drop(object sender, DragEventArgs e)
        {
            if (e.DataView.Contains(StandardDataFormats.Text))
            {
                string text = await e.DataView.GetTextAsync();
                TreeView destinationTreeView = sender as TreeView;

                if (destinationTreeView.RootNodes != null)
                {
                    TreeViewNode newNode = new TreeViewNode() { Content = text };
                    destinationTreeView.RootNodes[0].Children.Add(newNode);
                    deletedItem = newNode;
                }
            }
        }

        private void TreeView_DragItemsStarting(TreeView sender, TreeViewDragItemsStartingEventArgs args)
        {
            if (args.Items.Count == 1)
            {
                args.Data.RequestedOperation = DataPackageOperation.Move;
                sourceTreeView = sender;

                foreach (var item in args.Items)
                {
                    args.Data.SetText(item.ToString());
                }
            }
        }

        private void TreeView_DragItemsCompleted(TreeView sender, TreeViewDragItemsCompletedEventArgs args)
        {
            var children = sourceTreeView.RootNodes[0].Children;

            if (deletedItem != null)
            {
                for (int i = 0; i < children.Count; i++)
                {
                    if (children[i].Content.ToString() == deletedItem.Content.ToString())
                    {
                        children.RemoveAt(i);
                        break;
                    }
                }
            }

            sourceTreeView = null;
            deletedItem = null;
        }
    }
}
```

## <a name="related-articles"></a>관련 문서

- [TreeView 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview)
- [ListView 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview)
- [ListView 및 GridView](listview-and-gridview.md)
