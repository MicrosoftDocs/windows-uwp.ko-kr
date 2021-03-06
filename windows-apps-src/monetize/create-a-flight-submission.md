---
ms.assetid: CD866083-EB7F-4389-A907-FC43DC2FCB5E
description: Microsoft Store 제출 API에서이 메서드를 사용 하 여 파트너 센터 계정에 등록 된 앱에 대 한 새 패키지 비행 전송을 만듭니다.
title: 패키지 플라이트 제출 만들기
ms.date: 08/03/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 비행 전송 만들기
ms.localizationpriority: medium
ms.openlocfilehash: 5b00439656bfd4bcfaa90d976d28c4c71ab81e27
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175077"
---
# <a name="create-a-package-flight-submission"></a>패키지 플라이트 제출 만들기

Microsoft Store 제출 API에서이 메서드를 사용 하 여 앱에 대 한 패키지 항공편에 대 한 새 제출을 만듭니다. 이 방법을 사용 하 여 새 제출을 성공적으로 만든 후에 제출 데이터를 변경 하 여 제출 데이터를 [변경 하 고](update-a-flight-submission.md) 수집 및 게시를 위해 [제출을 커밋합니다](commit-a-flight-submission.md) .

Microsoft Store 제출 API를 사용 하 여 패키지 비행 제출을 만드는 프로세스에이 방법을 사용 하는 방법에 대 한 자세한 내용은 [패키지 비행 전송 관리](manage-flight-submissions.md)를 참조 하세요.

> [!NOTE]
> 이 메서드는 기존 패키지 항공편에 대 한 제출을 만듭니다. 패키지 비행을 만들려면 [패키지 만들기 비행](create-a-flight.md) 메서드를 사용 합니다.

## <a name="prerequisites"></a>필수 구성 요소

이 방법을 사용 하려면 먼저 다음을 수행 해야 합니다.

* 아직 수행 하지 않은 경우 Microsoft Store 제출 API에 대 한 모든 [필수 구성 요소](create-and-manage-submissions-using-windows-store-services.md#prerequisites) 를 완료 합니다.
* 이 메서드에 대 한 요청 헤더에 사용할 [AZURE AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) . 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후 새 토큰을 얻을 수 있습니다.
* 앱에 대 한 패키지 비행을 만듭니다. 파트너 센터에서이 작업을 수행 하거나 [패키지 만들기 비행](create-a-flight.md) 메서드를 사용 하 여이 작업을 수행할 수 있습니다.

## <a name="request"></a>요청

이 메서드의 구문은 다음과 같습니다. 헤더 및 요청 본문의 사용 예제와 설명은 다음 섹션을 참조하세요.

| 방법 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions` |


### <a name="request-header"></a>요청 헤더

| header        | 유형   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수 요소. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 이름        | 유형   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 문자열 | 필수 요소. 패키지 비행 제출을 만들려는 앱의 저장소 ID입니다. 저장소 ID에 대 한 자세한 내용은 [앱 id 세부 정보 보기](../publish/view-app-identity-details.md)를 참조 하세요.  |
| flightId | 문자열 | 필수 요소. 제출을 추가 하려는 패키지 비행의 ID입니다. 이 ID는 [패키지 비행 만들기](create-a-flight.md) 요청에 대 한 응답 데이터와 [앱에 대 한 패키지 항공편 가져오기](get-flights-for-an-app.md)요청에 사용할 수 있습니다.  |


### <a name="request-body"></a>요청 본문

이 메서드에 대 한 요청 본문을 제공 하지 마십시오.

### <a name="request-example"></a>요청 예제

다음 예제에서는 매장 ID 9WZDNCRD91MD를 포함 하는 앱에 대 한 새 패키지 비행 전송을 만드는 방법을 보여 줍니다.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

다음 예제에서는이 메서드를 성공적으로 호출 하기 위한 JSON 응답 본문을 보여 줍니다. 응답 본문에는 새 전송에 대 한 정보가 포함 됩니다. 응답 본문의 값에 대 한 자세한 내용은 [패키지 비행 전송 리소스](manage-flight-submissions.md#flight-submission-object)를 참조 하세요.

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

## <a name="error-codes"></a>오류 코드

요청이 성공적으로 완료 될 수 없는 경우 응답은 다음 HTTP 오류 코드 중 하나를 포함 합니다.

| 오류 코드 |  Description   |
|--------|------------------|
| 400  | 요청이 잘못 되어 패키지 비행 제출을 만들 수 없습니다. |
| 409  | 앱의 현재 상태 때문에 패키지 비행 제출을 만들지 못했습니다. 또는 앱이 [현재 Microsoft Store 제출 API에서 지원 하지](create-and-manage-submissions-using-windows-store-services.md#not_supported)않는 파트너 센터 기능을 사용 합니다. |   

## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용 하 여 제출 작성 및 관리](create-and-manage-submissions-using-windows-store-services.md)
* [패키지 플라이트 제출 관리](manage-flight-submissions.md)
* [패키지 비행 제출 가져오기](get-a-flight-submission.md)
* [패키지 플라이트 제출 커밋](commit-a-flight-submission.md)
* [패키지 플라인트 제출 업데이트](update-a-flight-submission.md)
* [패키지 플라이트 제출 삭제](delete-a-flight-submission.md)
* [패키지 비행 제출 상태 가져오기](get-status-for-a-flight-submission.md)