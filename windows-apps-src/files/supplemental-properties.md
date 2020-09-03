---
title: 추가 속성 사용
description: 추가 속성 사용 및 Windows에서 추가 속성을 구현하는 자세한 방법을 설명합니다.
ms.date: 01/10/2017
ms.topic: article
keywords: windows 10, uwp, WinRT API, 인덱서, 검색
localizationpriority: medium
ms.openlocfilehash: 3103074e7d691897e9a8982a254ba36ee331a2b6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175397"
---
# <a name="using-supplemental-properties"></a>추가 속성 사용  

## <a name="summary"></a>요약  
- 추가 속성을 사용하면 파일을 변경하지 않고도 앱에서 속성을 사용하여 파일에 태그를 지정할 수 있습니다. 
- 계산하기 어려운 속성이 있거나 파일을 수정할 수 없는 경우에 유용합니다. 
- 추가 속성을 사용하는 방법은 Windows 속성 시스템에서 다른 속성을 사용하는 방법과 동일합니다.  

## <a name="introduction"></a>소개 
최근 몇 년 사이에 출시된 여러 흥미로운 최신 앱은 생성된 날짜 같은 기본 정보보다 유용한 속성을 파일에서 추출하려면 사용자 파일에서 CPU 집약적인 작업을 실행해야 합니다. 이러한 앱은 이미지의 개체를 인식하고, 이메일의 의도를 추출하고, 텍스트를 분석하여 문서를 그룹화합니다. 이것이 가능한 이유는 이제 대부분의 소비자 PC에서 매우 강력한 컴퓨팅이 가능하기 때문입니다.   

이 메타데이터를 즉시 검색할 수 있게 만들면 사용자의 생산성이 급격하게 향상됩니다. 사진 속에 딸이 있다는 것을 아는 것만으로도 흥미가 생기지만, 딸과 할머니가 함께 찍은 사진을 검색할 수 있다면 훨씬 더 좋습니다. 보다 개인적이고 생생한 컴퓨터 사용 경험을 얻을 수 있습니다. 머신에 저장된 누군가의 사진을 보며 소중한 추억을 떠올릴 수 있는 것이죠. 

지난 수십 년 동안 Windows에서 빠르게 검색하기 위한 솔루션은 인덱서였으며, 크리에이터스 업데이트가 출시되면서 이러한 새 시나리오를 지원하도록 업데이트되었습니다. 이제 앱에서는 시스템이 추출한 속성 이외의 추가 속성을 사용하여 파일에 태그를 지정할 수 있습니다. 이러한 속성은 일급 개체로 취급됩니다.  

## <a name="windows-properties"></a>Windows 속성 
[Windows 속성 시스템](/windows/desktop/properties/windows-properties-system)은 오랫동안 파일과의 상호 작용에서 핵심적인 역할을 담당했습니다. 이 시스템 덕분에 앱은 다른 모든 파일 형식 또는 가능한 파일 언어의 내부 구조를 이해하지 않고도 파일의 속성을 읽을 수 있습니다. 모든 것이 자동으로 추출되므로 개발자는 목록을 요청하고 오름차순 또는 내림차순으로 지정하지만 하면 됩니다.  

속성 시스템은 Windows 인덱서와 밀접하게 관련되어 있으며, 범위 내에 있는 파일의 모든 속성을 읽고 저장합니다. 나중에 앱에서 폴더의 모든 .docx 파일을 수정 날짜 기준으로 정렬한 목록을 요청할 때 John Smith가 작성한 파일을 제외하면 인덱서가 즉시 목록을 반환할 수 있습니다.  

이러한 시스템이 함께 작동하는 방식의 단점은 파일에 대해 저장할 모든 속성을 요구하는 데 사용되는 인덱서를 즉시 제공할 수 있어야 한다는 것입니다. 이 때문에 계산 시간이 오래 걸리는 보다 흥미로운 속성을 알 수 없다는 제한이 있습니다. 시간 요구 사항이 매우 까다롭기 때문입니다.  

속성을 사용하기는 쉽지만, 데이터베이스를 사용할 때처럼 앱이 파일에 대해 정렬된 속성 세트를 요청하거나, 검색 엔진을 사용하는 것처럼 앱이 쿼리를 전달할 수 있습니다. 인덱서는 쿼리를 처리하고 결과를 반환합니다. 따라서 개발자는 유연하게 필터(예: jpg 파일만 검색)와 사용자 쿼리("bird"로 시작하는 파일 이름)를 결합할 수 있습니다. 

