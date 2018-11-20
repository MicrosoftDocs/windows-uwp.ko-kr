---
author: drewbatgit
ms.assetid: dd2a1e01-c284-4d62-963e-f59f58dca61a
description: 이 문서에서는 사용 가능한 미디어 원본 검색, 사진 및 사이드카 파일과 같은 파일 가져오기, 원본 디바이스에서 가져온 파일 삭제 등 디바이스에서 미디어를 가져오는 방법을 설명합니다.
title: 미디어 가져오기
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 36f8956e2ca167d98bb8e6ecf35ce3d131d7b032
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7291833"
---
# <a name="import-media-from-a-device"></a>디바이스에서 미디어 가져오기

이 문서에서는 사용 가능한 미디어 원본 검색, 비디오, 사진 및 사이드카 파일과 같은 파일 가져오기, 원본 디바이스에서 가져온 파일 삭제 등 디바이스에서 미디어를 가져오는 방법을 설명합니다.

> [!NOTE] 
> 이 문서의 코드는 [**MediaImport UWP 앱 샘플**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport)에서 조정되었습니다. [**유니버설 Windows 앱 샘플 Git 리포지토리**](https://github.com/Microsoft/Windows-universal-samples)에서 이 샘플을 복제하거나 다운로드하여 컨텍스트에서 코드를 확인하거나 고유한 앱의 시작점으로 사용할 수 있습니다.

## <a name="create-a-simple-media-import-ui"></a>간단한 미디어 가져오기 UI 만들기
이 문서의 예제에서는 최소한의 UI를 사용하여 핵심 미디어 가져오기 시나리오를 가능하게 합니다. 미디어 가져오기 앱에 사용할 보다 강력한 UI를 만드는 방법은 [**MediaImport 샘플**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport)을 참조하세요. 아래 XAML은 다음과 같은 컨트롤이 있는 스택 패널을 만듭니다.
* [**Button**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Button) - 미디어를 가져올 수 있는 원본 검색을 시작합니다.
* [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ComboBox) - 발견된 미디어 가져오기 원본을 표시하고 선택합니다.
* [**ListView**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ListView) 컨트롤 - 선택한 가져오기 원본의 미디어 항목을 표시하고 선택합니다.
* **Button** - 선택한 원본에서 미디어 항목 가져오기를 시작합니다.
* **Button** - 선택한 원본에서 가져온 항목 삭제를 시작합니다.
* **Button** - 비동기 미디어 가져오기 작업을 취소합니다.

[!code-xml[ImportXAML](./code/PhotoImport_Win10/cs/MainPage.xaml#SnippetImportXAML)]

## <a name="set-up-your-code-behind-file"></a>코드 숨김 파일 설정
*using* 지시문을 추가하여 이 예제에서 사용되고 기본 프로젝트 템플릿에 포함되지 않은 네임스페이스를 포함합니다.

[!code-cs[Using](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetUsing)]

## <a name="set-up-task-cancellation-for-media-import-operations"></a>미디어 가져오기 작업에 대해 작업 취소 설정

미디어 가져오기 작업은 시간이 오래 걸릴 수 있으므로 [**IAsyncOperationWithProgress**](https://msdn.microsoft.com/library/windows/apps/br206594.aspx)를 사용하여 비동기적으로 수행됩니다. 사용자가 취소 단추를 클릭할 경우 진행 중인 작업을 취소하는 데 사용할 [**CancellationTokenSource**](https://msdn.microsoft.com/library/system.threading.cancellationtokensource) 형식의 클래스 멤버 변수를 선언합니다.

[!code-cs[DeclareCts](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareCts)]

취소 단추에 대한 처리기를 구현합니다. 이 문서의 뒷부분에 있는 예제에서는 작업이 시작될 때 **CancellationTokenSource**를 초기화하고, 작업이 완료되면 null로 설정합니다. 취소 단추 처리기에서 토큰이 null인지 확인하고, null이 아니면 [**Cancel**](https://msdn.microsoft.com/library/dd321955)을 호출하여 작업을 취소합니다.

[!code-cs[OnCancel](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetOnCancel)]

## <a name="data-binding-helper-classes"></a>데이터 바인딩 도우미 클래스

일반적인 미디어 가져오기 시나리오에서는 가져올 수 있는 미디어 항목 목록을 사용자에게 표시하고, 선택할 수 있는 많은 미디어 파일이 있을 수 있으며, 각 미디어 항목에 대한 미리 보기를 표시하는 것이 좋습니다. 따라서 이 예제에서는 세 가지 도우미 클래스를 사용하여 사용자가 목록을 아래로 스크롤할 때 ListView 컨트롤에 항목을 증분 방식으로 로드합니다.

* **IncrementalLoadingBase** 클래스 - [**IList**](https://msdn.microsoft.com/library/system.collections.ilist), [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.isupportincrementalloading) 및 [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/system.collections.specialized.inotifycollectionchanged(v=vs.105).aspx)를 구현하여 기본 증분 로드 동작을 제공합니다.
* **GeneratorIncrementalLoadingClass** 클래스 - 증분 로드 기본 클래스 구현을 제공합니다.
* **ImportableItemWrapper** 클래스 - 가져온 각 항목에 대한 미리 보기 이미지의 바인딩 가능한 [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.Imaging.BitmapImage) 속성을 추가하는 [**PhotoImportItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportItem) 클래스를 둘러싼 씬 래퍼입니다.

이러한 클래스는 [**MediaImport 샘플**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport)에서 제공되며 수정하지 않고 해당 프로젝트에 추가할 수 있습니다. 프로젝트에 도우미 클래스를 추가한 후 이 예제의 뒷부분에서 사용할 **GeneratorIncrementalLoadingClass** 형식의 클래스 멤버 변수를 선언합니다.

[!code-cs[GeneratorIncrementalLoadingClass](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetGeneratorIncrementalLoadingClass)]


## <a name="find-available-sources-from-which-media-can-be-imported"></a>미디어를 가져올 수 있는 사용 가능한 원본 찾기

원본 찾기 단추의 클릭 처리기에서 정적 메서드 [**PhotoImportManager.FindAllSourcesAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportManager.FindAllSourcesAsync)를 호출하여 미디어를 가져올 수 있는 디바이스에 대한 시스템 검색을 시작합니다. 작업이 완료될 때까지 기다린 후 반환된 목록에 있는 각 [**PhotoImportSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportSource) 개체를 반복하고, 사용자가 선택할 때 쉽게 검색할 수 있도록 원본 개체 자체에 **Tag** 속성을 설정하여 **ComboBox**에 항목을 추가합니다.

[!code-cs[FindSourcesClick](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetFindSourcesClick)]

사용자가 선택한 가져오기 원본을 저장할 클래스 멤버 변수를 선언합니다.

[!code-cs[DeclareImportSource](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareImportSource)]

가져오기 원본 **ComboBox**에 대한 [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.Selector.SelectionChanged) 처리기에서 클래스 멤버 변수를 선택한 원본으로 설정한 다음 이 문서의 뒷부분에서 보여 줄 **FindItems** 도우미 메서드를 호출합니다. 

[!code-cs[SourcesSelectionChanged](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetSourcesSelectionChanged)]

## <a name="find-items-to-import"></a>가져올 항목 찾기

다음 단계에서 사용할 [**PhotoImportSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportSession) 및 [**PhotoImportFindItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult) 형식의 클래스 멤버 변수를 추가합니다.

[!code-cs[DeclareImport](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareImport)]

필요한 경우 찾기 작업을 취소하는 데 사용할 수 있도록 **FindItems** 메서드에서 **CancellationTokenSource** 변수를 초기화합니다. **try** 블록 내에서 사용자가 선택한 [**PhotoImportSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportSource) 개체에 대해 [**CreateImportSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportSource.CreateImportSession)을 호출하여 새 가져오기 세션을 만듭니다. 새 [**Progress**](https://msdn.microsoft.com/library/hh193692.aspx) 개체를 만들어 찾기 작업의 진행률을 표시할 콜백을 제공합니다. 그런 다음, **[FindItemsAsync](https://docs.microsoft.com/uwp/api/windows.media.import.photoimportsession.finditemsasync)** 을 호출하여 찾기 작업을 시작합니다. [**PhotoImportContentTypeFilter**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportContentTypeFilter) 값을 제공하여 사진, 비디오 또는 둘 다를 반환할지 지정합니다. [**PhotoImportItemSelectionMode**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportItemSelectionMode) 값을 제공하여 모두 반환할지, 아무것도 반환하지 않을지 또는 해당 [**IsSelected**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportItem.IsSelected) 속성이 true로 설정된 새 미디어 항목만 반환할지를 지정합니다. 이 속성은 ListBox 항목 템플릿에 있는 각 미디어 항목의 확인란에 바인딩되어 있습니다.

**FindItemsAsync**는 [**IAsyncOperationWithProgress**](https://msdn.microsoft.com/library/windows/apps/br206594.aspx)를 반환합니다. 확장 메서드 [**AsTask**](https://msdn.microsoft.com/library/hh779750.aspx)는 대기할 수 있고, 취소 토큰을 사용하여 취소할 수 있으며, 지원되는 **Progress** 개체를 사용하여 진행률을 보고하는 작업을 만드는 데 사용됩니다.

데이터 바인딩 도우미 클래스 **GeneratorIncrementalLoadingClass**가 초기화됩니다. 대기 중에서 반환될 때 **FindItemsAsync**는 [**PhotoImportFindItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult) 개체를 반환합니다. 이 개체에는 작업 성공, 발견된 각 유형의 미디어 항목 수 등 찾기 작업에 대한 상태 정보가 포함되어 있습니다. [**FoundItems**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.FoundItems) 속성에는 찾은 미디어 항목을 나타내는 [**PhotoImportItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportItem) 개체 목록이 포함됩니다. **GeneratorIncrementalLoadingClass** 생성자는 증분 방식으로 로드되는 총 항목 수와 필요에 따라 로드할 새 항목을 생성하는 함수를 인수로 사용합니다. 이 경우 제공된 람다 식은 **PhotoImportItem**을 래핑하고 각 항목에 대한 미리 보기를 포함하는 **ImportableItemWrapper**의 새 인스턴스를 만듭니다. 증분 로드 클래스가 초기화되면 UI에 있는 **ListView** 컨트롤의 [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ItemsControl.ItemsSource) 속성으로 설정합니다. 이제 찾은 미디어 항목이 증분 방식으로 로드되고 목록에 표시됩니다.

찾기 작업의 상태 정보가 출력됩니다. 일반적인 앱은 UI에서 이 정보를 사용자에게 표시하지만 이 예제에서는 단순히 정보를 디버그 콘솔에 출력합니다. 마지막으로, 작업이 완료되었으므로 취소 토큰을 null로 설정합니다.

[!code-cs[FindItems](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetFindItems)]

## <a name="import-media-items"></a>미디어 항목 가져오기

가져오기 작업을 구현하기 전에 가져오기 작업 결과를 저장할 [**PhotoImportImportItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportImportItemsResult) 개체를 선언합니다. 이 개체는 나중에 원본에서 성공적으로 가져온 미디어 항목을 삭제하는 데 사용됩니다.

[!code-cs[DeclareImportResult](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareImportResult)]

미디어 가져오기 작업을 시작하기 전에 [**ProgressBar**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ProgressBar) 컨트롤의 값을 0으로 설정하여 **CancellationTokenSource** 변수를 초기화합니다.

**ListView** 컨트롤에서 선택한 항목이 없는 경우 가져올 항목이 없습니다. 항목을 선택한 경우 [**Progress**](https://msdn.microsoft.com/library/hh193692.aspx) 개체를 초기화하여 진행률 표시줄 컨트롤의 값을 업데이트하는 진행률 콜백을 제공합니다. 찾기 작업에서 반환된 [**PhotoImportFindItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult)의 [**ItemImported**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.ItemImported) 이벤트 처리기를 등록합니다. 이 이벤트는 항목을 가져올 때마다 발생하며, 이 예제에서는 가져온 각 파일의 이름을 디버그 콘솔에 출력합니다.

[**ImportItemsAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.ImportItemsAsync)를 호출하여 가져오기 작업을 시작합니다. 찾기 작업과 마찬가지로, [**AsTask**](https://msdn.microsoft.com/library/hh779750.aspx) 확장 메서드를 사용하여 반환된 작업을 대기할 수 있고 진행률을 보고하며 취소할 수 있는 작업으로 변환합니다.

가져오기 작업이 완료되면 [**ImportItemsAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.ImportItemsAsync)에서 반환된 [**PhotoImportImportItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportImportItemsResult) 개체를 통해 작업 상태를 얻을 수 있습니다. 이 예제에서는 상태 정보를 디버그 콘솔에 출력한 다음 마지막으로, 취소 토큰을 null로 설정합니다.

[!code-cs[ImportClick](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetImportClick)]

## <a name="delete-imported-items"></a>가져온 항목 삭제
가져온 원본에서 성공적으로 가져온 항목을 삭제하려면 먼저 삭제 작업을 취소할 수 있도록 취소 토큰을 초기화하고 진행률 표시줄 값을 0으로 설정합니다. [**ImportItemsAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.ImportItemsAsync)에서 반환된 [**PhotoImportImportItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportImportItemsResult)가 null이 아닌지 확인합니다. null이 아니면 다시 [**Progress**](https://msdn.microsoft.com/library/hh193692.aspx) 개체를 만들어 삭제 작업의 진행률 콜백을 제공합니다. [**DeleteImportedItemsFromSourceAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportImportItemsResult.DeleteImportedItemsFromSourceAsync)를 호출하여 가져온 항목 삭제를 시작합니다. **AsTask**를 사용하여 결과를 진행률 및 취소 기능이 있는 대기 가능한 작업으로 변환합니다. 대기한 후 [**PhotoImportDeleteImportedItemsFromSourceResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportDeleteImportedItemsFromSourceResult) 개체를 사용하여 삭제 작업에 대한 상태 정보를 가져와 표시할 수 있습니다.

[!code-cs[DeleteClick](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeleteClick)]








 


