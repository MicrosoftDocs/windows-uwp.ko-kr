---
Description: A calendar view lets a user view and interact with a calendar that they can navigate by month, year, or decade.
title: 달력 보기
ms.assetid: d8ec5ba8-7a9d-405d-a1a5-5a1b502b9e64
label: Calendar view
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 9214ec2f2a1e352f94205675f2200920b8a00bd2
ms.sourcegitcommit: a60ab85e9f2f9690e0141050ec3aa51f18ec61ec
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/30/2019
ms.locfileid: "9037255"
---
# <a name="calendar-view"></a>달력 보기

달력 보기를 통해 월, 연도 또는 10년 단위로 이동하면서 달력을 보고 조작할 수 있습니다. 사용자는 단일 날짜 또는 날짜 범위를 선택할 수 있습니다. 선택 화면이 없고 달력이 항상 표시됩니다. 

> **중요 API**:  [CalendarView 클래스](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.aspx), [SelectedDatesChanged 이벤트](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.selecteddateschanged.aspx)


## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?
달력 보기를 사용하여 항상 표시되는 달력에서 단일 날짜 또는 날짜 범위를 선택할 수 있도록 합니다.

사용자가 한 번에 여러 날짜를 선택하도록 해야 할 경우 달력 보기를 사용해야 합니다. 사용자가 단일 날짜만 선택하도록 해야 하고 달력이 항상 표시될 필요가 없는 경우 [달력 날짜 선택](calendar-date-picker.md) 또는 [날짜 선택](date-picker.md) 컨트롤을 사용하는 것이 좋습니다.

올바른 컨트롤을 선택하는 방법에 대한 자세한 내용은 [날짜 및 시간 컨트롤](date-and-time.md) 문서를 참조하세요.

## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/CalendarView">앱을 열고 작동 중인 CalendarView를 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML 컨트롤 갤러리 앱 다운로드(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 다운로드(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

달력 보기는 3개의 개별적인 보기(월 보기, 연도 보기 및 10년 보기)로 구성됩니다. 기본적으로 월 보기가 열린 상태로 시작합니다. [DisplayMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.displaymode.aspx) 속성을 설정하여 시작 보기를 지정할 수 있습니다.

![달력 보기의 3가지 보기](images/calendar-view-3-views.png)

월 보기의 헤더를 클릭하여 연도 보기를 열고 연도 보기의 헤더를 클릭하여 10년 보기를 엽니다. 10년 보기의 연도를 선택하여 연도 보기로 돌아가고 연도 보기의 월을 선택하여 월 보기로 돌아갑니다. 헤더 측면의 두 개의 화살표는 월, 연도 또는 10년으로 앞뒤로 이동합니다. 

## <a name="create-a-calendar-view"></a>달력 보기 만들기

이 예제에서는 간단한 달력 보기를 만드는 방법을 보여 줍니다.

```xaml
<CalendarView/>
```

결과 달력 보기는 다음과 같습니다.

![달력 보기의 예](images/controls_calendar_monthview.png)

### <a name="selecting-dates"></a>날짜 선택

기본적으로 [SelectionMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.selectionmode.aspx) 속성은 **Single**로 설정됩니다. 이렇게 하면 사용자가 달력에서 단일 날짜를 선택할 수 있습니다. SelectionMode를 **None**으로 설정하여 날짜 선택을 사용하지 않도록 설정합니다. 

SelectionMode를 **Multiple**으로 설정하여 여러 날짜를 선택할 수 있도록 합니다. 다음과 같이 [DateTime](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetime.aspx)/[DateTimeOffset](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetimeoffset.aspx) 개체를 [SelectedDates](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.selecteddates.aspx) 컬렉션에 추가하여 프로그래밍 방식으로 여러 날짜를 선택할 수 있습니다.

```csharp
calendarView1.SelectedDates.Add(DateTimeOffset.Now);
calendarView1.SelectedDates.Add(new DateTime(1977, 1, 5));
```

