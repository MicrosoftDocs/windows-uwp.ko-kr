---
author: Xansky
ms.assetid: 039B8810-5C9E-4DB9-A6AF-33E7401311FF
description: Microsoft Store 제출 API에서 이 메서드를 사용하여 앱 제출의 상태를 가져옵니다.
title: 앱 제출의 상태 가져오기
ms.author: mhopkins
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 앱 제출, 상태
ms.localizationpriority: medium
ms.openlocfilehash: f92911d431579f9408b02680abf1d49f17740903
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6856732"
---
# <a name="get-the-status-of-an-app-submission"></a>앱 제출의 상태 가져오기

Microsoft Store 제출 API에서 이 메서드를 사용하여 앱 제출의 상태를 가져옵니다. Microsoft Store 제출 API를 사용하여 앱 제출을 만드는 프로세스의 절차에 대한 자세한 내용은 [앱 제출 관리](manage-app-submissions.md)를 참조하세요.

## <a name="prerequisites"></a>필수 조건

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 제출 API에 대한 모든 [필수 조건](create-and-manage-submissions-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청

이 메서드에는 다음 구문이 있습니다. 헤더 및 요청 본문의 사용 예제와 설명은 다음 섹션을 참조하세요.

| 메서드 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| GET   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 이름        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | 필수. 상태를 가져오려는 제출이 포함된 앱의 스토어 ID입니다. 스토어 ID에 대한 자세한 내용은 [앱 ID 세부 정보 보기](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)를 참조하세요.  |
| submissionId | string | 필수. 상태를 가져올 제출의 ID입니다. 이 ID는 [앱 제출 만들기](create-an-app-submission.md) 요청에 대한 응답 데이터에서 사용할 수 있습니다. 파트너 센터에서 생성 된 제출에 대해이 ID는 또한 파트너 센터에서 제출 페이지의 URL에 사용할 수 있습니다.  |


### <a name="request-body"></a>요청 본문

이 메서드에 대한 요청 본문을 제공하지 않습니다.

### <a name="request-example"></a>요청 예제

다음 예제에서는 앱 제출의 상태를 가져오는 방법을 보여 줍니다.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621243610/status HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

다음 예제에서는 이 메서드를 성공적으로 호출하기 위한 JSON 응답 본문을 보여 줍니다. 응답 본문에 지정된 제출에 대한 정보가 포함되어 있습니다. 응답 본문의 값에 대한 자세한 내용은 다음 섹션을 참조하세요.

```json
{
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
}
```

### <a name="response-body"></a>응답 본문

| 값      | 유형   | 설명                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| status           | 문자열  | 제출의 상태입니다. 다음 값 중 하나일 수 있습니다. <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>릴리스</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | object  |  오류에 대한 정보를 포함하여 제출 상태에 대한 추가 세부 정보가 포함됩니다. 자세한 내용은 [상태 세부 정보 리소스](manage-app-submissions.md#status-details-object)를 참조하세요. |


## <a name="error-codes"></a>오류 코드

요청을 성공적으로 완료할 수 없으면 응답에 다음 HTTP 오류 코드 중 하나가 포함됩니다.

| 오류 코드 |  설명   |
|--------|------------------|
| 404  | 제출을 찾을 수 없습니다. |
| 409  | 앱은 [Microsoft Store 제출 API에서 지원 되지 않는 현재](create-and-manage-submissions-using-windows-store-services.md#not_supported)파트너 센터 기능을 사용 합니다.  |


## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)
* [앱 제출 가져오기](get-an-app-submission.md)
* [앱 제출 만들기](create-an-app-submission.md)
* [앱 제출 커밋](commit-an-app-submission.md)
* [앱 제출 업데이트](update-an-app-submission.md)
* [앱 제출 삭제](delete-an-app-submission.md)
