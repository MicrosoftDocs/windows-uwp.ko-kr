---
author: stevewhims
Description: Use the Windows.Globalization.DateTimeFormatting API with custom templates and patterns to display dates and times in exactly the format you wish.
title: 날짜 및 시간 형식 지정 패턴 사용
ms.assetid: 012028B3-9DA2-4E72-8C0E-3E06BEC3B3FE
label: Use patterns to format dates and times
template: detail.hbs
ms.author: stwhi
ms.date: 11/09/2017
ms.topic: article
keywords: windows 10, uwp, 세계화, 현지화, 지역화
ms.localizationpriority: medium
ms.openlocfilehash: 04a0288d0b28c12eb68cf56225747224e8df9777
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6852669"
---
# <a name="use-templates-and-patterns-to-format-dates-and-times"></a>날짜 및 시간 형식 지정 템플릿 및 패턴 사용

날짜 및 시간을 원하는 형식으로 정확히 표시하려면 [**Windows.Globalization.DateTimeFormatting**](/uwp/api/windows.globalization.datetimeformatting?branch=live) 네임스페이스와 사용자 지정 템플릿에 클래스를 사용하세요.

## <a name="introduction"></a>소개

[**DateTimeFormatter**](/uwp/api/windows.globalization.datetimeformatting?branch=live) 클래스는 세계 여러 국가와 언어에 맞게 적절하게 날짜 및 시간의 형식을 지정하는 다양한 방법을 제공합니다. 년, 월, 일 등에 표준 형식을 사용할 수 있습니다. 또는 **DateTimeFormatter** 생성자의 *formatTemplate* 인수에 "긴 날짜 형식" 또는 "월 일"과 같은 형식 템플릿을 전달할 수 있습니다.

그러나 표시하려는 [**DateTime**](/uwp/api/windows.foundation.datetime?branch=live) 개체 구성 요소의 순서 및 형식을 조금 더 제어하려는 경우 생성자의 *formatTemplate* 인수에 형식 패턴을 전달할 수 있습니다. 형식 패턴은 특수 구문을 사용하므로 **DateTime** 개체의 개별 구성 요소인 월 이름만 또는 연도 값만을 가져와 직접 선택한 사용자 지정 형식으로 표시할 수 있습니다. 또한 다른 언어 및 지역에 맞게 패턴을 지역화할 수 있습니다.

**참고**형식 패턴의 개요 일 뿐입니다. 형식 템플릿 및 형식 패턴에 대한 자세한 내용은 [**DateTimeFormatter**](/uwp/api/windows.globalization.datetimeformatting?branch=live) 클래스의 설명 섹션을 참조하세요.

## <a name="the-difference-between-format-templates-and-format-patterns"></a>형식 템플릿과 형식 패턴의 차이점

형식 템플릿은 문화권의 영향을 받지 않는 형식 문자열입니다. 따라서 형식 템플릿을 사용하여 **DateTimeFormatter**를 생성하는 경우 포맷터는 현재 언어에 올바른 순서로 형식 구성 요소를 표시합니다. 반대로 형식 패턴은 문화권별로 다릅니다. 형식 템플릿을 사용하여 **DateTimeFormatter**를 구성하면 포맷터가 제공된 것과 동일한 패턴을 사용합니다. 따라서 패턴이 문화권 간에 반드시 유효한 것은 아닙니다.

이러한 차이점을 예를 들어 설명해 봅시다. 패턴이 아닌 간단한 형식 템플릿을 **DateTimeFormatter** 생성자에 전달해 보겠습니다. 이는 "월 일" 형식 템플릿입니다.

```csharp
var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
```

이 템플릿은 현재 컨텍스트의 언어와 지역 값을 기반으로 포맷터를 만듭니다. 형식 템플릿의 구성 요소 순서는 상관이 없습니다. 포맷터는 현재 언어에 맞는 순서로 표시합니다. 따라서 영어(미국)는 "January 1"이고, 프랑스어(프랑스)는 "1 janvier"이며, 일본어(일본)의 경우는 "1月1日"입니다.

반면 형식 패턴은 문화권마다 다릅니다. 형식 템플릿의 형식 패턴에 액세스해 봅시다.

```csharp
IReadOnlyList<string> monthDayPatterns = dateFormatter.Patterns;
```

이 코드는 런타임 언어와 국가에 따라 다른 결과를 만듭니다. 지역별로 다른 구성 요소를 다른 순서로, 추가 문자 및 공백을 포함하거나 제외하여 사용할 수 있습니다.

```syntax
En-US: "{month.full} {day.integer}"
Fr-FR: "{day.integer} {month.full}"
Ja-JP: "{month.integer}月{day.integer}日"
```

위 예에서는 문화권별 형식 문자를을 입력했으며 문화권별 형식 문자열을 받았습니다(`dateFormatter.Patterns`를 호출할 때 효력이 발생하는 언어와 지역의 기능). 따라서 문화권별 형식 패턴에서 **DateTimeFormatter**를 생성할 경우 특정 언어/지역에 대해서만 유효합니다.

