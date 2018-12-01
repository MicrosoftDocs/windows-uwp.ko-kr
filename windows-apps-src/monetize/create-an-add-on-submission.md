---
ms.assetid: C09F4B7C-6324-4973-980A-A60035792EFC
description: Microsoft Store 제출 API에서에서이 메서드를 사용 하 여 파트너 센터에 등록 된 앱에 대 한 새 추가 기능 제출을 만듭니다.
title: 추가 기능 제출 만들기
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 추가 기능 제출 만들기, 앱에서 바로 구매 제품, IAP
ms.localizationpriority: medium
ms.openlocfilehash: fcc98252efb1157bc539b68656c96f7afec7104a
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8349926"
---
# <a name="create-an-add-on-submission"></a>추가 기능 제출 만들기


Microsoft Store 제출 API에서에서이 메서드를 사용 하 여 파트너 센터 계정에 등록 된 앱에 대 한 새 추가 기능 (라고도 앱에서 바로 구매 또는 IAP) 제출을 만듭니다. 이 메서드를 사용하여 새 제출을 성공적으로 만든 후 [제출을 업데이트](update-an-add-on-submission.md)하여 제출 데이터에 필요한 변경을 수행한 다음 수집 및 게시를 위해 [제출을 커밋](commit-an-add-on-submission.md)합니다.

이 메서드가 Microsoft Store 제출 API를 사용하여 추가 기능 제출을 만드는 프로세스에 적용되는 방법은 [추가 기능 제출 관리](manage-add-on-submissions.md)를 참조하세요.

> [!NOTE]
> 이 메서드는 기존 추가 기능에 대한 제출을 만듭니다. 추가 기능을 만들려면 [추가 기능 만들기](create-an-add-on.md) 메서드를 사용합니다.

## <a name="prerequisites"></a>필수 조건

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 제출 API에 대한 모든 [필수 조건](create-and-manage-submissions-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.
* 앱 중 하나에 대 한 추가 기능을 만듭니다. 파트너 센터에서 이렇게 하려면 또는 [추가 기능 만들기](create-an-add-on.md) 메서드를 사용 하 여이 수행할 수 있습니다.

## <a name="request"></a>요청

이 메서드에는 다음 구문이 있습니다. 헤더 및 요청 본문의 사용 예제와 설명은 다음 섹션을 참조하세요.

| 메서드 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 이름        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | string | 필수. 제출을 만들려는 추가 기능의 스토어 ID입니다. 파트너 센터에서 스토어 ID는 사용할 수 있으며 [추가 기능 만들기](create-an-add-on.md) 또는 [추가 기능 세부 정보 가져오기](get-all-add-ons.md)에 대 한 요청에 대 한 응답 데이터에 포함 되어 있습니다.  |


### <a name="request-body"></a>요청 본문

이 메서드에 대한 요청 본문을 제공하지 않습니다.

### <a name="request-example"></a>요청 예제

다음 예제에서는 추가 기능에 대한 새 제출을 만드는 방법을 보여 줍니다.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

다음 예제에서는 이 메서드를 성공적으로 호출하기 위한 JSON 응답 본문을 보여 줍니다. 응답 본문에 새 제출에 대한 정보가 포함되어 있습니다. 응답 본문의 값에 대한 자세한 내용은 [추가 기능 제출 리소스](manage-add-on-submissions.md#add-on-submission-object)를 참조하세요.

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
    "sales": [
      {
         "name": "Sale1",
         "basePriceId": "Free",
         "startDate": "2016-05-21T18:40:11.7369008Z",
         "endDate": "2016-05-22T18:40:11.7369008Z",
         "marketSpecificPricings": {
            "RU": "NotAvailable"
         }
      }
    ],
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
| 400  | 요청이 잘못되어 제출을 만들 수 없습니다. |
| 409  | 제출을 만들 수 없습니다 앱의 현재 상태 때문 또는 앱은 [Microsoft Store 제출 API에서 지원 되지 않는 현재](create-and-manage-submissions-using-windows-store-services.md#not_supported)는 파트너 센터 기능을 사용 합니다. |   


## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)
* [추가 기능 제출 관리](manage-add-on-submissions.md)
* [추가 기능 제출 가져오기](get-an-add-on-submission.md)
* [추가 기능 제출 커밋](commit-an-add-on-submission.md)
* [추가 기능 제출 업데이트](update-an-add-on-submission.md)
* [추가 기능 제출 삭제](delete-an-add-on-submission.md)
* [추가 기능 제출 상태 가져오기](get-status-for-an-add-on-submission.md)
