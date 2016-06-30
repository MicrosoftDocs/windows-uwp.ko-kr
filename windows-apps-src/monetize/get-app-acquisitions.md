---
author: mcleanbyron
ms.assetid: C1E42E8B-B97D-4B09-9326-25E968680A0F
description: "Windows 스토어 분석 API에서 이 메서드를 사용하여 지정된 날짜 범위 및 다른 선택 필터 동안 응용 프로그램의 집계 구입 데이터를 가져옵니다."
title: "앱 획득 가져오기"
ms.sourcegitcommit: 02131e641cdaa76256845b38bcc50aa42d718601
ms.openlocfilehash: 7f87f931c92eca1f64fbd23b4fcba3359293f94a

---

# 앱 획득 가져오기


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

Windows 스토어 분석 API에서 이 메서드를 사용하여 지정된 날짜 범위 및 다른 선택 필터 동안 응용 프로그램의 집계 구입 데이터를 가져옵니다. 이 메서드는 JSON 형식의 데이터를 반환합니다.

## 필수 조건


이 메서드를 사용하려면 다음이 필요합니다.

-   이 메서드 호출에 사용할 Azure AD 응용 프로그램을 개발자 센터 계정과 연결합니다.

-   응용 프로그램에 대한 Azure AD 액세스 토큰을 가져옵니다.

자세한 내용은 [Windows 스토어 서비스를 사용하여 분석 데이터에 액세스](access-analytics-data-using-windows-store-services.md)를 참조하세요.

## 요청


### 요청 구문

| 메서드 | 요청 URI                                                              |
|--------|--------------------------------------------------------------------------|
| GET    | https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions |

 

### 요청 헤더

| 헤더        | 유형   | 설명                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 권한 부여 | 문자열 | 필수. **Bearer**&lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |

 

