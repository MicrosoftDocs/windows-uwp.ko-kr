---
description: 이 문서에서는 Windows 앱에서 끌어서 놓기를 추가 하는 방법을 설명 합니다.
title: 끌어서 놓기
ms.assetid: A15ED2F5-1649-4601-A761-0F6C707A8B7E
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8ab8d696ddb1a4ef9e3dc3549754cbf51fc91374
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220546"
---
# <a name="drag-and-drop"></a>끌어서 놓기

끌어서 놓기는 응용 프로그램 내에서 또는 Windows 데스크톱의 응용 프로그램 간에 데이터를 전송 하는 직관적인 방법입니다. 끌어서 놓기를 사용 하면 사용자가 표준 제스처를 사용 하 여 응용 프로그램이 나 응용 프로그램 간에 데이터를 전송할 수 있습니다. 즉, 손가락을 사용 하 여 누르고 있다가 마우스나 스타일러스를 사용 하 여 누르고 이동 합니다.

> **중요 한 api**: [candrag 속성](/uwp/api/windows.ui.xaml.uielement.candrag), [allowdrop 속성](/uwp/api/windows.ui.xaml.uielement.allowdrop) 

끌기 제스처가 트리거되는 응용 프로그램이 나 영역인 끌기 소스는 텍스트, RTF, HTML, 비트맵, 저장소 항목 또는 사용자 지정 데이터 형식을 비롯 한 표준 데이터 형식을 포함할 수 있는 데이터 패키지 개체를 채워 전송할 데이터를 제공 합니다. 소스는 지원 되는 작업의 종류 (복사, 이동 또는 연결)도 나타냅니다. 포인터를 놓으면 drop이 발생 합니다. 포인터 아래에 있는 응용 프로그램 또는 영역인 놓기 대상은 데이터 패키지를 처리 하 고 수행 된 작업의 유형을 반환 합니다.

끌어서 놓기 작업을 수행 하는 동안 끌기 UI는 발생 하는 끌어서 놓기 작업의 형식을 시각적으로 표시 합니다. 이 시각적 피드백은 처음에 소스에서 제공 되지만 포인터가 위로 이동할 때 대상에 의해 변경 될 수 있습니다.

최신 끌어서 놓기를 UWP를 지 원하는 모든 장치에서 사용할 수 있습니다. 클래식 Windows 앱을 포함 하 여 모든 종류의 응용 프로그램 간에 데이터를 전송할 수 있습니다. 그러나이 문서에서는 최신 끌어서 놓기를 위한 XAML API에 초점을 맞춘 것입니다. 구현되면 끌어서 놓기가 앱에서 앱, 앱에서 데스크톱, 데스크톱에서 앱을 비롯한 모든 방향으로 원활하게 작동합니다.

앱에서 끌어서 놓기를 사용 하도록 설정 하기 위해 수행 해야 하는 작업에 대 한 개요는 다음과 같습니다.

1. **Candrag** 속성을 true로 설정 하 여 요소에 대 한 끌기를 가능 하 게 합니다.  
2. 데이터 패키지를 빌드합니다. 시스템에서는 이미지와 텍스트를 자동으로 처리 하지만 다른 콘텐츠에 대해서는 **Dragstarted** 및 **dragstarted** 이벤트를 처리 하 고이를 사용 하 여 고유한 데이터 패키지를 생성 해야 합니다. 
3. 삭제 된 콘텐츠를 받을 수 있는 모든 요소에 대해 **Allowdrop** 속성을 **true** 로 설정 하 여 삭제를 사용 하도록 설정 합니다. 
4. **System.windows.uielement.dragover>** 이벤트를 처리 하 여 요소가 수신할 수 있는 끌기 작업 유형을 시스템에 알릴 수 있습니다. 
5. **Drop** 이벤트를 처리 하 여 삭제 된 콘텐츠를 수신 합니다. 



## <a name="enable-dragging"></a>끌기 사용

요소에 대 한 끌기를 사용 하도록 설정 하려면 [**Candrag**](/uwp/api/windows.ui.xaml.uielement.candrag) 속성을 **true**로 설정 합니다. 이로 인해 ListView와 같은 컬렉션의 경우 요소 및이 요소에 포함 된 요소를 만듭니다.

