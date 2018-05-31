---
author: mcleanbyron
ms.assetid: 8425F704-8A03-493F-A3D2-8442E85FD835
description: Microsoft Store 분석 API에서 이 메서드를 사용하여 특정 하드웨어 오류에 대한 자세한 데이터를 가져올 수 있습니다. 이 메서드는 OEM용으로만 제공됩니다.
title: OEM 하드웨어 오류에 대한 세부 정보 가져오기
ms.author: mcleans
ms.date: 01/18/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store 서비스, Microsoft Store 분석 API, 오류, 세부 정보
ms.localizationpriority: medium
ms.openlocfilehash: 429ebc5237ce35baa6f9c3f31a25d480410d9c86
ms.sourcegitcommit: 1773bec0f46906d7b4d71451ba03f47017a87fec
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/17/2018
ms.locfileid: "1663213"
---
# <a name="get-details-for-an-oem-hardware-error"></a>OEM 하드웨어 오류에 대한 세부 정보 가져오기

Microsoft Store 분석 API에서 이 메서드를 사용하여 특정 OEM 하드웨어 오류에 대한 자세한 데이터를 JSON 형식으로 가져올 수 있습니다. 이 메서드를 사용하려면 먼저 [OEM 하드웨어 오류 보고 데이터 가져오기](get-oem-hardware-error-reporting-data.md) 메서드를 통해 자세한 정보를 가져오려는 오류 ID를 검색해야 합니다.

> [!NOTE]
> 이 메서드는 [Windows 하드웨어 개발자 센터 프로그램](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard)에 속하는 개발자 계정으로만 사용할 수 있습니다.

## <a name="prerequisites"></a>필수 조건


