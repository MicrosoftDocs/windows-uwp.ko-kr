---
title: 일본의 시대 변경을 대비한 응용 프로그램 준비
description: 2019년 5월 일본의 시대 변경과 응용 프로그램을 준비하는 방법에 대해 알아보세요.
ms.assetid: 5A945F9A-8632-4038-ADD6-C0568091EF27
ms.date: 11/29/2018
ms.topic: article
keywords: 'windows 10, uwp, 현지화 가능성, 현지화, 일본, 시대'
ms.localizationpriority: high
---

# 일본의 시대 변경을 대비한 응용 프로그램 준비

일본력은 시대별로 구분되는 데 현대 컴퓨터 시대의 대부분은 헤이세이 시대에 있습니다; 그러나 2019년 5월 1일부로 새 시대가 시작됩니다. 시대가 바뀌는 것은 수 십년만에 처음이기 때문에, 새 시대가 시작될 때 일본력을 지원하는 소프트웨어가 올바르게 기능할 것인지 확인하기 위해 소프트웨어를 테스트할 필요가 있습니다.

다음 섹션들에서 여러분은 다가오는 새 시대를 위해 여러분의 응용 프로그램을 준비하고 테스트하기 위해 무엇을 할 수 있는지를 배울 것입니다.

> [!NOTE]
> 여러분이 수행하는 변경은 컴퓨터 전체의 동작에 영향을 미칠 것이기 때문에 이를 위해 테스트 컴퓨터를 사용하는 것이 좋습니다.

## 새 시대를 위한 레지스트리 키 추가

시대가 바뀌기 전에 양립성 문제를 테스트하는 것이 중요한 데 이제는 자리 표시자 명칭을 사용해 그렇게 할 수 있습니다. 그렇게 하려면 **레지스트리 편집기**를 사용하여 새 시대를 위한 레지스트리 키를 추가하십시오:

1. **Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Nls\Calendars\Japanese\Eras**로 이동하십시오.
2. **편집 > 신규 > 문자열 값**을 선택하고, **2019 05 01**이라는 명칭을 부여합니다.
3. 해당 키를 마우스 오른쪽 버튼으로 클릭하고 **수정**을 선택합니다.
4. **값 데이터** 필드에 **？？\_？\_??????\_?** 을 입력합니다. (쉽게 수행할 수 있도록 여기에서 복사하여 붙여넣을 수 있습니다).

