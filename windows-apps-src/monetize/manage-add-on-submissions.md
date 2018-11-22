---
author: Xansky
ms.assetid: 66400066-24BF-4AF2-B52A-577F5C3CA474
description: Microsoft Store 제출 API에서에서 이러한 메서드를 사용 하 여 파트너 센터 계정에 등록 된 앱에 대 한 추가 기능 제출을 관리 합니다.
title: 추가 기능 제출 관리
ms.author: mhopkins
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 추가 기능 제출, 앱에서 바로 구매 제품, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 0ae0e07b588415094281683ff762c02ed5242654
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7576760"
---
# <a name="manage-add-on-submissions"></a>추가 기능 제출 관리

Microsoft Store 제출 API에서 제공하는 여러 메서드를 사용하여 앱에 대한 추가 기능(앱에서 바로 구매 제품 또는 IAP라고도 함) 제출을 관리할 수 있습니다. API 사용을 위한 필수 조건을 비롯하여 Microsoft Store 제출 API에 대한 자세한 내용은 [Microsoft Store 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)를 참조하세요.

> [!IMPORTANT]
> Microsoft Store 제출 API를 사용 하 여 추가 기능에 대 한 제출을 생성 하는 경우에 추가 변경 된 제출에 파트너 센터에서 변경 하는 것이 아니라 API를 사용 하 여만 해야 합니다. 파트너 센터를 사용 하 여 원래 API를 사용 하 여 만든 제출을 변경을 변경 하거나 해당 제출 API를 사용 하 여 커밋할 수 없게 됩니다. 경우에 따라 제출이 제출 프로세스를 더 이상 진행할 수 없는 오류 상태로 남을 수 있습니다. 이러한 문제가 발생하는 경우, 해당 제출을 삭제하고 새 제출을 생성해야 합니다.

<span id="methods-for-add-on-submissions" />

## <a name="methods-for-managing-add-on-submissions"></a>추가 기능 제출 관리 메서드

추가 기능 제출의 가져오기, 만들기, 업데이트, 커밋, 삭제에는 다음 메서드를 사용합니다. 이러한 메서드를 사용 하려면 먼저 추가 기능이 파트너 센터 계정에 이미 있어야 합니다. 파트너 센터에 설명 된 [추가 기능 관리의](manage-add-ons.md)Microsoft Store 제출 API 메서드를 사용 하 여 또는 [제품 유형과 제품 ID를 정의](../publish/set-your-add-on-product-id.md) 하 여 추가 기능을 만들 수 있습니다.

<table>
<colgroup>
<col width="10%" />
<col width="30%" />
<col width="60%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">메서드</th>
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
<td align="left"><a href="commit-an-add-on-submission.md">새로운 또는 업데이트된 추가 기능 제출 커밋</a></td>
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

추가 기능에 대한 제출을 만들려면 이 프로세스를 따릅니다.

1. 아직 수행 하는 경우 필수 구성 요소에 설명 된 완료 [만들기 및 Microsoft Store 서비스를 사용 하 여 제출 관리](create-and-manage-submissions-using-windows-store-services.md)를 파트너 센터 계정으로 Azure AD 응용 프로그램 연결 및 클라이언트 ID와 키 얻기를 비롯 하 여 합니다. 이 작업은 한 번만 수행하면 됩니다. 클라이언트 ID와 키를 얻은 후에는 새 Azure AD 액세스 토큰을 만들어야 할 때마다 다시 사용할 수 있습니다.  

