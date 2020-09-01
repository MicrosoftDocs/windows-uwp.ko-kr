---
ms.assetid: dd2a1e01-c284-4d62-963e-f59f58dca61a
description: 이 문서에서는 사용 가능한 미디어 원본 검색, 사진 및 사이드카 파일 등의 파일 가져오기, 원본 장치에서 가져온 파일 삭제를 비롯 하 여 장치에서 미디어를 가져오는 방법을 설명 합니다.
title: 미디어 가져오기
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 71743459227b05fff23524a81d8d192c382d4973
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157437"
---
# <a name="import-media-from-a-device"></a>디바이스에서 미디어 가져오기

이 문서에서는 사용 가능한 미디어 원본 검색, 비디오, 사진 및 사이드카 파일 등의 파일 가져오기, 원본 장치에서 가져온 파일 삭제를 비롯 하 여 장치에서 미디어를 가져오는 방법을 설명 합니다.

> [!NOTE] 
> 이 문서의 코드는 [**Mediaimport UWP 앱 샘플**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport) 에서 도입 되었습니다. [**유니버설 Windows 앱 샘플 Git**](https://github.com/Microsoft/Windows-universal-samples) 리포지토리에서이 샘플을 복제 하거나 다운로드 하 여 컨텍스트에서 코드를 확인 하거나 사용자 고유의 앱에 대 한 시작 점으로 사용할 수 있습니다.

## <a name="create-a-simple-media-import-ui"></a>간단한 미디어 가져오기 UI 만들기
이 문서의 예제에서는 최소 UI를 사용 하 여 핵심 미디어 가져오기 시나리오를 사용 하도록 설정 합니다. 미디어 가져오기 앱에 대 한 보다 강력한 UI를 만드는 방법을 보려면 [**Mediaimport 샘플**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport)을 참조 하세요. 다음 XAML은 다음 컨트롤을 사용 하 여 스택 패널을 만듭니다.
* 미디어를 가져올 수 있는 원본에 대 한 검색을 시작 하는 [**단추**](/uwp/api/Windows.UI.Xaml.Controls.Button) 입니다.
* 검색 된 미디어 가져오기 원본에서 나열 하 고 선택 하는 [**콤보 상자**](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) 입니다.
* 선택한 가져오기 원본의 미디어 항목을 표시 하 고 선택할 [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) 컨트롤입니다.
* 선택한 소스에서 미디어 항목 가져오기를 시작 하는 **단추** 입니다.
* 선택한 소스에서 가져온 항목 삭제를 시작 하는 **단추** 입니다.
* 비동기 미디어 가져오기 작업을 취소 하는 **단추** 입니다.

[!code-xml[ImportXAML](./code/PhotoImport_Win10/cs/MainPage.xaml#SnippetImportXAML)]

## <a name="set-up-your-code-behind-file"></a>코드 숨김이 파일 설정
*Using* 지시문을 추가 하 여이 예제에서 사용 하는 네임 스페이스 중 기본 프로젝트 템플릿에 포함 되지 않은 네임 스페이스를 포함 합니다.

[!code-cs[Using](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetUsing)]

## <a name="set-up-task-cancellation-for-media-import-operations"></a>미디어 가져오기 작업을 위한 작업 취소 설정

미디어 가져오기 작업은 시간이 오래 걸릴 수 있으므로 [**IAsyncOperationWithProgress**](/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_)를 사용 하 여 비동기적으로 수행 됩니다. 사용자가 취소 단추를 클릭 하는 경우 진행 중인 작업을 취소 하는 데 사용 되는 [**CancellationTokenSource**](/dotnet/api/system.threading.cancellationtokensource) 형식의 클래스 멤버 변수를 선언 합니다.

[!code-cs[DeclareCts](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareCts)]

취소 단추에 대 한 처리기를 구현 합니다. 이 문서의 뒷부분에 있는 예제에서는 작업이 시작될 때 **CancellationTokenSource**를 초기화하고, 작업이 완료되면 null로 설정합니다. 취소 단추 처리기에서 토큰이 null 인지 확인 하 고, 그렇지 않은 경우 [**취소**](/dotnet/api/system.threading.cancellationtokensource.cancel#System_Threading_CancellationTokenSource_Cancel) 를 호출 하 여 작업을 취소 합니다.

[!code-cs[OnCancel](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetOnCancel)]

## <a name="data-binding-helper-classes"></a>데이터 바인딩 도우미 클래스

일반적인 미디어 가져오기 시나리오에서는 사용자에 게 가져올 수 있는 미디어 항목의 목록을 표시 합니다. 많은 미디어 파일을 선택할 수 있으며, 일반적으로 각 미디어 항목의 미리 보기를 표시 하려고 합니다. 이러한 이유로,이 예제에서는 사용자가 목록에서 아래로 스크롤할 때 세 개의 도우미 클래스를 사용 하 여 항목을 ListView 컨트롤로 증분 로드 합니다.

* **IncrementalLoadingBase** 클래스- [**IList**](/dotnet/api/system.collections.ilist), [**은 isupportincrementalloading**](/uwp/api/windows.ui.xaml.data.isupportincrementalloading)및 [**INotifyCollectionChanged**](/dotnet/api/system.collections.specialized.inotifycollectionchanged) 를 구현 하 여 기본 증분 로드 동작을 제공 합니다.
* **GeneratorIncrementalLoadingClass** 클래스-증분 로드 기본 클래스의 구현을 제공 합니다.
* **ImportableItemWrapper** 클래스-가져온 각 항목의 미리 보기 이미지에 대 한 바인딩 가능한 [**BitmapImage**](/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage) 속성을 추가 하는 [**사진 importitem**](/uwp/api/Windows.Media.Import.PhotoImportItem) 클래스 주위의 씬 래퍼입니다.

이러한 클래스는 [**Mediaimport 샘플**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport) 에서 제공 되며 수정 하지 않고 프로젝트에 추가할 수 있습니다. 도우미 클래스를 프로젝트에 추가한 후이 예제의 뒷부분에서 사용할 **GeneratorIncrementalLoadingClass** 형식의 클래스 멤버 변수를 선언 합니다.

[!code-cs[GeneratorIncrementalLoadingClass](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetGeneratorIncrementalLoadingClass)]


## <a name="find-available-sources-from-which-media-can-be-imported"></a>미디어를 가져올 수 있는 사용 가능한 원본 찾기

소스 찾기 단추에 대 한 클릭 처리기에서 정적 메서드 [**FindAllSourcesAsync**](/uwp/api/windows.media.import.photoimportmanager.findallsourcesasync) 를 호출 하 여 미디어를 가져올 수 있는 장치에 대 한 시스템 검색을 시작 합니다. 작업이 완료 될 때까지 대기 하 고 나 서 반환 된 목록에서 각 [**사진 Importsource**](/uwp/api/Windows.Media.Import.PhotoImportSource) 개체를 반복 하 고 **콤보 상자**에 항목을 추가 합니다. **Tag** 속성을 원본 개체 자체에 설정 하 여 사용자가 선택할 때 쉽게 검색할 수 있습니다.

[!code-cs[FindSourcesClick](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetFindSourcesClick)]

사용자가 선택한 가져오기 소스를 저장 하도록 클래스 멤버 변수를 선언 합니다.

[!code-cs[DeclareImportSource](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareImportSource)]

가져오기 원본 **콤보 상자의** [**selectionchanged**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) 처리기에서 클래스 멤버 변수를 선택한 원본으로 설정한 다음이 문서의 뒷부분에 표시 되는 **finditems** 도우미 메서드를 호출 합니다. 

[!code-cs[SourcesSelectionChanged](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetSourcesSelectionChanged)]

## <a name="find-items-to-import"></a>가져올 항목 찾기

다음 단계에서 사용 되는 [**사진 Importsession**](/uwp/api/Windows.Media.Import.PhotoImportSession) 및 [**사진 Importfindvariables 결과**](/uwp/api/Windows.Media.Import.PhotoImportFindItemsResult) 형식의 클래스 멤버 변수를 추가 합니다.

[!code-cs[DeclareImport](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareImport)]

**Finditems** 메서드에서 필요한 경우 찾기 작업을 취소 하는 데 사용할 수 있도록 **CancellationTokenSource** 변수를 초기화 합니다. **Try** 블록 내에서 사용자가 선택한 [**사진 importsource**](/uwp/api/Windows.Media.Import.PhotoImportSource) 개체에 대해 [**createimportsession**](/uwp/api/windows.media.import.photoimportsource.createimportsession) 을 호출 하 여 새 가져오기 세션을 만듭니다. 새 [**진행률**](/dotnet/api/system.progress-1) 개체를 만들어 찾기 작업의 진행률을 표시 하는 콜백을 제공 합니다. 그런 다음, **[Findnode.js async](/uwp/api/windows.media.import.photoimportsession.finditemsasync)** 를 호출 하 여 찾기 작업을 시작 합니다. 사진, 비디오 또는 둘 다를 반환할지를 지정 하는 [**PhotoImportContentTypeFilter**](/uwp/api/Windows.Media.Import.PhotoImportContentTypeFilter) 값을 제공 합니다. [**IsSelected**](/uwp/api/windows.media.import.photoimportitem.isselected) 속성이 true로 설정 된 상태에서 새 미디어 항목만 반환 되는지 여부를 지정 하려면 [**PhotoImportItemSelectionMode**](/uwp/api/Windows.Media.Import.PhotoImportItemSelectionMode) 값을 제공 합니다. 이 속성은 ListBox 항목 템플릿의 각 미디어 항목에 대 한 확인란에 바인딩됩니다.

**FindIAsyncOperationWithProgress async** 는이 [**IAsyncOperationWithProgress**](/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_)를 반환 합니다. 확장 [**메서드 (대기)는 취소**](/dotnet/api/system) 토큰을 사용 하 여 취소할 수 있고 제공 된 **진행률** 개체를 사용 하 여 진행률을 보고 하는 작업을 만드는 데 사용 됩니다.

그런 다음 **GeneratorIncrementalLoadingClass** 데이터 바인딩 도우미 클래스를 초기화 합니다. **Find대기**에서 반환 될 때의 Find는 [**사진 Importfind의 결과**](/uwp/api/Windows.Media.Import.PhotoImportFindItemsResult) 개체를 반환 합니다. 이 개체에는 작업의 성공 여부와 검색 된 미디어 항목 형식의 수를 포함 하 여 찾기 작업에 대 한 상태 정보가 포함 됩니다. [**FoundItems**](/uwp/api/windows.media.import.photoimportfinditemsresult.founditems) 속성에는 찾은 미디어 항목을 나타내는 [**사진 importitem**](/uwp/api/Windows.Media.Import.PhotoImportItem) 개체의 목록이 포함 되어 있습니다. **GeneratorIncrementalLoadingClass** 생성자는 증분 로드 되는 항목의 총 수를 인수로 사용 하 고 필요에 따라 로드할 새 항목을 생성 하는 함수를 사용 합니다. 이 경우 제공 된 람다 식은 **ImportableItemWrapper** 의 새 인스턴스를 만듭니다 .이 인스턴스는 **사진 importitem** 을 래핑하고 각 항목에 대 한 미리 보기를 포함 합니다. 증분 로드 클래스가 초기화 되 면 UI에서 **ListView** 컨트롤의 [**system.windows.controls.itemscontrol.itemssource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 속성으로 설정 합니다. 이제 찾은 미디어 항목이 증분 방식으로 로드 되 고 목록에 표시 됩니다.

그런 다음 찾기 작업의 상태 정보가 출력 됩니다. 일반적인 앱은 UI에서이 정보를 사용자에 게 표시 하지만이 예제에서는 단순히 정보를 디버그 콘솔로 출력 합니다. 마지막으로 작업이 완료 되었으므로 취소 토큰을 null로 설정 합니다.

[!code-cs[FindItems](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetFindItems)]

## <a name="import-media-items"></a>미디어 항목 가져오기

가져오기 작업을 구현 하기 전에 [**PhotoImportImportItemsResult**](/uwp/api/Windows.Media.Import.PhotoImportImportItemsResult) 개체를 선언 하 여 가져오기 작업의 결과를 저장 합니다. 이는 나중에 원본에서 성공적으로 가져온 미디어 항목을 삭제 하는 데 사용 됩니다.

[!code-cs[DeclareImportResult](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareImportResult)]

미디어 가져오기 작업을 시작 하기 전에 **CancellationTokenSource** 변수를 초기화 하 고 [**ProgressBar**](/uwp/api/Windows.UI.Xaml.Controls.ProgressBar) 컨트롤의 값을 0으로 설정 합니다.

**ListView** 컨트롤에 선택 된 항목이 없으면 가져올 항목이 없습니다. 그렇지 않으면 진행률 개체를 초기화 하 여 진행률 표시줄 컨트롤의 값을 업데이트 하 [**는 진행률 콜백을**](/dotnet/api/system.progress-1) 제공 합니다. 찾기 작업에서 반환 하는 [**사진 Importfind된 결과**](/uwp/api/Windows.Media.Import.PhotoImportFindItemsResult) 의 [**itemimported**](/uwp/api/windows.media.import.photoimportfinditemsresult.itemimported) 이벤트에 대 한 처리기를 등록 합니다. 이 이벤트는 항목을 가져올 때마다 발생 하며,이 예제에서는 가져온 각 파일의 이름을 디버그 콘솔로 출력 합니다.

[**ImportItemsAsync**](/uwp/api/windows.media.import.photoimportfinditemsresult.importitemsasync) 를 호출 하 여 가져오기 작업을 시작 합니다. 찾기 작업의 경우와 마찬가지로 대기 수 있는 작업으로 반환 된 작업을 변환 하 고 진행률을 보고 하 고 취소할 수 있는 작업으로 반환 된 작업을 변환 [**하는 데**](/dotnet/api/system) 사용 됩니다.

가져오기 작업이 완료 되 면 [**ImportItemsAsync**](/uwp/api/windows.media.import.photoimportfinditemsresult.importitemsasync)에서 반환 된 [**PhotoImportImportItemsResult**](/uwp/api/Windows.Media.Import.PhotoImportImportItemsResult) 개체에서 작업 상태를 가져올 수 있습니다. 이 예제에서는 상태 정보를 디버그 콘솔에 출력 한 다음, 취소 토큰을 null로 설정 합니다.

[!code-cs[ImportClick](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetImportClick)]

## <a name="delete-imported-items"></a>가져온 항목 삭제
성공적으로 가져온 항목을 가져온 원본에서 삭제 하려면 먼저 취소 토큰을 초기화 하 여 삭제 작업을 취소할 수 있도록 하 고 진행률 표시줄 값을 0으로 설정 합니다. [**ImportItemsAsync**](/uwp/api/windows.media.import.photoimportfinditemsresult.importitemsasync) 에서 반환 된 [**PhotoImportImportItemsResult**](/uwp/api/Windows.Media.Import.PhotoImportImportItemsResult) 가 null이 아닌지 확인 합니다. 그렇지 않으면 다시 한 번 [**진행률**](/dotnet/api/system.progress-1) 개체를 만들어 삭제 작업에 대 한 진행률 콜백을 제공 합니다. [**DeleteImportedItemsFromSourceAsync**](/uwp/api/windows.media.import.photoimportimportitemsresult.deleteimporteditemsfromsourceasync) 를 호출 하 여 가져온 항목 삭제를 시작 합니다. 결과를 진행률 및 취소 기능을 포함 하는 대기 가능 작업으로 변환 하도록 **요청** 합니다. 대기 후 반환 된 [**PhotoImportDeleteImportedItemsFromSourceResult**](/uwp/api/Windows.Media.Import.PhotoImportDeleteImportedItemsFromSourceResult) 개체를 사용 하 여 삭제 작업에 대 한 상태 정보를 가져오고 표시할 수 있습니다.

[!code-cs[DeleteClick](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeleteClick)]








