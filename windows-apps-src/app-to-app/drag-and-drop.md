---
description: "이 문서에서는 UWP(유니버설 Windows 플랫폼) 앱에 끌어서 놓기를 추가하는 방법을 설명합니다."
title: "끌어서 놓기"
ms.assetid: A15ED2F5-1649-4601-A761-0F6C707A8B7E
author: awkoren
ms.sourcegitcommit: 03f3f86ed1310e6e3ac5f53cc5e81ebef708a1a2
ms.openlocfilehash: ffa2f0f368a61ef4f3003c1fa03e143b26c6859b

---
# 끌어서 놓기

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


이 문서에서는 UWP(유니버설 Windows 플랫폼) 앱에 끌어서 놓기를 추가하는 방법을 설명합니다. 끌어서 놓기는 이미지 및 파일과 같은 콘텐츠를 조작하는 전형적이고 자연스러운 방법입니다. 구현되면 끌어서 놓기가 앱에서 앱, 앱에서 데스크톱, 데스크톱에서 앱을 비롯한 모든 방향으로 원활하게 작동합니다.

## 유효한 영역 설정

[
            **AllowDrop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.AllowDrop) 및 [**CanDrag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.CanDrag) 속성을 사용하여 끌어서 놓기를 사용할 수 있는 앱 영역을 지정할 수 있습니다.

아래 태그는 XAML에서 [**AllowDrop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.AllowDrop)을 사용하여 앱의 특정 영역을 놓기 작업이 가능하도록 설정하는 방법을 보여 줍니다. 사용자가 다른 위치를 끌려고 해도 시스템에서 허용되지 않습니다. 사용자가 앱의 아무 곳에나 항목을 놓을 수 있게 하려면 전체 배경을 놓기 대상으로 설정합니다.

[!code-xml[기본](./code/drag_drop/cs/MainPage.xaml#SnippetDropArea)]

끌기를 사용할 때는 일반적으로 끌 수 있는 대상을 구체적으로 지정하려고 할 것입니다. 사용자는 앱의 모든 항목이 아니라 사진과 같은 특정 항목을 끌려고 합니다. 다음은 XAML을 사용하여 [**CanDrag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.CanDrag)를 설정하는 방법입니다.

[!code-xml[기본](./code/drag_drop/cs/MainPage.xaml#SnippetDragArea)]

이 문서 뒷부분에 나오는 것처럼 UI를 사용자 지정하려는 경우가 아니면 끌기를 허용하기 위해 다른 작업을 수행할 필요가 없습니다. 놓기를 수행하려면 몇 가지 추가 단계가 필요합니다.

## DragOver 이벤트 처리

[
            **DragOver**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DragOver) 이벤트는 사용자가 앱에서 항목을 놓을 때가 아니라 항목을 끌 때 발생합니다. 이 처리기에서 [**AcceptedOperation**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.AcceptedOperation) 속성을 사용하여 앱이 지원하는 작업 종류를 지정해야 합니다. 복사가 가장 일반적입니다.

[!code-cs[기본](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_DragOver)]

## Drop 이벤트 처리

[
            **Drop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.Drop) 이벤트는 유효한 놓기 영역에 항목을 놓을 때 발생합니다. [
            **DataView**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.DataView) 속성을 사용하여 처리합니다.

아래 예제에서는 편의상 사용자가 사진 한 장을 놓았다고 가정하고 액세스합니다. 실제로 사용자는 다양한 형식의 여러 항목을 동시에 놓을 수 있습니다. 앱은 사용자가 놓은 파일 형식을 확인하고 그에 따라 처리하며, 앱이 지원하지 않는 작업을 사용자가 시도할 때 알림을 표시함으로써 이러한 가능성을 처리해야 합니다.

[!code-cs[기본](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_Drop)]

## UI 사용자 지정

시스템에서 끌어서 놓기를 위한 기본 UI를 제공합니다. 그러나 사용자 지정 자막과 문자 모양을 설정하거나 UI를 전혀 표시하지 않도록 선택하여 UI의 다양한 부분을 사용자 지정하도록 선택할 수도 있습니다. UI를 사용자 지정하려면 [**DragEventArgs.DragUIOverride**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.DragUIOverride) 속성을 사용합니다.

[!code-cs[기본](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_DragOverCustom)]

## 참고 항목

* [AllowDrop](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.allowdrop.aspx)
* [CanDrag](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.candrag.aspx)
* [DragOver](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.dragover.aspx)
* [AcceptedOperation](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.acceptedoperation.aspx)
* [DataView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.dataview.aspx)
* [DragUiOverride](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.draguioverride.aspx)
* [놓기](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.drop.aspx)


<!--HONumber=Jun16_HO4-->