달력 표에서 선택한 날짜를 클릭 또는 탭하여 선택 취소할 수 있습니다.

[SelectedDates](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.selecteddateschanged.aspx) 컬렉션이 변경되었을 때 알림을 받도록 [SelectedDatesChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.selecteddates.aspx) 이벤트를 처리할 수 있습니다.

> [!NOTE]
> 날짜 값에 대한 중요한 내용은 날짜 및 시간 컨트롤 문서의 [DateTime 및 Calendar 값](date-and-time.md#datetime-and-calendar-values)을 참조하세요.

### <a name="customizing-the-calendar-views-appearance"></a>달력 보기의 표시 형식 사용자 지정

달력 보기는 ControlTemplate에서 정의되는 XAML 요소와 컨트롤에서 직접 렌더링되는 시각적 요소로 구성됩니다. 
- 컨트롤 템플릿에서 정의되는 XAML 요소는 컨트롤, 헤더, 이전 및 다음 단추와 DayOfWeek 요소를 묶는 테두리를 포함합니다. 모든 XAML 컨트롤과 마찬가지로 이러한 요소의 스타일을 지정하고 다시 템플릿을 만들 수 있습니다. 
- 달력 표는 [CalendarViewDayItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarviewdayitem.aspx) 개체로 구성됩니다. 이러한 요소의 스타일을 지정하거나 템플릿을 다시 만들 수 없지만 표시 형식을 사용자 지정할 수 있도록 다양한 속성이 제공됩니다.

이 다이어그램은 달력의 월 보기를 구성하는 요소를 보여 줍니다. 자세한 내용은 [CalendarViewDayItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarviewdayitem.aspx) 클래스에 대한 설명을 참조하세요.

![달력 월 보기의 요소](images/calendar-view-month-elements.png)

이 표에 달력 요소의 표시 형식을 수정하기 위해 변경할 수 있는 속성이 나열됩니다.

요소 | 속성
--------|-----------
DayOfWeek | [DayOfWeekFormat](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.dayofweekformat.aspx)  
CalendarItem | [CalendarItemBackground](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.calendaritembackground.aspx), [CalendarItemBorderBrush](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.calendaritemborderbrush.aspx), [CalendarItemBorderThickness](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.calendaritemborderthickness.aspx), [CalendarItemForeground](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.calendaritemforeground.aspx)  
DayItem | [DayItemFontFamily](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.dayitemfontfamily.aspx), [DayItemFontSize](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.dayitemfontsize.aspx), [DayItemFontStyle](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.dayitemfontstyle.aspx), [DayItemFontWeight](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.dayitemfontweight.aspx), [HorizontalDayItemAlignment](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.horizontaldayitemalignment.aspx), [VerticalDayItemAlignment](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.verticaldayitemalignment.aspx), [CalendarViewDayItemStyle](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.calendarviewdayitemstyle.aspx)  
MonthYearItem(연도 및 10년 보기에서 DayItem과 같음) | [MonthYearItemFontFamily](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.monthyearitemfontfamily.aspx), [MonthYearItemFontSize](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.monthyearitemfontsize.aspx), [MonthYearItemFontStyle](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.monthyearitemfontstyle.aspx), [MonthYearItemFontWeight](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.monthyearitemfontweight.aspx)  
FirstOfMonthLabel | [FirstOfMonthLabelFontFamily](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontfamily.aspx), [FirstOfMonthLabelFontSize](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontsize.aspx), [FirstOfMonthLabelFontStyle](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontstyle.aspx), [FirstOfMonthLabelFontWeight](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontweight.aspx), [HorizontalFirstOfMonthLabelAlignment](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.horizontalfirstofmonthlabelalignment.aspx), [VerticalFirstOfMonthLabelAlignment](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.verticalfirstofmonthlabelalignment.aspx), [IsGroupLabelVisible](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.isgrouplabelvisible.aspx)  
FirstofYearDecadeLabel(연도 및 10년 보기에서 FirstOfMonthLabel과 같음) | [FirstOfYearDecadeLabelFontFamily](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontfamily.aspx), [FirstOfYearDecadeLabelFontSize](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontsize.aspx), [FirstOfYearDecadeLabelFontStyle](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontstyle.aspx), [FirstOfYearDecadeLabelFontWeight](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontweight.aspx)  
시각적 상태 테두리 | [FocusBorderBrush](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.focusborderbrush.aspx), [HoverBorderBrush](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.hoverborderbrush.aspx), [PressedBorderBrush](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.pressedborderbrush.aspx), [SelectedBorderBrush](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.selectedborderbrush.aspx), [SelectedForeground](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.selectedforeground.aspx), [SelectedHoverBorderBrush](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.selectedhoverborderbrush.aspx), [SelectedPressedBorderBrush](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.selectedpressedborderbrush.aspx)  
OutofScope | [IsOutOfScopeEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.isoutofscopeenabled.aspx), [OutOfScopeBackground](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.outofscopebackground.aspx), [OutOfScopeForeground](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.outofscopeforeground.aspx)  
오늘 | [IsTodayHighlighted](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.istodayhighlighted.aspx), [TodayFontWeight](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.todayfontweight.aspx), [TodayForeground](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.todayforeground.aspx)  

 기본적으로 월 보기에서 한 번에 6주가 표시됩니다. [NumberOfWeeksInView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.numberofweeksinview.aspx) 속성을 설정하여 표시되는 주 수를 변경할 수 있습니다. 표시할 주의 최소 수는 2이며 최대 수는 8입니다.

기본적으로 연도와 10년 보기는 4x4 표에 표시됩니다. 행 또는 열의 수를 변경하려면 원하는 수의 열과 행이 포함된 [SetYearDecadeDisplayDimensions](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.setyeardecadedisplaydimensions.aspx)를 호출합니다. 이렇게 하면 연도와 10년 보기에 대한 표가 변경됩니다.

여기서 연도와 10년 보기는 3x4 표에 표시되도록 설정됩니다.

```csharp
calendarView1.SetYearDecadeDisplayDimensions(3, 4);
```

기본적으로 달력 보기에 표시되는 최소 날짜는 현재 날짜 이전 100년이며 표시되는 최대 날짜는 현재 날짜 이후 100년입니다. [MinDate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.mindate.aspx) 및 [MaxDate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.maxdate.aspx) 속성을 설정하여 달력이 표시하는 최소 및 최대 날짜를 변경할 수 있습니다.

```csharp
calendarView1.MinDate = new DateTime(2000, 1, 1);
calendarView1.MaxDate = new DateTime(2099, 12, 31);
```

### <a name="updating-calendar-day-items"></a>달력 날짜 항목 업데이트

달력의 각 날짜는 [CalendarViewDayItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarviewdayitem.aspx) 개체로 표시됩니다. 개별 날짜 항목에 액세스하고 속성 및 메서드를 사용하려면 [CalendarViewDayItemChanging](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.calendarviewdayitemchanging.aspx) 이벤트를 처리하고 이벤트 인수의 항목 속성을 사용하여 CalendarViewDayItem에 액세스합니다.

[CalendarViewDayItem.IsBlackout](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarviewdayitem.isblackout.aspx) 속성을 **true**로 설정하여 달력 보기에서 날짜를 선택할 수 없도록 만들 수 있습니다. 

[CalendarViewDayItem.SetDensityColors](https://msdn.microsoft.com/library/windows/apps/xaml/dn890067.aspx) 메서드를 호출하여 하루의 이벤트 밀도에 대한 상황별 정보를 표시할 수 있습니다. 각 날짜에 대해 0에서 10 사이 밀도 막대를 표시하고 각 막대의 색을 설정할 수 있습니다. 

달력의 일부 날짜 항목은 다음과 같습니다. 1일과 2일은 검게 표시됩니다. 2일, 3일 및 4일은 다양한 밀도 막대 집합을 가집니다.

![밀도 막대가 있는 달력 날짜](images/calendar-view-density-bars.png)

### <a name="phased-rendering"></a>단계적인 렌더링

달력 보기에는 많은 CalendarViewDayItem 개체를 포함할 수 있습니다. UI를 응답 가능한 상태로 유지하고 달력을 통해 부드러운 이동을 가능하도록 하기 위해 달력 보기에서는 단계별 렌더링을 지원합니다. 이를 통해 날짜 항목 처리를 단계로 나눌 수 있습니다. 모든 단계가 완료되기 전에 하루가 보기 외부로 이동할 경우 더 이상 해당 항목을 처리하고 렌더링하는 데 시간을 사용하지 않습니다.

이 예제에서는 약속을 예약하기 위한 달력 보기의 단계적인 렌더링을 보여 줍니다. 
- 0단계에서 기본 날짜 항목이 렌더링됩니다. 
- 1단계에서 예약할 수 없는 날짜를 블랙아웃합니다. 여기에는 지난 날짜, 일요일 및 이미 예약된 날짜가 포함됩니다. 
- 2단계에서 해당 날짜에 예약된 각 약속을 확인합니다. 확정된 각 약속에 대해서는 녹색 밀도 막대를 표시하고 미정인 각 약속에 대해서는 파란색 밀도 막대를 표시합니다. 

이 예제의 `Bookings` 클래스는 가상의 약속 예약 앱에서 온 것이며 표시되지 않습니다.

```xaml
<CalendarView CalendarViewDayItemChanging="CalendarView_CalendarViewDayItemChanging"/>
```

```csharp
private void CalendarView_CalendarViewDayItemChanging(CalendarView sender, 
                                   CalendarViewDayItemChangingEventArgs args)
{
    // Render basic day items.
    if (args.Phase == 0)
    {
        // Register callback for next phase.
        args.RegisterUpdateCallback(CalendarView_CalendarViewDayItemChanging);
    }
    // Set blackout dates.
    else if (args.Phase == 1)
    {   
        // Blackout dates in the past, Sundays, and dates that are fully booked.
        if (args.Item.Date < DateTimeOffset.Now ||
            args.Item.Date.DayOfWeek == DayOfWeek.Sunday ||
            Bookings.HasOpenings(args.Item.Date) == false)
        {
            args.Item.IsBlackout = true;
        }
        // Register callback for next phase.
        args.RegisterUpdateCallback(CalendarView_CalendarViewDayItemChanging);
    }
    // Set density bars.
    else if (args.Phase == 2)
    {
        // Avoid unnecessary processing.
        // You don't need to set bars on past dates or Sundays.
        if (args.Item.Date > DateTimeOffset.Now &&
            args.Item.Date.DayOfWeek != DayOfWeek.Sunday)
        {
            // Get bookings for the date being rendered.
            var currentBookings = Bookings.GetBookings(args.Item.Date);

            List<Color> densityColors = new List<Color>();
            // Set a density bar color for each of the days bookings.
            // It's assumed that there can't be more than 10 bookings in a day. Otherwise,
            // further processing is needed to fit within the max of 10 density bars.
            foreach (booking in currentBookings)
            {
                if (booking.IsConfirmed == true)
                {
                    densityColors.Add(Colors.Green);
                }
                else
                {
                    densityColors.Add(Colors.Blue);
                }
            }
            args.Item.SetDensityColors(densityColors);
        }
    }
}
```

## <a name="get-the-sample-code"></a>샘플 코드 다운로드

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Xaml-Controls-Gallery) - 대화형 형식의 모든 XAML 컨트롤을 보여줍니다.

## <a name="related-articles"></a>관련 문서

- [날짜 및 시간 컨트롤](date-and-time.md)
- [달력 날짜 선택](calendar-date-picker.md)
- [날짜 선택기](date-picker.md)
- [시간 선택기](time-picker.md)
