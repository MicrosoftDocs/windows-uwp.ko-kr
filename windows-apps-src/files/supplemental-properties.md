---
title: 보조 속성을 사용 하 여
description: 추가 속성 및 세부 정보를 사용 하 여 Windows에서 구현 된 방법에 대 한 소개
ms.date: 01/10/2017
ms.topic: article
keywords: windows 10, uwp, WinRT API 인덱서 검색
localizationpriority: medium
ms.openlocfilehash: 2a77bfc37d853efd28bde9bc3043d072888822f2
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369261"
---
# <a name="using-supplemental-properties"></a>보조 속성을 사용 하 여  

## <a name="summary"></a>요약  
- 추가 속성 파일을 변경 하지 않고 속성을 사용 하 여 태그 파일에 대 한 앱 허용 
- 속성을 계산 하기 어려운 있는 경우 또는 파일을 수정할 수 없습니다 하는 데 유용 합니다. 
- 보조 속성을 사용 하 여 동일 Windows 속성 시스템에서 다른 속성을 사용 하 여  

## <a name="introduction"></a>소개 
지난 몇 년에 흥미로운 새로운 앱의 여러 유용한 속성을 만든 날짜와 같은 기본 작업 이외의 파일에서 추출 되는 사용자 파일에 CPU 집약적인 작업을 실행 해야 합니다. 이러한 앱에는 함께 이미지 그룹 문서에 전자 메일 및 텍스트 분석에 의도 추출에서에서 인식을 개체 수행 합니다. 얼마나 진행 되 고는이 강력한 컴퓨팅은 이제 대부분의 소비자 Pc에서 사용할 수 있습니다.   

사용자가 생산성을 기하급수적으로이 메타 데이터를 즉시 검색할 수 있도록 허용 합니다. 단순히 딸이 프로그램은 그림을 파악 하는 것은 흥미로운 이지만 그림 그녀의 자신의 할머니를 사용 하 여 검색할 수 있는 훨씬 더 유용 합니다. 더 많은 개인 및 더 활성 컴퓨터 느낌을 사용 하는 환경을 만듭니다. 같은 컴퓨터에서 누군가 전 아끼는 메모리를 찾을 수 있도록 합니다. 

에서는 지난 수십 년간 Windows에 빠른 검색에 대 한 솔루션, 인덱서가 되었으며 크리에이터 스 업데이트에서 업데이트 된 이러한 새로운 시나리오를 지원 하기 위해. 앱은 이제 시스템에서 추출 되는 것 이상의 추가 속성을 사용 하 여 태그 파일 수 있습니다. 이러한 속성은 첫 번째 클래스 시민으로 처리 됩니다.  

