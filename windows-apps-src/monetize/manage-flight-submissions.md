---
ms.assetid: 2A454057-FF14-40D2-8ED2-CEB5F27E0226
description: Microsoft Store 제출 API에서 이러한 메서드를 사용 하 여 파트너 센터 계정에 등록 된 앱에 대 한 패키지 비행 제출을 관리할 수 있습니다.
title: 패키지 비행 서브 미션 관리
ms.date: 04/16/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 비행 서브 미션
ms.localizationpriority: medium
ms.openlocfilehash: 50596fdadae2ac4a0625687e7c8acaf985ccfaa7
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690371"
---
# <a name="manage-package-flight-submissions"></a>패키지 비행 서브 미션 관리

Microsoft Store 제출 API는 점진적 패키지 롤아웃을 비롯 하 여 앱에 대 한 패키지 비행 서브 미션을 관리 하는 데 사용할 수 있는 메서드를 제공 합니다. API를 사용 하기 위한 필수 구성 요소를 포함 하 여 Microsoft Store 제출 API에 대 한 소개는 [Microsoft Store services를 사용 하 여 전송 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)를 참조 하세요.

> [!IMPORTANT]
> Microsoft Store 제출 API를 사용 하 여 패키지 항공편에 대 한 제출을 만들려면 파트너 센터가 아닌 API를 사용 하 여 제출을 추가로 변경 해야 합니다. 대시보드를 사용 하 여 원래 API를 사용 하 여 만든 제출을 변경 하는 경우에는 더 이상 API를 사용 하 여 해당 제출을 변경 하거나 커밋할 수 없습니다. 경우에 따라 제출 프로세스에서 계속할 수 없는 오류 상태로 제출 될 수 있습니다. 이 문제가 발생 하는 경우 제출을 삭제 하 고 새 제출을 만들어야 합니다.

<span id="methods-for-package-flight-submissions" />

## <a name="methods-for-managing-package-flight-submissions"></a>패키지 비행 서브 미션 관리 방법

다음 방법을 사용 하 여 패키지 비행 제출을 가져오거나 생성, 업데이트, 커밋 또는 삭제할 수 있습니다. 이러한 방법을 사용 하기 전에 패키지 비행이 파트너 센터에 이미 있어야 합니다. [파트너 센터에서](https://docs.microsoft.com/windows/uwp/publish/package-flights) 또는 [패키지 관리 항공편](manage-flights.md)에 설명 된의 Microsoft Store 제출 API 메서드를 사용 하 여 패키지를 만들 수 있습니다.

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
<td align="left"><a href="get-a-flight-submission.md">기존 패키지 비행 제출 가져오기</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status</td>
<td align="left"><a href="get-status-for-a-flight-submission.md">기존 패키지 비행 전송의 상태를 가져옵니다.</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions</td>
<td align="left"><a href="create-a-flight-submission.md">새 패키지 비행 전송 만들기</a></td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}</td>
<td align="left"><a href="update-a-flight-submission.md">기존 패키지 비행 제출 업데이트</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit</td>
<td align="left"><a href="commit-a-flight-submission.md">신규 또는 업데이트 된 패키지 비행 전송 커밋</a></td>
</tr>
<tr>
<td align="left">삭제</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}</td>
<td align="left"><a href="delete-a-flight-submission.md">패키지 비행 전송 삭제</a></td>
</tr>
</tbody>
</table>

<span id="create-a-package-flight-submission">

## <a name="create-a-package-flight-submission"></a>패키지 비행 전송 만들기

패키지 항공편에 대 한 제출을 만들려면이 프로세스를 수행 합니다.

1. 아직 수행 하지 않은 경우 Azure AD 응용 프로그램을 파트너 센터 계정에 연결 하 고 클라이언트 ID 및 키를 가져오는 등 [Microsoft Store 서비스를 사용 하 여 전송 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)에 설명 된 필수 구성 요소를 완료 합니다. 이 작업은 한 번만 수행 하면 됩니다. 클라이언트 ID와 키를 받은 후에는 새 Azure AD 액세스 토큰을 만들어야 할 때마다 다시 사용할 수 있습니다.  