2. [Azure AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Microsoft Store 제출 API의 메서드에 이 액세스 토큰을 전달해야 합니다. 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

3. Microsoft Store 제출 API에서 다음 메서드를 실행합니다. 이 메서드는 마지막으로 게시된 제출의 복사본인 새 진행 중 제출을 만듭니다. 자세한 내용은 [추가 기능 제출 만들기](create-an-add-on-submission.md)를 참조하세요.

    ```
    POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions
    ```

    응답 본문에는 새 제출 ID, Azure Blob Storage 제출을 위한 추가 기능 아이콘 업로딩용 SAS(Shared Access Signature) RUI, 새 제출에 대한 모든 데이터(목록과 가격 정보 등)가 포함된 [추가 기능 제출](#add-on-submission-object) 리소스가 포함되어 있습니다.

    > [!NOTE]
    > SAS URI는 계정 키를 요구하지 않고 Azure Storage의 보안 리소스에 대한 액세스를 제공합니다. SAS URI 및 Azure Blob Storage를 통한 SAS URI 사용에 대한 배경 정보는 [공유 액세스 서명, 1부: SAS 모델 이해](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1) 및  [공유 액세스 서명, 2부: Blob Storage를 사용하여 SAS 만들기 및 사용](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/)을 참조하세요.

4. 제출에 대한 새 아이콘을 추가하는 경우 [아이콘을 준비](https://msdn.microsoft.com/windows/uwp/publish/create-iap-descriptions#icon)하고 ZIP 보관 파일에 추가합니다.

5. 새 제출에 대해 필요한 변경 사항으로 [추가 기능 제출](#add-on-submission-object) 데이터를 업데이트하고 다음 메서드를 실행하여 제출을 업데이트합니다. 자세한 내용은 [추가 기능 제출 업데이트](update-an-add-on-submission.md)를 참조하세요.

    ```
    PUT https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}
    ```
      > [!NOTE]
      > 제출에 대한 새 아이콘을 추가하는 경우 ZIP 보관 파일에서 이러한 파일의 상대 경로 및 이름을 참조하도록 제출 데이터를 업데이트해야 합니다.

4. 제출에 대한 새 아이콘을 추가하는 경우 이전에 호출한 POST 메서드의 응답 본문에 제공된 SAS URI를 사용하여 [Azure Blob Storage](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage)에 ZIP 보관 파일을 업로드합니다. 다음과 같이 다양한 플랫폼에서 이 작업을 수행할 수 있는 여러 Azure 라이브러리가 있습니다.

    * [.NET용 Azure Storage Client Library](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-blobs)
    * [Java용 Azure Storage SDK](https://docs.microsoft.com/azure/storage/storage-java-how-to-use-blob-storage)
    * [Python용 Azure Storage SDK](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-blob-storage)

    다음 C# 코드 예제는 .NET용 Azure Storage Client Library의 [CloudBlockBlob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.aspx) 클래스를 사용하여 Azure Blob Storage에 ZIP 보관 파일을 업로드하는 방법을 보여 줍니다. 이 예제에서는 ZIP 보관 파일이 이미 스트림 개체에 기록되어 있다고 가정합니다.

    ```csharp
    string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";
    Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
      new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
    await blockBob.UploadFromStreamAsync(stream);
    ```

5. 다음 메서드를 실행하여 제출을 커밋합니다. 이 것을 경고 파트너 센터 제출을 완료 되 고 업데이트 계정에 지금 적용 해야 합니다. 자세한 내용은 [추가 기능 제출 커밋](commit-an-add-on-submission.md)을 참조하세요.

    ```
    POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/commit
    ```

6. 다음 메서드를 실행하여 커밋 상태를 확인합니다. 자세한 내용은 [추가 기능 제출의 상태 가져오기](get-status-for-an-add-on-submission.md)를 참조하세요.

    ```
    GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/status
    ```

    제출 상태를 확인하려면 응답 본문에서 *status* 값을 검토합니다. 이 값은 요청이 성공한 경우 **CommitStarted**에서 **PreProcessing**으로, 요청에 오류가 발생한 경우 **CommitFailed**로 변경됩니다. 오류가 있는 경우 *statusDetails* 필드에 오류에 대한 추가 정보가 포함됩니다.

7. 커밋이 성공적으로 완료되면 수집을 위해 제출이 스토어로 전송됩니다. 이전 메서드를 사용 하 여 또는 파트너 센터를 방문 하 여 제출 진행 상황을 모니터링할 계속 수 있습니다.

<span/>

## <a name="code-examples"></a>코드 예제

다음 문서는 여러 다른 프로그래밍 언어로 추가 기능 제출을 만드는 방법을 보여 주는 자세한 코드 예제를 제공합니다.

* [C# 코드 예제](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Java 코드 예제](java-code-examples-for-the-windows-store-submission-api.md)
* [Python 코드 예제](python-code-examples-for-the-windows-store-submission-api.md)

## <a name="storebroker-powershell-module"></a>StoreBroker PowerShell 모듈

Microsoft Store 제출 API를 직접 호출하는 대신 이 API 위에 명령줄 인터페이스를 구현하는 오픈 소스 PowerShell 모듈도 제공합니다. 이 모듈을 [StoreBroker](https://aka.ms/storebroker)라고 합니다. 이 모듈을 사용하여 Microsoft Store 제출 API를 직접 호출하는 대신 명령줄에서 앱, 플라이트 및 추가 기능 제출을 관리할 수 있습니다. 또는 소스에서 이 API를 호출하는 방법에 대한 예제를 더 찾아볼 수 있습니다. StoreBroker 모듈은 많은 자사 응용 프로그램이 스토어에 제출되는 기본 방식으로 Microsoft 내에서 많이 사용됩니다.

자세한 내용은 [GitHub의 StoreBroker 페이지](https://aka.ms/storebroker)를 참조하세요.

<span/>

## <a name="data-resources"></a>데이터 리소스

추가 기능 제출을 관리하는 Microsoft Store 제출 API 메서드는 다음과 같은 JSON 데이터 리소스를 사용합니다.

<span id="add-on-submission-object" />

### <a name="add-on-submission-resource"></a>추가 기능 제출 리소스

이 리소스는 추가 기능 제출에 대해 설명합니다.

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

| 값      | 유형   | 설명        |
|------------|--------|----------------------|
| id            | 문자열  | 제출의 ID입니다. 이 ID는 [추가 기능 제출 만들기](create-an-add-on-submission.md), [모든 추가 기능 가져오기](get-all-add-ons.md) 및 [추가 기능 가져오기](get-an-add-on.md) 요청에 대한 응답 데이터에서 사용할 수 있습니다. 파트너 센터에서 생성 된 제출에 대해이 ID는 또한 파트너 센터에서 제출 페이지의 URL에 사용할 수 있습니다.  |
| contentType           | 문자열  |  추가 기능에 제공된 [콘텐츠의 유형](../publish/enter-add-on-properties.md#content-type)입니다. 다음 값 중 하나일 수 있습니다. <ul><li>NotSet</li><li>BookDownload</li><li>EMagazine</li><li>ENewspaper</li><li>MusicDownload</li><li>MusicStream</li><li>OnlineDataStorage</li><li>VideoDownload</li><li>VideoStream</li><li>Asp</li><li>OnlineDownload</li></ul> |  
| 키워드           | 배열  | 추가 기능에 대한 [키워드](../publish/enter-add-on-properties.md#keywords)를 최대 10개까지 포함하는 문자열의 배열입니다. 앱에서 이러한 키워드를 사용하여 추가 기능을 쿼리할 수 있습니다.   |
| lifetime           | 문자열  |  추가 기능의 수명입니다. 다음 값 중 하나일 수 있습니다. <ul><li>Forever</li><li>OneDay</li><li>ThreeDays</li><li>FiveDays</li><li>OneWeek</li><li>TwoWeeks</li><li>OneMonth</li><li>TwoMonths</li><li>ThreeMonths</li><li>SixMonths</li><li>OneYear</li></ul> |
| listings           | object  |  키와 값 쌍의 사전입니다. 여기서 각 키는 두 자로 된 ISO 3166-1 alpha-2 국가 코드이며 각 값은 추가 기능에 대한 목록 정보를 포함하는 [목록 리소스](#listing-object)입니다.  |
| pricing           | object  | 추가 기능에 대한 가격 정보를 포함하는 [가격 리소스](#pricing-object)입니다.   |
| targetPublishMode           | 문자열  | 제출의 게시 모드입니다. 다음 값 중 하나일 수 있습니다. <ul><li>즉시</li><li>수동</li><li>SpecificDate</li></ul> |
| targetPublishDate           | 문자열  | *targetPublishMode*가 SpecificDate로 설정된 경우 제출의 게시 날짜(ISO 8601 형식)입니다.  |
| 태그           | 문자열  |  추가 기능에 대한 [사용자 지정 개발자 데이터](../publish/enter-add-on-properties.md#custom-developer-data)(이 정보를 이전에는 *태그*라고 지칭함)입니다.   |
| visibility  | 문자열  |  추가 기능의 표시 여부입니다. 다음 값 중 하나일 수 있습니다. <ul><li>Hidden</li><li>Public</li><li>개인 정보 보호</li><li>NotSet</li></ul>  |
| status  | 문자열  |  제출의 상태입니다. 다음 값 중 하나일 수 있습니다. <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>릴리스</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | object  |  오류에 대한 정보를 비롯하여 제출 상태에 대한 추가 세부 정보가 포함된 [상태 세부 정보 리소스](#status-details-object)입니다. |
| fileUploadUrl           | 문자열  | 제출에 대한 패키지를 업로드하기 위한 SAS(공유 액세스 서명) URI입니다. 제출에 대한 새 패키지를 추가하는 경우 패키지가 포함된 ZIP 보관 파일을 이 URI에 업로드합니다. 자세한 내용은 [추가 기능 제출 만들기](#create-an-add-on-submission)를 참조하세요.  |
| FriendlyName  | 문자열  |  파트너 센터에 표시 된 대로 제출의 식별 이름입니다. 이 값은 제출을 만들 때 생성됩니다.  |

<span id="listing-object" />

### <a name="listing-resource"></a>목록 리소스

이 리소스는 [추가 기능에 대한 목록 정보](../publish/create-add-on-store-listings.md)를 포함합니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 유형    | 설명       |
|-----------------|---------|------|
|  description               |    문자열     |   추가 기능 목록에 대한 설명입니다.   |     
|  icon               |   object      |추가 기능 목록의 아이콘에 대한 데이터를 포함하는 [아이콘 리소스](#icon-object)입니다.    |
|  title               |     문자열    |   추가 기능 목록의 제목입니다.   |  

<span id="icon-object" />

### <a name="icon-resource"></a>아이콘 리소스

이 리소스에는 추가 기능 목록에 대한 아이콘 데이터가 포함되어 있습니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 유형    | 설명     |
|-----------------|---------|------|
|  fileName               |    문자열     |   제출을 위해 업로드한 ZIP 보관 파일에 있는 아이콘 파일의 이름입니다. 아이콘은 정확히 300 x 300픽셀의 .png 파일이어야 합니다.   |     
|  fileStatus               |   문자열      |  아이콘 파일의 상태입니다. 다음 값 중 하나일 수 있습니다. <ul><li>None</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>   |

<span id="pricing-object" />

### <a name="pricing-resource"></a>가격 리소스

이 리소스는 추가 기능에 대한 가격 정보를 포함합니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 유형    | 설명    |
|-----------------|---------|------|
|  marketSpecificPricings               |    object     |  키와 값 쌍의 사전입니다. 여기서 각 키는 두 자로 된 ISO 3166-1 alpha-2 국가 코드이며 각 값은 [기준 가격](#price-tiers)입니다. 이러한 항목은 [특정 지역/국가에서 추가 기능에 대한 사용자 지정 가격](https://msdn.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability#markets-and-custom-prices)을 나타냅니다. 이 사전의 항목은 지정된 지역/국가의 *priceId* 값으로 지정된 기본 가격을 재정의합니다.     |     
|  sales               |   배열      |  **사용되지 않음**. 추가 기능에 대한 판매 정보를 포함하는 [판매 리소스](#sale-object) 배열입니다.     |     
|  priceId               |   문자열      |  추가 기능에 대한 [기본 가격](https://msdn.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability#base-price)을 지정하는 [기준 가격](#price-tiers)입니다.    |    
|  isAdvancedPricingModel               |   boolean      |  **true**인 경우, 개발자 계정은 .99 USD에서 1999.99 USD까지의 확장된 기준 가격 집합에 액세스할 수 있는 권한이 있습니다. **false**인 경우, 개발자 계정은 .99 USD에서 999.99 USD까지의 원래 기준 가격 집합에 액세스할 수 있는 권한이 있습니다. 여러 계층에 대한 자세한 내용은 [기준 가격](#price-tiers)을 참조하세요.<br/><br/>**참고**&nbsp;&nbsp;이 필드는 읽기 전용입니다.   |


<span id="sale-object" />

### <a name="sale-resource"></a>판매 리소스

이 리소스는 추가 기능에 대한 판매 정보를 포함합니다.

> [!IMPORTANT]
> **판매** 리소스는 더 이상 지원되지 않으며, 현재 Microsoft Store 제출 API를 사용하여 추가 기능 제출에 대한 판매 데이터를 가져오거나 수정할 수 없습니다. 앞으로 Microsoft Store 제출 API를 업데이트하여 추가 기능 제출에 대한 판매 정보에 프로그래밍 방식으로 액세스하는 새로운 방법을 도입할 예정입니다.
>    * [GET 메서드를 호출하여 추가 기능 제출을 가져오면](get-an-add-on-submission.md) *판매* 값이 빈 상태로 표시됩니다. 파트너 센터를 사용 하 여 추가 기능 제출에 대 한 판매 데이터를 가져올 계속 수 있습니다.
>    * [PUT 메서드를 호출하여 추가 기능 제출을 업데이트하는 경우](update-an-add-on-submission.md) *판매* 값의 정보는 무시됩니다. 추가 기능 제출에 대 한 판매 데이터를 변경 하려면 파트너 센터를 사용 하 여 계속 수 있습니다.

이 리소스의 값은 다음과 같습니다.

| 값           | 유형    | 설명           |
|-----------------|---------|------|
|  name               |    문자열     |   판매의 이름입니다.    |     
|  basePriceId               |   문자열      |  판매의 기본 가격으로 사용할 [기준 가격](#price-tiers)입니다.    |     
|  startDate               |   문자열      |   판매의 시작 날짜(ISO 8601 형식)입니다.  |     
|  endDate               |   문자열      |  판매의 종료 날짜(ISO 8601 형식)입니다.      |     
|  marketSpecificPricings               |   object      |   키와 값 쌍의 사전입니다. 여기서 각 키는 두 자로 된 ISO 3166-1 alpha-2 국가 코드이며 각 값은 [기준 가격](#price-tiers)입니다. 이러한 항목은 [특정 지역/국가에서 추가 기능에 대한 사용자 지정 가격](https://msdn.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability#markets-and-custom-pricess)을 나타냅니다. 이 사전의 항목은 지정된 지역/국가의 *basePriceId* 값으로 지정된 기본 가격을 재정의합니다.    |

<span id="status-details-object" />

### <a name="status-details-resource"></a>상태 세부 정보 리소스

이 리소스에는 제출 상태에 대한 추가 세부 정보가 포함됩니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 유형    | 설명       |
|-----------------|---------|------|
|  errors               |    object     |   제출에 대한 오류 세부 정보가 포함된 [상태 세부 정보 리소스](#status-detail-object)의 배열입니다.   |     
|  warnings               |   object      | 제출에 대한 경고 세부 정보가 포함된 [상태 세부 정보 리소스](#status-detail-object)의 배열입니다.     |
|  certificationReports               |     object    |   제출에 대한 인증 보고서 데이터에 대한 액세스를 제공하는 [인증 보고서 리소스](#certification-report-object)의 배열입니다. 인증에 실패할 경우 이러한 보고서에서 자세한 내용을 확인할 수 있습니다.    |  

<span id="status-detail-object" />

### <a name="status-detail-resource"></a>상태 세부 정보 리소스

이 리소스에는 제출과 관련된 오류 또는 경고에 대한 추가 정보가 포함되어 있습니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 유형    | 설명    |
|-----------------|---------|------|
|  code               |    문자열     |   오류 또는 경고의 유형을 설명하는 [제출 상태 코드](#submission-status-code)입니다.   |     
|  details               |     문자열    |  문제에 대한 자세한 정보가 있는 메시지입니다.     |

<span id="certification-report-object" />

### <a name="certification-report-resource"></a>인증 보고서 리소스

이 리소스는 제출의 인증 보고서 데이터에 대한 액세스를 제공합니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 유형    | 설명               |
|-----------------|---------|------|
|     date            |    string     |  날짜 및 시간 보고서 생성 된 ISO 8601 형식에서입니다.    |
|     reportUrl            |    문자열     |  보고서에 액세스할 수 있는 URL입니다.    |

## <a name="enums"></a>열거

이러한 메서드는 다음 열거형을 사용합니다.

<span id="price-tiers" />

### <a name="price-tiers"></a>기준 가격

다음 값은 추가 기능 제출에 대한 [가격 리소스](#pricing-object) 리소스에서 사용할 수 있는 기준 가격을 나타냅니다.

| 값           | 설명       |
|-----------------|------|
|  기본               |   기준 가격이 설정되지 않았습니다. 추가 기능에 대한 기본 가격을 사용합니다.      |     
|  NotAvailable              |   지정된 영역에 추가 기능을 사용할 수 없습니다.    |     
|  무료              |   추가 기능은 무료입니다.    |    
|  계층*xxxx*               |   추가 기능에 대한 기준 가격을 **계층<em>xxxx</em>** 형식으로 지정하는 문자열입니다. 현재 다음 범위의 기준 가격이 지원됩니다.<br/><br/><ul><li>[가격 리소스](#pricing-object)의 *isAdvancedPricingModel* 값이 **true**인 경우, 사용자 계정에 대해 사용 가능한 기준 가격 값은 **Tier1012** - **Tier1424**입니다.</li><li>[가격 리소스](#pricing-object)의 *isAdvancedPricingModel* 값이 **false**인 경우, 사용자 계정에 대해 사용 가능한 기준 가격 값은 **Tier2** - **Tier96**입니다.</li></ul>각 계층과 관련 된 시장별 가격을 포함 하 여, 개발자 계정에 사용할 수 있는 계층 가격의 전체 표를 보려면 파트너 센터에서 앱 제출에 대 한 **가격 책정 및 가용성** 페이지로 이동 하 고 **지역/국가 및 사용자 지정 가격** 섹션의 **표 보기** 링크를 클릭 (일부 개발자 계정의 경우,이 링크는 **가격 책정** 섹션에서).     |

<span id="submission-status-code" />

### <a name="submission-status-code"></a>제출 상태 코드

다음 값은 제출의 상태 코드를 나타냅니다.

| 값           |  설명      |
|-----------------|---------------|
|  None            |     코드가 지정되지 않았습니다.         |     
|      InvalidArchive        |     패키지가 포함된 ZIP 보관 파일이 잘못되거나 인식할 수 없는 보관 파일 형식입니다.  |
| MissingFiles | 제출 데이터에 나열된 모든 파일이 ZIP 보관 파일에 없거나 보관 파일에서 잘못된 위치에 있습니다. |
| PackageValidationFailed | 제출에서 하나 이상의 패키지가 유효성 검사에 실패했습니다. |
| InvalidParameterValue | 요청 본문의 매개 변수 중 하나가 잘못되었습니다. |
| InvalidOperation | 시도한 작업이 유효하지 않습니다. |
| InvalidState | 시도한 작업이 패키지 플라이트의 현재 상태에 적합하지 않습니다. |
| ResourceNotFound | 지정된 패키지 플라이트를 찾을 수 없습니다. |
| ServiceError | 내부 서비스 오류가 발생하여 요청이 실패했습니다. 요청을 다시 시도하세요. |
| ListingOptOutWarning | 개발자가 이전 제출에서 목록을 제거하거나 패키지에서 지원되는 목록 정보를 포함하지 않았습니다. |
| ListingOptInWarning  | 개발자가 목록을 추가했습니다. |
| UpdateOnlyWarning | 개발자가 업데이트만 지원되는 항목을 삽입하려고 합니다. |
| 기타  | 제출이 인식할 수 없거나 분류되지 않은 상태입니다. |
| PackageValidationWarning | 패키지 유효성 검사 프로세스에서 경고가 발생했습니다. |

<span/>

## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)
* [Microsoft Store 제출 API를 사용하여 추가 기능 관리](manage-add-ons.md)
* [파트너 센터에서 추가 기능 제출](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions)
