---
ms.assetid: 66400066-24BF-4AF2-B52A-577F5C3CA474
description: Microsoft Store 제출 API에서 이러한 메서드를 사용 하 여 파트너 센터 계정에 등록 된 앱에 대 한 추가 기능 제출을 관리할 수 있습니다.
title: 추가 기능 제출 관리
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 추가 기능 제출, 앱 내 제품, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 16c9fa0f4fa4b7b6ac3ec8e0fb005b80ab1b7872
ms.sourcegitcommit: 7e8dfd83b181fe720b4074cb42adc908e1ba5e44
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98811309"
---
# <a name="manage-add-on-submissions"></a>추가 기능 제출 관리

Microsoft Store 제출 API는 앱에 대 한 추가 기능 (앱 내 제품이 나 IAP 라고도 함)을 관리 하는 데 사용할 수 있는 메서드를 제공 합니다. API를 사용 하기 위한 필수 구성 요소를 포함 하 여 Microsoft Store 제출 API에 대 한 소개는 [Microsoft Store services를 사용 하 여 전송 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)를 참조 하세요.

> [!IMPORTANT]
> Microsoft Store 제출 API를 사용 하 여 추가 기능에 대 한 제출을 만드는 경우 파트너 센터에서 변경 하는 대신 API를 사용 하 여 제출을 추가로 변경 해야 합니다. 파트너 센터를 사용 하 여 원래 API를 사용 하 여 만든 제출을 변경 하는 경우 API를 사용 하 여 해당 제출을 변경 하거나 커밋할 수 없습니다. 경우에 따라 제출 프로세스에서 계속할 수 없는 오류 상태로 제출 될 수 있습니다. 이 문제가 발생 하는 경우 제출을 삭제 하 고 새 제출을 만들어야 합니다.

<span id="methods-for-add-on-submissions" />

## <a name="methods-for-managing-add-on-submissions"></a>추가 기능 제출을 관리 하는 방법

다음 방법을 사용 하 여 추가 기능 제출을 가져오거나 만들거나 업데이트 하거나 커밋하거나 삭제할 수 있습니다. 이러한 방법을 사용 하려면 먼저 파트너 센터 계정에 추가 기능이 있어야 합니다. 파트너 센터에서 [제품 유형 및 제품 ID를 정의](../publish/set-your-add-on-product-id.md) 하거나 [추가 기능 관리](manage-add-ons.md)에 설명 된의 Microsoft Store 제출 API 메서드를 사용 하 여 추가 기능을 만들 수 있습니다.

<table>
<colgroup>
<col width="10%" />
<col width="30%" />
<col width="60%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">방법</th>
<th align="left">URI</th>
<th align="left">설명</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}</td>
<td align="left"><a href="get-an-add-on-submission.md">기존 추가 기능 제출 가져오기</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/status</td>
<td align="left"><a href="get-status-for-an-add-on-submission.md">기존 추가 기능 제출의 상태 가져오기</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions</td>
<td align="left"><a href="create-an-add-on-submission.md">새 추가 기능 제출 만들기</a></td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}</td>
<td align="left"><a href="update-an-add-on-submission.md">기존 추가 기능 제출 업데이트</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/commit</td>
<td align="left"><a href="commit-an-add-on-submission.md">신규 또는 업데이트 된 추가 기능 제출을 커밋합니다.</a></td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}</td>
<td align="left"><a href="delete-an-add-on-submission.md">추가 기능 제출 삭제</a></td>
</tr>
</tbody>
</table>

<span id="create-an-add-on-submission">

## <a name="create-an-add-on-submission"></a>추가 기능 제출 만들기

추가 기능에 대 한 제출을 만들려면이 프로세스를 수행 합니다.

1. 아직 수행 하지 않은 경우 Azure AD 응용 프로그램을 파트너 센터 계정에 연결 하 고 클라이언트 ID 및 키를 가져오는 등 [Microsoft Store 서비스를 사용 하 여 전송 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)에 설명 된 필수 구성 요소를 완료 합니다. 이 작업은 한 번만 수행 하면 됩니다. 클라이언트 ID와 키를 받은 후에는 새 Azure AD 액세스 토큰을 만들어야 할 때마다 다시 사용할 수 있습니다.  

