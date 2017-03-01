---
author: mcleanbyron
ms.assetid: 038903d6-efab-4da6-96b5-046c7431e6e7
description: "Windows 스토어 리뷰 API에서 이 메서드를 사용하여 앱 리뷰에 응답을 제출합니다."
title: "앱 리뷰에 대한 응답 제출"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 스토어 서비스, Windows 스토어 리뷰 API, 추가 기능 구입"
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 1531059831b4c20d11661eb87fceda7b8dcb7f02
ms.lasthandoff: 02/08/2017

---

# <a name="submit-responses-to-app-reviews"></a>앱 리뷰에 대한 응답 제출


Windows 스토어 리뷰 API에서 이 메서드를 사용하여 앱 리뷰에 프로그래밍 방식으로 응답할 수 있습니다. 이 메서드를 호출하는 경우 응답하려는 리뷰의 ID를 지정해야 합니다. 리뷰 ID는 Windows 스토어 분석 API의 [앱 리뷰 가져오기](get-app-reviews.md) 메서드의 응답 데이터와 [리뷰 보고서](../publish/reviews-report.md)의 [오프라인 다운로드](../publish/download-analytic-reports.md)에서 사용할 수 있습니다.

리뷰를 제출하는 고객은 해당 리뷰에 대한 응답을 받지 않기로 선택할 수 있습니다. 고객이 응답을 받지 않겠다고 선택한 리뷰에 응답하려 하면 이 메서드의 응답 본문이 응답 시도가 실패한 것으로 나타냅니다. 이 메서드를 호출하기 전에 선택적으로 [앱 리뷰에 대한 응답 정보 가져오기](get-response-info-for-app-reviews.md) 메서드를 사용하여 지정된 리뷰에 응답할 수 있는지 여부를 확인할 수 있습니다.

>**참고**&nbsp;&nbsp;이 메서드를 사용하여 프로그래밍 방식으로 리뷰에 응답하는 이외에, [Windows 개발자 센터 대시보드를 사용](../publish/respond-to-customer-reviews.md)하여 리뷰에 응답할 수도 있습니다.

## <a name="prerequisites"></a>필수 조건

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Windows 스토어 리뷰 API에 대한 모든 [필수 조건](respond-to-reviews-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](respond-to-reviews-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.
* 응답하려는 리뷰의 ID를 가져옵니다. 리뷰 ID는 Windows 스토어 분석 API의 [앱 리뷰 가져오기](get-app-reviews.md) 메서드의 응답 데이터와 [리뷰 보고서](../publish/reviews-report.md)의 [오프라인 다운로드](../publish/download-analytic-reports.md)에서 사용할 수 있습니다.

## <a name="request"></a>요청

### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/reviews/responses``` |

<span/> 

### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |

<span/> 

### <a name="request-parameters"></a>요청 매개 변수

이 메서드에는 요청 매개 변수가 없습니다.

<span/> 

### <a name="request-body"></a>요청 본문

요청 본문에는 다음 값이 포함됩니다.

| 값        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------|
| 응답 | 배열 | 제출할 응답 데이터를 포함하는 개체의 배열입니다. 각 개체의 데이터에 대한 자세한 내용은 다음 표를 참조하세요. |

*응답* 배열의 각 객체에는 다음 값이 포함됩니다.

| 값        | 유형   | 설명           |  필수  |
|---------------|--------|-----------------------------|-----|
| ApplicationId | 문자열 |  응답할 리뷰가 포함된 앱의 스토어 ID입니다. 스토어 ID는 개발자 센터 대시보드의 [앱 ID 페이지](../publish/view-app-identity-details.md)에서 확인할 수 있습니다. 스토어 ID의 예로는 9WZDNCRFJ3Q8이 있습니다.   |  예  |
| ReviewId | 문자열 |  응답하려는 리뷰의 ID(GUID)입니다. 리뷰 ID는 Windows 스토어 분석 API의 [앱 리뷰 가져오기](get-app-reviews.md) 메서드의 응답 데이터와 [리뷰 보고서](../publish/reviews-report.md)의 [오프라인 다운로드](../publish/download-analytic-reports.md)에서 사용할 수 있습니다.   |  예  |
| ResponseText | 문자열 | 제출할 응답입니다. 응답은 [이 지침](../publish/respond-to-customer-reviews.md#guidelines-for-responses)을 따라 합니다.   |  예  |
| SupportEmail | 문자열 | 고객이 직접 문의할 때 사용할 수 있는 앱 지원 이메일 주소입니다. 유효한 메일 주소여야 합니다.     |  예  |
| IsPublic | 부울 |  값 **true**는 응답이 앱의 스토어 목록에서 고객 리뷰 바로 밑에 표시되고 모든 고객에게 노출됨을 나타냅니다. 값 **false**는 응답이 고객에게 메일을 통해 보내지고 앱의 스토어 목록에서 다른 고객에게 노출되지 않음을 나타냅니다.     |  예  |

### <a name="request-example"></a>요청 예제

다음 예제는 이 메서드를 사용하여 여러 리뷰에 대해 응답을 제출하는 방법입니다.

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
      "IsPublic": "true"
    },
    {
      "ApplicationId": "9NBLGGH1RP08",
      "ReviewId": "80c9671a-96c2-4278-bcbc-be0ce5a32a7c",
      "ResponseText": "Thank you for submitting your review. Can you tell more about what you were doing in the app when it froze? Thanks very much for your help.",
      "SupportEmail": "support@contoso.com",
      "IsPublic": "false"
    }
  ]
}
```

## <a name="response"></a>응답

### <a name="response-body"></a>응답 본문

| 값        | 유형   | 설명            |
|---------------|--------|---------------------|
| 결과 | 배열 | 제출한 각 응답에 대한 데이터를 포함하는 개체의 배열입니다. 각 개체의 데이터에 대한 자세한 내용은 다음 표를 참조하세요.  |

*결과* 배열의 각 객체에는 다음 값이 포함됩니다.

| 값        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------|
| ApplicationId | 문자열 |  응답한 리뷰가 포함된 앱의 스토어 ID입니다. 스토어 ID의 예로는 9WZDNCRFJ3Q8이 있습니다.   |
| ReviewId | 문자열 |  응답한 리뷰의 ID입니다. 이 ID는 GUID입니다.   |
| Successful | 문자열 | 값 **true**는 응답이 성공적으로 전송되었음을 나타냅니다. 값 **false** 응답이 실패했음을 나타냅니다.    |
| FailureReason | 문자열 | **Successful**이 **false**일 경우 이 값에 실패 이유가 포함됩니다. **Successful**이 **true**일 경우 이 값은 비어 있습니다.      |

### <a name="response-example"></a>응답 예제

다음 예제에서는 이 요청에 대한 예제 JSON 응답 본문을 보여 줍니다.

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

* [개발자 센터 대시보드를 사용하여 고객 리뷰에 응답](../publish/respond-to-customer-reviews.md)
* [Windows 스토어 서비스를 사용하여 리뷰에 응답](respond-to-reviews-using-windows-store-services.md)
* [앱 리뷰에 대한 응답 정보 가져오기](get-response-info-for-app-reviews.md)
* [앱 리뷰 가져오기](get-app-reviews.md)