이 메서드를 사용하려면 다음을 먼저 수행해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 분석 API에 대한 모든 [필수 조건](access-analytics-data-using-windows-store-services.md#prerequisites)을 완료합니다.
* 이 메서드에 대한 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.
* 자세한 정보를 가져오려는 오류 ID를 가져옵니다. 이 ID를 가져오려면 [OEM 하드웨어 오류 보고 데이터 가져오기](get-oem-hardware-error-reporting-data.md) 메서드를 사용하고 해당 메서드의 응답 본문에 **failureHash** 값을 사용합니다.

## <a name="request"></a>요청


### <a name="request-syntax"></a>요청 구문

| 메서드 | 요청 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/hardware/failuredetails``` |


### <a name="request-header"></a>요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |


### <a name="request-parameters"></a>요청 매개 변수

| 매개 변수        | 유형   |  설명      |  필수  
|---------------|--------|---------------|------|
| failureHash | 문자열 | 자세한 정보를 가져오려는 오류의 고유 ID입니다. 관심 있는 오류의 이 값을 가져오려면 [OEM 하드웨어 오류 보고 데이터 가져오기](get-oem-hardware-error-reporting-data.md) 메서드를 사용하고 해당 메서드의 응답 본문에 **failureHash** 값을 사용합니다. |  예  |
| startDate | date | 검색할 자세한 오류 데이터의 날짜 범위에 대한 시작 날짜입니다. 기본값은 현재 날짜보다 30일 전입니다. |  아니요  |
| endDate | date | 검색할 자세한 오류 데이터의 날짜 범위에 대한 종료 날짜입니다. 기본값은 현재 날짜입니다. |  아니요  |
| top | int | 요청에서 반환할 데이터의 행의 수입니다. 지정되지 않은 경우 최대값 및 기본값은 10000입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 다음 링크가 포함되어 있습니다. |  아니요  |
| skip | int | 쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용하여 큰 데이터 집합의 페이지를 탐색할 수 있습니다. 예를 들어 top=10 및 skip=0이면 데이터의 처음 10개 행을 검색하고 top=10 및 skip=10이면 데이터의 다음 10개 행을 검색하는 방식입니다. |  아니요  |
| filter |문자열  | 응답에서 행을 필터링하는 하나 이상의 문입니다. 각 문에는 응답 본문의 필드 이름 및 **eq** 또는 **ne** 연산자와 연결된 값이 포함되어 있으며 문은 **and** 또는 **or**를 사용하여 결합될 수 있습니다. 문자열 값은 *filter* 매개 변수에서 단일 따옴표로 묶여야 합니다. 다음 필드를 지정할 수 있습니다.<p/><ul><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>OSVersion</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>moduleName</strong></li><li><strong>moduleVersion</strong></li><li><strong>mode</strong></li><li><strong>architecture</strong></li><li><strong>model</strong></li><li><strong>baseboard</strong></li><li><strong>modelFamily</strong></li><li><strong>flightRing</strong></li><li><strong>cabIdHash</strong></li><li><strong>cabType</strong></li><li><strong>cabExpirationTime</strong></li></ul> | 아니요   |
| orderby | string | 결과 데이터 값의 순서를 지정하는 문입니다. 구문은 <em>orderby=field [order],field [order],...</em>입니다. 응답 본문의 다음 필드를 지정할 수 있습니다.<p/><ul><li><strong>date</strong></li><li><strong>market</strong></li><li><strong>cabIdHash</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>cabType</strong></li><li><strong>deviceType</strong></li><li><strong>moduleName</strong></li><li><strong>moduleVersion</strong></li><li><strong>osVersion</strong></li><li><strong>packageVersion</strong></li><li><strong>osBuild</strong></li><li><strong>mode</strong></li><li><strong>architecture</strong></li><li><strong>model</strong></li><li><strong>baseboard</strong></li><li><strong>modelFamily</strong></li></ul><p><em>order</em> 매개 변수는 옵션이며 <strong>asc</strong> 또는 <strong>desc</strong>로 각 필드를 내림차순 또는 오름차순으로 지정할 수 있습니다. 기본값은 <strong>asc</strong>입니다.</p><p>다음은 <em>orderby</em> 문자열 예입니다. <em>orderby=date,market</em></p> |  아니요  |


### <a name="request-example"></a>요청 예제

다음 예제에서는 자세한 오류 데이터를 가져오는 데 필요한 몇 가지 요청을 보여 줍니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/hardware/failuredetails?failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/hardware/failuredetails?failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>응답


### <a name="response-body"></a>응답 본문

| 값      | 유형    | 설명    |
|------------|---------|------------|
| 값      | array   | 자세한 오류 데이터를 포함하는 개체의 배열입니다. 각 개체의 데이터에 대한 자세한 내용은 다음 표를 참조하세요.          |
| @nextLink  | string  | 데이터의 추가 페이지가 있는 경우 이 문자열에는 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 URI가 포함됩니다. 예를 들어 요청의 **top** 매개 변수가 10으로 설정되어 있지만 쿼리에 대한 오류의 행이 10개보다 많은 경우 이 값이 반환됩니다. |
| TotalCount | inumber | 쿼리에 대한 데이터 결과에 있는 행의 총 수입니다.        |


*값* 배열의 요소에는 다음 값이 포함됩니다.

| 값           | 유형    | 설명     |
|-----------------|---------|----------------------------|
| date            | 문자열  | 오류 데이터에 대한 날짜 범위의 시작 날짜입니다. 요청에서 하루를 지정한 경우 이 값은 해당 날짜입니다. 요청에서 주, 월 또는 다른 날짜 범위를 지정한 경우 이 값은 해당 날짜 범위의 시작 날짜입니다. |
| sellerId   | string  | 개발자 계정과 연결된 판매자 ID 값입니다(개발자 센터 계정 설정의 **판매자 ID** 값과 일치). |
| failureName     | 문자열  | 오류 이름은 하나 이상의 문제 클래스, 예외/버그 확인 코드, 오류가 발생한 이미지/드라이버의 이름 및 관련된 기능 이름 등 네 부분으로 구성됩니다.             |
| failureHash     | 문자열  | 오류의 고유 식별자입니다.     |
| osVersion       | string  | 오류가 발생한 OS의 네 부분으로 된 빌드 버전입니다.    |
| market          | 문자열  | 디바이스 시장의 ISO 3166 국가 코드입니다.     |
| deviceType      | string  | 오류가 발생한 디바이스 유형을 나타내는 다음 문자열 중 하나입니다.<p/><ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul>     |
| moduleName     | string  | 이 오류와 연결된 모듈의 고유 이름입니다.      |
| moduleVersion  | string  | 이 오류와 연결된 모듈의 버전입니다.   |
| architecture | string |  오류가 발생한 디바이스의 아키텍처입니다.  |
| model | string | 오류가 발생한 디바이스 모델의 이름입니다. |
| baseboard | string | 오류가 발생한 디바이스 베이스보드의 이름입니다. |
| modelFamily | string | 오류가 발생한 디바이스 모델 제품군의 이름입니다. |
| mode | string | 이 값은 항상 *kernel*입니다. |
| cabIdHash         | string  | 이 오류와 연결된 CAB 파일의 고유 ID입니다.   |
| cabType         | string  | CAB 파일의 유형입니다.   |
| cabExpirationTime  | 문자열  | CAB 파일이 만료되고 더 이상 다운로드할 수 없는 ISO 8601 형식의 날짜 및 시간입니다.   |


### <a name="response-example"></a>응답 예제

다음 예제에서는 이 요청에 대한 예제 JSON 응답 본문을 보여 줍니다.

```json
{
  "Value": [
    {
      "date": "2017-03-14 23:51:11",
      "sellerId": "14313740",
      "mode": "Kernel",
      "failureName": "0x109_18_2_LoadImageNotify",
      "failureHash": "f060c0b6-fbe6-463f-a9f1-b7ebc1cc722f",
      "cabIdHash": "cc1797f9-b783-44d4-b0e5-46ae7ca668bc",
      "cabType": "MINI",
      "cabExpirationTime": "2017-06-12 23:51:11",
      "osVersion": "10.0.14393.10",
      "market": "US",
      "deviceType": "PC",
      "moduleName": "Unknown_Image",
      "moduleVersion": "Unknown",
      "architecture": "x64",
      "model": "101",
      "baseboard": "750-ABX",
      "modelFamily": "ContosoPad",
      "flightRing": "Windows 10 Retail"
    }]
}
```

## <a name="related-topics"></a>관련 항목

* [OEM 하드웨어 오류 보고 데이터 가져오기](get-oem-hardware-error-reporting-data.md)
* [OEM 하드웨어 오류에 대한 CAB 파일 다운로드](download-the-cab-file-for-an-oem-hardware-error.md)
