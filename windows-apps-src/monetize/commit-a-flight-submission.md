---
ms.assetid: F94AF8F6-0742-4A3F-938E-177472F96C00
description: 파트너 센터에 새롭거나 업데이트 된 패키지 비행 제출 커밋 Microsoft Store 제출 API 사용 하 여이 메서드를 사용 합니다.
title: 패키지 플라이트 제출 커밋
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 플라이트 제출 커밋
ms.localizationpriority: medium
ms.openlocfilehash: 0cf1abcb1575252456383fd8fe187962567a6096
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334411"
---
# <a name="commit-a-package-flight-submission"></a>패키지 플라이트 제출 커밋

파트너 센터에 새롭거나 업데이트 된 패키지 비행 제출 커밋 Microsoft Store 제출 API 사용 하 여이 메서드를 사용 합니다. 커밋 작업 경고 파트너 센터 전송 데이터 (모든 관련된 패키지 포함) 업로드 되었습니다. 파트너 센터에 대 한 응답에서 제출 데이터를 수집 하 고 게시의 변경 사항을 커밋합니다. 커밋 작업이 성공한 후 변경 내용은 제출 하려면 파트너 센터에 표시 됩니다.

커밋 작업이 Microsoft Store 제출 API를 사용하여 패키지 플라이트 제출을 만드는 프로세스에 적용되는 방법은 [패키지 플라이트 제출 관리](manage-flight-submissions.md)를 참조하세요.

## <a name="prerequisites"></a>사전 요구 사항

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 제출 API에 대한 모든 [필수 조건](create-and-manage-submissions-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.
* [패키지 플라이트 제출을 만든](create-a-flight-submission.md) 다음 제출 데이터에 필요한 모든 변경 사항으로 [제출을 업데이트](update-a-flight-submission.md)합니다.

## <a name="request"></a>요청

이 메서드에는 다음 구문이 있습니다. 헤더 및 요청 본문의 사용 예제와 설명은 다음 섹션을 참조하세요.

| 메서드 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| 올리기    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit` |

### <a name="request-header"></a>요청 헤더

| 헤더        | 형식   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | 필수. 폼에서 Azure AD 액세스 토큰 **전달자** &lt; *토큰*&gt;합니다. |

### <a name="request-parameters"></a>요청 매개 변수

| 이름        | 형식   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | 필수 사항입니다. 커밋하려는 패키지 플라이트 제출이 포함된 앱의 스토어 ID입니다. 파트너 센터에서 앱에 대 한 Store ID 제공 됩니다.  |
| flightId | string | 필수. 커밋할 제출이 포함된 패키지 플라이트의 ID입니다. 이 ID는 개발자 센터 대시보드에서 사용할 수 있으며 [패키지 플라이트 만들기](create-a-flight.md) 및 [앱의 패키지 플라이트 가져오기](get-flights-for-an-app.md) 요청에 대한 응답 데이터에 포함되어 있습니다. 파트너 센터에서 생성 된 비행이이 ID 파트너 센터에서 비행 페이지의 URL에서 사용할 수 있는 이기도 합니다.  |
| submissionId | string | 필수 사항입니다. 커밋할 제출의 ID입니다. 이 ID는 [패키지 플라이트 제출 만들기](create-a-flight-submission.md) 요청에 대한 응답 데이터에서 사용할 수 있습니다. 파트너 센터에서 생성 된 제출에 대 한이 ID의 파트너 센터에서 제출 페이지의 URL을 사용할 수도 있습니다.  |

### <a name="request-body"></a>요청 본문

이 메서드에 대한 요청 본문을 제공하지 않습니다.

### <a name="request-example"></a>요청 예제

다음 예제에서는 패키지 플라이트 제출을 커밋하는 방법을 보여 줍니다.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649/commit HTTP/1.1
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
* [비행 서브 미션 패키지를 관리 합니다.](manage-flight-submissions.md)
* [가져오기 패키지 비행 제출](get-a-flight-submission.md)
* [비행 제출 패키지 만들기](create-a-flight-submission.md)
* [업데이트 패키지 비행 제출](update-a-flight-submission.md)
* [비행 제출 패키지를 삭제 합니다.](delete-a-flight-submission.md)
* [패키지 비행 제출물의 상태를 가져옵니다.](get-status-for-a-flight-submission.md)
