---
author: mcleanbyron
description: "Windows 스토어 제출 API에서 이 메서드를 사용하여 패키지 플라이트 제출에 대한 패키지 출시 정보를 받습니다."
title: "Windows 스토어 제출 API를 사용하여 패키지 플라이트 제출에 대한 패키지 출시 정보 가져오기"
translationtype: Human Translation
ms.sourcegitcommit: 9b76a11adfab838b21713cb384cdf31eada3286e
ms.openlocfilehash: 72dcad2c6f278e81c8f4fbbc7353c1aada123862

---

# Windows 스토어 제출 API를 사용하여 패키지 플라이트 제출에 대한 패키지 출시 정보 가져오기


Windows 스토어 제출 API에서 이 메서드를 사용하여 패키지 플라이트 제출에 대한 [패키지 출시](../publish/gradual-package-rollout.md) 정보를 받습니다. Windows 스토어 제출 API를 사용하여 패키지 플라이트 제출을 만드는 프로세스의 절차에 대한 자세한 내용은 [패키지 플라이트 제출 관리](manage-flight-submissions.md)를 참조하세요.

## 필수 조건

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Windows 스토어 제출 API에 대한 모든 [필수 조건](create-and-manage-submissions-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.
* 개발자 센터 계정의 앱에 대한 패키지 플라이트 제출을 만듭니다. 이 작업은 개발자 센터 대시보드에서 수행하거나 [패키지 플라이트 제출 만들기](create-a-flight-submission.md) 메서드를 사용하여 수행할 수 있습니다.

>**참고**&nbsp;&nbsp;이 메서드는 Windows 스토어 제출 API를 사용할 수 있는 권한이 부여된 Windows 개발자 센터 계정에만 사용할 수 있습니다. 일부 계정은 이 권한을 사용할 수 없습니다.

## 요청

이 메서드에는 다음 구문이 있습니다. 헤더 및 요청 매개 변수의 사용 예제와 설명은 다음 섹션을 참조하세요.

| 메서드 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| GET   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/packagerollout   ``` |

<span/>
 

### 요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |

<span/>

### 요청 매개 변수

| 이름        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 문자열 | 필수. 가져올 패키지 출시 정보가 있는 패키지 플라이트 제출을 포함하는 앱의 스토어 ID입니다. 스토어 ID에 대한 자세한 내용은 [앱 ID 세부 정보 보기](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)를 참조하세요.  |
| flightId | 문자열 | 필수. 가져올 패키지 출시 정보가 있는 제출을 포함하는 패키지 플라이트의 ID입니다. 이 ID는 개발자 센터 대시보드에서 사용할 수 있으며 [패키지 플라이트 만들기](create-a-flight.md) 및 [앱의 패키지 플라이트 가져오기](get-flights-for-an-app.md) 요청에 대한 응답 데이터에 포함되어 있습니다.  |
| submissionId | 문자열 | 필수. 가져올 패키지 출시 정보가 있는 제출 ID입니다. 이 ID는 개발자 센터 대시보드에서 사용할 수 있으며 [패키지 플라이트 제출 만들기](create-a-flight-submission.md) 요청에 대한 응답 데이터에 포함되어 있습니다.  |

<span/>

### 요청 본문

이 메서드에 대한 요청 본문을 제공하지 않습니다.

### 요청 예제

다음 예제에서는 패키지 플라이트 제출에 대한 패키지 출시 정보를 가져오는 방법을 보여 줍니다.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649/packagerollout HTTP/1.1
Authorization: Bearer <your access token>
```

## 응답

다음 예제에서는 패키지 출시를 점진적으로 사용하도록 설정한 패키지 플라이트 제출에 대해 이 메서드를 성공적으로 호출하기 위한 JSON 응답 본문을 보여 줍니다. 응답 본문의 값에 대한 자세한 내용은 [패키지 출시 리소스](manage-flight-submissions.md#package-rollout-object)를 참조하세요.

```json
{
    "isPackageRollout": true,
    "packageRolloutPercentage": 25,
    "packageRolloutStatus": "PackageRolloutInProgress",
    "fallbackSubmissionId": "1212922684621243058"
}
```

패키지 플라이트 제출에서 점진적인 패키지 출시를 사용할 수 없는 경우 다음 응답 본문이 반환됩니다.

```json
{
    "isPackageRollout": false,
    "packageRolloutPercentage": 0,
    "packageRolloutStatus": "PackageRolloutNotStarted",
    "fallbackSubmissionId": "0"
}
```

## 오류 코드

요청을 성공적으로 완료할 수 없으면 응답에 다음 HTTP 오류 코드 중 하나가 포함됩니다.

| 오류 코드 |  설명   |
|--------|------------------|
| 404  | 패키지 플라이트 제출을 찾을 수 없습니다. |
| 409  | 패키지 플라이트 제출이 지정된 패키지 플라이트에 속하지 않거나 앱이 [현재 Windows 스토어 제출 API에서 지원되지 않는](create-and-manage-submissions-using-windows-store-services.md#not_supported) 개발자 센터 대시보드 기능을 사용합니다. |   

<span/>


## 관련 항목

* [점진적 패키지 배포](../publish/gradual-package-rollout.md)
* [Windows 스토어 제출 API를 사용하여 패키지 플라이트 제출 관리](manage-flight-submissions.md)
* [Windows 스토어 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)



<!--HONumber=Nov16_HO1-->

