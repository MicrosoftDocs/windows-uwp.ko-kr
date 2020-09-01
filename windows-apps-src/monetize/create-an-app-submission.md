---
ms.assetid: D34447FF-21D2-44D0-92B0-B3FF9B32D6F7
description: Microsoft Store 제출 API에서이 방법을 사용 하 여 파트너 센터 계정에 등록 된 앱에 대 한 새 제출을 만듭니다.
title: 앱 제출 만들기
ms.date: 07/10/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 앱 제출 만들기
ms.localizationpriority: medium
ms.openlocfilehash: cbaf7e13c937762716bfd0cf35fbfdfb4776b1f1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158627"
---
# <a name="create-an-app-submission"></a>앱 제출 만들기

Microsoft Store 제출 API에서이 방법을 사용 하 여 파트너 센터 계정에 등록 된 앱에 대 한 새 제출을 만듭니다. 이 방법을 사용 하 여 새 제출을 성공적으로 만든 후에 제출 데이터를 변경 하 여 제출 데이터를 [변경 하 고](update-an-app-submission.md) 수집 및 게시를 위해 [제출을 커밋합니다](commit-an-app-submission.md) .

Microsoft Store 제출 API를 사용 하 여 앱 제출을 만드는 프로세스에이 방법을 사용 하는 방법에 대 한 자세한 내용은 [앱 제출 관리](manage-app-submissions.md)를 참조 하세요.

## <a name="prerequisites"></a>필수 구성 요소

이 방법을 사용 하려면 먼저 다음을 수행 해야 합니다.

* 아직 수행 하지 않은 경우 Microsoft Store 제출 API에 대 한 모든 [필수 구성 요소](create-and-manage-submissions-using-windows-store-services.md#prerequisites) 를 완료 합니다.
* 이 메서드에 대 한 요청 헤더에 사용할 [AZURE AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) . 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후 새 토큰을 얻을 수 있습니다.
* 앱에 [age 등급](../publish/age-ratings.md) 정보가 완료 된 제출이 이미 하나 이상 있는지 확인 합니다.

## <a name="request"></a>요청

이 메서드의 구문은 다음과 같습니다. 헤더 및 요청 본문의 사용 예제와 설명은 다음 섹션을 참조하세요.

| 방법 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions` |

### <a name="request-header"></a>요청 헤더

| header        | 유형   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수 요소. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |

### <a name="request-parameters"></a>요청 매개 변수

| 이름        | 유형   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 문자열 | 필수 요소. 제출을 만들려는 앱의 저장소 ID입니다. 저장소 ID에 대 한 자세한 내용은 [앱 id 세부 정보 보기](../publish/view-app-identity-details.md)를 참조 하세요.  |

### <a name="request-body"></a>요청 본문

이 메서드에 대 한 요청 본문을 제공 하지 마십시오.

### <a name="request-example"></a>요청 예제

다음 예제에서는 앱에 대 한 새 제출을 만드는 방법을 보여 줍니다.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

다음 예제에서는이 메서드를 성공적으로 호출 하기 위한 JSON 응답 본문을 보여 줍니다. 응답 본문에는 새 전송에 대 한 정보가 포함 됩니다. 응답 본문의 값에 대 한 자세한 내용은 [앱 제출 리소스](manage-app-submissions.md#app-submission-object)를 참조 하세요.

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

요청이 성공적으로 완료 될 수 없는 경우 응답은 다음 HTTP 오류 코드 중 하나를 포함 합니다.

| 오류 코드 |  Description   |
|--------|------------------|
| 400  | 요청이 잘못 되었으므로 제출을 만들 수 없습니다. |
| 409  | 앱의 현재 상태로 인해 제출을 만들지 못했습니다. 또는 앱이 [Microsoft Store 제출 API에서 현재 지원 하지](create-and-manage-submissions-using-windows-store-services.md#not_supported)않는 파트너 센터 기능을 사용 합니다. |   

## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용 하 여 제출 작성 및 관리](create-and-manage-submissions-using-windows-store-services.md)
* [앱 제출 가져오기](get-an-app-submission.md)
* [앱 제출 커밋](commit-an-app-submission.md)
* [앱 제출 업데이트](update-an-app-submission.md)
* [앱 제출 삭제](delete-an-app-submission.md)
* [앱 제출의 상태를 가져오기](get-status-for-an-app-submission.md)