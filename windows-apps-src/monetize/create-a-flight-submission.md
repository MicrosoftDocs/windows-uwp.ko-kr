---
author: Xansky
ms.assetid: CD866083-EB7F-4389-A907-FC43DC2FCB5E
description: Microsoft Store 제출 API에서 이 메서드를 사용하여 Windows 개발자 센터 계정에 등록된 앱에 대한 새 패키지 플라이트 제출을 만듭니다.
title: 패키지 플라이트 제출 만들기
ms.author: mhopkins
ms.date: 08/03/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store 제출 API, 플라이트 제출 만들기
ms.localizationpriority: medium
ms.openlocfilehash: 87e544c2d9d04e62f324fe14cd7aab3e1b7ee500
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/22/2018
ms.locfileid: "5402643"
---
# <a name="create-a-package-flight-submission"></a>패키지 플라이트 제출 만들기

Microsoft Store 제출 API에서 이 메서드를 사용하여 앱에 대한 패키지 플라이트의 새 제출을 만듭니다. 이 메서드를 사용하여 새 제출을 성공적으로 만든 후 [제출을 업데이트](update-a-flight-submission.md)하여 제출 데이터에 필요한 변경을 수행한 다음 수집 및 게시를 위해 [제출을 커밋](commit-a-flight-submission.md)합니다.

이 메서드가 Microsoft Store 제출 API를 사용하여 패키지 플라이트 제출을 만드는 프로세스에 적용되는 방법은 [패키지 플라이트 제출 관리](manage-flight-submissions.md)를 참조하세요.

> [!NOTE]
> 이 메서드는 기존 패키지 플라이트에 대한 제출을 만듭니다. 패키지 플라이트를 만들려면 [패키지 플라이트 만들기](create-a-flight.md) 메서드를 사용합니다.

## <a name="prerequisites"></a>필수 조건

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 제출 API에 대한 모든 [필수 조건](create-and-manage-submissions-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.
* 개발자 센터 계정에서 앱에 대한 패키지 플라이트를 만듭니다. 이 작업은 개발자 센터 대시보드에서 수행하거나 [패키지 플라이트 만들기](create-a-flight.md) 메서드를 사용하여 수행할 수 있습니다.

## <a name="request"></a>요청

이 메서드에는 다음 구문이 있습니다. 헤더 및 요청 본문의 사용 예제와 설명은 다음 섹션을 참조하세요.

| 메서드 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 이름        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | 필수. 패키지 플라이트 제출을 만들려는 앱의 스토어 ID입니다. 스토어 ID에 대한 자세한 내용은 [앱 ID 세부 정보 보기](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)를 참조하세요.  |
| flightId | 문자열 | 필수. 제출을 추가하려는 패키지 플라이트의 ID입니다. 이 ID는 개발자 센터 대시보드에서 사용할 수 있으며 [패키지 플라이트 만들기](create-a-flight.md) 및 [앱의 패키지 플라이트 가져오기](get-flights-for-an-app.md) 요청에 대한 응답 데이터에 포함되어 있습니다.  |


### <a name="request-body"></a>요청 본문

이 메서드에 대한 요청 본문을 제공하지 않습니다.

### <a name="request-example"></a>요청 예제

다음 예제에서는 스토어 ID가 9WZDNCRD91MD인 앱의 새 패키지 플라이트 제출을 만드는 방법을 보여 줍니다.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

다음 예제에서는 이 메서드를 성공적으로 호출하기 위한 JSON 응답 본문을 보여 줍니다. 응답 본문에 새 제출에 대한 정보가 포함되어 있습니다. 응답 본문의 값에 대한 자세한 내용은 [패키지 플라이트 제출 리소스](manage-flight-submissions.md#flight-submission-object)를 참조하세요.

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

요청을 성공적으로 완료할 수 없으면 응답에 다음 HTTP 오류 코드 중 하나가 포함됩니다.

| 오류 코드 |  설명   |
|--------|------------------|
| 400  | 요청이 잘못되어 패키지 플라이트 제출을 만들 수 없습니다. |
| 409  | 앱의 현재 상태 때문에 패키지 플라이트 제출을 만들 수 없거나 앱이 [현재 Microsoft Store 제출 API에서 지원되지 않는](create-and-manage-submissions-using-windows-store-services.md#not_supported) 개발자 센터 대시보드 기능을 사용합니다. |   


## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)
* [패키지 플라이트 제출 관리](manage-flight-submissions.md)
* [패키지 플라이트 제출 가져오기](get-a-flight-submission.md)
* [패키지 플라이트 제출 커밋](commit-a-flight-submission.md)
* [패키지 플라인트 제출 업데이트](update-a-flight-submission.md)
* [패키지 플라이트 제출 삭제](delete-a-flight-submission.md)
* [패키지 플라이트 제출 상태 가져오기](get-status-for-a-flight-submission.md)
