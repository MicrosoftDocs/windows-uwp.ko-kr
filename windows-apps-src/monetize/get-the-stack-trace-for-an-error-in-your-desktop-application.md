---
description: Microsoft Store analytics API에서이 메서드를 사용 하 여 데스크톱 응용 프로그램에서 오류에 대 한 스택 추적을 가져옵니다.
title: 데스크톱 애플리케이션에서 오류에 대한 스택 추적 가져오기
ms.date: 06/05/2018
ms.topic: article
keywords: windows 10, uwp, 저장소 서비스, Microsoft Store 분석 API, 스택 추적, 오류, 데스크톱 응용 프로그램
ms.localizationpriority: medium
ms.openlocfilehash: 319323ffbd8ed9aecda29cb012a47476f74c7811
ms.sourcegitcommit: b0a82c2a132212eb5fb72b67f0789cac1014642f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254178"
---
# <a name="get-the-stack-trace-for-an-error-in-your-desktop-application"></a>데스크톱 애플리케이션에서 오류에 대한 스택 추적 가져오기

Microsoft Store analytics API에서이 메서드를 사용 하 여 [Windows 데스크톱 응용](/windows/desktop/appxpkg/windows-desktop-application-program)프로그램에 추가한 데스크톱 응용 프로그램에서 오류에 대 한 스택 추적을 가져옵니다. 이 메서드는 지난 30 일 동안 발생 한 오류에 대 한 스택 추적만 다운로드할 수 있습니다. 스택 추적은 파트너 센터의 데스크톱 응용 프로그램에 대 한 [상태 보고서](/windows/desktop/appxpkg/windows-desktop-application-program) 에서도 사용할 수 있습니다.

이 메서드를 사용 하려면 먼저 [데스크톱 응용 프로그램 메서드에서 오류에 대 한 세부 정보 가져오기](get-details-for-an-error-in-your-desktop-application.md) 를 사용 하 여 스택 추적을 검색 하려는 오류와 연결 된 CAB 파일의 ID 해시를 검색 해야 합니다.

## <a name="prerequisites"></a>사전 요구 사항

이 방법을 사용 하려면 먼저 다음을 수행 해야 합니다.

* 아직 수행 하지 않은 경우 Microsoft Store 분석 API에 대 한 모든 [필수 구성 요소](access-analytics-data-using-windows-store-services.md#prerequisites) 를 완료 합니다.
* 이 메서드에 대 한 요청 헤더에 사용할 [AZURE AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) . 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후 새 토큰을 얻을 수 있습니다.
* 스택 추적을 검색 하려는 오류와 연결 된 CAB 파일의 ID 해시를 가져옵니다. 이 값을 얻으려면 [데스크톱 응용 프로그램에서 오류에 대 한 세부 정보 가져오기](get-details-for-an-error-in-your-desktop-application.md) 메서드를 사용 하 여 앱의 특정 오류에 대 한 세부 정보를 검색 하 고 해당 메서드의 응답 본문에서 **cabidhash** 값을 사용 합니다.

## <a name="request"></a>요청

### <a name="request-syntax"></a>요청 구문

| 방법 | 요청 URI                                                                   |
|--------|-------------------------------------------------------------------------------|
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/stacktrace` |

### <a name="request-header"></a>요청 헤더

| 헤더        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수 요소. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |

### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | Type   |  Description      |  필수  |
|---------------|--------|---------------|------|
| applicationId | 문자열 | 스택 추적을 가져오려는 데스크톱 응용 프로그램의 제품 ID입니다. 데스크톱 응용 프로그램의 제품 ID를 얻으려면 파트너 센터 (예: **상태 보고서**) [에서 데스크톱 응용 프로그램에 대 한 분석 보고서](/windows/desktop/appxpkg/windows-desktop-application-program) 를 열고 URL에서 제품 id를 검색 합니다. |  예  |
| cabIdHash | 문자열 | 스택 추적을 검색 하려는 오류와 연결 된 CAB 파일의 고유 ID 해시입니다. 이 값을 얻으려면 [데스크톱 응용 프로그램에서 오류에 대 한 세부 정보 가져오기](get-details-for-an-error-in-your-desktop-application.md) 메서드를 사용 하 여 응용 프로그램의 특정 오류에 대 한 세부 정보를 검색 하 고 해당 메서드의 응답 본문에서 **cabidhash** 값을 사용 합니다. |  예  |

### <a name="request-example"></a>요청 예제

다음 예제에서는이 메서드를 사용 하 여 스택 추적을 가져오는 방법을 보여 줍니다. *ApplicationId* 및 *cabidhash* 매개 변수를 데스크톱 응용 프로그램에 대 한 적절 한 값으로 바꿉니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/stacktrace?applicationId=10238467886765136388&cabIdHash=54ffb83a-e159-41d2-8158-f36f306cc01e HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

### <a name="response-body"></a>응답 본문

| 값      | Type    | Description                  |
|------------|---------|--------------------------------|
| 값      | array   | 각각 스택 추적 데이터의 프레임 하나를 포함 하는 개체의 배열입니다. 각 개체의 데이터에 대 한 자세한 내용은 아래의 [stack 추적 값](#stack-trace-values) 섹션을 참조 하십시오. |
| @nextLink  | 문자열  | 추가 데이터 페이지가 있는 경우이 문자열에는 다음 데이터 페이지를 요청 하는 데 사용할 수 있는 URI가 포함 됩니다. 예를 들어 요청의 **top** 매개 변수가 10으로 설정 되어 있지만 해당 쿼리에 대해 10 개 이상의 행이 있는 경우이 값이 반환 됩니다. |
| TotalCount | integer | 쿼리의 데이터 결과에 있는 총 행 수입니다.          |

### <a name="stack-trace-values"></a>스택 추적 값

*값* 배열의 요소에는 다음 값이 포함 됩니다.

| 값           | Type    | Description      |
|-----------------|---------|----------------|
| 수준            | 문자열  |  이 요소가 호출 스택에 나타내는 프레임 번호입니다.  |
| 이미지   | 문자열  |   이 스택 프레임에서 호출 되는 함수를 포함 하는 실행 파일 또는 라이브러리 이미지의 이름입니다.           |
| function | 문자열  |  이 스택 프레임에서 호출 되는 함수의 이름입니다. 앱에 실행 파일이 나 라이브러리에 대 한 기호가 포함 된 경우에만 사용할 수 있습니다.              |
| offset     | 문자열  |  함수의 시작 부분을 기준으로 하는 현재 명령의 바이트 오프셋입니다.      |

### <a name="response-example"></a>응답 예제

다음 예제에서는이 요청에 대 한 예제 JSON 응답 본문을 보여 줍니다.

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
* [Microsoft Store 서비스를 사용 하 여 분석 데이터 액세스](access-analytics-data-using-windows-store-services.md)
* [데스크톱 애플리케이션에 대한 오류 보고 데이터 가져오기](get-desktop-application-error-reporting-data.md)
* [데스크톱 애플리케이션에서 오류 세부 정보 가져오기](get-details-for-an-error-in-your-desktop-application.md)
* [데스크톱 애플리케이션의 오류에 대한 CAB 파일 다운로드](download-the-cab-file-for-an-error-in-your-desktop-application.md)