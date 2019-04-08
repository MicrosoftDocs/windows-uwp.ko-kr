---
ms.assetid: 87708690-079A-443D-807E-D2BF9F614DDF
description: Microsoft Store 제출 API 사용 하 여이 메서드를 사용 하 여 파트너 센터 계정에 등록 된 앱에 대 한 패키지 항공편에 대 한 데이터를 가져오려고 합니다.
title: 패키지 플라이트 가져오기
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 플라이트, 패키지 플라이트
ms.localizationpriority: medium
ms.openlocfilehash: c4ff6c929a7264b5dece0057701c8348fe5d39be
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646038"
---
# <a name="get-a-package-flight"></a>패키지 플라이트 가져오기

Microsoft Store 제출 API 사용 하 여이 메서드를 사용 하 여 파트너 센터 계정에 등록 된 앱에 대 한 패키지 항공편에 대 한 데이터를 가져오려고 합니다.

## <a name="prerequisites"></a>필수 구성 요소

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 제출 API에 대한 모든 [필수 조건](create-and-manage-submissions-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청

이 메서드에는 다음 구문이 있습니다. 헤더 및 요청 본문의 사용 예제와 설명은 다음 섹션을 참조하세요.

| 메서드 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 형식   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. 폼에서 Azure AD 액세스 토큰 **전달자** &lt; *토큰*&gt;합니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 이름        | 형식   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 문자열 | 필수. 가져올 패키지 플라이트가 포함된 앱의 스토어 ID입니다. 파트너 센터에서 앱에 대 한 Store ID 제공 됩니다.  |
| flightId | 문자열 | 필수. 가져올 패키지 플라이트의 ID입니다. 이 ID는 개발자 센터 대시보드에서 사용할 수 있으며 [패키지 플라이트 만들기](create-a-flight.md) 및 [앱의 패키지 플라이트 가져오기](get-flights-for-an-app.md) 요청에 대한 응답 데이터에 포함되어 있습니다. 파트너 센터에서 생성 된 비행이이 ID 파트너 센터에서 비행 페이지의 URL에서 사용할 수 있는 이기도 합니다.  |


### <a name="request-body"></a>요청 본문

이 메서드에 대한 요청 본문을 제공하지 않습니다.

### <a name="request-example"></a>요청 예제

다음 예제에서는 스토어 ID 값이 9WZDNCRD91MD인 앱의 ID가 43e448df-97c9-4a43-a0bc-2a445e736bcd인 패키지 플라이트에 대한 정보를 검색하는 방법을 보여 줍니다.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

다음 예제에서는 이 메서드를 성공적으로 호출하기 위한 JSON 응답 본문을 보여 줍니다. 응답 본문의 값에 대한 자세한 내용은 다음 섹션을 참조하세요.

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

| 값      | 형식   | 설명                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightId            | 문자열  | 패키지 플라이트의 ID입니다. 이 값은 파트너 센터에서 제공 됩니다.  |
| FriendlyName           | 문자열  | 개발자가 지정한 패키지 플라이트 이름입니다.   |  
| lastPublishedFlightSubmission       | object | 패키지 플라이트의 마지막 게시된 제출에 대한 정보를 제공하는 개체입니다. 자세한 내용은 아래의 [제출 개체](#submission_object) 섹션을 참조하세요.  |
| pendingFlightSubmission        | object  |  패키지 플라이트의 현재 보류 중인 제출에 대한 정보를 제공하는 개체입니다. 자세한 내용은 아래의 [제출 개체](#submission_object) 섹션을 참조하세요.  |   
| groupIds           | 배열  | 패키지 플라이트와 연결된 플라이트 그룹의 ID가 포함된 문자열의 배열입니다. 플라이트 그룹에 대한 자세한 내용은 [패키지 플라이트](https://msdn.microsoft.com/windows/uwp/publish/package-flights)를 참조하세요.   |
| rankHigherThan           | 문자열  | 현재 패키지 플라이트보다 순위가 바로 아래인 패키지 플라이트의 식별 이름입니다. 플라이트 그룹의 순위 지정에 대한 자세한 내용은 [패키지 플라이트](https://msdn.microsoft.com/windows/uwp/publish/package-flights)를 참조하세요.  |


<span id="submission_object" />

### <a name="submission-object"></a>제출 개체

응답 본문의 *lastPublishedFlightSubmission* 및 *pendingFlightSubmission* 값에는 패키지 플라이트의 제출에 대한 리소스 정보를 제공하는 개체가 포함되어 있습니다. 이러한 개체는 다음 값을 갖습니다.

| 값           | 형식    | 설명                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | 문자열  | 제출의 ID입니다.    |
| resourceLocation   | 문자열  | 기본 ```https://manage.devcenter.microsoft.com/v1.0/my/``` 요청 URI에 추가하여 제출에 대한 전체 데이터를 검색할 수 있는 상대 경로입니다.               |


## <a name="error-codes"></a>오류 코드

요청을 성공적으로 완료할 수 없으면 응답에 다음 HTTP 오류 코드 중 하나가 포함됩니다.

| 오류 코드 |  설명     |
|--------|---------------------  |
| 400  | 요청이 잘못되었습니다. |
| 404  | 지정된 패키지 플라이트를 찾을 수 없습니다.   |   
| 409  | 파트너 센터 기능을 사용 하는 앱 [현재 Microsoft Store 전송 API에 의해 지원 되지 않습니다](create-and-manage-submissions-using-windows-store-services.md#not_supported)합니다. |                                                                                                 


## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용 하 여 서브 미션을 만들고 설정 합니다.](create-and-manage-submissions-using-windows-store-services.md)
* [패키지 항공편 만들기](create-a-flight.md)
* [패키지 항공편 삭제](delete-a-flight.md)
