---
description: Microsoft Store 제출 API에서이 방법을 사용 하 여 앱 제출을 위한 패키지 출시를 중지 합니다.
title: 앱 제출에 대한 출시 중지
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 패키지 출시, 앱 전송, 중지
ms.assetid: 4ce79fe3-deda-4d31-b938-d672c3869051
ms.localizationpriority: medium
ms.openlocfilehash: ba6c32bcf61f16c5b734ad58c591a9575a3440a2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158547"
---
# <a name="halt-the-rollout-for-an-app-submission"></a>앱 제출에 대한 출시 중지


Microsoft Store 제출 API에서이 방법을 사용 하 여 앱 제출을 위한 [패키지 출시를 중지](../publish/gradual-package-rollout.md#completing-the-rollout) 합니다. Microsoft Store 제출 API를 사용 하 여 앱 제출을 만드는 프로세스에 대 한 자세한 내용은 [앱 제출 관리](manage-app-submissions.md)를 참조 하세요.

> [!NOTE]
> 앱 제출을 중단 하 고 [새 앱 제출을 만들면](create-an-app-submission.md)새 제출은 중단 된 전송의 복제본입니다.


## <a name="prerequisites"></a>필수 구성 요소

이 방법을 사용 하려면 먼저 다음을 수행 해야 합니다.

* 아직 수행 하지 않은 경우 Microsoft Store 제출 API에 대 한 모든 [필수 구성 요소](create-and-manage-submissions-using-windows-store-services.md#prerequisites) 를 완료 합니다.
* 이 메서드에 대 한 요청 헤더에 사용할 [AZURE AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) . 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후 새 토큰을 얻을 수 있습니다.
* 앱 중 하나에 대 한 제출을 만듭니다. 파트너 센터에서이 작업을 수행할 수도 있고, [앱 전송 만들기](create-an-app-submission.md) 방법을 사용 하 여이 작업을 수행할 수도 있습니다.
* 제출에 대 한 점진적 패키지 출시를 사용 하도록 설정 합니다. [파트너 센터에서](../publish/gradual-package-rollout.md)이 작업을 수행 하거나 [Microsoft Store 제출 API를 사용](manage-app-submissions.md#manage-gradual-package-rollout)하 여이 작업을 수행할 수 있습니다.

## <a name="request"></a>요청

이 메서드의 구문은 다음과 같습니다. 헤더 및 요청 매개 변수에 대 한 사용 예제 및 설명은 다음 단원을 참조 하세요.

| 방법 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| POST   | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/haltpackagerollout` |


### <a name="request-header"></a>요청 헤더

| header        | 유형   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수 요소. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 이름        | 유형   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 문자열 | 필수 요소. 중지할 패키지 롤아웃이 포함 된 앱의 저장소 ID입니다. 저장소 ID에 대 한 자세한 내용은 [앱 id 세부 정보 보기](../publish/view-app-identity-details.md)를 참조 하세요.  |
| submissionId | 문자열 | 필수 요소. 중지할 패키지 롤아웃의 전송 ID입니다. 이 ID는 [앱 제출을 만들기](create-an-app-submission.md)위한 요청에 대 한 응답 데이터에서 사용할 수 있습니다. 파트너 센터에서 만든 제출의 경우이 ID는 파트너 센터의 제출 페이지에 대 한 URL 에서도 사용할 수 있습니다.  |


### <a name="request-body"></a>요청 본문

이 메서드에 대 한 요청 본문을 제공 하지 마십시오.

### <a name="request-example"></a>요청 예제

다음 예제에서는 앱 제출을 위한 패키지 출시를 중지 하는 방법을 보여 줍니다.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621243680/haltpackagerollout HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

다음 예제에서는이 메서드를 성공적으로 호출 하기 위한 JSON 응답 본문을 보여 줍니다. 응답 본문의 값에 대 한 자세한 내용은 [패키지 롤아웃 리소스](manage-app-submissions.md#package-rollout-object)를 참조 하세요.

```json
{
    "isPackageRollout": true,
    "packageRolloutPercentage": 0.0,
    "packageRolloutStatus": "PackageRolloutStopped",
    "fallbackSubmissionId": "1212922684621243058"
}
```

## <a name="error-codes"></a>오류 코드

요청이 성공적으로 완료 될 수 없는 경우 응답은 다음 HTTP 오류 코드 중 하나를 포함 합니다.

| 오류 코드 |  Description   |
|--------|------------------|
| 404  | 제출을 찾을 수 없습니다. |
| 409  | 이 코드는 다음 오류 중 하나를 나타냅니다.<br/><br/><ul><li>전송이 점진적 롤아웃 작업에 대해 올바른 상태가 아닙니다 .이 메서드를 호출 하기 전에 제출을 게시 하 고 [packageRolloutStatus](manage-app-submissions.md#package-rollout-object) 값을 **PackageRolloutInProgress**로 설정 해야 합니다.</li><li>제출이 지정 된 앱에 속하지 않습니다.</li><li>앱이 [Microsoft Store 제출 API에서 현재 지원 하지](create-and-manage-submissions-using-windows-store-services.md#not_supported)않는 파트너 센터 기능을 사용 합니다.</li></ul> |   


## <a name="related-topics"></a>관련 항목

* [점진적 패키지 배포](../publish/gradual-package-rollout.md)
* [Microsoft Store 제출 API를 사용 하 여 앱 서브 미션 관리](manage-app-submissions.md)
* [Microsoft Store 서비스를 사용 하 여 제출 작성 및 관리](create-and-manage-submissions-using-windows-store-services.md)