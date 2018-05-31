---
author: mcleanbyron
ms.assetid: 2A454057-FF14-40D2-8ED2-CEB5F27E0226
description: Microsoft Store 제출 API에서 다음 메서드를 사용하여 Windows 개발자 센터 계정에 등록된 앱을 위한 패키지 플라이트 제출을 관리합니다.
title: 패키지 플라이트 제출 관리
ms.author: mcleans
ms.date: 04/16/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store 제출 API, 플라이트 제출
ms.localizationpriority: medium
ms.openlocfilehash: 7a9cba76b693a871d10ee1f14fd9023bd166b0de
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/30/2018
ms.locfileid: "1817341"
---
# <a name="manage-package-flight-submissions"></a>패키지 플라이트 제출 관리

Microsoft Store 제출 API는 점진적 패키지 출시를 포함하여 앱의 패키지 플라이트 제출을 관리하는 데 사용할 수 있는 메서드를 제공합니다. API 사용을 위한 필수 조건을 비롯하여 Microsoft Store 제출 API에 대한 자세한 내용은 [Microsoft Store 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)를 참조하세요.

> [!IMPORTANT]
> Microsoft Store 제출 API를 사용하여 패키지 플라이트에 대한 제출을 생성하는 경우 개발자 센터 대시보드 대신 API만 사용하여 제출을 추가 변경해야 합니다. 대시보드를 사용하여 원래 API로 만든 제출을 변경하는 경우 더 이상 API를 사용하여 해당 제출을 변경하거나 커밋할 수 없습니다. 경우에 따라 제출이 제출 프로세스를 더 이상 진행할 수 없는 오류 상태로 남을 수 있습니다. 이러한 문제가 발생하는 경우, 해당 제출을 삭제하고 새 제출을 생성해야 합니다.

<span id="methods-for-package-flight-submissions" />

## <a name="methods-for-managing-package-flight-submissions"></a>패키지 플라이트 제출 관리 메서드

패키지 플라이트 제출 가져오기, 만들기, 업데이트, 커밋, 삭제 작업에는 다음 메서드를 사용합니다. 이러한 메서드를 사용하려면 먼저 패키지 플라이트가 개발자 센터 계정에 이미 있어야 합니다. [Windows 개발자 센터 대시보드를 사용](https://msdn.microsoft.com/windows/uwp/publish/package-flights)하거나 [패키지 플라이트 관리](manage-flights.md)에 설명된 Microsoft Store 제출 API 메서드를 사용하여 패키지 플라이트를 만들 수 있습니다.

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
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}</td>
<td align="left"><a href="get-a-flight-submission.md">기존 패키지 플라이트 제출 가져오기</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status</td>
<td align="left"><a href="get-status-for-a-flight-submission.md">기존 패키지 플라이트 제출의 상태 가져오기</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions</td>
<td align="left"><a href="create-a-flight-submission.md">새 패키지 플라이트 제출 만들기</a></td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}</td>
<td align="left"><a href="update-a-flight-submission.md">기존 패키지 플라이트 제출 업데이트</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit</td>
<td align="left"><a href="commit-a-flight-submission.md">새로운 또는 업데이트된 패키지 플라이트 제출 커밋</a></td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}</td>
<td align="left"><a href="delete-a-flight-submission.md">패키지 플라이트 제출 삭제</a></td>
</tr>
</tbody>
</table>

<span id="create-a-package-flight-submission">

## <a name="create-a-package-flight-submission"></a>패키지 플라이트 제출 만들기

패키지 플라이트에 대한 제출을 만들려면 이 프로세스를 따릅니다.

1. 아직 수행하지 않은 경우 Windows 개발자 센터 계정으로 Azure AD 응용 프로그램 연결 및 클라이언트 ID와 키 얻기를 비롯하여 [Microsoft Store 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)에 설명된 필수 조건을 완료합니다. 이 작업은 한 번만 수행하면 됩니다. 클라이언트 ID와 키를 얻은 후에는 새 Azure AD 액세스 토큰을 만들어야 할 때마다 다시 사용할 수 있습니다.  

