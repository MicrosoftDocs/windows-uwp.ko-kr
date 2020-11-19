---
ms.assetid: 94B5B2E9-BAEE-4B7F-BAF1-DA4D491427D7
description: Microsoft Store 구매 API에서이 방법을 사용 하 여 지정 된 사용자가 사용할 수 있는 권한을 가진 구독을 가져옵니다.
title: 사용자의 구독 가져오기
ms.date: 07/10/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 구매 API, 구독
ms.localizationpriority: medium
ms.openlocfilehash: c77218cc7157e3d9a5508146395b284b57397d0f
ms.sourcegitcommit: 2a23972e9a0807256954d6da5cf21d0bbe7afb0a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/19/2020
ms.locfileid: "94941799"
---
# <a name="get-subscriptions-for-a-user"></a>사용자의 구독 가져오기

Microsoft Store 구매 API에서이 방법을 사용 하 여 지정 된 사용자에 게 사용 권한이 있는 구독 추가 기능을 가져옵니다.

> [!NOTE]
> 이 방법은 유니버설 Windows 플랫폼 (UWP) 앱에 대 한 구독 추가 기능을 만들 수 있도록 Microsoft에서 프로 비전 한 개발자 계정에만 사용할 수 있습니다. 현재 대부분의 개발자 계정에는 구독 추가 기능을 사용할 수 없습니다.

## <a name="prerequisites"></a>사전 요구 사항

이 방법을 사용 하려면 다음이 필요 합니다.

* 대상 URI 값을 포함 하는 Azure AD 액세스 토큰입니다 `https://onestore.microsoft.com` .
* 구독을 가져오려는 사용자의 id를 나타내는 Microsoft Store ID 키입니다.

자세한 내용은 [서비스에서 제품 자격 관리](view-and-grant-products-from-a-service.md)를 참조 하세요.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 방법 | 요청 URI                                            |
|--------|--------------------------------------------------------|
| POST   | ```https://purchase.mp.microsoft.com/v8.0/b2b/recurrences/query``` |


### <a name="request-header"></a>요청 헤더

| 헤더         | 유형   | Description      |
|----------------|--------|-------------------|
| 권한 부여  | 문자열 | 필수 요소. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다.                           |
| 호스트           | 문자열 | **Purchase.mp.microsoft.com** 값으로 설정 해야 합니다.                                            |
| Content-Length | 숫자 | 요청 본문의 길이입니다.                                                                       |
| 콘텐츠 형식   | 문자열 | 요청 및 응답 유형을 지정 합니다. 현재 유일 하 게 지원 되는 값은 **application/json** 입니다. |


### <a name="request-body"></a>요청 본문

