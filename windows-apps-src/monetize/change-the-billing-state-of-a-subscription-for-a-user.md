---
ms.assetid: F37C2CEC-9ED1-4F9E-883D-9FBB082504D4
description: Microsoft Store 구매 API에서 이 메서드를 사용하여 사용자의 구독 청구 상태를 변경할 수 있습니다.
title: 사용자의 구독 청구 상태 변경
ms.date: 08/01/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 구매 API, 구독
ms.localizationpriority: medium
ms.openlocfilehash: 9e4cf27331a218c0c0ef06ee1a80c141b889504a
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7699966"
---
# <a name="change-the-billing-state-of-a-subscription-for-a-user"></a>사용자의 구독 청구 상태 변경

Microsoft Store 구매 API에서 이 메서드를 사용하여 지정된 사용자의 구독 추가 기능 청구 상태를 변경할 수 있습니다. 구독을 취소, 연장, 환불 요청, 자동 갱신을 중지할 수 있습니다.

> [!NOTE]
> 이 메서드는 UWP(유니버설 Windows 플랫폼) 앱에 대한 구독 추가 기능을 만들 수 있도록 Microsoft가 프로비전한 개발자 계정에서만 사용할 수 있습니다. 구독 추가 기능은 현재 대부분의 개발자 계정에서 사용할 수 없습니다.

## <a name="prerequisites"></a>필수 구성 요소

이 메서드를 사용하려면 다음이 필요합니다.

* 대상 URI 값이 `https://onestore.microsoft.com`인 Azure AD 액세스 토큰.
* 변경할 구독에 대한 권한을 갖고 있는 사용자의 ID를 나타내는 Microsoft Store ID 키.

자세한 내용은 [서비스에서 제품 권리 유형 관리](view-and-grant-products-from-a-service.md)를 참조하세요.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI                                            |
|--------|--------------------------------------------------------|
| POST   | ```https://purchase.mp.microsoft.com/v8.0/b2b/recurrences/{recurrenceId}/change``` |


### <a name="request-header"></a>요청 헤더

| 헤더         | 유형   | 설명   |
|----------------|--------|-------------|
| 권한 부여  | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다.                           |
| 호스트           | 문자열 | **purchase.mp.microsoft.com** 값으로 설정해야 합니다.                                            |
| Content-Length | 숫자 | 요청 본문의 길이입니다.                                                                       |
| Content-Type   | 문자열 | 요청 및 응답 유형을 지정합니다. 현재 **application/json** 값만 지원됩니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 이름         | 유형  | 설명   |  필수  |
|----------------|--------|-------------|-----------|
| recurrenceId | 문자열 | 변경하려는 구독의 ID입니다. 이 ID를 가져오려면 [사용자의 구독을 가져오고](get-subscriptions-for-a-user.md) 메서드를 호출, 변경 하려는 구독 추가 기능을 나타내는 응답 본문 항목을 식별 하 고 항목에 대 한 **id** 필드의 값을 사용 합니다.     | 예      |


### <a name="request-body"></a>요청 본문

