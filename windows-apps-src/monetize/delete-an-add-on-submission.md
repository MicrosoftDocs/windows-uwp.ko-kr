---
ms.assetid: D677E126-C3D6-46B6-87A5-6237EBEDF1A9
description: Microsoft Store 제출 API에서 이 메서드를 사용하여 기존 추가 기능 제출을 삭제합니다.
title: 추가 기능 제출 삭제
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 추가 기능 제출, 삭제, 앱에서 바로 구매 제품, IAP
ms.localizationpriority: medium
ms.openlocfilehash: cdcd74b86ce846f19f8a4eb912781762adb66a2d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655778"
---
# <a name="delete-an-add-on-submission"></a>추가 기능 제출 삭제

Microsoft Store 제출 API에서 이 메서드를 사용하여 기존 추가 기능(앱에서 바로 구매 제품 또는 IAP라고도 함) 제출을 삭제합니다.

## <a name="prerequisites"></a>필수 구성 요소

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 제출 API에 대한 모든 [필수 조건](create-and-manage-submissions-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청

이 메서드에는 다음 구문이 있습니다. 헤더 및 요청 본문의 사용 예제와 설명은 다음 섹션을 참조하세요.

| 메서드 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId}``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 형식   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. 폼에서 Azure AD 액세스 토큰 **전달자** &lt; *토큰*&gt;합니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 이름        | 형식   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | 문자열 | 필수. 삭제할 제출이 포함된 추가 기능의 스토어 ID입니다. 파트너 센터에서 Store ID 제공 됩니다.  |
| submissionId | 문자열 | 필수. 삭제할 제출의 ID입니다. 이 ID는 [추가 기능 제출 만들기](create-an-add-on-submission.md) 요청에 대한 응답 데이터에서 사용할 수 있습니다. 파트너 센터에서 생성 된 제출에 대 한이 ID의 파트너 센터에서 제출 페이지의 URL을 사용할 수도 있습니다.  |


### <a name="request-body"></a>요청 본문

이 메서드에 대한 요청 본문을 제공하지 않습니다.


### <a name="request-example"></a>요청 예제

다음 예제에서는 추가 기능 제출을 삭제하는 방법을 보여 줍니다.

```
DELETE https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions/1152921504621230023 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

요청이 성공하면 이 메서드는 빈 응답 본문을 반환합니다.

## <a name="error-codes"></a>오류 코드

요청을 성공적으로 완료할 수 없으면 응답에 다음 HTTP 오류 코드 중 하나가 포함됩니다.

| 오류 코드 |  설명   |
|--------|------------------|
| 400  | 요청 매개 변수가 잘못되었습니다. |
| 404  | 지정된 제출을 찾을 수 없습니다. |
| 409  | 지정 된 전송 찾을 수 있지만 현재 상태의 삭제할 수 없습니다 또는 파트너 센터 기능을 사용 하는 추가 기능 [현재 Microsoft Store 전송 API에 의해 지원 되지 않습니다](create-and-manage-submissions-using-windows-store-services.md#not_supported)합니다. |


## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용 하 여 서브 미션을 만들고 설정 합니다.](create-and-manage-submissions-using-windows-store-services.md)
* [추가 기능 제출 가져오기](get-an-add-on-submission.md)
* [추가 기능 제출 만들기](create-an-add-on-submission.md)
* [추가 기능 제출 커밋](commit-an-add-on-submission.md)
* [업데이트는 추가 기능 제출](update-an-add-on-submission.md)
* [추가 기능 제출 상태를 가져옵니다.](get-status-for-an-add-on-submission.md)
