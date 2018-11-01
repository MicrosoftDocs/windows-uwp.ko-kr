---
author: Xansky
ms.assetid: d305746a-d370-4404-8cde-c85765bf3578
description: Microsoft Store 프로모션 API에서 이 메서드를 사용하여 홍보용 광고 캠페인 타기팅 프로필을 관리합니다.
title: 타기팅 프로필 관리
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 프로모션 API, 광고 캠페인
ms.localizationpriority: medium
ms.openlocfilehash: 50960a079e2c38d52d3a15403aef091ea99d7696
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5882541"
---
# <a name="manage-targeting-profiles"></a>타기팅 프로필 관리


Microsoft Store 프로모션 API에서 이 메서드를 사용하여 홍보용 광고 캠페인에서 각 배달 라인의 대상으로 지정할 사용자, 지역 및 인벤토리 유형을 선택합니다. 다수의 배달 라인에 대해 타기팅 프로필을 만들고 다시 사용할 수 있습니다.

타기팅 프로필과 광고 캠페인, 배달 라인 및 크리에이티브 간의 관계에 대한 자세한 내용은 [Microsoft Store 서비스를 사용하여 광고 캠페인 실행](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api)을 참조하세요.

## <a name="prerequisites"></a>필수 조건

이 메서드를 사용하려면 먼저 다음 작업을 완료해야 합니다.