2. [AZURE AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Microsoft Store 제출 API의 메서드에이 액세스 토큰을 전달 해야 합니다. 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후 새 토큰을 얻을 수 있습니다.

3. Microsoft Store 제출 API에서 다음 메서드를 실행 합니다. 이 메서드는 마지막으로 게시 된 제출의 복사본 인 진행 중인 새 제출을 만듭니다. 자세한 내용은 [추가 기능 제출 만들기](create-an-add-on-submission.md)를 참조 하세요.

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions
    ```

    응답 본문에는 새 제출의 ID, Azure Blob Storage에 제출할 추가 기능 아이콘을 업로드 하기 위한 SAS (공유 액세스 서명) URI, 새 전송에 대 한 모든 데이터 (예: 목록 및 가격 정보)를 포함 하는 [추가 기능 제출](#add-on-submission-object) 리소스가 포함 되어 있습니다.

    > [!NOTE]
    > SAS URI는 계정 키를 요구 하지 않고 Azure storage의 보안 리소스에 대 한 액세스를 제공 합니다. SAS Uri와 Azure Blob Storage에 대 한 자세한 내용은 [공유 액세스 서명, 1 부: sas 모델](/azure/storage/common/storage-sas-overview) 및 [공유 액세스 서명 이해, 2 부: Blob 저장소를 사용 하 여 sas 만들기 및 사용](/azure/storage/common/storage-sas-overview)을 참조 하세요.

4. 제출에 대 한 새 아이콘을 추가 하는 경우 [아이콘을 준비](../publish/create-add-on-store-listings.md) 하 고 ZIP 보관 파일에 추가 합니다.

5. 새 제출에 대 한 필수 변경 내용으로 [추가 기능 제출](#add-on-submission-object) 데이터를 업데이트 하 고 다음 메서드를 실행 하 여 제출을 업데이트 합니다. 자세한 내용은 [추가 기능 제출 업데이트](update-an-add-on-submission.md)를 참조 하세요.

    ```json
    PUT https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}
    ```
      > [!NOTE]
      > 제출에 대 한 새 아이콘을 추가 하는 경우 ZIP 보관 파일에 있는 이러한 파일의 이름 및 상대 경로를 참조 하도록 제출 데이터를 업데이트 해야 합니다.

4. 제출에 대 한 새 아이콘을 추가 하는 경우 이전에 호출한 POST 메서드의 응답 본문에 제공 된 SAS URI를 사용 하 여 [Azure Blob Storage](/azure/storage/storage-introduction#blob-storage) 에 ZIP 보관 파일을 업로드 합니다. 다음과 같은 다양 한 플랫폼에서이 작업을 수행 하는 데 사용할 수 있는 다양 한 Azure 라이브러리가 있습니다.

    * [.NET용 Azure Storage 클라이언트 라이브러리](/azure/storage/storage-dotnet-how-to-use-blobs)
    * [Java용 Azure Storage SDK](/azure/storage/storage-java-how-to-use-blob-storage)
    * [Python 용 Azure Storage SDK](/azure/storage/storage-python-how-to-use-blob-storage)

    다음 c # 코드 예제에서는 .NET 용 Azure Storage 클라이언트 라이브러리에서 [Cloudblockblob](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob) 클래스를 사용 하 여 AZURE BLOB STORAGE에 ZIP 보관 파일을 업로드 하는 방법을 보여 줍니다. 이 예에서는 ZIP 보관 파일이 스트림 개체에 이미 기록 된 것으로 가정 합니다.

    ```csharp
    string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";
    Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
      new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
    await blockBob.UploadFromStreamAsync(stream);
    ```

5. 다음 메서드를 실행 하 여 제출을 커밋합니다. 그러면 전송 작업을 수행 하 고 사용자의 계정에 업데이트가 적용 되도록 파트너 센터에 경고를 표시 합니다. 자세한 내용은 [추가 기능 제출 커밋](commit-an-add-on-submission.md)을 참조 하세요.

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/commit
    ```

