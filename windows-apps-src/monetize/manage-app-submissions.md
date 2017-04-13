---
author: mcleanbyron
ms.assetid: C7428551-4B31-4259-93CD-EE229007C4B8
description: "Windows 스토어 제출 API에서 다음 메서드를 사용하여 Windows 개발자 센터 계정에 등록된 앱의 제출을 관리합니다."
title: "앱 제출 관리"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, Windows 스토어 제출 API, 앱 제출"
ms.openlocfilehash: f6f0342619f91190bf021842c3ac3b0c61964d25
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="manage-app-submissions"></a>앱 제출 관리

Windows 스토어 제출 API는 점진적 패키지 출시를 비롯하여 앱 제출을 관리하는 데 사용할 수 있는 메서드를 제공합니다. API 사용을 위한 필수 조건을 비롯하여 Windows 스토어 제출 API에 대한 자세한 내용은 [Windows 스토어 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)를 참조하세요.

>**참고**&nbsp;&nbsp;이러한 메서드는 Windows 스토어 제출 API를 사용할 수 있는 권한을 가진 Windows 개발자 센터 계정에 대해서만 사용할 수 있습니다. 이 권한은 개발자 계정에서 단계별로 사용할 수 있으며, 현재로서는 이 권한을 모든 계정에서 사용할 수 있는 것은 아닙니다. 이전 액세스를 요청하려면 개발자 센터 대시보드에 로그온하고 대시보드의 아래쪽에서 **피드백**을 클릭하여 피드백 영역의 **제출 API**를 선택한 다음 요청을 제출합니다. 계정에 대해 이 권한을 사용할 수 있는 경우 메일을 받게 됩니다.

>**중요**&nbsp;&nbsp;Windows 스토어 제출 API를 사용하여 앱에 대한 제출을 생성하는 경우, 개발자 센터 대시보드 대신 API를 통해서만 제출을 추가 변경해야 합니다. 대시보드를 사용하여 원래 API로 만든 제출을 변경하는 경우 더 이상 API를 사용하여 해당 제출을 변경하거나 커밋할 수 없습니다. 경우에 따라 제출이 제출 프로세스를 더 이상 진행할 수 없는 오류 상태로 남을 수 있습니다. 이러한 문제가 발생하는 경우, 해당 제출을 삭제하고 새 제출을 생성해야 합니다.

<span id="methods-for-app-submissions" />
## <a name="methods-for-managing-app-submissions"></a>앱 제출 관리 메서드

