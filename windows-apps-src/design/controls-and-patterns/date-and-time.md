---
Description: 날짜 및 시간 컨트롤을 통해 날짜 및 시간을 보고 설정할 수 있습니다. 이 문서는 디자인 지침을 제공하며, 올바른 컨트롤을 선택하는 데 도움이 됩니다.
title: 날짜 및 시간 컨트롤에 대한 지침
ms.assetid: 4641FFBB-8D82-4290-94C1-D87617997F61
label: Calendar, date, and time controls
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 13901e044cbf6a14ac0ede6e9ed0f451859e49a1
ms.sourcegitcommit: 4fdab7be28aca18cb3879fc205eb49edc4f9a96b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77629154"
---
# <a name="calendar-date-and-time-controls"></a>달력, 날짜 및 시간 컨트롤

 

날짜 및 시간 컨트롤은 사용자가 앱에서 날짜 및 시간 값을 보고 설정할 수 있는 표준적이고 지역화된 방법을 제공합니다. 이 문서는 디자인 지침을 제공하며, 올바른 컨트롤을 선택하는 데 도움이 됩니다.

> **중요 API**: [CalendarView 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView), [CalendarDatePicker 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarDatePicker), [DatePicker 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePicker), [TimePicker 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePicker)

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/category/DataInput">앱을 열고 컨트롤이 작동하는 모습을 확인</a>하세요.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="which-date-or-time-control-should-you-use"></a>어떤 날짜 또는 시간 컨트롤을 사용해야 하나요?

시나리오에 따라 선택할 수 있는 4개의 날짜 및 시간 컨트롤이 있습니다. 이 정보를 사용하여 앱에서 사용할 올바른 컨트롤을 선택합니다.

&nbsp;|&nbsp;|&nbsp;                                                                                                                      
--------------------|-------|-------------------------------------------------------------------------------------------------------------------------------
달력 보기       |![달력 보기의 예](images/controls_calendar_monthview_small.png)|항상 표시되는 달력에서 단일 날짜 또는 날짜 범위를 선택하는 데 사용합니다.                   
달력 날짜 선택|![달력 날짜 선택의 예](images/calendar-date-picker-closed.png)|상황별 달력에서 단일 날짜를 선택하는 데 사용합니다. 
날짜 선택기         |![날짜 선택의 예](images/date-picker-closed.png)|상황별 정보가 중요하지 않은 알려진 단일 날짜를 선택하는 데 사용합니다.
시간 선택기         |![시간 선택기의 예](images/time-picker-closed.png)|단일 시간 값을 선택하는 데 사용합니다.                                        

<!-- This table seems redundant, not sure it's needed.-->

### <a name="calendar-view"></a>달력 보기

**CalendarView**를 통해 월, 연도 또는 10년 단위로 이동하면서 달력을 보고 조작할 수 있습니다. 사용자는 단일 날짜 또는 날짜 범위를 선택할 수 있습니다. 선택 화면이 없고 달력이 항상 표시됩니다.

달력 보기는 3개의 개별적인 보기(월 보기, 연도 보기 및 10년 보기)로 구성됩니다. 기본적으로 월 보기가 열리면서 시작하지만 시작 보기로 다른 보기를 지정할 수 있습니다.

![달력 날짜 선택의 예](images/calendar-view-3-views.png)

- 사용자가 여러 날짜를 선택할 수 있도록 해야 하는 경우 **CalendarView**를 사용해야 합니다.
- 사용자가 단일 날짜만 선택하도록 하고 달력을 항상 표시할 필요가 없는 경우 **CalendarDatePicker** 또는 **DatePicker** 컨트롤을 사용하는 것이 좋습니다.

### <a name="calendar-date-picker"></a>달력 날짜 선택

**CalendarDatePicker**는 요일이나 일정의 예약률과 같이 컨텍스트 정보가 중요한 달력 보기에서 단일 날짜를 선택하는 데 최적화된 드롭다운 컨트롤입니다. 달력을 수정하여 추가 컨텍스트를 제공하거나 사용할 수 있는 날짜를 제한할 수 있습니다.

날짜를 설정하지 않으면 진입점에 개체 틀 텍스트가 표시되며, 그러지 않으면 선택한 날짜가 표시됩니다. 사용자가 진입점을 선택하면 사용자가 날짜를 선택할 수 있도록 달력 보기가 확장됩니다. 이 달력 보기는 다른 UI에 겹쳐지며 다른 UI를 보이지 않도록 합니다.

![달력 날짜 선택의 예](images/calendar-date-picker-2-views.png)

- 약속 또는 출발 날짜 선택 등에 달력 날짜 선택을 사용합니다. 

### <a name="date-picker"></a>날짜 선택기

**DatePicker** 컨트롤은 특정 날짜를 선택할 수 있는 표준화된 방법을 제공합니다. 

진입점에 선택한 날짜가 표시되고 사용자가 진입점을 선택하면 선택할 수 있게 선택 화면이 가운데에서 세로로 확장됩니다. 이 날짜 선택 컨트롤은 다른 UI에 겹쳐지며 다른 UI를 보이지 않도록 합니다.

![날짜 선택 확장 예](images/controls_datepicker_expand.png)

- 날짜 선택을 사용하여 사용자는 달력의 컨텍스트가 중요하지 않은 생일과 같은 알려진 날짜를 선택할 수 있습니다.

### <a name="time-picker"></a>시간 선택기

**TimePicker**는 약속 또는 출발 시간 등에 대한 단일 시간 값을 선택하는 데 사용합니다. 사용자가 설정하거나 코드에 설정하는 정적 디스플레이이지만 현재 시간을 표시하도록 업데이트되지는 않습니다.