## <a name="windows-properties"></a>Windows 속성 
합니다 [Windows 속성 시스템](https://docs.microsoft.com/windows/desktop/properties/windows-properties-system) 오랫동안 파일을 사용 하 여 상호 작용의 핵심 부분 되었습니다. 앱을에서 모든 다른 파일 형식 또는 파일에 있을 수 있습니다 하는 언어의 내부 구조를 이해 하지 않고도 파일에서 속성을 읽을 수 있습니다. 모든 추상화를 개발자로 서는 수행 해야 하는 모든 목록을 요청 하 고 오름차순 또는 내림차순을 지정 합니다.  

Windows 인덱서를 사용 하 여 속성 시스템 밀접 – 속성을 해당 범위 내에서 파일에서 읽고 저장 합니다. 나중에 때 앱 요청 목록을 수정 된 날짜를 정렬할 수 폴더의 모든.docx 인덱서 John Smith가 작성 제외 하 고 반환할 수 있습니다 목록 즉시.  

이러한 시스템이 함께 작동 하는 방법에 단점은 모든 속성을 요구 하는 데는 인덱서는 저장 즉시 사용 가능 하도록 파일에 대 한 것입니다. 이 해당 계산 어려움 요구 사항 이므로 하는 데 걸리는 흥미로운 속성에 대해 알고 있으면 제한.  

속성을 사용 하 여 하지만 쉽습니다, 그리고 앱 수는 데이터베이스 작업을 매우 유사 하 게 파일에 대 한 속성의 정렬 된 집합을 요청 하거나 또는 검색 엔진을 사용 하 여 같은 쿼리를 전달할 수 있습니다. 인덱서는 쿼리를 처리 하 고 결과 반환 합니다. 이 개발자에 게 제공 (예를 들어만 검색 jpg 파일) 해당 필터를 결합할 수 사용자의 쿼리 (파일 이름 "bird"로 시작)를 사용 하 여 합니다. 

## <a name="supplemental-properties"></a>추가 속성 

추가 속성 매우 중요 한 차이점을 사용 하 여 일반 Windows 속성에 동일 하 게 작동 – 인덱서에 파일이 추가 되 면 쓸 하지. 나중에 시스템에서 다른 앱에서 추가 속성을 추가 해야 합니다. 2 분 나중에 한 번 개체 인식 완료 되거나 일 이후 때문일 수 있습니다. 

속성은 작성 되 면 검색, 필터링, 정렬 하거나 이동할 수 있습니다 시스템에서 다른 속성과 마찬가지로 그룹화 합니다. 도 사용할 수는 시스템의 다른 속성을 사용 하 여 결합 된 쿼리에서 하거나 추가 여부. 이 다시 작성 하지 않고도 쉽게 기존 파일 시스템 코드를 사용 하 여 추가 속성을 결합 하는 데 유연성을 제공 합니다.  

### <a name="example-scenarios"></a>예제 시나리오 

수천 개의 다른 속성이 추가 속성을 쓸 수 있지만이 자습서를 계속 다시 하는 주요 시나리오의 두 가지가 있습니다.  

#### <a name="tagging-pictures-with-extracted-properties"></a>추출 된 속성을 사용 하 여 사진 태그 지정 
이러한 앱 이미지에서 개체와 같은 시스템 알지는 이미지에서 기능 추출에 ML 학습 된 모델을 사용할 수 있습니다. 다음 이미지에서를 식별 하는 개체를 사용 하 고 나중에 검색 또는 그룹화에 대 한 속성 시스템에 추가할 수 있습니다 것.  

#### <a name="tagging-files-with-an-app-specific-id"></a>앱 특정 ID 사용 하 여 파일에 태그 지정 
많은 파일 동기화 앱으로 서버와 다양 한 클라이언트 장치 간에 이동 하면서 파일을 추적 하는 자체 고유 ID를 사용 합니다. 동기화 클라이언트 파일 영향 없이 속성 시스템에이 ID를 작성할 수 있습니다. 이 ID는 이제 빠르게 액세스 하기 위해 나중에 앱에 사용 가능 하 고 동기화 공급자와 통신 하는 경우 읽기가 시스템에서 다른 앱에서 사용할 수 있습니다. 

보조 속성을 사용 하 여에 대 한 다른 많은 옵션도 있습니다 하지만 두 가지 모두 빠른 조회 또는 검색 해야 하므로 유용한 예제를 확인, 정보 시스템, 알지 및 파일 자체에 추가할 수 없습니다.  

### <a name="using-supplemental-properties"></a>보조 속성을 사용 하 여 
보조 속성을 사용 하 여 파일 시스템에는 일반 속성을 쓰는와 같습니다. StorageFiles 및 속성 사용에 익숙한 경우이 통해 건너뛸 수 있습니다. 그렇지 않으면 빠른 및 샘플을 파일에 단일 속성을 작성 한 다음 나중에 다시 동일한 속성을 읽는 살펴보겠습니다.  

### <a name="writing-supplemental-properties"></a>추가 속성 작성  
샘플에서는 편의상 발견 되는 첫 번째 파일 수정만 하지만 일반적으로 앱 속성 검색 되는 모든 파일에 추가 됩니다.  

```csharp
// Only indexed jpg files are going to be used 
QueryOptions option = new QueryOptions(CommonFileQuery.DefaultQuery, new string[] { ".jpg" }); 
option.IndexerOption = IndexerOption.OnlyUseIndexer; 
// Typically an app would loop over all the files in the library, updating them all with the new 
// value. To make the sample easier to understand however, this app is only going to update the  
// first file it finds 
var query = KnownFolders.PicturesLibrary.CreateFileQueryWithOptions(option); 
StorageFile file = (await query.GetFilesAsync()).FirstOrDefault(); 
if (file == null) 
{ 
    log("No jpg file found in the library. Stopping"); 
    return; 
} 
log("Found file: " + file.Path); 
// Selecting the property to modify and writing it out 
List<KeyValuePair<string, object>> props = new List<KeyValuePair<string, object>>();             
props.Add(new KeyValuePair<string, object>("System.Supplemental.ResourceId", fileId)); 
await file.Properties.SavePropertiesAsync(props); 
```

위치 속성을 쓰기 전에 인덱싱된 경우 중요 한 확인을 있습니다. 이 샘플에서는 위치를 인덱스를 필터링 하려면 쿼리 옵션을 사용 합니다. 이 불가능 하는 경우에 인덱싱된 상태의 부모 폴더 (파일을 확인할 수 있습니다. GetParentAsync() 합니다. GetIndexedStateAsync()) 합니다. 어느 방법이 든 동일한 결과 생성 

### <a name="reading-supplemental-properties"></a>추가 속성 읽기 
추가 속성을 읽는 역시 다른 파일 시스템 속성을 읽는 것과 같습니다. 이 샘플 앱은 방금 하나의 속성, StorageFile 이미 파일에서 읽지만 동시에 다른 속성을 참조할 수 없습니다.  

```csharp
// An object to hold the result from the indexer, and a string to store  
// the value in once we have confirmed it is valid. 
object uncheckedResourceId; 
string resourceId = ""; 
// Fetching the key value pair from the indexer 
IDictionary<string,object> returnedProps =  
    await file.Properties.RetrievePropertiesAsync(new string[] { "System.Supplemental.ResourceId" });             
if (returnedProps.TryGetValue("System.Supplemental.ResourceId", out uncheckedResourceId)) 
{ 
    if (uncheckedResourceId != null && !String.IsNullOrEmpty(uncheckedResourceId.ToString())) 
    { 
        resourceId = uncheckedResourceId.ToString(); 
    } 
} 
```
속성 시스템에서 다시 전달 되는 값을 예상 대로 인지 확인 하는 검사가 있습니다. 가능성이 높은 경우에 가능 하므로 앱을 작성 하는 값을 지웠습니다. 아래에서 자세히 설명 합니다.  

### <a name="implementation-notes"></a>구현 참고 사항 
보조 속성의 디자인에 적용 된 몇 가지 미묘한 선택할 수 있습니다. 구현에 도움이 되도록 다음 섹션에서는 기능 엔지니어링 디자인 사양에서 복사 되었습니다. 잠깐 기능 설계 된 방식 및 제한 사항 중 일부는 존재 이유를 제공 합니다. 

### <a name="supplemental-properties-available"></a>사용할 수 있는 추가 속성 
사용할 수 있는 앱에 대 한 처음 두 개의 속성만 가지 있습니다. System.Supplemental.ResourceId 및 System.Supplemental.AlbumID 합니다. 추가할 수 있습니다 더 필요한 경우. 앨범 ID는 다양 한 응용 프로그램에 대해 사용할 수 있는 다중 값 문자열 및 클라우드 동기화 공급자에 대 한 ResourceId는 고유 id입니다. 

#### <a name="file-system-support"></a>파일 시스템 지원 
FAT 포맷 된 이동식 미디어는 중요 한 시나리오를 보조 속성을 FAT 또는 NTFS 드라이브 지원 됩니다. 이렇게 하면 보조 속성은 것임 각자의 장치 유형에 관계 없이 모든 사용자에 대해 사용할 수 있습니다.   

### <a name="non-indexed-locations"></a>인덱싱되지 않은 위치  
바탕 화면에 많은 인덱싱되지 않은 폴더가 있습니다. 이러한 경우에 앱 추가 속성에 액세스할 수 있도록 할 수 있습니다. 그러나 추가 속성은 외부 인덱싱된 위치에서 사용할 수 없습니다. 몇 가지 원인은이 절충 실행 되었습니다.  

- 모든 라이브러리와 클라우드 저장소 위치는 기본적으로 인덱싱됩니다.   
  이러한 위치는 UWP 앱에 주로 사용 됩니다. 인덱싱된 (시스템 또는 네트워크 드라이브) 하지 않은 다른 위치는 사용자 데이터를 저장 하는 데 자주 사용 되지 있습니다. 

- WinRT API 화면 디자인 인덱서는 사용 가능한 거의 항상 가정 합니다.  
  따라서 인덱서가 이미 사용할 수 있는 대부분의 앱에서 관심 있는 위치입니다. 사용자를 인덱싱되지 않은 위치에 데이터를 저장할 발견 되 면 인덱스에 해당 위치를 추가할 수는 가장 쉬운 해결 방법은 것입니다. 그런 다음 보조 속성 작업을 열거 하면 속도가 더 및 앱을 변경 하는 일을 할 수 위치를 추적 합니다.

### <a name="reading-or-writing-supplemental-properties-from-a-file-in-a-non-indexed-location"></a>읽기 또는 쓰기 Non-Indexed 위치에 파일에서 추가 속성 
그런 다음 앱 추가 속성을 현재 인덱싱되지 않은 위치에 쓰려고 시도 하는 경우 API 호출 예외가 throw 됩니다. 이 누군가.docx 파일 (잘못 된 Args)에서 System.Music.AlbumArtist 업데이트 하려고 하는 경우로 throw 되는 동일한 예외가 됩니다.  
 
### <a name="change-notifications"></a>변경 알림:  
UWP 변경 알림 및 변경 내용 추적는 계속 표준 속성에 대 한 추가 속성에 대해 작동 합니다. 이렇게 하면 해당 앱 중 하나에 모든 변경 내용을 추적 하는 데이터를 제공 하는 앱 
  
### <a name="invalidating-properties"></a>속성을 무효화 합니다.  
파일에서 추가 속성 파일을 수정 하거나 시스템에서 이동 될 때마다 만료 될 수 있습니다. 데이터가 잘못 되거나 시스템 자체 파악 하기 하기 위해 도구만 제공 하도록 업데이트 해야 하는 경우 데이터를 푸시하는 앱에 대 한 정보를 사용 하 여 항목을 수 있습니다.  
 
경우에는 파일을 수정 하지만 하지 이동 또는 이름이 바뀐 파일에 모든 보조 속성 변경 되지 것입니다. 앱 기존 API 화면을 통해 변경 알림을 등록 하 고 필요에 따라 속성을 업데이트 하는 일을 할 수 있습니다. 
 
파일 이동 되 면 속성을 무효화 하려는. 앱의 삭제 하거나 변경 알림 받기을 만들거나, 이름이 바뀌었거나, 이동 하는 방법에 따라 작업을 정확 하 게 수행 됩니다. 앱 변경 알림을 받으면 파일을 검사 하 고 필요에 따라 파일에 추가 속성을 업데이트 하는 일을 할 됩니다. 
 
### <a name="indexer-rebuilds"></a>인덱서 다시 작성  
시스템 인덱스를 다양 한 이유로 중 하나에 대해 다시 작성 해야 하는 경우에 따라-속성 스키마를 변경할 수, 사용자 EDP를 사용 하도록 설정할 수 또는 단순히 데이터베이스 파일을 손상 될 수 있습니다. 이러한 경우 보조 속성 유지 되지 않습니다. 인덱스를 다시 작성 하는 되었지만 몇 가지 주요 블 때 보조 속성을 유지 하려고 하는 작업을 수행 하 라고 간주 했습니다.  

### <a name="protecting-the-data"></a>데이터 보호 
디스크 오류 또는 악의적인 소프트웨어로 데이터베이스 파일이 손상 된 경우에서 해당 파일에 저장 된 데이터를 보호할 수 없는 것입니다. 저장 하 여 시스템에서 다른 위치 또는 어떤 식으로든 데이터베이스의 나머지 부분과에서 격리 해야 합니다. 

이미 많은 인덱스 손상 될 가능성을 확인 하는 작업을 수행 하는, 하므로 이렇게 발생 변동률이 여기서를 그래도 저하 됩니다.  
다시 작성 하는 동안 파일 및 해당 메타 데이터 간의 매핑을 유지 관리 

인덱스 다시 빌드 간 데이터를 보호할 수 있습니다, 경우에 인덱스를 다시 작성 하는 동안 파일을 변경 하는 경우 알고자 하는 것이 불가능 합니다. 인덱스 파일에서 보호 하는 데이터 파일을 수정 하거나 이동 하는 경우 유효한 더 이상 수 없습니다.  
동작 

인덱서를 다시 작성 하는 경우 모든 추가 데이터는 손실 됩니다. 앱 다시 작성 하는 동안 손실 된 인덱서에 데이터를 다시 설치 해야 합니다. 이 앱에 추가 부담을 삽입 하지만 자신의 모든 데이터에 대 한 마스터 상태 보유할 항상 있으므로 적절 한 간주 됩니다.  

### <a name="recovering"></a>복구 
앱은 인덱스 다시 작성 하는 것을 보았을, 면 편의에 보조 속성을 업데이트할 책임이 됩니다.  
### <a name="privacy"></a>개인 정보 
파일을 쓸 수 있는 속성 중 일부 하려고 사용자 수 없도록 할 다른 응용 프로그램과 공유할 수 있도록 합니다. 앱 속성에 작성 하는 정보에 비공개 것임을 나타내기 위해 수 있어야 합니다. 해당 응용 프로그램을 다른 몇 가지 응용 프로그램을 사용 하 여 공유 또는 공용 시스템에서 모든 앱.  

경우에이 잠재적으로 채택한 기업 기능 중 일부에 대 한 흥미로운 기능을 이러한 공용 속성을 가져오는 여전히 것임 디자인에 많은 값을 추가할 생각 합니다. 따라서을 유용한 것으로 표시 됩니다 하 고 필요한 경우 값 숨기기에 대 한 지원은 기능을 빌드할 수 계속 해야 합니다. 나중에 추가 되므로 모든 디자인에서 고려해 야 할 중요 한 더 많은 시나리오를 열고 됩니다.  

## <a name="conclusions"></a>결론 
이것이, 추가 속성은 시스템에 더 많은 파일 속성을 저장 하는 간편한 방법은 합니다. 단점은 물론 선택 사항 이지만 정렬 하 고 해당 데이터를 신속 하 게 검색할 수 있는 다른 앱을 통해에 지 앱을 제공할 수 있습니다. 

중점을 두고 앞으로 이러한 속성을 사용 하기 시작 하는 앱을 표시 합니다. 어떻게 사용 하 여 헤더 알려 주시기 바랍니다 아래 설명에 대 한 질문이 있는 경우 
