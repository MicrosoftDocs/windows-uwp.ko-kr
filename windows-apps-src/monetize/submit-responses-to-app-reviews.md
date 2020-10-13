---
ms.assetid: 038903d6-efab-4da6-96b5-046c7431e6e7
description: Microsoft Store 리뷰 API에서이 방법을 사용 하 여 앱의 검토에 대 한 응답을 제출할 수 있습니다.
title: 리뷰에 대한 응답 제출
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 스토어 서비스, Microsoft Store 리뷰 API, 추가 기능 인수
ms.localizationpriority: medium
ms.openlocfilehash: 9cf62f5f619eba0431f1398a391eef03b47bb13d
ms.sourcegitcommit: 140bbbab0f863a7a1febee85f736b0412bff1ae7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91984639"
---
# <a name="submit-responses-to-reviews"></a>리뷰에 대한 응답 제출


Microsoft Store 리뷰 API에서이 방법을 사용 하 여 프로그래밍 방식으로 앱 검토에 응답 합니다. 이 메서드를 호출 하는 경우 응답 하려는 검토의 Id를 지정 해야 합니다. 검토 Id는 Microsoft Store 분석 API 및 [검토 보고서](../publish/reviews-report.md)의 [오프 라인 다운로드](../publish/download-analytic-reports.md) 에서 [앱 검토 가져오기](get-app-reviews.md) 메서드의 응답 데이터에서 사용할 수 있습니다.

고객은 리뷰를 제출할 때 해당 검토에 대 한 응답을 수신 하지 않도록 선택할 수 있습니다. 고객이 응답을 수신 하지 않기로 선택한 리뷰에 응답 하려는 경우이 메서드의 응답 본문은 응답 시도가 실패 했음을 표시 합니다. 이 메서드를 호출 하기 전에 [앱 검토에 대 한 응답 정보 가져오기](get-response-info-for-app-reviews.md) 방법을 사용 하 여 지정 된 검토에 응답할 수 있는지 여부를 선택적으로 결정할 수 있습니다.

> [!NOTE]
> 이 메서드를 사용 하 여 프로그래밍 방식으로 검토에 응답 하는 것 외에도 [파트너 센터를 사용 하 여](../publish/respond-to-customer-reviews.md)리뷰에 응답할 수 있습니다.

## <a name="prerequisites"></a>필수 구성 요소

이 방법을 사용 하려면 먼저 다음을 수행 해야 합니다.