| 필드      | 유형   | 설명   | 필수 |
|----------------|--------|---------------|----------|
| b2bKey         | 문자열 | 구독을 변경할 사용자의 ID를 나타내는 [Microsoft Store ID 키](view-and-grant-products-from-a-service.md#step-4)입니다.     | 예      |
| changeType     | 문자열 |  수행하려는 변경 유형을 식별하는 다음 문자열 중 하나입니다.<ul><li>**취소**: 구독을 취소합니다.</li><li>**연장**: 구독을 연장 합니다. 이 값을 지정하는 경우 *extensionTimeInDays* 매개 변수도 포함해야 합니다.</li><li>**환불**: 고객에게 구독을 환불합니다.</li><li>**ToggleAutoRenew**: 구독에 자동 갱신을 사용하지 않습니다. 현재 구독의 자동 갱신이 비활성화되어 있으면 이 값이 아무 것도 수행하지 않습니다.</li></ul>   | 예      |
| extensionTimeInDays  | 문자열  | *changeType* 매개 변수 값이 **연장**이면 이 매개 변수는 구독을 연장하는 일 수를 지정합니다. |  *changeType*의 값이 **연장**이면 예, 그렇지 않으면 아니요입니다.  ||


### <a name="request-example"></a>요청 예제

다음 예제는 이 메서드를 사용하여 구독 기간을 5일 연장하는 방법을 보여 줍니다. *b2bKey* 값을 변경할 구독의 사용자 ID를 나타내는 [Microsoft Store ID 키](view-and-grant-products-from-a-service.md#step-4)로 바꿉니다.

```json
POST https://purchase.mp.microsoft.com/v8.0/b2b/recurrences/mdr:0:bc0cb6960acd4515a0e1d638192d77b7:77d5ebee-0310-4d23-b204-83e8613baaac/change HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
Host: https://purchase.mp.microsoft.com

{
  "b2bKey":  "eyJ0eXAiOiJ...",
  "changeType": "Extend",
  "extensionTimeInDays": "5"
}
```


## <a name="response"></a>Response

이 메서드는 수정된 필드를 포함하여 수정된 구독 추가 기능에 대한 정보가 포함된 JSON 응답 본문을 반환합니다. 다음 예제에서는 이 메서드의 응답 본문을 보여 줍니다.

```json
{
  "items": [
    {
      "autoRenew":true,
      "beneficiary":"pub:gFVuEBiZHPXonkYvtdOi+tLE2h4g2Ss0ZId0RQOwzDg=",
      "expirationTime":"2017-06-16T03:07:49.2552941+00:00",
      "id":"mdr:0:bc0cb6960acd4515a0e1d638192d77b7:77d5ebee-0310-4d23-b204-83e8613baaac",
      "lastModified":"2017-01-10T21:08:13.1459644+00:00",
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

응답 본문에는 다음 데이터가 포함됩니다.

| 값        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------|
| autoRenew | 부울 |  현재 구독 기간이 종료되면 구독을 자동으로 갱신할 것인지 여부를 나타냅니다.   |
| beneficiary | 문자열 |  이 구독과 연결된 권한의 수취인 ID입니다.   |
| expirationTime | 문자열 | 구독이 만료되는 날짜 및 시간이며, ISO 8601 형식입니다. 이 필드는 구독이 특정 상태일 때만 사용할 수 있습니다. 만료 시간은 일반적으로 현재 상태가 만료되는 시간을 나타냅니다. 예를 들어 활성 구독의 경우 만료 날짜는 다음 자동 갱신이 이루어지는 시간을 나타냅니다.    |
| expirationTimeWithGrace | string | 날짜 및 시간을 ISO 8601 형식에서 유예 기간을 포함 하 여 구독 만료 됩니다. 이 값 때 통해 해당 사용자 구독 자동 갱신에 실패 한 후 구독에 대 한 액세스를 잃게 됩니다.    |
| id | 문자열 |  구독의 ID입니다. 이 값을 사용하여 [change the billing state of a subscription for a user](change-the-billing-state-of-a-subscription-for-a-user.md) 메서드를 호출할 때 수정할 구독을 나타냅니다.    |
| isTrial | 부울 |  구독이 평가판인지 여부를 나타냅니다.     |
| lastModified | 문자열 |  구독이 마지막으로 수정된 날짜 및 시간이며, ISO 8601 형식입니다.      |
| market | 문자열 | 사용자가 구독을 획득한 국가 코드입니다(두 문자로 된 ISO 3166-1 alpha-2 형식).      |
| productId | 문자열 | Microsoft Store 카탈로그에서 구독 추가 기능을 나타내는 [제품](in-app-purchases-and-trials.md#products-skus-and-availabilities)의 [Store ID](in-app-purchases-and-trials.md#store-ids)입니다. 제품에 대한 스토어 ID의 예는 9NBLGGH42CFD입니다.     |
| skuId | 문자열 |  Microsoft Store 카탈로그에서 구독 추가 기능을 나타내는 [SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities)의 [Store ID](in-app-purchases-and-trials.md#store-ids)입니다. SKU에 대한 스토어 ID의 예는 0010입니다.    |
| startTime | 문자열 |  구독의 시작 날짜 및 시간이며, ISO 8601 형식입니다.     |
| recurrenceState | 문자열  |  다음 값 중 하나입니다.<ul><li>**없음**:&nbsp;&nbsp;영구 구독을 나타냅니다.</li><li>**활성**:&nbsp;&nbsp;구독이 활성 상태이며 사용자가 서비스를 사용할 수 있습니다.</li><li>**비활성**:&nbsp;&nbsp;구독의 만료 날짜가 지났으며 사용자가 구독의 자동 갱신 옵션을 해제했습니다.</li><li>**취소됨**:&nbsp;&nbsp;만료 날짜 이전에 환불과 함께 또는 환불 없이 구독이 의도적으로 종료되었습니다.</li><li>**InDunning**:&nbsp;&nbsp;구독이 *재촉 중*(즉, 구독 만료 날짜가 얼마 남지 않아 Microsoft에서 구독을 자동 갱신하기 위한 금액을 취득하려 시도하는 중)입니다.</li><li>**실패**:&nbsp;&nbsp;재촉 기간이 종료되었으며 여러 차례 시도 후 구독 갱신에 실패했습니다.</li></ul><p>**참고:**</p><ul><li>**비활성**/**취소됨**/**실패**는 최종 상태입니다. 구독이 이러한 상태 중 하나가 되면 사용자는 구독을 재구매하여 다시 활성화해야 합니다. 사용자는 이러한 상태의 서비스를 사용할 수 없습니다.</li><li>구독 상태가 **취소됨**이면 expirationTime이 취소된 날짜 및 시간으로 업데이트됩니다.</li><li>구독의 ID는 전체 수명 주기 동안 그대로 유지됩니다. 자동 갱신 옵션이 켜져 있든 꺼져 있든 변경되지 않습니다. 최종 상태에 도달한 후 사용자가 구독을 재구매하는 경우 새 구독 ID가 생성됩니다.</li><li>각 구독에서 작업을 수행하려면 구독 ID를 사용하여야 합니다.</li><li>사용자가 구독을 취소 또는 중단한 후 구독을 재구매하는 경우 해당 사용자에 대한 결과를 쿼리하면 두 개 항목(하나는 최종 상태의 기존 구독 ID, 다른 하나는 활성 상태의 새 구독 ID)을 받게 됩니다.</li><li>recurrenceState 업데이트가 몇 분(때로는 몇 시간) 지연될 수 있으므로 항상 recurrenceState와 expirationTime을 모두 확인하는 것이 좋습니다.       |
| cancellationDate | 문자열   |  사용자의 구독이 취소된 날짜 및 시간이며, ISO 8601 형식입니다.     |


## <a name="related-topics"></a>관련 항목


* [서비스에서 제품 권한 관리](view-and-grant-products-from-a-service.md)
* [사용자의 구독 가져오기](get-subscriptions-for-a-user.md)
* [제품에 대한 쿼리](query-for-products.md)
* [소모성 제품을 처리됨으로 보고](report-consumable-products-as-fulfilled.md)
* [Microsoft Store ID 키 갱신](renew-a-windows-store-id-key.md)
