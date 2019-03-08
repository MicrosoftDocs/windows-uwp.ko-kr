---
ms.assetid: E8751EBF-AE0F-4107-80A1-23C186453B1C
description: Microsoft Store 제출 API에서 이 메서드를 사용하여 기존 앱 제출을 업데이트합니다.
title: 앱 제출 업데이트
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 앱 제출, 업데이트
ms.localizationpriority: medium
ms.openlocfilehash: b61508edf2ebc2ab155110189fe67df63e2bab30
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57634618"
---
# <a name="update-an-app-submission"></a>앱 제출 업데이트

Microsoft Store 제출 API에서 이 메서드를 사용하여 기존 앱 제출을 업데이트합니다. 이 메서드를 사용하여 제출을 성공적으로 업데이트한 후 수집 및 게시를 위해 [제출을 커밋](commit-an-app-submission.md)합니다.

이 메서드가 Microsoft Store 제출 API를 사용하여 앱 제출을 만드는 프로세스에 적용되는 방법은 [앱 제출 관리](manage-app-submissions.md)를 참조하세요.

## <a name="prerequisites"></a>필수 구성 요소

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 제출 API에 대한 모든 [필수 조건](create-and-manage-submissions-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.
* 앱 중 하나에 대해 제출을 생성 합니다. 파트너 센터에서이 수행할 수 있는 또는 사용 하 여이 수행할 수는 [는 응용 프로그램 제출 만들기](create-an-app-submission.md) 메서드.

## <a name="request"></a>요청

이 메서드에는 다음 구문이 있습니다. 헤더 및 요청 본문의 사용 예제와 설명은 다음 섹션을 참조하세요.

| 메서드 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| PUT   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}  ``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 형식   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. 폼에서 Azure AD 액세스 토큰 **전달자** &lt; *토큰*&gt;합니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 이름        | 형식   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 문자열 | 필수. 제출을 업데이트하려는 앱의 스토어 ID입니다. 스토어 ID에 대한 자세한 내용은 [앱 ID 세부 정보 보기](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)를 참조하세요.  |
| submissionId | 문자열 | 필수. 업데이트할 제출의 ID입니다. 이 ID는 [앱 제출 만들기](create-an-app-submission.md) 요청에 대한 응답 데이터에서 사용할 수 있습니다. 파트너 센터에서 생성 된 제출에 대 한이 ID의 파트너 센터에서 제출 페이지의 URL을 사용할 수도 있습니다.  |


### <a name="request-body"></a>요청 본문

요청 본문에는 다음 매개 변수가 있습니다.

