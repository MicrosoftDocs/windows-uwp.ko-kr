---
ms.assetid: AD80F9B3-CED0-40BD-A199-AB81CDAE466C
description: Microsoft Store 제출 API 사용 하 여 파트너 센터 계정에 등록 된 앱에 대 한 패키지 항공편을 삭제 하려면이 메서드를 사용 합니다.
title: 패키지 플라이트 삭제
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 플라이트 삭제
ms.localizationpriority: medium
ms.openlocfilehash: a3455973d86b0c9e7c779cca429e36fa32266ed6
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58335111"
---
# <a name="delete-a-package-flight"></a>패키지 플라이트 삭제

Microsoft Store 제출 API 사용 하 여 파트너 센터 계정에 등록 된 앱에 대 한 패키지 항공편을 삭제 하려면이 메서드를 사용 합니다.


## <a name="prerequisites"></a>사전 요구 사항

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 제출 API에 대한 모든 [필수 조건](create-and-manage-submissions-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청

이 메서드에는 다음 구문이 있습니다. 헤더 및 요청 본문의 사용 예제와 설명은 다음 섹션을 참조하세요.

| 메서드 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| Delete    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 형식   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | 필수 사항입니다. 폼에서 Azure AD 액세스 토큰 **전달자** &lt; *토큰*&gt;합니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 이름        | 형식   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | 필수 사항입니다. 삭제할 패키지 플라이트가 포함된 앱의 스토어 ID입니다. 파트너 센터에서 앱에 대 한 Store ID 제공 됩니다.  |
| flightId | string | 필수. 삭제할 패키지 플라이트의 ID입니다. 이 ID는 개발자 센터 대시보드에서 사용할 수 있으며 [패키지 플라이트 만들기](create-a-flight.md) 및 [앱의 패키지 플라이트 가져오기](get-flights-for-an-app.md) 요청에 대한 응답 데이터에 포함되어 있습니다. 파트너 센터에서 생성 된 비행이이 ID 파트너 센터에서 비행 페이지의 URL에서 사용할 수 있는 이기도 합니다.  |


### <a name="request-body"></a>요청 본문

이 메서드에 대한 요청 본문을 제공하지 않습니다.


### <a name="request-example"></a>요청 예제

다음 예제에서는 패키지 플라이트를 삭제하는 방법을 보여 줍니다.

```json
DELETE https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

요청이 성공하면 이 메서드는 빈 응답 본문을 반환합니다.

## <a name="error-codes"></a>오류 코드

요청을 성공적으로 완료할 수 없으면 응답에 다음 HTTP 오류 코드 중 하나가 포함됩니다.

| 오류 코드 |  설명                                                                                                                                                                           |
|--------|------------------|
| 400  | 요청 매개 변수가 잘못되었습니다. |
| 404  | 지정된 패키지 플라이트를 찾을 수 없습니다.  |
| 409  | 지정 된 패키지 비행을 찾을 수 있지만 현재 상태의 삭제할 수 없습니다 또는 파트너 센터 기능을 사용 하는 앱 [현재 Microsoft Store 전송 API에 의해 지원 되지 않습니다](create-and-manage-submissions-using-windows-store-services.md#not_supported)합니다. |   


## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용 하 여 서브 미션을 만들고 설정 합니다.](create-and-manage-submissions-using-windows-store-services.md)
* [패키지 항공편 만들기](create-a-flight.md)
* [패키지 항공편 가져오기](get-a-flight.md)