2. [AZURE AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Microsoft Store 제출 API의 메서드에이 액세스 토큰을 전달 해야 합니다. 액세스 토큰을 얻은 후 만료 되기 전에 사용 하는 데 60 분이 있습니다. 토큰이 만료 된 후 새 토큰을 얻을 수 있습니다.

3. Microsoft Store 제출 API에서 다음 메서드를 실행 하 여 [패키지 비행 제출을 만듭니다](create-a-flight-submission.md) . 이 메서드는 마지막으로 게시 된 제출의 복사본 인 진행 중인 새 제출을 만듭니다.

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions
    ```

    응답 본문에는 새 제출의 ID, Azure Blob storage에 제출할 패키지를 업로드 하기 위한 SAS (공유 액세스 서명) URI 및 새 전송에 대 한 데이터 (모두 포함)를 포함 하는 [비행 전송](#flight-submission-object) 리소스가 포함 되어 있습니다. 목록 및 가격 정보).

    > [!NOTE]
    > SAS URI는 계정 키를 요구 하지 않고 Azure storage의 보안 리소스에 대 한 액세스를 제공 합니다. SAS Uri 및 Azure Blob storage에서의 사용에 대 한 배경 정보는 [공유 액세스 서명, 1 부: sas 모델](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/) 및 [공유 액세스 서명 이해, 2 부: Blob 저장소를 사용 하 여 sas 만들기 및 사용](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/)을 참조 하세요.

4. 제출할 새 패키지를 추가 하는 경우 패키지를 [준비](https://docs.microsoft.com/windows/uwp/publish/app-package-requirements) 하 고 ZIP 보관 파일에 추가 합니다.

5. 새 제출에 필요한 변경 내용을 사용 하 여 [비행 전송](#flight-submission-object) 데이터를 수정 하 고, 다음 메서드를 실행 하 여 [패키지 비행 제출을 업데이트](update-a-flight-submission.md)합니다.

    ```json
    PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}
    ```
      > [!NOTE]
      > 제출할 새 패키지를 추가 하는 경우 ZIP 보관 파일에 있는 이러한 파일의 이름 및 상대 경로를 참조 하도록 제출 데이터를 업데이트 해야 합니다.

4. 제출할 새 패키지를 추가 하는 경우 이전에 호출한 POST 메서드의 응답 본문에 제공 된 SAS URI를 사용 하 여 ZIP 보관 파일을 [Azure Blob storage](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage) 에 업로드 합니다. 다음과 같은 다양 한 플랫폼에서이 작업을 수행 하는 데 사용할 수 있는 다양 한 Azure 라이브러리가 있습니다.

    * [.NET용 Azure Storage 클라이언트 라이브러리](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-blobs)
    * [Java용 Azure Storage SDK](https://docs.microsoft.com/azure/storage/storage-java-how-to-use-blob-storage)
    * [Azure Storage SDK for Python](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-blob-storage)

    다음 C# 코드 예제에서는 .net 용 Azure Storage 클라이언트 라이브러리에서 [cloudblockblob](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob) 클래스를 사용 하 여 ZIP 보관 파일을 Azure Blob storage에 업로드 하는 방법을 보여 줍니다. 이 예에서는 ZIP 보관 파일이 스트림 개체에 이미 기록 된 것으로 가정 합니다.

    ```csharp
    string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";
    Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
        new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
    await blockBob.UploadFromStreamAsync(stream);
    ```

5. 다음 방법을 실행 하 여 [패키지 비행 제출을 커밋합니다](commit-a-flight-submission.md) . 그러면 전송 작업을 수행 하 고 사용자의 계정에 업데이트가 적용 되도록 파트너 센터에 경고를 표시 합니다.

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit
    ```

