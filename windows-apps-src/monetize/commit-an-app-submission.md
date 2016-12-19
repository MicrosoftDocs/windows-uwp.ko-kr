---
author: mcleanbyron
ms.assetid: 934F2DBF-2C7E-4B77-997D-17B9B0535D51
description: "Windows 스토어 제출 API에서 이 메서드를 사용하여 Windows 개발자 센터에 새롭거나 업데이트된 앱 제출을 커밋합니다."
title: "Windows 스토어 제출 API를 사용하여 앱 제출 커밋"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 14698cfcd57d71682c40a8820a8a3d2c43dd3567

---

# Windows 스토어 제출 API를 사용하여 앱 제출 커밋


Windows 스토어 제출 API에서 이 메서드를 사용하여 Windows 개발자 센터에 새롭거나 업데이트된 앱 제출을 커밋합니다. 커밋 작업은 제출 데이터가 업로드되었음(모든 관련된 패키지 및 이미지 포함)을 개발자 센터에 알려줍니다. 응답에서 개발자 센터는 수집 및 게시를 위해 제출 데이터에 대한 변경 사항을 커밋합니다. 커밋 작업이 성공하면 개발자 센터 대시보드에 제출의 변경 사항이 표시됩니다.

커밋 작업이 Windows 스토어 제출 API를 사용하여 앱을 제출하는 프로세스에 적용되는 방법은 [앱 제출 관리](manage-app-submissions.md)를 참조하세요.

## 필수 조건

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Windows 스토어 제출 API에 대한 모든 [필수 조건](create-and-manage-submissions-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.
* [앱 제출을 만든](create-an-app-submission.md) 다음 제출 데이터에 필요한 모든 변경 사항으로 [제출을 업데이트](update-an-app-submission.md)합니다.

>**참고**  이 메서드는 Windows 스토어 제출 API를 사용할 수 있는 권한이 부여된 Windows 개발자 센터 계정에만 사용할 수 있습니다. 일부 계정은 이 권한을 사용할 수 없습니다.

## 요청

이 메서드에는 다음 구문이 있습니다. 헤더 및 요청 본문의 사용 예제와 설명은 다음 섹션을 참조하세요.

| 메서드 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit``` |

<span/>
 

### 요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |

<span/>

### 요청 매개 변수

| 이름        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 문자열 | 필수. 커밋하려는 제출이 포함된 앱의 스토어 ID입니다. 스토어 ID에 대한 자세한 내용은 [앱 ID 세부 정보 보기](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)를 참조하세요.  |
| submissionId | 문자열 | 필수. 커밋하려는 제출의 ID입니다. 이 ID는 개발자 센터 대시보드에서 사용할 수 있으며 [앱 제출 만들기](create-an-app-submission.md) 요청에 대한 응답 데이터에 포함되어 있습니다.  |

<span/>

### 요청 본문

이 메서드에 대한 요청 본문을 제공하지 않습니다.

### 요청 예제

다음 예제에서는 앱 제출을 커밋하는 방법을 보여 줍니다.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621243610/commit HTTP/1.1
Authorization: Bearer <your access token>
```

## 응답

다음 예제에서는 이 메서드를 성공적으로 호출하기 위한 JSON 응답 본문을 보여 줍니다. 응답 본문의 값에 대한 자세한 내용은 다음 섹션을 참조하세요.

```json
{
  "status": "CommitStarted"
}
```

### 응답 본문

| 값      | 유형   | 설명                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| status           | 문자열  | 제출의 상태입니다. 다음 값 중 하나일 수 있습니다. <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>  |

<span/>

## 오류 코드

요청을 성공적으로 완료할 수 없으면 응답에 다음 HTTP 오류 코드 중 하나가 포함됩니다.

| 오류 코드 |  설명   |
|--------|------------------|
| 400  | 요청 매개 변수가 잘못되었습니다. |
| 404  | 지정된 제출을 찾을 수 없습니다. |
| 409  | 지정된 제출을 찾았지만 현재 상태로 커밋할 수 없거나 앱이 [현재 Windows 스토어 제출 API에서 지원되지 않는](create-and-manage-submissions-using-windows-store-services.md#not_supported) 개발자 센터 대시보드 기능을 사용합니다. |

<span/>


## 관련 항목

* [Windows 스토어 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)
* [앱 제출 가져오기](get-an-app-submission.md)
* [앱 제출 만들기](create-an-app-submission.md)
* [앱 제출 업데이트](update-an-app-submission.md)
* [앱 제출 삭제](delete-an-app-submission.md)
* [앱 제출의 상태를 가져오기](get-status-for-an-app-submission.md)



<!--HONumber=Aug16_HO5-->


