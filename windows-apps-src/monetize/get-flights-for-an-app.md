---
ms.assetid: B0AD0B8E-867E-4403-9CF6-43C81F3C30CA
description: Microsoft Store 제출 API에서이 메서드를 사용 하 여 파트너 센터 계정에 등록 된 앱에 대 한 패키지 비행 정보를 검색 합니다.
title: 앱의 패키지 플라이트 가져오기
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 항공편, 패키지 항공편
ms.localizationpriority: medium
ms.openlocfilehash: a5cfe7aa579e5d050ddf808e35c06d52c84eeca5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171437"
---
# <a name="get-package-flights-for-an-app"></a>앱의 패키지 플라이트 가져오기

Microsoft Store 제출 API에서이 메서드를 사용 하 여 파트너 센터 계정에 등록 된 앱에 대 한 패키지를 항공편으로 나열할 수 있습니다. 패키지 항공편에 대 한 자세한 내용은 [항공편 패키지](../publish/package-flights.md)를 참조 하세요.

## <a name="prerequisites"></a>필수 구성 요소

이 방법을 사용 하려면 먼저 다음을 수행 해야 합니다.

* 아직 수행 하지 않은 경우 Microsoft Store 제출 API에 대 한 모든 [필수 구성 요소](create-and-manage-submissions-using-windows-store-services.md#prerequisites) 를 완료 합니다.
* 이 메서드에 대 한 요청 헤더에 사용할 [AZURE AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) . 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후 새 토큰을 얻을 수 있습니다.

## <a name="request"></a>요청

이 메서드의 구문은 다음과 같습니다. 헤더 및 요청 본문의 사용 예제와 설명은 다음 섹션을 참조하세요.

| 방법 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listflights` |


### <a name="request-header"></a>요청 헤더

| header        | 유형   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수 요소. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

|  이름  |  유형  |  Description  |  필수  |
|------|------|------|------|
|  applicationId  |  문자열  |  패키지를 항공편 할 앱의 저장소 ID입니다. 저장소 ID에 대 한 자세한 내용은 [앱 id 세부 정보 보기](../publish/view-app-identity-details.md)를 참조 하세요.  |  예  |
|  top  |  int  |  요청에서 반환할 항목 수입니다 (반환 될 패키지의 수). 계정에 쿼리에 지정 된 값 보다 많은 패키지 항공편이 있는 경우 응답 본문에는 다음 데이터 페이지를 요청 하기 위해 메서드 URI에 추가할 수 있는 상대 URI 경로가 포함 됩니다.  |  예  |
|  skip  |  int  |  나머지 항목을 반환 하기 전에 쿼리에서 무시할 항목 수입니다. 이 매개 변수를 사용 하 여 데이터 집합을 페이징 합니다. 예를 들어 top = 10과 skip = 0은 항목 1부터 10까지, top = 10, skip = 10은 항목 11에서 20까지 검색 하는 식입니다.  |  예  |


### <a name="request-body"></a>요청 본문

이 메서드에 대 한 요청 본문을 제공 하지 마십시오.

### <a name="request-examples"></a>요청 예제

다음 예제에서는 앱에 대 한 모든 패키지를 나열 하는 방법을 보여 줍니다.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listflights HTTP/1.1
Authorization: Bearer <your access token>
```

다음 예제에서는 앱에 대 한 첫 번째 패키지 비행을 나열 하는 방법을 보여 줍니다.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listflights?top=1 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

다음 예제에서는 세 개의 총 패키지가 있는 앱에 대해 첫 번째 패키지 비행에 대해 성공한 요청에서 반환 된 JSON 응답 본문을 보여 줍니다. 응답 본문의 값에 대 한 자세한 내용은 다음 섹션을 참조 하십시오.

```json
{
  "value": [
    {
      "flightId": "7bfc11d5-f710-47c5-8a98-e04bb5aad310",
      "friendlyName": "myflight",
      "lastPublishedFlightSubmission": {
        "id": "1152921504621086517",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621086517"
      },
      "pendingFlightSubmission": {
        "id": "1152921504621215786",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621215786"
      },
      "groupIds": [
        "1152921504606962205"
      ],
      "rankHigherThan": "Non-flighted submission"
    }
  ],
  "totalCount": 3
}
```

### <a name="response-body"></a>응답 본문

| 값      | 형식   | Description       |
|------------|--------|---------------------|
| @nextLink  | 문자열 | 추가 데이터 페이지가 있는 경우이 문자열에는 `https://manage.devcenter.microsoft.com/v1.0/my/` 다음 데이터 페이지를 요청 하기 위해 기본 요청 URI에 추가할 수 있는 상대 경로가 포함 됩니다. 예를 들어 초기 요청 본문의 *맨 위* 매개 변수가 2로 설정 되어 있지만 앱에 대해 네 개의 패키지가 항공편 인 경우 응답 본문에는 @nextLink 값이 포함 됩니다 .이 값은 `applications/{applicationid}/listflights/?skip=2&top=2` 를 호출 하 여 `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationid}/listflights/?skip=2&top=2` 다음 2 개의 패키지를 항공편으로 요청할 수 있음을 나타냅니다. |
| value      | array  | 지정 된 앱의 패키지 항공편에 대 한 정보를 제공 하는 개체의 배열입니다. 각 개체의 데이터에 대 한 자세한 내용은 [비행 리소스](get-app-data.md#flight-object)를 참조 하세요.               |
| totalCount | int    | 쿼리의 데이터 결과에 있는 총 행 수 (지정 된 앱에 대 한 총 패키지 수)입니다.   |


## <a name="error-codes"></a>오류 코드

요청이 성공적으로 완료 될 수 없는 경우 응답은 다음 HTTP 오류 코드 중 하나를 포함 합니다.

| 오류 코드 |  Description   |
|--------|------------------|
| 404  | 항공편 패키지를 찾을 수 없습니다. |
| 409  | 앱이 [Microsoft Store 제출 API에서 현재 지원 하지](create-and-manage-submissions-using-windows-store-services.md#not_supported)않는 파트너 센터 기능을 사용 합니다.  |


## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용 하 여 제출 작성 및 관리](create-and-manage-submissions-using-windows-store-services.md)
* [모든 앱 가져오기](get-all-apps.md)
* [앱 가져오기](get-an-app.md)
* [앱에 대한 추가 기능 가져오기](get-add-ons-for-an-app.md)