---
description: Microsoft Store 제출 API에서이 메서드를 사용 하 여 패키지 비행 전송에 대 한 패키지 출시 비율을 업데이트 합니다.
title: 플라이트 제출에 대한 출시 백분율 업데이트
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 패키지 출시, 비행 전송, 업데이트, 백분율
ms.assetid: ee9aa223-e945-4c11-b430-1f4b1e559743
ms.localizationpriority: medium
ms.openlocfilehash: 449b4a72751c2f60628c15739200a28562564f44
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158327"
---
# <a name="update-the-rollout-percentage-for-a-flight-submission"></a>플라이트 제출에 대한 출시 백분율 업데이트


Microsoft Store 제출 API에서이 메서드를 사용 하 여 패키지 비행 제출의 [롤아웃 백분율을 업데이트](../publish/gradual-package-rollout.md#setting-the-rollout-percentage) 합니다. Microsoft Store 제출 API를 사용 하 여 패키지 비행 전송을 만드는 프로세스에 대 한 자세한 내용은 [패키지 비행 전송 관리](manage-flight-submissions.md)를 참조 하세요.

## <a name="prerequisites"></a>필수 조건

이 방법을 사용 하려면 먼저 다음을 수행 해야 합니다.

* 아직 수행 하지 않은 경우 Microsoft Store 제출 API에 대 한 모든 [필수 구성 요소](create-and-manage-submissions-using-windows-store-services.md#prerequisites) 를 완료 합니다.
* 이 메서드에 대 한 요청 헤더에 사용할 [AZURE AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) . 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후 새 토큰을 얻을 수 있습니다.
* 앱 중 하나에 대 한 제출을 만듭니다. 파트너 센터에서이 작업을 수행할 수도 있고, [앱 전송 만들기](create-an-app-submission.md) 방법을 사용 하 여이 작업을 수행할 수도 있습니다.
* 제출에 대 한 점진적 패키지 출시를 사용 하도록 설정 합니다. [파트너 센터에서](../publish/gradual-package-rollout.md)이 작업을 수행 하거나 [Microsoft Store 제출 API를 사용](manage-flight-submissions.md#manage-gradual-package-rollout)하 여이 작업을 수행할 수 있습니다.

## <a name="request"></a>요청

이 메서드의 구문은 다음과 같습니다. 헤더 및 요청 매개 변수에 대 한 사용 예제 및 설명은 다음 단원을 참조 하세요.

| 방법 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| POST   | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/updatepackagerolloutpercentage` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수 요소. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 이름        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 문자열 | 필수 요소. 업데이트 하려는 패키지 출시 비율을 사용 하 여 패키지 비행 제출을 포함 하는 앱의 저장소 ID입니다. 저장소 ID에 대 한 자세한 내용은 [앱 id 세부 정보 보기](../publish/view-app-identity-details.md)를 참조 하세요.  |
| flightId | 문자열 | 필수 요소. 업데이트 하려는 패키지 출시 비율을 사용 하 여 제출을 포함 하는 패키지 비행의 ID입니다. 이 ID는 [패키지 비행 만들기](create-a-flight.md) 요청에 대 한 응답 데이터와 [앱에 대 한 패키지 항공편 가져오기](get-flights-for-an-app.md)요청에 사용할 수 있습니다. 파트너 센터에서 만든 비행의 경우이 ID는 파트너 센터의 비행 페이지에 대 한 URL 에서도 사용할 수 있습니다.  |
| submissionId | 문자열 | 필수 요소. 업데이트 하려는 패키지 출시 백분율이 포함 된 제출의 ID입니다. 이 ID는 [패키지 비행 전송 만들기](create-a-flight-submission.md)요청에 대 한 응답 데이터에서 사용할 수 있습니다. 파트너 센터에서 만든 제출의 경우이 ID는 파트너 센터의 제출 페이지에 대 한 URL 에서도 사용할 수 있습니다.  |
| percentage  |  float  |  필수 요소. 점진적 출시 패키지를 수신 하는 사용자의 백분율입니다.  |


### <a name="request-body"></a>요청 본문

이 메서드에 대 한 요청 본문을 제공 하지 마십시오.

### <a name="request-example"></a>요청 예제

다음 예에서는 패키지 비행 전송에 대 한 패키지 출시 비율을 업데이트 하는 방법을 보여 줍니다.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243680/updatepackagerolloutpercentage?percentage=25 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

다음 예제에서는이 메서드를 성공적으로 호출 하기 위한 JSON 응답 본문을 보여 줍니다. 응답 본문의 값에 대 한 자세한 내용은 [패키지 롤아웃 리소스](manage-flight-submissions.md#package-rollout-object)를 참조 하세요.

```json
{
    "isPackageRollout": true,
    "packageRolloutPercentage": 25.0,
    "packageRolloutStatus": "PackageRolloutInProgress",
    "fallbackSubmissionId": "1212922684621243058"
}
```

## <a name="error-codes"></a>오류 코드

요청이 성공적으로 완료 될 수 없는 경우 응답은 다음 HTTP 오류 코드 중 하나를 포함 합니다.

| 오류 코드 |  Description   |
|--------|------------------|
| 404  | 패키지 비행 전송을 찾을 수 없습니다. |
| 409  | 이 코드는 다음 오류 중 하나를 나타냅니다.<br/><br/><ul><li>전송이 점진적 롤아웃 작업에 대해 올바른 상태가 아닙니다 .이 메서드를 호출 하기 전에 제출을 게시 하 고 [packageRolloutStatus](manage-flight-submissions.md#package-rollout-object) 값을 **PackageRolloutInProgress**로 설정 해야 합니다.</li><li>제출이 지정 된 앱에 속하지 않습니다.</li><li>앱이 [Microsoft Store 제출 API에서 현재 지원 하지](create-and-manage-submissions-using-windows-store-services.md#not_supported)않는 파트너 센터 기능을 사용 합니다.</li></ul> |   


## <a name="related-topics"></a>관련 항목

* [점진적 패키지 배포](../publish/gradual-package-rollout.md)
* [Microsoft Store 제출 API를 사용 하 여 패키지 비행 서브 미션 관리](manage-flight-submissions.md)
* [Microsoft Store 서비스를 사용 하 여 제출 작성 및 관리](create-and-manage-submissions-using-windows-store-services.md)