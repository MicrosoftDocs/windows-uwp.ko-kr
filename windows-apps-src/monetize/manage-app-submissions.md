---
ms.assetid: C7428551-4B31-4259-93CD-EE229007C4B8
description: Microsoft Store 제출 API에서 이러한 메서드를 사용 하 여 파트너 센터 계정에 등록 된 앱에 대 한 제출을 관리할 수 있습니다.
title: 앱 제출 관리
ms.date: 04/30/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 앱 서브 미션
ms.localizationpriority: medium
ms.openlocfilehash: de612607da2192af3358c94874e0896557ca6d08
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171367"
---
# <a name="manage-app-submissions"></a>앱 제출 관리

Microsoft Store 제출 API는 점진적 패키지 롤아웃을 비롯 하 여 앱에 대 한 제출을 관리 하는 데 사용할 수 있는 메서드를 제공 합니다. API를 사용 하기 위한 필수 구성 요소를 포함 하 여 Microsoft Store 제출 API에 대 한 소개는 [Microsoft Store services를 사용 하 여 전송 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)를 참조 하세요.

> [!IMPORTANT]
> Microsoft Store 제출 API를 사용 하 여 앱에 대 한 제출을 만드는 경우 파트너 센터가 아닌 API를 사용 하 여 제출을 추가로 변경 해야 합니다. 파트너 센터를 사용 하 여 원래 API를 사용 하 여 만든 제출을 변경 하는 경우 API를 사용 하 여 해당 제출을 변경 하거나 커밋할 수 없습니다. 경우에 따라 제출 프로세스에서 계속할 수 없는 오류 상태로 제출 될 수 있습니다. 이 문제가 발생 하는 경우 제출을 삭제 하 고 새 제출을 만들어야 합니다.

> [!IMPORTANT]
> 이 API를 사용 하 여 비즈니스에 대 한 [Microsoft Store 및 교육용 Microsoft Store를 통해 대량 구매](../publish/organizational-licensing.md) 를 위한 제출을 게시 하거나 [LOB 앱](../publish/distribute-lob-apps-to-enterprises.md) 에 대 한 제출을 엔터프라이즈에 직접 게시할 수 없습니다. 이러한 두 시나리오 모두에 대해 파트너 센터를 사용 하 여 제출을 게시 해야 합니다.


<span id="methods-for-app-submissions" />

## <a name="methods-for-managing-app-submissions"></a>앱 제출을 관리 하는 방법

다음 방법을 사용 하 여 앱 제출을 가져오거나, 만들고, 업데이트 하 고, 커밋하거나, 삭제할 수 있습니다. 이러한 방법을 사용 하려면 먼저 파트너 센터 계정에 앱이 있어야 하 고 파트너 센터에서 앱에 대 한 제출을 하나 만들어야 합니다. 자세한 내용은 [필수 구성 요소](create-and-manage-submissions-using-windows-store-services.md#prerequisites)를 참조 하세요.

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
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}</td>
<td align="left"><a href="get-an-app-submission.md">기존 앱 제출 가져오기</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status</td>
<td align="left"><a href="get-status-for-an-app-submission.md">기존 앱 제출의 상태를 가져옵니다.</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions</td>
<td align="left"><a href="create-an-app-submission.md">새 앱 제출 만들기</a></td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}</td>
<td align="left"><a href="update-an-app-submission.md">기존 앱 제출 업데이트</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit</td>
<td align="left"><a href="commit-an-app-submission.md">신규 또는 업데이트 된 앱 제출을 커밋합니다.</a></td>
</tr>
<tr>
<td align="left">Delete</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}</td>
<td align="left"><a href="delete-an-app-submission.md">앱 제출 삭제</a></td>
</tr>
</tbody>
</table>

<span id="create-an-app-submission">

### <a name="create-an-app-submission"></a>앱 제출 만들기

앱에 대 한 제출을 만들려면이 프로세스를 수행 합니다.

