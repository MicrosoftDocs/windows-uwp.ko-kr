---
ms.assetid: 934F2DBF-2C7E-4B77-997D-17B9B0535D51
description: 파트너 센터는 신규 또는 업데이트 된 앱 제출 커밋 Microsoft Store 제출 API에서에서이 메서드를 사용 합니다.
title: 앱 제출 커밋
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 앱 제출 커밋
ms.localizationpriority: medium
ms.openlocfilehash: 36c645303a86bc28f12fa8e31b8a83653c167664
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371991"
---
# <a name="commit-an-app-submission"></a>앱 제출 커밋

파트너 센터는 신규 또는 업데이트 된 앱 제출 커밋 Microsoft Store 제출 API에서에서이 메서드를 사용 합니다. 커밋 작업 경고 파트너 센터 전송 데이터 (모든 관련된 패키지 및 이미지 포함) 업로드 되었습니다. 파트너 센터에 대 한 응답에서 제출 데이터를 수집 하 고 게시의 변경 사항을 커밋합니다. 커밋 작업이 성공한 후 변경 내용은 제출 하려면 파트너 센터에 표시 됩니다.

커밋 작업이 Microsoft Store 제출 API를 사용하여 앱을 제출하는 프로세스에 적용되는 방법은 [앱 제출 관리](manage-app-submissions.md)를 참조하세요.

## <a name="prerequisites"></a>사전 요구 사항

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 제출 API에 대한 모든 [필수 조건](create-and-manage-submissions-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.
* [앱 제출을 만든](create-an-app-submission.md) 다음 제출 데이터에 필요한 모든 변경 사항으로 [제출을 업데이트](update-an-app-submission.md)합니다.

## <a name="request"></a>요청

이 메서드에는 다음 구문이 있습니다. 헤더 및 요청 본문의 사용 예제와 설명은 다음 섹션을 참조하세요.

| 메서드 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| 올리기    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 형식   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | 필수. 폼에서 Azure AD 액세스 토큰 **전달자** &lt; *토큰*&gt;합니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 이름        | 형식   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | 필수. 커밋하려는 제출이 포함된 앱의 스토어 ID입니다. 스토어 ID에 대한 자세한 내용은 [앱 ID 세부 정보 보기](https://docs.microsoft.com/windows/uwp/publish/view-app-identity-details)를 참조하세요.  |
| submissionId | string | 필수. 커밋하려는 제출의 ID입니다. 이 ID는 [앱 제출 만들기](create-an-app-submission.md) 요청에 대한 응답 데이터에서 사용할 수 있습니다. 파트너 센터에서 생성 된 제출에 대 한이 ID의 파트너 센터에서 제출 페이지의 URL을 사용할 수도 있습니다.  |

### <a name="request-body"></a>요청 본문

이 메서드에 대한 요청 본문을 제공하지 않습니다.

### <a name="request-example"></a>요청 예제

다음 예제에서는 앱 제출을 커밋하는 방법을 보여 줍니다.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621243610/commit HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

다음 예제에서는 이 메서드를 성공적으로 호출하기 위한 JSON 응답 본문을 보여 줍니다. 응답 본문의 값에 대한 자세한 내용은 다음 섹션을 참조하세요.

```json
{
  "status": "CommitStarted"
}
```

### <a name="response-body"></a>응답 본문

| 값      | 형식   | 설명                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 상태           | string  | 제출의 상태입니다. 다음 값 중 하나일 수 있습니다. <ul><li>없음</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>릴리스</li><li>ReleaseFailed</li></ul>  |

## <a name="error-codes"></a>오류 코드

요청을 성공적으로 완료할 수 없으면 응답에 다음 HTTP 오류 코드 중 하나가 포함됩니다.

| 오류 코드 |  설명   |
|--------|------------------|
| 400  | 요청 매개 변수가 잘못되었습니다. |
| 404  | 지정된 제출을 찾을 수 없습니다. |
| 409  | 지정 된 전송 찾을 수 있지만 현재 상태의 커밋할 수 또는 파트너 센터 기능을 사용 하는 앱 [현재 Microsoft Store 전송 API에 의해 지원 되지 않습니다](create-and-manage-submissions-using-windows-store-services.md#not_supported)합니다. |

## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용 하 여 서브 미션을 만들고 설정 합니다.](create-and-manage-submissions-using-windows-store-services.md)
* [가져오기는 앱 제출](get-an-app-submission.md)
* [만들기는 앱 제출](create-an-app-submission.md)
* [업데이트는 응용 프로그램 제출](update-an-app-submission.md)
* [삭제는 앱 제출](delete-an-app-submission.md)
* [응용 프로그램 제출의 상태를 가져옵니다.](get-status-for-an-app-submission.md)
