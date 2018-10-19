---
author: Xansky
ms.assetid: AC74B4FA-5554-4C03-9683-86EE48546C05
description: Microsoft Store 제출 API에서 이 메서드를 사용하여 Windows 개발자 센터에 새롭거나 업데이트된 추가 기능 제출을 커밋합니다.
title: 추가 기능 제출 커밋
ms.author: mhopkins
ms.date: 04/17/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store 제출 API, 추가 기능 제출 커밋, 앱에서 바로 구매 제품, IAP
ms.localizationpriority: medium
ms.openlocfilehash: b9d20bffe2be163db568af0b16bdfef8cd600271
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/19/2018
ms.locfileid: "4957648"
---
# <a name="commit-an-add-on-submission"></a>추가 기능 제출 커밋

Microsoft Store 제출 API에서 이 메서드를 사용하여 Windows 개발자 센터에 새롭거나 업데이트된 추가 기능(앱에서 바로 구매 제품 또는 IAP라고도 함) 제출을 커밋합니다. 커밋 작업은 제출 데이터가 업로드되었음(모든 관련된 아이콘 포함)을 개발자 센터에 알려줍니다. 응답에서 개발자 센터는 수집 및 게시를 위해 제출 데이터에 대한 변경 사항을 커밋합니다. 커밋 작업이 성공하면 개발자 센터 대시보드에 제출의 변경 사항이 표시됩니다.

커밋 작업이 Microsoft Store 제출 API를 사용하여 추가 기능을 제출하는 프로세스에 적용되는 방법은 [추가 기능 제출 관리](manage-add-on-submissions.md)를 참조하세요.

## <a name="prerequisites"></a>필수 조건

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 제출 API에 대한 모든 [필수 조건](create-and-manage-submissions-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.
* [추가 기능 제출을 만든](create-an-add-on-submission.md) 다음 제출 데이터에 필요한 모든 변경 사항으로 [제출을 업데이트](update-an-add-on-submission.md)합니다.

## <a name="request"></a>요청

이 메서드에는 다음 구문이 있습니다. 헤더 및 요청 본문의 사용 예제와 설명은 다음 섹션을 참조하세요.

| 메서드 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId}/commit``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 이름        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | string | 필수. 커밋하려는 제출이 포함된 추가 기능의 스토어 ID입니다. 이 스토어 ID는 개발자 센터 대시보드에서 사용할 수 있으며 [모든 추가 기능 가져오기](get-all-add-ons.md) 및 [추가 기능 만들기](create-an-add-on.md) 요청에 대한 응답 데이터에 포함되어 있습니다. |
| submissionId | string | 필수. 커밋하려는 제출의 ID입니다. 이 ID는 [추가 기능 제출 만들기](create-an-add-on-submission.md) 요청에 대한 응답 데이터에서 사용할 수 있습니다. 개발자 센터 대시보드에서 만든 제출의 경우 이 ID는 대시보드에 있는 제출 페이지의 URL에도 사용할 수 있습니다.  |


### <a name="request-body"></a>요청 본문

이 메서드에 대한 요청 본문을 제공하지 않습니다.

### <a name="request-example"></a>요청 예제

다음 예제에서는 추가 기능 제출을 커밋하는 방법을 보여 줍니다.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions/1152921504621230023/commit HTTP/1.1
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

| 값      | 유형   | 설명                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| status           | 문자열  | 제출의 상태입니다. 다음 값 중 하나일 수 있습니다. <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>릴리스</li><li>ReleaseFailed</li></ul>  |


## <a name="error-codes"></a>오류 코드

요청을 성공적으로 완료할 수 없으면 응답에 다음 HTTP 오류 코드 중 하나가 포함됩니다.

| 오류 코드 |  설명   |
|--------|------------------|
| 400  | 요청 매개 변수가 잘못되었습니다. |
| 404  | 지정된 제출을 찾을 수 없습니다. |
| 409  | 지정된 제출을 찾았지만 현재 상태로 커밋할 수 없거나 추가 기능이 [현재 Microsoft Store 제출 API에서 지원되지 않는](create-and-manage-submissions-using-windows-store-services.md#not_supported) 개발자 센터 대시보드 기능을 사용합니다. |


## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)
* [추가 기능 제출 가져오기](get-an-add-on-submission.md)
* [추가 기능 제출 만들기](create-an-add-on-submission.md)
* [추가 기능 제출 업데이트](update-an-add-on-submission.md)
* [추가 기능 제출 삭제](delete-an-add-on-submission.md)
* [추가 기능 제출 상태 가져오기](get-status-for-an-add-on-submission.md)
