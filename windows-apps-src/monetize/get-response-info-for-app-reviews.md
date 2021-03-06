---
ms.assetid: fb6bb856-7a1b-4312-a602-f500646a3119
description: Microsoft Store 리뷰 API에서 이 메서드를 사용하여 특정 리뷰에 응답할 수 있는지, 지정된 앱에 대한 리뷰에 응답할 수 있는지 판단할 수 있습니다.
title: 리뷰에 대한 응답 정보 가져오기
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 서비스, Microsoft Store 리뷰 API, 응답 정보
ms.localizationpriority: medium
ms.openlocfilehash: 095afd1eab9b7bd0acdac7c38d9e8e99dd59f38c
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63785752"
---
# <a name="get-response-info-for-reviews"></a>리뷰에 대한 응답 정보 가져오기

앱의 고객 리뷰에 프로그래밍 방식으로 응답하려면 Microsoft Store 리뷰 API에서 이 메서드를 사용하여 먼저 리뷰에 응답할 권한이 있는지 확인할 수 있습니다. 리뷰 응답을 받지 않기로 선택한 고객이 제출한 리뷰에는 응답할 수 없습니다. 리뷰에 응답할 수 있는지 확인한 후 [앱 리뷰에 대한 응답 제출](submit-responses-to-app-reviews.md) 메서드를 사용하여 프로그래밍 방식으로 응답할 수 있습니다.


## <a name="prerequisites"></a>사전 요구 사항

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 분석 API에 대한 모든 [필수 조건](respond-to-reviews-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](respond-to-reviews-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.
* 리뷰에 응답할 수 있는지 확인하려는 리뷰의 ID를 가져옵니다. 리뷰 ID는 Microsoft Store 분석 API의 [앱 리뷰 가져오기](get-app-reviews.md) 메서드의 응답 데이터와 [리뷰 보고서](../publish/reviews-report.md)의 [오프라인 다운로드](../publish/download-analytic-reports.md)에서 사용할 수 있습니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| 가져오기    | ```https://manage.devcenter.microsoft.com/v1.0/my/reviews/{reviewId}/apps/{applicationId}/responses/info``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 형식   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | 필수. 폼에서 Azure AD 액세스 토큰 **전달자** &lt; *토큰*&gt;합니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | 형식   | 설명                                     |  필수  |
|---------------|--------|--------------------------------------------------|--------------|
| applicationId | string | 응답할 수 있는지 확인하려는 리뷰가 포함된 앱의 스토어 ID입니다. Store ID 사용할 수는 [앱 id 페이지](../publish/view-app-identity-details.md) 파트너 센터에서. 스토어 ID의 예로는 9WZDNCRFJ3Q8이 있습니다. |  예  |
| reviewId | string | 응답하려는 리뷰의 ID(GUID)입니다. 리뷰 ID는 Microsoft Store 분석 API의 [앱 리뷰 가져오기](get-app-reviews.md) 메서드의 응답 데이터와 [리뷰 보고서](../publish/reviews-report.md)의 [오프라인 다운로드](../publish/download-analytic-reports.md)에서 사용할 수 있습니다. <br/>이 매개 변수를 생략하면, 이 메서드에 대한 응답 본문에 지정된 앱의 리뷰에 응답할 수 있는 권한이 있는지 표시됩니다. |  아니오  |


### <a name="request-example"></a>요청 예제

다음 예제는 지정된 리뷰에 응답할 수 있는지 확인하기 위해 이 메서드를 사용하는 방법을 보여줍니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/reviews/6be543ff-1c9c-4534-aced-af8b4fbe0316/apps/9WZDNCRFJ3Q8/responses/info HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답


### <a name="response-body"></a>응답 본문

| 값      | 형식   | 설명    |  
|------------|--------|-----------------------|
| CanRespond      | Boolean  | **true** 값은 지정된 리뷰에 응답할 수 있거나 지정된 앱에 대한 리뷰에 응답할 권한이 있음을 나타냅니다. 그렇지 않으면 이 값은 **false**입니다.       |
| DefaultSupportEmail  | string |  앱의 스토어 목록에 지정된 앱의 [지원 이메일 주소](../publish/enter-app-properties.md#support-contact-info)입니다. 지원 이메일 주소를 지정하지 않으면 이 필드는 비어 있습니다.    |

 
### <a name="response-example"></a>응답 예제

다음 예제에서는 이 요청에 대한 예제 JSON 응답 본문을 보여 줍니다.

```json
{
  "CanRespond": true,
  "DefaultSupportEmail": "support@contoso.com"
}
```

## <a name="related-topics"></a>관련 항목

* [Microsoft Store 분석 API를 사용 하 여 리뷰에 대 한 응답을 제출 합니다.](submit-responses-to-app-reviews.md)
* [파트너 센터를 사용 하 여 고객 리뷰에 응답](../publish/respond-to-customer-reviews.md)
* [Microsoft Store 서비스를 사용 하 여 리뷰에 응답](respond-to-reviews-using-windows-store-services.md)
* [앱 검토를 가져오기](get-app-reviews.md)
