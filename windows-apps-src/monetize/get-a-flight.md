---
ms.assetid: 87708690-079A-443D-807E-D2BF9F614DDF
description: Microsoft Store 제출 API에서이 메서드를 사용 하 여 파트너 센터 계정에 등록 된 앱에 대 한 패키지 비행 데이터를 가져옵니다.
title: 패키지 플라이트 가져오기
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 비행, 패키지 비행
ms.localizationpriority: medium
ms.openlocfilehash: 3916328f2c48642596c6a0b11401f583957bc973
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172907"
---
# <a name="get-a-package-flight"></a>패키지 플라이트 가져오기

Microsoft Store 제출 API에서이 메서드를 사용 하 여 파트너 센터 계정에 등록 된 앱에 대 한 패키지 비행 데이터를 가져옵니다.

## <a name="prerequisites"></a>필수 구성 요소

이 방법을 사용 하려면 먼저 다음을 수행 해야 합니다.

* 아직 수행 하지 않은 경우 Microsoft Store 제출 API에 대 한 모든 [필수 구성 요소](create-and-manage-submissions-using-windows-store-services.md#prerequisites) 를 완료 합니다.
* 이 메서드에 대 한 요청 헤더에 사용할 [AZURE AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) . 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후 새 토큰을 얻을 수 있습니다.

## <a name="request"></a>요청

이 메서드의 구문은 다음과 같습니다. 헤더 및 요청 본문의 사용 예제와 설명은 다음 섹션을 참조하세요.

| 방법 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}` |


### <a name="request-header"></a>요청 헤더

| header        | 유형   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수 요소. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 이름        | 유형   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 문자열 | 필수 요소. 가져오려는 패키지 비행이 포함 된 앱의 저장소 ID입니다. 앱에 대 한 저장소 ID는 파트너 센터에서 사용할 수 있습니다.  |
| flightId | 문자열 | 필수 요소. 가져올 패키지의 ID입니다. 이 ID는 [패키지 비행 만들기](create-a-flight.md) 요청에 대 한 응답 데이터와 [앱에 대 한 패키지 항공편 가져오기](get-flights-for-an-app.md)요청에 사용할 수 있습니다. 파트너 센터에서 만든 비행의 경우이 ID는 파트너 센터의 비행 페이지에 대 한 URL 에서도 사용할 수 있습니다.  |


### <a name="request-body"></a>요청 본문

이 메서드에 대 한 요청 본문을 제공 하지 마십시오.

### <a name="request-example"></a>요청 예제

다음 예제에서는 매장 ID 값 9WZDNCRD91MD를 사용 하 여 앱에 대 한 ID 43e448df-97c9-4a43-a0bc-2a445e736bcd를 사용 하 여 패키지 항공편에 대 한 정보를 검색 하는 방법을 보여 줍니다.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

다음 예제에서는이 메서드를 성공적으로 호출 하기 위한 JSON 응답 본문을 보여 줍니다. 응답 본문의 값에 대 한 자세한 내용은 다음 섹션을 참조 하십시오.

```json
{
  "flightId": "43e448df-97c9-4a43-a0bc-2a445e736bcd",
  "friendlyName": "myflight",
  "lastPublishedFlightSubmission": {
    "id": "1152921504621086517",
    "resourceLocation": "flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621086517"
  },
  "pendingFlightSubmission": {
    "id": "115292150462124364",
    "resourceLocation": "flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243647"
  },
  "groupIds": [
    "0"
  ],
  "rankHigherThan": "671c2857-725e-4faf-9e9e-ea1191ef879c"
}
```

### <a name="response-body"></a>응답 본문

| 값      | 형식   | Description                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightId            | 문자열  | 비행 패키지의 ID입니다. 이 값은 파트너 센터에서 제공 합니다.  |
| friendlyName           | 문자열  | 개발자가 지정한 패키지의 이름입니다.   |  
| lastPublishedFlightSubmission       | 개체 | 비행 패키지에 대해 마지막으로 게시 된 전송에 대 한 정보를 제공 하는 개체입니다. 자세한 내용은 아래의 [제출 개체](#submission_object) 섹션을 참조 하세요.  |
| pendingFlightSubmission        | 개체  |  비행 패키지에 대해 현재 보류 중인 전송에 대 한 정보를 제공 하는 개체입니다. 자세한 내용은 아래의 [제출 개체](#submission_object) 섹션을 참조 하세요.  |   
| groupIds           | array  | 항공편 패키지와 연결 된 비행 그룹의 Id를 포함 하는 문자열의 배열입니다. 비행 그룹에 대 한 자세한 내용은 [항공편 패키지](../publish/package-flights.md)를 참조 하세요.   |
| rankHigherThan           | 문자열  | 현재 패키지 비행 보다 즉시 순위가 매겨진 패키지 비행의 이름입니다. 비행 그룹의 순위를 지정 하는 방법에 대 한 자세한 내용은 [항공편 패키지](../publish/package-flights.md)를 참조 하세요.  |


<span id="submission_object" />

### <a name="submission-object"></a>제출 개체

응답 본문의 *lastPublishedFlightSubmission* 및 *pendingFlightSubmission* 값에는 패키지 비행의 전송에 대 한 리소스 정보를 제공 하는 개체가 포함 되어 있습니다. 이러한 개체의 값은 다음과 같습니다.

| 값           | 형식    | Description                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | 문자열  | 제출 ID입니다.    |
| resourceLocation   | 문자열  | `https://manage.devcenter.microsoft.com/v1.0/my/`전송에 대 한 전체 데이터를 검색 하기 위해 기본 요청 URI에 추가할 수 있는 상대 경로입니다.               |


## <a name="error-codes"></a>오류 코드

요청이 성공적으로 완료 될 수 없는 경우 응답은 다음 HTTP 오류 코드 중 하나를 포함 합니다.

| 오류 코드 |  Description     |
|--------|---------------------  |
| 400  | 요청이 잘못되었습니다. |
| 404  | 지정 된 패키지 비행을 찾을 수 없습니다.   |   
| 409  | 앱이 [Microsoft Store 제출 API에서 현재 지원 하지](create-and-manage-submissions-using-windows-store-services.md#not_supported)않는 파트너 센터 기능을 사용 합니다. |                                                                                                 


## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용 하 여 제출 작성 및 관리](create-and-manage-submissions-using-windows-store-services.md)
* [패키지 플라이트 만들기](create-a-flight.md)
* [패키지 플라이트 삭제](delete-a-flight.md)