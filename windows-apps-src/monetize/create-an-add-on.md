---
ms.assetid: 5BD650D2-AA26-4DE9-8243-374FDB7D932B
description: Microsoft Store 제출 API에서이 방법을 사용 하 여 사용자 센터 계정에 등록 된 앱에 대 한 추가 기능을 만듭니다.
title: 추가 기능 만들기
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 추가 기능 만들기, 앱 내 제품, IAP
ms.localizationpriority: medium
ms.openlocfilehash: a54ad4bcdb0c6fd652d56c767aa8362073b07875
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167727"
---
# <a name="create-an-add-on"></a>추가 기능 만들기

Microsoft Store 제출 API에서이 방법을 사용 하 여 파트너 센터 계정에 등록 된 앱에 대 한 추가 기능 (앱 내 제품 또는 IAP 라고도 함)을 만듭니다.

> [!NOTE]
> 이 메서드는 제출 하지 않고 추가 기능을 만듭니다. 추가 기능에 대 한 제출을 만들려면 [추가 기능 제출 관리](manage-add-on-submissions.md)의 메서드를 참조 하세요.

## <a name="prerequisites"></a>필수 구성 요소

이 방법을 사용 하려면 먼저 다음을 수행 해야 합니다.

* 아직 수행 하지 않은 경우 Microsoft Store 제출 API에 대 한 모든 [필수 구성 요소](create-and-manage-submissions-using-windows-store-services.md#prerequisites) 를 완료 합니다.
* 이 메서드에 대 한 요청 헤더에 사용할 [AZURE AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) . 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후 새 토큰을 얻을 수 있습니다.

## <a name="request"></a>요청

이 메서드의 구문은 다음과 같습니다. 헤더 및 요청 본문의 사용 예제와 설명은 다음 섹션을 참조하세요.

| 방법 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | `https://manage.devcenter.microsoft.com/v1.0/my/inappproducts` |


### <a name="request-header"></a>요청 헤더

| header        | 유형   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수 요소. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-body"></a>요청 본문

요청 본문에는 다음 매개 변수가 있습니다.

|  매개 변수  |  형식  |  Description  |  필수  |
|------|------|------|------|
|  applicationIds  |  array  |  이 추가 기능이 연결 된 앱의 저장소 ID를 포함 하는 배열입니다. 이 배열에서는 하나의 항목만 지원 됩니다.   |  예  |
|  productId  |  문자열  |  추가 기능에 대 한 제품 ID입니다. 이 식별자는 코드에서 추가 기능을 참조 하는 데 사용할 수 있는 식별자입니다. 자세한 내용은 [제품 유형 및 제품 ID 설정](../publish/set-your-add-on-product-id.md)을 참조 하세요.  |  예  |
|  productType  |  문자열  |  추가 기능에 대 한 제품 유형입니다. 지 **속성** 및 사용할 수 있는 값은 다음과 **같습니다.**  |  예  |


### <a name="request-example"></a>요청 예제

다음 예제에서는 앱에 대 한 새 소비 가능 추가 기능을 만드는 방법을 보여 줍니다.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1Q...
Content-Type: application/json
{
    "applicationIds": [  "9NBLGGH4R315"  ],
    "productId": "my-new-add-on",
    "productType": "Consumable",
}
```

## <a name="response"></a>응답

다음 예제에서는이 메서드를 성공적으로 호출 하기 위한 JSON 응답 본문을 보여 줍니다. 응답 본문의 값에 대 한 자세한 내용은 [추가 기능 리소스](manage-add-ons.md#add-on-object)를 참조 하세요.

```json
{
  "applications": {
    "value": [
      {
        "id": "9NBLGGH4R315",
        "resourceLocation": "applications/9NBLGGH4R315"
      }
    ],
    "totalCount": 1
  },
  "id": "9NBLGGH4TNMP",
  "productId": "my-new-add-on",
  "productType": "Consumable",
}
```

## <a name="error-codes"></a>오류 코드

요청이 성공적으로 완료 될 수 없는 경우 응답은 다음 HTTP 오류 코드 중 하나를 포함 합니다.

| 오류 코드 |  Description                                                                                                                                                                           |
|--------|------------------|
| 400  | 요청이 잘못되었습니다. |
| 409  | 현재 상태로 인해 추가 기능을 만들 수 없거나, 추가 기능에서 [현재 Microsoft Store 제출 API에서 지원 하지](create-and-manage-submissions-using-windows-store-services.md#not_supported)않는 파트너 센터 기능을 사용 하 고 있습니다. |   


## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용 하 여 제출 작성 및 관리](create-and-manage-submissions-using-windows-store-services.md)
* [추가 기능 제출 관리](manage-add-on-submissions.md)
* [모든 추가 기능 가져오기](get-all-add-ons.md)
* [추가 기능 가져오기](get-an-add-on.md)
* [추가 기능 삭제](delete-an-add-on.md)