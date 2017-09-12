---
author: mcleanbyron
ms.assetid: 
description: "Windows 스토어 분석 API에서 이 메서드를 사용하여 앱 오류에 대한 CAB 파일을 다운로드합니다."
title: "앱의 오류에 대한 CAB 파일 다운로드"
ms.author: mcleans
ms.date: 06/16/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, Windows 스토어 분석 API, CAB 다운로드"
ms.openlocfilehash: 6c9e3f024a0b222be89ede5cf81a14bc923b7af4
ms.sourcegitcommit: 7aabd2e59d45bbc5512dd4ddd9110ae62b79d552
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/19/2017
---
# <a name="download-the-cab-file-for-an-error-in-your-app"></a>앱의 오류에 대한 CAB 파일 다운로드

Windows 스토어 분석 API에서 이 메서드를 사용하여, 개발자 센터로 보고된 앱의 특정 오류와 관련된 CAB 파일을 다운로드합니다. 이 메서드는 지난 30일 동안 발생한 앱 오류에 대한 CAB 파일만 다운로드할 수 있습니다. 또한 Windows 개발자 센터 대시보드의 [상태 보고서](../publish/health-report.md)의 **오류** 섹션에서도 CAB 파일을 다운로드할 수 있습니다.

이 메서드를 사용하려면 먼저 [앱의 오류에 대한 세부 정보 가져오기](get-details-for-an-error-in-your-app.md) 메서드를 사용하여 다운로드할 CAB 파일의 ID를 검색해야 합니다.

## <a name="prerequisites"></a>필수 구성 요소


이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Windows 스토어 분석 API에 대한 모든 [필수 조건](access-analytics-data-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.
* 다운로드하려는 CAB 파일의 ID를 가져옵니다. 이 ID를 가져오려면 [앱에서 오류에 대한 세부 정보 가져오기](get-details-for-an-error-in-your-app.md) 메서드를 사용하여 앱에서 특정 오류에 대한 세부 정보를 검색하고 해당 메서드의 응답 본문에 **cabId** 값을 사용합니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/cabdownload``` |

<span/> 

### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |

<span/> 

### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | 유형   |  설명      |  필수  |
|---------------|--------|---------------|------|
| applicationId | string | CAB 파일을 다운로드 하기 원하는 앱의 스토어 ID입니다. 스토어 ID는 개발자 센터 대시보드의 [앱 ID 페이지](../publish/view-app-identity-details.md)에서 확인할 수 있습니다. 스토어 ID의 예로는 9WZDNCRFJ3Q8이 있습니다. |  예  |
| cabId | string | 다운로드하려는 CAB 파일의 고유한 ID입니다. 이 ID를 가져오려면 [앱에서 오류에 대한 세부 정보 가져오기](get-details-for-an-error-in-your-app.md) 메서드를 사용하여 앱에서 특정 오류에 대한 세부 정보를 검색하고 해당 메서드의 응답 본문에 **cabId** 값을 사용합니다. |  예  |

<span/>
 
### <a name="request-example"></a>요청 예제

다음 예제에서는 이 메서드를 사용하여 CAB 파일을 다운로드하는 방법을 보여 줍니다. *applicationId* 및 *cabId* 매개변수를 앱의 승인 값으로 교체합니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/cabdownload?applicationId=9NBLGGGZ5QDR&cabId=1336373323853 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

이 메서드는 302(리디렉션) 응답 코드를 반환하고, 이 응답의 **Location** 헤더는 CAB 파일의 SAS(공유 액세스 서명) URI에 할당됩니다. 호출자는 이 URI로 리디렉션되어 자동으로 CAB 파일을 다운로드합니다.

## <a name="related-topics"></a>관련 항목

* [상태 보고서](../publish/health-report.md)
* [Windows 스토어 서비스를 사용하여 분석 데이터에 액세스](access-analytics-data-using-windows-store-services.md)
* [오류 보고 데이터 가져오기](get-error-reporting-data.md)
* [앱에서 오류에 대한 세부 정보 가져오기](get-details-for-an-error-in-your-app.md)
* [앱에서 오류에 대한 스택 추적 가져오기](get-the-stack-trace-for-an-error-in-your-app.md)