## <a name="supplemental-properties"></a>추가 속성 

추가 속성은 일반 Windows 속성과 동일하게 작동하지만, 파일이 인덱서에 추가될 때 작성되지 않는다는 결정적인 차이점이 있습니다. 추가 속성은 나중에 시스템의 다른 앱에서 추가해야 합니다. 개체 인식이 완료된 지 2분 후가 될 수도 있고 며칠 뒤가 될 수도 있습니다. 

속성이 작성되면 시스템의 다른 속성과 마찬가지로 속성을 검색, 필터링, 정렬 또는 그룹화할 수 있습니다. 시스템의 다른 속성(추가 속성이든 아니든 관계없이)과 결합된 쿼리에도 사용할 수 있습니다. 따라서 코드를 작성할 필요 없이 간편하게 추가 속성을 기존 파일 시스템 코드와 유연하게 결합할 수 있습니다.  

### <a name="example-scenarios"></a>예제 시나리오 

추가 속성에 작성할 수 있는 수천 개의 다른 속성이 있지만, 이 자습서에서는 다음과 같은 몇 가지 주요 시나리오를 지속적으로 살펴볼 것입니다.  

#### <a name="tagging-pictures-with-extracted-properties"></a>추출된 속성을 사용하여 사진에 태그 지정 
이러한 앱은 ML 학습된 모델을 사용하여 이미지의 개체처럼 시스템에서 알지 못하는 이미지 특징을 추출할 수 있습니다. 그런 다음, 이미지에서 식별되는 개체를 가져와서 나중에 검색 또는 그룹화할 수 있도록 속성 시스템에 추가할 수 있습니다.  

#### <a name="tagging-files-with-an-app-specific-id"></a>앱별 ID를 사용하여 파일에 태그 지정 
많은 파일 동기화 앱이 자체적인 고유 ID를 사용하여 서버와 다양한 클라이언트 디바이스 간에 이동하는 파일을 추적합니다. 동기화 클라이언트는 파일에 영향을 주지 않고 이 ID를 속성 시스템에 쓸 수 있습니다. 이제 이 ID는 나중에 앱에 빠르게 액세스할 때 사용하거나 시스템의 다른 앱이 동기화 공급자와 통신할 때 이 ID를 읽을 수 있습니다. 

그 외에도 추가 속성을 사용하는 여러 옵션이 있지만, 빠른 조회 또는 검색이 필요하고 시스템에서 알지 못하는 정보이고 파일 자체에 추가할 수 없는 위의 두 가지가 가장 좋은 예제입니다.  

### <a name="using-supplemental-properties"></a>추가 속성 사용 
추가 속성을 사용하는 방법은 파일 시스템에 일반 속성을 작성하는 방법과 동일합니다. StorageFiles 및 속성을 익숙하게 사용할 수 있는 분들은 이 단계를 건너뛰어도 좋습니다. 그렇지 않은 분들은 저와 함께 파일에 속성을 하나 작성한 다음, 샘플 속성을 다시 읽어보는 간단한 샘플을 연습하겠습니다.  

### <a name="writing-supplemental-properties"></a>추가 속성 작성  
이 샘플에서는 편의상 첫 번째로 발견되는 파일만 수정하지만, 일반적으로 앱은 발견하는 모든 파일에 속성을 추가합니다.  

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

속성을 작성하기 전에 위치가 먼저 인덱싱되는 경우 확인해야 할 중요한 사항이 있습니다. 이 샘플에서는 인덱싱된 위치만 필터링하는 쿼리 옵션을 사용하겠습니다. 이 방법을 구현할 수 없는 경우에는 부모 폴더의 인덱싱 상태를 확인하면 됩니다(file.GetParentAsync().GetIndexedStateAsync()). 어떤 방법을 사용해도 같은 결과를 얻습니다. 

### <a name="reading-supplemental-properties"></a>추가 속성 읽기 
마찬가지로, 추가 속성을 읽는 방법은 다른 파일 시스템 속성을 읽는 방법과 동일합니다. 이 샘플의 앱은 이미 StorageFile을 갖고 있는 파일의 한 속성을 읽지만, 다른 속성을 동시에 읽을 수도 있습니다.  

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
속성 시스템에서 반환되는 값이 예상과 일치하도록 한 가지 사항을 확인해야 합니다. 가능성이 낮지만, 앱이 값을 작성한 후 값이 제거되었을 가능성이 있습니다. 이 내용은 아래에서 자세히 다루겠습니다.  

