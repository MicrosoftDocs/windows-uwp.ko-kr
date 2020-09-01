---
description: Microsoft Store analytics API에서이 방법을 사용 하 여 데스크톱 응용 프로그램에서 오류에 대 한 CAB 파일을 다운로드 합니다.
title: 데스크톱 애플리케이션의 오류에 대한 CAB 파일 다운로드
ms.date: 03/06/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 분석 API, 다운로드 CAB, 데스크톱 응용 프로그램
ms.localizationpriority: medium
ms.openlocfilehash: 98a0d6931a59e037bb15679a20fae11eb82dae32
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162497"
---
# <a name="download-the-cab-file-for-an-error-in-your-desktop-application"></a>데스크톱 애플리케이션의 오류에 대한 CAB 파일 다운로드

Microsoft Store analytics API에서이 메서드를 사용 하 여 [Windows 데스크톱 응용](/windows/desktop/appxpkg/windows-desktop-application-program)프로그램에 추가한 데스크톱 응용 프로그램에 대 한 특정 오류와 관련 된 CAB 파일을 다운로드 합니다. 이 메서드는 지난 30 일 동안 발생 한 앱 오류에 대 한 CAB 파일만 다운로드할 수 있습니다. CAB 파일 다운로드는 파트너 센터의 데스크톱 응용 프로그램에 대 한 [상태 보고서](/windows/desktop/appxpkg/windows-desktop-application-program) 에서도 사용할 수 있습니다.

이 메서드를 사용 하려면 먼저 [데스크톱 응용 프로그램 메서드에서 오류에 대 한 세부 정보 가져오기](get-details-for-an-error-in-your-desktop-application.md) 를 사용 하 여 다운로드할 CAB 파일의 ID 해시를 검색 해야 합니다.

## <a name="prerequisites"></a>필수 구성 요소


이 방법을 사용 하려면 먼저 다음을 수행 해야 합니다.

* 아직 수행 하지 않은 경우 Microsoft Store 분석 API에 대 한 모든 [필수 구성 요소](access-analytics-data-using-windows-store-services.md#prerequisites) 를 완료 합니다.
* 이 메서드에 대 한 요청 헤더에 사용할 [AZURE AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) . 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료 된 후 새 토큰을 얻을 수 있습니다.
* 다운로드 하려는 CAB 파일의 ID 해시를 가져옵니다. 이 값을 얻으려면 [데스크톱 응용 프로그램에서 오류에 대 한 세부 정보 가져오기](get-details-for-an-error-in-your-desktop-application.md) 메서드를 사용 하 여 앱의 특정 오류에 대 한 세부 정보를 검색 하 고 해당 메서드의 응답 본문에서 **cabidhash** 값을 사용 합니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 방법 | 요청 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/cabdownload``` |


### <a name="request-header"></a>요청 헤더

| header        | 유형   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수 요소. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | 형식   |  Description      |  필수  |
|---------------|--------|---------------|------|
| applicationId | 문자열 | CAB 파일을 다운로드 하려는 데스크톱 응용 프로그램의 제품 ID입니다. 데스크톱 응용 프로그램의 제품 ID를 가져오려면 데스크톱 응용 프로그램 (예: **상태 보고서**) [에 대 한 파트너 센터 분석 보고서](/windows/desktop/appxpkg/windows-desktop-application-program) 를 열고 URL에서 제품 id를 검색 합니다. |  예  |
| cabIdHash | 문자열 | 다운로드 하려는 CAB 파일의 고유 ID 해시입니다. 이 값을 얻으려면 [데스크톱 응용 프로그램에서 오류에 대 한 세부 정보 가져오기](get-details-for-an-error-in-your-desktop-application.md) 메서드를 사용 하 여 응용 프로그램의 특정 오류에 대 한 세부 정보를 검색 하 고 해당 메서드의 응답 본문에서 **cabidhash** 값을 사용 합니다. |  예  |


### <a name="request-example"></a>요청 예제

다음 예제에서는이 메서드를 사용 하 여 CAB 파일을 다운로드 하는 방법을 보여 줍니다. *ApplicationId* 및 *cabidhash* 매개 변수를 데스크톱 응용 프로그램에 대 한 적절 한 값으로 바꿉니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/cabdownload?applicationId=10238467886765136388&cabIdHash=54ffb83a-e159-41d2-8158-f36f306cc01e HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

이 메서드는 302 (리디렉션) 응답 코드를 반환 하 고 응답의 **위치** 헤더는 CAB 파일의 SAS (공유 액세스 서명) URI에 할당 됩니다. CAB 파일을 자동으로 다운로드 하기 위해 호출자가이 URI로 리디렉션됩니다.

## <a name="related-topics"></a>관련 항목

* [상태 보고서](../publish/health-report.md)
* [Microsoft Store 서비스를 사용 하 여 분석 데이터 액세스](access-analytics-data-using-windows-store-services.md)
* [데스크톱 애플리케이션에 대한 오류 보고 데이터 가져오기](get-desktop-application-error-reporting-data.md)
* [데스크톱 애플리케이션에서 오류 세부 정보 가져오기](get-details-for-an-error-in-your-desktop-application.md)
* [데스크톱 애플리케이션에서 오류에 대한 스택 추적 가져오기](get-the-stack-trace-for-an-error-in-your-desktop-application.md)