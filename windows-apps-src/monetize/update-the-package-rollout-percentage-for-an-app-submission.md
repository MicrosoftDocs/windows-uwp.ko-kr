---
author: mcleanbyron
description: Microsoft Store 제출 API에서 이 메서드를 사용하여 앱 제출에 대한 패키지 출시 백분율을 업데이트합니다.
title: 앱 제출에 대한 출시 백분율 업데이트
ms.author: mcleans
ms.date: 04/17/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store 제출 API, 패키지 출시, 앱 제출, 업데이트, 백분율
ms.assetid: 4c82d837-7a25-4f3a-997e-b7be33b521cc
ms.localizationpriority: medium
ms.openlocfilehash: 3225a8387355f60a69ba892e4d9a379e552ff02c
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/30/2018
ms.locfileid: "1816118"
---
# <a name="update-the-rollout-percentage-for-an-app-submission"></a>앱 제출에 대한 출시 백분율 업데이트


Microsoft Store 제출 API에서 이 메서드를 사용하여 앱 제출에 대한 [출시 백분율을 업데이트](../publish/gradual-package-rollout.md#setting-the-rollout-percentage)합니다. Microsoft Store 제출 API를 사용하여 앱 제출을 만드는 프로세스의 절차에 대한 자세한 내용은 [앱 제출 관리](manage-app-submissions.md)를 참조하세요.


## <a name="prerequisites"></a>필수 조건

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 제출 API에 대한 모든 [필수 조건](create-and-manage-submissions-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.
* 개발자 센터 계정의 앱에 대한 제출을 만듭니다. 이 작업은 개발자 센터 대시보드에서 수행하거나 [앱 제출 만들기](create-an-app-submission.md) 메서드를 사용하여 수행할 수 있습니다.
* 제출에 대한 점진적 패키지 출시를 사용하도록 설정합니다. 이 작업은 [개발자 센터 대시보드](../publish/gradual-package-rollout.md)에서 수행하거나 [Microsoft Store 제출 API를 사용하여](manage-app-submissions.md#manage-gradual-package-rollout) 수행할 수 있습니다.

## <a name="request"></a>요청

이 메서드에는 다음 구문이 있습니다. 헤더 및 요청 매개 변수의 사용 예제와 설명은 다음 섹션을 참조하세요.

| 메서드 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/updatepackagerolloutpercentage``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 이름        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 문자열 | 필수. 업데이트하려는 패키지 출시 백분율의 제출을 포함하는 앱의 스토어 ID입니다. 스토어 ID에 대한 자세한 내용은 [앱 ID 세부 정보 보기](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)를 참조하세요.  |
| submissionId | 문자열 | 필수. 업데이트하려는 패키지 출시 백분율의 제출 ID입니다. 이 ID는 [앱 제출 만들기](create-an-app-submission.md) 요청에 대한 응답 데이터에서 사용할 수 있습니다. 개발자 센터 대시보드에서 만든 제출의 경우 이 ID는 대시보드에 있는 제출 페이지의 URL에도 사용할 수 있습니다.   |
| percentage  |  float  |  필수. 점진적 배포 패키지를 받을 사용자의 백분율입니다.  |


### <a name="request-body"></a>요청 본문

이 메서드에 대한 요청 본문을 제공하지 않습니다.

### <a name="request-example"></a>요청 예제

다음 예제에서는 앱 제출에 대한 패키지 출시 백분율을 업데이트하는 방법을 보여 줍니다.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621243680/updatepackagerolloutpercentage?percentage=25 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

다음 예제에서는 이 메서드를 성공적으로 호출하기 위한 JSON 응답 본문을 보여 줍니다. 응답 본문의 값에 대한 자세한 내용은 [패키지 출시 리소스](manage-app-submissions.md#package-rollout-object)를 참조하세요.

```json
{
    "isPackageRollout": true,
    "packageRolloutPercentage": 25.0,
    "packageRolloutStatus": "PackageRolloutInProgress",
    "fallbackSubmissionId": "1212922684621243058"
}
```

## <a name="error-codes"></a>오류 코드

요청을 성공적으로 완료할 수 없으면 응답에 다음 HTTP 오류 코드 중 하나가 포함됩니다.

| 오류 코드 |  설명   |
|--------|------------------|
| 404  | 제출을 찾을 수 없습니다. |
| 409  | 이 코드는 다음 오류 중 하나를 나타냅니다.<br/><br/><ul><li>제출이 점진적 배포 작업에 대해 유효한 상태가 아닙니다(이 메서드를 호출하기 전에 제출을 게시해야 하고 [packageRolloutStatus](manage-app-submissions.md#package-rollout-object) 값을 **PackageRolloutInProgress**로 설정해야 함).</li><li>제출이 지정된 앱에 속해 있지 않습니다.</li><li>앱이 [현재 Microsoft Store 제출 API에서 지원되지 않는](create-and-manage-submissions-using-windows-store-services.md#not_supported) 개발자 센터 대시보드 기능을 사용합니다.</li></ul> |   


## <a name="related-topics"></a>관련 항목

* [점진적 패키지 배포](../publish/gradual-package-rollout.md)
* [Microsoft Store 제출 API를 사용하여 앱 제출 관리](manage-app-submissions.md)
* [Microsoft Store 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)
