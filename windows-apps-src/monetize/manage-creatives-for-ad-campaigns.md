---
author: Xansky
ms.assetid: c5246681-82c7-44df-87e1-a84a926e6496
description: Microsoft Store 프로모션 API에서 이 메서드를 사용하여 홍보용 광고 캠페인 크리에이티브를 관리합니다.
title: 크리에이티브 관리
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store 프로모션 API, 광고 캠페인
ms.localizationpriority: medium
ms.openlocfilehash: 838329101695c21abfb7ac89dd9c83330b7bd26b
ms.sourcegitcommit: 2c4daa36fb9fd3e8daa83c2bd0825f3989d24be8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "5513839"
---
# <a name="manage-creatives"></a>크리에이티브 관리

Microsoft Store 프로모션 API에서 이 메서드를 사용하여 홍보용 광고 캠페인에서 사용할 자신만의 사용자 지정 크리에이티브를 업로드하거나 기존 크리에이티브를 가져옵니다. 크리에이티브는 하나 이상의 배달 라인과 연결될 수 있으며, 항상 동일한 앱을 나타내는 경우 여러 광고 캠페인에서 공유될 수도 있습니다.

크리에이티브와 광고 캠페인, 배달 라인 및 타기팅 프로필 간의 관계에 대한 자세한 내용은 [Microsoft Store 서비스를 사용하여 광고 캠페인 실행](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api)을 참조하세요.

> [!NOTE]
> 이 API를 사용하여 직접 만든 크리에이티브를 업로드할 때 크리에이티브에 허용되는 최대 크기는 40KB입니다. 이보다 큰 크리에이티브 파일을 제출하면 이 API에서 오류를 반환하지는 않지만 캠페인이 성공적으로 생성되지 않습니다.

## <a name="prerequisites"></a>필수 조건

