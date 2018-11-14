---
author: Xansky
ms.assetid: E59FB6FE-5318-46DF-B050-73F599C3972A
description: Microsoft Store 제출 API에서에서이 메서드를 사용 하 여 파트너 센터에 등록 된 앱에 대 한 앱에서 바로 구매에 대 한 정보를 검색 합니다.
title: 앱에 대한 추가 기능 가져오기
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 추가 기능, 앱에서 바로 구매 제품, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 3689a4fe16d016bb23bb7141630fd1f6a7b83142
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/08/2018
ms.locfileid: "6164819"
---
# <a name="get-add-ons-for-an-app"></a>앱에 대한 추가 기능 가져오기

Microsoft Store 제출 API에서에서이 메서드를 사용 하 여 파트너 센터 계정에 등록 된 앱에 대 한 추가 기능 목록을 만듭니다.

## <a name="prerequisites"></a>사전 요구 사항

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 제출 API에 대한 모든 [필수 조건](create-and-manage-submissions-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청

이 메서드에는 다음 구문이 있습니다. 헤더 및 요청 본문의 사용 예제와 설명은 다음 섹션을 참조하세요.

| 메서드 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listinappproducts``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수


|  이름  |  유형  |  설명  |  필수  |
|------|------|------|------|
|  applicationId  |  문자열  |  추가 기능을 검색하려는 앱의 스토어 ID입니다. 스토어 ID에 대한 자세한 내용은 [앱 ID 세부 정보 보기](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)를 참조하세요.  |  예  |
|  top  |  int  |  요청에 반환할 항목 수(즉, 반환할 추가 기능 수)입니다. 앱에 쿼리에서 지정한 값보다 더 많은 추가 기능이 있을 경우 응답 본문에는 데이터의 다음 페이지를 요청하기 위해 메서드 URI에 추가할 수 있는 상대 URI 경로가 포함됩니다.  |  아니요  |
|  skip |  int  | 나머지 항목을 반환하기 전에 쿼리에서 바이패스할 항목 수입니다. 이 매개 변수를 사용하여 데이터 집합의 페이지를 탐색합니다. 예를 들어 top=10이고 skip=0이면 1-10 항목을 검색하고 top=10이고 skip=10이면 11-20 항목을 검색합니다.   |  아니요  |


### <a name="request-body"></a>요청 본문

이 메서드에 대한 요청 본문을 제공하지 않습니다.

### <a name="request-examples"></a>요청 예제

다음 예제에서는 앱의 모든 추가 기능을 나열하는 방법을 보여 줍니다.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listinappproducts HTTP/1.1
Authorization: Bearer <your access token>
```

다음 예제에서는 앱의 처음 10개의 추가 기능을 나열하는 방법을 보여 줍니다.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listinappproducts?top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

다음 예제에서는 총 53개의 추가 기능이 있는 앱의 처음 10개의 추가 기능을 성공적으로 요청하여 반환된 JSON 응답 본문을 보여 줍니다. 편의를 위해 이 예제에서는 요청으로 반환된 처음 세 개의 추가 기능에 대한 데이터만 보여 줍니다. 응답 본문의 값에 대한 자세한 내용은 다음 섹션을 참조하세요.

```json
{
  "@nextLink": "applications/9NBLGGH4R315/listinappproducts/?skip=10&top=10",
  "value": [
    {
      "inAppProductId": "9NBLGGH4TNMP"
    },
    {
      "inAppProductId": "9NBLGGH4TNMN"
    },
    {
      "inAppProductId": "9NBLGGH4TNNR"
    },
    // Next 7 add-ons  are omitted for brevity ...
  ],
  "totalCount": 53
}
```

### <a name="response-body"></a>응답 본문

| 값      | 유형   | 설명                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| @nextLink  | string | 데이터의 추가 페이지가 있는 경우 이 문자열에는 데이터의 다음 페이지를 요청하기 위해 기본 ```https://manage.devcenter.microsoft.com/v1.0/my/``` 요청 URI를 추가할 수 있는 상대 경로가 포함됩니다. 예를 들어 초기 요청 본문의 *top* 매개 변수는 10으로 설정되어 있지만 앱의 추가 기능이 50개인 경우 응답 본문에는 ```applications/{applicationid}/listinappproducts/?skip=10&top=10```의 @nextLink 값이 포함되며 이는 ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationid}/listinappproducts/?skip=10&top=10```을 호출하여 다음 10개의 추가 기능을 호출할 수 있음을 나타냅니다. |
| value      | 배열  | 지정한 앱에 대한 각 추가 기능의 스토어 ID를 나열하는 개체의 배열입니다. 각 개체의 데이터에 대한 자세한 내용은 [추가 기능 리소스](get-app-data.md#add-on-object)를 참조하세요.                                                                                                                           |
| totalCount | int    | 쿼리에 대한 데이터 결과의 총 행 수(즉, 지정한 앱에 대한 추가 기능의 총 수)입니다.    |


## <a name="error-codes"></a>오류 코드

요청을 성공적으로 완료할 수 없으면 응답에 다음 HTTP 오류 코드 중 하나가 포함됩니다.

| 오류 코드 |  설명   |
|--------|------------------|
| 404  | 추가 기능이 없습니다. |
| 409  | 추가 기능을 사용 하 여 [Microsoft Store 제출 API에서 지원 되지 않는 현재](create-and-manage-submissions-using-windows-store-services.md#not_supported)됩니다 파트너 센터 기능.  |


## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)
* [모든 앱 가져오기](get-all-apps.md)
* [앱 가져오기](get-an-app.md)
* [앱의 패키지 플라이트 가져오기](get-flights-for-an-app.md)
