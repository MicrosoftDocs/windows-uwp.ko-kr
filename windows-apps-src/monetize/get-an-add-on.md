---
ms.assetid: 78278741-09A4-4406-A112-9AF3C73F5C16
description: 파트너 센터 계정에 등록 된 앱에 대 한 추가 기능에 대 한 정보를 검색할 Microsoft Store 제출 API에서에서이 메서드를 사용 합니다.
title: 추가 기능 가져오기
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 추가 기능, 앱에서 바로 구매 제품, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 01438f4f684720a2f8285471c626d7040724c79f
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334421"
---
# <a name="get-an-add-on"></a>추가 기능 가져오기

파트너 센터 계정에 등록 된 앱에 대 한 추가 기능 (라고도 앱에서 제품 또는 IAP)에 대 한 정보를 검색할 Microsoft Store 제출 API에서에서이 메서드를 사용 합니다.

## <a name="prerequisites"></a>사전 요구 사항

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 제출 API에 대한 모든 [필수 조건](create-and-manage-submissions-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청

이 메서드에는 다음 구문이 있습니다. 헤더 및 요청 본문의 사용 예제와 설명은 다음 섹션을 참조하세요.

| 메서드 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| 가져오기    | `https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 형식   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | 필수. 폼에서 Azure AD 액세스 토큰 **전달자** &lt; *토큰*&gt;합니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 이름        | 형식   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| id | string | 필수. 검색할 추가 기능의 스토어 ID입니다. 파트너 센터에서 Store ID 제공 됩니다.  |


### <a name="request-body"></a>요청 본문

이 메서드에 대한 요청 본문을 제공하지 않습니다.


### <a name="request-example"></a>요청 예제

다음 예제에서는 추가 기능에 대한 정보를 검색하는 방법을 보여 줍니다.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

다음 예제에서는 이 메서드를 성공적으로 호출하기 위한 JSON 응답 본문을 보여 줍니다. 응답 본문의 값에 대한 자세한 내용은 [추가 기능 리소스](manage-add-ons.md#add-on-object)를 참조하세요.

```json
{
  "applications": {
    "value": [
      {
        "id": "9NBLGGH4R315",
        "resourceLocation": "applications/9NBLGGH4R315"
      }
    ],
    "totalCount": 1
  },
  "id": "9NBLGGH4TNMP",
  "productId": "Test-add-on",
  "productType": "Durable",
  "pendingInAppProductSubmission": {
    "id": "1152921504621243619",
    "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243619"
  },
  "lastPublishedInAppProductSubmission": {
    "id": "1152921504621243705",
    "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243705"
  }
}
```

## <a name="error-codes"></a>오류 코드

요청을 성공적으로 완료할 수 없으면 응답에 다음 HTTP 오류 코드 중 하나가 포함됩니다.

| 오류 코드 |  설명   |
|--------|------------------|
| 404  | 지정된 추가 기능을 찾을 수 없습니다. |
| 409  | 파트너 센터 기능을 사용 하는 추가 기능 [현재 Microsoft Store 전송 API에 의해 지원 되지 않습니다](create-and-manage-submissions-using-windows-store-services.md#not_supported)합니다.  |


## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용 하 여 서브 미션을 만들고 설정 합니다.](create-and-manage-submissions-using-windows-store-services.md)
* [추가 기능 제안 관리](manage-add-on-submissions.md)
* [모든 추가 기능 얻기](get-all-add-ons.md)
* [추가 기능 만들기](create-an-add-on.md)
* [추가 기능 삭제](delete-an-add-on.md)