| 매개 변수      | Type   | Description     | 필수 |
|----------------|--------|-----------------|----------|
| b2bKey         | 문자열 | 구독을 가져오려는 사용자의 id를 나타내는 [MICROSOFT STORE id 키](view-and-grant-products-from-a-service.md#step-4) 입니다.  | 예      |
| continuationToken |  문자열     |  사용자가 여러 구독에 대 한 자격을 보유 하는 경우 페이지 제한에 도달 하면 응답 본문에서 연속 토큰을 반환 합니다. 나머지 제품을 검색 하는 후속 호출에서 연속 토큰을 여기에 제공 합니다.    | 예      |
| pageSize       | 문자열 | 하나의 응답에서 반환할 최대 구독 수입니다. 기본값은 25입니다.     |  예      |


### <a name="request-example"></a>요청 예제

다음 예제에서는이 메서드를 사용 하 여 지정 된 사용자에 게 사용 권한이 있는 구독 추가 기능을 가져오는 방법을 보여 줍니다. *B2bKey* 값을 구독을 가져올 사용자의 id를 나타내는 [Microsoft Store id 키](view-and-grant-products-from-a-service.md#step-4) 로 바꿉니다.

```json
POST https://purchase.mp.microsoft.com/v8.0/b2b/recurrences/query HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
Host: purchase.mp.microsoft.com

{
  "b2bKey":  "eyJ0eXAiOiJ..."
}
```


## <a name="response"></a>응답

이 메서드는 사용자에 게 사용 권한이 있는 구독 추가 기능을 설명 하는 데이터 개체의 컬렉션을 포함 하는 JSON 응답 본문을 반환 합니다. 다음 예에서는 하나의 구독에 대 한 자격이 있는 사용자의 응답 본문을 보여 줍니다.

```json
{
  "items": [
    {
      "autoRenew":true,
      "beneficiary":"pub:gFVuEBiZHPXonkYvtdOi+tLE2h4g2Ss0ZId0RQOwzDg=",
      "expirationTime":"2017-06-11T03:07:49.2552941+00:00",
      "id":"mdr:0:bc0cb6960acd4515a0e1d638192d77b7:77d5ebee-0310-4d23-b204-83e8613baaac",
      "lastModified":"2017-01-08T21:07:51.1459644+00:00",
      "market":"US",
      "productId":"9NBLGGH52Q8X",
      "skuId":"0024",
      "startTime":"2017-01-10T21:07:49.2552941+00:00",
      "recurrenceState":"Active"
    }
  ]
}
```


### <a name="response-body"></a>응답 본문

응답 본문에는 다음 데이터가 포함 됩니다.

| 값        | Type   | Description            |
|---------------|--------|---------------------|
| items | array | 지정 된 사용자가 사용할 권한이 있는 각 구독 추가 기능에 대 한 데이터를 포함 하는 개체의 배열입니다. 각 개체의 데이터에 대 한 자세한 내용은 다음 표를 참조 하십시오.  |


*항목* 배열의 각 개체에는 다음 값이 포함 됩니다.

| 값        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------|
| autoRenew | 부울 |  현재 구독 기간이 끝날 때 구독이 자동으로 갱신 되도록 구성 되었는지 여부를 나타냅니다.   |
| 수혜자 | 문자열 |  이 구독과 연결 된 권리 수혜자의 ID입니다.   |
| expirationTime | 문자열 | 구독이 만료 되는 날짜 및 시간 (ISO 8601 형식)입니다. 이 필드는 구독이 특정 상태에 있는 경우에만 사용할 수 있습니다. 만료 시간은 일반적으로 현재 상태가 만료 되는 시점을 나타냅니다. 예를 들어 활성 구독의 경우 만료 날짜는 다음 자동 갱신이 발생 하는 시기를 나타냅니다.    |
| expirationTimeWithGrace | 문자열 | ISO 8601 형식의 유예 기간을 포함 하 여 구독이 만료 되는 날짜와 시간입니다. 이 값은 구독이 자동으로 갱신 되지 못한 후 사용자가 구독에 대 한 액세스 권한을 상실 하는 시기를 나타냅니다.    |
| id | 문자열 |  구독의 ID입니다. 이 값을 사용 하 여 [사용자 방법에 대 한 구독의 청구 상태 변경](change-the-billing-state-of-a-subscription-for-a-user.md) 을 호출할 때 수정할 구독을 지정할 수 있습니다.    |
| isTrial | 부울 |  구독이 평가판 인지 여부를 나타냅니다.     |
| lastModified | 문자열 |  구독이 마지막으로 수정 된 날짜와 시간 (ISO 8601 형식)입니다.      |
| market | 문자열 | 사용자가 구독을 획득 한 국가 코드 (두 문자로 된 ISO 3166-1 알파-2 형식)입니다.      |
| productId | 문자열 |  Microsoft Store 카탈로그의 구독 추가 기능을 나타내는 [제품](in-app-purchases-and-trials.md#products-skus-and-availabilities) 의 [저장소 ID](in-app-purchases-and-trials.md#store-ids) 입니다. 제품의 저장소 ID 예는 9NBLGGH42CFD입니다.     |
| skuId | 문자열 |  Microsoft Store 카탈로그의 구독 추가 기능을 나타내는 [SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities) 의 [저장소 ID](in-app-purchases-and-trials.md#store-ids) 입니다. SKU에 대 한 저장소 ID의 예는 mylnxcn-0006 mylnxcn-0010입니다.    |
| startTime | 문자열 |  ISO 8601 형식으로 된 구독의 시작 날짜와 시간입니다.     |
| recurrenceState | 문자열  |  다음 값 중 하나입니다.<ul><li>**없음**: &nbsp; &nbsp; 영구 구독을 나타냅니다.</li><li>**활성**: &nbsp; &nbsp; 구독이 활성화 되 고 사용자에 게 서비스를 사용할 수 있는 권한이 부여 됩니다.</li><li>**비활성**: &nbsp; &nbsp; 구독이 만료 날짜를 지났습니다. 사용자가 구독에 대 한 자동 갱신 옵션을 해제 했습니다.</li><li>**취소 됨**: &nbsp; &nbsp; 구독이 만료 날짜 이전에 의도적으로 또는 환불 없이 종료 되었습니다.</li><li>**InDunning**: 구독은 &nbsp; &nbsp; *dunning* 에 있습니다. 즉, 구독이 곧 만료 되며 Microsoft에서 자동으로 구독을 갱신 하기 위해 자금을 획득 하려고 합니다.</li><li>**실패**: &nbsp; &nbsp; dunning 기간이 초과 되었고 몇 번의 시도 후 구독을 갱신 하지 못했습니다.</li></ul><p>**참고:**</p><ul><li>**비활성** / **취소 됨** / **실패** 한 터미널 상태입니다. 구독이 이러한 상태 중 하나에 들어가면 사용자는 구독을 다시 구입 하 여 다시 활성화 해야 합니다. 사용자는 이러한 상태에서 서비스를 사용할 수 없습니다.</li><li>구독이 **취소** 되 면 expirationTime은 취소 날짜 및 시간으로 업데이트 됩니다.</li><li>구독의 ID는 전체 수명 동안 동일 하 게 유지 됩니다. 자동 갱신 옵션이 설정 되거나 해제 된 경우에는 변경 되지 않습니다. 터미널 상태에 도달한 후 사용자가 구독을 다시 구입 하는 경우 새 구독 ID가 만들어집니다.</li><li>구독 ID는 개별 구독에서 모든 작업을 실행 하는 데 사용 해야 합니다.</li><li>사용자가 취소 하거나 중단 후에 구독을 다시 구입 하는 경우 사용자에 대 한 결과를 쿼리하면 이전 구독 ID를 사용 하는 항목 및 활성 상태의 새 구독 id를 포함 하는 두 개의 항목을 가져옵니다.</li><li>RecurrenceState에 대 한 업데이트가 몇 분 (또는 가끔씩 시간) 지연 될 수 있으므로 recurrenceState와 expirationTime를 모두 확인 하는 것이 항상 좋은 방법입니다.       |
| cancellationDate | 문자열   |  ISO 8601 형식에서 사용자의 구독이 취소 된 날짜와 시간입니다.     |


## <a name="related-topics"></a>관련 항목

* [서비스에서 제품 권한 관리](view-and-grant-products-from-a-service.md)
* [사용자의 구독 청구 상태 변경](change-the-billing-state-of-a-subscription-for-a-user.md)
* [제품에 대한 쿼리](query-for-products.md)
* [소모성 제품을 처리됨으로 보고](report-consumable-products-as-fulfilled.md)
* [Microsoft Store ID 키 갱신](renew-a-windows-store-id-key.md)
