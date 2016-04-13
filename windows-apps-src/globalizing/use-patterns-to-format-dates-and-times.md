---
정확히 개발자가 원하는 날짜 및 시간을 표시하려면 Windows.Globalization.DateTimeFormatting API와 사용자 지정 패턴을 사용하세요.
날짜 및 시간 형식 지정 패턴 사용
ms.assetid: 012028B3-9DA2-4E72-8C0E-3E06BEC3B3FE
날짜 및 시간 형식 지정 패턴 사용
template: detail.hbs
---

# 날짜 및 시간 형식 지정 패턴 사용


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


**중요 API**

-   [**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)
-   [**DateTimeFormatter**](https://msdn.microsoft.com/library/windows/apps/br206828)
-   [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576)

정확히 개발자가 원하는 날짜 및 시간을 표시하려면 [**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859) API와 사용자 지정 패턴을 사용하세요.

## <span id="Introduction"> </span> <span id="introduction"> </span> <span id="INTRODUCTION"> </span>소개


[
            **Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)은 세계 여러 국가와 언어에 맞게 적절하게 날짜 및 시간의 형식을 지정하는 다양한 방법을 제공합니다. 연도, 월, 일 등에 대한 표준 형식을 사용하거나 "자세한 날짜" 또는 "월 일"과 같은 표준 문자열 템플릿을 사용할 수 있습니다.

그러나 표시하려는 [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576) 문자열 구성 요소의 순서 및 형식을 좀더 제어하려는 경우 "패턴"이라는 문자열 템플릿 매개 변수에 대한 특수 구문을 사용할 수 있습니다. 패턴 구문을 사용하면 **DateTime** 개체의 개별 구성 요소인 월 이름만 또는 연도 값만을 가져와 직접 선택한 사용자 지정 형식으로 표시할 수 있습니다. 또한 다른 언어 및 지역에 맞게 패턴을 지역화할 수 있습니다.

**참고** 이는 형식 패턴의 개요입니다. 형식 템플릿 및 형식 패턴에 대한 자세한 내용은 [**DateTimeFormatter**](https://msdn.microsoft.com/library/windows/apps/br206828) 클래스의 설명 섹션을 참조하세요.

 

## <span id="What_you_need_to_know"> </span> <span id="what_you_need_to_know"> </span> <span id="WHAT_YOU_NEED_TO_KNOW"> </span>알아야 할 사항


패턴을 사용할 때는 해당 문화권에서 유효한 것이라고 보장할 수 없는 사용자 지정 형식을 작성 중이라는 점을 염두에 두어야 합니다. "월 일" 템플릿을 예로 들어 보겠습니다.

**C#**
```CSharp
var datefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
```
**JavaScript**
```JavaScript
var datefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
```

이 템플릿은 현재 컨텍스트의 언어와 지역 값을 기반으로 포맷터를 만듭니다. 따라서 적절한 글로벌 형식으로 월과 일을 항상 함께 표시합니다. 예를 들어, 영어(미국)는 "January 1"이고, 프랑스어(프랑스)는 "1 janvier"이며, 일본어(일본)의 경우는 "1月1日"입니다. 이는 템플릿이 패턴 속성을 통해 액세스할 수 있는 문화권별 패턴 문자열을 기반으로 하기 때문입니다.

**C#**
```CSharp
var monthdaypattern = datefmt.Patterns;
```
**JavaScript**
```JavaScript
var monthdaypattern = datefmt.patterns;
```

이 코드는 포맷터의 언어와 국가에 따라 다른 결과를 만듭니다. 지역별로 다른 구성 요소를 다른 순서로, 추가 문자 및 공백을 포함하거나 제외하여 사용할 수 있습니다.

``` syntax
En-US: "{month.full} {day.integer}"
Fr-FR: "{day.integer} {month.full}"
Ja-JP: "{month.integer}月{day.integer}日"
```

패턴을 사용하여 사용자 지정 [**DateTimeFormatter**](https://msdn.microsoft.com/library/windows/apps/br206828)를 생성할 수 있습니다. 예를 들어 다음 코드는 미국 영어 패턴을 기반으로 합니다.

**C#**
```CSharp
var datefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("{month.full} {day.integer}");
```
**JavaScript**
```JavaScript
var datefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("{month.full} {day.integer}");
```

Windows에서는 개별 구성 요소에 대한 문화권별 값을 대괄호 {}로 묶어 반환합니다. 그러나 패턴 구문을 사용하면 구성 요소 순서가 변하지 않습니다. 원하는 정확한 결과를 얻게 되지만 문화적으로 적절하지 않을 수 있습니다.

``` syntax
En-US: January 1
Fr-FR: janvier 1 (inappropriate for France; non-standard order)
Ja-JP: 1月1 (inappropriate for Japan; the day symbol is missing)
```

또한 패턴은 시간에 따라 일관된 상태를 유지하도록 보장되지 않습니다. 국가나 지역은 달력 시스템을 변경할 수 있으며, 그에 따라 형식 템플릿이 수정됩니다. Windows 업데이트는 그러한 변경 사항을 수용하도록 포맷터의 출력을 업데이트합니다. 따라서 다음과 같은 경우에는 [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576)의 형식을 지정하는 데 패턴 구문만 사용해야 합니다.

-   형식에 대한 특정 출력에 의존하지 않는 경우
-   형식이 일부 문화권별 표준을 따르지 않아도 되는 경우
-   서로 다른 문화권에서 패턴이 변하지 않도록 특별히 의도한 경우
-   패턴을 지역화하려는 경우

표준 문자열 템플릿과 비표준 문자열 패턴의 차이점을 요약하면 다음과 같습니다.

**문자열 템플릿(예: "월 일")**

-   월과 일에 대한 값을 특정 순서로 포함하는 [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576) 형식의 추상적 표현입니다.
-   Windows에서 지원하는 모든 언어-국가 값에 대해 유효한 표준 형식을 반환하도록 보장됩니다.
-   지정된 언어-국가에 대해 문화권에 알맞게 형식이 지정된 문자열을 제공하도록 보장됩니다.
-   모든 구성 요소의 조합이 유효하지는 않습니다. 예를 들어 "요일 일"에 대한 문자열 템플릿은 없습니다.

**문자열 패턴(예: "{month.full} {day.integer}")**

-   전체 월 이름, 공백, 일 정수순으로 나타내도록 명시적으로 순서가 지정된 문자열입니다.
-   언어-국가 쌍에 대해 유효한 표준 형식에 해당하지 않을 수 있습니다.
-   문화권에 맞게 표현되도록 보장되지 않습니다.
-   구성 요소의 조합은 임의의 순서로 지정될 수 있습니다.

## <span id="Tasks"> </span> <span id="tasks"> </span> <span id="TASKS"> </span>작업


현재 월 및 일과 함께 현재 시간을 특정 형식으로 표시하려는 상황을 가정해 보세요. 예를 들면, 미국 영어 사용자에게는 다음과 같이 표시되어야 합니다.

``` syntax
June 25 | 1:38 PM
```

날짜 부분은 "월 일" 템플릿에 해당하고, 시간 부분은 "시간 분" 템플릿에 해당합니다. 따라서 해당 템플릿을 구성하는 패턴을 연결하는 사용자 지정 형식을 만들 수 있습니다.

먼저 관련된 날짜 및 시간 템플릿에 대한 포맷터를 가져온 다음 해당 템플릿의 패턴을 가져옵니다.

**C#**
```CSharp
// Get formatters for the date part and the time part.
var mydate = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
var mytime = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("hour minute");

// Get the patterns from these formatters.
var mydatepattern = mydate.Patterns[0];
var mytimepattern = mytime.Patterns[0];
```
**JavaScript**
```JavaScript
// Get formatters for the date part and the time part.
var dtf = Windows.Globalization.DateTimeFormatting;
var mydate = dtf.DateTimeFormatter("month day");
var mytime = dtf.DateTimeFormatter("hour minute");

// Get the patterns from these formatters.
var mydatepattern = mydate.patterns[0];
var mytimepattern = mytime.patterns[0];
```

사용자 지정 형식은 지역화가 가능한 리소스 문자열로 저장해야 합니다. 예를 들어, 영어(미국)에 대한 문자열은 "{date} | {time}"입니다. 지역화 담당자는 이 문자열을 필요에 맞게 조정할 수 있습니다. 예를 들어, 일부 언어나 지역에서 시간이 날짜 앞에 오는 것이 더 자연스럽게 느껴지면 구성 요소의 순서를 변경할 수 있습니다. 또는 "|"를 다른 구분 기호 문자로 바꿀 수 있습니다. 런타임에 문자열의 {date} 및 {time} 부분을 관련 패턴으로 바꿉니다.

**C#**
```CSharp
// Assemble the custom pattern. This string comes from a resource, and should be localizable. 
var resourceLoader = new Windows.ApplicationModel.Resources.ResourceLoader();
var mydateplustime = resourceLoader.GetString("date_plus_time");
mydateplustime = mydateplustime.replace("{date}", mydatepattern);
mydateplustime = mydateplustime.replace("{time}", mytimepattern);
```
**JavaScript**
```JavaScript
// Assemble the custom pattern. This string comes from a resource, and should be localizable. 
var mydateplustime = WinJS.Resources.getString("date_plus_time");
mydateplustime = mydateplustime.replace("{date}", mydatepattern);
mydateplustime = mydateplustime.replace("{time}", mytimepattern);
```

이제 사용자 지정 패턴을 기반으로 새로운 포맷터를 구성할 수 있습니다.

**C#**
```CSharp
// Get the custom formatter.
var mydateplustimefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter(mydateplustime);
```
**JavaScript**
```JavaScript
// Get the custom formatter.
var mydateplustimefmt = new dtf.DateTimeFormatter(mydateplustime);
```

## <span id="related_topics"> </span>관련 항목


* [날짜 및 시간 형식 지정 샘플](http://go.microsoft.com/fwlink/p/?LinkId=231618)
* [**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)
* [**Windows.Foundation.DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576)
 

 





<!--HONumber=Mar16_HO1-->


