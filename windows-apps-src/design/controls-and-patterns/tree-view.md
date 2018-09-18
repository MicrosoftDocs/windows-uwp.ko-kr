---
author: Jwmsft
description: ItemsSource 계층적 데이터 원본에 바인딩하여 확장 가능한 트리 보기를 생성 하거나 만들고 TreeViewNode 개체를 직접 관리할 수 있습니다.
title: 트리 뷰
label: Tree view
template: detail.hbs
ms.author: jimwalk
ms.localizationpriority: medium
pm-contact: predavid
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
dev_langs:
- csharp
- vb
ms.openlocfilehash: 20de58d13c4ace6b71ec952dc88cd59d1ab6114f
ms.sourcegitcommit: f5321b525034e2b3af202709e9b942ad5557e193
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/18/2018
ms.locfileid: "4023009"
---
# <a name="treeview"></a>TreeView

> [!IMPORTANT]
> 이 문서에서는 아직 출시되지 않아 상업적으로 출시하기 전에 크게 수정될 수 있는 기능에 대해 설명합니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다. 미리 보기 기능에는 [최신 Windows 10 Insider Preview 빌드 및 SDK](https://insider.windows.com/for-developers/) 또는 [Windows UI 라이브러리](https://docs.microsoft.com/uwp/toolkits/winui/)필요합니다.

XAML TreeView 컨트롤은 중첩된 항목이 포함된 노드를 확장 및 축소하는 계층적 목록을 지원합니다. 이 컨트롤은 UI에 폴더 구조나 중첩된 관계를 나타내는 데 사용할 수 있습니다.

TreeView API는 다음과 같은 기능을 지원합니다.

- N 수준의 중첩
- 단일 또는 다중 노드 중에서 선택
- (미리 보기) TreeView 및 TreeViewItem에 ItemsSource 속성을 데이터 바인딩
- (미리 보기) TreeViewItem 루트 TreeView 항목 템플릿
- (미리 보기) 임의의 유형의 콘텐츠를 TreeViewItem에
- (미리 보기) 끌어서 놓기 트리 보기 간에

| **Windows UI 라이브러리 가져오기** |
| - |
| 이 컨트롤은 Windows UI 라이브러리 새 컨트롤 및 UWP 앱에 대 한 UI 기능을 포함 하는 NuGet 패키지의 일부로 포함 합니다. 설치 지침을 비롯 한 자세한 내용은 [Windows UI 라이브러리 개요](https://docs.microsoft.com/uwp/toolkits/winui/)를 참조 하세요. |

| **플랫폼 Api** | **Windows UI 라이브러리 Api** |
| - | - |
| [TreeView 클래스](/uwp/api/windows.ui.xaml.controls.treeview), [TreeViewNode 클래스](/uwp/api/windows.ui.xaml.controls.treeviewnode) [TreeView.ItemsSource 속성](/uwp/api/windows.ui.xaml.controls.treeview.itemssource) | [TreeView 클래스](/uwp/api/microsoft.ui.xaml.controls.treeview), [TreeViewNode 클래스](/uwp/api/microsoft.ui.xaml.controls.treeviewnode) [TreeView.ItemsSource 속성](/uwp/api/microsoft.ui.xaml.controls.treeview.itemssource) |

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

- 항목에 중첩된 목록 항목이 있고 해당 피어 및 노드에 대한 항목의 계층적 관계를 설명해야 하는 경우에 TreeView를 사용합니다.

- 우선 순위가 아닌 항목의 중첩된 관계를 강조해야 하는 경우에는 TreeView를 사용하지 않습니다. 대부분의 반복 연습 시나리오에서는 일반 목록 보기가 적절합니다.

## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치 된 경우 여기를 클릭 <a href="xamlcontrolsgallery:/item/TreeView">앱을 열고 중인 TreeView를 참조 하세요</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML 컨트롤 갤러리 앱 다운로드(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">소스 코드 다운로드(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="treeview-ui"></a>TreeView UI

트리 보기는 들여쓰기와 아이콘의 조합을 사용하여 폴더/부모 노드와 비폴더/자식 노드 사이 간의 중첩된 관계를 나타냅니다. 축소된 노드는 오른쪽을 가리키는 펼침 단추를 사용하고, 확장된 노드는 아래쪽을 가리키는 펼침 단추를 사용합니다.

![TreeView의 펼침 단추 아이콘](images/treeview-simple.png)

트리 보기의 항목 데이터 템플릿에 아이콘을 포함시켜 노드를 나타낼 수 있습니다. 이 경우에는 디스크의 폴더 구조와 같이 리터럴 폴더를 표시하는 노드에서만 폴더 아이콘을 사용해야 합니다.

![TreeView에서 펼침 단추와 폴더 아이콘을 함께 사용](images/treeview-icons.png)

## <a name="create-a-tree-view"></a>트리 보기 만들기

[ItemsSource](/uwp/api/windows.ui.xaml.controls.treeview.itemssource) 계층적 데이터 원본에 바인딩하여 트리 보기를 생성 하거나 만들고 TreeViewNode 개체를 직접 관리할 수 있습니다.

트리 보기를 만들려면 [TreeView](/uwp/api/windows.ui.xaml.controls.treeview) 컨트롤과 [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode) 개체의 계층을 사용합니다. TreeView 컨트롤의 [RootNodes](/uwp/api/windows.ui.xaml.controls.treeview.rootnodes) 컬렉션에 하나 이상의 루트 노드를 추가 하 여 노드 계층을 만듭니다. 이제 각 TreeViewNode에서 자식 노드 컬렉션에 더 많은 노드를 추가할 수 있습니다. 트리 보기 노드를 필요한 깊이 만큼 중첩시킬 수 있습니다.

Windows Insider Preview부터 바인딩할 수 있습니다 계층적 데이터 소스 트리 보기 콘텐츠를 제공 하기 위해 [ItemsSource](/uwp/api/windows.ui.xaml.controls.treeview.itemssource) 속성 ListView의 ItemsSource와 마찬가지로 합니다. 마찬가지로, 항목을 렌더링 하는 DataTemplate을 제공 하기 위해 [ItemTemplate](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate) (및 선택적 [ItemTemplateSelector](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate))를 사용 합니다.

> [!IMPORTANT]
> ItemsSource에는 TreeView 컨트롤에 콘텐츠를 배치 하는 것에 대 한 TreeView.RootNodes 하는 대체 메커니즘입니다. ItemsSource 및 RootNodes 모두 동시에 설정할 수 없습니다. ItemsSource를 사용 하 여 노드를 만들고 TreeView.RootNodes 속성에서 액세스할 수 있습니다.

다음은 XAML에 선언된 간단한 트리 보기의 예입니다. 사용자는 일반적으로 코드에 노드를 추가하지만, 여기서는 XAML 계층을 보여주고자 합니다. 왜냐하면 노드의 계층이 생성되는 방법을 시각화하는 데 도움이 될 수 있기 때문입니다.

```xaml
<TreeView>
    <TreeView.RootNodes>
        <TreeViewNode Content="Flavors" IsExpanded="True">
            <TreeViewNode.Children>
                <TreeViewNode Content="Vanilla"/>
                <TreeViewNode Content="Strawberry"/>
                <TreeViewNode Content="Chocolate"/>
            </TreeViewNode.Children>
        </TreeViewNode>
    </TreeView.RootNodes>
</TreeView>
```

대부분의 경우 일반적으로 루트 TreeView 컨트롤을 XAML에서 선언 코드나 데이터 바인딩을 사용 하 여에 TreeViewNode 개체를 추가 하지만 트리 보기 데이터 원본에서 데이터를 표시 합니다.

### <a name="bind-to-a-hierarchical-data-source"></a>계층적 데이터 원본에 바인딩합니다

데이터 바인딩을 사용 하 여 트리 보기를 만들려면 계층적 컬렉션 TreeView.ItemsSource 속성을 설정 합니다. 다음은 ItemTemplate에서 자식 항목 컬렉션 속성을 설정 합니다 TreeViewItem.ItemsSource.

```xaml
<TreeView ItemsSource="{x:Bind DataSource}">
    <TreeView.ItemTemplate>
        <DataTemplate x:DataType="local:Item">
            <TreeViewItem ItemsSource="{x:Bind Children}"
                          Content="{x:Bind Name}"/>
        </DataTemplate>
    </TreeView.ItemTemplate>
</TreeView>
```

_데이터 바인딩을 사용 하는 트리 보기_ 에 대 한 전체 코드 예제 섹션을 참조 하세요.

#### <a name="items-and-item-containers"></a>항목 및 항목 컨테이너

이러한 Api는 컨테이너에서 노드 또는 데이터 항목을 가져올 수 있는 TreeView.ItemsSource를 사용 하는 경우 또는 그 반대로 합니다.

| **[TreeViewItem](/uwp/api/windows.ui.xaml.controls.treeviewitem)** | |
| - | - |
| [TreeView.ItemFromContainer](/uwp/api/windows.ui.xaml.controls.treeview.itemfromcontainer) | 지정된 된 TreeViewItem 컨테이너에 대 한 데이터 항목을 가져옵니다. |
| [TreeView.ContainerFromItem](/uwp/api/windows.ui.xaml.controls.treeview.containerfromitem) | 지정된 된 데이터 항목에 대 한 TreeViewItem 컨테이너를 가져옵니다. |

| **[TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode)** | |
| - | - |
| [TreeView.NodeFromContainer](/uwp/api/windows.ui.xaml.controls.treeview.nodefromcontainer) | 지정된 된 TreeViewItem 컨테이너에 대 한 TreeViewNode를 가져옵니다. |
| [TreeView.ContainerFromNode](/uwp/api/windows.ui.xaml.controls.treeview.containerfromnode) | 지정 된 TreeViewNode TreeViewItem 컨테이너를 가져옵니다. |

### <a name="manage-tree-view-nodes"></a>트리 보기 노드를 관리 합니다.

이 트리 보기는 앞서 XAML로 생성한 보기와 동일하지만, 대신에 코드에서 노드가 생성됩니다.

```xaml
<TreeView x:Name="sampleTreeView"/>
```

```csharp
private void InitializeTreeView()
{
    TreeViewNode rootNode = new TreeViewNode() { Content = "Flavors" };
    rootNode.IsExpanded = true;
    rootNode.Children.Add(new TreeViewNode() { Content = "Vanilla" });
    rootNode.Children.Add(new TreeViewNode() { Content = "Strawberry" });
    rootNode.Children.Add(new TreeViewNode() { Content = "Chocolate" });

    sampleTreeView.RootNodes.Add(rootNode);
}
```

```vb
Private Sub InitializeTreeView()
    Dim rootNode As New TreeViewNode With {.Content = "Flavors", .IsExpanded = True}
    With rootNode.Children
        .Add(New TreeViewNode With {.Content = "Vanilla"})
        .Add(New TreeViewNode With {.Content = "Strawberry"})
        .Add(New TreeViewNode With {.Content = "Chocolate"})
    End With
    sampleTreeView.RootNodes.Add(rootNode)
End Sub
```

이러한 API는 트리 보기의 데이터 계층을 관리하는 데 사용할 수 있습니다.

| **[TreeView](/uwp/api/windows.ui.xaml.controls.treeview)** | |
| - | - |
| [RootNodes](/uwp/api/windows.ui.xaml.controls.treeview.rootnodes) | 트리 보기에는 하나 이상의 루트 노드가 포함될 수 있습니다. TreeViewNode 개체를 RootNodes 컬렉션에 추가하여 루트 노드를 생성합니다. 루트 노드의 **부모**는 항상 **null**입니다. 루트 노드의 **깊이**는 0입니다. |

| **[TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode)** | |
| - | - |
| [자식](/uwp/api/windows.ui.xaml.controls.treeviewnode.children) | TreeViewNode 개체를 부모 노드의 자식 컬렉션에 추가하여 노드 계층을 만듭니다. 한 노드는 **자식** 컬렉션에 있는 모든 노드의 **부모**가 됩니다. |
| [HasChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.haschildren) | 노드가 자식으로 구현된 경우 이 값은 **true**입니다. **false**는 빈 폴더 또는 항목을 나타냅니다. |
| [HasUnrealizedChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.hasunrealizedchildren) | 확장에 따라 노드를 채우고 있는 경우에는 이 속성을 사용합니다. 이 문서 뒷부분의 _확장 시 노드 채우기_를 참조하세요. |
| [깊이](/uwp/api/windows.ui.xaml.controls.treeviewnode.depth) | 자식 노드가 루트 노드에서 얼마나 멀리 있는지 나타냅니다. |
| [부모](/uwp/api/windows.ui.xaml.controls.treeviewnode.parent) | 이 노드가 포함된 **자식** 컬렉션을 소유하고 있는 TreeViewNode를 가져옵니다. |

트리 보기는 **HasChildren** 및 **HasUnrealizedChildren** 속성을 사용하여 확장/축소 아이콘의 표시 여부를 결정합니다. 속성 중 하나가 **true**이면 아이콘이 표시되고, 그렇지 않으면 표시되지 않습니다.

## <a name="tree-view-node-content"></a>트리 보기 노드 콘텐츠

트리 보기 노드가 [콘텐츠](/uwp/api/windows.ui.xaml.controls.treeviewnode.content) 속성에 표시하는 데이터 항목을 저장할 수 있습니다.

위 예제에서는 콘텐츠가 간단한 문자열 값이었습니다. 여기 나온 트리 보기 노드는 사용자의 사진 폴더를 표시하고 있기 때문에 사진 라이브러리 [StorageFolder](/uwp/api/windows.storage.storagefolder)가 노드의 콘텐츠 속성에 할당됩니다.

```csharp
StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
TreeViewNode pictureNode = new TreeViewNode();
pictureNode.Content = picturesFolder;
```

```vb
Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
Dim pictureNode As New TreeViewNode With {.Content = picturesFolder}
```

트리 보기에 데이터 항목이 표시되는 방법을 지정하기 위해 [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate)을 제공할 수 있습니다.

> [!NOTE]
> Windows 10 버전 1803에서는 콘텐츠가 문자열이 아닌 경우 TreeView 컨트롤의 템플릿을 다시 작성하고 사용자 지정 ItemTemplate를 지정해야 합니다. 자세한 내용은 이 문서의 마지막에 나와 있는 전체 예제를 참조하세요. 이후 버전에서 [TreeView.ItemTemplate](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate) 속성을 설정 합니다.

### <a name="item-container-style"></a>항목 컨테이너 스타일

ItemsSource 또는 RootNodes, 실제 요소 – 각 노드를 표시 하는 데 사용 여부를 "컨테이너" 라는 – [TreeViewItem](/uwp/api/windows.ui.xaml.controls.treeviewitem) 개체입니다. TreeView의를 사용 하 여 컨테이너를 지정 하면 ItemContainerStyle 또는 ItemContainerStyleSelector 속성.

### <a name="item-template-selectors"></a>항목 템플릿 선택기

항목의 형식에 따라 트리 보기 항목에 대 한 다른 DataTemplate을 설정할 수 있습니다. 예를 들어 앱을 파일 탐색기에서 폴더 및 파일에 대 한 다른 하나의 데이터 템플릿을 사용할 수 있습니다.

![다른 데이터 템플릿을 사용 하 여 파일 및 폴더](images/treeview-icons.png)

만들고 항목 템플릿 선택기를 사용 하는 방법의 예는 다음과 같습니다.

```xaml
<Page.Resources>
    <DataTemplate x:Key="FolderTemplate" x:DataType="local:ExplorerItem">
        <TreeViewItem ItemsSource="{x:Bind Children}">
            <StackPanel Orientation="Horizontal">
                <Image Width="20" Source="Assets/folder.png"/>
                <TextBlock Text="{x:Bind Name}" />
            </StackPanel>
        </TreeViewItem>
    </DataTemplate>

    <DataTemplate x:Key="FileTemplate" x:DataType="local:ExplorerItem">
        <TreeViewItem>
            <StackPanel Orientation="Horizontal">
                <Image Width="20" Source="Assets/file.png"/>
                <TextBlock Text="{Binding Name}"/>
            </StackPanel>
        </TreeViewItem>
    </DataTemplate>

    <local:ExplorerItemTemplateSelector
            x:Key="ExplorerItemTemplateSelector"
            FolderTemplate="{StaticResource FolderTemplate}"
            FileTemplate="{StaticResource FileTemplate}" />
</Page.Resources>

<Grid>
    <TreeView ItemsSource="{x:Bind DataSource}"
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

## <a name="interacting-with-a-tree-view"></a>트리 보기와의 상호 작용

여러 가지 방법으로 사용자가 상호 작용을 하도록 트리 보기를 구성할 수 있습니다.

- 노드 확장/축소
- 단일 또는 다중 선택 항목
- 항목 호출을 위한 클릭

### <a name="expandcollapse"></a>확장/축소

자식 노드가 있는 트리 보기는 확장/축소 문자 모양을 클릭하여 언제든지 확장 또는 축소가 가능합니다. 또한 프로그래밍 방식으로 노드를 확장 또는 축소하고 노드 상태가 변경될 때 응답을 할 수 있습니다.

#### <a name="expandcollapse-a-node-programmatically"></a>프로그래밍 방식으로 노드 확대/축소

코드에서 트리 보기 노드를 확장 또는 축소할 수 있는 방법은 두 가지입니다.

- [TreeView](/uwp/api/windows.ui.xaml.controls.treeview) 클래스에는 [Collapse](/uwp/api/windows.ui.xaml.controls.treeview.collapse) 및 [Expand](/uwp/api/windows.ui.xaml.controls.treeview.expand) 메서드가 포함되어 있습니다. 이러한 메서드를 호출하면 확장 또는 축소하고 싶은 TreeViewNode에 전달됩니다.

- 각 [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode)에는 [IsExpanded](/uwp/api/windows.ui.xaml.controls.treeviewnode.isexpanded) 속성이 포함되어 있습니다. 이 속성을 사용하여 노드의 상태를 확인하거나 상태를 변경하도록 속성을 설정할 수 있습니다. 또한 XAML에서 이 속성을 설정하여 노드의 초기 상태를 설정할 수도 있습니다.

### <a name="fill-a-node-when-its-expanding"></a>확장 시 노드 채우기

트리 보기에 다수의 노드를 표시해야 하는 경우도 있고, 얼마나 많은 노드가 필요한지 미리 모르는 경우도 있습니다. TreeView 컨트롤은 가상화가 되지 않으므로 확장 시에는 각 노드를 채우고 축소 시에는 자식 노드를 제거하는 방식으로 리소스를 관리할 수 있습니다.

[확장](/uwp/api/windows.ui.xaml.controls.treeview.expand) 이벤트를 처리하고 [HasUnrealizedChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.hasunrealizedchildren) 속성을 사용하여 확장 시 노드에 자식 노드를 추가합니다. HasUnrealizedChildren 속성은 노드를 채워야 하는지 여부나 자식 컬렉션이 이미 채워져 있는지 여부를 나타냅니다. TreeViewNode는 이 값을 설정하지 않는다는 것을 기억하고 앱 코드에서 이를 관리하는 것이 중요합니다.

이러한 API가 어떻게 사용되는지에 대한 예는 다음과 같습니다. 'FillTreeNode' 구현을 포함한 상황에 대해서는 이 문서 끝에 나와 있는 전체 예제 코드를 참조하세요.

```csharp
private void SampleTreeView_Expanding(TreeView sender, TreeViewExpandingEventArgs args)
{
    if (args.Node.HasUnrealizedChildren)
    {
        FillTreeNode(args.Node);
    }
}
```

```vb
Private Sub SampleTreeView_Expanding(sender As TreeView, args As TreeViewExpandingEventArgs)
    If args.Node.HasUnrealizedChildren Then
        FillTreeNode(args.Node)
    End If
End Sub
```

필수 요건은 아니지만, [축소](/uwp/api/windows.ui.xaml.controls.treeview.collapsed) 이벤트를 처리하고 부모 노드가 닫힐 때 자식 노드를 제거하기를 원할 수도 있습니다. 트리 보기에 다수의 노드가 있거나 노드 데이터가 다양한 리소스를 사용하는 경우에는 중요할 수 있습니다. 닫힌 노드에 자식 노드를 남겨두는 것과 비교해 열릴 때마다 노드를 채우는 것이 성능에 미치는 영향을 고려해야 합니다. 최상의 옵션은 앱에 따라 달라집니다.

축소 이벤트에 대한 처리기의 예는 다음과 같습니다.

```csharp
private void SampleTreeView_Collapsed(TreeView sender, TreeViewCollapsedEventArgs args)
{
    args.Node.Children.Clear();
    args.Node.HasUnrealizedChildren = true;
}
```

```vb
Private Sub SampleTreeView_Collapsed(sender As TreeView, args As TreeViewCollapsedEventArgs)
    args.Node.Children.Clear()
    args.Node.HasUnrealizedChildren = True
End Sub
```

### <a name="invoking-an-item"></a>항목 호출

사용자가 해당 항목을 선택하는 대신 작업을 호출할 수 있습니다(단추처럼 항목 취급). [ItemInvoked](/uwp/api/windows.ui.xaml.controls.treeview.iteminvoked) 이벤트를 처리하여 이러한 사용자 상호 작용에 응답합니다.

> [!NOTE]
> [IsItemClickEnabled](/uwp/api/windows.ui.xaml.controls.listviewbase.isitemclickenabled) 속성을 가지고 있는 ListView와 달리, 항목 호출은 트리 보기에서 항상 활성화되어 있습니다. 여전히 이벤트 처리 여부를 선택할 수 있습니다.

**[TreeViewItemInvokedEventArgs](/uwp/api/windows.ui.xaml.controls.treeviewiteminvokedeventargs) 클래스**

ItemInvoked 이벤트 인수는 호출된 항목에 액세스할 수 있도록 합니다. [InvokedItem](/uwp/api/windows.ui.xaml.controls.treeviewiteminvokedeventargs.invokeditem) 속성은 호출된 노드를 가지고 있습니다. TreeViewNode로 이를 캐스팅하고 TreeViewNode.Content 속성에서 데이터 항목을 가져올 수 있습니다.

다음은 ItemInvoked 이벤트 처리기의 예입니다. 데이터 항목은 [IStorageItem](/uwp/api/windows.storage.istorageitem)이고, 이 예제에는 파일 및 트리에 대한 몇 가지 정보만 나와 있습니다. 또한 노드가 폴더 노드면 노드를 동시에 확장 또는 축소할 수 있습니다. 그렇지 않으면 펼침 단추를 클릭할 때만 노드가 확장 또는 축소됩니다.

```csharp
private void SampleTreeView_ItemInvoked(TreeView sender, TreeViewItemInvokedEventArgs args)
{
    var node = args.InvokedItem as TreeViewNode;
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
Private Sub SampleTreeView_ItemInvoked(sender As TreeView, args As TreeViewItemInvokedEventArgs)
    Dim node = TryCast(args.InvokedItem, TreeViewNode)
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

TreeView 컨트롤은 단일 선택 및 다중 선택을 모두 지원합니다. 기본적으로 노드 선택 기능은 꺼져 있지만, 노드 선택이 가능하도록 [TreeView.SelectionMode](/uwp/api/windows.ui.xaml.controls.treeview.selectionmode) 속성을 설정할 수 있습니다. [TreeViewSelectionMode](/uwp/api/windows.ui.xaml.controls.treeviewselectionmode) 값은 **없음**, **단일** 및 **다중**입니다.

선택 기능이 활성화되면 각 트리 보기 노드 옆에 확인란이 표시되고 선택된 항목이 강조 표시됩니다. 사용자는 확인란을 사용하여 항목을 선택 또는 선택 취소할 수 있습니다. 해당 항목을 클릭해도 여전히 항목이 호출됩니다.

디스테이징 부모 노드를 선택 하거나 선택는 선택 하거나 해당 노드 아래에 있는 모든 자식 항목을 선택 취소 합니다. 일부 경우 하지만의 전체가 아닌 부모 노드에 자식 선택 됩니다으로 부모 노드에 대 한 확인란으로 표시 됩니다으로 확정 되지 않음 (블랙 박스 채워진).

![트리 보기의 다중 선택](images/treeview-selection.png)

선택된 노드는 트리 보기의 [SelectedNodes](/uwp/api/windows.ui.xaml.controls.treeview.selectednodes) 컬렉션에 추가됩니다. [SelectAll](/uwp/api/windows.ui.xaml.controls.treeview.selectall) 메서드를 호출하여 트리 보기의 모든 노드를 선택할 수 있습니다.

> [!NOTE]
> **SelectAll**을 호출하면 SelectionMode에 관계없이 실현된 모든 노드가 선택됩니다. 일관된 사용자 환경을 제공하려면 SelectionMode가 **다중**일 경우에 SelectAll만 호출해야 합니다.

#### <a name="selection-and-realizedunrealized-nodes"></a>선택 및 실현/미실현 노드

트리 보기에 미실현 노드가 있으면 선택 시 이들을 고려하지 않습니다. 미실현 노드에서의 선택과 관련해 유의해야 할 사항들이 몇 가지 있습니다.

- 사용자가 부모 노드를 선택하면 해당 부모 노드에서 실현된 모든 자식 노드가 선택됩니다. 마찬가지로, 모든 자녀 노드를 선택하면 부모 노드도 선택이 됩니다.
- SelectAll 메서드는 SelectedNodes 컬렉션에 실현된 노드만 추가합니다.
- 미실현 자식 노드가 있는 부모 노드를 선택하면 실현이 되었을 때 자식 노드가 선택됩니다.

## <a name="code-examples"></a>코드 예제

### <a name="tree-view-using-xaml"></a>XAML을 사용 하 여 트리 보기

이 예제에서는 XAML에서 간단한 트리 보기 구조를 생성하는 방법을 보여줍니다. 이 트리 보기에는 사용자가 선택할 수 있는 범주화된 아이스크림 맛과 토핑을 보여줍니다. 다중 선택이 활성화되어 있는 상태에서 사용자가 단추를 클릭하면 SelectedItems가 기본 앱 UI에 표시됩니다.

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" Padding="100">
    <SplitView IsPaneOpen="True"
               DisplayMode="Inline"
               OpenPaneLength="296">
        <SplitView.Pane>
            <TreeView x:Name="DessertTree" SelectionMode="Multiple">
                <TreeView.RootNodes>
                    <TreeViewNode Content="Flavors" IsExpanded="True">
                        <TreeViewNode.Children>
                            <TreeViewNode Content="Vanilla"/>
                            <TreeViewNode Content="Strawberry"/>
                            <TreeViewNode Content="Chocolate"/>
                        </TreeViewNode.Children>
                    </TreeViewNode>

                    <TreeViewNode Content="Toppings">
                        <TreeViewNode.Children>
                            <TreeViewNode Content="Candy">
                                <TreeViewNode.Children>
                                    <TreeViewNode Content="Chocolate"/>
                                    <TreeViewNode Content="Mint"/>
                                    <TreeViewNode Content="Sprinkles"/>
                                </TreeViewNode.Children>
                            </TreeViewNode>
                            <TreeViewNode Content="Fruits">
                                <TreeViewNode.Children>
                                    <TreeViewNode Content="Mango"/>
                                    <TreeViewNode Content="Peach"/>
                                    <TreeViewNode Content="Kiwi"/>
                                </TreeViewNode.Children>
                            </TreeViewNode>
                            <TreeViewNode Content="Berries">
                                <TreeViewNode.Children>
                                    <TreeViewNode Content="Strawberry"/>
                                    <TreeViewNode Content="Blueberry"/>
                                    <TreeViewNode Content="Blackberry"/>
                                </TreeViewNode.Children>
                            </TreeViewNode>
                        </TreeViewNode.Children>
                    </TreeViewNode>
                </TreeView.RootNodes>
            </TreeView>
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
```

```csharp
private void OrderButton_Click(object sender, RoutedEventArgs e)
{
    FlavorList.Text = string.Empty;
    ToppingList.Text = string.Empty;

    foreach (TreeViewNode node in DessertTree.SelectedNodes)
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
    if (DessertTree.SelectionMode == TreeViewSelectionMode.Multiple)
    {
        DessertTree.SelectAll();
    }
}
```

```vb
Private Sub OrderButton_Click(sender As Object, e As RoutedEventArgs)
    FlavorList.Text = String.Empty
    ToppingList.Text = String.Empty
    For Each node As TreeViewNode In DessertTree.SelectedNodes
        If node.Parent.Content?.ToString() = "Flavors" Then
            FlavorList.Text += node.Content & "; "
        ElseIf node.HasChildren = False Then
            ToppingList.Text += node.Content & "; "
        End If
    Next
End Sub

Private Sub SelectAllButton_Click(sender As Object, e As RoutedEventArgs)
    If DessertTree.SelectionMode = TreeViewSelectionMode.Multiple Then
        DessertTree.SelectAll()
    End If
End Sub
```

### <a name="tree-view-using-data-binding"></a>데이터 바인딩을 사용 하는 트리 보기

이 예제에서는 이전 예제와 동일한 트리 보기를 만드는 방법을 보여 줍니다. 그러나 XAML에서 데이터 계층 구조를 만드는 대신 데이터 코드에서 생성 되 고 트리 보기의 ItemsSource 속성에 바인딩됩니다. (이전 예제에 표시 된 단추 이벤트 처리기에이 예제도 적용 합니다.)

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" Padding="100">
    <SplitView IsPaneOpen="True"
               DisplayMode="Inline"
               OpenPaneLength="296">
        <SplitView.Pane>
            <TreeView Name="DessertTree"
                      SelectionMode="Multiple"
                      ItemsSource="{x:Bind DataSource}">
                <TreeView.ItemTemplate>
                    <DataTemplate x:DataType="local:Item">
                        <TreeViewItem ItemsSource="{x:Bind Children}"
                                      Content="{x:Bind Name}"/>
                    </DataTemplate>
                </TreeView.ItemTemplate>
            </TreeView>
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
```

```csharp
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

    // Button event handlers...
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
```

### <a name="pictures-and-music-library-tree-view"></a>사진 및 음악 라이브러리 트리 보기

이 예제는 사용자 사진 및 음악 라이브러리의 콘텐츠 및 구조를 표시하는 트리 보기를 생성하는 방법을 보여줍니다. 항목 수를 미리 알 수 없으므로 확장 시에 각 노드를 채우고 축소 시에 노드를 비웁니다.

사용자 지정 항목 템프릿은 형식이 [IStorageItem](/uwp/api/windows.storage.istorageitem)인 데이터 항목을 표시하는 데 사용됩니다.

> [!IMPORTANT]
> 이 예제의 코드에서는 picturesLibrary 및 musicLibrary 기능이 필요합니다. 파일 액세스에 대한 자세한 내용은 [파일 액세스 권한](../../files/file-access-permissions.md), [열거 및 쿼리 파일/폴더](../../files/quickstart-listing-files-and-folders.md) 및 [음악, 사진 및 동영상 라이브러리의 파일 및 폴더](../../files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)를 참조하세요.

```xaml
<Page
    x:Class="TreeViewApp1.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:TreeViewApp1"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">
    <Page.Resources>
        <DataTemplate x:Key="TreeViewItemDataTemplate">
            <Grid Height="44">
                <TextBlock
                    Text="{Binding Content.DisplayName}"
                    HorizontalAlignment="Left"
                    VerticalAlignment="Center"
                    Style="{ThemeResource BodyTextBlockStyle}"/>
            </Grid>
        </DataTemplate>

        <Style TargetType="TreeView">
            <Setter Property="IsTabStop" Value="False" />
            <Setter Property="Template">
                <Setter.Value>
                    <ControlTemplate TargetType="TreeView">
                        <TreeViewList x:Name="ListControl"
                                      ItemTemplate="{StaticResource TreeViewItemDataTemplate}"
                                      ItemContainerStyle="{StaticResource TreeViewItemStyle}"
                                      CanDragItems="True"
                                      AllowDrop="True"
                                      CanReorderItems="True">
                            <TreeViewList.ItemContainerTransitions>
                                <TransitionCollection>
                                    <ContentThemeTransition />
                                    <ReorderThemeTransition />
                                    <EntranceThemeTransition IsStaggeringEnabled="False" />
                                </TransitionCollection>
                            </TreeViewList.ItemContainerTransitions>
                        </TreeViewList>
                    </ControlTemplate>
                </Setter.Value>
            </Setter>
        </Style>
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
                    <TreeView x:Name="sampleTreeView" Grid.Row="1" SelectionMode="Single"
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
    TreeViewNode pictureNode = new TreeViewNode();
    pictureNode.Content = picturesFolder;
    pictureNode.IsExpanded = true;
    pictureNode.HasUnrealizedChildren = true;
    sampleTreeView.RootNodes.Add(pictureNode);
    FillTreeNode(pictureNode);

    // Get Music library.
    StorageFolder musicFolder = KnownFolders.MusicLibrary;
    TreeViewNode musicNode = new TreeViewNode();
    musicNode.Content = musicFolder;
    musicNode.IsExpanded = true;
    musicNode.HasUnrealizedChildren = true;
    sampleTreeView.RootNodes.Add(musicNode);
    FillTreeNode(musicNode);
}

private async void FillTreeNode(TreeViewNode node)
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
        var newNode = new TreeViewNode();
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

private void SampleTreeView_Expanding(TreeView sender, TreeViewExpandingEventArgs args)
{
    if (args.Node.HasUnrealizedChildren)
    {
        FillTreeNode(args.Node);
    }
}

private void SampleTreeView_Collapsed(TreeView sender, TreeViewCollapsedEventArgs args)
{
    args.Node.Children.Clear();
    args.Node.HasUnrealizedChildren = true;
}

private void SampleTreeView_ItemInvoked(TreeView sender, TreeViewItemInvokedEventArgs args)
{
    var node = args.InvokedItem as TreeViewNode;
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
```

```vb
Public Sub New()
    InitializeComponent()
    InitializeTreeView()
End Sub

Private Sub InitializeTreeView()
    ' A TreeView can have more than 1 root node. The Pictures library
    ' and the Music library will each be a root node in the tree.
    ' Get Pictures library.
    Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
    Dim pictureNode As New TreeViewNode With {
        .Content = picturesFolder,
        .IsExpanded = True,
        .HasUnrealizedChildren = True
    }
    sampleTreeView.RootNodes.Add(pictureNode)
    FillTreeNode(pictureNode)

    ' Get Music library.
    Dim musicFolder As StorageFolder = KnownFolders.MusicLibrary
    Dim musicNode As New TreeViewNode With {
        .Content = musicFolder,
        .IsExpanded = True,
        .HasUnrealizedChildren = True
    }
    sampleTreeView.RootNodes.Add(musicNode)
    FillTreeNode(musicNode)
End Sub

Private Async Sub FillTreeNode(node As TreeViewNode)
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
        Dim newNode As New TreeViewNode With {
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

Private Sub SampleTreeView_Expanding(sender As TreeView, args As TreeViewExpandingEventArgs)
    If args.Node.HasUnrealizedChildren Then
        FillTreeNode(args.Node)
    End If
End Sub

Private Sub SampleTreeView_Collapsed(sender As TreeView, args As TreeViewCollapsedEventArgs)
    args.Node.Children.Clear()
    args.Node.HasUnrealizedChildren = True
End Sub

Private Sub SampleTreeView_ItemInvoked(sender As TreeView, args As TreeViewItemInvokedEventArgs)
    Dim node = TryCast(args.InvokedItem, TreeViewNode)
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
```

## <a name="related-articles"></a>관련 문서

- [TreeView 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview)
- [ListView 클래스](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx)
- [ListView 및 GridView](listview-and-gridview.md)