진입점에 선택한 시간이 표시되고 사용자가 진입점을 선택하면 선택할 수 있게 선택기 화면이 가운데에서 세로로 확장됩니다. 이 시간 선택 컨트롤은 다른 UI에 겹쳐지며 다른 UI를 보이지 않도록 합니다.

![확장되는 시간 선택기의 예](images/controls_timepicker_expand.png)

- 사용자가 단일 시간 값을 선택할 수 있게 하려면 시간 선택기를 사용합니다.

## <a name="create-a-date-or-time-control"></a>날짜 또는 시간 컨트롤 만들기

각 날짜 및 시간 컨트롤과 관련된 정보와 예제는 다음 문서를 참조하세요.

- [달력 보기](calendar-view.md)
- [달력 날짜 선택](calendar-date-picker.md)
- [날짜 선택기](date-picker.md)
- [시간 선택기](time-picker.md)

### <a name="globalization"></a>세계화

XAML 날짜 컨트롤은 Windows에서 지원하는 각 일정 시스템을 지원합니다. 이러한 일정은 [Windows.Globalization.CalendarIdentifiers](https://docs.microsoft.com/uwp/api/Windows.Globalization.CalendarIdentifiers) 클래스에서 지정됩니다. 각 컨트롤에서 앱의 기본 언어에 대한 올바른 일정 시스템이 사용되거나 **CalendarIdentifier** 속성을 설정하여 특정 일정 시스템을 사용할 수 있습니다.

시간 선택 컨트롤은 [Windows.Globalization.ClockIdentifiers](https://docs.microsoft.com/uwp/api/Windows.Globalization.ClockIdentifiers) 클래스에 지정된 각 시계 시스템을 지원합니다. [ClockIdentifier](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.timepicker.clockidentifier) 속성을 설정하여 12시간제 시계 또는 24시간제 시계를 사용할 수 있습니다. 속성의 형식이 문자열이지만 ClockIdentifiers 클래스의 정적 문자열 속성에 해당하는 값을 사용해야 합니다. 옵션은 다음과 같습니다. TwelveHour(문자열 "12HourClock") 및 TwentyFourHour(문자열 "24HourClock") "12HourClock"이 기본값입니다.

### <a name="datetime-and-calendar-values"></a>DateTime 및 Calendar 값

XAML 날짜 및 시간 컨트롤에 사용된 날짜 개체는 프로그래밍 언어에 따라 서로 다른 구조를 가집니다.

- C# 및 Visual Basic은 .NET의 일부인 [System.DateTimeOffset](https://docs.microsoft.com/dotnet/api/system.datetimeoffset) 구조를 사용합니다. 
- C++/CX는 [Windows::Foundation::DateTime](https://docs.microsoft.com/windows/desktop/api/windows.foundation/ns-windows-foundation-datetime) 구조를 사용합니다. 

관련 개념은 날짜가 컨텍스트에서 해석되는 방식에 영향을 주는 Calendar 클래스입니다. 모든 Windows 런타임 앱은 [Windows.Globalization.Calendar](https://docs.microsoft.com/uwp/api/Windows.Globalization.Calendar) 클래스를 사용할 수 있습니다. C# 및 Visual Basic 앱은 매우 유사한 기능을 가진 [System.Globalization.Calendar](https://docs.microsoft.com/dotnet/api/system.globalization.calendar) 클래스를 사용할 수도 있습니다. (Windows 런타임 앱은 기본 .NET Calendar 클래스를 사용할 수 있으나 GregorianCalendar와 같은 특정 구현은 사용할 수 없습니다.)

.NET은 또한 암시적으로 [DateTimeOffset](https://docs.microsoft.com/dotnet/api/system.datetimeoffset)으로 변환될 수 있는 [DateTime](https://docs.microsoft.com/dotnet/api/system.datetime)이라는 형식을 지원합니다. 따라서 실제로 DateTimeOffset인 값을 설정하는 데 사용되는 .NET 코드에서 "DateTime" 형식이 사용되는 것을 볼 수 있습니다. DateTime과 DateTimeOffset 간의 차이점에 대한 자세한 내용은 [DateTimeOffset](https://docs.microsoft.com/dotnet/api/system.datetimeoffset) 클래스의 설명을 참조하세요.

> [!NOTE]
> Windows 런타임 XAML 파서에는 문자열을 DateTime/DateTimeOffset 개체인 날짜로 변환하는 변환 논리가 없기 때문에 날짜 개체를 사용하는 속성은 XAML 특성 문자열로 설정할 수 없습니다. 일반적으로 코드에서 이러한 값을 설정합니다. 가능한 다른 기술은 사용할 수 있는 날짜를 날짜 개체로 또는 데이터 컨텍스트에서 정의한 다음, 속성을 XAML 특성(날짜를 데이터로 액세스할 수 있는 [\{Binding\} 태그 확장](../../xaml-platform/binding-markup-extension.md) 식 참조)으로 설정하는 것입니다.

## <a name="get-the-sample-code"></a>샘플 코드 가져오기

- [XAML UI 기본 사항 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics)
- [Calendar 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Calendar)
- [날짜 및 시간 형식 지정 샘플](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/DateTimeFormatting)

## <a name="related-topics"></a>관련 항목

### <a name="for-developers-xaml"></a>개발자용(XAML)

- [CalendarView 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView)
- [CalendarDatePicker 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarDatePicker)
- [DatePicker 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePicker)
- [TimePicker 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePicker)
