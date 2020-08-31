---
ms.assetid: 24C5F796-5FB8-4B5D-B428-C3154B3098BD
description: Microsoft Store 제출 API에서이 메서드를 사용 하 여 기존 패키지 비행 제출을 업데이트 합니다.
title: 패키지 플라인트 제출 업데이트
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 비행 전송, 업데이트
ms.localizationpriority: medium
ms.openlocfilehash: d2603319e2a1fc242210f79ef38c2ba5362c9c3e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171337"
---
# <a name="update-a-package-flight-submission"></a>패키지 플라인트 제출 업데이트


Microsoft Store 제출 API에서이 메서드를 사용 하 여 기존 패키지 비행 제출을 업데이트 합니다. 이 방법을 사용 하 여 제출을 성공적으로 업데이트 한 후에는 수집 및 게시를 위해 [제출을 커밋해야](commit-a-flight-submission.md) 합니다.

Microsoft Store 제출 API를 사용 하 여 패키지 비행 제출을 만드는 프로세스에이 방법을 사용 하는 방법에 대 한 자세한 내용은 [패키지 비행 전송 관리](manage-flight-submissions.md)를 참조 하세요.

## <a name="prerequisites"></a>필수 구성 요소

이 방법을 사용 하려면 먼저 다음을 수행 해야 합니다.

* 아직 수행 하지 않은 경우 Microsoft Store 제출 API에 대 한 모든 [필수 구성 요소](create-and-manage-submissions-using-windows-store-services.md#prerequisites) 를 완료 합니다.
* 이 메서드에 대 한 요청 헤더에 사용할 [AZURE AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) . 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후 새 토큰을 얻을 수 있습니다.
* 앱 중 하나에 대 한 패키지 비행 제출을 만듭니다. 파트너 센터에서이 작업을 수행 하거나, [패키지 비행 전송 방법 만들기](create-a-flight-submission.md) 방법을 사용 하 여이 작업을 수행할 수 있습니다.

## <a name="request"></a>요청

이 메서드의 구문은 다음과 같습니다. 헤더 및 요청 본문의 사용 예제와 설명은 다음 섹션을 참조하세요.

| 방법 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}``` |


### <a name="request-header"></a>요청 헤더

| header        | 유형   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수 요소. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 이름        | 유형   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 문자열 | 필수 요소. 패키지 비행 제출을 업데이트 하려는 앱의 저장소 ID입니다. 저장소 ID에 대 한 자세한 내용은 [앱 id 세부 정보 보기](../publish/view-app-identity-details.md)를 참조 하세요.  |
| flightId | 문자열 | 필수 요소. 전송을 업데이트 하려는 패키지 비행의 ID입니다. 이 ID는 [패키지 비행 만들기](create-a-flight.md) 요청에 대 한 응답 데이터와 [앱에 대 한 패키지 항공편 가져오기](get-flights-for-an-app.md)요청에 사용할 수 있습니다. 파트너 센터에서 만든 비행의 경우이 ID는 파트너 센터의 비행 페이지에 대 한 URL 에서도 사용할 수 있습니다.  |
| submissionId | 문자열 | 필수 요소. 업데이트할 전송의 ID입니다. 이 ID는 [패키지 비행 전송 만들기](create-a-flight-submission.md)요청에 대 한 응답 데이터에서 사용할 수 있습니다. 파트너 센터에서 만든 제출의 경우이 ID는 파트너 센터의 제출 페이지에 대 한 URL 에서도 사용할 수 있습니다.  |


### <a name="request-body"></a>요청 본문

요청 본문에는 다음 매개 변수가 있습니다.

| 값      | 형식   | Description                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightPackages           | array  | 제출의 각 패키지에 대 한 세부 정보를 제공 하는 개체를 포함 합니다. 응답 본문의 값에 대 한 자세한 내용은 [비행 패키지 리소스](manage-flight-submissions.md#flight-package-object)를 참조 하세요. 응용 프로그램 제출을 업데이트 하기 위해이 메서드를 호출 하는 경우 요청 본문에 이러한 개체의 *fileName*, *fileStatus,,*,가 중 및 *ram* 값만 필요 합니다. *minimumDirectXVersion* 다른 값은 파트너 센터에서 채워집니다. |
| packageDeliveryOptions    | 개체  | 전송에 대 한 점진적 패키지 롤아웃 및 필수 업데이트 설정이 포함 되어 있습니다. 자세한 내용은 [패키지 배달 옵션 개체](manage-flight-submissions.md#package-delivery-options-object)를 참조 하세요.  |
| targetPublishMode           | 문자열  | 제출에 대 한 게시 모드입니다. 다음 값 중 하나일 수 있습니다. <ul><li>즉시</li><li>설명서</li><li>SpecificDate</li></ul> |
| Target버전           | 문자열  | *TargetSpecificDate mode* 가로 설정 된 경우 ISO 8601 형식의 제출에 대 한 게시 날짜입니다.  |
| 메모 For인증           | 문자열  |  테스트 계정 자격 증명과 기능에 액세스 하 고 확인 하는 단계와 같은 인증 테스터에 대 한 추가 정보를 제공 합니다. 자세한 내용은 [인증에 대 한 참고 사항](../publish/notes-for-certification.md)을 참조 하세요. |


### <a name="request-example"></a>요청 예제

다음 예제에서는 앱에 대 한 패키지 비행 제출을 업데이트 하는 방법을 보여 줍니다.

```json
PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649 HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
{
  "flightPackages": [
    {
      "fileName": "newPackage.appx",
      "fileStatus": "PendingUpload",
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
  "targetPublishMode": "Immediate",
  "targetPublishDate": "",
  "notesForCertification": "No special steps are required for certification of this app."
}
```

## <a name="response"></a>응답

다음 예제에서는이 메서드를 성공적으로 호출 하기 위한 JSON 응답 본문을 보여 줍니다. 응답 본문에는 업데이트 된 제출에 대 한 정보가 포함 됩니다. 응답 본문의 값에 대 한 자세한 내용은 [패키지 비행 전송 리소스](manage-flight-submissions.md#flight-submission-object)를 참조 하세요.

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
| 400  | 요청이 잘못 되어 패키지 비행 전송의 업데이트를 업데이트할 수 없습니다. |
| 409  | 앱의 현재 상태 때문에 패키지 비행 제출을 업데이트 하지 못했습니다. 또는 앱이 [현재 Microsoft Store 제출 API에서 지원 하지](create-and-manage-submissions-using-windows-store-services.md#not_supported)않는 파트너 센터 기능을 사용 합니다. |   


## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용 하 여 제출 작성 및 관리](create-and-manage-submissions-using-windows-store-services.md)
* [패키지 플라이트 제출 관리](manage-flight-submissions.md)
* [패키지 비행 제출 가져오기](get-a-flight-submission.md)
* [패키지 플라이트 제출 만들기](create-a-flight-submission.md)
* [패키지 플라이트 제출 커밋](commit-a-flight-submission.md)
* [패키지 플라이트 제출 삭제](delete-a-flight-submission.md)
* [패키지 비행 제출 상태 가져오기](get-status-for-a-flight-submission.md)