---
author: mcleblanc
ms.assetid: 3A477380-EAC5-44E7-8E0F-18346CC0C92F
title: "ListView 및 GridView 데이터 가상화"
description: "데이터 가상화를 통해 ListView 및 GridView 성능과 시작 시간이 향상됩니다."
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 0b1dfaeb098ac4b73c89f4d1a51ec658312aee4e
ms.lasthandoff: 02/07/2017

---
# <a name="listview-and-gridview-data-virtualization"></a>ListView 및 GridView 데이터 가상화

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

**참고**  자세한 내용은 //build/ 세션 [사용자가 GridView 및 ListView에서 많은 데이터를 조작할 때의 획기적인 성능 향상](https://channel9.msdn.com/Events/Build/2013/3-158)을 참조하세요.

데이터 가상화를 통해 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 및 [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) 성능과 시작 시간이 향상됩니다. UI 가상화, 요소 감소 및 항목의 점진적인 업데이트를 보려면 [ListView 및 GridView UI 최적화](optimize-gridview-and-listview.md)를 참조하세요.

데이터 가상화 방법은 너무 커서 메모리에 한 번에 저장할 수 없거나 저장해서는 안 되는 데이터 집합에 필요합니다. 초기 부분을 메모리에 로드(로컬 디스크, 네트워크 또는 클라우드)하고 이 부분 데이터 집합에 UI 가상화를 적용합니다. 나중에 데이터를 증분식으로 로드하거나 필요에 따라 마스터 데이터 집합의 임의 지점(임의 액세스)에서 로드할 수 있습니다. 데이터 가상화가 적합한지 여부는 여러 요인에 따라 달라집니다.

-   데이터 집합의 크기
-   각 항목의 크기
-   데이터 집합의 소스(로컬 디스크, 네트워크 또는 클라우드)
-   앱의 전체 메모리 소비

**참고**  ListView 및 GridView에서 사용자가 빠르게 이동/스크롤하는 동안 일시적으로 자리 표시자 화면 효과를 표시하는 기능이 기본적으로 사용되도록 설정되어 있는지 확인합니다. 데이터가 로드되면 이러한 자리 표시자 화면 효과가 항목 템플릿으로 바뀝니다. [**ListViewBase.ShowsScrollingPlaceholders**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.showsscrollingplaceholders)를 false로 설정하여 이 기능을 끌 수 있지만 이렇게 하는 경우 x:Phase 특성을 사용하여 항목 템플릿의 요소를 점진적으로 렌더링하는 것이 좋습니다. [점진적으로 ListView 및 GridView 항목 업데이트](optimize-gridview-and-listview.md#update-items-incrementally)를 참조하세요.

증분 및 임의 액세스 데이터 가상화 기술에 대한 자세한 내용은 다음과 같습니다.

## <a name="incremental-data-virtualization"></a>증분 데이터 가상화

증분 데이터 가상화에서는 데이터가 순차적으로 로드됩니다. 증분 데이터 가상화를 사용하는 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878)는 100만 항목의 컬렉션을 보는 데 사용될 수 있지만 초기에는 50개의 항목만 로드됩니다. 사용자가 이동/스크롤하면 다음 50개가 로드됩니다. 항목이 로드됨에 따라 스크롤 막대의 위치 조정 컨트롤의 크기가 줄어듭니다. 이 데이터 가상화 유형의 경우 이러한 인터페이스를 구현하는 데이터 원본 클래스를 작성합니다.

