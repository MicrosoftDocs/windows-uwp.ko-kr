---
author: Xansky
ms.assetid: BF296C25-A2E6-48E4-9D08-0CCDB5FAE0C8
description: Microsoft Store 제출 API에서 이 메서드를 사용하여 기존 앱 제출에 대한 데이터를 가져옵니다.
title: 앱 제출 가져오기
ms.author: mhopkins
ms.date: 04/17/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store 제출 API, 앱 제출
ms.localizationpriority: medium
ms.openlocfilehash: 9838798778a2d66b2a52fcfe02defce0c9167d2d
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/19/2018
ms.locfileid: "5165162"
---
# <a name="get-an-app-submission"></a>앱 제출 가져오기


Microsoft Store 제출 API에서 이 메서드를 사용하여 기존 앱 제출에 대한 데이터를 가져옵니다. Microsoft Store 제출 API를 사용하여 앱 제출을 만드는 프로세스의 절차에 대한 자세한 내용은 [앱 제출 관리](manage-app-submissions.md)를 참조하세요.

## <a name="prerequisites"></a>필수 조건

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 제출 API에 대한 모든 [필수 조건](create-and-manage-submissions-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.
* 개발자 센터 계정의 앱에 대한 제출을 만듭니다. 이 작업은 개발자 센터 대시보드에서 수행하거나 [앱 제출 만들기](create-an-app-submission.md) 메서드를 사용하여 수행할 수 있습니다.

## <a name="request"></a>요청

이 메서드에는 다음 구문이 있습니다. 헤더 및 요청 본문의 사용 예제와 설명은 다음 섹션을 참조하세요.

| 메서드 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| GET   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId} ``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 이름        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | 필수. 받으려는 제출이 포함된 앱의 스토어 ID입니다. 스토어 ID에 대한 자세한 내용은 [앱 ID 세부 정보 보기](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)를 참조하세요.  |
| submissionId | string | 필수. 가져올 제출의 ID입니다. 이 ID는 [앱 제출 만들기](create-an-app-submission.md) 요청에 대한 응답 데이터에서 사용할 수 있습니다. 개발자 센터 대시보드에서 만든 제출의 경우 이 ID는 대시보드에 있는 제출 페이지의 URL에도 사용할 수 있습니다.  |


### <a name="request-body"></a>요청 본문

이 메서드에 대한 요청 본문을 제공하지 않습니다.

### <a name="request-example"></a>요청 예제

다음 예제에서는 앱 제출을 가져오는 방법을 보여 줍니다.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621243680 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

다음 예제에서는 이 메서드를 성공적으로 호출하기 위한 JSON 응답 본문을 보여 줍니다. 응답 본문에 지정된 제출에 대한 정보가 포함되어 있습니다. 응답 본문의 값에 대한 자세한 내용은 [앱 제출 리소스](manage-app-submissions.md#app-submission-object)를 참조하세요.

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

## <a name="error-codes"></a>오류 코드

요청을 성공적으로 완료할 수 없으면 응답에 다음 HTTP 오류 코드 중 하나가 포함됩니다.

| 오류 코드 |  설명   |
|--------|------------------|
| 404  | 제출을 찾을 수 없습니다. |
| 409  | 제출이 지정된 앱에 속하지 않거나 앱이 [현재 Microsoft Store 제출 API에서 지원되지 않는](create-and-manage-submissions-using-windows-store-services.md#not_supported) 개발자 센터 대시보드 기능을 사용합니다. |   


## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)
* [앱 제출 만들기](create-an-app-submission.md)
* [앱 제출 커밋](commit-an-app-submission.md)
* [앱 제출 업데이트](update-an-app-submission.md)
* [앱 제출 삭제](delete-an-app-submission.md)
* [앱 제출의 상태를 가져오기](get-status-for-an-app-submission.md)
