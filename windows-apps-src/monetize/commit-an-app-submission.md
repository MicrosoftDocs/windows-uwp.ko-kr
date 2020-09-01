---
ms.assetid: 934F2DBF-2C7E-4B77-997D-17B9B0535D51
description: Microsoft Store 제출 API에서이 메서드를 사용 하 여 신규 또는 업데이트 된 앱 제출을 파트너 센터에 커밋합니다.
title: 앱 제출 커밋
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 커밋 앱 제출
ms.localizationpriority: medium
ms.openlocfilehash: 83d9527b93e60af144b8c38d98a4f2e4af7852d9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171667"
---
# <a name="commit-an-app-submission"></a>앱 제출 커밋

Microsoft Store 제출 API에서이 메서드를 사용 하 여 신규 또는 업데이트 된 앱 제출을 파트너 센터에 커밋합니다. 커밋 작업은 전송 데이터가 업로드 된 (관련 패키지 및 이미지 포함) 파트너 센터에 알립니다. 이에 대 한 응답으로 파트너 센터는 수집 및 게시를 위해 제출 데이터에 대 한 변경 내용을 커밋합니다. 커밋 작업이 성공 하면 제출에 대 한 변경 내용이 파트너 센터에 표시 됩니다.

Microsoft Store 제출 API를 사용 하 여 응용 프로그램을 제출 하는 프로세스에 커밋 작업을 수행 하는 방법에 대 한 자세한 내용은 [앱 전송 관리](manage-app-submissions.md)를 참조 하세요.

## <a name="prerequisites"></a>필수 구성 요소

이 방법을 사용 하려면 먼저 다음을 수행 해야 합니다.

* 아직 수행 하지 않은 경우 Microsoft Store 제출 API에 대 한 모든 [필수 구성 요소](create-and-manage-submissions-using-windows-store-services.md#prerequisites) 를 완료 합니다.
* 이 메서드에 대 한 요청 헤더에 사용할 [AZURE AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) . 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후 새 토큰을 얻을 수 있습니다.
* [앱 제출을 만든](create-an-app-submission.md) 다음 제출 데이터에 대 한 필요한 변경 내용을 사용 하 여 [제출을 업데이트](update-an-app-submission.md) 합니다.

## <a name="request"></a>요청

이 메서드의 구문은 다음과 같습니다. 헤더 및 요청 본문의 사용 예제와 설명은 다음 섹션을 참조하세요.

| 방법 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit` |


### <a name="request-header"></a>요청 헤더

| header        | 유형   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수 요소. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 이름        | 유형   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 문자열 | 필수 요소. 커밋 하려는 제출이 포함 된 앱의 저장소 ID입니다. 저장소 ID에 대 한 자세한 내용은 [앱 id 세부 정보 보기](../publish/view-app-identity-details.md)를 참조 하세요.  |
| submissionId | 문자열 | 필수 요소. 커밋하 려는 전송의 ID입니다. 이 ID는 [앱 제출을 만들기](create-an-app-submission.md)위한 요청에 대 한 응답 데이터에서 사용할 수 있습니다. 파트너 센터에서 만든 제출의 경우이 ID는 파트너 센터의 제출 페이지에 대 한 URL 에서도 사용할 수 있습니다.  |

### <a name="request-body"></a>요청 본문

이 메서드에 대 한 요청 본문을 제공 하지 마십시오.

### <a name="request-example"></a>요청 예제

다음 예제에서는 앱 제출을 커밋하는 방법을 보여 줍니다.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621243610/commit HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

다음 예제에서는이 메서드를 성공적으로 호출 하기 위한 JSON 응답 본문을 보여 줍니다. 응답 본문의 값에 대 한 자세한 내용은 다음 섹션을 참조 하십시오.

```json
{
  "status": "CommitStarted"
}
```

### <a name="response-body"></a>응답 본문

| 값      | 형식   | Description                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 상태           | 문자열  | 제출의 상태입니다. 다음 값 중 하나일 수 있습니다. <ul><li>없음</li><li>취소됨</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>게시</li><li>게시 날짜</li><li>이상 실패</li><li>바꾸면</li><li>PreProcessingFailed</li><li>인증</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>  |

## <a name="error-codes"></a>오류 코드

요청이 성공적으로 완료 될 수 없는 경우 응답은 다음 HTTP 오류 코드 중 하나를 포함 합니다.

| 오류 코드 |  Description   |
|--------|------------------|
| 400  | 요청 매개 변수가 잘못 되었습니다. |
| 404  | 지정 된 제출을 찾을 수 없습니다. |
| 409  | 지정 된 제출이 있지만 현재 상태에서 커밋할 수 없거나 앱이 [현재 Microsoft Store 제출 API에서 지원 하지](create-and-manage-submissions-using-windows-store-services.md#not_supported)않는 파트너 센터 기능을 사용 합니다. |

## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용 하 여 제출 작성 및 관리](create-and-manage-submissions-using-windows-store-services.md)
* [앱 제출 가져오기](get-an-app-submission.md)
* [앱 제출 만들기](create-an-app-submission.md)
* [앱 제출 업데이트](update-an-app-submission.md)
* [앱 제출 삭제](delete-an-app-submission.md)
* [앱 제출의 상태를 가져오기](get-status-for-an-app-submission.md)