끌 수 있는 기능에 대해 구체적으로 지정 합니다. 사용자는 응용 프로그램에서 이미지 또는 텍스트와 같은 특정 항목만 끌어 놓는 것을 원하지 않습니다. 

[**Candrag**](/uwp/api/windows.ui.xaml.uielement.candrag)를 설정 하는 방법은 다음과 같습니다.

:::code language="xml" source="~/../snippets-windows/windows-uwp/design/input/drag_drop/cs/MainPage.xaml" id="SnippetDragArea":::

UI를 사용자 지정 하려는 경우 (이 문서의 뒷부분에서 설명)에는 다른 작업을 수행 하 여 끌기를 허용할 필요가 없습니다. 삭제 하려면 몇 가지 추가 단계가 필요 합니다.

## <a name="construct-a-data-package"></a>데이터 패키지 생성 

대부분의 경우 시스템은 데이터 패키지를 생성 합니다. 시스템은 다음을 자동으로 처리 합니다.
* 이미지
* 텍스트 

다른 콘텐츠의 경우 **Dragstarted** 및 **dragstarted** 이벤트를 처리 하 고이를 사용 하 여 고유한 [DataPackage](/uwp/api/windows.applicationmodel.datatransfer.datapackage)을 생성 해야 합니다.

## <a name="enable-dropping"></a>삭제 사용

다음 태그는 XAML에서 [**Allowdrop**](/uwp/api/windows.ui.xaml.uielement.allowdrop) 을 사용 하 여 앱의 특정 영역을 삭제 하는 데 유효한 것으로 설정 하는 방법을 보여 줍니다. 사용자가 다른 위치를 삭제 하려고 하면 시스템에서이를 허용 하지 않습니다. 사용자가 앱의 모든 위치에서 항목을 삭제할 수 있도록 하려면 전체 배경을 놓기 대상으로 설정 합니다.

:::code language="xml" source="~/../snippets-windows/windows-uwp/design/input/drag_drop/cs/MainPage.xaml" id="SnippetDropArea":::


## <a name="handle-the-dragover-event"></a>DragOver 이벤트 처리

[**System.windows.uielement.dragover>**](/uwp/api/windows.ui.xaml.uielement.dragover) 이벤트는 사용자가 앱 위로 항목을 끌 때 발생 하지만 아직 삭제 하지 않은 경우에 발생 합니다. 이 처리기에서는 [**AcceptedOperation**](/uwp/api/windows.ui.xaml.drageventargs.acceptedoperation) 속성을 사용 하 여 앱이 지 원하는 작업의 종류를 지정 해야 합니다. Copy는 가장 일반적입니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/design/input/drag_drop/cs/MainPage.xaml.cs" id="SnippetGrid_DragOver":::

## <a name="process-the-drop-event"></a>Drop 이벤트 처리

[**Drop**](/uwp/api/windows.ui.xaml.uielement.drop) 이벤트는 사용자가 유효한 끌어 놓기 영역에서 항목을 해제할 때 발생 합니다. [**DataView**](/uwp/api/windows.ui.xaml.drageventargs.dataview) 속성을 사용 하 여 처리 합니다.

아래 예제에서는 간단한 설명을 위해 사용자가 단일 사진을 삭제 하 고 직접 액세스 한다고 가정 합니다. 실제로 사용자는 다양 한 형식의 여러 항목을 동시에 삭제할 수 있습니다. 응용 프로그램은 어떤 형식의 파일이 삭제 되었는지 확인 하 고 각각에 대해 적절 하 게 처리 하 여 이러한 가능성을 처리 해야 합니다. 또한 앱에서 지원 하지 않는 작업을 수행 하려고 하는 경우 사용자에 게 알리는 것이 좋습니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/design/input/drag_drop/cs/MainPage.xaml.cs" id="SnippetGrid_Drop":::

## <a name="customize-the-ui"></a>UI 사용자 지정

시스템에서는 끌어서 놓기를 위한 기본 UI를 제공 합니다. 그러나 사용자 지정 캡션과 문자 모양을 설정 하거나 UI를 전혀 표시 하지 않도록 선택 하 여 UI의 여러 부분을 사용자 지정할 수도 있습니다. UI를 사용자 지정 하려면 [**Drageventargs. DragUIOverride**](/uwp/api/windows.ui.xaml.drageventargs.draguioverride) 속성을 사용 합니다.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/design/input/drag_drop/cs/MainPage.xaml.cs" id="SnippetGrid_DragOverCustom":::