```csharp
var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("{month.full} {day.integer}");
```

위의 포맷터는 괄호 안에 개별 구성 요소에 대 한 문화권별 값을 반환 합니다. {}. 그러나 형식 패턴의 구성 요소 순서는 고정되어 있습니다. 원하는 정확한 결과를 얻게 되지만 문화적으로 적절할 수도 있고 그렇지 않을 수 있습니다. 이 포맷터는 영어(미국)에는 유효하지만 프랑스어(프랑스)나 일본어에는 유효하지 않습니다.

``` syntax
En-US: January 1
Fr-FR: janvier 1 (inappropriate for France; non-standard order)
Ja-JP: 1月1 (inappropriate for Japan; the day symbol 日 is missing)
```

또한 현재 올바른 패턴이 나중에는 올바르지 않을 수 있습니다. 국가나 지역은 달력 시스템을 변경할 수 있으며, 그에 따라 형식 템플릿이 수정됩니다. Windows 업데이트는 그러한 변경 사항을 수용하도록 형식 템플릿을 기반으로 포맷터의 출력을 업데이트합니다. 따라서 하나 이상의 이러한 조건을 준수하여 패턴 구문을 사용해야만 합니다.

-   형식에 대한 특정 출력에 의존하지 않는 경우
-   형식이 일부 문화권별 표준을 따르지 않아도 되는 경우
-   서로 다른 문화권에서 패턴이 변하지 않도록 특별히 의도한 경우
-   실제 형식 패턴 문자열을 지역화하려고 합니다.

형식 템플릿과 형식 패턴 간의 차이점을 다음과 같이 요약할 수 있습니다.

**"월 일"과 같은 형식 템플릿**

-   월과 일에 대한 값을 특정 순서로 포함하는 [DateTime](/uwp/api/windows.foundation.datetime?branch=live) 형식의 추상적 표현입니다.
-   Windows에서 지원하는 모든 언어-국가 값에 대해 유효한 표준 형식을 반환하도록 보장됩니다.
-   지정된 언어-국가에 대해 문화권에 알맞게 형식이 지정된 문자열을 제공하도록 보장됩니다.
-   모든 구성 요소의 조합이 유효하지는 않습니다. 예를 들어 "요일 일"은 유효하지 않습니다.

**형식 패턴(예: "{month.full} {day.integer}")**

-   전체 월 이름, 공백, 일 정수순으로 또는 지정하는 특정 형식 패턴으로 나타내도록 명시적으로 순서가 지정된 문자열입니다.
-   언어-국가 쌍에 대해 유효한 표준 형식에 해당하지 않을 수 있습니다.
-   문화권에 맞게 표현되도록 보장되지 않습니다.
-   구성 요소의 조합은 임의의 순서로 지정될 수 있습니다.

## <a name="examples"></a>예

현재 월 및 일과 함께 현재 시간을 특정 형식으로 표시하려는 상황을 가정해 보세요. 예를 들면, 미국 영어 사용자에게는 다음과 같이 표시되어야 합니다.

``` syntax
June 25 | 1:38 PM
```

날짜 부분은 "월 일" 형식 템플릿에 해당하고, 시간 부분은 "시간 분" 형식 템플릿에 해당합니다. 따라서 관련 날짜 및 시간 형식 템플릿에 대한 포맷터를 생성한 후 지역화 가능한 형식 문자열을 사용하여 출력을 연결할 수 있습니다.

```csharp
var dateToFormat = System.DateTime.Now;
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();

var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
var timeFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("hour minute");

var date = dateFormatter.Format(dateToFormat);
var time = timeFormatter.Format(dateToFormat);

string output = string.Format(resourceLoader.GetString("CustomDateTimeFormatString"), date, time);
```

`CustomDateTimeFormatString` 리소스 파일(.resw)에서 지역화할 리소스를 나타내는 리소스 ID입니다. 값으로 설정 됩니다이 기본 언어인 영어 (미국), "{0} | {1}"나타낸다는 설명과 함께"{0}"은 날짜 및"{1}"시간입니다. 이런 방식으로 번역자는 필요에 따라 형식 항목을 조정할 수 있습니다. 예를 들어, 일부 언어나 지역에서 시간이 날짜 앞에 오는 것이 더 자연스럽게 느껴지면 항목의 순서를 변경할 수 있습니다. 또는 "|"를 다른 구분 기호 문자로 바꿀 수 있습니다.

이 예를 구현하는 또다른 방법은 형식 패턴에 2개의 포맷터를 쿼리하고 함께 연결한 다음 결과 형식 패턴에서 세 번째 포맷터를 구성하는 것입니다.

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

* [날짜 및 시간 형식 지정 샘플](http://go.microsoft.com/fwlink/p/?LinkId=231618)