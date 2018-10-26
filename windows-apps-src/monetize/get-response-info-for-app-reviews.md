---
author: Xansky
ms.assetid: fb6bb856-7a1b-4312-a602-f500646a3119
description: Microsoft Store 리뷰 API에서 이 메서드를 사용하여 특정 리뷰에 응답할 수 있는지, 지정된 앱에 대한 리뷰에 응답할 수 있는지 판단할 수 있습니다.
title: 리뷰에 대한 응답 정보 가져오기
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 서비스, Microsoft Store 리뷰 API, 응답 정보
ms.localizationpriority: medium
ms.openlocfilehash: 71497a858060109eaac0f593ce03f2ba3cbf03cc
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "5572442"
---
# <a name="get-response-info-for-reviews"></a>리뷰에 대한 응답 정보 가져오기

앱의 고객 리뷰에 프로그래밍 방식으로 응답하려면 Microsoft Store 리뷰 API에서 이 메서드를 사용하여 먼저 리뷰에 응답할 권한이 있는지 확인할 수 있습니다. 리뷰 응답을 받지 않기로 선택한 고객이 제출한 리뷰에는 응답할 수 없습니다. 리뷰에 응답할 수 있는지 확인한 후 [앱 리뷰에 대한 응답 제출](submit-responses-to-app-reviews.md) 메서드를 사용하여 프로그래밍 방식으로 응답할 수 있습니다.


## <a name="prerequisites"></a>필수 조건

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 분석 API에 대한 모든 [필수 조건](respond-to-reviews-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](respond-to-reviews-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.
* 리뷰에 응답할 수 있는지 확인하려는 리뷰의 ID를 가져옵니다. 리뷰 ID는 Microsoft Store 분석 API의 [앱 리뷰 가져오기](get-app-reviews.md) 메서드의 응답 데이터와 [리뷰 보고서](../publish/reviews-report.md)의 [오프라인 다운로드](../publish/download-analytic-reports.md)에서 사용할 수 있습니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/reviews/{reviewId}/apps/{applicationId}/responses/info``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | 유형   | 설명                                     |  필수  |
|---------------|--------|--------------------------------------------------|--------------|
| applicationId | 문자열 | 응답할 수 있는지 확인하려는 리뷰가 포함된 앱의 스토어 ID입니다. 스토어 ID는 개발자 센터 대시보드의 [앱 ID 페이지](../publish/view-app-identity-details.md)에서 확인할 수 있습니다. 스토어 ID의 예로는 9WZDNCRFJ3Q8이 있습니다. |  예  |
| reviewId | 문자열 | 응답하려는 리뷰의 ID(GUID)입니다. 리뷰 ID는 Microsoft Store 분석 API의 [앱 리뷰 가져오기](get-app-reviews.md) 메서드의 응답 데이터와 [리뷰 보고서](../publish/reviews-report.md)의 [오프라인 다운로드](../publish/download-analytic-reports.md)에서 사용할 수 있습니다. <br/>이 매개 변수를 생략하면, 이 메서드에 대한 응답 본문에 지정된 앱의 리뷰에 응답할 수 있는 권한이 있는지 표시됩니다. |  아니요  |


### <a name="request-example"></a>요청 예제

다음 예제는 지정된 리뷰에 응답할 수 있는지 확인하기 위해 이 메서드를 사용하는 방법을 보여줍니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/reviews/6be543ff-1c9c-4534-aced-af8b4fbe0316/apps/9WZDNCRFJ3Q8/responses/info HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답


### <a name="response-body"></a>응답 본문

| 값      | 유형   | 설명    |  
|------------|--------|-----------------------|
| CanRespond      | 부울  | **true** 값은 지정된 리뷰에 응답할 수 있거나 지정된 앱에 대한 리뷰에 응답할 권한이 있음을 나타냅니다. 그렇지 않으면 이 값은 **false**입니다.       |
| DefaultSupportEmail  | 문자열 |  앱의 스토어 목록에 지정된 앱의 [지원 이메일 주소](../publish/enter-app-properties.md#support-contact-info)입니다. 지원 이메일 주소를 지정하지 않으면 이 필드는 비어 있습니다.    |

 
### <a name="response-example"></a>응답 예제

다음 예제에서는 이 요청에 대한 예제 JSON 응답 본문을 보여 줍니다.

```json
{
  "CanRespond": true,
  "DefaultSupportEmail": "support@contoso.com"
}
```

## <a name="related-topics"></a>관련 항목

* [Microsoft Store 분석 API를 사용하여 리뷰에 응답 제출](submit-responses-to-app-reviews.md)
* [개발자 센터 대시보드를 사용하여 고객 리뷰에 응답](../publish/respond-to-customer-reviews.md)
* [Microsoft Store 서비스를 사용하여 리뷰에 응답](respond-to-reviews-using-windows-store-services.md)
* [앱 리뷰 가져오기](get-app-reviews.md)
