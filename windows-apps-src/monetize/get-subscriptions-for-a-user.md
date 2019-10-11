---
ms.assetid: 94B5B2E9-BAEE-4B7F-BAF1-DA4D491427D7
description: Microsoft Store 구매 API에서 이 메서드를 사용하여 특정 사용자에게 사용 권한이 부여된 구독을 가져옵니다.
title: 사용자의 구독 가져오기
ms.date: 07/10/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 구매 API, 구독
ms.localizationpriority: medium
ms.openlocfilehash: f6ff6ea5a5daac1a6412c26c76dad899ca1f5881
ms.sourcegitcommit: 74c674c70b86bafeac7c8c749b1662fae838c428
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72252326"
---
# <a name="get-subscriptions-for-a-user"></a>사용자의 구독 가져오기

Microsoft Store 구매 API에서 이 메서드를 사용하여 특정 사용자에게 사용 권한이 부여된 구독 추가 기능을 가져옵니다.

> [!NOTE]
> 이 메서드는 UWP(유니버설 Windows 플랫폼) 앱에 대한 구독 추가 기능을 만들 수 있도록 Microsoft가 프로비전한 개발자 계정에서만 사용할 수 있습니다. 구독 추가 기능은 현재 대부분의 개발자 계정에서 사용할 수 없습니다.

## <a name="prerequisites"></a>사전 요구 사항

이 메서드를 사용하려면 다음이 필요합니다.

* 대상 URI 값이 `https://onestore.microsoft.com`인 Azure AD 액세스 토큰.
* 가져오려는 구독을 소유한 사용자의 ID를 나타내는 Microsoft Store ID 키.

자세한 내용은 [서비스에서 제품 권리 유형 관리](view-and-grant-products-from-a-service.md)를 참조하세요.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI                                            |
|--------|--------------------------------------------------------|
| 올리기   | ```https://purchase.mp.microsoft.com/v8.0/b2b/recurrences/query``` |


### <a name="request-header"></a>요청 헤더

| 헤더         | 형식   | 설명      |
|----------------|--------|-------------------|
| Authorization  | string | 필수. **전달자** 의 Azure AD 액세스 토큰 &lt;*토큰*&gt;입니다.                           |
| Host           | string | **purchase.mp.microsoft.com** 값으로 설정해야 합니다.                                            |
| Content-Length | number | 요청 본문의 길이입니다.                                                                       |
| Content-Type   | string | 요청 및 응답 유형을 지정합니다. 현재 **application/json** 값만 지원됩니다. |


### <a name="request-body"></a>요청 본문