| 값      | 형식   | 설명                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| applicationCategory           | 문자열  |   앱에 대한 [범주 및/또는 하위 범주](https://msdn.microsoft.com/windows/uwp/publish/category-and-subcategory-table)를 지정하는 문자열입니다. 범주와 하위 범주는 **BooksAndReference_EReader**처럼 밑줄(_) 문자를 사용하여 단일 문자열로 연결합니다.      |  
| pricing           |  object  | 앱에 대한 가격 정보를 포함하는 개체입니다. 자세한 내용은 [가격 리소스](manage-app-submissions.md#pricing-object) 섹션을 참조하세요.       |   
| visibility           |  문자열  |  앱의 표시 여부입니다. 다음 값 중 하나일 수 있습니다. <ul><li>Hidden</li><li>Public</li><li>개인 정보 보호</li><li>NotSet</li></ul>       |   
| targetPublishMode           | 문자열  | 제출의 게시 모드입니다. 다음 값 중 하나일 수 있습니다. <ul><li>즉시</li><li>수동</li><li>SpecificDate</li></ul> |
| targetPublishDate           | 문자열  | *targetPublishMode*가 SpecificDate로 설정된 경우 제출의 게시 날짜(ISO 8601 형식)입니다.  |  
| listings           |   object  |  키와 값 쌍의 사전입니다. 여기서 각 키는 국가 코드이며 각 값은 앱에 대한 목록 정보를 포함하는 [목록 리소스](manage-app-submissions.md#listing-object) 개체입니다.       |   
| hardwarePreferences           |  배열  |   앱에 대한 [하드웨어 기본 설정](https://msdn.microsoft.com/windows/uwp/publish/enter-app-properties#hardware_preferences)을 정의하는 문자열의 배열입니다. 다음 값 중 하나일 수 있습니다. <ul><li>터치</li><li>키보드</li><li>마우스</li><li>카메라</li><li>NfcHce</li><li>Nfc</li><li>BluetoothLE</li><li>Telephony</li></ul>     |   
| automaticBackupEnabled           |  boolean  |   Windows에서 이 앱의 데이터를 OneDrive에 대한 자동 백업에 포함할 수 있는지 여부를 나타냅니다. 자세한 내용은 [앱 선언](https://msdn.microsoft.com/windows/uwp/publish/app-declarations)을 참조하세요.   |   
| canInstallOnRemovableMedia           |  boolean  |   고객이 이동식 저장소에 앱을 설치할 수 있는지 여부를 나타냅니다. 자세한 내용은 [앱 선언](https://msdn.microsoft.com/windows/uwp/publish/app-declarations)을 참조하세요.     |   
| isGameDvrEnabled           |  boolean |   앱에 대한 게임 DVR을 사용할지 여부를 나타냅니다.    |   
| gamingOptions           |  object |   앱에 대한 게임 관련 설정을 정의하는 [게임 옵션 리소스](manage-app-submissions.md#gaming-options-object)가 포함된 배열입니다     |   
| hasExternalInAppProducts           |     boolean          |   Microsoft Store 상거래 시스템 밖에서 사용자가 이 앱을 구매할 수 있는지 여부를 나타냅니다. 자세한 내용은 [앱 선언](https://msdn.microsoft.com/windows/uwp/publish/app-declarations)을 참조하세요.     |   
| meetAccessibilityGuidelines           |    boolean           |  이 앱이 접근성 지침을 준수하도록 테스트되었는지 여부를 나타냅니다. 자세한 내용은 [앱 선언](https://msdn.microsoft.com/windows/uwp/publish/app-declarations)을 참조하세요.      |   
| notesForCertification           |  문자열  |   앱의 [인증에 대한 참고 사항](https://msdn.microsoft.com/windows/uwp/publish/notes-for-certification)이 포함됩니다.    |    
| applicationPackages           |   배열  | 제출의 각 패키지에 대한 세부 정보를 제공하는 개체가 포함됩니다. 자세한 내용은 [응용 프로그램 패키지](manage-app-submissions.md#application-package-object) 섹션을 참조하세요. 이 메서드를 호출하여 앱 제출을 업데이트할 때는 요청 본문에 이러한 개체의 *fileName*, *fileStatus*, *minimumDirectXVersion* 및 *minimumSystemRam* 값만 필요합니다. 파트너 센터에서 다른 값은 채워집니다.   |    
| packageDeliveryOptions    | object  | 제출에 대한 점진적 패키지 출시 및 필수 업데이트 설정을 포함합니다. 자세한 내용은 [패키지 배달 옵션 개체](manage-app-submissions.md#package-delivery-options-object)를 참조하세요.  |
| enterpriseLicensing           |  문자열  |  앱의 엔터프라이즈 라이선스 동작을 나타내는 [엔터프라이즈 라이선스 값](manage-app-submissions.md#enterprise-licensing) 값 중 하나입니다.  |    
| allowMicrosftDecideAppAvailabilityToFutureDeviceFamilies           |  boolean   |  Microsoft에서 [이후의 Windows 10 디바이스 패밀리에 앱을 제공하도록](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families) 허용할지 여부를 나타냅니다.    |    
| allowTargetFutureDeviceFamilies           | boolean   |  앱에서 [이후의 Windows 10 디바이스 패밀리를 대상으로](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families) 할 수 있는지 여부를 나타냅니다.     |   
| trailers           |  배열 |   앱 목록에 대한 비디오 예고편을 나타내는 [예고편 리소스](manage-app-submissions.md#trailer-object) 가 포함된 배열입니다.   |   


### <a name="request-example"></a>요청 예제

다음 예제에서는 앱 제출을 업데이트하는 방법을 보여 줍니다.

```json
PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621230023 HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
{
  "applicationCategory": "BooksAndReference_EReader",
  "pricing": {
    "trialPeriod": "FifteenDays",
    "marketSpecificPricings": {},
    "sales": [],
    "priceId": "Tier2"
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
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
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
  "enterpriseLicensing": "Online",
  "allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies": true,
  "allowTargetFutureDeviceFamilies": {
    "Desktop": false,
    "Mobile": true,
    "Holographic": true,
    "Xbox": false,
    "Team": true
  },
  "trailers": []
}
```

## <a name="response"></a>응답

다음 예제에서는 이 메서드를 성공적으로 호출하기 위한 JSON 응답 본문을 보여 줍니다. 응답 본문에 업데이트한 제출에 대한 정보가 포함되어 있습니다. 응답 본문의 값에 대한 자세한 내용은 [앱 제출 리소스](manage-app-submissions.md#app-submission-object)를 참조하세요.

```json
{
  "id": "1152921504621243540",
  "applicationCategory": "BooksAndReference_EReader",
  "pricing": {
    "trialPeriod": "FifteenDays",
    "marketSpecificPricings": {},
    "sales": [],
    "priceId": "Tier2"
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
          "description": "Ebook reader for Windows 8.1",
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
      "fileStatus": "PendingUpload",
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
| 400  | 요청이 유효하지 않아서 제출을 업데이트할 수 없습니다. |
| 409  | 파트너 센터 기능을 사용 하는 앱 또는 앱의 현재 상태로 인해 제출을 업데이트 하지 못했습니다 [현재 Microsoft Store 전송 API에 의해 지원 되지 않습니다](create-and-manage-submissions-using-windows-store-services.md#not_supported)합니다. |   


## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용 하 여 서브 미션을 만들고 설정 합니다.](create-and-manage-submissions-using-windows-store-services.md)
* [가져오기는 앱 제출](get-an-app-submission.md)
* [만들기는 앱 제출](create-an-app-submission.md)
* [커밋에 앱 제출](commit-an-app-submission.md)
* [삭제는 앱 제출](delete-an-app-submission.md)
* [응용 프로그램 제출의 상태를 가져옵니다.](get-status-for-an-app-submission.md)
