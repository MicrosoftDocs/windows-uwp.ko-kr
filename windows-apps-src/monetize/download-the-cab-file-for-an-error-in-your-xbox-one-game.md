---
description: Microsoft Store 분석 API에서에서이 메서드를 사용 하 여 Xbox One 게임에서 오류에 대 한 CAB 파일을 다운로드 합니다.
title: Xbox One 게임에서 오류에 대 한 CAB 파일 다운로드
ms.date: 11/06/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 분석 API, CAB 다운로드
ms.localizationpriority: medium
ms.openlocfilehash: 736219533a254e6380c10600e97f707f15e37de6
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8883831"
---
# <a name="download-the-cab-file-for-an-error-in-your-xbox-one-game"></a>Xbox One 게임에서 오류에 대 한 CAB 파일 다운로드

Microsoft Store 분석 API에서에서이 메서드를 사용 하는 Xbox 개발자 포털 (XDP)을 통해 수집 된 Xbox One 게임의 특정 오류와 연결 되어 XDP 분석 파트너 센터 대시보드에서 사용할 수 있는 CAB 파일을 다운로드 합니다. 이 메서드는 지난 30 일 동안에서 발생 한 오류에 대 한 CAB 파일만 다운로드할 수 있습니다.

이 메서드를 사용 하려면 먼저 다운로드 하려는 CAB 파일의 ID를 검색할 [Xbox One 게임에서 오류에 대 한 세부 정보 가져오기](get-details-for-an-error-in-your-xbox-one-game.md) 메서드를 먼저 사용 해야 합니다.

## <a name="prerequisites"></a>사전 요구 사항


이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 분석 API에 대한 모든 [필수 조건](access-analytics-data-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.
* 다운로드하려는 CAB 파일의 ID를 가져옵니다. 이 ID를 가져오려면 [Xbox One에서 오류에 대 한 세부 정보를 게임 가져오기](get-details-for-an-error-in-your-xbox-one-game.md) 메서드를 사용 하 여 앱에서 특정 오류에 대 한 세부 정보를 검색 하 고 해당 메서드의 응답 본문에 **cabId** 값을 사용 합니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/cabdownload``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | 유형   |  설명      |  필수  |
|---------------|--------|---------------|------|
| applicationId | string | CAB 파일을 다운로드 하는 Xbox One 게임의 제품 ID입니다. 게임의 제품 ID를 가져오려면 Xbox 개발자 포털(XDP)에서 사용자 게임으로 이동한 후 URL에서 제품 ID를 검색합니다. 또는 Windows 파트너 센터 분석 보고서에서 상태 데이터를 다운로드 하는 경우 제품 ID는.tsv 파일에 포함 됩니다. |  예  |
| cabId | string | 다운로드하려는 CAB 파일의 고유한 ID입니다. 이 ID를 가져오려면 [Xbox One에서 오류에 대 한 세부 정보를 게임 가져오기](get-details-for-an-error-in-your-xbox-one-game.md) 메서드를 사용 하 여 앱에서 특정 오류에 대 한 세부 정보를 검색 하 고 해당 메서드의 응답 본문에 **cabId** 값을 사용 합니다. |  예  |

 
### <a name="request-example"></a>요청 예제

다음 예제에서는 이 메서드를 사용하여 CAB 파일을 다운로드하는 방법을 보여 줍니다. *applicationId* 및 *cabId* 매개변수를 앱의 승인 값으로 교체합니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/cabdownload?applicationId=BRRT4NJ9B3D1&cabId=1336373323853 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

이 메서드는 302(리디렉션) 응답 코드를 반환하고, 이 응답의 **Location** 헤더는 CAB 파일의 SAS(공유 액세스 서명) URI에 할당됩니다. 호출자는 이 URI로 리디렉션되어 자동으로 CAB 파일을 다운로드합니다.

## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용하여 분석 데이터에 액세스](access-analytics-data-using-windows-store-services.md)
* [Xbox One에 대 한 데이터를 보고 하는 오류를 게임 가져오기](get-error-reporting-data-for-your-xbox-one-game.md)
* [게임에서 Xbox One 오류에 대 한 세부 정보를 가져오기](get-details-for-an-error-in-your-xbox-one-game.md)
* [Xbox One에서 오류에 대 한 스택 추적을 게임 가져오기](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md)
