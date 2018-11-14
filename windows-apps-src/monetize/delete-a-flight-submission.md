---
author: Xansky
ms.assetid: 1A69A388-B1CC-4D2C-886B-EA07E6E60252
description: Microsoft Store 제출 API에서 이 메서드를 사용하여 기존 패키지 플라이트 제출을 삭제합니다.
title: 패키지 플라이트 제출 삭제
ms.author: mhopkins
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 플라이트 제출, 삭제, 패키지 플라이트
ms.localizationpriority: medium
ms.openlocfilehash: 2196a6b7023a062905ae721ebdb536e2c8044057
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/12/2018
ms.locfileid: "6448688"
---
# <a name="delete-a-package-flight-submission"></a>패키지 플라이트 제출 삭제

Microsoft Store 제출 API에서 이 메서드를 사용하여 기존 패키지 플라이트 제출을 삭제합니다.

## <a name="prerequisites"></a>필수 조건

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 제출 API에 대한 모든 [필수 조건](create-and-manage-submissions-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청

이 메서드에는 다음 구문이 있습니다. 헤더 및 요청 본문의 사용 예제와 설명은 다음 섹션을 참조하세요.

| 메서드 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationid}/flights/{flightId}/submissions/{submissionId}``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 이름        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | 필수. 삭제할 패키지 플라이트 제출이 포함된 앱의 스토어 ID입니다. 스토어 ID에 대한 자세한 내용은 [앱 ID 세부 정보 보기](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)를 참조하세요.  |
| flightId | 문자열 | 필수. 삭제할 제출이 포함된 패키지 플라이트의 ID입니다. 이 ID는 개발자 센터 대시보드에서 사용할 수 있으며 [패키지 플라이트 만들기](create-a-flight.md) 및 [앱의 패키지 플라이트 가져오기](get-flights-for-an-app.md) 요청에 대한 응답 데이터에 포함되어 있습니다. 파트너 센터에서 만든 플라이트 경우이 ID 파트너 센터에서 플라이트 페이지의 URL에 사용할 수 있는 이기도 합니다.  |
| submissionId | string | 필수. 삭제할 제출의 ID입니다. 이 ID는 [패키지 플라이트 제출 만들기](create-a-flight-submission.md) 요청에 대한 응답 데이터에서 사용할 수 있습니다. 파트너 센터에서 생성 된 제출에 대해이 ID는 또한 파트너 센터에서 제출 페이지의 URL에 사용할 수 있습니다.  |


### <a name="request-body"></a>요청 본문

이 메서드에 대한 요청 본문을 제공하지 않습니다.


### <a name="request-example"></a>요청 예제

다음 예제에서는 패키지 플라이트 제출을 삭제하는 방법을 보여 줍니다.

```
DELETE https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649 HTTP/1.1
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
| 409  | 지정 된 제출을 찾았지만 현재 상태로 삭제할 수 없거나 앱은 [Microsoft Store 제출 API에서 지원 되지 않는 현재](create-and-manage-submissions-using-windows-store-services.md#not_supported)는 파트너 센터 기능을 사용 합니다. |


## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)
* [패키지 플라이트 제출 관리](manage-flight-submissions.md)
* [패키지 플라이트 제출 가져오기](get-a-flight-submission.md)
* [패키지 플라이트 제출 만들기](create-a-flight-submission.md)
* [패키지 플라이트 제출 커밋](commit-a-flight-submission.md)
* [패키지 플라인트 제출 업데이트](update-a-flight-submission.md)
* [패키지 플라이트 제출 상태 가져오기](get-status-for-a-flight-submission.md)
