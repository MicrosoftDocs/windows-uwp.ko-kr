---
author: Xansky
description: Microsoft Store 분석 API에서 이 메서드를 사용하여 데스크톱 응용 프로그램의 오류에 대한 CAB 파일을 다운로드합니다.
title: 데스크톱 응용 프로그램의 오류에 대한 CAB 파일 다운로드
ms.author: mhopkins
ms.date: 03/06/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 분석 API, CAB 다운로드, 데스크톱 응용 프로그램
ms.localizationpriority: medium
ms.openlocfilehash: f9dcd76767662b5e40f587d7ac32ffd7d94a6053
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7419837"
---
# <a name="download-the-cab-file-for-an-error-in-your-desktop-application"></a>데스크톱 응용 프로그램의 오류에 대한 CAB 파일 다운로드

Microsoft Store 분석 API에서 이 메서드를 사용하여 [Windows 데스크톱 응용 프로그램 프로그램](https://msdn.microsoft.com/library/windows/desktop/mt826504)에 추가한 데스크톱 응용 프로그램의 특정 오류와 연결된 CAB 파일을 다운로드합니다. 이 메서드는 지난 30일 동안 발생한 앱 오류에 대한 CAB 파일만 다운로드할 수 있습니다. CAB 파일을 다운로드할 데스크톱 응용 프로그램의 파트너 센터에 대 한 [상태 보고서](https://msdn.microsoft.com/library/windows/desktop/mt826504) 에서 사용할 수 있습니다.

이 메서드를 사용하려면 먼저 [데스크톱 응용 프로그램의 오류에 대한 세부 정보 가져오기](get-details-for-an-error-in-your-desktop-application.md) 메서드를 사용하여 다운로드할 CAB 파일의 ID 해시를 검색해야 합니다.

## <a name="prerequisites"></a>필수 조건


이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 분석 API에 대한 모든 [필수 조건](access-analytics-data-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.
* 다운로드하려는 CAB 파일의 ID 해시를 가져옵니다. 이 값을 가져오려면 [데스크톱 응용 프로그램에서 오류에 대한 세부 정보 가져오기](get-details-for-an-error-in-your-desktop-application.md) 메서드를 사용하여 앱에서 특정 오류에 대한 세부 정보를 검색하고 해당 메서드의 응답 본문에 **cabIdHash** 값을 사용합니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/cabdownload``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | 유형   |  설명      |  필수  |
|---------------|--------|---------------|------|
| applicationId | 문자열 | CAB 파일을 다운로드할 데스크톱 응용 프로그램의 제품 ID입니다. 데스크톱 응용 프로그램의 제품 ID를 가져오려면 모든 [데스크톱 응용 프로그램에 대 한 파트너 센터 분석 보고서](https://msdn.microsoft.com/library/windows/desktop/mt826504) (예: **상태 보고서**) 열고 URL에서 제품 ID를 검색 합니다. |  예  |
| cabIdHash | 문자열 | 다운로드하려는 CAB 파일의 고유한 ID 해시입니다. 이 값을 가져오려면 [데스크톱 응용 프로그램에서 오류에 대한 세부 정보 가져오기](get-details-for-an-error-in-your-desktop-application.md) 메서드를 사용하여 응용 프로그램에서 특정 오류에 대한 세부 정보를 검색하고 해당 메서드의 응답 본문에 **cabIdHash** 값을 사용합니다. |  예  |


### <a name="request-example"></a>요청 예제

다음 예제에서는 이 메서드를 사용하여 CAB 파일을 다운로드하는 방법을 보여 줍니다. *applicationId* 및 *cabIdHash* 매개변수를 데스크톱 응용 프로그램에 대한 적절한 값으로 바꿉니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/cabdownload?applicationId=10238467886765136388&cabIdHash=54ffb83a-e159-41d2-8158-f36f306cc01e HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

이 메서드는 302(리디렉션) 응답 코드를 반환하고, 이 응답의 **Location** 헤더는 CAB 파일의 SAS(공유 액세스 서명) URI에 할당됩니다. 호출자는 이 URI로 리디렉션되어 자동으로 CAB 파일을 다운로드합니다.

## <a name="related-topics"></a>관련 항목

* [상태 보고서](../publish/health-report.md)
* [Microsoft Store 서비스를 사용하여 분석 데이터에 액세스](access-analytics-data-using-windows-store-services.md)
* [데스크톱 응용 프로그램에 대한 오류 보고 데이터 가져오기](get-desktop-application-error-reporting-data.md)
* [데스크톱 응용 프로그램에서 오류에 대한 세부 정보 가져오기](get-details-for-an-error-in-your-desktop-application.md)
* [데스크톱 응용 프로그램에서 오류에 대한 스택 추적 가져오기](get-the-stack-trace-for-an-error-in-your-desktop-application.md)