* 아직 수행 하지 않은 경우 Microsoft Store 리뷰 API에 대 한 모든 [필수 구성 요소](respond-to-reviews-using-windows-store-services.md#prerequisites) 를 완료 합니다.
* 이 메서드에 대 한 요청 헤더에 사용할 [AZURE AD 액세스 토큰을 가져옵니다](respond-to-reviews-using-windows-store-services.md#obtain-an-azure-ad-access-token) . 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후 새 토큰을 얻을 수 있습니다.
* 응답 하려는 검토의 Id를 가져옵니다. 검토 Id는 Microsoft Store 분석 API 및 [검토 보고서](../publish/reviews-report.md)의 [오프 라인 다운로드](../publish/download-analytic-reports.md) 에서 [앱 검토 가져오기](get-app-reviews.md) 메서드의 응답 데이터에서 사용할 수 있습니다.

## <a name="request"></a>요청

### <a name="request-syntax"></a>요청 구문

| 방법 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/reviews/responses``` |


### <a name="request-header"></a>요청 헤더

| header        | 유형   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수 요소. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

이 메서드에는 요청 매개 변수가 없습니다.


### <a name="request-body"></a>요청 본문

요청 본문의 값은 다음과 같습니다.

| 값        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------|
| 응답 | array | 전송 하려는 응답 데이터를 포함 하는 개체의 배열입니다. 각 개체의 데이터에 대 한 자세한 내용은 다음 표를 참조 하십시오. |


*응답* 배열의 각 개체에는 다음 값이 포함 됩니다.

| 값        | Type   | Description           |  필수  |
|---------------|--------|-----------------------------|-----|
| ApplicationId | 문자열 |  응답 하려는 검토가 포함 된 앱의 저장소 ID입니다. 저장소 ID는 파트너 센터의 [앱 id 페이지](../publish/view-app-identity-details.md) 에서 사용할 수 있습니다. 예 매장 ID는 9WZDNCRFJ3Q8입니다.   |  예  |
| ReviewId | 문자열 |  응답 하려는 검토의 ID (GUID)입니다. 검토 Id는 Microsoft Store 분석 API 및 [검토 보고서](../publish/reviews-report.md)의 [오프 라인 다운로드](../publish/download-analytic-reports.md) 에서 [앱 검토 가져오기](get-app-reviews.md) 메서드의 응답 데이터에서 사용할 수 있습니다.   |  예  |
| ResponseText | 문자열 | 전송 하려는 응답입니다. 응답은 [다음 지침](../publish/respond-to-customer-reviews.md#guidelines-for-responses)을 따라야 합니다.   |  예  |
| SupportEmail | 문자열 | 고객이 직접 연락 하는 데 사용할 수 있는 앱의 지원 전자 메일 주소입니다. 유효한 전자 메일 주소 여야 합니다.     |  예  |
| IsPublic | 부울 |  **True**를 지정 하면 고객의 검토 바로 아래에 앱의 스토어 목록에 응답이 표시 되 고 모든 고객에 게 표시 됩니다. **False** 를 지정 하 고 사용자가 전자 메일 응답을 받을 수 없는 경우, 전자 메일을 통해 고객에 게 응답이 전송 되며, 앱의 스토어 목록에서 다른 고객에 게 표시 되지 않습니다. **False** 를 지정 하 고 사용자가 전자 메일 응답을 받을 수 없는 경우 오류가 반환 됩니다.   |  예  |


### <a name="request-example"></a>요청 예제

다음 예제에서는이 메서드를 사용 하 여 여러 검토에 응답을 전송 하는 방법을 보여 줍니다.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/reviews/responses HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
{
  "Responses": [
    {
      "ApplicationId": "9WZDNCRFJ3Q8",
      "ReviewId": "6be543ff-1c9c-4534-aced-af8b4fbe0316",
      "ResponseText": "Thank you for pointing out this bug. I fixed it and published an update, you should have the fix soon",
      "SupportEmail": "support@contoso.com",
      "IsPublic": true
    },
    {
      "ApplicationId": "9NBLGGH1RP08",
      "ReviewId": "80c9671a-96c2-4278-bcbc-be0ce5a32a7c",
      "ResponseText": "Thank you for submitting your review. Can you tell more about what you were doing in the app when it froze? Thanks very much for your help.",
      "SupportEmail": "support@contoso.com",
      "IsPublic": false
    }
  ]
}
```

## <a name="response"></a>응답

### <a name="response-body"></a>응답 본문

| 값        | Type   | Description            |
|---------------|--------|---------------------|
| 결과 | array | 제출한 각 응답에 대 한 데이터를 포함 하는 개체의 배열입니다. 각 개체의 데이터에 대 한 자세한 내용은 다음 표를 참조 하십시오.  |


*결과* 배열의 각 개체에는 다음 값이 포함 됩니다.

| 값        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------|
| ApplicationId | 문자열 |  응답 한 검토가 있는 앱의 저장소 ID입니다. 예 매장 ID는 9WZDNCRFJ3Q8입니다.   |
| ReviewId | 문자열 |  응답 한 검토의 ID입니다. GUID입니다.   |
| 성공 | 문자열 | **True** 값은 응답이 성공적으로 전송 되었음을 나타냅니다. **False** 값은 응답에 실패 했음을 나타냅니다.    |
| FailureReason | 문자열 | **성공** 이 **false**이면이 값에 실패의 이유가 포함 됩니다. **성공** 이 **true**이면이 값이 비어 있습니다.      |


### <a name="response-example"></a>응답 예제

다음 예제에서는이 요청에 대 한 예제 JSON 응답 본문을 보여 줍니다.

```json
{
  "Result": [
    {
      "ApplicationId": "9WZDNCRFJ3Q8",
      "ReviewId": "6be543ff-1c9c-4534-aced-af8b4fbe0316",
      "Successful": "true",
      "FailureReason": ""
    },
    {
      "ApplicationId": "9NBLGGH1RP08",
      "ReviewId": "80c9671a-96c2-4278-bcbc-be0ce5a32a7c",
      "Successful": "false",
      "FailureReason": "No Permission"
    }
  ]
}
```

## <a name="related-topics"></a>관련 항목

* [파트너 센터를 사용 하 여 고객 리뷰에 응답](../publish/respond-to-customer-reviews.md)
* [Microsoft Store 서비스를 사용 하 여 검토에 대응](respond-to-reviews-using-windows-store-services.md)
* [앱 리뷰에 대 한 응답 정보 가져오기](get-response-info-for-app-reviews.md)
* [앱 리뷰 가져오기](get-app-reviews.md)