### <a name="implementation-notes"></a>구현 참고 사항 
추가 속성을 디자인할 때 몇 가지 영리한 선택을 했습니다. 구현에 도움을 드리기 위해 기능에 대한 엔지니어링 디자인 사양에서 다음 섹션을 복사해 왔습니다. 다음 섹션에서는 기능을 어떻게 디자인했으며 어떤 제한이 있는지 간단하게 설명합니다. 

### <a name="supplemental-properties-available"></a>사용 가능한 추가 속성 
처음에는 앱에 사용할 수 있는 속성이 두 개밖에 없습니다. 하나는 System.Supplemental.ResourceId이고 다른 하나는 System.Supplemental.AlbumID입니다. 다른 속성이 더 필요하면 속성을 더 추가할 수 있습니다. 앨범 ID는 다양한 애플리케이션에 사용할 수 있는 다중 값 문자열이고, ResourceId는 클라우드 동기화 공급자의 고유 ID로 사용됩니다. 

#### <a name="file-system-support"></a>파일 시스템 지원 
FAT로 포맷된 이동식 미디어는 중요한 시나리오이므로 추가 속성에서 FAT 및 NTFS 드라이브를 지원할 것입니다. 이렇게 하면 모든 사용자가 디바이스 유형에 관계 없이 추가 속성을 사용할 수 있습니다.   

### <a name="non-indexed-locations"></a>인덱싱되지 않은 위치  
바탕 화면에는 인덱싱되지 않은 여러 폴더가 있습니다. 이 경우에도 앱이 여전히 추가 속성에 대한 액세스가 필요할 수 있습니다. 그러나 추가 속성은 인덱싱된 위치 외부에서 사용할 수 없습니다. 이와 같은 절충 지점을 만든 이유는 다음과 같습니다.  

- 모든 라이브러리 및 클라우드 스토리지 위치는 기본적으로 인덱싱됩니다.   
  이 위치는 UWP 앱에서 주로 사용하는 위치입니다. 인덱싱되지 않는 다른 위치(시스템 또는 네트워크 드라이브)가 있지만 이러한 위치는 사용자 데이터를 저장하는 데 자주 사용되지 있습니다. 

- WinRT API 화면을 디자인할 때 인덱서는 거의 항상 사용 가능한 것으로 가정합니다.  
  따라서 앱이 관심을 갖는 대부분의 위치에서 이미 인덱서를 사용할 수 있습니다. 사용자가 인덱싱되지 않은 위치에 데이터를 저장하는 것으로 확인되는 경우 가장 쉬운 솔루션은 해당 위치를 인덱스에 추가하는 것입니다. 그러면 추가 속성이 작동하고, 열거형이 더 빨라지고, 앱이 위치를 변경 및 추적할 수 있습니다.

### <a name="reading-or-writing-supplemental-properties-from-a-file-in-a-non-indexed-location"></a>인덱싱되지 않은 위치의 파일에서 속성 읽기 또는 쓰기 
앱이 현재 인덱싱되지 않은 위치에 추가 속성을 작성하려고 시도하면 API 호출 시 예외가 throw됩니다. 이 예외는 누군가가 .docx 파일의 System.Music.AlbumArtist(잘못된 인수)를 업데이트하려고 시도할 때 throw되는 예외와 똑같습니다.  
 
### <a name="change-notifications"></a>변경 알림:  
UWP 변경 알림 및 변경 내용 추적은 표준 속성과 동일한 방식으로 추가 속성에 대해 작동합니다. 따라서 데이터를 제공하는 앱이 앱 중 하나의 모든 변경 내용을 추적할 수 있습니다. 
  
### <a name="invalidating-properties"></a>속성 무효화  
파일이 수정되거나 시스템에서 이동되면 해당 파일의 추가 속성이 쓸모없게 될 수 있습니다. 데이터를 푸시하는 앱은 데이터가 유효한지 아니면 데이터를 업데이트해야 하는지 여부에 대한 정보를 갖고 있으므로 시스템에서는 이를 직접 확인할 수 있는 도구를 앱에 제공합니다.  
 