* 아직 완료하지 않은 경우 Microsoft Store 프로모션 API의 [필수 조건](run-ad-campaigns-using-windows-store-services.md#prerequisites)을 모두 완료합니다.
* 이 메서드의 요청 헤더에 사용할 [Azure AD 액세스 토큰을 가져옵니다](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token). 액세스 토큰을 얻은 후 만료되기 전에 60분 동안 사용할 수 있습니다. 토큰이 만료된 후 새 토큰을 가져올 수 있습니다.

## <a name="request"></a>요청

이 메서드에는 다음 URI가 있습니다.

| 메서드 유형 | 요청 URI                                                      |  설명  |
|--------|------------------------------------------------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile``` |  새 타기팅 프로필을 만듭니다.  |
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile/{targetingProfileId}``` |  *targetingProfileId*에서 지정한 타기팅 프로필을 편집합니다.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile/{targetingProfileId}``` |  *targetingProfileId*에서 지정한 타기팅 프로필을 가져옵니다.  |


### <a name="header"></a>헤더

| 헤더        | 유형   | 설명         |
|---------------|--------|---------------------|
| 권한 부여 | 문자열 | 필수. **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰입니다. |
| 추적 ID   | GUID   | 선택 사항입니다. 호출 흐름을 추적하는 ID입니다.                                  |


### <a name="request-body"></a>요청 본문

POST 및 PUT 메서드는 [타기팅 프로필](#targeting-profile) 개체의 필수 필드 및 설정하거나 변경하려는 추가 필드가 있는 JSON 요청 본문이 필요합니다.


### <a name="request-examples"></a>요청 예시

다음 예시에서는 POST 메서드를 호출하여 타기팅 프로필을 만드는 방법을 보여줍니다.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile HTTP/1.1
Authorization: Bearer <your access token>

{
    "name": "Contoso App Campaign - Targeting Profile 1",
    "targetingType": "Manual",
    "age": [
      651,
      652],
    "gender": [
      700
    ],
    "country": [
      11,
      12
    ],
    "osVersion": [
      504
    ],
    "deviceType": [
      710
    ],
    "supplyType": [
      11470
    ]
}
```

다음 예시에서는 GET 메서드를 호출하여 타기팅 프로필을 검색하는 방법을 보여줍니다.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile/310023951  HTTP/1.1
Authorization: Bearer <your access token>
```

<span/>

## <a name="response"></a>응답

이러한 메서드는 만들거나 업데이트하거나 검색한 타기팅 프로필에 대한 정보가 포함된 [타기팅 프로필](#targeting-profile) 개체가 있는 JSON 응답 본문을 반환합니다. 다음 예시에서는 이 메서드에 대한 응답 본문을 보여줍니다.

```json
{
  "Data": {
    "id": 310021746,
    "name": "Contoso App Campaign - Targeting Profile 1",
    "targetingType": "Manual",
    "age": [
      651,
      652
    ],
    "gender": [
      700
    ],
    "country": [
      6,
      13,
      29
    ],
    "osVersion": [
      504,
      505,
      506,
      507,
      508
    ],
    "deviceType": [
      710,
      711
    ],
    "supplyType": [
      11470
    ]
  }
}
```

<span id="targeting-profile"/>

## <a name="targeting-profile-object"></a>타기팅 프로필 개체

이 메서드에 대한 요청 및 응답 본문은 다음 필드를 포함합니다. 이 표에서는 읽기 전용(PUT 메서드에서 변경할 수 없음을 의미) 필드와 POST 메서드에 대한 요청 본문에서 필요한 필드가 어떤 것인지 보여줍니다.

| 필드        | 유형   |  설명      |  읽기 전용  | 기본값  | POST에 필요한지 여부 |  
|--------------|--------|---------------|------|-------------|------------|
|  id   |  정수   |  타기팅 프로필의 ID입니다.     |   예    |       |   아니요      |       
|  name   |  문자열   |   대상 프로필의 이름입니다.    |    아니요   |      |  예     |       
|  targetingType   |  문자열   |  다음 값 중 하나입니다. <ul><li>**자동**: 이 값을 지정하여 Microsoft가 개발자 센터의 앱 설정에 따라 타기팅 프로필을 선택할 수 있도록 허용합니다.</li><li>**수동**: 이 값을 지정하여 자신만의 타기팅 프로필을 정의합니다.</li></ul>     |  아니요     |  자동    |   예    |       
|  age   |  배열   |   대상으로 지정할 사용자의 연령대를 식별하는 하나 이상의 정수입니다. 정수의 전체 목록을 보려면 이 문서에 있는 [연령 값](#age-values)을 참조하세요.    |    아니요    |  null    |     아니요    |       
|  gender   |  배열   |  대상으로 지정할 사용자의 성별을 식별하는 하나 이상의 정수입니다. 정수의 전체 목록을 보려면 이 문서에 있는 [성별 값](#gender-values)을 참조하세요.       |  아니요    |  null    |     아니요    |       
|  country   |  배열   |  대상으로 지정할 사용자의 국가 코드를 식별하는 하나 이상의 정수입니다. 정수의 전체 목록을 보려면 이 문서에 있는 [국가 코드 값](#country-code-values)을 참조하세요.    |  아니요    |  null   |      아니요   |       
|  osVersion   |  배열   |   대상으로 지정할 사용자의 OS 버전을 식별하는 하나 이상의 정수입니다. 정수의 전체 목록을 보려면 이 문서에 있는 [OS 버전 값](#osversion-values)을 참조하세요.     |   아니요    |  null   |     아니요    |       
|  deviceType   | 배열    |  대상으로 지정할 사용자의 장치 유형을 식별하는 하나 이상의 정수입니다. 정수의 전체 목록을 보려면 이 문서에 있는 [장치 유형 값](#devicetype-values)을 참조하세요.       |   아니요    |  null    |    아니요     |       
|  supplyType   |  배열   |  캠페인의 광고가 표시될 인벤토리 유형을 식별하는 하나 이상의 정수입니다. 정수의 전체 목록을 보려면 이 문서에 있는 [제공 유형 값](#supplytype-values)을 참조하세요.      |   아니요    |  null   |     아니요    |   |  


<span id="age-values"/>

### <a name="age-values"></a>연령 값

[TargetingProfile](#targeting-profile) 개체의 *age* 필드에는 대상으로 지정할 사용자의 연령대를 식별하는 다음과 같은 정수 중 하나 이상이 포함되어 있습니다.

|  *age* 필드의 정수 값  |  해당 연령대  |  
|---------------------------------|---------------------------|
|     651     |            13~17             |
|     652     |           18~24             |
|     653     |            25~34             |
|     654     |            35~49             |
|     655     |            50 이상             |

*age* 필드에 지원되는 값을 프로그래밍 방식으로 가져오려면 다음과 같이 GET 메서드를 호출하면 됩니다.  ```Authorization``` 헤더의 경우, **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰을 전달합니다.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/age
Authorization: Bearer <your access token>
```

다음 예시에서는 이 메서드에 대한 응답 본문을 보여줍니다.

```json
{
  "Data": {
    "Age": {
      "651": "Age13To17",
      "652": "Age18To24",
      "653": "Age25To34",
      "654": "Age35To49",
      "655": "Age50AndAbove"
    }
  }
}
```

<span id="gender-values"/>

### <a name="gender-values"></a>성별 값

[TargetingProfile](#targeting-profile) 개체의 *gender* 필드에는 대상으로 지정할 사용자의 성별을 식별하는 다음과 같은 정수 중 하나 이상이 포함되어 있습니다.

|  *gender* 필드의 정수 값  |  해당 성별  |  
|---------------------------------|---------------------------|
|     700     |            남성             |
|     701     |           여성             |

*gender* 필드에 지원되는 값을 프로그래밍 방식으로 가져오려면 다음과 같이 GET 메서드를 호출하면 됩니다.  ```Authorization``` 헤더의 경우, **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰을 전달합니다.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/gender
Authorization: Bearer <your access token>
```

다음 예시에서는 이 메서드에 대한 응답 본문을 보여줍니다.

```json
{
  "Data": {
    "Gender": {
      "700": "Male",
      "701": "Female"
    }
  }
}
```


<span id="osversion-values"/>

### <a name="os-version-values"></a>OS 버전 값

[TargetingProfile](#targeting-profile) 개체의 *osVersion* 필드에는 대상으로 지정할 사용자의 OS 버전을 식별하는 다음과 같은 정수 중 하나 이상이 포함되어 있습니다.

|  *osVersion* 필드의 정수 값  |  해당 OS 버전  |  
|---------------------------------|---------------------------|
|     500     |            Windows Phone 7             |
|     501     |           Windows Phone 7.1             |
|     502     |           Windows Phone 7.5             |
|     503     |           Windows Phone 7.8             |
|     504     |           Windows Phone 8.0             |
|     505     |           Windows Phone 8.1             |
|     506     |           Windows 8.0             |
|     507     |           Windows8.1             |
|     508     |           Windows 10             |
|     509     |           Windows 10 Mobile             |

*osVersion* 필드에 지원되는 값을 프로그래밍 방식으로 가져오려면 다음과 같이 GET 메서드를 호출하면 됩니다.  ```Authorization``` 헤더의 경우, **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰을 전달합니다.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/osversion
Authorization: Bearer <your access token>
```

다음 예시에서는 이 메서드에 대한 응답 본문을 보여줍니다.

```json
{
  "Data": {
    "OsVersion": {
      "500": "WindowsPhone70",
      "501": "WindowsPhone71",
      "502": "WindowsPhone75",
      "503": "WindowsPhone78",
      "504": "WindowsPhone80",
      "505": "WindowsPhone81",
      "506": "Windows80",
      "507": "Windows81",
      "508": "Windows10",
      "509": "WindowsPhone10"
    }
  }
}
```


<span id="devicetype-values"/>

### <a name="device-type-values"></a>디바이스 유형 값

[TargetingProfile](#targeting-profile) 개체의 *deviceType* 필드에는 대상으로 지정할 사용자의 디바이스 유형을 식별하는 다음과 같은 정수 중 하나 이상이 포함되어 있습니다.

|  *deviceType* 필드의 정수 값  |  해당 디바이스 유형  |  설명  |
|---------------------------------|---------------------------|---------------------------|
|     710     |  Windows   |  이것은 데스크톱 버전 Windows 10 또는 Windows 8.x를 실행하는 디바이스를 나타냅니다.  |
|     711     |  전화     |  이것은 Windows 10 Mobile, Windows Phone 8.x 또는 Windows Phone 7.x를 실행하는 디바이스를 나타냅니다.

*deviceType* 필드에 지원되는 값을 프로그래밍 방식으로 가져오려면 다음과 같이 GET 메서드를 호출하면 됩니다.  ```Authorization``` 헤더의 경우, **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰을 전달합니다.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/devicetype
Authorization: Bearer <your access token>
```

다음 예시에서는 이 메서드에 대한 응답 본문을 보여줍니다.

```json
{
  "Data": {
    "DeviceType": {
      "710": "Windows",
      "711": "Phone"
    }
  }
}
```


<span id="supplytype-values"/>

### <a name="supply-type-values"></a>유형 값을 제공

[TargetingProfile](#targeting-profile) 개체의 *supplyType* 필드에는 캠페인의 광고가 표시될 인벤토리 유형을 식별하는 다음과 같은 정수 중 하나 이상이 포함되어 있습니다.

|  *supplyType* 필드의 정수 값  |  해당하는 제공 형식  |  설명  |
|---------------------------------|---------------------------|---------------------------|
|     11470     |  앱        |  이것은 앱에만 표시되는 광고를 가리킵니다.  |
|     11471     |  범용        |  이것은 앱, 웹 및 기타 디스플레이 화면에 표시되는 광고를 가리킵니다.  |

*supplyType* 필드에 지원되는 값을 프로그래밍 방식으로 가져오려면 다음과 같이 GET 메서드를 호출하면 됩니다.  ```Authorization``` 헤더의 경우, **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰을 전달합니다.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/supplytype
Authorization: Bearer <your access token>
```

다음 예시에서는 이 메서드에 대한 응답 본문을 보여줍니다.

```json
{
  "Data": {
    "SupplyType": {
      "11470": "App",
      "11471": "Universal"
    }
  }
}
```

<span id="country-code-values"/>

### <a name="country-code-values"></a>국가 코드 값

[TargetingProfile](#targeting-profile) 개체의 *country* 필드에는 대상으로 지정할 사용자의 [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) 국가 코드를 식별하는 다음과 같은 정수 중 하나 이상이 포함되어 있습니다.

|  *country* 필드의 정수 값  |  해당 국가 코드  |  
|-------------------------------------|------------------------------|
|     1      |            US                  |
|     2      |            AU                  |
|     3      |            AT                  |
|     4      |            BE                  |
|     5      |            BR                  |
|     6      |            CA                  |
|     7      |            DK                  |
|     8      |            FI                  |
|     9      |            FR                  |
|     10      |            DE                  |
|     11      |            GR                  |
|     12      |            HK                  |
|     13      |            IN                  |
|     14      |            IE                  |
|     15      |            IT                  |
|     16      |            JP                  |
|     17      |            LU                  |
|     18      |            MX                  |
|     19      |            NL                  |
|     20      |            NZ                  |
|     21      |            NO                  |
|     22      |            PL                  |
|     23      |            PT                  |
|     24      |            SG                  |
|     25      |            ES                  |
|     26      |            SE                  |
|     27      |            CH                  |
|     28      |            TW                  |
|     29      |            GB                  |
|     30      |            RU                  |
|     31      |            CL                  |
|     32      |            CO                  |
|     33      |            CZ                  |
|     34      |            HU                  |
|     35      |            ZA                  |
|     36      |            KR                  |
|     37      |            CN                  |
|     38      |            RO                  |
|     39      |            TR                  |
|     40      |            SK                  |
|     41      |            IL                  |
|     42      |            ID                  |
|     43      |            AR                  |
|     44      |            MY                  |
|     45      |            PH                  |
|     46      |            PE                  |
|     47      |            UA                  |
|     48      |            AE                  |
|     49      |            TH                  |
|     50      |            IQ                  |
|     51      |            VN                  |
|     52      |            CR                  |
|     53      |            VE                  |
|     54      |            QA                  |
|     55      |            SI                  |
|     56      |            BG                  |
|     57      |            LT                  |
|     58      |            RS                  |
|     59      |            HR                  |
|     60      |            HR                  |
|     61      |            LV                  |
|     62      |            EE                  |
|     63      |            IS                  |
|     64      |            KZ                  |
|     65      |            SA                  |
|     67      |            AL                  |
|     68      |            DZ                  |
|     70      |            AO                  |
|     72      |            AM                  |
|     73      |            AZ                  |
|     74      |            BS                  |
|     75      |            BD                  |
|     76      |            BB                  |
|     77      |            BY                  |
|     81      |            BO                  |
|     82      |            BA                  |
|     83      |            BW                  |
|     87      |            KH                  |
|     88      |            CM                  |
|     94      |            CD                  |
|     95      |            CI                  |
|     96      |            CY                  |
|     99      |            DO                  |
|     100      |            EC                  |
|     101      |            EG                  |
|     102      |            SV                  |
|     107      |            FJ                  |
|     108      |            GA                  |
|     110      |            GE                  |
|     111      |            GH                  |
|     114      |            GT                  |
|     118      |            HT                  |
|     119      |            HN                  |
|     120      |            JM                  |
|     121      |            JO                  |
|     122      |            KE                  |
|     124      |            KW                  |
|     125      |            KG                  |
|     126      |            LA                  |
|     127      |            LB                  |
|     133      |            MK                  |
|     135      |            MW                  |
|     138      |            MT                  |
|     141      |            MU                  |
|     145      |            ME                  |
|     146      |            MA                  |
|     147      |            MZ                  |
|     148      |            NA                  |
|     150      |            NP                  |
|     151      |            NI                  |
|     153      |            NG                  |
|     154      |            OM                  |
|     155      |            PK                  |
|     157      |            PA                  |
|     159      |            PY                  |
|     167      |            SN                  |
|     172      |            LK                  |
|     176      |            TZ                  |
|     180      |            TT                  |
|     181      |            TN                  |
|     184      |            UG                  |
|     185      |            UY                  |
|     186      |            UZ                  |
|     189      |            ZM                  |
|     190      |            ZW                  |
|     219      |            MD                  |
|     224      |            PS                  |
|     225      |            RE                  |
|     246      |            PR                  |

*country* 필드에 지원되는 값을 프로그래밍 방식으로 가져오려면 다음과 같이 GET 메서드를 호출하면 됩니다.  ```Authorization``` 헤더의 경우, **Bearer** &lt;*token*&gt; 형식의 Azure AD 액세스 토큰을 전달합니다.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/country
Authorization: Bearer <your access token>
```

다음 예시에서는 이 메서드에 대한 응답 본문을 보여줍니다.

```json
{
  "Data": {
    "Country": {
      "1": "US",
      "2": "AU",
      "3": "AT",
      "4": "BE",
      "5": "BR",
      "6": "CA",
      "7": "DK",
      "8": "FI",
      "9": "FR",
      "10": "DE",
      "11": "GR",
      "12": "HK",
      "13": "IN",
      "14": "IE",
      "15": "IT",
      "16": "JP",
      "17": "LU",
      "18": "MX",
      "19": "NL",
      "20": "NZ",
      "21": "NO",
      "22": "PL",
      "23": "PT",
      "24": "SG",
      "25": "ES",
      "26": "SE",
      "27": "CH",
      "28": "TW",
      "29": "GB",
      "30": "RU",
      "31": "CL",
      "32": "CO",
      "33": "CZ",
      "34": "HU",
      "35": "ZA",
      "36": "KR",
      "37": "CN",
      "38": "RO",
      "39": "TR",
      "40": "SK",
      "41": "IL",
      "42": "ID",
      "43": "AR",
      "44": "MY",
      "45": "PH",
      "46": "PE",
      "47": "UA",
      "48": "AE",
      "49": "TH",
      "50": "IQ",
      "51": "VN",
      "52": "CR",
      "53": "VE",
      "54": "QA",
      "55": "SI",
      "56": "BG",
      "57": "LT",
      "58": "RS",
      "59": "HR",
      "60": "BH",
      "61": "LV",
      "62": "EE",
      "63": "IS",
      "64": "KZ",
      "65": "SA",
      "67": "AL",
      "68": "DZ",
      "70": "AO",
      "72": "AM",
      "73": "AZ",
      "74": "BS",
      "75": "BD",
      "76": "BB",
      "77": "BY",
      "81": "BO",
      "82": "BA",
      "83": "BW",
      "87": "KH",
      "88": "CM",
      "94": "CD",
      "95": "CI",
      "96": "CY",
      "99": "DO",
      "100": "EC",
      "101": "EG",
      "102": "SV",
      "107": "FJ",
      "108": "GA",
      "110": "GE",
      "111": "GH",
      "114": "GT",
      "118": "HT",
      "119": "HN",
      "120": "JM",
      "121": "JO",
      "122": "KE",
      "124": "KW",
      "125": "KG",
      "126": "LA",
      "127": "LB",
      "133": "MK",
      "135": "MW",
      "138": "MT",
      "141": "MU",
      "145": "ME",
      "146": "MA",
      "147": "MZ",
      "148": "NA",
      "150": "NP",
      "151": "NI",
      "153": "NG",
      "154": "OM",
      "155": "PK",
      "157": "PA",
      "159": "PY",
      "167": "SN",
      "172": "LK",
      "176": "TZ",
      "180": "TT",
      "181": "TN",
      "184": "UG",
      "185": "UY",
      "186": "UZ",
      "189": "ZM",
      "190": "ZW",
      "219": "MD",
      "224": "PS",
      "225": "RE",
      "246": "PR"
    }
  }
}
```

## <a name="related-topics"></a>관련 항목

* [Microsoft Store 서비스를 사용하여 광고 캠페인 실행](run-ad-campaigns-using-windows-store-services.md)
* [광고 캠페인 관리](manage-ad-campaigns.md)
* [광고 캠페인 배달 라인 관리](manage-delivery-lines-for-ad-campaigns.md)
* [광고 캠페인 크리에이티브 관리](manage-creatives-for-ad-campaigns.md)
* [광고 캠페인 성과 데이터 가져오기](get-ad-campaign-performance-data.md)
