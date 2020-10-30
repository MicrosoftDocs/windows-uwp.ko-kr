---
description: 사용자 지정 템플릿 및 패턴으로 Windows.globalization.datetimeformatting API를 사용 하 여 원하는 형식으로 날짜 및 시간을 표시 합니다.
title: 패턴을 사용 하 여 날짜 및 시간 형식 지정
ms.assetid: 012028B3-9DA2-4E72-8C0E-3E06BEC3B3FE
label: Use patterns to format dates and times
template: detail.hbs
ms.date: 11/09/2017
ms.topic: article
keywords: windows 10, uwp, 세계화, 지역화 가능성, 지역화
ms.localizationpriority: medium
ms.openlocfilehash: dbabbcaccd88b187a03c83909bcb38d5f64b30bb
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034316"
---
# <a name="use-templates-and-patterns-to-format-dates-and-times"></a>날짜 및 시간 형식을 지정하는 템플릿 및 패턴 사용

사용자 지정 템플릿과 패턴을 사용 하 여 [**windows.globalization.datetimeformatting**](/uwp/api/windows.globalization.datetimeformatting?branch=live) 네임 스페이스의 클래스를 사용 하 여 원하는 형식으로 날짜 및 시간을 표시 합니다.

## <a name="introduction"></a>소개

[**DateTimeFormatter**](/uwp/api/windows.globalization.datetimeformatting?branch=live) 클래스는 전 세계의 언어 및 지역에 대 한 날짜 및 시간 형식을 올바르게 지정 하는 다양 한 방법을 제공 합니다. 연도, 월, 일 등에 표준 형식을 사용할 수 있습니다. 또는 DateTimeFormatter 생성자의 *formatTemplate* 인수 (예: " **DateTimeFormatter** ")에 서식 템플릿을 전달할 수 있습니다.

하지만 표시할 [**DateTime**](/uwp/api/windows.foundation.datetime?branch=live) 개체의 구성 요소에 대 한 순서와 형식을 더 세부적으로 제어 하려는 경우에는 형식 패턴을 생성자의 *formatTemplate* 인수에 전달할 수 있습니다. 형식 패턴은 특정 구문을 사용 합니다 .이 구문을 사용 하 여 **DateTime** 개체의 개별 구성 요소를 &mdash; 월 이름 으로만 얻거나 연도 값만을 선택 하 여 &mdash; 사용자 지정 형식으로 표시할 수 있습니다. 또한 패턴을 지역화 하 여 다른 언어 및 지역에 맞게 적용할 수 있습니다.

**참고**  이는 형식 패턴의 개요에 불과합니다. 서식 템플릿 및 서식 패턴에 대 한 자세한 내용은 [**DateTimeFormatter**](/uwp/api/windows.globalization.datetimeformatting?branch=live) 클래스의 설명 섹션을 참조 하세요.

## <a name="the-difference-between-format-templates-and-format-patterns"></a>서식 템플릿과 형식 패턴의 차이점

서식 템플릿은 문화권을 구분 하지 않는 형식 문자열입니다. 따라서 서식 템플릿을 사용 하 여 **DateTimeFormatter** 를 생성 하는 경우 포맷터는 현재 언어에 맞는 올바른 순서로 형식 구성 요소를 표시 합니다. 반대로 형식 패턴은 문화권에 맞게 적용 됩니다. 형식 패턴을 사용 하 여 **DateTimeFormatter** 를 생성 하는 경우 포맷터는 지정 된 대로 패턴을 정확 하 게 사용 합니다. 따라서 패턴은 문화권에서 necesssarily 유효 하지 않습니다.

예제와의 차이점을 살펴보겠습니다. 간단한 서식 지정 템플릿 (패턴 아님)을 **DateTimeFormatter** 생성자에 전달 합니다. 서식 템플릿 "month day"입니다.

```csharp
var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
```

이렇게 하면 현재 컨텍스트의 언어 및 지역 값을 기반으로 포맷터가 만들어집니다. 서식 템플릿에서 구성 요소의 순서는 중요 하지 않습니다. 포맷터는 현재 언어에 대 한 올바른 순서로 이러한 항목을 표시 합니다. 따라서 영어 (미국)의 경우 "1 월 1 일", 프랑스어 (프랑스)의 경우 "1 janvier", 일본어의 경우 "1 月 1 日"를 표시 합니다.

반면 형식 패턴은 문화권에 따라 다릅니다. 서식 템플릿에 대 한 형식 패턴에 액세스 하겠습니다.

```csharp
IReadOnlyList<string> monthDayPatterns = dateFormatter.Patterns;
```

이는 런타임 언어와 지역에 따라 다른 결과를 생성 합니다. 여러 지역에서 다른 구성 요소를 사용 하거나 추가 문자 및 간격 없이 다른 순서로 사용할 수 있습니다.

```syntax
En-US: "{month.full} {day.integer}"
Fr-FR: "{day.integer} {month.full}"
Ja-JP: "{month.integer}月{day.integer}日"
```

위의 예제에서는 문화권을 구분 하지 않는 형식 문자열을 입력, 문화권별 형식 문자열 (를 호출한 경우 적용 되는 언어 및 지역의 함수 임)을 다시 가져왔습니다 `dateFormatter.Patterns` . 따라서 문화권별 형식 패턴에서 **DateTimeFormatter** 을 생성 하는 경우 특정 언어/지역에 대해서만 유효 합니다.

```csharp
var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("{month.full} {day.integer}");
```

