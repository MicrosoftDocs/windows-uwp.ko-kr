---
author: mcleanbyron
ms.assetid: 2F30E68B-B643-4387-9430-793D08AAF0E7
description: "Windows 스토어 분석 API에서 이 메서드를 사용하여 지정된 날짜 범위 및 다른 선택적 필터에 대한 Windows 7 및 Windows 8.x 드라이버의 집계 오류 보고 데이터를 가져옵니다. 이 메서드는 IHV용으로만 제공됩니다."
title: "Windows 7 및 Windows 8.x 드라이버에 대한 오류 보고 데이터 가져오기"
ms.author: mcleans
ms.date: 03/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 스토어 서비스, Windows 스토어 분석 API, 오류"
ms.openlocfilehash: bac99a46ccf890437216c60eaf406299e9172d30
ms.sourcegitcommit: 64cfb79fd27b09d49df99e8c9c46792c884593a7
translationtype: HT
---
# <a name="get-error-reporting-data-for-windows-7-and-windows-8x-drivers"></a>Windows 7 및 Windows 8.x 드라이버에 대한 오류 보고 데이터 가져오기

Windows 스토어 분석 API에서 이 메서드를 사용하여 지정된 날짜 범위 및 다른 선택적 필터에 대한 Windows 7/Windows 8.x 드라이버 오류의 집계 보고 데이터를 가져옵니다. [Windows 7 또는 Windows 8.x 드라이버 오류에 대한 세부 정보 가져오기](get-details-for-a-windows-7-or-windows-8.x-driver-error.md) 메서드를 사용하여 추가 오류 정보를 검색할 수 있습니다.

> [!NOTE]
> 이 메서드는 [Windows 하드웨어 개발자 센터 프로그램](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard)에 속하는 개발자 계정으로만 사용할 수 있습니다.

## <a name="prerequisites"></a>필수 조건