파일이 수정되었지만 이동되거나 이름이 변경되지는 않은 경우 파일의 모든 추가 속성이 변경되지 않고 유지됩니다. 앱에서는 기존 API 화면을 통해 변경 알림을 등록하고 필요에 따라 속성을 업데이트할 수 있습니다. 
 
파일이 이동되면 속성이 무효화됩니다. 앱은 작업이 수행된 정확한 방식에 따라 파일이 삭제, 생성, 이름 변경 또는 이동되었다는 알림을 받게 됩니다. 앱은 변경 알림을 받으면 파일을 검사하고 필요에 따라 파일의 추가 속성을 업데이트할 수 있습니다. 
 
### <a name="indexer-rebuilds"></a>인덱서 다시 작성  
속성 스키마가 변경될 가능성이 있거나, 사용자가 EDP를 사용할 가능성이 있거나, 단순히 데이터베이스 파일이 손상될 가능성이 있거나, 기타 이유로 시스템 인덱스를 다시 작성해야 하는 경우가 종종 있습니다. 이 경우 추가 속성이 유지되지 않습니다. 인덱스를 다시 작성할 때 추가 속성을 유지하기 위한 작업을 진행하려 했으나, 몇 가지 장애물이 있었습니다.  

### <a name="protecting-the-data"></a>데이터 보호 
디스크 오류 또는 악의적인 소프트웨어로 인해 데이터베이스 파일이 손상된 경우 해당 파일에 저장된 데이터를 보호할 수 없습니다. 시스템 내 다른 위치 또는 나머지 데이터베이스와 격리된 위치에 데이터를 저장해야 합니다. 

이미 인덱스 손상 가능성을 줄이기 위한 여러 작업이 진행 중이지만, 어쨌거나 이렇게 하면 발생률이 감소할 것입니다.  
다시 작성하는 동안 파일과 해당 메타데이터 사이의 매핑 유지 

다시 작성하는 동안 인덱스가 데이터를 보호할 수 있더라도 인덱스가 다시 작성되는 동안 파일이 변경되었는지 알아내는 것은 불가능합니다. 파일이 수정 또는 이동되면 인덱스가 보호하는 이 파일의 데이터가 더 이상 유효하지 않을 수 있습니다.  
동작 

인덱서를 다시 작성하는 경우 모든 추가 데이터가 손실됩니다. 앱은 다시 작성하는 동안 손실된 인덱서에 데이터를 다시 배치해야 합니다. 따라서 앱의 부담이 증가하지만, 앱이 항상 모든 데이터의 마스터 상태를 보유하게 되므로 합리적이라고 볼 수 있습니다.  

### <a name="recovering"></a>복구 
앱은 인덱스가 다시 작성되고 있는 것을 인지하면 편한 시간에 추가 속성을 업데이트해야 합니다.  
### <a name="privacy"></a>개인 정보 
파일에 작성되는 속성 중 일부는 사용자가 다른 애플리케이션과 공유하기를 원하지 않을 수 있습니다. 앱은 속성에 작성하는 정보를 소수의 애플리케이션에만 비공개적으로 공유할 것인지 아니면 시스템의 모든 앱에 공개할 것인지 나타낼 수 있어야 합니다.  

이 기능은 일부 얼리 어답터의 흥미를 자극할 수 있지만, 대부분은 여전히 공개 속성을 사용하는 것이 디자인의 가치를 높이는 길이라고 생각합니다. 따라서 이 기능은 있으면 좋은 정도로 정리할 수 있으며, 필요하다면 값 숨기기를 지원하지 않는 기능을 계속 작성해야 합니다. 나중에 이 기능을 추가하면 더 많은 시나리오를 지원할 수 있으므로 모든 디자인에서 이 기능을 고려해야 합니다.  

## <a name="conclusions"></a>결론 
추가 속성은 시스템에 더 많은 파일 속성을 저장하는 간편한 방법입니다. 추가 속성을 사용하는 것은 당연히 선택 사항이지만, 데이터를 신속하게 정렬하고 검색할 수 없는 다른 앱보다 우위에 설 수 있습니다. 

앱에 이러한 속성이 사용되는 모습을 볼 수 있기를 기대합니다. 헤더 사용 방법에 대한 질문이 있으면 아래에 댓글로 남겨주시기 바랍니다.