1. 아직 수행 하지 않은 경우 Microsoft Store 제출 API에 대 한 모든 [필수 구성 요소](create-and-manage-submissions-using-windows-store-services.md#prerequisites) 를 완료 합니다.
    > [!NOTE]
    > 앱에 [보존 기간 등급](../publish/age-ratings.md) 정보가 포함 된 완료 된 제출이 이미 하나 이상 있는지 확인 합니다.

2. [AZURE AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Microsoft Store 제출 API의 메서드에이 액세스 토큰을 전달 해야 합니다. 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후 새 토큰을 얻을 수 있습니다.

3. Microsoft Store 제출 API에서 다음 메서드를 실행 하 여 [앱 제출을 만듭니다](create-an-app-submission.md) . 이 메서드는 마지막으로 게시 된 제출의 복사본 인 진행 중인 새 제출을 만듭니다.

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions
    ```

    응답 본문에는 새 제출의 ID, Azure Blob storage에 전송 하기 위한 모든 관련 파일을 업로드 하기 위한 SAS (공유 액세스 서명) URI (예: 앱 패키지, 목록 이미지 및 트레일러 파일) 및 새 전송에 대 한 모든 데이터 (예: 목록 및 가격 정보)를 포함 하는 [앱 제출](#app-submission-object) 리소스가 포함 되어 있습니다.

    > [!NOTE]
    > SAS URI는 계정 키를 요구 하지 않고 Azure storage의 보안 리소스에 대 한 액세스를 제공 합니다. SAS Uri 및 Azure Blob storage에서의 사용에 대 한 배경 정보는 [공유 액세스 서명, 1 부: sas 모델](/azure/storage/common/storage-sas-overview) 및 [공유 액세스 서명 이해, 2 부: Blob 저장소를 사용 하 여 sas 만들기 및 사용](/azure/storage/common/storage-sas-overview)을 참조 하세요.

4. 제출에 대 한 새 패키지, 목록 이미지 또는 트레일러 파일을 추가 하는 경우 [앱 패키지를 준비](../publish/app-package-requirements.md) 하 고 [앱 스크린샷, 이미지 및 트레일러를 준비](../publish/app-screenshots-and-images.md)합니다. 이러한 모든 파일을 ZIP 보관 파일에 추가 합니다.

5. 새 제출을 위해 필요한 변경 내용을 사용 하 여 [앱 제출](#app-submission-object) 데이터를 수정 하 고 다음 메서드를 실행 하 여 [앱 제출을 업데이트](update-an-app-submission.md)합니다.

    ```json
    PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}
    ```
      > [!NOTE]
      > 제출에 대 한 새 파일을 추가 하는 경우 ZIP 보관 파일에 있는 이러한 파일의 이름 및 상대 경로를 참조 하도록 제출 데이터를 업데이트 해야 합니다.

4. 전송에 대 한 새 패키지, 목록 이미지 또는 트레일러 파일을 추가 하는 경우 앞에서 호출한 POST 메서드의 응답 본문에 제공 된 SAS URI를 사용 하 여 ZIP 보관 파일을 [Azure Blob 저장소](/azure/storage/storage-introduction#blob-storage) 에 업로드 합니다. 다음과 같은 다양 한 플랫폼에서이 작업을 수행 하는 데 사용할 수 있는 다양 한 Azure 라이브러리가 있습니다.

    * [.NET용 Azure Storage 클라이언트 라이브러리](/azure/storage/storage-dotnet-how-to-use-blobs)
    * [Java용 Azure Storage SDK](/azure/storage/storage-java-how-to-use-blob-storage)
    * [Python 용 Azure Storage SDK](/azure/storage/storage-python-how-to-use-blob-storage)

    다음 c # 코드 예제에서는 .NET 용 Azure Storage 클라이언트 라이브러리에서 [Cloudblockblob](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob) 클래스를 사용 하 여 ZIP 보관 파일을 Azure Blob storage에 업로드 하는 방법을 보여 줍니다. 이 예에서는 ZIP 보관 파일이 스트림 개체에 이미 기록 된 것으로 가정 합니다.

    ```csharp
    string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";
    Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
        new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
    await blockBob.UploadFromStreamAsync(stream);
    ```

5. 다음 메서드를 실행 하 여 [앱 제출을 커밋합니다](commit-an-app-submission.md) . 그러면 전송 작업을 수행 하 고 사용자의 계정에 업데이트가 적용 되도록 파트너 센터에 경고를 표시 합니다.

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit
    ```

6. [앱 제출 상태를 가져오려면](get-status-for-an-app-submission.md)다음 방법을 실행 하 여 커밋 상태를 확인 합니다.

    ```json
    GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status
    ```

    제출 상태를 확인 하려면 응답 본문에서 *상태* 값을 검토 합니다. 요청에 오류가 있는 경우 요청이 성공 하거나 **Commitstarted가 실패** 한 경우이 값은 **Commitstarted** 에서 **전처리** 로 변경 해야 합니다. 오류가 있는 경우 *statusdetails* 필드에 오류에 대 한 추가 정보가 포함 됩니다.

7. 커밋이 성공적으로 완료 되 면 수집을 위해 스토어로 전송 됩니다. 이전 방법을 사용 하거나 파트너 센터를 방문 하 여 제출 진행 상황을 계속 모니터링할 수 있습니다.

<span id="manage-gradual-package-rollout">

## <a name="methods-for-managing-a-gradual-package-rollout"></a>점진적 패키지 출시를 관리 하는 방법

앱 제출의 업데이트 된 패키지를 Windows 10에 대 한 앱 고객의 비율로 점진적으로 롤아웃할 수 있습니다. 이렇게 하면 특정 패키지에 대 한 사용자 의견 및 분석 데이터를 모니터링 하 여 업데이트에 대 한 확신을 확인할 수 있습니다. 새 제출을 만들지 않고도 게시 된 제출에 대 한 출시 백분율 (또는 업데이트 중단)을 변경할 수 있습니다. 파트너 센터에서 점진적 패키지 출시를 사용 하도록 설정 하 고 관리 하는 방법에 대 한 지침을 비롯 한 자세한 내용은 [이 문서](../publish/gradual-package-rollout.md)를 참조 하세요.

앱 제출을 위한 점진적 패키지 출시를 프로그래밍 방식으로 사용 하도록 설정 하려면 Microsoft Store 제출 API의 메서드를 사용 하 여이 프로세스를 수행 합니다.

  1. [앱 제출을 만들거나](create-an-app-submission.md) [기존 앱 제출을 가져옵니다](get-an-app-submission.md).
  2. 응답 데이터에서 [packageRollout](#package-rollout-object) 리소스를 찾은 다음 *isPackageRollout* 필드를 **True**로 설정 하 고 *packageRolloutPercentage* 필드를 업데이트 된 패키지를 가져와야 하는 앱 고객의 백분율로 설정 합니다.
  3. 업데이트 된 앱 제출 데이터를 [앱 전송 업데이트](update-an-app-submission.md) 방법으로 전달 합니다.

앱 제출을 위한 점진적 패키지 출시를 사용 하도록 설정한 후에는 다음 방법을 사용 하 여 점진적 출시를 프로그래밍 방식으로 가져오기, 업데이트, 중지 또는 종료할 수 있습니다.

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
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/packagerollout</td>
<td align="left"><a href="get-package-rollout-info-for-an-app-submission.md">앱 제출에 대 한 점진적 출시 정보 가져오기</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/updatepackagerolloutpercentage</td>
<td align="left"><a href="update-the-package-rollout-percentage-for-an-app-submission.md">앱 제출에 대한 점진적 출시 백분율 업데이트</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/haltpackagerollout</td>
<td align="left"><a href="halt-the-package-rollout-for-an-app-submission.md">앱 제출을 위한 점진적 롤아웃 중지</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/finalizepackagerollout</td>
<td align="left"><a href="finalize-the-package-rollout-for-an-app-submission.md">앱 전송에 대 한 점진적 출시 마무리</a></td>
</tr>
</tbody>
</table>


## <a name="code-examples-for-managing-app-submissions"></a>앱 서브 미션을 관리 하기 위한 코드 예제

다음 문서에서는 다양 한 프로그래밍 언어로 앱 제출을 만드는 방법을 보여 주는 자세한 코드 예제를 제공 합니다.

* [C# 샘플: 앱, 추가 기능 및 플라이트 제출](csharp-code-examples-for-the-windows-store-submission-api.md)
* [C# 샘플: 게임 옵션과 예고편이 포함된 앱 제출](csharp-code-examples-for-submissions-game-options-and-trailers.md)
* [Java 샘플: 앱, 추가 기능 및 플라이트 제출](java-code-examples-for-the-windows-store-submission-api.md)
* [Java 샘플: 게임 옵션과 예고편이 포함된 앱 제출](java-code-examples-for-submissions-game-options-and-trailers.md)
* [Python 샘플: 앱, 추가 기능 및 플라이트 제출](python-code-examples-for-the-windows-store-submission-api.md)
* [Python 샘플: 게임 옵션과 예고편이 포함된 앱 제출](python-code-examples-for-submissions-game-options-and-trailers.md)

## <a name="storebroker-powershell-module"></a>을 (를) Broker PowerShell 모듈

Microsoft Store 제출 API를 직접 호출 하는 대신 API를 기반으로 명령줄 인터페이스를 구현 하는 오픈 소스 PowerShell 모듈을 제공 합니다. 이 모듈을이 모듈을이 모듈 [이라고 합니다.](https://github.com/Microsoft/StoreBroker) 이 모듈을 사용 하 여 Microsoft Store 제출 API를 직접 호출 하는 대신 명령줄에서 앱, 비행 및 추가 기능 제출을 관리 하거나 단순히 소스를 검색 하 여이 API를 호출 하는 방법에 대 한 더 많은 예제를 볼 수 있습니다. 여러 자사 응용 프로그램을 저장소에 제출 하는 기본 방법으로 Microsoft 내에서이 기능을 사용 합니다.

자세한 내용은 [GitHub의 microsoft 컴퓨터 브로커 페이지](https://github.com/Microsoft/StoreBroker)를 참조 하세요.

<span/>

## <a name="data-resources"></a>데이터 리소스
앱 제출을 관리 하기 위한 Microsoft Store 제출 API 메서드는 다음 JSON 데이터 리소스를 사용 합니다.

<span id="app-submission-object" />

### <a name="app-submission-resource"></a>앱 제출 리소스

이 리소스는 앱 제출을 설명 합니다.

```json
{
  "id": "1152921504621243540",
  "applicationCategory": "BooksAndReference_EReader",
  "pricing": {
    "trialPeriod": "FifteenDays",
    "marketSpecificPricings": {},
    "sales": [],
    "priceId": "Tier2",
    "isAdvancedPricingModel": true
  },
  "visibility": "Public",
  "targetPublishMode": "Manual",
  "targetPublishDate": "1601-01-01T00:00:00Z",
  "listings": {
    "en-us": {
      "baseListing": {
        "copyrightAndTrademarkInfo": "",
        "keywords": [
          "epub"
        ],
        "licenseTerms": "",
        "privacyPolicy": "",
        "supportContact": "",
        "websiteUrl": "",
        "description": "Description",
        "features": [
          "Free ebook reader"
        ],
        "releaseNotes": "",
        "images": [
          {
            "fileName": "contoso.png",
            "fileStatus": "Uploaded",
            "id": "1152921504672272757",
            "description": "Main page",
            "imageType": "Screenshot"
          }
        ],
        "recommendedHardware": [],
        "title": "Contoso ebook reader"
      },
      "platformOverrides": {
        "Windows81": {
          "description": "Ebook reader for Windows 8.1"
        }
      }
    }
  },
  "hardwarePreferences": [
    "Touch"
  ],
  "automaticBackupEnabled": false,
  "canInstallOnRemovableMedia": true,
  "isGameDvrEnabled": false,
  "gamingOptions": [],
  "hasExternalInAppProducts": false,
  "meetAccessibilityGuidelines": true,
  "notesForCertification": "",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/387a9ea8-a412-43a9-8fb3-a38d03eb483d?sv=2014-02-14&sr=b&sig=sdd12JmoaT6BhvC%2BZUrwRweA%2Fkvj%2BEBCY09C2SZZowg%3D&se=2016-06-17T18:32:26Z&sp=rwl",
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
      "fileStatus": "Uploaded",
      "id": "1152921504620138797",
      "version": "1.0.0.0",
      "architecture": "ARM",
      "languages": [
        "en-US"
      ],
      "capabilities": [
        "ID_RESOLUTION_HD720P",
        "ID_RESOLUTION_WVGA",
        "ID_RESOLUTION_WXGA"
      ],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None",
      "targetDeviceFamilies": [
        "Windows.Mobile min version 10.0.10240.0"
      ]
    }
  ],
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0.0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
  "enterpriseLicensing": "Online",
  "allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies": true,
  "allowTargetFutureDeviceFamilies": {
    "Desktop": false,
    "Mobile": true,
    "Holographic": true,
    "Xbox": false,
    "Team": true
  },
  "friendlyName": "Submission 2",
  "trailers": []
}
```

이 리소스의 값은 다음과 같습니다.

| 값      | 형식   | Description      |
|------------|--------|-------------------|
| id            | 문자열  | 제출 ID입니다. 이 ID는 [앱 제출을 만들고](create-an-app-submission.md) [모든 앱을 가져오고](get-all-apps.md) [앱을 가져오는](get-an-app.md)요청에 대 한 응답 데이터에서 사용할 수 있습니다. 파트너 센터에서 만든 제출의 경우이 ID는 파트너 센터의 제출 페이지에 대 한 URL 에서도 사용할 수 있습니다.  |
| applicationCategory           | 문자열  |   앱에 대 한 [범주 및/또는 하위](../publish/category-and-subcategory-table.md) 범주를 지정 하는 문자열입니다. 범주와 하위 범주는 **BooksAndReference_EReader**와 같이 밑줄 ' _ ' 문자를 사용 하 여 단일 문자열로 결합 됩니다.      |  
| 가격 책정           |  개체  | 앱에 대 한 가격 정보를 포함 하는 [가격 책정 리소스](#pricing-object) 입니다.        |   
| 표시 여부           |  문자열  |  앱의 표시 여부입니다. 다음 값 중 하나일 수 있습니다. <ul><li>숨김</li><li>공용</li><li>프라이빗</li><li>NotSet</li></ul>       |   
| targetPublishMode           | 문자열  | 제출에 대 한 게시 모드입니다. 다음 값 중 하나일 수 있습니다. <ul><li>즉시</li><li>설명서</li><li>SpecificDate</li></ul> |
| Target버전           | 문자열  | *TargetSpecificDate mode* 가로 설정 된 경우 ISO 8601 형식의 제출에 대 한 게시 날짜입니다.  |  
| 목록           |   개체  |  키/값 쌍의 사전입니다. 여기서 각 키는 국가 코드이 고 각 값은 앱에 대 한 목록 정보를 포함 하는 [목록 리소스](#listing-object) 입니다.       |   
| hardwarePreferences           |  array  |   앱에 대 한 [하드웨어 기본 설정을](../publish/enter-app-properties.md) 정의 하는 문자열 배열입니다. 다음 값 중 하나일 수 있습니다. <ul><li>터치</li><li>키보드</li><li>마우스</li><li>카메라</li><li>NfcHce</li><li>Reader</li><li>BluetoothLE</li><li>Telephony</li></ul>     |   
| 자동 Backup사용           |  boolean  |   Windows에서 OneDrive에 자동 백업에 앱의 데이터를 포함할 수 있는지 여부를 나타냅니다. 자세한 내용은 [앱 선언](../publish/product-declarations.md)을 참조 하세요.   |   
| canInstallOnRemovableMedia           |  boolean  |   고객이 이동식 저장소에 앱을 설치할 수 있는지 여부를 나타냅니다. 자세한 내용은 [앱 선언](../publish/product-declarations.md)을 참조 하세요.     |   
| isGameDvrEnabled           |  boolean |   앱에 게임 DVR이 사용 되는지 여부를 나타냅니다.    |   
| gamingOptions           |  array |   앱에 대 한 게임 관련 설정을 정의 하는 [게임 옵션 리소스](#gaming-options-object) 하나를 포함 하는 배열입니다.     |   
| hasExternalInAppProducts           |     boolean          |   앱을 통해 사용자가 Microsoft Store 상거래 시스템 외부에서 구매할 수 있는지 여부를 나타냅니다. 자세한 내용은 [앱 선언](../publish/product-declarations.md)을 참조 하세요.     |   
| meetAccessibilityGuidelines           |    boolean           |  앱이 내게 필요한 옵션 지침을 충족 하는지 테스트 되었는지 여부를 나타냅니다. 자세한 내용은 [앱 선언](../publish/product-declarations.md)을 참조 하세요.      |   
| 메모 For인증           |  문자열  |   앱에 대 한 [인증에 대 한 정보를](../publish/notes-for-certification.md) 포함 합니다.    |    
| 상태           |   문자열  |  제출의 상태입니다. 다음 값 중 하나일 수 있습니다. <ul><li>없음</li><li>취소됨</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>게시</li><li>게시 날짜</li><li>이상 실패</li><li>바꾸면</li><li>PreProcessingFailed</li><li>인증</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>      |    
| statusDetails           |   개체  | 오류에 대 한 정보를 포함 하 여 제출 상태에 대 한 추가 정보를 포함 하는 [상태 세부 정보 리소스](#status-details-object) 입니다.       |    
| fileUploadUrl           |   문자열  | 제출할 패키지를 업로드 하기 위한 SAS (공유 액세스 서명) URI입니다. 제출에 대 한 새 패키지, 목록 이미지 또는 트레일러 파일을 추가 하는 경우 패키지 및 이미지를 포함 하는 ZIP 보관 파일을이 URI에 업로드 합니다. 자세한 내용은 [앱 제출 만들기](#create-an-app-submission)를 참조 하세요.       |    
| applicationPackages           |   array  | 제출의 각 패키지에 대 한 세부 정보를 제공 하는 [응용 프로그램 패키지 리소스](#application-package-object) 의 배열입니다. |    
| packageDeliveryOptions    | 개체  | 전송에 대 한 점진적 패키지 롤아웃 및 필수 업데이트 설정을 포함 하는 [패키지 배달 옵션 리소스](#package-delivery-options-object) 입니다.  |
| enterpriseLicensing           |  문자열  |  앱에 대 한 엔터프라이즈 라이선스 동작을 나타내는 [엔터프라이즈 라이선스 값](#enterprise-licensing) 값 중 하나입니다.  |    
| allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies           |  boolean   |  Microsoft에서 [이후 Windows 10 장치 제품군에 앱을 사용할 수 있도록](../publish/set-app-pricing-and-availability.md)허용할지 여부를 나타냅니다.    |    
| allowTargetFutureDeviceFamilies           | 개체   |  키/값 쌍의 사전입니다. 각 키는 [Windows 10 장치 패밀리](../publish/set-app-pricing-and-availability.md) 이 고 각 값은 앱이 지정 된 장치 패밀리를 대상으로 할 수 있는지 여부를 나타내는 부울입니다.     |    
| friendlyName           |   문자열  |  파트너 센터에 표시 된 것 처럼 전송의 이름입니다. 이 값은 제출을 만들 때 생성 됩니다.       |  
| 부           |  array |   앱 목록에 대 한 비디오 트레일러를 나타내는 최대 15 개의 [트레일러 리소스](#trailer-object) 를 포함 하는 배열입니다.<br/><br/>   |  


<span id="pricing-object" />

### <a name="pricing-resource"></a>가격 책정 리소스

이 리소스에는 앱에 대 한 가격 정보가 포함 되어 있습니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 형식    | Description        |
|-----------------|---------|------|
|  trialPeriod               |    문자열     |  앱의 평가 기간을 지정 하는 문자열입니다. 다음 값 중 하나일 수 있습니다. <ul><li>NoFreeTrial</li><li>OneDay</li><li>TrialNeverExpires</li><li>SevenDays</li><li>FifteenDays</li><li>ThirtyDays</li></ul>    |
|  marketSpecificPricings               |    개체     |  키/값 쌍의 사전입니다. 여기서 각 키는 두 문자로 된 ISO 3166-1 알파 2 국가 코드이 고 각 값은 [가격 책정 계층](#price-tiers)입니다. 이러한 항목은 [특정 시장의 앱에 대 한 사용자 지정 가격](../publish/define-market-selection.md)을 나타냅니다. 이 사전의 모든 항목은 지정 된 시장에 대해 *priceId* 값으로 지정 된 기본 가격을 재정의 합니다.      |     
|  sales               |   array      |  **사용 되지 않습니다**. 앱에 대 한 판매 정보를 포함 하는 [판매 리소스](#sale-object) 의 배열입니다.   |     
|  priceId               |   문자열      |  앱에 대 한 [기본 가격](../publish/define-market-selection.md) 을 지정 하는 [가격 책정 계층](#price-tiers) 입니다.   |     
|  isAdvancedPricingModel               |   boolean      |  **True**이면 개발자 계정에서 99 usd부터 1999.99 usd 까지의 확장 된 가격 책정 계층 집합에 액세스할 수 있습니다. **False**이면 개발자 계정에서 99 usd에서 999.99 usd 까지의 원래 가격 책정 계층에 액세스할 수 있습니다. 여러 계층에 대 한 자세한 내용은 가격 책정 [계층](#price-tiers)을 참조 하세요.<br/><br/>**Note** &nbsp; 참고 &nbsp; 이 필드는 읽기 전용입니다.   |


<span id="sale-object" />

### <a name="sale-resource"></a>판매 리소스

이 리소스에는 앱에 대 한 판매 정보가 포함 되어 있습니다.

> [!IMPORTANT]
> **판매** 리소스는 더 이상 지원 되지 않으며 현재 MICROSOFT STORE 제출 API를 사용 하 여 앱 제출에 대 한 판매 데이터를 가져오거나 수정할 수 없습니다. 향후에는 앱 제출을 위한 판매 정보에 프로그래밍 방식으로 액세스 하는 새로운 방법을 도입 하기 위해 Microsoft Store 제출 API를 업데이트할 예정입니다.
>    * [Get 메서드를 호출 하 여 앱 제출을 가져온](get-an-app-submission.md)후 *판매* 값이 비어 있게 됩니다. 파트너 센터를 계속 사용 하 여 앱 제출에 대 한 판매 데이터를 가져올 수 있습니다.
>    * [PUT 메서드를 호출 하 여 앱 제출을 업데이트할](update-an-app-submission.md)때 *sales* 값의 정보는 무시 됩니다. 파트너 센터를 계속 사용 하 여 앱 제출에 대 한 판매 데이터를 변경할 수 있습니다.

이 리소스의 값은 다음과 같습니다.

| 값           | 형식    | Description    |
|-----------------|---------|------|
|  name               |    문자열     |   판매의 이름입니다.    |     
|  basePriceId               |   문자열      |  판매의 기본 가격에 사용할 [가격 계층](#price-tiers) 입니다.    |     
|  startDate               |   문자열      |   ISO 8601 형식의 판매에 대 한 시작 날짜입니다.  |     
|  endDate               |   문자열      |  ISO 8601 형식의 판매에 대 한 종료 날짜입니다.      |     
|  marketSpecificPricings               |   개체      |   키/값 쌍의 사전입니다. 여기서 각 키는 두 문자로 된 ISO 3166-1 알파 2 국가 코드이 고 각 값은 [가격 책정 계층](#price-tiers)입니다. 이러한 항목은 [특정 시장의 앱에 대 한 사용자 지정 가격](../publish/define-market-selection.md)을 나타냅니다. 이 사전의 모든 항목은 지정 된 시장에 대해 *basePriceId* 값으로 지정 된 기본 가격을 재정의 합니다.    |


<span id="listing-object" />

### <a name="listing-resource"></a>리소스 나열

이 리소스에는 앱에 대 한 나열 정보가 포함 되어 있습니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 형식    | Description                  |
|-----------------|---------|------|
|  baseListing               |   개체      |  모든 플랫폼에 대 한 기본 목록 정보를 정의 하는 앱에 대 한 [기본 목록](#base-listing-object) 정보입니다.   |     
|  platformOverrides               | 개체 |   각 키가 목록 정보를 재정의할 플랫폼을 식별 하는 문자열이 고, 각 값은 지정 된 플랫폼에 대해 재정의할 목록 정보를 지정 하는 [기본 목록](#base-listing-object) 리소스 (description에서 제목의 값만 포함) 인 키 및 값 쌍의 사전입니다. 키에는 다음 값을 사용할 수 있습니다. <ul><li>Unknown</li><li>Package.windows80.appxmanifest</li><li>Windows81</li><li>WindowsPhone71</li><li>WindowsPhone80</li><li>WindowsPhone81</li></ul>     |      |     

<span id="base-listing-object" />

### <a name="base-listing-resource"></a>기본 목록 리소스

이 리소스는 앱에 대 한 기본 목록 정보를 포함 합니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 형식    | Description       |
|-----------------|---------|------|
|  copyrightAndTrademarkInfo                |   문자열      |  선택적 [저작권 및/또는 상표 정보](../publish/create-app-store-listings.md)입니다.  |
|  키워드                |  array       |  앱이 검색 결과에 표시 되는 데 도움이 되는 [키워드](../publish/create-app-store-listings.md) 의 배열입니다.    |
|  licenseTerms                |    문자열     | 앱에 대 한 선택적 [사용 조건](../publish/create-app-store-listings.md) 입니다.     |
|  privacyPolicy                |   문자열      |   이 변수는 오래된 것입니다. 앱에 대 한 개인 정보 보호 정책 URL을 설정 하거나 변경 하려면 파트너 센터의 [속성](../publish/enter-app-properties.md#privacy-policy-url) 페이지에서이 작업을 수행 해야 합니다. 제출 API에 대 한 호출에서이 값을 생략할 수 있습니다. 이 값을 설정 하면 무시 됩니다.       |
|  지원 연락처                |   문자열      |  이 변수는 오래된 것입니다. 앱에 대 한 지원 연락처 URL 또는 메일 주소를 설정 하거나 변경 하려면 파트너 센터의  [속성](../publish/enter-app-properties.md#support-contact-info) 페이지에서이 작업을 수행 해야 합니다. 제출 API에 대 한 호출에서이 값을 생략할 수 있습니다. 이 값을 설정 하면 무시 됩니다.        |
|  websiteUrl                |   문자열      |  이 변수는 오래된 것입니다. 앱에 대 한 웹 페이지의 URL을 설정 하거나 변경 하려면 파트너 센터의  [속성](../publish/enter-app-properties.md#website) 페이지에서이 작업을 수행 해야 합니다. 제출 API에 대 한 호출에서이 값을 생략할 수 있습니다. 이 값을 설정 하면 무시 됩니다.      |    
|  description               |    문자열     |   앱 목록에 대 한 [설명](../publish/create-app-store-listings.md) 입니다.   |     
|  기능               |    array     |  앱에 대 한 [기능](../publish/create-app-store-listings.md) 을 나열 하는 최대 20 개의 문자열로 구성 된 배열입니다.     |
|  releaseNotes               |  문자열       |  앱에 대 한 [릴리스 정보](../publish/create-app-store-listings.md) 입니다.    |
|  images               |   array      |  앱 목록에 대 한 [이미지 및 아이콘](#image-object) 리소스의 배열입니다.  |
|  recommendedHardware               |   array      |  앱에 [권장 되는 하드웨어 구성을](../publish/create-app-store-listings.md#additional-information) 나열 하는 최대 11 개의 문자열로 구성 된 배열입니다.     |
|  이상 하드웨어               |     문자열    |  앱에 대 한 [최소 하드웨어 구성을](../publish/create-app-store-listings.md#additional-information) 나열 하는 최대 11 개의 문자열로 구성 된 배열입니다.    |  
|  title               |     문자열    |   앱 목록에 대 한 제목입니다.   |  
|  shortDescription               |     문자열    |  게임에만 사용 됩니다. 이 설명은 Xbox One의 게임 허브 **정보** 섹션에 표시 되며 고객이 게임에 대해 더 잘 이해할 수 있도록 도와줍니다.   |  
|  shortTitle               |     문자열    |  제품 이름의 약식 버전입니다. 제공 되는 경우 제품의 전체 제목 대신 Xbox One (설치 중, 업적 등)의 다양 한 위치에이 짧은 이름이 표시 될 수 있습니다.    |  
|  sortTitle               |     문자열    |   제품이 다른 방식으로 사전순으로 표시 될 수 있는 경우 다른 버전을 여기에 입력할 수 있습니다. 이를 통해 고객이 검색할 때 제품을 더 빠르게 찾을 수 있습니다.    |  
|  voiceTitle               |     문자열    |   제품에 대 한 대체 이름 (제공 된 경우)은 Kinect 또는 헤드셋을 사용할 때 Xbox One의 오디오 환경에서 사용할 수 있습니다.    |  
|  devStudio               |     문자열    |   목록에서 **개발한 사람** 필드를 포함 하려면이 값을 지정 합니다. ( **게시 한 사람** 필드에는 *devstudio* 값을 제공 하는지 여부에 관계 없이 계정에 연결 된 게시자 표시 이름이 나열 됩니다.)    |  

<span id="image-object" />

### <a name="image-resource"></a>이미지 리소스

이 리소스에는 앱 목록에 대 한 이미지 및 아이콘 데이터가 포함 되어 있습니다. 앱 목록에 대 한 이미지와 아이콘에 대 한 자세한 내용은 [앱 스크린샷 및 이미지](../publish/app-screenshots-and-images.md)를 참조 하세요. 이 리소스의 값은 다음과 같습니다.

| 값           | 형식    | Description           |
|-----------------|---------|------|
|  fileName               |    문자열     |   제출을 위해 업로드 한 ZIP 보관 파일에 있는 이미지 파일의 이름입니다.    |     
|  fileStatus               |   문자열      |  이미지 파일의 상태입니다. 다음 값 중 하나일 수 있습니다. <ul><li>없음</li><li>PendingUpload</li><li>업로드됨</li><li>PendingDelete</li></ul>   |
|  id  |  문자열  | 이미지에 대 한 ID입니다. 이 값은 파트너 센터에서 제공 합니다.  |
|  description  |  문자열  | 이미지에 대 한 설명입니다.  |
|  imageType  |  문자열  | 이미지의 유형을 나타냅니다. 현재 지원 되는 문자열은 다음과 같습니다. <p/>[스크린샷 이미지](../publish/app-screenshots-and-images.md#screenshots): <ul><li>스크린샷 (바탕 화면 스크린샷에이 값 사용)</li><li>MobileScreenshot</li><li>XboxScreenshot</li><li>SurfaceHubScreenshot</li><li>HoloLensScreenshot</li></ul><p/>[스토어 로고](../publish/app-screenshots-and-images.md#store-logos):<ul><li>StoreLogo9x16 </li><li>StoreLogoSquare</li><li>아이콘 (1:1 300 x 300 픽셀 로고의 경우이 값 사용)</li></ul><p/>[프로 모션 이미지](../publish/app-screenshots-and-images.md#promotional-images): <ul><li>PromotionalArt16x9</li><li>PromotionalArtwork2400X1200</li></ul><p/>[Xbox 이미지](../publish/app-screenshots-and-images.md#xbox-images): <ul><li>XboxBrandedKeyArt</li><li>XboxTitledHeroArt</li><li>XboxFeaturedPromotionalArt</li></ul><p/>[선택적 프로 모션 이미지](../publish/app-screenshots-and-images.md#optional-promotional-images): <ul><li>SquareIcon358X358</li><li>BackgroundImage1000X800</li><li>PromotionalArtwork414X180</li></ul><p/> <!-- The following strings are also recognized for this field, but they correspond to image types that are no longer for listings in the Store.<ul><li>PromotionalArtwork846X468</li><li>PromotionalArtwork558X756</li><li>PromotionalArtwork414X468</li><li>PromotionalArtwork558X558</li><li>WideIcon358X173</li><li>Unknown</li></ul> -->   |


<span id="gaming-options-object" />

### <a name="gaming-options-resource"></a>게임 옵션 리소스

이 리소스에는 앱에 대 한 게임 관련 설정이 포함 됩니다. 이 리소스의 값은 파트너 센터의 서브 미션에 대 한 [게임 설정](../publish/enter-app-properties.md#game-settings) 에 해당 합니다.

```json
{
  "gamingOptions": [
    {
      "genres": [
        "Games_ActionAndAdventure",
        "Games_Casino"
      ],
      "isLocalMultiplayer": true,
      "isLocalCooperative": true,
      "isOnlineMultiplayer": false,
      "isOnlineCooperative": false,
      "localMultiplayerMinPlayers": 2,
      "localMultiplayerMaxPlayers": 12,
      "localCooperativeMinPlayers": 2,
      "localCooperativeMaxPlayers": 12,
      "isBroadcastingPrivilegeGranted": true,
      "isCrossPlayEnabled": false,
      "kinectDataForExternal": "Enabled"
    }
  ],
}
```

이 리소스의 값은 다음과 같습니다.

| 값           | 형식    | Description        |
|-----------------|---------|------|
|  장르               |    array     |  게임 장르를 설명 하는 다음 문자열 중 하나 이상으로 이루어진 배열입니다. <ul><li>Games_ActionAndAdventure</li><li>Games_CardAndBoard</li><li>Games_Casino</li><li>Games_Educational</li><li>Games_FamilyAndKids</li><li>Games_Fighting</li><li>Games_Music</li><li>Games_Platformer</li><li>Games_PuzzleAndTrivia</li><li>Games_RacingAndFlying</li><li>Games_RolePlaying</li><li>Games_Shooter</li><li>Games_Simulation</li><li>Games_Sports</li><li>Games_Strategy</li><li>Games_Word</li></ul>    |
|  isLocalMultiplayer               |    boolean     |  게임에서 로컬 여럿이 지원 되는지 여부를 나타냅니다.      |     
|  isLocalCooperative               |   boolean      |  게임에서 로컬 공동 작업이 지원 되는지 여부를 나타냅니다.    |     
|  isOnlineMultiplayer               |   boolean      |  게임에서 온라인 멀티 플레이를 지원할지 여부를 나타냅니다.    |     
|  isOnlineCooperative               |   boolean      |  게임에서 온라인 공동 작업이 지원 되는지 여부를 나타냅니다.    |     
|  localMultiplayerMinPlayers               |   int      |   게임이 로컬 멀티 플레이에 대해 지 원하는 최소 플레이어 수를 지정 합니다.   |     
|  localMultiplayerMaxPlayers               |   int      |   게임에서 로컬 멀티 플레이에 대해 지 원하는 최대 플레이어 수를 지정 합니다.  |     
|  localCooperativeMinPlayers               |   int      |   게임에서 로컬 공동 op에 대해 지 원하는 최소 플레이어 수를 지정 합니다.  |     
|  localCooperativeMaxPlayers               |   int      |   게임에서 로컬 공동 op에 대해 지 원하는 최대 플레이어 수를 지정 합니다.  |     
|  isBroadcastingPrivilegeGranted               |   boolean      |  게임에서 브로드캐스팅을 지원 하는지 여부를 나타냅니다.   |     
|  isCrossPlayEnabled               |   boolean      |   게임에서 Windows 10 Pc 및 Xbox의 플레이어 간 멀티 플레이 세션을 지원할지 여부를 나타냅니다.  |     
|  kinectDataForExternal               |   문자열      |  게임에서 Kinect 데이터를 수집 하 여 외부 서비스로 보낼 수 있는지 여부를 나타내는 다음 문자열 값 중 하나입니다. <ul><li>NotSet</li><li>Unknown</li><li>사용</li><li>사용 안 함</li></ul>   |

> [!NOTE]
> *GamingOptions* 리소스는 MICROSOFT STORE 제출 API가 개발자에 게 처음 출시 된 후 2017 년 5 월에 추가 되었습니다. 이 리소스가 도입 되기 전에 제출 API를 통해 앱에 대 한 제출을 생성 했 고이 제출이 아직 진행 중인 경우 제출을 성공적으로 커밋하거나 삭제할 때까지 앱에 대 한 제출을 위해이 리소스는 null입니다. 앱에 대 한 전송에 *gamingOptions* 리소스를 사용할 수 없는 경우 [app Get](get-an-app.md) 메서드에서 반환한 [응용 프로그램 리소스](get-app-data.md#application_object) 의 *hasAdvancedListingPermission* 필드가 false입니다.

<span id="status-details-object" />

### <a name="status-details-resource"></a>상태 정보 리소스

이 리소스는 제출 상태에 대 한 추가 정보를 포함 합니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 형식    | Description         |
|-----------------|---------|------|
|  오류               |    개체     |   제출에 대 한 오류 정보를 포함 하는 [상태 정보 리소스](#status-detail-object) 의 배열입니다.    |     
|  경고               |   개체      | 제출에 대 한 경고 세부 정보를 포함 하는 [상태 정보 리소스](#status-detail-object) 의 배열입니다.      |
|  certificationReports               |     개체    |   제출할 인증 보고서 데이터에 대 한 액세스를 제공 하는 [인증 보고서 리소스](#certification-report-object) 의 배열입니다. 이러한 보고서에서 인증에 실패 하는 경우 자세한 정보를 확인할 수 있습니다.   |  


<span id="status-detail-object" />

### <a name="status-detail-resource"></a>상태 정보 리소스

이 리소스에는 전송에 대 한 모든 관련 오류 또는 경고에 대 한 추가 정보가 포함 되어 있습니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 형식    | 설명        |
|-----------------|---------|------|
|  code               |    문자열     |   오류 또는 경고 유형을 설명 하는 [제출 상태 코드](#submission-status-code) 입니다.   |     
|  자세히               |     문자열    |  문제에 대 한 자세한 정보를 포함 하는 메시지입니다.     |


<span id="application-package-object" />

### <a name="application-package-resource"></a>응용 프로그램 패키지 리소스

이 리소스에는 전송용 앱 패키지에 대 한 세부 정보가 포함 됩니다.

```json
{
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
      "fileStatus": "Uploaded",
      "id": "1152921504620138797",
      "version": "1.0.0.0",
      "architecture": "ARM",
      "languages": [
        "en-US"
      ],
      "capabilities": [
        "ID_RESOLUTION_HD720P",
        "ID_RESOLUTION_WVGA",
        "ID_RESOLUTION_WXGA"
      ],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None",
      "targetDeviceFamilies": [
        "Windows.Mobile min version 10.0.10240.0"
      ]
    }
  ],
}
```

이 리소스의 값은 다음과 같습니다.  

> [!NOTE]
> [앱 전송 업데이트](update-an-app-submission.md) 메서드를 호출 하는 경우 요청 본문에이 개체의 *fileName*, *fileStatus,* *,가 중*및 *ram* 값만 필요 합니다. 다른 값은 파트너 센터에서 채워집니다.

| 값           | 형식    | Description                   |
|-----------------|---------|------|
| fileName   |   문자열      |  패키지의 이름입니다.    |  
| fileStatus    | 문자열    |  패키지의 상태입니다. 다음 값 중 하나일 수 있습니다. <ul><li>없음</li><li>PendingUpload</li><li>업로드됨</li><li>PendingDelete</li></ul>    |  
| id    |  문자열   |  패키지를 고유 하 게 식별 하는 ID입니다. 이 값은 파트너 센터에서 제공 합니다.   |     
| 버전    |  문자열   |  응용 프로그램 패키지의 버전입니다. 자세한 내용은 [패키지 버전 번호 매기기](../publish/package-version-numbering.md)를 참조 하세요.   |   
| 아키텍처    |  문자열   |  패키지의 아키텍처 (예: ARM)입니다.   |     
| 언어    | array    |  앱에서 지 원하는 언어에 대 한 언어 코드의 배열입니다. 자세한 내용은 [지원되는 언어](../publish/supported-languages.md)를 참조하세요.    |     
| capabilities    |  array   |  패키지에 필요한 기능 배열입니다. 기능에 대 한 자세한 내용은 [앱 기능 선언](../packaging/app-capability-declarations.md)을 참조 하세요.   |     
| 이상 버전    |  문자열   |  앱 패키지에서 지 원하는 최소 DirectX 버전입니다. 이는 Windows 8.x를 대상으로 하는 앱에 대해서만 설정할 수 있습니다. 다른 OS 버전을 대상으로 하는 앱의 경우이 값은 [앱 전송 업데이트](update-an-app-submission.md) 메서드를 호출할 때 표시 되어야 하지만 지정한 값은 무시 됩니다. 다음 값 중 하나일 수 있습니다. <ul><li>없음</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| 이상 및 Ram    | 문자열    |  앱 패키지에 필요한 최소 RAM입니다. 이는 Windows 8.x를 대상으로 하는 앱에 대해서만 설정할 수 있습니다. 다른 OS 버전을 대상으로 하는 앱의 경우이 값은 [앱 전송 업데이트](update-an-app-submission.md) 메서드를 호출할 때 표시 되어야 하지만 지정한 값은 무시 됩니다. 다음 값 중 하나일 수 있습니다. <ul><li>없음</li><li>Memory2GB</li></ul>   |       
| targetDeviceFamilies    | array    |  패키지가 대상으로 하는 장치 패밀리를 나타내는 문자열의 배열입니다. 이 값은 Windows 10을 대상으로 하는 패키지에만 사용 됩니다. 이전 릴리스를 대상으로 하는 패키지의 경우이 값의 값은 **None**입니다. 다음 장치 패밀리 문자열은 현재 Windows 10 패키지에 대해 지원 됩니다 *{0}* . 여기서은 10.0.10240.0, 10.0.10586.0 또는 10.0.14393.0와 같은 windows 10 버전 문자열입니다. <ul><li>Windows. Universal min version *{0}*</li><li>Windows 데스크톱 최소 버전 *{0}*</li><li>Windows. Mobile 최소 버전 *{0}*</li><li>Windows Xbox 최소 버전 *{0}*</li><li>Holographic min 버전 *{0}*</li></ul>   |    

<span/>

<span id="certification-report-object" />

### <a name="certification-report-resource"></a>인증 보고서 리소스

이 리소스는 제출에 대 한 인증 보고서 데이터에 대 한 액세스를 제공 합니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 형식    | Description             |
|-----------------|---------|------|
|     date            |    문자열     |  보고서가 생성 된 날짜와 시간 (ISO 8601 형식)입니다.    |
|     reportUrl            |    문자열     |  보고서에 액세스할 수 있는 URL입니다.    |


<span id="package-delivery-options-object" />

### <a name="package-delivery-options-resource"></a>패키지 제공 옵션 리소스

이 리소스에는 전송에 대 한 점진적 패키지 롤아웃 및 필수 업데이트 설정이 포함 되어 있습니다.

```json
{
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
}
```

이 리소스의 값은 다음과 같습니다.

| 값           | 형식    | Description        |
|-----------------|---------|------|
| packageRollout   |   개체      |  제출에 대 한 점진적 패키지 롤아웃 설정을 포함 하는 [패키지 롤아웃 리소스](#package-rollout-object) 입니다.   |  
| isMandatoryUpdate    | boolean    |  이 제출의 패키지를 자동 설치 앱 업데이트에 대 한 필수로 처리할지 여부를 나타냅니다. 자동 설치 앱 업데이트에 대 한 필수 패키지에 대 한 자세한 내용은 [앱에 대 한 패키지 업데이트 다운로드 및 설치](../packaging/self-install-package-updates.md)를 참조 하세요.    |  
| mandatoryUpdateEffectiveDate    |  date   |  ISO 8601 형식 및 UTC 표준 시간대에서이 제출의 패키지가 필수 인 날짜 및 시간입니다.   |        

<span id="package-rollout-object" />

### <a name="package-rollout-resource"></a>패키지 출시 리소스

이 리소스에는 제출에 대 한 점진적 [패키지 롤아웃 설정이](#manage-gradual-package-rollout) 포함 되어 있습니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 형식    | Description        |
|-----------------|---------|------|
| isPackageRollout   |   boolean      |  제출할 때 점진적 패키지 롤아웃이 사용 되는지 여부를 나타냅니다.    |  
| packageRolloutPercentage    | float    |  점진적 롤아웃에서 패키지를 수신 하는 사용자의 백분율입니다.    |  
| packageRolloutStatus    |  문자열   |  점진적 패키지 롤아웃 상태를 나타내는 다음 문자열 중 하나입니다. <ul><li>PackageRolloutNotStarted</li><li>PackageRolloutInProgress</li><li>PackageRolloutComplete</li><li>PackageRolloutStopped</li></ul>  |  
| fallbackSubmissionId    |  문자열   |  점진적 출시 패키지를 가져오지 않는 고객이 받는 제출의 ID입니다.   |          

> [!NOTE]
> *PackageRolloutStatus* 및 *FallbackSubmissionId* 값은 파트너 센터에서 할당 되며 개발자가 설정 하기 위한 것이 아닙니다. 요청 본문에 이러한 값을 포함 하는 경우 이러한 값은 무시 됩니다.

<span id="trailer-object" />

### <a name="trailers-resource"></a>트레일러 리소스

이 리소스는 앱 목록에 대 한 비디오 트레일러를 나타냅니다. 이 리소스의 값은 파트너 센터의 서브 미션에 대 한 [트레일러](../publish/app-screenshots-and-images.md#trailers) 옵션에 해당 합니다.

[앱 제출 리소스](#app-submission-object)에서 *트레일러 배열에* 최대 15 개의 트레일러 리소스를 추가할 수 있습니다. 전송에 대 한 트레일러 비디오 파일 및 미리 보기 이미지를 업로드 하려면 해당 파일을 제출할 패키지를 포함 하는 동일한 ZIP 보관 파일에 추가 하 고이 ZIP 보관 파일을 전송에 대 한 SAS (공유 액세스 서명) URI에 업로드 합니다. SAS URI에 ZIP 보관 파일을 업로드 하는 방법에 대 한 자세한 내용은 [앱 제출 만들기](#create-an-app-submission)를 참조 하세요.

```json
{
  "trailers": [
    {
      "id": "1158943556954955699",
      "videoFileName": "Trailers\\ContosoGameTrailer.mp4",
      "videoFileId": "1159761554639123258",
      "trailerAssets": {
        "en-us": {
          "title": "Contoso Game",
          "imageList": [
            {
              "fileName": "Images\\ContosoGame-Thumbnail.png",
              "id": "1155546904097346923",
              "description": "This is a still image from the video."
            }
          ]
        }
      }
    }
  ]
}
```

이 리소스의 값은 다음과 같습니다.

| 값           | 형식    | Description        |
|-----------------|---------|------|
|  id               |    문자열     |   트레일러의 ID입니다. 이 값은 파트너 센터에서 제공 합니다.   |
|  비디오 파일 이름               |    문자열     |    제출할 파일이 포함 된 ZIP 보관 파일의 트레일러 비디오 파일 이름입니다.    |     
|  videoFileId               |   문자열      |  트레일러 비디오 파일의 ID입니다. 이 값은 파트너 센터에서 제공 합니다.   |     
|  trailerAssets               |   개체      |  키/값 쌍의 사전입니다. 여기서 각 키는 언어 코드이 고 각 값은 트레일러의 추가 로캘별 자산을 포함 하는 [트레일러 자산 리소스](#trailer-assets-object) 입니다. 지원 되는 언어 코드에 대 한 자세한 내용은 [지원 되는 언어](../publish/supported-languages.md)를 참조 하세요.    |     

> [!NOTE]
> Microsoft Store 제출 API가 개발자에 게 처음 출시 된 후에는 2017 년 5 월에 *트레일러* 리소스가 추가 되었습니다. 이 리소스가 도입 되기 전에 제출 API를 통해 앱에 대 한 제출을 생성 했 고이 제출이 아직 진행 중인 경우 제출을 성공적으로 커밋하거나 삭제할 때까지 앱에 대 한 제출을 위해이 리소스는 null입니다. 응용 프로그램에 대 한 서브 *미션에 대* 한 서브 미션 리소스를 사용할 수 없는 경우 [앱 가져오기](get-an-app.md) 메서드에서 반환 하는 [응용 프로그램 리소스](get-app-data.md#application_object) 의 *hasAdvancedListingPermission* 필드는 false입니다.

<span id="trailer-assets-object" />

### <a name="trailer-assets-resource"></a>트레일러 자산 리소스

이 리소스는 [트레일러 리소스](#trailer-object)에 정의 된 트레일러에 대 한 추가 로캘별 자산을 포함 합니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 형식    | Description        |
|-----------------|---------|------|
| title   |   문자열      |  트레일러의 지역화 된 제목입니다. 사용자가 전체 화면 모드에서 트레일러를 재생 하면 제목이 표시 됩니다.     |  
| imageList    | array    |   트레일러에 대 한 미리 보기 이미지를 제공 하는 [이미지](#image-for-trailer-object) 리소스 하나를 포함 하는 배열입니다. 이 배열에는 [이미지](#image-for-trailer-object) 리소스를 하나만 포함할 수 있습니다.  |   


<span id="image-for-trailer-object" />

### <a name="image-resource-for-a-trailer"></a>이미지 리소스 (트레일러)

이 리소스는 트레일러의 미리 보기 이미지를 설명 합니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 형식    | Description           |
|-----------------|---------|------|
|  fileName               |    문자열     |   제출을 위해 업로드 한 ZIP 보관 파일에 있는 미리 보기 이미지 파일의 이름입니다.    |     
|  id  |  문자열  | 미리 보기 이미지의 ID입니다. 이 값은 파트너 센터에서 제공 합니다.  |
|  description  |  문자열  | 미리 보기 이미지에 대 한 설명입니다. 이 값은 메타 데이터 전용 이며 사용자에 게 표시 되지 않습니다.   |

<span/>

## <a name="enums"></a>열거형

이러한 메서드는 다음 열거형을 사용 합니다.

<span id="price-tiers" />

### <a name="price-tiers"></a>가격 책정 계층

다음 값은 앱 전송에 대 한 [가격 책정 리소스](#pricing-object) 리소스에서 사용 가능한 가격 계층을 나타냅니다.

| 값           | Description        |
|-----------------|------|
|  기준               |   가격 책정 계층이 설정 되지 않았습니다. 앱에 대 한 기본 가격을 사용 합니다.      |     
|  NotAvailable              |   지정 된 지역에서 앱을 사용할 수 없습니다.    |     
|  Free              |   앱은 무료입니다.    |    
|  계층*xxx*               |   **계층<em>xxxx</em>** 형식의 앱에 대 한 가격 책정 계층을 지정 하는 문자열입니다. 현재 지원 되는 가격 책정 계층 범위는 다음과 같습니다.<br/><br/><ul><li>[가격 책정 리소스](#pricing-object) 의 *isAdvancedPricingModel* 값이 **true**이면 계정에 사용할 수 있는 가격 책정 계층 값은 **Tier1012**  -  **Tier1424**입니다.</li><li>[가격 책정 리소스](#pricing-object) 의 *isAdvancedPricingModel* 값이 **false**이면 계정에 사용할 수 있는 가격 책정 계층 값은 **Tier2**  -  **Tier96**입니다.</li></ul>각 계층과 관련 된 시장 관련 가격을 비롯 하 여 개발자 계정에 사용할 수 있는 가격 책정 계층의 전체 표를 보려면 파트너 센터에서 앱 서브 미션에 대 한 **가격 책정 및 가용성** 페이지로 이동 하 고 **시장 및 사용자 지정 가격** 섹션에서 **테이블 보기** 링크를 클릭 합니다 (일부 개발자 계정의 경우이 링크는 **가격 책정** 섹션에 있음).    |


<span id="enterprise-licensing" />

### <a name="enterprise-licensing-values"></a>엔터프라이즈 라이선스 값

다음 값은 앱의 조직 라이선스 동작을 나타냅니다. 이러한 옵션에 대 한 자세한 내용은 [조직 라이선스 옵션](../publish/organizational-licensing.md)을 참조 하세요.

> [!NOTE]
> 제출 API를 통해 앱 전송에 대 한 조직 라이선스 옵션을 구성할 수 있지만이 API를 사용 하 여 비즈니스에 대 한 [Microsoft Store 및 교육용 Microsoft Store를 통해 대량 구매](../publish/organizational-licensing.md)에 대 한 제출을 게시할 수는 없습니다. 비즈니스 및 교육용 Microsoft Store에 대 한 Microsoft Store에 제출을 게시 하려면 파트너 센터를 사용 해야 합니다.


| 값           |  Description      |
|-----------------|---------------|
| 없음            |     스토어 관리 (온라인) 볼륨 라이선스를 사용 하는 기업에서 앱을 사용할 수 있도록 설정 하지 마세요.         |     
| 온라인        |     스토어 관리 (온라인) 볼륨 라이선스를 사용 하는 기업에서 앱을 사용할 수 있도록 설정 합니다.  |
| OnlineAndOffline | 스토어 관리 (온라인) 볼륨 라이선스를 사용 하는 기업에서 앱을 사용할 수 있도록 설정 하 고, 연결 끊김 (오프 라인) 라이선스를 통해 기업에서 앱을 사용할 수 있도록 합니다. |


<span id="submission-status-code" />

### <a name="submission-status-code"></a>제출 상태 코드

다음 값은 제출의 상태 코드를 나타냅니다.

| 값           |  Description      |
|-----------------|---------------|
| 없음            |     코드를 지정 하지 않았습니다.         |     
| InvalidArchive        |     패키지를 포함 하는 ZIP 보관 파일이 잘못 되었거나 보관 파일 형식을 인식할 수 없습니다.  |
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
* [Microsoft Store 제출 API를 사용 하 여 앱 데이터 가져오기](get-app-data.md)
* [파트너 센터의 앱 서브 미션](../publish/app-submissions.md)