### 요청 본문

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">매개 변수</th>
<th align="left">유형</th>
<th align="left">설명</th>
<th align="left">필수</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">applicationId</td>
<td align="left">문자열</td>
<td align="left">구입 데이터를 검색할 앱의 스토어 ID입니다. 스토어 ID는 개발자 센터 대시보드의 [앱 ID 페이지](../publish/view-app-identity-details.md)에서 사용할 수 있습니다. 스토어 ID의 예로는 9WZDNCRFJ3Q8이 있습니다.</td>
<td align="left">예</td>
</tr>
<tr class="even">
<td align="left">startDate</td>
<td align="left">date</td>
<td align="left">검색할 구입 데이터의 날짜 범위에 대한 시작 날짜입니다. 기본값은 현재 날짜입니다.</td>
<td align="left">아니요</td>
</tr>
<tr class="odd">
<td align="left">endDate</td>
<td align="left">date</td>
<td align="left">검색할 구입 데이터의 날짜 범위에 대한 종료 날짜입니다. 기본값은 현재 날짜입니다.</td>
<td align="left">아니요</td>
</tr>
<tr class="even">
<td align="left">top</td>
<td align="left">int</td>
<td align="left">요청에서 반환할 데이터의 행의 수입니다. 지정되지 않은 경우 최대값 및 기본값은 10000입니다. 쿼리에 더 많은 행이 있는 경우 응답 본문에 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 다음 링크가 포함되어 있습니다.</td>
<td align="left">아니요</td>
</tr>
<tr class="odd">
<td align="left">skip</td>
<td align="left">int</td>
<td align="left">쿼리에서 건너뛸 행의 수입니다. 이 매개 변수를 사용하여 큰 데이터 집합의 페이지를 탐색합니다. 예를 들어 top=10000 및 skip=0이면 데이터의 처음 10000개 행을 검색하고 top=10000 및 skip=10000이면 데이터의 다음 10000개 행을 검색하는 방식입니다.</td>
<td align="left">아니요</td>
</tr>
<tr class="even">
<td align="left">filter</td>
<td align="left">문자열</td>
<td align="left">응답에서 행을 필터링하는 하나 이상의 문입니다. 자세한 내용은 아래의 [필터 필드](#filter-fields) 섹션을 참조하세요.</td>
<td align="left">아니요</td>
</tr>
<tr class="odd">
<td align="left">aggregationLevel</td>
<td align="left">문자열</td>
<td align="left">집계 데이터를 검색할 시간 범위를 지정합니다. <strong>day</strong>, <strong>week</strong> 또는 <strong>month</strong> 문자열 중 하나일 수 있습니다. 지정하지 않을 경우 기본값은 <strong>day</strong>입니다.</td>
<td align="left">아니요</td>
</tr>
<tr class="even">
<td align="left">orderby</td>
<td align="left">문자열</td>
<td align="left">각 구입에 대한 결과 데이터 값의 순서를 지정하는 문입니다. 구문은 <em>orderby=field [order],field [order],...</em>입니다. <em>field</em> 매개 변수는 다음 문자열 중 하나일 수 있습니다.
<ul>
<li><strong>date</strong></li>
<li><strong>acquisitionType</strong></li>
<li><strong>ageGroup</strong></li>
<li><strong>storeClient</strong></li>
<li><strong>gender</strong></li>
<li><strong>market</strong></li>
<li><strong>OSVersion</strong></li>
<li><strong>deviceType</strong></li>
<li><strong>orderName</strong></li>
</ul>
<p><em>order</em> 매개 변수는 옵션이며 <strong>asc</strong> 또는 <strong>desc</strong>로 각 필드를 내림차순 또는 오름차순으로 지정할 수 있습니다. 기본값은 <strong>asc</strong>입니다.</p>
<p>다음은 <em>orderby</em> 문자열 예입니다. <em>orderby=date,market</em></p></td>
<td align="left">아니요</td>
</tr>
</tbody>
</table>

 
### 필드 필터링

요청 본문의 *filter* 매개 변수에는 응답에서 행을 필터링하는 하나 이상의 문이 포함되어 있습니다. 각 문에는 **eq** 또는 **ne** 연산자와 연결된 필드 및 값이 포함되어 있으며 문은 **and** 또는 **or**를 사용하여 결합될 수 있습니다. 다음은 *filter* 매개 변수의 몇 가지 예입니다.

-   *filter=market eq 'US' and gender eq 'm'*
-   *filter=(market ne 'US') and (gender ne 'Unknown') and (gender ne 'm') and (market ne 'NO') and (ageGroup ne 'greater than 55' or ageGroup ne 'less than 13')*

지원되는 필드 목록은 다음 표를 참조하세요. 문자열 값은 *filter* 매개 변수에서 단일 따옴표로 묶여야 합니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">필드</th>
<th align="left">설명</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">acquisitionType</td>
<td align="left">다음 문자열 중 하나입니다.
<ul>
<li><strong>무료</strong></li>
<li><strong>평가판</strong></li>
<li><strong>유료</strong></li>
<li><strong>홍보 코드</strong></li>
<li><strong>iap</strong></li>
</ul></td>
</tr>
<tr class="even">
<td align="left">ageGroup</td>
<td align="left">다음 문자열 중 하나입니다.
<ul>
<li><strong>13보다 작음</strong></li>
<li><strong>13-17</strong></li>
<li><strong>18-24</strong></li>
<li><strong>25-34</strong></li>
<li><strong>35-44</strong></li>
<li><strong>44-55</strong></li>
<li><strong>55보다 큼</strong></li>
<li><strong>알 수 없음</strong></li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">storeClient</td>
<td align="left">다음 문자열 중 하나입니다.
<ul>
<li><strong>Windows Phone 스토어(클라이언트)</strong></li>
<li><strong>Windows 스토어(클라이언트)</strong></li>
<li><strong>Windows 스토어(웹)</strong></li>
<li><strong>조직에서 대량 구매</strong></li>
<li><strong>기타</strong></li>
</ul></td>
</tr>
<tr class="even">
<td align="left">gender</td>
<td align="left">다음 문자열 중 하나입니다.
<ul>
<li><strong>m</strong></li>
<li><strong>f</strong></li>
<li><strong>알 수 없음</strong></li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">출시</td>
<td align="left">구입이 발생한 시장의 ISO 3166 국가 코드를 포함하는 문자열입니다.</td>
</tr>
<tr class="even">
<td align="left">OSVersion</td>
<td align="left">다음 문자열 중 하나입니다.
<ul>
<li><strong>Windows Phone 7.5</strong></li>
<li><strong>Windows Phone 8</strong></li>
<li><strong>Windows Phone 8.1</strong></li>
<li><strong>Windows Phone 10</strong></li>
<li><strong>Windows 8</strong></li>
<li><strong>Windows 8.1</strong></li>
<li><strong>Windows 10</strong></li>
<li><strong>알 수 없음</strong></li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">deviceType</td>
<td align="left">다음 문자열 중 하나입니다.
<ul>
<li><strong>PC</strong></li>
<li><strong>태블릿</strong></li>
<li><strong>휴대폰</strong></li>
<li><strong>IoT</strong></li>
<li><strong>착용식</strong></li>
<li><strong>서버</strong></li>
<li><strong>공동 작업</strong></li>
<li><strong>기타</strong></li>
</ul></td>
</tr>
<tr class="even">
<td align="left">orderName</td>
<td align="left">앱을 구입하는 데 사용한 홍보 코드 주문 이름을 지정하는 문자열입니다(이는 사용자가 홍보 코드를 사용하여 앱을 구입한 경우에만 적용됩니다).</td>
</tr>
</tbody>
</table>

 

### 요청 예제

다음 예제에서는 앱 구입 데이터를 가져오는 데 필요한 몇 가지 요청을 보여 줍니다. *applicationId* 값을 앱의 스토어 ID로 바꿉니다.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US'; and gender eq 'm'  HTTP/1.1
Authorization: Bearer <your access token>
```

## 응답


### 응답 본문

| 값      | 유형   | 설명                                                                                                                                                                                                                                                                            |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 값      | 배열  | 집계 등급 데이터가 포함된 개체의 배열입니다. 각 개체의 데이터에 대한 자세한 내용은 아래 [구입 값](#acquisition-values) 섹션을 참조하세요.                                                                                                                      |
| @nextLink  | 문자열 | 데이터의 추가 페이지가 있는 경우 이 문자열에는 데이터의 다음 페이지를 요청하는 데 사용할 수 있는 URI가 포함됩니다. 예를 들어 요청의 **top** 매개 변수가 10000으로 설정되어 있지만 쿼리에 대한 구입 데이터의 행이 10000개보다 많은 경우 이 값이 반환됩니다. |
| TotalCount | int    | 쿼리에 대한 데이터 결과에 있는 행의 총 수입니다.                                                                                                                                                                                                                             |

 
### 구입 값

*값* 배열의 요소에는 다음 값이 포함됩니다.

| 값               | 유형   | 설명                                                                                                                                                                                                                              |
|---------------------|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| date                | 문자열 | 구입 데이터의 날짜 범위에 대한 시작 날짜입니다. 요청에서 하루를 지정한 경우 이 값은 해당 날짜입니다. 요청에서 주, 월 또는 다른 날짜 범위를 지정한 경우 이 값은 해당 날짜 범위의 시작 날짜입니다. |
| applicationId       | 문자열 | 구입 데이터를 검색할 앱의 스토어 ID입니다.                                                                                                                                                                 |
| applicationName     | 문자열 | 앱의 표시 이름                                                                                                                                                                                                             |
| deviceType          | 문자열 | 구입을 완료한 디바이스의 유형입니다. 지원되는 문자열의 목록은 위의 [필드 필터링](#filter-fields) 섹션을 참조하세요.                                                                                                  |
| orderName           | 문자열 | 주문의 이름입니다.                                                                                                                                                                                                                   |
| storeClient         | 문자열 | 구입이 발생한 스토어의 버전입니다. 지원되는 문자열의 목록은 위의 [필드 필터링](#filter-fields) 섹션을 참조하세요.                                                                                            |
| OSVersion           | 문자열 | 구입이 발생한 OS 버전입니다. 지원되는 문자열의 목록은 위의 [필드 필터링](#filter-fields) 섹션을 참조하세요.                                                                                                   |
| 출시              | 문자열 | 구입이 발생한 시장의 ISO 3166 국가 코드입니다.                                                                                                                                                                  |
| gender              | 문자열 | 구입한 사용자의 성별입니다. 지원되는 문자열의 목록은 위의 [필드 필터링](#filter-fields) 섹션을 참조하세요.                                                                                                    |
| ageGroup            | 문자열 | 구입한 사용자의 연령 그룹입니다. 지원되는 문자열의 목록은 위의 [필드 필터링](#filter-fields) 섹션을 참조하세요.                                                                                                 |
| acquisitionType     | 문자열 | 구입 형식(무료, 유료 등)입니다. 지원되는 문자열의 목록은 위의 [필드 필터링](#filter-fields) 섹션을 참조하세요.                                                                                                    |
| acquisitionQuantity | 숫자 | 지정된 집계 수준 중에 발생한 구입 횟수입니다.                                                                                                                                                         |

 

### 응답 예제

다음 예제에서는 이 요청에 대한 예제 JSON 응답 본문을 보여 줍니다.

```json
{
  "Value": [
    {
      "date": "2016-02-01",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Demo",
      "deviceType": "Phone",
      "orderName": "",
      "storeClient": "Windows Phone Store (client)",
      "osVersion": "Windows Phone 8.1",
      "market": "IT",
      "gender": "m",
      "ageGroup": "0-17",
      "acquisitionType": "Free",
      "acquisitionQuantity": 1
    }
  ],
  "@nextLink": "appacquisitions?applicationId=9NBLGGGZ5QDR&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1&orderby=date desc",
  "TotalCount": 466766
}
```

## 관련 항목

* [Windows 스토어 서비스를 사용하여 분석 데이터에 액세스](access-analytics-data-using-windows-store-services.md)
* [IAP 구입 가져오기](get-in-app-acquisitions.md)
* [오류 보고 데이터 가져오기](get-error-reporting-data.md)
* [앱 등급 가져오기](get-app-ratings.md)
* [앱 리뷰 가져오기](get-app-reviews.md)



<!--HONumber=Jun16_HO4-->