6. [패키지 비행 제출의 상태를 가져오려면](get-status-for-a-flight-submission.md)다음 메서드를 실행 하 여 커밋 상태를 확인 합니다.

    ```json
    GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status
    ```

    제출 상태를 확인 하려면 응답 본문에서 *상태* 값을 검토 합니다. 요청에 오류가 있는 경우 요청이 성공 하거나 **Commitstarted가 실패** 한 경우이 값은 **Commitstarted** 에서 **전처리** 로 변경 해야 합니다. 오류가 있는 경우 *statusdetails* 필드에 오류에 대 한 추가 정보가 포함 됩니다.

7. 커밋이 성공적으로 완료 되 면 수집을 위해 스토어로 전송 됩니다. 이전 방법을 사용 하거나 파트너 센터를 방문 하 여 제출 진행 상황을 계속 모니터링할 수 있습니다.

<span/>

## <a name="code-examples"></a>코드 예제

다음 문서에서는 다양 한 프로그래밍 언어로 패키지 비행 제출을 만드는 방법을 보여 주는 자세한 코드 예제를 제공 합니다.

* [C#코드 예제](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Java 코드 예제](java-code-examples-for-the-windows-store-submission-api.md)
* [Python 코드 예제](python-code-examples-for-the-windows-store-submission-api.md)

## <a name="storebroker-powershell-module"></a>을 (를) Broker PowerShell 모듈

Microsoft Store 제출 API를 직접 호출 하는 대신 API를 기반으로 명령줄 인터페이스를 구현 하는 오픈 소스 PowerShell 모듈을 제공 합니다. 이 모듈을이 모듈을이 모듈 [이라고 합니다.](https://aka.ms/storebroker) 이 모듈을 사용 하 여 Microsoft Store 제출 API를 직접 호출 하는 대신 명령줄에서 앱, 비행 및 추가 기능 제출을 관리 하거나 단순히 소스를 검색 하 여이 API를 호출 하는 방법에 대 한 더 많은 예제를 볼 수 있습니다. 여러 자사 응용 프로그램을 저장소에 제출 하는 기본 방법으로 Microsoft 내에서이 기능을 사용 합니다.

자세한 내용은 [GitHub의 microsoft 컴퓨터 브로커 페이지](https://aka.ms/storebroker)를 참조 하세요.

<span id="manage-gradual-package-rollout">

## <a name="manage-a-gradual-package-rollout-for-a-package-flight-submission"></a>패키지 비행 전송에 대 한 점진적 패키지 롤아웃 관리

패키지 비행 제출의 업데이트 된 패키지를 Windows 10에서 앱 고객의 백분율로 점진적으로 롤아웃할 수 있습니다. 이렇게 하면 특정 패키지에 대 한 사용자 의견 및 분석 데이터를 모니터링 하 여 업데이트에 대 한 확신을 확인할 수 있습니다. 새 제출을 만들지 않고도 게시 된 제출에 대 한 출시 백분율 (또는 업데이트 중단)을 변경할 수 있습니다. 파트너 센터에서 점진적 패키지 출시를 사용 하도록 설정 하 고 관리 하는 방법에 대 한 지침을 비롯 한 자세한 내용은 [이 문서](../publish/gradual-package-rollout.md)를 참조 하세요.

패키지 비행 전송에 대 한 점진적 패키지 출시를 프로그래밍 방식으로 사용 하도록 설정 하려면 Microsoft Store 제출 API의 메서드를 사용 하 여이 프로세스를 수행 합니다.

  1. [패키지 비행 전송 만들기](create-a-flight-submission.md) 또는 [패키지 비행 제출 가져오기](get-a-flight-submission.md).
  2. 응답 데이터에서 [packageRollout](#package-rollout-object) 리소스를 찾은 다음 *isPackageRollout* 필드를 true로 설정 하 고 *packageRolloutPercentage* 필드를 업데이트 된 패키지를 가져와야 하는 앱 고객의 백분율로 설정 합니다.
  3. 업데이트 된 패키지 비행 전송 데이터를 [패키지 비행 전송 방법 업데이트](update-a-flight-submission.md) 로 전달 합니다.

패키지 비행 전송에 대 한 점진적 패키지 출시를 사용 하도록 설정한 후에는 다음 방법을 사용 하 여 점진적 출시를 프로그래밍 방식으로 가져오기, 업데이트, 중지 또는 종료할 수 있습니다.

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
<td align="left"><a href="get-package-rollout-info-for-a-flight-submission.md">패키지 비행 전송에 대 한 점진적 출시 정보 가져오기</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/updatepackagerolloutpercentage</td>
<td align="left"><a href="update-the-package-rollout-percentage-for-a-flight-submission.md">패키지 비행 전송에 대 한 점진적 출시 비율 업데이트</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/haltpackagerollout</td>
<td align="left"><a href="halt-the-package-rollout-for-a-flight-submission.md">패키지 비행 전송에 대 한 점진적 출시를 중지 합니다.</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/finalizepackagerollout</td>
<td align="left"><a href="finalize-the-package-rollout-for-a-flight-submission.md">패키지 비행 전송에 대 한 점진적 출시 마무리</a></td>
</tr>
</tbody>
</table>

<span/>

## <a name="data-resources"></a>데이터 리소스

패키지 비행 서브 미션을 관리 하기 위한 Microsoft Store 제출 API 메서드는 다음 JSON 데이터 리소스를 사용 합니다.

<span id="flight-submission-object" />

### <a name="flight-submission-resource"></a>비행 전송 리소스

이 리소스는 패키지 비행 제출을 설명 합니다.

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

| 값      | 형식   | 설명              |
|------------|--------|------------------------------|
| id            | string  | 제출 ID입니다.  |
| flightId           | string  |  전송에 연결 된 패키지 비행의 ID입니다.  |  
| status           | string  | 제출의 상태입니다. 다음 값 중 하나일 수 있습니다. <ul><li>없음</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>게시</li><li>게시됨</li><li>이상 실패</li><li>바꾸면</li><li>PreProcessingFailed</li><li>인증</li><li>CertificationFailed</li><li>해제</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | object  |  오류에 대 한 정보를 포함 하 여 제출 상태에 대 한 추가 정보를 포함 하는 [상태 세부 정보 리소스](#status-details-object) 입니다.  |
| flightPackages           | array  | 제출의 각 패키지에 대 한 세부 정보를 제공 하는 [비행 패키지 리소스](#flight-package-object) 를 포함 합니다.   |
| packageDeliveryOptions    | object  | 전송에 대 한 점진적 패키지 롤아웃 및 필수 업데이트 설정을 포함 하는 [패키지 배달 옵션 리소스](#package-delivery-options-object) 입니다.   |
| fileUploadUrl           | string  | 제출할 패키지를 업로드 하기 위한 SAS (공유 액세스 서명) URI입니다. 제출할 새 패키지를 추가 하는 경우 패키지를 포함 하는 ZIP 보관 파일을이 URI에 업로드 합니다. 자세한 내용은 [패키지 비행 제출 만들기](#create-a-package-flight-submission)를 참조 하세요.  |
| targetPublishMode           | string  | 제출에 대 한 게시 모드입니다. 다음 값 중 하나일 수 있습니다. <ul><li>즉시</li><li>설명서</li><li>SpecificDate</li></ul> |
| Target버전           | string  | *TargetSpecificDate mode* 가로 설정 된 경우 ISO 8601 형식의 제출에 대 한 게시 날짜입니다.  |
| 메모 For인증           | string  |  테스트 계정 자격 증명과 기능에 액세스 하 고 확인 하는 단계와 같은 인증 테스터에 대 한 추가 정보를 제공 합니다. 자세한 내용은 [인증에 대 한 참고 사항](https://docs.microsoft.com/windows/uwp/publish/notes-for-certification)을 참조 하세요. |

<span id="status-details-object" />

### <a name="status-details-resource"></a>상태 정보 리소스

이 리소스는 제출 상태에 대 한 추가 정보를 포함 합니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 형식    | 설명                   |
|-----------------|---------|------|
|  errors               |    object     |   제출에 대 한 오류 정보를 포함 하는 [상태 정보 리소스](#status-detail-object) 의 배열입니다.   |     
|  기록               |   object      | 제출에 대 한 경고 세부 정보를 포함 하는 [상태 정보 리소스](#status-detail-object) 의 배열입니다.     |
|  certificationReports               |     object    |   제출할 인증 보고서 데이터에 대 한 액세스를 제공 하는 [인증 보고서 리소스](#certification-report-object) 의 배열입니다. 이러한 보고서에서 인증에 실패 하는 경우 자세한 정보를 확인할 수 있습니다.    |  


<span id="status-detail-object" />

### <a name="status-detail-resource"></a>상태 정보 리소스

이 리소스에는 전송에 대 한 모든 관련 오류 또는 경고에 대 한 추가 정보가 포함 되어 있습니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 형식    | 설명       |
|-----------------|---------|------|
|  코드               |    string     |   오류 또는 경고 유형을 설명 하는 [제출 상태 코드](#submission-status-code) 입니다. |  
|  세부 정보               |     string    |  문제에 대 한 자세한 정보를 포함 하는 메시지입니다.     |


<span id="certification-report-object" />

### <a name="certification-report-resource"></a>인증 보고서 리소스

이 리소스는 제출에 대 한 인증 보고서 데이터에 대 한 액세스를 제공 합니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 형식    | 설명         |
|-----------------|---------|------|
|     date            |    string     |  보고서가 생성 된 날짜와 시간 (ISO 8601 형식)입니다.    |
|     reportUrl            |    string     |  보고서에 액세스할 수 있는 URL입니다.    |


<span id="flight-package-object" />

### <a name="flight-package-resource"></a>비행 패키지 리소스

이 리소스는 제출의 패키지에 대 한 세부 정보를 제공 합니다.

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
> [패키지 비행 전송 방법 업데이트](update-a-flight-submission.md) 메서드를 호출 하는 경우 요청 본문에이 개체의 *fileName*, *fileStatus,* *,가 중*및 *ram* 값만 필요 합니다. 다른 값은 파트너 센터에서 채워집니다.

| 값           | 형식    | 설명              |
|-----------------|---------|------|
| fileName   |   string      |  패키지의 이름입니다.    |  
| fileStatus    | string    |  패키지의 상태입니다. 다음 값 중 하나일 수 있습니다. <ul><li>없음</li><li>PendingUpload</li><li>업로드됨</li><li>PendingDelete</li></ul>    |  
| id    |  string   |  패키지를 고유 하 게 식별 하는 ID입니다. 이 값은 파트너 센터에서 사용 됩니다.   |     
| 버전    |  string   |  응용 프로그램 패키지의 버전입니다. 자세한 내용은 [패키지 버전 번호 매기기](https://docs.microsoft.com/windows/uwp/publish/package-version-numbering)를 참조 하세요.   |   
| 아키텍처    |  string   |  앱 패키지의 아키텍처 (예: ARM)입니다.   |     
| 언어    | array    |  앱에서 지 원하는 언어에 대 한 언어 코드의 배열입니다. 자세한 내용은 [지원 되는 언어](https://docs.microsoft.com/windows/uwp/publish/supported-languages)를 참조 하세요.    |     
| capabilities    |  array   |  패키지에 필요한 기능 배열입니다. 기능에 대 한 자세한 내용은 [앱 기능 선언](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations)을 참조 하세요.   |     
| 이상 버전    |  string   |  앱 패키지에서 지 원하는 최소 DirectX 버전입니다. 이는 Windows 8. x를 대상으로 하는 앱에 대해서만 설정할 수 있습니다. 다른 버전을 대상으로 하는 앱에 대해서는 무시 됩니다. 다음 값 중 하나일 수 있습니다. <ul><li>없음</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| 이상 및 Ram    | string    |  앱 패키지에 필요한 최소 RAM입니다. 이는 Windows 8. x를 대상으로 하는 앱에 대해서만 설정할 수 있습니다. 다른 버전을 대상으로 하는 앱에 대해서는 무시 됩니다. 다음 값 중 하나일 수 있습니다. <ul><li>없음</li><li>Memory2GB</li></ul>   |    


<span id="package-delivery-options-object" />

### <a name="package-delivery-options-resource"></a>패키지 제공 옵션 리소스

이 리소스에는 전송에 대 한 점진적 패키지 롤아웃 및 필수 업데이트 설정이 포함 되어 있습니다.

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

| 값           | 형식    | 설명        |
|-----------------|---------|------|
| packageRollout   |   object      |   제출에 대 한 점진적 패키지 롤아웃 설정을 포함 하는 [패키지 롤아웃 리소스](#package-rollout-object) 입니다.    |  
| isMandatoryUpdate    | 부울    |  이 제출의 패키지를 자동 설치 앱 업데이트에 대 한 필수로 처리할지 여부를 나타냅니다. 자동 설치 앱 업데이트에 대 한 필수 패키지에 대 한 자세한 내용은 [앱에 대 한 패키지 업데이트 다운로드 및 설치](../packaging/self-install-package-updates.md)를 참조 하세요.    |  
| mandatoryUpdateEffectiveDate    |  date   |  ISO 8601 형식 및 UTC 표준 시간대에서이 제출의 패키지가 필수 인 날짜 및 시간입니다.   |        

<span id="package-rollout-object" />

### <a name="package-rollout-resource"></a>패키지 출시 리소스

이 리소스에는 제출에 대 한 점진적 [패키지 롤아웃 설정이](#manage-gradual-package-rollout) 포함 되어 있습니다. 이 리소스의 값은 다음과 같습니다.

| 값           | 형식    | 설명        |
|-----------------|---------|------|
| isPackageRollout   |   부울      |  제출할 때 점진적 패키지 롤아웃이 사용 되는지 여부를 나타냅니다.    |  
| packageRolloutPercentage    | float    |  점진적 롤아웃에서 패키지를 수신 하는 사용자의 백분율입니다.    |  
| packageRolloutStatus    |  string   |  점진적 패키지 롤아웃 상태를 나타내는 다음 문자열 중 하나입니다. <ul><li>PackageRolloutNotStarted</li><li>PackageRolloutInProgress</li><li>PackageRolloutComplete</li><li>PackageRolloutStopped</li></ul>  |  
| fallbackSubmissionId    |  string   |  점진적 출시 패키지를 가져오지 않는 고객이 받는 제출의 ID입니다.   |          

> [!NOTE]
> *PackageRolloutStatus* 및 *FallbackSubmissionId* 값은 파트너 센터에서 할당 되며 개발자가 설정 하기 위한 것이 아닙니다. 요청 본문에 이러한 값을 포함 하는 경우 이러한 값은 무시 됩니다.

<span/>

## <a name="enums"></a>열거형

이러한 메서드는 다음 열거형을 사용 합니다.

<span id="submission-status-code" />

### <a name="submission-status-code"></a>제출 상태 코드

다음 코드는 제출 상태를 나타냅니다.

| 코드           |  설명      |
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

## <a name="related-topics"></a>관련된 항목

* [Microsoft Store 서비스를 사용 하 여 제출 작성 및 관리](create-and-manage-submissions-using-windows-store-services.md)
* [Microsoft Store 제출 API를 사용 하 여 항공편 패키지 관리](manage-flights.md)
* [패키지 비행 제출 가져오기](get-a-flight-submission.md)
* [패키지 비행 전송 만들기](create-a-flight-submission.md)
* [패키지 비행 제출 업데이트](update-a-flight-submission.md)
* [패키지 비행 제출 커밋](commit-a-flight-submission.md)
* [패키지 비행 전송 삭제](delete-a-flight-submission.md)
* [패키지 비행 제출의 상태를 가져옵니다.](get-status-for-a-flight-submission.md)