-   [**IList**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.ilist.aspx)
-   [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx)(C#/VB) 또는 [**IObservableVector&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/BR226052)(C++/CX)
-   [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916)

이와 같은 데이터 원본은 지속적으로 확장될 수 있는 메모리 내 목록입니다. 항목 컨트롤은 표준 [**IList**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.ilist.aspx) 인덱서 및 개수 속성을 사용하는 항목을 요청합니다. 개수는 데이터 집합의 실제 크기가 아니라 항목 수를 로컬로 나타내야 합니다.

항목 컨트롤이 기존 데이터의 끝에 가까워지면 [**ISupportIncrementalLoading.HasMoreItems**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.isupportincrementalloading.hasmoreitems)를 호출합니다. **true**를 반환하면 로드할 권장 항목 수를 전달하는 [**ISupportIncrementalLoading.LoadMoreItemsAsync**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.isupportincrementalloading.loadmoreitemsasync)를 호출합니다. 데이터를 로드하는 위치(로컬 디스크, 네트워크 또는 클라우드)에 따라 권장 개수와 다른 개수의 항목을 로드하도록 선택할 수 있습니다. 예를 들어 서비스에서 50개 항목의 일괄 처리를 지원하지만 항목 컨트롤에서 10개를 요청하는 경우 50개를 로드할 수 있습니다. 백 엔드에서 데이터를 로드하고 목록에 추가한 후 항목 컨트롤이 새 항목을 인식하도록 [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx) 또는 [**IObservableVector&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/BR226052)를 통해 변경 알림을 발생시킵니다. 또한 실제로 로드한 항목 수를 반환합니다. 권장 개수보다 적은 항목을 로드하거나 항목 컨트롤이 중간에 훨씬 멀리 이동/스크롤된 경우 추가 항목에 대해 데이터 원본가 다시 호출되고 주기가 계속됩니다. Windows 8.1용 [XAML 데이터 바인딩 샘플](https://code.msdn.microsoft.com/windowsapps/Data-Binding-7b1d67b5)을 다운로드하고 Windows 10 앱에서 해당 소스 코드를 다시 사용하여 자세히 알아볼 수 있습니다.

## <a name="random-access-data-virtualization"></a>임의 액세스 데이터 가상화

임의 액세스 데이터 가상화에서는 데이터 집합의 임의 지점에서 데이터를 로드할 수 있습니다. 100만 개의 항목을 보는 데 사용되는 임의 액세스 데이터 가상화를 사용하는 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878)는 100,000~100,050개의 항목을 로드할 수 있습니다. 사용자가 목록의 시작 부분으로 이동하면 컨트롤이 1~50개의 항목을 로드합니다. 스크롤 막대의 위치 조정 컨트롤은 항상 **ListView**에 100만 개의 항목이 포함되어 있음을 나타냅니다. 스크롤 막대의 위치 조정 컨트롤 위치는 컬렉션의 전체 데이터 집합에서 표시된 항목이 있는 위치를 기준으로 합니다. 이 유형의 데이터 가상화는 메모리 요구 사항 및 컬렉션 로드 시간을 크게 줄일 수 있습니다. 이를 지원하려면 필요에 따라 데이터를 가져오고 로컬 캐시를 관리하며 이러한 인터페이스를 구현하는 데이터 원본 클래스를 작성해야 합니다.

-   [**IList**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.ilist.aspx)
-   [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx)(C#/VB) 또는 [**IObservableVector&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/BR226052)(C++/CX)
-   (선택적으로) [**IItemsRangeInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877070)
-   (선택적으로) [**ISelectionInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877074)

[**IItemsRangeInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877070)는 컨트롤에서 자주 사용되는 항목에 대한 정보를 제공합니다. 항목 컨트롤은 해당 보기가 변경될 때마다 이 메서드를 호출하며 다음 두 가지 범위 집합을 포함합니다.

-   뷰포트에 있는 항목 집합
-   컨트롤에서 사용하지만 뷰포트에 없을 수 있는 가상화되지 않은 항목 집합
    -   터치 이동이 원활하도록 항목 컨트롤에서 유지하는 뷰포트 주위의 항목 버퍼
    -   포커스가 있는 항목
    -   첫 번째 항목

[**IItemsRangeInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877070)를 구현하면 데이터 원본가 가져오고 캐시해야 하는 항목 및 더 이상 필요 없는 캐시 데이터를 정리할 시점을 인식합니다. **IItemsRangeInfo**에서는 [**ItemIndexRange**](https://msdn.microsoft.com/library/windows/apps/Dn877081) 개체를 사용하여 컬렉션 내 해당 인덱스를 기반으로 항목 집합을 설명합니다. 이는 올바르거나 안정적이지 않을 수 있는 항목 포인터를 사용하지 않도록 하기 위한 것입니다. **IItemsRangeInfo**는 항목 컨트롤에 대한 상태 정보를 기반으로 하기 때문에 해당 항목 컨트롤의 단일 인스턴스에서만 사용되도록 설계되었습니다. 여러 항목 컨트롤이 동일한 데이터에 액세스해야 하는 경우 각각에 대해 별도의 데이터 원본 인스턴스가 필요합니다. 여러 항목 컨트롤은 공용 캐시를 공유할 수 있지만 캐시에서 정리하는 논리가 보다 복잡합니다.

임의 액세스 데이터 가상화 데이터 원본에 대한 기본 전략은 다음과 같습니다.

-   항목이 요청될 때
    -   메모리에 사용할 수 있는 경우 해당 항목을 반환합니다.
    -   해당 항목이 없는 경우 null 또는 자리 표시자 항목을 반환합니다.
    -   필요한 항목을 알고 해당 항목에 대한 데이터를 백 엔드에서 비동기식으로 가져오려면 항목에 대한 요청(또는 [**IItemsRangeInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877070)의 범위 정보)을 사용합니다. 데이터를 검색한 후 항목 컨트롤이 새 항목을 인식하도록 [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx) 또는 [**IObservableVector&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/BR226052)를 통해 변경 알림을 발생시킵니다.
-   (옵션) 항목 컨트롤의 뷰포트가 변경되면 [**IItemsRangeInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877070)의 구현을 통해 데이터 원본에서 필요한 항목을 식별합니다.

이러한 전략 외에 데이터 항목을 로드할 시점, 로드할 개수 및 메모리에서 유지할 항목에 대한 전략은 응용 프로그램에 달려 있습니다. 몇 가지 일반적인 고려 사항은 다음과 같습니다.

-   데이터에 대한 비동기 요청을 만듭니다. UI 스레드를 차단하지 마세요.
-   항목을 가져오는 일괄 처리의 적정한 크기를 확인합니다. chatty보다 chunky를 기본적으로 사용합니다. 작은 요청이 너무 많이 만들어질 정도로 작게 설정하지 말고, 검색하는 너무 오래 걸릴 정도로 크게 설정하지 마세요.
-   동시에 보류할 요청 수를 고려합니다. 한 번에 하나씩 수행하는 것이 더 간편하지만 소요 시간이 긴 경우 너무 느릴 수 있습니다.
-   데이터 요청을 취소할 수 있나요?
-   호스팅 서비스를 사용하는 경우 트랜잭션당 비용이 있나요?
-   쿼리 결과가 변경된 경우 서비스에서 어떤 종류의 알림이 제공되나요? 항목이 인덱스 33에 삽입된 경우 이를 알 수 있나요? 서비스가 key-plus-offset 기반의 쿼리를 지원하는 경우 인덱스만 사용하는 것보다 더 나을 수 있습니다.
-   항목 사전 가져오기가 어느 정도 지능적으로 수행되기를 원하시나요? 스크롤 방향 및 속도를 추적하여 필요한 항목을 예측하려고 하시나요?
-   캐시 지우기가 어느 정도 적극적이기를 원하시나요? 메모리와 환경 간에 상충 관계가 있습니다.





