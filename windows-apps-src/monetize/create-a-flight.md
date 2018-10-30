---
author: Xansky
ms.assetid: 8C1E9E36-13AF-4386-9D0F-F9CB320F02F5
description: Microsoft Store 제출 API에서 이 메서드를 사용하여 Windows 개발자 센터 계정에 등록된 앱을 위한 패키지 플라이트를 만듭니다.
title: 패키지 플라이트 만들기
ms.author: mhopkins
ms.date: 04/16/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 제출 API, 플라이트 만들기
ms.localizationpriority: medium
ms.openlocfilehash: 57ad1847e8989cb6aed20024d1c13d36e154d834
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5752178"
---
# <a name="create-a-package-flight"></a>패키지 플라이트 만들기

Microsoft Store 제출 API에서 이 메서드를 사용하여 Windows 개발자 센터 계정에 등록된 앱을 위한 패키지 플라이트를 만듭니다.

> [!NOTE]
> 이 메서드는 제출 없이 패키지 플라이트를 만듭니다. 패키지 플라이트에 대한 제출을 만들려면 [패키지 플라이트 제출 관리](manage-flight-submissions.md)의 메서드를 참조하세요.

## <a name="prerequisites"></a>필수 조건

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 제출 API에 대한 모든 [필수 조건](create-and-manage-submissions-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청

이 메서드에는 다음 구문이 있습니다. 헤더 및 요청 본문의 사용 예제와 설명은 다음 섹션을 참조하세요.

| 메서드 | 요청 URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 이름        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | 필수. 패키지 플라이트를 만들려는 앱의 스토어 ID입니다. 스토어 ID에 대한 자세한 내용은 [앱 ID 세부 정보 보기](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)를 참조하세요.  |


### <a name="request-body"></a>요청 본문

요청 본문에는 다음 매개 변수가 있습니다.

|  매개 변수  |  유형  |  설명  |  필수  |
|------|------|------|------|
|  FriendlyName  |  문자열  |  개발자가 지정한 패키지 플라이트 이름입니다.  |  아니요  |
|  groupIds  |  배열  |  패키지 플라이트와 연결된 플라이트 그룹의 ID가 포함된 문자열의 배열입니다. 플라이트 그룹에 대한 자세한 내용은 [패키지 플라이트](https://msdn.microsoft.com/windows/uwp/publish/package-flights)를 참조하세요.  |  아니요  |
|  rankHigherThan  |  문자열  |  현재 패키지 플라이트보다 순위가 바로 아래인 패키지 플라이트의 식별 이름입니다. 이 매개 변수를 설정하지 않으면 새 패키지 플라이트에 모든 패키지 플라이트 중 가장 높은 순위가 지정됩니다. 플라이트 그룹의 순위 지정에 대한 자세한 내용은 [패키지 플라이트](https://msdn.microsoft.com/windows/uwp/publish/package-flights)를 참조하세요.    |  아니요  |


### <a name="request-example"></a>요청 예제

다음 예제에서는 스토어 ID가 9WZDNCRD911W인 앱의 새 패키지 플라이트를 만드는 방법을 보여 줍니다.

```syntax
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1Q...
Content-Type: application/json
{
  "friendlyName": "myflight",
  "groupIds": [
    0
  ],
  "rankHigherThan": null
}

```

## <a name="response"></a>응답

다음 예제에서는 이 메서드를 성공적으로 호출하기 위한 JSON 응답 본문을 보여 줍니다. 응답 본문의 값에 대한 자세한 내용은 다음 섹션을 참조하세요.

```json
{
  "flightId": "43e448df-97c9-4a43-a0bc-2a445e736bcd",
  "friendlyName": "myflight",
  "groupIds": [
    "0"
  ],
  "rankHigherThan": "671c2857-725e-4faf-9e9e-ea1191ef879c"
}
```

### <a name="response-body"></a>응답 본문

| 값      | 유형   | 설명                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightId            | 문자열  | 패키지 플라이트의 ID입니다. 이 값은 개발자 센터에서 제공됩니다.  |
| FriendlyName           | 문자열  | 요청에 지정된 대로의 패키지 플라이트 이름입니다.   |  
| groupIds           | 배열  | 요청에 지정된 대로 패키지 플라이트와 연결된 플라이트 그룹의 ID를 포함하는 문자열의 배열입니다. 플라이트 그룹에 대한 자세한 내용은 [패키지 플라이트](https://msdn.microsoft.com/windows/uwp/publish/package-flights)를 참조하세요.   |
| rankHigherThan           | 문자열  | 요청에 지정된 대로 현재 패키지 플라이트보다 순위가 바로 아래인 패키지 플라이트의 식별 이름입니다. 플라이트 그룹의 순위 지정에 대한 자세한 내용은 [패키지 플라이트](https://msdn.microsoft.com/windows/uwp/publish/package-flights)를 참조하세요.  |


## <a name="error-codes"></a>오류 코드

요청을 성공적으로 완료할 수 없으면 응답에 다음 HTTP 오류 코드 중 하나가 포함됩니다.

| 오류 코드 |  설명   |
|--------|------------------|
| 400  | 요청이 잘못되었습니다. |
| 409  | 현재 상태 때문에 패키지 플라이트를 만들 수 없거나 앱이 [현재 Microsoft Store 제출 API에서 지원되지 않는](create-and-manage-submissions-using-windows-store-services.md#not_supported) 개발자 센터 대시보드 기능을 사용합니다. |   


## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용하여 제출 만들기 및 관리](create-and-manage-submissions-using-windows-store-services.md)
* [패키지 플라이트 가져오기](get-a-flight.md)
* [패키지 플라이트 삭제](delete-a-flight.md)
