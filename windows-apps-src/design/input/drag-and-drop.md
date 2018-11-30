---
description: 이 문서에서는 UWP(유니버설 Windows 플랫폼) 앱에 끌어서 놓기를 추가하는 방법을 설명합니다.
title: 끌어서 놓기
ms.assetid: A15ED2F5-1649-4601-A761-0F6C707A8B7E
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e508feb8a530f29b40d5a3839df573cb2ce89896
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8207660"
---
# <a name="drag-and-drop"></a>끌어서 놓기

끌어서 놓기는 Windows 바탕화면에 있는 응용 프로그램 내 또는 응용 프로그램 간에 데이터를 전송하는 직관적인 방법입니다. 끌어서 놓기의 표준 제스처(손가락으로 누른 채 이동하기 또는 마우스나 스타일러스로 눌러서 이동하기)를 사용하면 응용 프로그램 간에 또는 응용 프로그램 내에서 데이터를 전송할 수 있습니다.

> **중요 API**: [CanDrag 속성](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.CanDrag), [AllowDrop 속성](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.AllowDrop) 

끌기 제스처가 트리거되는 영역 또는 응용 프로그램인 끌기 소스는 텍스트, RTF, HTML, 비트맵, 저장소 항목 또는 사용자 지정 데이터 형식 등의 표준 데이터 형식을 포함할 수 있는 데이터 패키지 개체를 입력함으로써 데이터가 전송되도록 해줍니다. 소스는 또한 복사, 이동 또는 연결 등 지원되는 작업의 종류도 나타냅니다. 마우스 포인터를 놓으면 놓기가 발생합니다. 포인터 아래에 있는 영역이나 응용 프로그램인 놓기 대상은 데이터 패키지를 처리하고 수행된 작업 유형을 반환합니다.

끌어서 놓기 도중 끌기 UI는 발생하고 있는 끌어서 놓기 작업의 유형을 시각적으로 보여줍니다. 이 시각적 피드백은 처음에는 소스에 의해 제공되지만 마우스 포인터가 대상 위로 이동할 때 대상에 의해 변경될 수 있습니다.

최신 끌어서 놓기는 UWP를 지원하는 모든 장치에서 사용할 수 있습니다. 이 문서에서는 최신 끌어서 놓기의 XAML API를 중점적으로 설명하지만 클래식 Windows 앱을 포함하여 모든 종류의 응용 프로그램 간에 또는 내에서 끌어서 놓기를 사용하여 데이터를 전송할 수 있습니다. 구현되면 끌어서 놓기가 앱에서 앱, 앱에서 데스크톱, 데스크톱에서 앱을 비롯한 모든 방향으로 원활하게 작동합니다.

다음은 앱에서 끌어서 놓기를 사용하기 위해 수행해야 하는 작업에 대한 간단한 설명입니다.

1. **CanDrag** 속성을 True로 설정하여 특정 요소에 대해 끌기를 설정합니다.  
2. 데이터 패키지를 작성합니다. 시스템에서 이미지 및 텍스트를 자동으로 처리하지만, 그 외 다른 콘텐츠의 경우 사용자가 **DragStarted** 및 **DragCompleted** 이벤트를 처리하여 사용자만의 데이터 패키지를 만들어야 합니다. 
3. 놓아진 콘텐츠를 받을 수 있는 모든 요소에서 **AllowDrop** 속성을 **True**로 설정하여 놓기를 설정합니다. 
4. 시스템에서 해당 요소가 받을 수 있는 끌기 작업 유형을 알 수 있도록 **DragOver** 이벤트를 처리합니다. 
5. 놓아진 콘텐츠를 받도록 **Drop** 이벤트를 처리합니다. 



## <a name="enable-dragging"></a>끌기 사용

요소에 대해 끌기를 설정하려면 [**CanDrag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.CanDrag) 속성을 **True**로 설정합니다. 이렇게 하면 요소 및 요소에 포함된 요소(ListView와 같은 모음의 경우)에 끌기를 사용할 수 있습니다.

끌 수 있는 항목을 구체적으로 지정하세요. 사용자는 이미지나 텍스트와 같은 특정 항목만 앱에서 끌고자 할 것입니다. 

다음은 [**CanDrag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.CanDrag)를 설정하는 방법입니다.