이 메서드를 사용하려면 먼저 다음 작업을 완료해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 프로모션 API의 [필수 조건](run-ad-campaigns-using-windows-store-services.md#prerequisites)을 모두 완료합니다.
* 이 메서드의 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.


## <a name="request"></a>요청

이 메서드에는 다음 URI가 있습니다.

| 메서드 유형 | 요청 URI     |  설명  |
|--------|-----------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative``` |  새 크리에이티브를 만듭니다.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative/{creativeId}``` |  *creativeId*에서 지정한 크리에이티브를 가져옵니다.  |

> [!NOTE]
> 이 API는 현재 PUT 메서드를 지원하지 않습니다.


### <a name="header"></a>헤더

| 헤더        | 유형   | 설명         |
|---------------|--------|---------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |
| 추적 ID   | GUID   | 선택 사항입니다. 호출 흐름을 추적하는 ID입니다.                                  |


### <a name="request-body"></a>요청 본문

POST 메서드는 [크리에이티브](#creative) 개체의 필수 필드가 있는 JSON 요청 본문이 필요합니다.


### <a name="request-examples"></a>요청 예시

다음 예시에서는 POST 메서드를 호출하여 크리에이티브를 만드는 방법을 보여줍니다. 이 예시에서는 간략히 나타내기 위해 *콘텐츠* 값을 줄였습니다.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative HTTP/1.1
Authorization: Bearer <your access token>

{
  "name": "Contoso App Campaign - Creative 1",
  "content": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEAAQABAAD/2wBDAAgGB...other base64 data shortened for brevity...",
  "height": 80,
  "width": 480,
  "imageAttributes":
  {
    "imageExtension": "PNG"
  }
}
```

다음 예시에서는 GET 메서드를 호출하여 크리에이티브를 검색하는 방법을 보여줍니다.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative/106851  HTTP/1.1
Authorization: Bearer <your access token>
```


## <a name="response"></a>응답

이 메서드는 만들거나 검색한 크리에이티브에 대한 정보가 포함된 [크리에이티브](#creative) 개체가 있는 JSON 응답 본문을 반환합니다. 다음 예시에서는 이 메서드에 대한 응답 본문을 보여줍니다. 이 예시에서는 간략히 나타내기 위해 *콘텐츠* 값을 줄였습니다.

```json
{
    "Data": {
        "id": 106126,
        "name": "Contoso App Campaign - Creative 2",
        "content": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEAAQABAAD/2wBDAAgGB...other base64 data shortened for brevity...",
        "height": 50,
        "width": 300,
        "format": "Banner",
        "imageAttributes":
        {
          "imageExtension": "PNG"
        },
        "storeProductId": "9nblggh42cfd"
    }
}
```


<span id="creative"/>

## <a name="creative-object"></a>크리에이티브 개체

이 메서드에 대한 요청 및 응답 본문은 다음 필드를 포함합니다. 이 표에서는 읽기 전용(PUT 메서드에서 변경할 수 없음을 의미) 필드와 POST 메서드에 대한 요청 본문에서 필요한 필드가 어떤 것인지 보여줍니다.

| 필드        | 유형   |  설명      |  읽기 전용  | 기본값  |  POST에 필요한지 여부 |  
|--------------|--------|---------------|------|-------------|------------|
|  id   |  정수   |  크리에이티브의 ID입니다.     |   예    |      |    아니요   |       
|  name   |  문자열   |   크리에이티브의 이름입니다.    |    아니요   |      |  예     |       
|  콘텐츠   |  문자열   |  Base64 인코딩 형식의 크리에이티브 이미지 콘텐츠입니다.<br/><br/>**참고**&nbsp;&nbsp;크리에이티브에 허용되는 최대 크기는 40KB입니다. 이보다 큰 크리에이티브 파일을 제출하면 이 API에서 오류를 반환하지는 않지만 캠페인이 성공적으로 생성되지 않습니다.     |  아니요     |      |   예    |       
|  height   |  integer   |   크리에이티브의 높이입니다.    |    아니요    |      |   예    |       
|  width   |  integer   |  크리에이티브의 너비입니다.     |  아니요    |     |    예   |       
|  landingUrl   |  string   |  Kochava, AppsFlyer 또는 Tune과 같은 캠페인 추적 서비스를 사용하여 앱에 대한 설치 분석을 측정하는 경우 POST 메서드를 호출할 때 이 필드에 추적 URL을 할당합니다(지정할 경우 이 값은 유효한 URI여야 합니다). 캠페인 추적 서비스를 사용하지 않는 경우 POST 메서드를 호출할 때 이 값을 생략하세요(이 경우 이 URL이 자동으로 생성됩니다).   |  아니요    |     |   예    |       
|  format   |  string   |   광고 형식입니다. 현재는 **Banner** 값만 지원합니다.    |   아니요    |  배너   |  아니요     |       
|  imageAttributes   | [ImageAttributes](#image-attributes)    |   크리에이티브에 속성을 제공합니다.     |   아니요    |      |   예    |       
|  storeProductId   |  string   |   이 광고 캠페인이 연결된 앱의 [스토어 ID](in-app-purchases-and-trials.md#store-ids)입니다. 제품에 대한 예시 스토어 ID는 9nblggh42cfd입니다.    |   아니요    |    |  아니요     |   |  


<span id="image-attributes"/>

## <a name="imageattributes-object"></a>ImageAttributes 개체

| 필드        | 유형   |  설명      |  읽기 전용  | 기본값  | POST에 필요한지 여부 |  
|--------------|--------|---------------|------|-------------|------------|
|  imageExtension   |   string  |   **PNG** 또는 **JPG** 값 중 하나입니다.    |    아니요   |      |   예    |       |


## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용하여 광고 캠페인 실행](run-ad-campaigns-using-windows-store-services.md)
* [광고 캠페인 관리](manage-ad-campaigns.md)
* [광고 캠페인 배달 라인 관리](manage-delivery-lines-for-ad-campaigns.md)
* [광고 캠페인 타기팅 프로필 관리](manage-targeting-profiles-for-ad-campaigns.md)
* [광고 캠페인 성과 데이터 가져오기](get-ad-campaign-performance-data.md)
