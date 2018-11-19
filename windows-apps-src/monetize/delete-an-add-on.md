---
author: Xansky
ms.assetid: 16D4C3B9-FC9B-46ED-9F87-1517E1B549FA
description: Microsoft Store 제출 API에서에서이 메서드를 사용 하 여 파트너 센터 계정에 등록 된 앱에 대 한 추가 기능을 삭제 합니다.
title: 추가 기능 삭제
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 추가 기능, 삭제, 앱에서 바로 구매 제품, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 53c7125b48c32c8e39861f933f80fb88d6d904be
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7283803"
---
# <a name="delete-an-add-on"></a>추가 기능 삭제

Microsoft Store 제출 API에서에서이 메서드를 사용 하 여 파트너 센터 계정에 등록 된 앱에 대 한 추가 기능 (라고도 앱에서 제품 또는 IAP) 삭제 합니다.

## <a name="prerequisites"></a>사전 요구 사항

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 제출 API에 대한 모든 [필수 조건](create-and-manage-submissions-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청

이 메서드에는 다음 구문이 있습니다. 헤더 및 요청 본문의 사용 예제와 설명은 다음 섹션을 참조하세요.

| 메서드 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 이름        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| id | string | 필수. 삭제할 추가 기능의 스토어 ID입니다. 스토어 ID는 파트너 센터에서 사용할 수 있습니다.  |


### <a name="request-body"></a>요청 본문

이 메서드에 대한 요청 본문을 제공하지 않습니다.


### <a name="request-example"></a>요청 예제

다음 예제에서는 추가 기능을 삭제하는 방법을 보여 줍니다.

```
DELETE https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

요청이 성공하면 이 메서드는 빈 응답 본문을 반환합니다.

## <a name="error-codes"></a>오류 코드

요청을 성공적으로 완료할 수 없으면 응답에 다음 HTTP 오류 코드 중 하나가 포함됩니다.

| 오류 코드 |  설명                                                                                                                                                                           |
|--------|------------------|
| 400  | 요청이 잘못되었습니다. |
| 404  | 지정된 추가 기능을 찾을 수 없습니다.  |
| 409  | 지정 된 추가 기능의 현재 상태에서 삭제할 수 없거나 있는 [Microsoft Store 제출 API에서 지원 되지 않는 현재](create-and-manage-submissions-using-windows-store-services.md#not_supported)파트너 센터 기능을 사용 합니다. |   


## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)
* [모든 추가 기능 가져오기](get-all-add-ons.md)
* [추가 기능 가져오기](get-an-add-on.md)
* [추가 기능 만들기](create-an-add-on.md)