[!code-xml[Main](./code/drag_drop/cs/MainPage.xaml#SnippetDragArea)]

이 문서 뒷부분에 나오는 것처럼 UI를 사용자 지정하려는 경우가 아니면 끌기를 허용하기 위해 다른 작업을 수행할 필요가 없습니다. 놓기를 수행하려면 몇 가지 추가 단계가 필요합니다.

## <a name="construct-a-data-package"></a>데이터 패키지 작성 

대부분의 경우, 데이터 패키지는 자동으로 작성됩니다. 자동으로 처리되는 항목은 다음과 같습니다.
* 이미지
* 텍스트 

그 외 콘텐츠의 경우, 사용자가 **DragStarted** 및 **DragCompleted** 이벤트를 처리하고 이를 사용하여 사용자 고유의 [DataPackage](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage)를 만들어야 합니다.

## <a name="enable-dropping"></a>놓기 사용

다음 태그는 XAML에서 [**AllowDrop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.AllowDrop)을 사용하여 앱의 특정 영역을 놓기를 수행할 수 있는 영역으로 설정하는 방법을 보여줍니다. 사용자가 다른 위치를 끌려고 해도 시스템에서 허용되지 않습니다. 사용자가 앱의 아무 곳에나 항목을 놓을 수 있게 하려면 전체 배경을 놓기 대상으로 설정합니다.

[!code-xml[Main](./code/drag_drop/cs/MainPage.xaml#SnippetDropArea)]


## <a name="handle-the-dragover-event"></a>DragOver 이벤트 처리

[**DragOver**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DragOver) 이벤트는 사용자가 앱에서 항목을 놓을 때가 아니라 항목을 끌 때 발생합니다. 이 처리기에서 [**AcceptedOperation**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.AcceptedOperation) 속성을 사용하여 앱이 지원하는 작업 종류를 지정해야 합니다. 복사가 가장 일반적입니다.

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_DragOver)]

## <a name="process-the-drop-event"></a>Drop 이벤트 처리

[**Drop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.Drop) 이벤트는 유효한 놓기 영역에 항목을 놓을 때 발생합니다. [**DataView**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.DataView) 속성을 사용하여 처리합니다.

아래 예제에서는 편의상 사용자가 사진 한 장을 놓았다고 가정하고 이에 직접 액세스합니다. 실제로 사용자는 다양한 형식의 여러 항목을 동시에 놓을 수 있습니다. 앱에서는 삭제된 파일 유형과 현재 개수를 확인하여 이에 대한 가능성을 처리하고 각각을 이에 따라 처리해야 합니다. 또한 사용자에게 앱에서 지원하지 않는 작업 수행 시도 유무에 대해서 알리는 것을 고려해야 합니다.

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_Drop)]

## <a name="customize-the-ui"></a>UI 사용자 지정

시스템에서 끌어서 놓기를 위한 기본 UI를 제공합니다. 그러나 사용자 지정 자막과 문자 모양을 설정하거나 UI를 전혀 표시하지 않도록 선택하여 UI의 다양한 부분을 사용자 지정하도록 선택할 수도 있습니다. UI를 사용자 지정하려면 [**DragEventArgs.DragUIOverride**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.DragUIOverride) 속성을 사용합니다.

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_DragOverCustom)]

## <a name="open-a-context-menu-on-an-item-you-can-drag-with-touch"></a>터치로 끌 수 있는 항목에서 상황에 맞는 메뉴 열기

터치를 사용할 때 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement)를 끌고 해당 상황에 맞는 메뉴를 여는 작업은 비슷한 터치 제스처를 공유합니다. 각각은 길게 누르기로 시작됩니다. 시스템에서 둘 다 지원하는 앱의 요소의 경우 두 작업 간을 식별하는 방법은 다음과 같습니다. 

* 사용자가 항목을 누른 채 500밀리초 내에 끌기 시작하면 항목이 끌리고 상황에 맞는 메뉴가 표시되지 않습니다. 
* 사용자가 항목을 누른 채 500밀리초 내에 끌지 않으면 상황에 맞는 메뉴가 열립니다. 
* 상황에 맞는 메뉴가 열린 후 사용자가 항목을 끌려고 하면(손가락을 떼지 않고) 상황에 맞는 메뉴가 해제되고 끌기가 시작됩니다.

## <a name="designate-an-item-in-a-listview-or-gridview-as-a-folder"></a>ListView 또는 GridView의 항목을 폴더로 지정

[**ListViewItem**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ListViewItem) 또는 [**GridViewItem**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.GridViewItem)을 폴더로 지정할 수 있습니다. 이는 TreeView 및 파일 탐색기 시나리오에 특히 유용합니다. 이렇게 하려면 해당 항목에서 명시적으로 [**AllowDrop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.AllowDrop) 속성을 **True**로 설정합니다. 

시스템은 폴더 및 비폴더 항목에 놓을 적절한 애니메이션을 자동으로 표시합니다. 데이터 원본을 업데이트하고 대상 폴더에 놓은 항목을 추가하기 위해 앱 코드는 폴더 항목(및 비폴더 항목)에서 [**Drop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.Drop) 이벤트를 계속 처리해야 합니다.

## <a name="implementing-custom-drag-and-drop"></a>사용자 지정 끌어서 놓기 구현

[UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 클래스는 끌어서 놓기를 구현하는 데 필요한 대부분의 작업을 수행해 줍니다. 하지만 [Windows.ApplicationModel.DataTransfer.DragDrop.Core 네임 스페이스](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core)의 Api를 사용 하 여 사용자 고유의 버전을 구현할 수 있습니다.

| 기능 | WinRT API |
| --- | --- |
|  끌기 사용 | [CoreDragOperation](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragoperation)  |
|  데이터 패키지 만들기 | [DataPackage](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage)  |
| 손으로 셸로 끌기  | [CoreDragOperation.StartAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragoperation)  |
| 셸에서 놓기 받기  | [CoreDragDropManager](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragdropmanager)<br/>[ICoreDropOperationTarget](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.icoredropoperationtarget)    |



## <a name="see-also"></a>참고 항목

* [앱 간 통신](index.md)
* [AllowDrop](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.allowdrop.aspx)
* [CanDrag](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.candrag.aspx)
* [DragOver](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.dragover.aspx)
* [AcceptedOperation](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.acceptedoperation.aspx)
* [DataView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.dataview.aspx)
* [DragUIOverride](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.draguioverride.aspx)
* [놓기](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.drop.aspx)
* [IsDragSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.isdragsource.aspx)
