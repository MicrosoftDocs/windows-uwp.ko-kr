---
author: mcleanbyron
ms.assetid: 3D6EE7D7-7D75-499D-AA7A-55DA1C485BA6
description: "Windows 스토어 분석 API에서 이 메서드를 사용하여 Windows 10 드라이버 오류에 대한 CAB 파일을 다운로드합니다. 이 메서드는 IHV용으로만 제공됩니다."
title: "Windows 10 드라이버 오류에 대한 CAB 파일 다운로드"
ms.author: mcleans
ms.date: 03/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, Windows 스토어 분석 API, CAB 다운로드"
ms.openlocfilehash: 9a8539a3d9d8f85fd9277165eb393eb372459b0a
ms.sourcegitcommit: 64cfb79fd27b09d49df99e8c9c46792c884593a7
translationtype: HT
---
# <a name="download-the-cab-file-for-a-windows-10-driver-error"></a>Windows 10 드라이버 오류에 대한 CAB 파일 다운로드

Windows 스토어 분석 API에서 이 메서드를 사용하여 특정 Windows 10 드라이버 오류와 관련된 CAB 파일을 다운로드합니다. 이 메서드를 사용하려면 먼저 [Windows 10 드라이버 오류에 대한 세부 정보 가져오기](get-details-for-a-windows-10-driver-error.md) 메서드를 사용하여 다운로드하려는 CAB 파일의 ID를 검색해야 합니다.

Windows 스토어 분석 API에서 [Windows 10 드라이버에 대한 오류 보고 데이터 가져오기](get-error-reporting-data-for-windows-10-drivers.md) 및 [Windows 10 드라이버 오류에 대한 세부 정보 가져오기](get-details-for-a-windows-10-driver-error.md) 메서드를 사용하여 OEM 하드웨어 오류에 대한 다른 정보를 가져올 수 있습니다.

> [!NOTE]
> 이 메서드는 [Windows 하드웨어 개발자 센터 프로그램](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard)에 속하는 개발자 계정으로만 사용할 수 있습니다.

## <a name="prerequisites"></a>필수 조건

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Windows 스토어 분석 API에 대한 모든 [필수 조건](access-analytics-data-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.
* 다운로드하려는 CAB 파일의 ID를 가져옵니다. 이 ID를 가져오려면 [Windows 10 드라이버 오류에 대한 세부 정보 가져오기](get-details-for-a-windows-10-driver-error.md) 메서드를 사용하여 특정 드라이버 오류에 대한 세부 정보를 검색하고 해당 메서드의 응답 본문에 **cabIdHash** 값을 사용합니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/driver/cabdownload``` |

<span/> 

### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |

<span/> 

### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | 유형   |  설명      |  필수  |
|---------------|--------|---------------|------|
| cabIdHash | string | 다운로드하려는 CAB 파일의 고유한 ID입니다. 이 ID를 가져오려면 [Windows 10 드라이버 오류에 대한 세부 정보 가져오기](get-details-for-a-windows-10-driver-error.md) 메서드를 사용하여 앱에서 특정 오류에 대한 세부 정보를 검색하고 해당 메서드의 응답 본문에 **cabIdHash** 값을 사용합니다. |  예  |

<span/>
 
### <a name="request-example"></a>요청 예제

다음 예제에서는 이 메서드를 사용하여 CAB 파일을 다운로드하는 방법을 보여 줍니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/driver/cabdownload?cabIdHash=c1a51104-d682-4230-be14-7278b18e3555 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답

이 메서드는 302(리디렉션) 응답 코드를 반환하고, 이 응답의 **Location** 헤더는 CAB 파일의 SAS(공유 액세스 서명) URI에 할당됩니다. 호출자는 이 URI로 리디렉션되어 자동으로 CAB 파일을 다운로드합니다.

## <a name="related-topics"></a>관련 항목

* [Windows 10 드라이버에 대한 오류 보고 데이터 가져오기](get-error-reporting-data-for-windows-10-drivers.md)
* [Windows 10 드라이버 오류에 대한 세부 정보 가져오기](get-details-for-a-windows-10-driver-error.md)