| 매개 변수      | type   | 설명     | 필수 |
|----------------|--------|-----------------|----------|
| b2bKey         | string | 가져오려는 구독을 소유한 사용자의 ID를 나타내는 [Microsoft Store ID 키](view-and-grant-products-from-a-service.md#step-4).  | 예      |
| continuationToken |  string     |  사용자가 여러 구독에 대한 권한을 갖고 있는 경우 페이지 제한에 도달하면 응답 본문이 연속 토큰을 반환합니다. 나머지 제품을 검색하는 후속 호출에서 그 연속 토큰을 제공합니다.    | 아니요      |
| pageSize       | string | 하나의 응답에 반환하는 최대 구독 수입니다. 기본값은 25입니다.     |  아니요      |


### <a name="request-example"></a>요청 예제

다음 예제에서는 이 메서드를 사용하여 특정 사용자에게 사용 권한이 부여된 구독 추가 기능을 가져오는 방법을 보여 줍니다. *b2bKey* 값을 가져오려는 구독의 사용자 ID를 나타내는 [Microsoft Store ID 키](view-and-grant-products-from-a-service.md#step-4)로 바꿉니다.

```json
POST https://purchase.mp.microsoft.com/v8.0/b2b/recurrences/query HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
Host: https://purchase.mp.microsoft.com

{
  "b2bKey":  "eyJ0eXAiOiJ..."
}
```


## <a name="response"></a>응답

이 메서드는 사용자가 사용 권한을 갖고 있는 구독 추가 기능에 대해 설명하는 데이터 개체 컬렉션이 포함된 JSON 응답 본문을 반환합니다. 다음 예제에서는 한 구독에 대한 권한을 갖고 있는 사용자에 대한 응답 본문을 보여 줍니다.

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

응답 본문에는 다음 데이터가 포함됩니다.

| 값        | 형식   | 설명            |
|---------------|--------|---------------------|
| 항목 | 배열 | 지정된 사용자가 사용 권한을 갖고 있는 각 구독 추가 기능에 대한 데이터를 포함하고 있는 배열입니다. 각 개체의 데이터에 대한 자세한 내용은 다음 표를 참조하세요.  |


*항목* 배열의 각 개체에는 다음 값이 포함됩니다.

| 값        | 형식   | 설명                                                                 |
|---------------|--------|-----------------------------------------------|
| autoRenew | Boolean |  현재 구독 기간이 종료되면 구독을 자동으로 갱신할 것인지 여부를 나타냅니다.   |
| beneficiary | string |  이 구독과 연결된 권한의 수취인 ID입니다.   |
| expirationTime | string | 구독이 만료되는 날짜 및 시간이며, ISO 8601 형식입니다. 이 필드는 구독이 특정 상태일 때만 사용할 수 있습니다. 만료 시간은 일반적으로 현재 상태가 만료되는 시간을 나타냅니다. 예를 들어 활성 구독의 경우 만료 날짜는 다음 자동 갱신이 이루어지는 시간을 나타냅니다.    |
| expirationTimeWithGrace | string | ISO 8601 형식의 유예 기간을 포함 하 여 구독이 만료 되는 날짜와 시간입니다. 이 값은 구독이 자동으로 갱신 되지 못한 후 사용자가 구독에 대 한 액세스 권한을 상실 하는 시기를 나타냅니다.    |
| id | string |  구독의 ID입니다. 이 값을 사용하여 [change the billing state of a subscription for a user](change-the-billing-state-of-a-subscription-for-a-user.md) 메서드를 호출할 때 수정할 구독을 나타냅니다.    |
| isTrial | Boolean |  구독이 평가판인지 여부를 나타냅니다.     |
| lastModified | string |  구독이 마지막으로 수정된 날짜 및 시간이며, ISO 8601 형식입니다.      |
| 출시 | string | 사용자가 구독을 획득한 국가 코드입니다(두 문자로 된 ISO 3166-1 alpha-2 형식).      |
| productId | string |  Microsoft Store 카탈로그에서 구독 추가 기능을 나타내는 [제품](in-app-purchases-and-trials.md#products-skus-and-availabilities)의 [Store ID](in-app-purchases-and-trials.md#store-ids)입니다. 제품에 대한 스토어 ID의 예는 9NBLGGH42CFD입니다.     |
| skuId | string |  Microsoft Store 카탈로그에서 구독 추가 기능을 나타내는 [SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities)의 [Store ID](in-app-purchases-and-trials.md#store-ids)입니다. SKU에 대한 스토어 ID의 예는 0010입니다.    |
| startTime | string |  구독의 시작 날짜 및 시간이며, ISO 8601 형식입니다.     |
| recurrenceState | string  |  다음 값 중 하나입니다.<ul><li>**없음**:&nbsp;&nbsp;영구 구독을 나타냅니다.</li><li>**활성**:&nbsp;&nbsp;구독이 활성 상태이며 사용자가 서비스를 사용할 수 있습니다.</li><li>**비활성**:&nbsp;&nbsp;구독의 만료 날짜가 지났으며 사용자가 구독의 자동 갱신 옵션을 해제했습니다.</li><li>**취소됨**:&nbsp;&nbsp;만료 날짜 이전에 환불과 함께 또는 환불 없이 구독이 의도적으로 종료되었습니다.</li><li>**InDunning**:&nbsp;&nbsp;구독이 *재촉 중*(즉, 구독 만료 날짜가 얼마 남지 않아 Microsoft에서 구독을 자동 갱신하기 위한 금액을 취득하려 시도하는 중)입니다.</li><li>**실패**:&nbsp;&nbsp;재촉 기간이 종료되었으며 여러 차례 시도 후 구독 갱신에 실패했습니다.</li></ul><p>**참고:**</p><ul><li>**비활성**/**취소됨**/**실패**는 최종 상태입니다. 구독이 이러한 상태 중 하나가 되면 사용자는 구독을 재구매하여 다시 활성화해야 합니다. 사용자는 이러한 상태의 서비스를 사용할 수 없습니다.</li><li>구독 상태가 **취소됨**이면 expirationTime이 취소된 날짜 및 시간으로 업데이트됩니다.</li><li>구독의 ID는 전체 수명 주기 동안 그대로 유지됩니다. 자동 갱신 옵션이 켜져 있든 꺼져 있든 변경되지 않습니다. 최종 상태에 도달한 후 사용자가 구독을 재구매하는 경우 새 구독 ID가 생성됩니다.</li><li>각 구독에서 작업을 수행하려면 구독 ID를 사용하여야 합니다.</li><li>사용자가 구독을 취소 또는 중단한 후 구독을 재구매하는 경우 해당 사용자에 대한 결과를 쿼리하면 두 개 항목(하나는 최종 상태의 기존 구독 ID, 다른 하나는 활성 상태의 새 구독 ID)을 받게 됩니다.</li><li>recurrenceState 업데이트가 몇 분(때로는 몇 시간) 지연될 수 있으므로 항상 recurrenceState와 expirationTime을 모두 확인하는 것이 좋습니다.       |
| cancellationDate | string   |  사용자의 구독이 취소된 날짜 및 시간이며, ISO 8601 형식입니다.     |


## <a name="related-topics"></a>관련 항목

* [서비스에서 제품 자격 관리](view-and-grant-products-from-a-service.md)
* [사용자에 대 한 구독의 청구 상태 변경](change-the-billing-state-of-a-subscription-for-a-user.md)
* [제품 쿼리](query-for-products.md)
* [소비 가능한 제품을 충족 된 것으로 보고](report-consumable-products-as-fulfilled.md)
* [Microsoft Store ID 키 갱신](renew-a-windows-store-id-key.md)