앱 제출 가져오기, 생성, 업데이트, 커밋, 삭제 작업에는 다음 메서드를 사용합니다. 이러한 메서드를 사용하려면 앱이 개발자 센터 계정에 이미 있어야 하고 앱에 대한 제출 한 개를 대시보드에 먼저 만들어야 합니다. 자세한 내용은 [전제 조건](create-and-manage-submissions-using-windows-store-services.md#prerequisites)을 참조하세요.

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
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}```</td>
<td align="left">[기존 앱 제출 가져오기](get-an-app-submission.md)</td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status```</td>
<td align="left">[기존 앱 제출의 상태 가져오기](get-status-for-an-app-submission.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions```</td>
<td align="left">[새 앱 제출 만들기](create-an-app-submission.md)</td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}```</td>
<td align="left">[기존 앱 제출 업데이트](update-an-app-submission.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit```</td>
<td align="left">[새로운 또는 업데이트된 앱 제출 커밋](commit-an-app-submission.md)</td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}```</td>
<td align="left">[앱 제출 삭제](delete-an-app-submission.md)</td>
</tr>
</tbody>
</table>

<span id="create-an-app-submission">
### 앱 제출 만들기

앱에 대한 제출을 만들려면 이 프로세스를 따릅니다.

1. 아직 완료하지 않은 경우 Windows 스토어 제출 API에 대한 모든 [필수 조건](create-and-manage-submissions-using-windows-store-services.md#prerequisites)을 완료합니다.

  >**참고**&nbsp;&nbsp;앱에 [연령별 등급](https://msdn.microsoft.com/windows/uwp/publish/age-ratings) 정보가 완료된 제출이 이미 하나 이상 있어야 합니다.

2. [Azure AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Windows 스토어 제출 API의 메서드에 이 액세스 토큰을 전달해야 합니다. 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

3. Windows 스토어 제출 API에서 다음 메서드를 실행하여 [앱 제출을 만듭니다](create-an-app-submission.md). 이 메서드는 마지막으로 게시된 제출의 복사본인 새 진행 중 제출을 만듭니다.

  > [!div class="tabbedCodeSnippets"]
  ``` syntax
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions
  ```

  응답 본문에는 새 제출의 ID, 새 제출의 데이터(모든 목록 및 가격 정보) 및 제출의 앱 패키지 및 목록 이미지를 Azure Blob Storage에 업로드하기 위한 SAS(공유 액세스 서명) URI가 포함되어 있습니다.

  >**참고**&nbsp;&nbsp;SAS URI는 계정 키를 요구하지 않고 Azure Storage의 보안 리소스에 대한 액세스를 제공합니다. SAS URI 및 Azure Blob Storage를 통한 SAS URI 사용에 대한 배경 정보는 [공유 액세스 서명, 1부: SAS 모델 이해](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1) 및 [공유 액세스 서명, 2부: Blob Storage를 사용하여 SAS 만들기 및 사용](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/)을 참조하세요.

4. 제출에 대한 새 패키지 또는 이미지를 추가하려면 [앱 패키지를 준비](https://msdn.microsoft.com/windows/uwp/publish/app-package-requirements)하고 [앱 스크린샷 및 이미지를 준비](https://msdn.microsoft.com/windows/uwp/publish/app-screenshots-and-images)합니다. ZIP 보관 파일에 이러한 파일을 모두 추가합니다.

5. 새 제출에 대해 필요한 변경 사항으로 제출 데이터를 수정하고 다음 메서드를 실행하여 [앱 제출을 업데이트](update-an-app-submission.md)합니다.

  > [!div class="tabbedCodeSnippets"]
  ``` syntax
  PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}
  ```

  <span/>
  >**참고**&nbsp;&nbsp;제출에 대한 새 패키지 또는 이미지를 추가하는 경우 ZIP 보관 파일에서 이러한 파일의 상대 경로 및 이름을 참조하도록 제출 데이터를 업데이트해야 합니다.

4. 제출에 대한 새 패키지 또는 이미지를 추가하는 경우 이전에 호출한 POST 메서드의 응답 본문에 제공된 SAS URI를 사용하여 [Azure Blob Storage](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage)에 ZIP 보관 파일을 업로드합니다. 다음과 같이 다양한 플랫폼에서 이 작업을 수행할 수 있는 여러 Azure 라이브러리가 있습니다.

  * [.NET용 Azure Storage Client Library](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-blobs)
  * [Java용 Azure Storage SDK](https://docs.microsoft.com/azure/storage/storage-java-how-to-use-blob-storage)
  * [Python용 Azure Storage SDK](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-blob-storage)

  <span/>

  다음 C# 코드 예제는 .NET용 Azure Storage Client Library의 [CloudBlockBlob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.aspx) 클래스를 사용하여 Azure Blob Storage에 ZIP 보관 파일을 업로드하는 방법을 보여 줍니다. 이 예제에서는 ZIP 보관 파일이 이미 스트림 개체에 기록되어 있다고 가정합니다.

  > [!div class="tabbedCodeSnippets"]
  ```csharp
  string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";
  Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
      new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
  await blockBob.UploadFromStreamAsync(stream);
  ```

5. 다음 메서드를 실행하여 [앱 제출을 커밋](commit-an-app-submission.md)합니다. 이렇게 하면 제출이 완료되었으며 업데이트를 해당 계정에 지금 적용해야 한다는 사실을 개발자 센터에 알려줍니다.

  > [!div class="tabbedCodeSnippets"]
  ``` syntax
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit
  ```

6. 다음 메서드 실행을 통해 [앱 제출의 상태를 가져와](get-status-for-an-app-submission.md) 커밋 상태를 확인합니다.

  > [!div class="tabbedCodeSnippets"]
  ``` syntax
  GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status
  ```

  제출 상태를 확인하려면 응답 본문에서 *status* 값을 검토합니다. 이 값은 요청이 성공한 경우 **CommitStarted**에서 **PreProcessing**으로, 요청에 오류가 발생한 경우 **CommitFailed**로 변경됩니다. 오류가 있는 경우 *statusDetails* 필드에 오류에 대한 추가 정보가 포함됩니다.

7. 커밋이 성공적으로 완료되면 수집을 위해 제출이 스토어로 전송됩니다. 이전 메서드를 사용하거나 Windows 개발자 센터 대시보드를 방문하여 제출 진행 상황을 계속 모니터링할 수 있습니다.

<span/>
### <a name="code-examples-for-managing-app-submissions"></a>앱 제출 관리에 대한 코드 예제

다음 문서는 여러 다른 프로그래밍 언어로 앱 제출 API를 만드는 방법을 보여 주는 자세한 코드 예제를 제공합니다.

* [C# 코드 예제](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Java 코드 예제](java-code-examples-for-the-windows-store-submission-api.md)
* [Python 코드 예제](python-code-examples-for-the-windows-store-submission-api.md)

>**참고**&nbsp;&nbsp;위에 나열된 코드 예제 외에도 the Windows 스토어 제출 API 위에 명령줄 인터페이스를 구현하는 오픈 소스 PowerShell 모듈도 제공합니다. 이 모듈을 [StoreBroker](https://aka.ms/storebroker)라고 합니다. 이 모듈을 사용하여 Windows 스토어 제출 API를 직접 호출하는 대신 명령줄에서 앱, 플라이트 및 추가 기능 제출을 관리할 수 있습니다. 또는 소스에서 이 API를 호출하는 방법에 대한 예제를 더 찾아볼 수 있습니다. StoreBroker 모듈은 다수의 자사 응용 프로그램을 스토어에 제출하는 기본 방법으로 Microsoft 내에서 적극적으로 사용됩니다. 자세한 내용은 [GitHub의 StoreBroker 페이지](https://aka.ms/storebroker)를 참조하세요.

<span id="manage-gradual-package-rollout">
## <a name="methods-for-managing-a-gradual-package-rollout"></a>점진적 패키지 출시 관리 메서드

앱 제출에서 업데이트된 패키지를 앱의 Windows10 고객의 비율로 점진적으로 배포할 수 있습니다. 이렇게 하면 피드백 및 분석 데이터를 모니터링하여 보다 광범위하게 출시하기 전에 업데이트의 품질을 확인할 수 있습니다. 새 제출을 만들지 않고도 게시된 제출에 대한 배포 백분율을 변경(또는 업데이트를 중단)할 수 있습니다. Windows 개발자 센터 대시보드에서 점진적 패키지 출시를 사용하도록 설정하고 관리하는 방법에 대한 지침을 비롯한 자세한 내용은 [이 문서](../publish/gradual-package-rollout.md)를 참조하세요.

또한 Windows 스토어 제출 API에서 여러 메서드를 사용하여 이 프로세스를 따르면 앱 제출에 대해 프로그래밍 방식으로 점진적 패키지 출시를 사용하도록 설정할 수도 있습니다.

  1. [앱 제출 만들기](create-an-app-submission.md) 또는 [기존 앱 제출 가져오기](get-an-app-submission.md).
  2. 응답 데이터에서 [packageRollout](#package-rollout-object) 리소스를 찾고, *isPackageRollout* 필드를 **true**로 설정하고, *packageRolloutPercentage* 필드를 업데이트된 패키지를 가져와야 하는 앱 고객의 백분율로 설정합니다.
  3. 업데이트된 앱 제출 데이터를 [앱 제출 업데이트](update-an-app-submission.md) 메서드로 전달합니다.

앱 제출에 대해 점진적 패키지 출시를 사용할 수 있게 설정한 후 다음 메서드를 사용하여 점진적 출시를 프로그래밍 방식으로 가져오고 업데이트하고 중지하며 완료할 수 있습니다.

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
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/packagerollout```</td>
<td align="left">[앱 제출에 대한 점진적 출시 가져오기](get-package-rollout-info-for-an-app-submission.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/updatepackagerolloutpercentage```</td>
<td align="left">[앱 제출에 대한 점진적 출시 백분율 업데이트](update-the-package-rollout-percentage-for-an-app-submission.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/haltpackagerollout```</td>
<td align="left">[앱 제출에 대한 점진적 출시 중지](halt-the-package-rollout-for-an-app-submission.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/finalizepackagerollout```</td>
<td align="left">[앱 제출에 대한 점진적 출시 완료](finalize-the-package-rollout-for-an-app-submission.md)</td>
</tr>
</tbody>
</table>

<span/>
## 데이터 리소스 앱 제출을 관리하는 Windows 스토어 제출 API 메서드는 다음과 같은 JSON 데이터 리소스를 사용합니다.

<span id="app-submission-object" />
### 앱 제출 리소스

이 리소스는 앱 제출을 설명합니다.

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
  "friendlyName": "Submission 2"
}
```

이 리소스의 값은 다음과 같습니다.

| 값      | 유형   | 설명      |
|------------|--------|-------------------|
| id            | 문자열  | 제출의 ID입니다.  |
| applicationCategory           | 문자열  |   앱에 대한 [범주 및/또는 하위 범주](https://msdn.microsoft.com/windows/uwp/publish/category-and-subcategory-table)를 지정하는 문자열입니다. 범주와 하위 범주는 **BooksAndReference_EReader**처럼 밑줄(_) 문자를 사용하여 단일 문자열로 연결합니다.      |  
| pricing           |  object  | 앱에 대한 가격 정보를 포함하는 [가격 리소스](#pricing-object)입니다.        |   
| visibility           |  문자열  |  앱의 표시 여부입니다. 다음 값 중 하나일 수 있습니다. <ul><li>Hidden</li><li>Public</li><li>개인 정보 보호</li><li>NotSet</li></ul>       |   
| targetPublishMode           | 문자열  | 제출의 게시 모드입니다. 다음 값 중 하나일 수 있습니다. <ul><li>즉시</li><li>수동</li><li>SpecificDate</li></ul> |
| targetPublishDate           | 문자열  | *targetPublishMode*가 SpecificDate로 설정된 경우 제출의 게시 날짜(ISO 8601 형식)입니다.  |  
| listings           |   object  |  키와 값 쌍의 사전입니다. 여기서 각 키는 국가 코드이며 각 값은 앱에 대한 목록 정보를 포함하는 [목록 리소스](#listing-object)입니다.       |   
| hardwarePreferences           |  배열  |   앱에 대한 [하드웨어 기본 설정](https://msdn.microsoft.com/windows/uwp/publish/enter-app-properties#hardware_preferences)을 정의하는 문자열의 배열입니다. 다음 값 중 하나일 수 있습니다. <ul><li>터치</li><li>Keyboard</li><li>마우스</li><li>Camera</li><li>NfcHce</li><li>Nfc</li><li>BluetoothLE</li><li>Telephony</li></ul>     |   
| automaticBackupEnabled           |  boolean  |   Windows에서 이 앱의 데이터를 OneDrive에 대한 자동 백업에 포함할 수 있는지 여부를 나타냅니다. 자세한 내용은 [앱 선언](https://msdn.microsoft.com/windows/uwp/publish/app-declarations)을 참조하세요.   |   
| canInstallOnRemovableMedia           |  boolean  |   고객이 이동식 저장소에 앱을 설치할 수 있는지 여부를 나타냅니다. 자세한 내용은 [앱 선언](https://msdn.microsoft.com/windows/uwp/publish/app-declarations)을 참조하세요.     |   
| isGameDvrEnabled           |  boolean |   앱에 대한 게임 DVR을 사용할지 여부를 나타냅니다.    |   
| hasExternalInAppProducts           |     boolean          |   Windows 스토어 상거래 시스템 밖에서 사용자가 이 앱을 구매할 수 있는지 여부를 나타냅니다. 자세한 내용은 [앱 선언](https://msdn.microsoft.com/windows/uwp/publish/app-declarations)을 참조하세요.     |   
| meetAccessibilityGuidelines           |    boolean           |  이 앱이 접근성 지침을 준수하도록 테스트되었는지 여부를 나타냅니다. 자세한 내용은 [앱 선언](https://msdn.microsoft.com/windows/uwp/publish/app-declarations)을 참조하세요.      |   
| notesForCertification           |  문자열  |   앱의 [인증에 대한 참고 사항](https://msdn.microsoft.com/windows/uwp/publish/notes-for-certification)이 포함됩니다.    |    
| status           |   문자열  |  제출의 상태입니다. 다음 값 중 하나일 수 있습니다. <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>릴리스</li><li>ReleaseFailed</li></ul>      |    
| statusDetails           |   object  | 오류에 대한 정보를 비롯하여 제출 상태에 대한 추가 세부 정보가 포함된 [상태 세부 정보 리소스](#status-details-object)입니다.       |    
| fileUploadUrl           |   문자열  | 제출에 대한 패키지를 업로드하기 위한 SAS(공유 액세스 서명) URI입니다. 제출에 대한 새 패키지 또는 이미지를 추가하는 경우 패키지 및 이미지가 포함된 ZIP 보관 파일을 이 URI에 업로드합니다. 자세한 내용은 [앱 제출 만들기](#create-an-app-submission)를 참조하세요.       |    
| applicationPackages           |   배열  | 제출의 각 패키지에 대한 세부 정보를 제공하는 [응용 프로그램 패키지 리소스](#application-package-object) 배열입니다. |    
| packageDeliveryOptions    | object  | 제출에 대한 점진적 패키지 출시 및 필수 업데이트 설정을 포함하는 [패키지 전송 옵션 리소스](#package-delivery-options-object)입니다.  |
| enterpriseLicensing           |  문자열  |  앱의 엔터프라이즈 라이선스 동작을 나타내는 [엔터프라이즈 라이선스 값](#enterprise-licensing) 값 중 하나입니다.  |    
| allowMicrosftDecideAppAvailabilityToFutureDeviceFamilies           |  boolean   |  Microsoft에서 [이후의 Windows10 디바이스 패밀리에 앱을 제공하도록](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families) 허용할지 여부를 나타냅니다.    |    
| allowTargetFutureDeviceFamilies           | object   |  키와 값 쌍의 사전입니다. 여기서 각 키는 [Windows10 장치 패밀리](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families)이며 각 값은 앱이 지정된 장치 패밀리를 대상으로 허용할지 여부를 나타내는 부울입니다.     |    
| FriendlyName           |   문자열  |  개발자 센터 대시보드와 같이 제출의 이름입니다. 이 값은 제출을 만들 때 생성됩니다.       |  


<span id="listing-object" />
### <a name="listing-resource"></a>목록 리소스

이 리소스는 앱에 대한 목록 정보를 포함합니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 유형    | 설명                  |
|-----------------|---------|------|
|  baseListing               |   object      |  모든 플랫폼에 대한 기본 목록 정보를 정의하는 앱의 [기본 목록](#base-listing-object) 정보입니다.   |     
|  platformOverrides               | object |   키와 값 쌍의 사전입니다. 여기서 각 키는 목록 정보를 재정의할 플랫폼을 식별하는 문자열이며 각 값은 지정된 플랫폼에 대해 재정의할 목록 정보를 지정하는 [기본 목록](#base-listing-object) 리소스(제목에 대한 설명 값만 포함)입니다. 키는 다음 값을 가질 수 있습니다. <ul><li>알 수 없음</li><li>Windows80</li><li>Windows81</li><li>WindowsPhone71</li><li>WindowsPhone80</li><li>WindowsPhone81</li></ul>     |      |     

<span id="base-listing-object" />
### <a name="base-listing-resource"></a>기본 목록 리소스

이 리소스는 앱에 대한 기본 목록 정보를 포함합니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 유형    | 설명       |
|-----------------|---------|------|
|  copyrightAndTrademarkInfo                |   문자열      |  선택적 [저작권 및/또는 상표 정보](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#copyright-and-trademark-info)입니다.  |
|  키워드                |  배열       |  검색 결과에 앱을 표시하는 데 도움이 되는 [키워드](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#keywords)의 배열입니다.    |
|  licenseTerms                |    문자열     | 앱에 대한 선택적 [사용 조건](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#additional-license-terms)입니다.     |
|  privacyPolicy                |   문자열      |   앱의 [개인 정보 취급 방침](../publish/create-app-store-listings.md#privacy-policy) URL입니다.    |
|  supportContact                |   문자열      |  앱의 [연락처 정보 지원](../publish/create-app-store-listings.md#support-contact-info)을 위한 URL 또는 메일 주소입니다.     |
|  websiteUrl                |   문자열      |  앱에 대한 [웹 페이지](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#website)의 URL입니다.    |    
|  description               |    문자열     |   앱 목록에 대한 [설명](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#description)입니다.   |     
|  기능               |    배열     |  앱의 [기능](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#app-features)을 나열하는 최대 20개 문자열의 배열입니다.     |
|  releaseNotes               |  문자열       |  앱의 [릴리스 정보](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#release-notes)입니다.    |
|  이미지               |   배열      |  앱 목록의 [이미지 및 아이콘](#image-object) 리소스의 배열입니다.  |
|  recommendedHardware               |   배열      |  앱의 [권장되는 하드웨어 구성](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#recommended-hardware)을 나열하는 최대 11개 문자열의 배열입니다.     |
|  title               |     문자열    |   앱 목록에 대한 제목입니다.   |  

<span id="image-object" />
### <a name="image-resource"></a>이미지 리소스

이 리소스에는 앱 목록에 대한 이미지 및 아이콘 데이터가 포함되어 있습니다. 목록의 이미지 및 아이콘에 대한 자세한 내용은 [앱 스크린샷 및 이미지](https://msdn.microsoft.com/windows/uwp/publish/app-screenshots-and-images)를 참조하세요. 이 리소스의 값은 다음과 같습니다.

| 값           | 유형    | 설명           |
|-----------------|---------|------|
|  fileName               |    문자열     |   제출을 위해 업로드한 ZIP 보관 파일에 있는 이미지 파일의 이름입니다.    |     
|  fileStatus               |   문자열      |  이미지 파일의 상태입니다. 다음 값 중 하나일 수 있습니다. <ul><li>None</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>   |
|  id  |  문자열  | 개발자 센터에서 지정한 대로 이미지에 대한 ID입니다.  |
|  description  |  문자열  | 이미지에 대한 설명입니다.  |
|  imageType  |  문자열  | 이미지의 유형을 나타내는 다음 문자열 중 하나입니다. <ul><li>알 수 없음</li><li>스크린샷</li><li>PromotionalArtwork414X180</li><li>PromotionalArtwork846X468</li><li>PromotionalArtwork558X756</li><li>PromotionalArtwork414X468</li><li>PromotionalArtwork558X558</li><li>PromotionalArtwork2400X1200</li><li>아이콘</li><li>WideIcon358X173</li><li>BackgroundImage1000X800</li><li>SquareIcon358X358</li><li>MobileScreenshot</li><li>XboxScreenshot</li><li>SurfaceHubScreenshot</li><li>HoloLensScreenshot</li></ul>      |


<span id="pricing-object" />
### <a name="pricing-resource"></a>가격 리소스

이 리소스는 앱에 대한 가격 정보를 포함합니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 유형    | 설명        |
|-----------------|---------|------|
|  trialPeriod               |    문자열     |  앱에 대한 평가 기간을 지정하는 문자열입니다. 다음 값 중 하나일 수 있습니다. <ul><li>NoFreeTrial</li><li>OneDay</li><li>TrialNeverExpires</li><li>SevenDays</li><li>FifteenDays</li><li>ThirtyDays</li></ul>    |
|  marketSpecificPricings               |    object     |  키와 값 쌍의 사전입니다. 여기서 각 키는 두 자로 된 ISO 3166-1 alpha-2 국가 코드이며 각 값은 [기준 가격](#price-tiers)입니다. 이러한 항목은 [특정 지역/국가에서 앱에 대한 사용자 지정 가격](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#markets-and-custom-prices)을 나타냅니다. 이 사전의 항목은 지정된 지역/국가의 *priceId* 값으로 지정된 기본 가격을 재정의합니다.      |     
|  sales               |   배열      |  **사용되지 않음**. 앱에 대한 판매 정보를 포함하는 [판매 리소스](#sale-object) 배열입니다.   |     
|  priceId               |   문자열      |  앱에 대한 [기본 가격](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#base-price)을 지정하는 [기준 가격](#price-tiers)입니다.   |     
|  isAdvancedPricingModel               |   boolean      |  **true**인 경우, 개발자 계정은 .99 USD에서 1999.99 USD까지의 확장된 기준 가격 집합에 액세스할 수 있는 권한이 있습니다. **false**인 경우, 개발자 계정은 .99 USD에서 999.99 USD까지의 원래 기준 가격 집합에 액세스할 수 있는 권한이 있습니다. 여러 계층에 대한 자세한 내용은 [기준 가격](#price-tiers)을 참조하세요.<br/><br/>**참고**&nbsp;&nbsp;이 필드는 읽기 전용입니다.   |


<span id="sale-object" />
### <a name="sale-resource"></a>판매 리소스

이 리소스는 앱에 대한 판매 정보를 포함합니다.

>**중요**&nbsp;&nbsp;**판매** 리소스는 더 이상 지원되지 않으며, 현재 Windows 스토어 제출 API를 사용하여 앱 제출에 대한 판매 데이터를 가져오거나 수정할 수 없습니다.

   > * [GET 메서드를 호출하여 앱 제출을 가져오면](get-an-app-submission.md) *판매* 값이 빈 상태로 표시됩니다. 계속해서 Windows 개발자 센터 대시보드를 사용하여 앱 제출에 대한 판매 데이터를 가져올 수 있습니다.
   > * [PUT 메서드를 호출하여 앱 제출을 업데이트하는 경우](update-an-app-submission.md) *판매* 값의 정보는 무시됩니다. 계속해서 Windows 개발자 센터 대시보드를 사용하여 앱 제출에 대한 판매 데이터를 변경할 수 있습니다.

> 앞으로 Windows 스토어 제출 API를 업데이트하여 앱 제출에 대한 판매 정보에 프로그래밍 방식으로 액세스하는 새로운 방법을 도입할 예정입니다.

이 리소스의 값은 다음과 같습니다.

| 값           | 유형    | 설명    |
|-----------------|---------|------|
|  name               |    문자열     |   판매의 이름입니다.    |     
|  basePriceId               |   문자열      |  판매의 기본 가격으로 사용할 [기준 가격](#price-tiers)입니다.    |     
|  startDate               |   문자열      |   판매의 시작 날짜(ISO 8601 형식)입니다.  |     
|  endDate               |   문자열      |  판매의 종료 날짜(ISO 8601 형식)입니다.      |     
|  marketSpecificPricings               |   object      |   키와 값 쌍의 사전입니다. 여기서 각 키는 두 자로 된 ISO 3166-1 alpha-2 국가 코드이며 각 값은 [기준 가격](#price-tiers)입니다. 이러한 항목은 [특정 지역/국가에서 앱에 대한 사용자 지정 가격](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#markets-and-custom-prices)을 나타냅니다. 이 사전의 항목은 지정된 지역/국가의 *basePriceId* 값으로 지정된 기본 가격을 재정의합니다.    |


<span id="status-details-object" />
### <a name="status-details-resource"></a>상태 세부 정보 리소스

이 리소스에는 제출 상태에 대한 추가 세부 정보가 포함됩니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 유형    | 설명         |
|-----------------|---------|------|
|  errors               |    object     |   제출에 대한 오류 세부 정보가 포함된 [상태 세부 정보 리소스](#status-detail-object)의 배열입니다.    |     
|  warnings               |   object      | 제출에 대한 경고 세부 정보가 포함된 [상태 세부 정보 리소스](#status-detail-object)의 배열입니다.      |
|  certificationReports               |     object    |   제출에 대한 인증 보고서 데이터에 대한 액세스를 제공하는 [인증 보고서 리소스](#certification-report-object)의 배열입니다. 인증에 실패할 경우 이러한 보고서에서 자세한 내용을 확인할 수 있습니다.   |  


<span id="status-detail-object" />
### <a name="status-detail-resource"></a>상태 세부 정보 리소스

이 리소스에는 제출과 관련된 오류 또는 경고에 대한 추가 정보가 포함되어 있습니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 유형    | 설명        |
|-----------------|---------|------|
|  code               |    문자열     |   오류 또는 경고의 유형을 설명하는 [제출 상태 코드](#submission-status-code)입니다.   |     
|  details               |     문자열    |  문제에 대한 자세한 정보가 있는 메시지입니다.     |


<span id="application-package-object" />
### <a name="application-package-resource"></a>응용 프로그램 패키지 리소스

이 리소스는 제출의 앱 패키지에 대한 세부 정보를 포함합니다.

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

>**참고**&nbsp;&nbsp;[앱 제출 업데이트](update-an-app-submission.md)를 호출할 때는 요청 본문에 이 개체의 *fileName*, *fileStatus*, *minimumDirectXVersion*, *minimumSystemRam* 값만 필요합니다. 다른 값은 개발자 센터에 의해 채워집니다.

| 값           | 유형    | 설명                   |
|-----------------|---------|------|
| fileName   |   문자열      |  패키지의 이름입니다.    |  
| fileStatus    | 문자열    |  패키지의 상태입니다. 다음 값 중 하나일 수 있습니다. <ul><li>None</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>    |  
| id    |  문자열   |  패키지를 고유하게 식별하는 ID입니다. 이 값은 개발자 센터에서 사용됩니다.   |     
| version    |  문자열   |  앱 패키지의 버전입니다. 자세한 내용은 [패키지 버전 번호](https://msdn.microsoft.com/windows/uwp/publish/package-version-numbering)를 참조하세요.   |   
| architecture    |  문자열   |  패키지의 아키텍처(예: ARM)입니다.   |     
| languages    | 배열    |  앱에서 지원하는 언어의 언어 코드 배열입니다. 자세한 내용은 [지원되는 언어](https://msdn.microsoft.com/windows/uwp/publish/supported-languages)를 참조하세요.    |     
| capabilities    |  배열   |  패키지에 필요한 접근 권한 값의 배열입니다. 접근 권한 값에 대한 자세한 내용은 [앱 접근 권한 값 선언](https://msdn.microsoft.com/windows/uwp/packaging/app-capability-declarations)을 참조하세요.   |     
| minimumDirectXVersion    |  문자열   |  앱 패키지에서 지원되는 최소 DirectX 버전입니다. Windows8.x를 대상으로 하는 앱에 대해서만 설정할 수 있습니다. 다른 버전을 대상으로 하는 앱에 대해서는 무시됩니다. 다음 값 중 하나일 수 있습니다. <ul><li>None</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | 문자열    |  앱 패키지에 필요한 최소 RAM입니다. Windows8.x를 대상으로 하는 앱에 대해서만 설정할 수 있습니다. 다른 버전을 대상으로 하는 앱에 대해서는 무시됩니다. 다음 값 중 하나일 수 있습니다. <ul><li>None</li><li>Memory2GB</li></ul>   |       
| targetDeviceFamilies    | 배열    |  패키지가 대상으로 하는 장치 패밀리를 나타내는 문자열의 배열입니다. 이 값은 Windows 10을 대상으로 하는 패키지에만 사용되며 이전 릴리스를 대상으로 하는 패키지의 경우 이 값은 **None** 값을 갖습니다. 다음 장치 패밀리 문자열은 현재 Windows10 패키지에 지원됩니다. 여기서 *{0}*은 10.0.10240.0, 10.0.10586.0 또는 10.0.14393.0과 같은 Windows10 버전 문자열입니다. <ul><li>Windows.Universal 최소 버전 *{0}*</li><li>Windows.Desktop 최소 버전 *{0}*</li><li>Windows.Mobile 최소 버전 *{0}*</li><li>Windows.Xbox 최소 버전 *{0}*</li><li>Windows.Holographic 최소 버전 *{0}*</li></ul>   |    

<span/>

<span id="certification-report-object" />
### <a name="certification-report-resource"></a>인증 보고서 리소스

이 리소스는 제출의 인증 보고서 데이터에 대한 액세스를 제공합니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 유형    | 설명             |
|-----------------|---------|------|
|     date            |    문자열     |  보고서가 생성된 날짜 및 시간(ISO 8601 형식)입니다.    |
|     reportUrl            |    문자열     |  보고서에 액세스할 수 있는 URL입니다.    |


<span id="package-delivery-options-object" />
### <a name="package-delivery-options-resource"></a>패키지 전송 옵션 리소스

이 리소스는 제출에 대한 점진적 패키지 출시 및 필수 업데이트 설정을 포함합니다.

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

| 값           | 유형    | 설명        |
|-----------------|---------|------|
| packageRollout   |   object      |  제출에 대한 점진적 패키지 출시 설정을 포함하는 [패키지 출시 리소스](#package-rollout-object)입니다.   |  
| isMandatoryUpdate    | boolean    |  이 제출의 패키지를 앱 업데이트 자동 설치를 필수 구성 요소로 처리할지 여부를 나타냅니다. 앱 업데이트 자동 설치를 필수 패키지에 대한 자세한 내용은 [앱에 대한 패키지 업데이트 다운로드 및 설치](../packaging/self-install-package-updates.md)를 참조하세요.    |  
| mandatoryUpdateEffectiveDate    |  date   |  이 제출의 패키지가 필수가 되는 날짜 및 시간을 ISO 8601 형식 및 UTC 표준 시간대로 나타낸 것입니다.   |        

<span id="package-rollout-object" />
### <a name="package-rollout-resource"></a>패키지 출시 리소스

이 리소스는 제출에 대한 점진적 [패키지 출시 설정](#manage-gradual-package-rollout)을 포함합니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 유형    | 설명        |
|-----------------|---------|------|
| isPackageRollout   |   boolean      |  제출에 대해 점진적 패키지 출시를 사용하도록 설정할지 여부를 나타냅니다.    |  
| packageRolloutPercentage    | float    |  점진적 출시에서 패키지를 받을 사용자의 백분율입니다.    |  
| packageRolloutStatus    |  문자열   |  점진적 패키지 출시의 상태를 나타내는 다음 문자열 중 하나입니다. <ul><li>PackageRolloutNotStarted</li><li>PackageRolloutInProgress</li><li>PackageRolloutComplete</li><li>PackageRolloutStopped</li></ul>  |  
| fallbackSubmissionId    |  문자열   |  점진적 출시 패키지를 가져오지 않는 고객이 수신할 제출의 ID입니다.   |          

>**참고**&nbsp;&nbsp;*packageRolloutStatus* 및 *fallbackSubmissionId* 값은 개발자 센터에서 할당하며, 개발자가 설정할 필요가 없습니다. 요청 본문에 이러한 값을 포함하면 값이 무시됩니다. 

<span/>

## <a name="enums"></a>열거

이러한 메서드는 다음 열거형을 사용합니다.

<span id="price-tiers" />
### <a name="price-tiers"></a>기준 가격

다음 값은 앱 제출에 대한 [가격 리소스](#pricing-object) 리소스에서 사용할 수 있는 기준 가격을 나타냅니다.

| 값           | 설명        |
|-----------------|------|
|  기본               |   기준 가격이 설정되지 않았습니다. 앱에 대한 기본 가격을 사용합니다.      |     
|  NotAvailable              |   지정된 영역에 앱을 사용할 수 없습니다.    |     
|  무료              |   앱은 무료입니다.    |    
|  계층*xxx*               |   앱에 대한 기준 가격을 **계층<em>xxxx</em>** 형식으로 지정하는 문자열입니다. 현재 다음 범위의 기준 가격이 지원됩니다.<br/><br/><ul><li>[가격 리소스](#pricing-object)의 *isAdvancedPricingModel* 값이 **true**인 경우, 사용자 계정에 대해 사용 가능한 기준 가격 값은 **Tier1012** - **Tier1424**입니다.</li><li>[가격 리소스](#pricing-object)의 *isAdvancedPricingModel* 값이 **false**인 경우, 사용자 계정에 대해 사용 가능한 기준 가격 값은 **Tier2** - **Tier96**입니다.</li></ul>각 계층과 관련된 시장별 가격을 포함하여 개발자 계정에 사용할 수 있는 기준 가격을 나열한 전체 표를 보려면 개발자 센터 대시보드에서 앱 제출에 대한 **가격 책정 및 가용성** 페이지로 이동하여 **지역/국가 및 사용자 지정 가격** 섹션에 있는 **표 보기** 링크를 클릭합니다(일부 개발자 계정의 경우, 이 링크는 **가격 책정** 섹션에 있음).    |


<span id="enterprise-licensing" />
### <a name="enterprise-licensing-values"></a>엔터프라이즈 라이선스 값

다음 값은 앱에 대한 엔터프라이즈 라이선스 동작을 나타냅니다. 이러한 옵션에 대한 자세한 내용은 [조직 라이선스 옵션](https://msdn.microsoft.com/windows/uwp/publish/organizational-licensing)을 참조하세요.

| 값           |  설명      |
|-----------------|---------------|
| None            |     엔터프라이즈에서 스토어 관리(온라인) 볼륨 라이선싱으로 앱을 사용할 수 없습니다.         |     
| 온라인        |     엔터프라이즈에서 스토어 관리(온라인) 볼륨 라이선싱으로 앱을 사용할 수 있습니다.  |
| OnlineAndOffline | 엔터프라이즈에서 스토어 관리(온라인) 볼륨 라이선싱으로 앱을 사용하고, 연결이 끊어진(오프라인) 라이선싱을 통해 앱을 사용할 수 있습니다. |


<span id="submission-status-code" />
### <a name="submission-status-code"></a>제출 상태 코드

다음 값은 제출의 상태 코드를 나타냅니다.

| 값           |  설명      |
|-----------------|---------------|
| None            |     코드가 지정되지 않았습니다.         |     
| InvalidArchive        |     패키지가 포함된 ZIP 보관 파일이 잘못되거나 인식할 수 없는 보관 파일 형식입니다.  |
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

* [Windows 스토어 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)
* [Windows 스토어 제출 API를 사용하여 앱 데이터 가져오기](get-app-data.md)
* [Windows 개발자 센터 대시보드에서 앱 제출](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)