## <a name="open-a-context-menu-on-an-item-you-can-drag-with-touch"></a>터치를 사용 하 여 끌 수 있는 항목에 대 한 상황에 맞는 메뉴 열기

터치를 사용 하는 경우 [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) 를 끌어서 상황에 맞는 메뉴를 열면 비슷한 터치 제스처를 공유 합니다. 각각은 누르고 있기 시작 합니다. 다음은 두 작업을 모두 지 원하는 앱의 요소에 대 한 두 작업 간에 시스템이 어떻게 구분 하는 방법입니다. 

* 사용자가 항목을 눌렀다가 유지 하 고 500 밀리초 내로 끌어 오면 항목은 끌고 상황에 맞는 메뉴는 표시 되지 않습니다. 
* 사용자가를 눌렀다가 유지 하지만 500 밀리초 내로 끌어 오면 안 되는 상황에 맞는 메뉴가 열립니다. 
* 상황에 맞는 메뉴가 열려 있는 상태에서 사용자가 손가락을 떼지 않고 항목을 끌기 하려고 하면 상황에 맞는 메뉴가 해제 되 고 끌기가 시작 됩니다.

## <a name="designate-an-item-in-a-listview-or-gridview-as-a-folder"></a>ListView 또는 GridView의 항목을 폴더로 지정

[**ListViewItem**](/uwp/api/Windows.UI.Xaml.Controls.ListViewItem) 또는 [**GridViewItem**](/uwp/api/Windows.UI.Xaml.Controls.GridViewItem) 을 폴더로 지정할 수 있습니다. 이 기능은 TreeView 및 파일 탐색기 시나리오에 특히 유용 합니다. 이렇게 하려면 해당 항목에 대해 [**Allowdrop**](/uwp/api/windows.ui.xaml.uielement.allowdrop) 속성을 **True** 로 명시적으로 설정 합니다. 

폴더에 폴더 이외의 항목에 대 한 적절 한 애니메이션을 자동으로 표시 합니다. 응용 프로그램 코드는 데이터 원본을 업데이트 하 고 삭제 된 항목을 대상 폴더에 추가 하기 위해 폴더 항목 뿐만 아니라 폴더 항목에 대 한 [**Drop**](/uwp/api/windows.ui.xaml.uielement.drop) 이벤트를 계속 처리 해야 합니다.

## <a name="implementing-custom-drag-and-drop"></a>사용자 지정 끌어서 놓기 구현

[UIElement](/uwp/api/windows.ui.xaml.uielement) 클래스는 끌어서 놓기를 구현 하는 대부분의 작업을 수행 합니다. 하지만 원하는 경우 [DataTransfer 네임 스페이스](/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core)에서 api를 사용 하 여 고유한 버전을 구현할 수 있습니다.

| 기능 | WinRT API |
| --- | --- |
|  끌기 사용 | [CoreDragOperation](/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragoperation)  |
|  데이터 패키지 만들기 | [DataPackage](/uwp/api/windows.applicationmodel.datatransfer.datapackage)  |
| 셸로 끌어서 놓기  | [CoreDragOperation.StartAsync](/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragoperation)  |
| 셸에서 수신 삭제  | [CoreDragDropManager](/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragdropmanager)<br/>[ICoreDropOperationTarget](/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.icoredropoperationtarget)    |



## <a name="see-also"></a>참고 항목

* [앱 간 통신](index.md)
* [System.windows.uielement.allowdrop](/uwp/api/windows.ui.xaml.uielement.allowdrop)
* [CanDrag](/uwp/api/windows.ui.xaml.uielement.candrag)
* [System.windows.uielement.dragover>](/uwp/api/windows.ui.xaml.uielement.dragover)
* [AcceptedOperation](/uwp/api/windows.ui.xaml.drageventargs.acceptedoperation)
* [DataView](/uwp/api/windows.ui.xaml.drageventargs.dataview)
* [DragUIOverride](/uwp/api/windows.ui.xaml.drageventargs.draguioverride)
* [그림자](/uwp/api/windows.ui.xaml.uielement.drop)
* [IsDragSource](/uwp/api/windows.ui.xaml.controls.listviewbase.isdragsource)
