---
ms.assetid: E8751EBF-AE0F-4107-80A1-23C186453B1C
description: Microsoft Store 제출 API에서이 방법을 사용 하 여 기존 앱 제출을 업데이트 합니다.
title: 앱 제출 업데이트
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 앱 제출, 업데이트
ms.localizationpriority: medium
ms.openlocfilehash: 393b7c48723409dfc630c6f85770c4c0f7dcd1a4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158367"
---
# <a name="update-an-app-submission"></a>앱 제출 업데이트

Microsoft Store 제출 API에서이 방법을 사용 하 여 기존 앱 제출을 업데이트 합니다. 이 방법을 사용 하 여 제출을 성공적으로 업데이트 한 후에는 수집 및 게시를 위해 [제출을 커밋해야](commit-an-app-submission.md) 합니다.

Microsoft Store 제출 API를 사용 하 여 앱 제출을 만드는 프로세스에이 방법을 사용 하는 방법에 대 한 자세한 내용은 [앱 제출 관리](manage-app-submissions.md)를 참조 하세요.

## <a name="prerequisites"></a>필수 구성 요소

이 방법을 사용 하려면 먼저 다음을 수행 해야 합니다.

* 아직 수행 하지 않은 경우 Microsoft Store 제출 API에 대 한 모든 [필수 구성 요소](create-and-manage-submissions-using-windows-store-services.md#prerequisites) 를 완료 합니다.
* 이 메서드에 대 한 요청 헤더에 사용할 [AZURE AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) . 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후 새 토큰을 얻을 수 있습니다.
* 앱 중 하나에 대 한 제출을 만듭니다. 파트너 센터에서이 작업을 수행할 수도 있고, [앱 전송 만들기](create-an-app-submission.md) 방법을 사용 하 여이 작업을 수행할 수도 있습니다.

## <a name="request"></a>요청

이 메서드의 구문은 다음과 같습니다. 헤더 및 요청 본문의 사용 예제와 설명은 다음 섹션을 참조하세요.

| 방법 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| PUT   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}  ``` |


### <a name="request-header"></a>요청 헤더

| header        | 유형   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수 요소. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 이름        | 유형   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 문자열 | 필수 요소. 제출을 업데이트 하려는 앱의 저장소 ID입니다. 저장소 ID에 대 한 자세한 내용은 [앱 id 세부 정보 보기](../publish/view-app-identity-details.md)를 참조 하세요.  |
| submissionId | 문자열 | 필수 요소. 업데이트할 전송의 ID입니다. 이 ID는 [앱 제출을 만들기](create-an-app-submission.md)위한 요청에 대 한 응답 데이터에서 사용할 수 있습니다. 파트너 센터에서 만든 제출의 경우이 ID는 파트너 센터의 제출 페이지에 대 한 URL 에서도 사용할 수 있습니다.  |


### <a name="request-body"></a>요청 본문

요청 본문에는 다음 매개 변수가 있습니다.

| 값      | 형식   | Description                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| applicationCategory           | 문자열  |   앱에 대 한 [범주 및/또는 하위](../publish/category-and-subcategory-table.md) 범주를 지정 하는 문자열입니다. 범주와 하위 범주는 **BooksAndReference_EReader**와 같이 밑줄 ' _ ' 문자를 사용 하 여 단일 문자열로 결합 됩니다.      |  
| 가격 책정           |  개체  | 앱에 대 한 가격 정보를 포함 하는 개체입니다. 자세한 내용은 [가격 책정 리소스](manage-app-submissions.md#pricing-object) 섹션을 참조 하세요.       |   
| 표시 여부           |  문자열  |  앱의 표시 여부입니다. 다음 값 중 하나일 수 있습니다. <ul><li>숨김</li><li>공용</li><li>프라이빗</li><li>NotSet</li></ul>       |   
| targetPublishMode           | 문자열  | 제출에 대 한 게시 모드입니다. 다음 값 중 하나일 수 있습니다. <ul><li>즉시</li><li>설명서</li><li>SpecificDate</li></ul> |
| Target버전           | 문자열  | *TargetSpecificDate mode* 가로 설정 된 경우 ISO 8601 형식의 제출에 대 한 게시 날짜입니다.  |  
| 목록           |   개체  |  키/값 쌍의 사전입니다. 여기서 각 키는 국가 코드이 고 각 값은 앱에 대 한 목록 정보를 포함 하는 [목록 리소스](manage-app-submissions.md#listing-object) 개체입니다.       |   
| hardwarePreferences           |  array  |   앱에 대 한 [하드웨어 기본 설정을](../publish/enter-app-properties.md) 정의 하는 문자열 배열입니다. 다음 값 중 하나일 수 있습니다. <ul><li>터치</li><li>키보드</li><li>마우스</li><li>카메라</li><li>NfcHce</li><li>Reader</li><li>BluetoothLE</li><li>Telephony</li></ul>     |   
| 자동 Backup사용           |  boolean  |   Windows에서 OneDrive에 자동 백업에 앱의 데이터를 포함할 수 있는지 여부를 나타냅니다. 자세한 내용은 [앱 선언](../publish/product-declarations.md)을 참조 하세요.   |   
| canInstallOnRemovableMedia           |  boolean  |   고객이 이동식 저장소에 앱을 설치할 수 있는지 여부를 나타냅니다. 자세한 내용은 [앱 선언](../publish/product-declarations.md)을 참조 하세요.     |   
| isGameDvrEnabled           |  boolean |   앱에 게임 DVR이 사용 되는지 여부를 나타냅니다.    |   
| gamingOptions           |  개체 |   앱에 대 한 게임 관련 설정을 정의 하는 [게임 옵션 리소스](manage-app-submissions.md#gaming-options-object) 하나를 포함 하는 배열입니다.     |   
| hasExternalInAppProducts           |     boolean          |   앱을 통해 사용자가 Microsoft Store 상거래 시스템 외부에서 구매할 수 있는지 여부를 나타냅니다. 자세한 내용은 [앱 선언](../publish/product-declarations.md)을 참조 하세요.     |   
| meetAccessibilityGuidelines           |    boolean           |  앱이 내게 필요한 옵션 지침을 충족 하는지 테스트 되었는지 여부를 나타냅니다. 자세한 내용은 [앱 선언](../publish/product-declarations.md)을 참조 하세요.      |   
| 메모 For인증           |  문자열  |   앱에 대 한 [인증에 대 한 정보를](../publish/notes-for-certification.md) 포함 합니다.    |    
| applicationPackages           |   array  | 제출의 각 패키지에 대 한 세부 정보를 제공 하는 개체를 포함 합니다. 자세한 내용은 [응용 프로그램 패키지](manage-app-submissions.md#application-package-object) 섹션을 참조 하세요. 응용 프로그램 제출을 업데이트 하기 위해이 메서드를 호출 하는 경우 요청 본문에 이러한 개체의 *fileName*, *fileStatus,,*,가 중 및 *ram* 값만 필요 합니다. *minimumDirectXVersion* 다른 값은 파트너 센터에서 채워집니다.   |    
| packageDeliveryOptions    | 개체  | 전송에 대 한 점진적 패키지 롤아웃 및 필수 업데이트 설정이 포함 되어 있습니다. 자세한 내용은 [패키지 배달 옵션 개체](manage-app-submissions.md#package-delivery-options-object)를 참조 하세요.  |
| enterpriseLicensing           |  문자열  |  앱에 대 한 엔터프라이즈 라이선스 동작을 나타내는 [엔터프라이즈 라이선스 값](manage-app-submissions.md#enterprise-licensing) 값 중 하나입니다.  |    
| allowMicrosftDecideAppAvailabilityToFutureDeviceFamilies           |  boolean   |  Microsoft에서 [이후 Windows 10 장치 제품군에 앱을 사용할 수 있도록](../publish/set-app-pricing-and-availability.md)허용할지 여부를 나타냅니다.    |    
| allowTargetFutureDeviceFamilies           | boolean   |  앱이 [이후 Windows 10 장치 패밀리를 대상](../publish/set-app-pricing-and-availability.md)으로 할 수 있는지 여부를 나타냅니다.     |   
| 부           |  array |   앱 목록에 대 한 비디오 트레일러를 나타내는 최대 [트레일러 리소스](manage-app-submissions.md#trailer-object) 를 포함 하는 배열입니다.   |   


### <a name="request-example"></a>요청 예제

다음 예제에서는 앱 제출을 업데이트 하는 방법을 보여 줍니다.

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

다음 예제에서는이 메서드를 성공적으로 호출 하기 위한 JSON 응답 본문을 보여 줍니다. 응답 본문에는 업데이트 된 제출에 대 한 정보가 포함 됩니다. 응답 본문의 값에 대 한 자세한 내용은 [앱 제출 리소스](manage-app-submissions.md#app-submission-object)를 참조 하세요.

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

요청이 성공적으로 완료 될 수 없는 경우 응답은 다음 HTTP 오류 코드 중 하나를 포함 합니다.

| 오류 코드 |  Description   |
|--------|------------------|
| 400  | 요청이 잘못 되었으므로 제출을 업데이트할 수 없습니다. |
| 409  | 앱의 현재 상태로 인해 제출을 업데이트 하지 못했습니다. 또는 앱이 [Microsoft Store 제출 API에서 현재 지원 하지](create-and-manage-submissions-using-windows-store-services.md#not_supported)않는 파트너 센터 기능을 사용 합니다. |   


## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용 하 여 제출 작성 및 관리](create-and-manage-submissions-using-windows-store-services.md)
* [앱 제출 가져오기](get-an-app-submission.md)
* [앱 제출 만들기](create-an-app-submission.md)
* [앱 제출 커밋](commit-an-app-submission.md)
* [앱 제출 삭제](delete-an-app-submission.md)
* [앱 제출의 상태를 가져오기](get-status-for-an-app-submission.md)