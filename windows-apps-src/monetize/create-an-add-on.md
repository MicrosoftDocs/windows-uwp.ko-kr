---
author: Xansky
ms.assetid: 5BD650D2-AA26-4DE9-8243-374FDB7D932B
description: Microsoft Store 제출 API에서 이 메서드를 사용하여 Windows 개발자 센터 계정에 등록된 앱에 대한 추가 기능을 만듭니다.
title: 추가 기능 만들기
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store 제출 API, 추가 기능 만들기, 앱에서 바로 구매 제품, IAP
ms.localizationpriority: medium
ms.openlocfilehash: a24355ca09380c46d8e648899ca2fe96f9e989c7
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/19/2018
ms.locfileid: "5160900"
---
# <a name="create-an-add-on"></a>추가 기능 만들기

Microsoft Store 제출 API에서 이 메서드를 사용하여 Windows 개발자 센터 계정에 등록된 앱에 대한 추가 기능(앱에서 바로 구매 제품 또는 IAP라고도 함)을 만듭니다.

> [!NOTE]
> 이 메서드는 제출 없이 추가 기능을 만듭니다. 추가 기능에 대한 제출을 만들려면 [추가 기능 제출 관리](manage-add-on-submissions.md)의 메서드를 참조하세요.

## <a name="prerequisites"></a>필수 조건

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 제출 API에 대한 모든 [필수 조건](create-and-manage-submissions-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청

이 메서드에는 다음 구문이 있습니다. 헤더 및 요청 본문의 사용 예제와 설명은 다음 섹션을 참조하세요.

| 메서드 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-body"></a>요청 본문

요청 본문에는 다음 매개 변수가 있습니다.

|  매개 변수  |  유형  |  설명  |  필수  |
|------|------|------|------|
|  applicationIds  |  배열  |  이 추가 기능이 연결된 앱의 스토어 ID가 포함된 배열입니다. 이 배열에서는 한 개의 항목만 지원됩니다.   |  예  |
|  productId  |  문자열  |  추가 기능의 제품 ID입니다. 코드에서 추가 기능을 참조하는 데 사용할 수 있는 식별자입니다. 자세한 내용은 [제품 유형 및 제품 ID 설정](https://msdn.microsoft.com/windows/uwp/publish/set-your-iap-product-id)을 참조하세요.  |  예  |
|  productType  |  문자열  |  추가 기능의 제품 유형입니다. **Durable** 및 **Consumable** 값이 지원됩니다.  |  예  |


### <a name="request-example"></a>요청 예제

다음 예제에는 앱에 대한 새 소모성 추가 기능을 만드는 방법을 보여 줍니다.

```syntax
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

다음 예제에서는 이 메서드를 성공적으로 호출하기 위한 JSON 응답 본문을 보여 줍니다. 응답 본문의 값에 대한 자세한 내용은 [추가 기능 리소스](manage-add-ons.md#add-on-object)를 참조하세요.

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

요청을 성공적으로 완료할 수 없으면 응답에 다음 HTTP 오류 코드 중 하나가 포함됩니다.

| 오류 코드 |  설명                                                                                                                                                                           |
|--------|------------------|
| 400  | 요청이 잘못되었습니다. |
| 409  | 현재 상태 때문에 추가 기능을 만들 수 없거나 추가 기능이 [현재 Microsoft Store 제출 API에서 지원되지 않는](create-and-manage-submissions-using-windows-store-services.md#not_supported) 개발자 센터 대시보드 기능을 사용합니다. |   


## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)
* [추가 기능 제출 관리](manage-add-on-submissions.md)
* [모든 추가 기능 가져오기](get-all-add-ons.md)
* [추가 기능 가져오기](get-an-add-on.md)
* [추가 기능 삭제](delete-an-add-on.md)
