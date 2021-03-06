---
ms.assetid: E3DF5D11-8791-4CFC-8131-4F59B928A228
description: Microsoft Store 제출 API에서 이 메서드를 사용하여 기존 추가 기능 제출에 대한 데이터를 가져옵니다.
title: 추가 기능 제출 가져오기
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 추가 기능 제출, 앱에서 바로 구매 제품, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 8d997dd4d1369ef09f29a165f9e0554d6f3ad7c7
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334748"
---
# <a name="get-an-add-on-submission"></a>추가 기능 제출 가져오기

Microsoft Store 제출 API에서 이 메서드를 사용하여 기존 추가 기능(앱에서 바로 구매 제품 또는 IAP라고도 함) 제출에 대한 데이터를 가져옵니다. Microsoft Store 제출 API를 사용하여 추가 기능 제출을 만드는 프로세스의 절차에 대한 자세한 내용은 [추가 기능 제출 관리](manage-add-on-submissions.md)를 참조하세요.

## <a name="prerequisites"></a>사전 요구 사항

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 제출 API에 대한 모든 [필수 조건](create-and-manage-submissions-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.
* 앱 중 하나에 대 한 추가 기능 제출을 만듭니다. 파트너 센터에서이 수행할 수 있는 또는 사용 하 여이 수행할 수는 [는 추가 기능 제출 만들기](create-an-add-on-submission.md) 메서드.

## <a name="request"></a>요청

이 메서드에는 다음 구문이 있습니다. 헤더 및 요청 본문의 사용 예제와 설명은 다음 섹션을 참조하세요.

| 메서드 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| 가져오기   | `https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId} ` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 형식   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | 필수. 폼에서 Azure AD 액세스 토큰 **전달자** &lt; *토큰*&gt;합니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 이름        | 형식   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | string | 필수. 가져올 제출이 포함된 추가 기능의 스토어 ID입니다. 파트너 센터에서 Store ID 사용할 수 있으며 응답 데이터에 대 한 요청에 포함 됩니다 [추가 기능을 만듭니다](create-an-add-on.md) 또는 [추가 기능 세부 정보 가져오기](get-all-add-ons.md)합니다.  |
| submissionId | string | 필수 사항입니다. 가져올 제출의 ID입니다. 이 ID는 [추가 기능 제출 만들기](create-an-add-on-submission.md) 요청에 대한 응답 데이터에서 사용할 수 있습니다. 파트너 센터에서 생성 된 제출에 대 한이 ID의 파트너 센터에서 제출 페이지의 URL을 사용할 수도 있습니다.  |


### <a name="request-body"></a>요청 본문

이 메서드에 대한 요청 본문을 제공하지 않습니다.

### <a name="request-example"></a>요청 예제

다음 예제에서는 추가 기능 제출을 가져오는 방법을 보여 줍니다.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions/1152921504621243680 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

다음 예제에서는 이 메서드를 성공적으로 호출하기 위한 JSON 응답 본문을 보여 줍니다. 응답 본문에 지정된 제출에 대한 정보가 포함되어 있습니다. 응답 본문의 값에 대한 자세한 내용은 [추가 기능 제출 리소스](manage-add-on-submissions.md#add-on-submission-object)를 참조하세요.

```json
{
  "id": "1152921504621243680",
  "contentType": "EMagazine",
  "keywords": [
    "books"
  ],
  "lifetime": "FiveDays",
  "listings": {
    "en": {
      "description": "English add-on description",
      "icon": {
        "fileName": "add-on-en-us-listing2.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (English)"
    },
    "ru": {
      "description": "Russian add-on description",
      "icon": {
        "fileName": "add-on-ru-listing.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (Russian)"
    }
  },
  "pricing": {
    "marketSpecificPricings": {
      "RU": "Tier3",
      "US": "Tier4",
    },
    "sales": [],
    "priceId": "Free",
    "isAdvancedPricingModel": true
  },
  "targetPublishDate": "2016-03-15T05:10:58.047Z",
  "targetPublishMode": "Immediate",
  "tag": "SampleTag",
  "visibility": "Public",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [
      {
        "code": "None",
        "details": "string"
      }
    ],
    "warnings": [
      {
        "code": "ListingOptOutWarning",
        "details": "You have removed listing language(s): []"
      }
    ],
    "certificationReports": [
      {
      }
    ]
  },

    "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl",
    "friendlyName": "Submission 2"
}
```


## <a name="error-codes"></a>오류 코드

요청을 성공적으로 완료할 수 없으면 응답에 다음 HTTP 오류 코드 중 하나가 포함됩니다.

| 오류 코드 |  설명   |
|--------|------------------|
| 404  | 제출을 찾을 수 없습니다. |
| 409  | 파트너 센터 기능을 사용 하는 추가 기능 또는 제출, 지정 된 추가 기능에 속하지 않는 [현재 Microsoft Store 전송 API에 의해 지원 되지 않습니다](create-and-manage-submissions-using-windows-store-services.md#not_supported)합니다. |   


## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용 하 여 서브 미션을 만들고 설정 합니다.](create-and-manage-submissions-using-windows-store-services.md)
* [추가 기능 제출 만들기](create-an-add-on-submission.md)
* [추가 기능 제출 커밋](commit-an-add-on-submission.md)
* [업데이트는 추가 기능 제출](update-an-add-on-submission.md)
* [추가 기능 제출 삭제](delete-an-add-on-submission.md)
* [추가 기능 제출 상태를 가져옵니다.](get-status-for-an-add-on-submission.md)
