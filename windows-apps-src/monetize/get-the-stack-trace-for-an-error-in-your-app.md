---
author: Xansky
ms.assetid: b556a245-6359-4ddc-a4bd-76f9873ab694
description: Microsoft Store 분석 API에서 이 메서드를 사용하여 앱의 오류에 대한 스택 추적을 가져올 수 있습니다.
title: 앱에서 오류에 대한 스택 추적 가져오기
ms.author: mhopkins
ms.date: 06/05/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store 서비스, Microsoft Store 분석 API, 스택 추적, 오류
ms.localizationpriority: medium
ms.openlocfilehash: 54c39d157bc2fcab6a67bd273f5d00eedd80be54
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/22/2018
ms.locfileid: "5400808"
---
# <a name="get-the-stack-trace-for-an-error-in-your-app"></a>앱에서 오류에 대한 스택 추적 가져오기

Microsoft Store 분석 API에서 이 메서드를 사용하여 앱의 오류에 대한 스택 추적을 가져올 수 있습니다. 이 메서드는 지난 30일 동안 발생한 앱 오류에 대한 스택 추적만 다운로드할 수 있습니다. 스택 추적은 Windows 개발자 센터 대시보드에서 [상태 보고서](../publish/health-report.md)의 **오류** 섹션을 통해서도 확인할 수 있습니다.

이 메서드를 사용하려면 먼저 [앱에서 오류에 대한 세부 정보 가져오기](get-details-for-an-error-in-your-app.md) 메서드를 통해 스택 추적을 검색하려는 오류와 연결된 CAB 파일의 ID를 검색해야 합니다.

## <a name="prerequisites"></a>필수 조건


이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 분석 API에 대한 모든 [필수 조건](access-analytics-data-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.
* 스택 추적을 검색하려는 오류와 연결된 CAB 파일의 ID를 가져옵니다. 이 ID를 가져오려면 [앱에서 오류에 대한 세부 정보 가져오기](get-details-for-an-error-in-your-app.md) 메서드를 사용하여 앱에서 특정 오류에 대한 세부 정보를 검색하고 해당 메서드의 응답 본문에 **cabId** 값을 사용합니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/stacktrace``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | 형식   |  설명      |  필수  |
|---------------|--------|---------------|------|
| applicationId | 문자열 | 스택 추적을 가져오려는 앱의 스토어 ID입니다. 스토어 ID는 개발자 센터 대시보드의 [앱 ID 페이지](../publish/view-app-identity-details.md)에서 확인할 수 있습니다. 스토어 ID의 예로는 9WZDNCRFJ3Q8이 있습니다. |  예  |
| cabId | 문자열 | 스택 추적을 검색하려는 오류와 연결된 CAB 파일의 고유 ID입니다. 이 ID를 가져오려면 [앱에서 오류에 대한 세부 정보 가져오기](get-details-for-an-error-in-your-app.md) 메서드를 사용하여 앱에서 특정 오류에 대한 세부 정보를 검색하고 해당 메서드의 응답 본문에 **cabId** 값을 사용합니다. |  예  |

 
### <a name="request-example"></a>요청 예제

다음 예제에서는 이 메서드를 사용하여 스택 추적을 가져오는 방법을 보여 줍니다. *applicationId* 값을 앱의 스토어 ID로 바꿉니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/stacktrace?applicationId=9NBLGGGZ5QDR&cabId=1336373323853 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답


### <a name="response-body"></a>응답 본문

| 값      | 유형    | 설명                  |
|------------|---------|--------------------------------|
| 값      | array   | 각각 스택 추적 데이터의 한 프레임을 포함하는 개체 배열입니다. 각 개체의 데이터에 대한 자세한 내용은 아래 [스택 추적 값](#stack-trace-values) 섹션을 참조하세요. |
| @nextLink  | string  | 데이터의 추가 페이지가 있는 경우 이 문자열에는 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 URI가 포함됩니다. 예를 들어 요청의 **top** 매개 변수가 10으로 설정되어 있지만 쿼리에 대한 오류의 행이 10개보다 많은 경우 이 값이 반환됩니다. |
| TotalCount | 정수 | 쿼리에 대한 데이터 결과에 있는 행의 총 수입니다.          |


### <a name="stack-trace-values"></a>스택 추적 값

*값* 배열의 요소에는 다음 값이 포함됩니다.

| 값           | 유형    | 설명      |
|-----------------|---------|----------------|
| level            | 문자열  |  호출 스택에서 이 요소가 나타내는 프레임 번호입니다.  |
| image   | 문자열  |   이 스택 프레임에서 호출되는 함수를 포함하는 실행 파일 또는 라이브러리 이미지의 이름입니다.           |
| function | 문자열  |  이 스택 프레임에서 호출된 함수 이름입니다. 이 함수는 앱에 실행 파일 또는 라이브러리의 기호가 포함된 경우에만 사용할 수 있습니다.              |
| offset     | 문자열  |  함수의 시작 부분을 기준으로 현재 명령의 바이트 오프셋입니다.      |


### <a name="response-example"></a>응답 예제

다음 예제에서는 이 요청에 대한 예제 JSON 응답 본문을 보여 줍니다.

```json
{
  "Value": [
    {
      "level": "0",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.MainPage.DoWork",
      "offset": "0x25C"
    }
    {
      "level": "1",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.MainPage.Initialize",
      "offset": "0x26"
    }
    {
      "level": "2",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.Start",
      "offset": "0x66"
    }
  ],
  "@nextLink": null,
  "TotalCount": 3
}

```

## <a name="related-topics"></a>관련 항목

* [상태 보고서](../publish/health-report.md)
* [Microsoft Store 서비스를 사용하여 분석 데이터에 액세스](access-analytics-data-using-windows-store-services.md)
* [오류 보고 데이터 가져오기](get-error-reporting-data.md)
* [앱에서 오류에 대한 세부 정보 가져오기](get-details-for-an-error-in-your-app.md)
* [앱의 오류에 대한 CAB 파일 다운로드](download-the-cab-file-for-an-error-in-your-app.md)