6. 다음 메서드를 실행 하 여 커밋 상태를 확인 합니다. 자세한 내용은 [추가 기능 제출의 상태 가져오기](get-status-for-an-add-on-submission.md)를 참조 하세요.

    ```json
    GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/status
    ```

    제출 상태를 확인 하려면 응답 본문에서 *상태* 값을 검토 합니다. 요청에 오류가 있는 경우 요청이 성공 하거나 **Commitstarted가 실패** 한 경우이 값은 **Commitstarted** 에서 **전처리** 로 변경 해야 합니다. 오류가 있는 경우 *statusdetails* 필드에 오류에 대 한 추가 정보가 포함 됩니다.

7. 커밋이 성공적으로 완료 되 면 수집을 위해 스토어로 전송 됩니다. 이전 방법을 사용 하거나 파트너 센터를 방문 하 여 제출 진행 상황을 계속 모니터링할 수 있습니다.

<span/>

## <a name="code-examples"></a>코드 예제

다음 문서에서는 다양 한 프로그래밍 언어로 추가 기능 제출을 만드는 방법을 보여 주는 자세한 코드 예제를 제공 합니다.

* [C # 코드 예제](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Java 코드 예제](java-code-examples-for-the-windows-store-submission-api.md)
* [Python 코드 예제](python-code-examples-for-the-windows-store-submission-api.md)

## <a name="storebroker-powershell-module"></a>을 (를) Broker PowerShell 모듈

Microsoft Store 제출 API를 직접 호출 하는 대신 API를 기반으로 명령줄 인터페이스를 구현 하는 오픈 소스 PowerShell 모듈을 제공 합니다. 이 모듈을이 모듈을이 모듈 [이라고 합니다.](https://github.com/Microsoft/StoreBroker) 이 모듈을 사용 하 여 Microsoft Store 제출 API를 직접 호출 하는 대신 명령줄에서 앱, 비행 및 추가 기능 제출을 관리 하거나 단순히 소스를 검색 하 여이 API를 호출 하는 방법에 대 한 더 많은 예제를 볼 수 있습니다. 여러 자사 응용 프로그램을 저장소에 제출 하는 기본 방법으로 Microsoft 내에서이 기능을 사용 합니다.

자세한 내용은 [GitHub의 microsoft 컴퓨터 브로커 페이지](https://github.com/Microsoft/StoreBroker)를 참조 하세요.

<span/>

## <a name="data-resources"></a>데이터 리소스

추가 기능 제출을 관리 하기 위한 Microsoft Store 제출 API 메서드는 다음 JSON 데이터 리소스를 사용 합니다.

<span id="add-on-submission-object" />

### <a name="add-on-submission-resource"></a>추가 기능 제출 리소스

이 리소스는 추가 기능 제출을 설명 합니다.

```json
{
  "id": "1152921504621243680",
  "contentType": "EMagazine",
  "keywords": [
    "books"
  ],
  "lifetime": "FiveDays",
  "listings": {
    "en": {
      "description": "English add-on description",
      "icon": {
        "fileName": "add-on-en-us-listing2.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (English)"
    },
    "ru": {
      "description": "Russian add-on description",
      "icon": {
        "fileName": "add-on-ru-listing.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (Russian)"
    }
  },
  "pricing": {
    "marketSpecificPricings": {
      "RU": "Tier3",
      "US": "Tier4",
    },
    "sales": [],
    "priceId": "Free",
    "isAdvancedPricingModel": true
  },
  "targetPublishDate": "2016-03-15T05:10:58.047Z",
  "targetPublishMode": "Immediate",
  "tag": "SampleTag",
  "visibility": "Public",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [
      {
        "code": "None",
        "details": "string"
      }
    ],
    "warnings": [
      {
        "code": "ListingOptOutWarning",
        "details": "You have removed listing language(s): []"
      }
    ],
    "certificationReports": [
      {
      }
    ]
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl",
  "friendlyName": "Submission 2"
}
```

이 리소스의 값은 다음과 같습니다.

| 값      | 형식   | Description        |
|------------|--------|----------------------|
| id            | 문자열  | 제출 ID입니다. 이 ID는 [추가 기능 제출 만들기](create-an-add-on-submission.md), [모든 추가 기능 가져오기](get-all-add-ons.md)및 [추가 기능 가져오기](get-an-add-on.md)요청에 대 한 응답 데이터에서 사용할 수 있습니다. 파트너 센터에서 만든 제출의 경우이 ID는 파트너 센터의 제출 페이지에 대 한 URL 에서도 사용할 수 있습니다.  |
| contentType           | 문자열  |  추가 기능에 제공 되는 [콘텐츠 형식](../publish/enter-add-on-properties.md#content-type) 입니다. 다음 값 중 하나일 수 있습니다. <ul><li>NotSet</li><li>BookDownload</li><li>EMagazine</li><li>ENewspaper</li><li>MusicDownload</li><li>MusicStream</li><li>OnlineDataStorage</li><li>비디오 다운로드</li><li>VideoStream</li><li>Asp</li><li>OnlineDownload</li></ul> |  
| 키워드           | array  | 추가 기능에 대해 최대 10 개의 [키워드](../publish/enter-add-on-properties.md#keywords) 를 포함 하는 문자열 배열입니다. 앱은 이러한 키워드를 사용 하 여 추가 기능을 쿼리할 수 있습니다.   |
| lifetime           | 문자열  |  추가 기능에 대 한 수명입니다. 다음 값 중 하나일 수 있습니다. <ul><li>영구</li><li>OneDay</li><li>ThreeDays</li><li>FiveDays</li><li>OneWeek</li><li>TwoWeeks</li><li>OneMonth</li><li>TwoMonths</li><li>ThreeMonths</li><li>SixMonths</li><li>OneYear</li></ul> |
| 목록           | 개체  |  키/값 쌍의 사전입니다. 여기서 각 키는 두 문자로 된 ISO 3166-1 알파 2 국가 코드이 고 각 값은 추가 기능에 대 한 나열 정보를 포함 하는 [목록 리소스](#listing-object) 입니다.  |
| 가격 책정           | 개체  | 추가 기능에 대 한 가격 책정 정보를 포함 하는 [가격 책정 리소스](#pricing-object) 입니다.   |
| targetPublishMode           | 문자열  | 제출에 대 한 게시 모드입니다. 다음 값 중 하나일 수 있습니다. <ul><li>직접 실행</li><li>수동</li><li>SpecificDate</li></ul> |
| Target버전           | 문자열  | *TargetSpecificDate mode* 가로 설정 된 경우 ISO 8601 형식의 제출에 대 한 게시 날짜입니다.  |
| tag           | 문자열  |  추가 기능에 대 한 [사용자 지정 개발자 데이터](../publish/enter-add-on-properties.md#custom-developer-data) 입니다 (이전에는이 정보를 *태그* 라고 함).   |
| 표시 여부  | 문자열  |  추가 기능에 대 한 표시 여부입니다. 다음 값 중 하나일 수 있습니다. <ul><li>숨김</li><li>공용</li><li>프라이빗</li><li>NotSet</li></ul>  |
| 상태  | 문자열  |  제출의 상태입니다. 다음 값 중 하나일 수 있습니다. <ul><li>없음</li><li>취소됨</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>게시</li><li>게시 날짜</li><li>이상 실패</li><li>바꾸면</li><li>PreProcessingFailed</li><li>인증</li><li>CertificationFailed</li><li>해제</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | 개체  |  오류에 대 한 정보를 포함 하 여 제출 상태에 대 한 추가 정보를 포함 하는 [상태 세부 정보 리소스](#status-details-object) 입니다. |
| fileUploadUrl           | 문자열  | 제출할 패키지를 업로드 하기 위한 SAS (공유 액세스 서명) URI입니다. 제출할 새 패키지를 추가 하는 경우 패키지를 포함 하는 ZIP 보관 파일을이 URI에 업로드 합니다. 자세한 내용은 [추가 기능 제출 만들기](#create-an-add-on-submission)를 참조 하세요.  |
| friendlyName  | 문자열  |  파트너 센터에 표시 된 것 처럼 전송의 이름입니다. 이 값은 제출을 만들 때 생성 됩니다.  |

<span id="listing-object" />

### <a name="listing-resource"></a>리소스 나열

이 리소스 [는 추가 기능에 대 한 나열 정보를](../publish/create-add-on-store-listings.md)포함 합니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 형식    | Description       |
|-----------------|---------|------|
|  description               |    문자열     |   추가 기능 목록에 대 한 설명입니다.   |     
|  icon               |   개체      |추가 기능 목록에 대 한 아이콘 데이터를 포함 하는 [아이콘 리소스](#icon-object) 입니다.    |
|  title               |     문자열    |   추가 기능 목록에 대 한 제목입니다.   |  

<span id="icon-object" />

### <a name="icon-resource"></a>아이콘 리소스

이 리소스는 추가 기능 목록에 대 한 아이콘 데이터를 포함 합니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 형식    | 설명     |
|-----------------|---------|------|
|  fileName               |    문자열     |   제출을 위해 업로드 한 ZIP 보관 파일에 있는 아이콘 파일의 이름입니다. 아이콘은 정확히 300 x 300 픽셀을 측정 하는 .png 파일 이어야 합니다.   |     
|  fileStatus               |   문자열      |  아이콘 파일의 상태입니다. 다음 값 중 하나일 수 있습니다. <ul><li>없음</li><li>PendingUpload</li><li>업로드됨</li><li>PendingDelete</li></ul>   |

<span id="pricing-object" />

### <a name="pricing-resource"></a>가격 책정 리소스

이 리소스에는 추가 기능에 대 한 가격 정보가 포함 되어 있습니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 형식    | 설명    |
|-----------------|---------|------|
|  marketSpecificPricings               |    개체     |  키/값 쌍의 사전입니다. 여기서 각 키는 두 문자로 된 ISO 3166-1 알파 2 국가 코드이 고 각 값은 [가격 책정 계층](#price-tiers)입니다. 이러한 항목은 [특정 시장에서 추가 기능에 대 한 사용자 지정 가격](../publish/set-add-on-pricing-and-availability.md)을 나타냅니다. 이 사전의 모든 항목은 지정 된 시장에 대해 *priceId* 값으로 지정 된 기본 가격을 재정의 합니다.     |     
|  sales               |   array      |  **사용 되지 않습니다**. 추가 기능에 대 한 판매 정보를 포함 하는 [판매 리소스](#sale-object) 의 배열입니다.     |     
|  priceId               |   문자열      |  추가 기능에 대 한 [기본 가격](../publish/set-add-on-pricing-and-availability.md) 을 지정 하는 [가격 책정 계층](#price-tiers) 입니다.    |    
|  isAdvancedPricingModel               |   boolean      |  **True** 이면 개발자 계정에서 99 usd부터 1999.99 usd 까지의 확장 된 가격 책정 계층 집합에 액세스할 수 있습니다. **False** 이면 개발자 계정에서 99 usd에서 999.99 usd 까지의 원래 가격 책정 계층에 액세스할 수 있습니다. 여러 계층에 대 한 자세한 내용은 가격 책정 [계층](#price-tiers)을 참조 하세요.<br/><br/> &nbsp; 참고 &nbsp; 이 필드는 읽기 전용입니다.   |


<span id="sale-object" />

### <a name="sale-resource"></a>판매 리소스

이 리소스는 추가 기능에 대 한 판매 정보를 포함 합니다.

> [!IMPORTANT]
> **판매** 리소스는 더 이상 지원 되지 않으며 현재 MICROSOFT STORE 제출 API를 사용 하 여 추가 기능 제출에 대 한 판매 데이터를 가져오거나 수정할 수 없습니다. 향후에는 추가 기능 제출에 대 한 판매 정보에 프로그래밍 방식으로 액세스 하는 새로운 방법을 도입 하도록 Microsoft Store 제출 API를 업데이트 합니다.
>    * [Get 메서드를 호출 하 여 추가 기능 제출을 가져온](get-an-add-on-submission.md)후에는 *sales* 값이 비어 있게 됩니다. 파트너 센터를 계속 사용 하 여 추가 기능 제출에 대 한 판매 데이터를 가져올 수 있습니다.
>    * [PUT 메서드를 호출 하 여 추가 기능 제출을 업데이트할](update-an-add-on-submission.md)때 *sales* 값의 정보는 무시 됩니다. 파트너 센터를 계속 사용 하 여 추가 기능 제출에 대 한 판매 데이터를 변경할 수 있습니다.

이 리소스의 값은 다음과 같습니다.

| 값           | 형식    | 설명           |
|-----------------|---------|------|
|  name               |    문자열     |   판매의 이름입니다.    |     
|  basePriceId               |   문자열      |  판매의 기본 가격에 사용할 [가격 계층](#price-tiers) 입니다.    |     
|  startDate               |   문자열      |   ISO 8601 형식의 판매에 대 한 시작 날짜입니다.  |     
|  endDate               |   문자열      |  ISO 8601 형식의 판매에 대 한 종료 날짜입니다.      |     
|  marketSpecificPricings               |   개체      |   키/값 쌍의 사전입니다. 여기서 각 키는 두 문자로 된 ISO 3166-1 알파 2 국가 코드이 고 각 값은 [가격 책정 계층](#price-tiers)입니다. 이러한 항목은 [특정 시장에서 추가 기능에 대 한 사용자 지정 가격](../publish/set-add-on-pricing-and-availability.md)을 나타냅니다. 이 사전의 모든 항목은 지정 된 시장에 대해 *basePriceId* 값으로 지정 된 기본 가격을 재정의 합니다.    |

<span id="status-details-object" />

### <a name="status-details-resource"></a>상태 정보 리소스

이 리소스는 제출 상태에 대 한 추가 정보를 포함 합니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 형식    | 설명       |
|-----------------|---------|------|
|  오류               |    개체     |   제출에 대 한 오류 정보를 포함 하는 [상태 정보 리소스](#status-detail-object) 의 배열입니다.   |     
|  경고               |   개체      | 제출에 대 한 경고 세부 정보를 포함 하는 [상태 정보 리소스](#status-detail-object) 의 배열입니다.     |
|  certificationReports               |     개체    |   제출할 인증 보고서 데이터에 대 한 액세스를 제공 하는 [인증 보고서 리소스](#certification-report-object) 의 배열입니다. 이러한 보고서에서 인증에 실패 하는 경우 자세한 정보를 확인할 수 있습니다.    |  

<span id="status-detail-object" />

### <a name="status-detail-resource"></a>상태 정보 리소스

이 리소스에는 전송에 대 한 모든 관련 오류 또는 경고에 대 한 추가 정보가 포함 되어 있습니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 형식    | 설명    |
|-----------------|---------|------|
|  code               |    문자열     |   오류 또는 경고 유형을 설명 하는 [제출 상태 코드](#submission-status-code) 입니다.   |     
|  자세히               |     문자열    |  문제에 대 한 자세한 정보를 포함 하는 메시지입니다.     |

<span id="certification-report-object" />

### <a name="certification-report-resource"></a>인증 보고서 리소스

이 리소스는 제출에 대 한 인증 보고서 데이터에 대 한 액세스를 제공 합니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 형식    | 설명               |
|-----------------|---------|------|
|     date            |    문자열     |  보고서가 생성 된 날짜와 시간 (ISO 8601 형식)입니다.    |
|     reportUrl            |    문자열     |  보고서에 액세스할 수 있는 URL입니다.    |

## <a name="enums"></a>열거형

이러한 메서드는 다음 열거형을 사용 합니다.

<span id="price-tiers" />

### <a name="price-tiers"></a>가격 책정 계층

다음 값은 추가 기능 제출에 대 한 [가격 책정 리소스](#pricing-object) 리소스에서 사용 가능한 가격 책정 계층을 나타냅니다.

| 값           | 설명       |
|-----------------|------|
|  기준               |   가격 책정 계층이 설정 되지 않았습니다. 추가 기능에 대 한 기본 가격을 사용 합니다.      |     
|  NotAvailable              |   지정 된 영역에서 추가 기능을 사용할 수 없습니다.    |     
|  무료              |   추가 기능은 무료입니다.    |    
|  계층 *xxxx*               |   **계층 <em>xxxx</em>** 형식의 추가 기능에 대 한 가격 책정 계층을 지정 하는 문자열입니다. 현재 지원 되는 가격 책정 계층 범위는 다음과 같습니다.<br/><br/><ul><li>[가격 책정 리소스](#pricing-object) 의 *isAdvancedPricingModel* 값이 **true** 이면 계정에 사용할 수 있는 가격 책정 계층 값은 **Tier1012**  -  **Tier1424** 입니다.</li><li>[가격 책정 리소스](#pricing-object) 의 *isAdvancedPricingModel* 값이 **false** 이면 계정에 사용할 수 있는 가격 책정 계층 값은 **Tier2**  -  **Tier96** 입니다.</li></ul>각 계층과 관련 된 시장 관련 가격을 비롯 하 여 개발자 계정에 사용할 수 있는 가격 책정 계층의 전체 표를 보려면 파트너 센터에서 앱 서브 미션에 대 한 **가격 책정 및 가용성** 페이지로 이동 하 고 **시장 및 사용자 지정 가격** 섹션에서 **테이블 보기** 링크를 클릭 합니다 (일부 개발자 계정의 경우이 링크는 **가격 책정** 섹션에 있음).     |

<span id="submission-status-code" />

### <a name="submission-status-code"></a>제출 상태 코드

다음 값은 제출의 상태 코드를 나타냅니다.

| 값           |  설명      |
|-----------------|---------------|
|  없음            |     코드를 지정 하지 않았습니다.         |     
|      InvalidArchive        |     패키지를 포함 하는 ZIP 보관 파일이 잘못 되었거나 보관 파일 형식을 인식할 수 없습니다.  |
| MissingFiles | ZIP 보관 파일에 제출 데이터에 나열 된 파일이 없거나 보관 파일의 잘못 된 위치에 있습니다. |
| PackageValidationFailed | 전송에서 하나 이상의 패키지의 유효성을 검사 하지 못했습니다. |
| InvalidParameterValue | 요청 본문의 매개 변수 중 하나가 잘못 되었습니다. |
| InvalidOperation | 시도한 작업이 잘못 되었습니다. |
| InvalidState | 시도한 작업은 패키지 비행의 현재 상태에 대해 유효 하지 않습니다. |
| ResourceNotFound | 지정 된 패키지 비행을 찾을 수 없습니다. |
| ServiceError | 내부 서비스 오류가 발생 하 여 요청이 성공 하지 못했습니다. 요청을 다시 시도 하세요. |
| ListingOptOutWarning | 개발자가 이전 제출에서 목록을 제거 했거나 패키지에서 지원 되는 나열 정보를 포함 하지 않았습니다. |
| ListingOptInWarning  | 개발자가 목록을 추가 했습니다. |
| Updateonly 경고 | 개발자가 업데이트 지원만 있는 항목을 삽입 하려고 합니다. |
| 기타  | 제출이 인식 되지 않거나 범주화 되지 않은 상태입니다. |
| PackageValidationWarning | 패키지 유효성 검사 프로세스로 인해 경고가 발생 했습니다. |

<span/>

## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용 하 여 제출 작성 및 관리](create-and-manage-submissions-using-windows-store-services.md)
* [Microsoft Store 제출 API를 사용 하 여 추가 기능 관리](manage-add-ons.md)
* [파트너 센터의 추가 기능 제출](../publish/add-on-submissions.md)