위의 포맷터는 대괄호 안의 개별 구성 요소에 대 한 문화권별 값을 반환 합니다 {} . 하지만 형식 패턴의 구성 요소 순서는 고정입니다. 사용자가 필요한 것을 정확 하 게 확인할 수 있습니다 .이 경우 문화권이 적절 하거나 적절 하지 않을 수도 있습니다. 이 포맷터는 영어 (미국)에 유효 하지만 프랑스어 (프랑스) 또는 일본어의 경우 유효 하지 않습니다.

``` syntax
En-US: January 1
Fr-FR: janvier 1 (inappropriate for France; non-standard order)
Ja-JP: 1月1 (inappropriate for Japan; the day symbol 日 is missing)
```

또한 오늘 올바른 패턴은 나중에 정확 하지 않을 수 있습니다. 국가 또는 지역에서 서식 템플릿을 변경 하는 일정 시스템이 변경 될 수 있습니다. Windows는 이러한 변경 내용을 수용 하기 위해 서식 템플릿에 기반한 포맷터의 출력을 업데이트 합니다. 따라서 다음 조건 중 하나 이상 에서만 패턴 구문을 사용 해야 합니다.

-   특정 형식에 대 한 특정 출력에 종속 되지 않습니다.
-   일부 문화권 특정 표준을 따르도록 형식이 필요 하지 않습니다.
-   문화권에 따라 패턴이 고정 되도록 의도 합니다.
-   실제 형식 패턴 문자열 자체를 지역화 하려고 합니다.

서식 템플릿과 형식 패턴의 차이에 대 한 요약은 다음과 같습니다.

**서식 템플릿 (예: "월 일")**

-   월, 일 등의 값을 순서에 관계 없이 추출 된 [DateTime](/uwp/api/windows.foundation.datetime?branch=live) 형식의 표현입니다.
-   Windows에서 지 원하는 모든 언어 지역 값에 대해 유효한 표준 형식을 반환 하도록 보장 됩니다.
-   지정 된 언어 지역에 대해 문화권에 맞게 형식이 지정 된 문자열을 제공 하도록 보장 됩니다.
-   구성 요소의 일부 조합이 유효 하지 않습니다. 예를 들어 "dayofweek day"는 유효 하지 않습니다.

**형식 패턴 (예: "{month. full} {day. 정수}")**

-   전체 월 이름, 공백, 해당 순서 또는 지정 된 특정 형식 패턴을 차례로 나타내는 문자열을 명시적으로 정렬 합니다.
-   는 언어 지역 쌍에 대 한 올바른 표준 형식과 일치 하지 않을 수 있습니다.
-   문화권이 적절 하지 않을 수도 있습니다.
-   모든 구성 요소 조합은 순서에 관계 없이 지정할 수 있습니다.

## <a name="examples"></a>예

현재 월과 날짜를 현재 시간과 함께 특정 형식으로 표시 하려는 경우를 가정해 보겠습니다. 예를 들어 미국 영어 사용자에 게 다음과 같은 내용이 표시 될 수 있습니다.

``` syntax
June 25 | 1:38 PM
```

날짜 부분은 "month day" 서식 템플릿에 해당 하며 time 부분은 "hour minute" 서식 템플릿에 해당 합니다. 따라서 관련 날짜 및 시간 서식 템플릿에 대 한 포맷터를 구성한 다음 지역화할 수 있는 형식 문자열을 사용 하 여 해당 출력을 함께 연결할 수 있습니다.

```csharp
var dateToFormat = System.DateTime.Now;
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();

var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
var timeFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("hour minute");

var date = dateFormatter.Format(dateToFormat);
var time = timeFormatter.Format(dateToFormat);

string output = string.Format(resourceLoader.GetString("CustomDateTimeFormatString"), date, time);
```

`CustomDateTimeFormatString` 는 리소스 파일 (. resw)에서 지역화할 수 있는 리소스를 참조 하는 리소스 식별자입니다. 영어 (미국)의 기본 언어의 경우이 값은 "|" 값, " {0} {1} {0} "가 날짜이 고 ""가 시간 임을 나타내는 주석과 함께 설정 됩니다 {1} . 이렇게 하면 변환기에서 필요에 따라 형식 항목을 조정할 수 있습니다. 예를 들어 일부 언어 또는 지역에서 시간이 날짜 보다 앞에 오도록 하는 경우 항목의 순서를 변경할 수 있습니다. 또는 "|"를 다른 구분 문자로 바꿀 수 있습니다.

이 예제를 구현 하는 또 다른 방법은 형식 패턴에 대해 두 포맷터를 쿼리하고 이러한 포맷터를 함께 연결 하 고 결과 형식 패턴에서 세 번째 포맷터를 생성 하는 것입니다.

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();

var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
var timeFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("hour minute");

string dateFormatterPattern = dateFormatter.Patterns[0];
string timeFormatterPattern = timeFormatter.Patterns[0];

string pattern = string.Format(resourceLoader.GetString("CustomDateTimeFormatString"), dateFormatterPattern, timeFormatterPattern);

var patternFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter(pattern);

string output = patternFormatter.Format(System.DateTime.Now);
```

## <a name="important-apis"></a>중요 API

* [Windows.Globalization.DateTimeFormatting](/uwp/api/windows.globalization.datetimeformatting?branch=live)
* [DateTimeFormatter](/uwp/api/windows.globalization.datetimeformatting?branch=live)
* [DateTime](/uwp/api/windows.foundation.datetime?branch=live)

## <a name="related-topics"></a>관련 항목

* [날짜 및 시간 형식 지정 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Date%20and%20time%20formatting%20sample%20(Windows%208))