2. [Azure AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Microsoft Store 제출 API의 메서드에 이 액세스 토큰을 전달해야 합니다. 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

3. Microsoft Store 제출 API에서 다음 메서드를 실행하여 [패키지 플라이트 제출을 만듭니다](create-a-flight-submission.md). 이 메서드는 마지막으로 게시된 제출의 복사본인 새 진행 중 제출을 만듭니다.

    ```
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications{applicationId}/flights/{flightId}/submissions
    ```

    응답 본문에는 새 제출 ID, Azure Blob Storage 제출을 위한 패키지 업로딩용 SAS(Shared Access Signature) RUI, 새 제출에 대한 데이터(모든 목록과 가격 정보 등)가 포함된 [플라이트 제출](#flight-submission-object) 리소스가 포함되어 있습니다.

    > [!NOTE]
    > SAS URI는 계정 키를 요구하지 않고 Azure Storage의 보안 리소스에 대한 액세스를 제공합니다. SAS URI 및 Azure Blob Storage를 통한 SAS URI 사용에 대한 배경 정보는 [공유 액세스 서명, 1부: SAS 모델 이해](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1) 및 [공유 액세스 서명, 2부: Blob Storage를 사용하여 SAS 만들기 및 사용](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/)을 참조하세요.

4. 제출에 대한 새 패키지를 추가하는 경우 [패키지를 준비](https://msdn.microsoft.com/windows/uwp/publish/app-package-requirements)하고 ZIP 보관 파일에 추가합니다.

5. 새 제출에 필요한 변경 사항으로 [플라이트 제출](#flight-submission-object) 데이터를 수정하고 다음 메서드를 실행하여 [패키지 플라이트 제출을 업데이트](update-a-flight-submission.md)합니다.

    ```
    PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}
    ```
      > [!NOTE]
      > 제출에 대한 새 패키지를 추가하는 경우 ZIP 보관 파일에서 이러한 파일의 상대 경로 및 이름을 참조하도록 제출 데이터를 업데이트해야 합니다.

4. 제출에 대한 새 패키지를 추가하는 경우 이전에 호출한 POST 메서드의 응답 본문에 제공된 SAS URI를 사용하여 [Azure Blob Storage](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage)에 ZIP 보관 파일을 업로드합니다. 다음과 같이 다양한 플랫폼에서 이 작업을 수행할 수 있는 여러 Azure 라이브러리가 있습니다.

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

5. 다음 메서드를 실행하여 [패키지 플라이트 제출을 커밋](commit-a-flight-submission.md)합니다. 이렇게 하면 제출이 완료되었으며 업데이트를 해당 계정에 지금 적용해야 한다는 사실을 개발자 센터에 알려줍니다.

    ```
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit
    ```

6. 다음 메서드 실행을 통해 [패키지 플라이트 제출의 상태를 가져와](get-status-for-a-flight-submission.md) 커밋 상태를 확인합니다.

    ```
    GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status
    ```

    제출 상태를 확인하려면 응답 본문에서 *status* 값을 검토합니다. 이 값은 요청이 성공한 경우 **CommitStarted**에서 **PreProcessing**으로, 요청에 오류가 발생한 경우 **CommitFailed**로 변경됩니다. 오류가 있는 경우 *statusDetails* 필드에 오류에 대한 추가 정보가 포함됩니다.

7. 커밋이 성공적으로 완료되면 수집을 위해 제출이 스토어로 전송됩니다. 이전 메서드를 사용하거나 Windows 개발자 센터 대시보드를 방문하여 제출 진행 상황을 계속 모니터링할 수 있습니다.

<span/>

## <a name="code-examples"></a>코드 예제

다음 문서는 여러 다른 프로그래밍 언어로 패키지 플라이트 제출을 만드는 방법을 보여 주는 자세한 코드 예제를 제공합니다.

* [C# 코드 예제](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Java 코드 예제](java-code-examples-for-the-windows-store-submission-api.md)
* [Python 코드 예제](python-code-examples-for-the-windows-store-submission-api.md)

## <a name="storebroker-powershell-module"></a>StoreBroker PowerShell 모듈

Microsoft Store 제출 API를 직접 호출하는 대신 이 API 위에 명령줄 인터페이스를 구현하는 오픈 소스 PowerShell 모듈도 제공합니다. 이 모듈을 [StoreBroker](https://aka.ms/storebroker)라고 합니다. 이 모듈을 사용하여 Microsoft Store 제출 API를 직접 호출하는 대신 명령줄에서 앱, 플라이트 및 추가 기능 제출을 관리할 수 있습니다. 또는 소스에서 이 API를 호출하는 방법에 대한 예제를 더 찾아볼 수 있습니다. StoreBroker 모듈은 많은 자사 응용 프로그램이 스토어에 제출되는 기본 방식으로 Microsoft 내에서 많이 사용됩니다.

자세한 내용은 [GitHub의 StoreBroker 페이지](https://aka.ms/storebroker)를 참조하세요.

<span id="manage-gradual-package-rollout">

## <a name="manage-a-gradual-package-rollout-for-a-package-flight-submission"></a>패키지 플라이트 제출의 점진적 패키지 출시 관리

패키지 플라이트 제출에서 업데이트된 패키지를 앱의 Windows10 고객의 비율로 점진적으로 배포할 수 있습니다. 이렇게 하면 피드백 및 분석 데이터를 모니터링하여 보다 광범위하게 출시하기 전에 업데이트의 품질을 확인할 수 있습니다. 새 제출을 만들지 않고도 게시된 제출에 대한 배포 백분율을 변경(또는 업데이트를 중단)할 수 있습니다. Windows 개발자 센터 대시보드에서 점진적 패키지 출시를 사용하도록 설정하고 관리하는 방법에 대한 지침을 비롯한 자세한 내용은 [이 문서](../publish/gradual-package-rollout.md)를 참조하세요.

Microsoft Store 제출 API에서 여러 메서드를 사용하여 이 프로세스를 따르면 패키지 플라이트 제출에 대해 프로그래밍 방식으로 점진적 패키지 출시를 사용하도록 설정할 수 있습니다.

  1. [패키지 플라이트 제출 만들기](create-a-flight-submission.md) 또는 [패키지 플라이트 제출 가져오기](get-a-flight-submission.md).
  2. 응답 데이터에서 [packageRollout](#package-rollout-object) 리소스를 찾고, *isPackageRollout* 필드를 true로 설정하고, *packageRolloutPercentage* 필드를 업데이트된 패키지를 가져와야 하는 앱 고객의 백분율로 설정합니다.
  3. 업데이트된 패키지 플라이트 제출 데이터를 [패키지 플라이트 제출 업데이트](update-a-flight-submission.md) 메서드에 전달합니다.

패키지 플라이트 제출에 대해 점진적 패키지 출시를 사용할 수 있도록 설정한 후 다음 메서드를 사용하여 프로그래밍 방식으로 점진적 출시를 가져오거나 업데이트, 중지, 완료할 수 있습니다.

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
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/packagerollout</td>
<td align="left"><a href="get-package-rollout-info-for-a-flight-submission.md">패키지 플라이트 제출에 대한 점진적 출시 정보 가져오기</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/updatepackagerolloutpercentage</td>
<td align="left"><a href="update-the-package-rollout-percentage-for-a-flight-submission.md">패키지 플라이트 제출에 대한 점진적 출시 백분율 업데이트</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/haltpackagerollout</td>
<td align="left"><a href="halt-the-package-rollout-for-a-flight-submission.md">패키지 플라이트 제출에 대한 점진적 출시 중지</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/finalizepackagerollout</td>
<td align="left"><a href="finalize-the-package-rollout-for-a-flight-submission.md">패키지 플라이트 제출에 대한 점진적 출시 마무리</a></td>
</tr>
</tbody>
</table>

<span/>

## <a name="data-resources"></a>데이터 리소스

패키지 플라이트 제출을 관리하는 Microsoft Store 제출 API 메서드는 다음과 같은 JSON 데이터 리소스를 사용합니다.

<span id="flight-submission-object" />

### <a name="flight-submission-resource"></a>플라이트 제출 리소스

이 리소스는 패키지 플라이트 제출에 대해 설명합니다.

```json
{
  "id": "1152921504621243649",
  "flightId": "cd2e368a-0da5-4026-9f34-0e7934bc6f23",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
  "flightPackages": [
    {
      "fileName": "newPackage.appx",
      "fileStatus": "PendingUpload",
      "id": "",
      "version": "1.0.0.0",
      "languages": ["en-us"],
      "capabilities": [],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None"
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
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/8b389577-5d5e-4cbe-a744-1ff2e97a9eb8?sv=2014-02-14&sr=b&sig=wgMCQPjPDkuuxNLkeG35rfHaMToebCxBNMPw7WABdXU%3D&se=2016-06-17T21:29:44Z&sp=rwl",
  "targetPublishMode": "Immediate",
  "targetPublishDate": "",
  "notesForCertification": "No special steps are required for certification of this app."
}
```

이 리소스의 값은 다음과 같습니다.

| 값      | 유형   | 설명              |
|------------|--------|------------------------------|
| id            | 문자열  | 제출 ID입니다.  |
| flightId           | 문자열  |  제출이 연결된 패키지 플라이트의 ID입니다.  |  
| status           | 문자열  | 제출의 상태입니다. 다음 값 중 하나일 수 있습니다. <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>릴리스</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | object  |  오류에 대한 정보를 비롯하여 제출 상태에 대한 추가 세부 정보가 포함된 [상태 세부 정보 리소스](#status-details-object)입니다.  |
| flightPackages           | 배열  | 제출의 각 패키지에 대한 세부 정보를 제공하는 [플라이트 패키지 리소스](#flight-package-object)가 포함됩니다.   |
| packageDeliveryOptions    | object  | 제출에 대한 점진적 패키지 출시 및 필수 업데이트 설정을 포함하는 [패키지 전송 옵션 리소스](#package-delivery-options-object)입니다.   |
| fileUploadUrl           | 문자열  | 제출에 대한 패키지를 업로드하기 위한 SAS(공유 액세스 서명) URI입니다. 제출에 대한 새 패키지를 추가하는 경우 패키지가 포함된 ZIP 보관 파일을 이 URI에 업로드합니다. 자세한 내용은 [패키지 플라이트 제출 만들기](#create-a-package-flight-submission)를 참조하세요.  |
| targetPublishMode           | 문자열  | 제출의 게시 모드입니다. 다음 값 중 하나일 수 있습니다. <ul><li>즉시</li><li>수동</li><li>SpecificDate</li></ul> |
| targetPublishDate           | 문자열  | *targetPublishMode*가 SpecificDate로 설정된 경우 제출의 게시 날짜(ISO 8601 형식)입니다.  |
| notesForCertification           | 문자열  |  테스트 계정 자격 증명, 기능 액세스 및 확인 단계 등 인증 테스터에 대한 추가 정보를 제공합니다. 자세한 내용은 [인증에 대한 참고 사항](https://msdn.microsoft.com/windows/uwp/publish/notes-for-certification)을 참조하세요. |

<span id="status-details-object" />

### <a name="status-details-resource"></a>상태 세부 정보 리소스

이 리소스에는 제출 상태에 대한 추가 세부 정보가 포함됩니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 유형    | 설명                   |
|-----------------|---------|------|
|  errors               |    object     |   제출에 대한 오류 세부 정보가 포함된 [상태 세부 정보 리소스](#status-detail-object)의 배열입니다.   |     
|  warnings               |   object      | 제출에 대한 경고 세부 정보가 포함된 [상태 세부 정보 리소스](#status-detail-object)의 배열입니다.     |
|  certificationReports               |     object    |   제출에 대한 인증 보고서 데이터에 대한 액세스를 제공하는 [인증 보고서 리소스](#certification-report-object)의 배열입니다. 인증에 실패할 경우 이러한 보고서에서 자세한 내용을 확인할 수 있습니다.    |  


<span id="status-detail-object" />

### <a name="status-detail-resource"></a>상태 세부 정보 리소스

이 리소스에는 제출과 관련된 오류 또는 경고에 대한 추가 정보가 포함되어 있습니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 유형    | 설명       |
|-----------------|---------|------|
|  code               |    문자열     |   오류 또는 경고의 유형을 설명하는 [제출 상태 코드](#submission-status-code)입니다. |  
|  details               |     문자열    |  문제에 대한 자세한 정보가 있는 메시지입니다.     |


<span id="certification-report-object" />

### <a name="certification-report-resource"></a>인증 보고서 리소스

이 리소스는 제출의 인증 보고서 데이터에 대한 액세스를 제공합니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 유형    | 설명         |
|-----------------|---------|------|
|     date            |    문자열     |  보고서가 생성된 날짜 및 시간(ISO 8601 형식)입니다.    |
|     reportUrl            |    문자열     |  보고서에 액세스할 수 있는 URL입니다.    |


<span id="flight-package-object" />

### <a name="flight-package-resource"></a>플라이트 패키지 리소스

이 리소스는 제출의 패키지에 대한 세부 정보를 제공합니다.

```json
{
  "flightPackages": [
    {
      "fileName": "newPackage.appx",
      "fileStatus": "PendingUpload",
      "id": "",
      "version": "1.0.0.0",
      "languages": ["en-us"],
      "capabilities": [],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None"
    }
  ],
}
```

이 리소스의 값은 다음과 같습니다.

> [!NOTE]
> [update a package flight submission](update-a-flight-submission.md) 메서드를 호출할 때는 요청 본문에 이 개체의 *fileName*, *fileStatus*, *minimumDirectXVersion* 및 *minimumSystemRam* 값만 필요합니다. 다른 값은 개발자 센터에 의해 채워집니다.

| 값           | 유형    | 설명              |
|-----------------|---------|------|
| fileName   |   문자열      |  패키지의 이름입니다.    |  
| fileStatus    | 문자열    |  패키지의 상태입니다. 다음 값 중 하나일 수 있습니다. <ul><li>None</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>    |  
| id    |  문자열   |  패키지를 고유하게 식별하는 ID입니다. 이 값은 개발자 센터에서 사용됩니다.   |     
| version    |  문자열   |  앱 패키지의 버전입니다. 자세한 내용은 [패키지 버전 번호](https://msdn.microsoft.com/windows/uwp/publish/package-version-numbering)를 참조하세요.   |   
| architecture    |  문자열   |  앱 패키지의 아키텍처(예: ARM)입니다.   |     
| languages    | 배열    |  앱에서 지원하는 언어의 언어 코드 배열입니다. 자세한 내용은 [지원되는 언어](https://msdn.microsoft.com/windows/uwp/publish/supported-languages)를 참조하세요.    |     
| capabilities    |  배열   |  패키지에 필요한 접근 권한 값의 배열입니다. 접근 권한 값에 대한 자세한 내용은 [앱 접근 권한 값 선언](https://msdn.microsoft.com/windows/uwp/packaging/app-capability-declarations)을 참조하세요.   |     
| minimumDirectXVersion    |  문자열   |  앱 패키지에서 지원되는 최소 DirectX 버전입니다. Windows8.x를 대상으로 하는 앱에 대해서만 설정할 수 있습니다. 다른 버전을 대상으로 하는 앱에 대해서는 무시됩니다. 다음 값 중 하나일 수 있습니다. <ul><li>None</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | 문자열    |  앱 패키지에 필요한 최소 RAM입니다. Windows8.x를 대상으로 하는 앱에 대해서만 설정할 수 있습니다. 다른 버전을 대상으로 하는 앱에 대해서는 무시됩니다. 다음 값 중 하나일 수 있습니다. <ul><li>None</li><li>Memory2GB</li></ul>   |    


<span id="package-delivery-options-object" />

### <a name="package-delivery-options-resource"></a>패키지 전송 옵션 리소스

이 리소스는 제출에 대한 점진적 패키지 출시 및 필수 업데이트 설정을 포함합니다.

```json
{
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
}
```

이 리소스의 값은 다음과 같습니다.

| 값           | 유형    | 설명        |
|-----------------|---------|------|
| packageRollout   |   object      |   제출에 대한 점진적 패키지 출시 설정을 포함하는 [패키지 출시 리소스](#package-rollout-object)입니다.    |  
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

> [!NOTE]
> *packageRolloutStatus* 및 *fallbackSubmissionId* 값은 개발자 센터에서 할당하며, 개발자가 설정할 필요가 없습니다. 요청 본문에 이러한 값을 포함하면 값이 무시됩니다.

<span/>

## <a name="enums"></a>열거

이러한 메서드는 다음 열거형을 사용합니다.

<span id="submission-status-code" />

### <a name="submission-status-code"></a>제출 상태 코드

다음 코드는 제출 상태를 나타냅니다.

| 코드           |  설명      |
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
* [Microsoft Store 제출 API를 사용하여 패키지 플라이트 관리](manage-flights.md)
* [패키지 플라이트 제출 가져오기](get-a-flight-submission.md)
* [패키지 플라이트 제출 만들기](create-a-flight-submission.md)
* [패키지 플라인트 제출 업데이트](update-a-flight-submission.md)
* [패키지 플라이트 제출 커밋](commit-a-flight-submission.md)
* [패키지 플라이트 제출 삭제](delete-a-flight-submission.md)
* [패키지 플라이트 제출 상태 가져오기](get-status-for-a-flight-submission.md)