이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Windows 스토어 분석 API에 대한 모든 [필수 조건](access-analytics-data-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/ihvdriver/failurehits``` |

<span/> 

### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |

<span/> 

### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | 유형   |  설명      |  필수  
|---------------|--------|---------------|------|
| startDate | date | 검색할 오류 보고 데이터의 날짜 범위에 대한 시작 날짜입니다. 기본값은 현재 날짜입니다. |  아니요  |
| endDate | date | 검색할 오류 보고 데이터의 날짜 범위에 대한 종료 날짜입니다. 기본값은 현재 날짜입니다. |  아니요  |
| top | int | 요청에서 반환할 데이터의 행의 수입니다. 지정되지 않은 경우 최대값 및 기본값은 10000입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 다음 링크가 포함되어 있습니다. |  아니요  |
| skip | int | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용하여 큰 데이터 집합의 페이지를 탐색합니다. 예를 들어 top=10000 및 skip=0이면 데이터의 처음 10000개 행을 검색하고 top=10000 및 skip=10000이면 데이터의 다음 10000개 행을 검색하는 방식입니다. |  아니요  |
| filter | 문자열  | 응답에서 행을 필터링하는 하나 이상의 문입니다. 각 문에는 응답 본문의 필드 이름 및 **eq** 또는 **ne** 연산자와 연결된 값이 포함되어 있으며 문은 **and** 또는 **or**를 사용하여 결합될 수 있습니다. 문자열 값은 *filter* 매개 변수에서 단일 따옴표로 묶여야 합니다. 응답 본문의 다음과 같은 필드를 지정할 수 있습니다.<p/><ul><li><strong>date</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>OSVersion</strong></li></li><li><strong>eventType</strong></li><li><strong>출시</strong></li><li><strong>deviceType</strong></li><li><strong>driverName</strong></li><li><strong>driverVersion</strong></li><li><strong>oemName</strong></li><li><strong>oemModel</strong></li><li><strong>flightRing</strong></li><li><strong>architecture</strong></li></ul> | 아니요   |
| aggregationLevel | 문자열 | 집계 데이터를 검색할 시간 범위를 지정합니다. <strong>day</strong>, <strong>week</strong> 또는 <strong>month</strong> 문자열 중 하나일 수 있습니다. 지정하지 않을 경우 기본값은 <strong>day</strong>입니다. <strong>week</strong> 또는 <strong>month</strong>를 지정하는 경우 <em>failureName</em> 및 <em>failureHash</em> 값은 1000개 버킷으로 제한됩니다. | 아니요 |
| orderby | 문자열 | 결과 데이터 값의 순서를 지정하는 문입니다. 구문은 <em>orderby=field [order],field [order],...</em>입니다. 응답 본문의 다음과 같은 필드를 지정할 수 있습니다.<p/><ul><li><strong>date</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>osName</strong></li><li><strong>eventType</strong></li><li><strong>출시</strong></li><li><strong>deviceType</strong></li><li><strong>driverName</strong></li><li><strong>driverVersion</strong></li><li><strong>oemName</strong></li><li><strong>oemModel</strong></li><li><strong>flightRing</strong></li><li><strong>architecture</strong></li></ul><p><em>order</em> 매개 변수는 옵션이며 <strong>asc</strong> 또는 <strong>desc</strong>로 각 필드를 내림차순 또는 오름차순으로 지정할 수 있습니다. 기본값은 <strong>asc</strong>입니다.</p><p>다음은 <em>orderby</em> 문자열 예입니다. <em>orderby=date,market</em></p> |  아니요  |
| groupby | 문자열 | 지정된 필드에 대한 데이터 집계에만 적용되는 문입니다. 응답 본문의 다음과 같은 필드를 지정할 수 있습니다.<p/><ul><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>OSVersion</strong></li><li><strong>eventType</strong></li><li><strong>출시</strong></li><li><strong>deviceType</strong></li><li><strong>driverName</strong></li><li><strong>driverVersion</strong></li><li><strong>oemName</strong></li><li><strong>oemModel</strong></li><li><strong>flightRing</strong></li><li><strong>architecture</strong></li></ul><p>반환되는 데이터 행은 <em>groupby</em> 매개 변수에서 지정된 필드 및 다음을 포함합니다.</p><ul><li><strong>date</strong></li><li><strong>eventCount</strong></li></ul><p><em>groupby</em> 매개 변수는 <em>aggregationLevel</em> 매개 변수와 함께 사용할 수 있습니다. 예: <em>&amp;groupby=failureName,market&amp;aggregationLevel=week</em></p></p> |  아니요  |

<span/>


### <a name="request-example"></a>요청 예제

다음 예제에서는 OEM 하드웨어 오류 보고 데이터를 가져오는 데 필요한 몇 가지 요청을 보여 줍니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ihvdriver/failurehits?startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ihvdriver/failurehits?startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답


### <a name="response-body"></a>응답 본문

| 값      | 유형    | 설명     |
|------------|---------|--------------|
| 값      | 배열   | 집계 오류 보고 데이터가 포함된 개체의 배열입니다. 각 개체의 데이터에 대한 자세한 내용은 다음 표를 참조하세요.     |
| @nextLink  | string  | 데이터의 추가 페이지가 있는 경우 이 문자열에는 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 URI가 포함됩니다. 예를 들어 요청의 **top** 매개 변수가 10000으로 설정되어 있지만 쿼리에 대한 오류의 행이 10000개보다 많은 경우 이 값이 반환됩니다. |
| TotalCount | inumber | 쿼리에 대한 데이터 결과에 있는 행의 총 수입니다.     |

<span/>

*값* 배열의 요소에는 다음 값이 포함됩니다.

| 값           | 유형    | 설명        |
|-----------------|---------|---------------------|
| date            | 문자열  | 오류 데이터에 대한 날짜 범위의 시작 날짜입니다. 요청에서 하루를 지정한 경우 이 값은 해당 날짜입니다. 요청에서 주, 월 또는 다른 날짜 범위를 지정한 경우 이 값은 해당 날짜 범위의 시작 날짜입니다. |
| sellerId   | string  | 개발자 계정과 연결된 판매자 ID 값입니다(개발자 센터 계정 설정의 **판매자 ID** 값과 일치). |
| failureName     | 문자열  | 오류의 이름입니다.  |
| failureHash     | 문자열  | 오류의 고유 식별자입니다.   |
| symbol          | 문자열  | 이 오류에 할당된 기호입니다. |
| OSVersion       | string  | 오류가 발생한 OS의 네 부분으로 된 빌드 버전입니다.  |
| osName       | string  | 오류가 발생한 OS의 이름입니다.  |
| eventType       | string  | 발생한 오류의 유형입니다.      |
| market          | 문자열  | 디바이스 시장의 ISO 3166 국가 코드입니다.   |
| deviceType      | 문자열  | 오류가 발생한 디바이스 유형입니다.    |
| driverName     | string  | 이 오류와 연결된 드라이버의 고유 이름입니다.      |
| driverVersion  | string  | 이 오류와 연결된 드라이버의 버전입니다.   |
| architecture | string |  오류가 발생한 디바이스의 아키텍처입니다.  |
| oemName | string | 오류가 발생한 디바이스의 OEM 이름입니다. |
| oemModel | string | 오류가 발생한 디바이스 모델의 이름입니다. |
| flightRing | string | 오류가 발생한 OS 플라이트의 이름입니다. |
| eventCount      | inumber | 지정된 집계 수준 중에 이 오류를 발생시킨 이벤트의 수입니다.      |

<span/> 

### <a name="response-example"></a>응답 예제

다음 예제에서는 이 요청에 대한 예제 JSON 응답 본문을 보여 줍니다.

```json
{
  "Value": [
    {
      "date": "2017-02-26",
      "sellerId":"14313740",
      "driverName": "fastboot.sys",
      "osVersion": "6.3.9600.9600",
      "osName": "Windows 8.1",
      "flightRing": "Unknown",
      "oemName": "Contoso",
      "oemModel": "C-122",
      "market": "US",
      "symbol": "fastboot",
      "failureName": "AV_fastboot!unknown_function",
      "failureHash": "f060c0b6-fbe6-463f-a9f1-b7ebc1cc722f",
      "architecture": "x64",
      "driverVersion": "2.1.1.0",
      "deviceType": "Unknown",
      "eventType": "System crash",
      "deviceCount": 1.0,
      "eventCount": 1.0
    }]
}
```

## <a name="related-topics"></a>관련 항목

* [Windows 7 또는 Windows 8.x 드라이버 오류에 대한 세부 정보 가져오기](get-details-for-a-windows-7-or-windows-8.x-driver-error.md)
* [Windows 7 또는 Windows 8.x 드라이버 오류에 대한 CAB 파일 다운로드](download-the-cab-file-for-a-windows-7-or-windows-8.x-driver-error.md)