이러한 레지스트리 키의 형식에 대한 자세한 설명은 [일본력을 위한 시대 취급](https://docs.microsoft.com/windows/desktop/Intl/era-handling-for-the-japanese-calendar)을 참조하십시오.

새 시대의 명칭이 발표되면 지원되는 Windows 버전을 위한 새로운 레지스트리 키와 함께 업데이트는 해당 명칭을 포함할 것이기 때문에 여러분은 자신의 응용 프로그램이 그것을 제대로 취급하는지를 검증할 수 있습니다. 이 업데이트는 지원된 이전의 Windows 10 릴리스뿐만 아니라 Windows 8 및 7에도 전파됩니다.

응용 프로그램 테스트가 완료되면 자리 표시자 레지스트리 키를 삭제할 수 있습니다. 이렇게 하면 Windows가 업데이트될 때 추가될 새로운 레지스트리 키와 그것이 간섭되지 않을 것입니다.

## 기기의 달력 형식 변경

새 시대를 위한 레지스트리 키를 추가했으면, 기기가 일본력을 사용하도록 구성해야 합니다. 이 작업을 수행하기 위해 일본어 기기가 필요한 것은 아닙니다. 철저한 테스트를 위해서는 일본어 팩을 설치할 수 있지만 기본적인 테스트에는 그것이 요구되지 않습니다.

기기가 일본력을 사용하도록 구성하는 방법:

1. **intl.cpl**을 엽니다(Windows 검색 창에서 검색).
2. **형식** 드롭다운에서 **일본어(일본)** 을 선택합니다.
3. **추가 설정**을 선택합니다.
4. **날짜** 탭을 선택합니다.
5. **달력 타입** 드롭다운에서 **和暦**(*와레키*, 화력)를 선택합니다. 그것은 두 번째 옵션입니다.
6. **확인**을 클릭합니다.
7. **지역** 창에서 **확인** 을 클릭합니다.

이제 기기가 일본력을 사용하도록 구성되었으며, 레지스트리에 어떤 시대가 있든 그것을 반영할 것입니다. 이제 화면의 오른쪽 아래에 보이는 것의 예가 아래에 있습니다:

![일본력 형식의 날짜 및 시간](images/japanese-calendar-format.png)

## 기기의 시계 조정

응용 프로그램이 새 시대와 함께 기능하는지 테스트하려면 컴퓨터의 시계를 2019년 5월 1일 이후로 진행시켜야 합니다. 다음 지침은 Windows 10을 위한 것이지만 Windows 8 및 7도 유사하게 기능합니다:

1. 화면의 오른쪽 아래에 있는 날짜 및 시간 영역을 마우스 오른쪽 버튼으로 클릭합니다.
2. **날짜/시간 조정**을 선택합니다.
3. 설정 앱의 **날짜 및 시간 변경**에서 **변경**을 선택합니다.
4. 날짜를 2019년 5월 1일 또는 이후로 변경합니다.

> [!NOTE]
> 조직 설정에 근거해서는 날짜를 변경하지 못할 수 있습니다; 이러한 경우, 관리자와 상담하십시오. 또는 이미 지난 날짜를 갖도록 자리 표시자 레지스트리 키를 편집해도 됩니다.

## 응용 프로그램 테스트

이제 응용 프로그램이 새 시대를 어떻게 취급하는지를 테스트하십시오. 타임스탬프와 날짜 선택기 같이 날짜가 표시되는 곳을 체크하십시오. 2019년 5월 1일 이전(Heisei, 平成) 또는 이후(？？)의 시대가 올바른지 확인합니다.

### *간넨*(元年)

일본력의 형식은 일반적으로 **&lt;시대 명칭&gt; &lt;시대년&gt;** 입니다. 예컨대 2018년은 **헤이세이 30년**(平成30年)입니다.  단, 시대의 첫 번째 해는 특별합니다; **&lt;시대 명칭&gt; 1년 대신에** **&lt;시대 명칭&gt; 元年**(*간넨*)입니다. 따라서 헤이세이 시대의 첫 번째 해는 平成元年(*헤이세이 간넨*)입니다. 응용 프로그램이 새 시대의 첫 번째 해를 제대로 취급하여 올바르게 ？？元年을 출력하는지 확인하십시오.

## 관련된 API

시대 변경을 취급하기 위해 업데이트될 여러 WinRT, .NET, 및 Win32 API가 있으므로 귀하가 그것들을 사용하는 경우에도 너무 걱정할 필요가 없습니다. 그러나 이러한 API에 전적으로 의존하는 경우에는 응용 프로그램을 테스트하여 원하는 대로 작동하는지 확인하는 것이 좋은 데, 특히 귀하가 그것으로 구문 분석 같은 특별한 일을 수행하는 경우에 그렇습니다.

[2019년 5월 일본의 시대 변경을 위한 업데이트](https://support.microsoft.com/help/4470918/updates-for-may-2019-japan-era-change)에서 OS 및 SDK의 업데이트를 추적할 수 있습니다.

다음 API들이 영향을 받습니다:

### WinRT

* [Windows.Globalization 네임스페이스](https://docs.microsoft.com/uwp/api/windows.globalization)
    * [Calendar 클래스](https://docs.microsoft.com/uwp/api/windows.globalization.calendar)
        * [AddDays 방법](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.adddays)
        * [AddEras 방법](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.adderas)
        * [AddHours 방법](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addhours)
        * [AddMinutes 방법](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addminutes)
        * [AddMonths 방법](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addmonths)
        * [AddNanoseconds 방법](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addnanoseconds)
        * [AddPeriods 방법](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addperiods)
        * [AddSeconds 방법](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addseconds)
        * [AddWeeks 방법](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addweeks)
        * [AddYears 방법](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addyears)
        * [Era 속성](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.era)
        * [EraAsString 방법](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.eraasstring)
        * [FirstYearInThisEra 속성](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.firstyearinthisera)
        * [LastEra 속성](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.lastera)
        * [LastYearInThisEra 속성](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.lastyearinthisera)
        * [NumberOfYearsInThisEra 속성](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.numberofyearsinthisera)     
* [Windows.Globalization.DateTimeFormatting 네임스페이스](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting)
    * [DateTimeFormatter 클래스](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting.datetimeformatter)
        * [Format 방법](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting.datetimeformatter.format)

### .NET

* [System 네임스페이스](https://docs.microsoft.com/dotnet/api/system)
    * [DateTime 구조](https://docs.microsoft.com/dotnet/api/system.datetime)
    * [DateTimeOffset 구조](https://docs.microsoft.com/dotnet/api/system.datetimeoffset)
* [System.Globalization 네임스페이스](https://docs.microsoft.com/dotnet/api/system.globalization)
    * [Calendar 클래스](https://docs.microsoft.com/dotnet/api/system.globalization.calendar)
    * [DateTimeFormatInfo 클래스](https://docs.microsoft.com/dotnet/api/system.globalization.datetimeformatinfo)
    * [JapaneseCalendar 클래스](https://docs.microsoft.com/dotnet/api/system.globalization.japanesecalendar)
    * [JapaneseLunisolarCalendar 클래스](https://docs.microsoft.com/dotnet/api/system.globalization.japaneselunisolarcalendar)

### Win32

* [datetimeapi.h 헤더](https://docs.microsoft.com/windows/desktop/api/datetimeapi/)
    * [GetDateFormatA 함수](https://docs.microsoft.com/windows/desktop/api/datetimeapi/nf-datetimeapi-getdateformata)
    * [GetDateFormatEx 함수](https://docs.microsoft.com/windows/desktop/api/datetimeapi/nf-datetimeapi-getdateformatex)
    * [GetDateFormatW 함수](https://docs.microsoft.com/windows/desktop/api/datetimeapi/nf-datetimeapi-getdateformatw)
* [winnls.h 헤더](https://docs.microsoft.com/windows/desktop/api/winnls/)
    * [EnumDateFormatsA 함수](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsa)
    * [EnumDateFormatsExA 함수](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsexa)
    * [EnumDateFormatsExEx 함수](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsexex)
    * [EnumDateFormatsExW 함수](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsexw)
    * [EnumDateFormatsW 함수](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsw)
    * [GetCalendarInfoA 함수](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-getcalendarinfoa)
    * [GetCalendarInfoEx 함수](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-getcalendarinfoex)
    * [GetCalendarInfoW 함수](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-getcalendarinfow)

## 참고 항목

* [일본력을 위한 시대 취급](https://docs.microsoft.com/windows/desktop/Intl/era-handling-for-the-japanese-calendar)
* [일본력의 Y2K 순간](https://blogs.msdn.microsoft.com/shawnste/2018/04/12/the-japanese-calendars-y2k-moment/)
* [레지스트리를 사용하여 Windows에서 새 일본 시대를 테스트하기](https://blogs.msdn.microsoft.com/shawnste/2018/08/07/using-the-registry-to-test-the-new-japanese-era-on-windows/)
* [간넨 대 이치넨](https://blogs.msdn.microsoft.com/shawnste/2018/11/12/gannen-vs-ichinen/)
* [2019년 5월 일본의 시대 변경을 위한 업데이트](https://support.microsoft.com/help/4470918/updates-for-may-2019-japan-era-change)
* [.NET에서 일본력의 새 시대 취급](https://blogs.msdn.microsoft.com/dotnet/2018/11/14/handling-a-new-era-in-the-japanese-calendar-in-net/)

<!--HONumber=12月18_HO1-->

