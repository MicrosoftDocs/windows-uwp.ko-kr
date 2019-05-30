---
title: 일본의 시대 변경을 대비한 응용 프로그램 준비
description: 2019년 5월 일본의 시대 변경과 응용 프로그램을 준비하는 방법에 대해 알아보세요.
ms.assetid: 5A945F9A-8632-4038-ADD6-C0568091EF27
ms.date: 4/26/2019
ms.topic: article
keywords: windows 10, uwp, 현지화 가능성, 현지화, 일본, 시대
ms.localizationpriority: high
ms.openlocfilehash: 54d66d0426e5f0c41d48b93ba96781786d6fab92
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66363775"
---
# <a name="prepare-your-application-for-the-japanese-era-change"></a>일본의 시대 변경을 대비한 응용 프로그램 준비

> [!NOTE]
> 2019 년 4 월 1 일에 새로운 이름을 발표 되었습니다. Reiwa (令和)입니다. 4 월 25 일에 Microsoft는 새로운 이름으로 업데이트 된 레지스트리 키를 포함 하는 다른 Windows 운영 체제에 대 한 패키지를 출시 했습니다. 장치를 업데이트 및 레지스트리를 새 키가 있는지를 확인 하 고 응용 프로그램을 테스트 합니다. 확인할 [이 지원 문서](https://support.microsoft.com/help/4469068/summary-of-new-japanese-era-updates-kb4469068) 운영 체제 업데이트 된 레지스트리 키를 받아야 합니다 되도록 합니다.

일본식 달력 연대로 나뉩니다 및 헤이세이 연대의; 했던 대부분의 컴퓨팅 현대의 최신 기술에 대해 그러나 2019 년 5 월 1 일에 새로운 시대 시작 됩니다. 시대가 바뀌는 것은 수 십년만에 처음이기 때문에, 새 시대가 시작될 때 일본력을 지원하는 소프트웨어가 올바르게 기능할 것인지 확인하기 위해 소프트웨어를 테스트할 필요가 있습니다.

다음 섹션들에서 여러분은 다가오는 새 시대를 위해 여러분의 응용 프로그램을 준비하고 테스트하기 위해 무엇을 할 수 있는지를 배울 것입니다.

> [!NOTE]
> 여러분이 수행하는 변경은 컴퓨터 전체의 동작에 영향을 미칠 것이기 때문에 이를 위해 테스트 컴퓨터를 사용하는 것이 좋습니다.

## <a name="add-a-registry-key-for-the-new-era"></a>새 시대를 위한 레지스트리 키 추가

> [!NOTE]
> 다음 지침을는 새 레지스트리 키를 사용 하 여 아직 업데이트 되지 않은 장치에 대 한 것입니다. 먼저 장치에 새 레지스트리 키를 포함 하는 경우를 확인 하 고 그렇지 않은 경우 다음 지침을 사용 하 여 테스트 합니다.

연대를 변경 하 고 이제 새로운 시대의 이름을 사용해 서 할 수 전에 호환성 문제에 대 한 테스트 하는 것이 반드시 합니다. 그렇게 하려면 **레지스트리 편집기**를 사용하여 새 시대를 위한 레지스트리 키를 추가하십시오:

1. **Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Nls\Calendars\Japanese\Eras**로 이동하십시오.
2. **편집 > 신규 > 문자열 값**을 선택하고, **2019 05 01**이라는 명칭을 부여합니다.
3. 해당 키를 마우스 오른쪽 버튼으로 클릭하고 **수정**을 선택합니다.
4. 에 **값 데이터** 필드를 입력 합니다**令和_令_Reiwa_R** (하면 복사 및 붙여넣을 수 쉽게 수행할 수 있도록 여기에서).

이러한 레지스트리 키의 형식에 대한 자세한 설명은 [일본력을 위한 시대 취급](https://docs.microsoft.com/windows/desktop/Intl/era-handling-for-the-japanese-calendar)을 참조하십시오.

2019 년 4 월 1 일에 새로운 이름을 발표 되었습니다. 4 월 25 일에 이름을 포함 하는 지원 되는 Windows 버전에 대 한 새 레지스트리 키를 사용 하 여 업데이트 된 릴리스, 제대로 처리 응용 프로그램을 허용 하는지 확인할 수 있습니다. 이 업데이트에 지원 되는 이전 릴리스에서 물론 Windows 10, Windows 8 및 7 전파 됩니다.

응용 프로그램 테스트가 완료되면 자리 표시자 레지스트리 키를 삭제할 수 있습니다. 이렇게 하면 Windows가 업데이트될 때 추가될 새로운 레지스트리 키와 그것이 간섭되지 않을 것입니다.

## <a name="change-your-devices-calendar-format"></a>기기의 달력 형식 변경

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

## <a name="adjust-your-devices-clock"></a>기기의 시계 조정

응용 프로그램이 새 시대와 함께 기능하는지 테스트하려면 컴퓨터의 시계를 2019년 5월 1일 이후로 진행시켜야 합니다. 다음 지침은 Windows 10을 위한 것이지만 Windows 8 및 7도 유사하게 기능합니다:

1. 화면의 오른쪽 아래에 있는 날짜 및 시간 영역을 마우스 오른쪽 버튼으로 클릭합니다.
2. **날짜/시간 조정**을 선택합니다.
3. 설정 앱의 **날짜 및 시간 변경**에서 **변경**을 선택합니다.
4. 날짜를 2019년 5월 1일 또는 이후로 변경합니다.

> [!NOTE]
> 조직 설정에 따라 날짜를 변경할 수 있습니다. 이 경우, 경우 관리자에 게 문의 또는 이미 경과 된 날짜에 자리 표시자 레지스트리 키를 편집할 수 있습니다.

## <a name="test-your-application"></a>응용 프로그램 테스트

이제 응용 프로그램이 새 시대를 어떻게 취급하는지를 테스트하십시오. 타임스탬프와 날짜 선택기 같이 날짜가 표시되는 곳을 체크하십시오. 연대 올바른지 2019 년 5 월 1 년 (헤이세이 平成) 전후 (Reiwa 令和) 있는지 확인 합니다.

### <a name="gannen-"></a>*간넨*(元年)

일본식 달력 형식은 일반적으로  **&lt;연대 이름이&gt; &lt;연대 연도의&gt;** 합니다. 예컨대 2018년은 **헤이세이 30년**(平成30年)입니다.  단, 시대의 첫 번째 해는 특별합니다; **&lt;시대 명칭&gt; 1년 대신에** **&lt;시대 명칭&gt; 元年**(*간넨*)입니다. 따라서 헤이세이 시대의 첫 번째 해는 平成元年(*헤이세이 간넨*)입니다. 응용 프로그램이 새로운 시대의 첫 해를 올바르게 처리 하 고 올바르게 令和元年 출력에 있는지 확인 합니다.

## <a name="related-apis"></a>관련된 API

시대 변경을 취급하기 위해 업데이트될 여러 WinRT, .NET, 및 Win32 API가 있으므로 귀하가 그것들을 사용하는 경우에도 너무 걱정할 필요가 없습니다. 그러나 이러한 API에 전적으로 의존하는 경우에는 응용 프로그램을 테스트하여 원하는 대로 작동하는지 확인하는 것이 좋은 데, 특히 귀하가 그것으로 구문 분석 같은 특별한 일을 수행하는 경우에 그렇습니다.

업데이트는 OS와 Sdk에 따라 [에 대 한 업데이트 수 2019 일본 연대 변경](https://support.microsoft.com/help/4470918/updates-for-may-2019-japan-era-change)합니다.

다음 API들이 영향을 받습니다:

### <a name="winrt"></a>WinRT

* [Windows.Globalization Namespace](https://docs.microsoft.com/uwp/api/windows.globalization)
  * [달력 클래스](https://docs.microsoft.com/uwp/api/windows.globalization.calendar)
    * [AddDays 메서드](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.adddays)
    * [AddEras 메서드](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.adderas)
    * [AddHours 메서드](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addhours)
    * [AddMinutes 메서드](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addminutes)
    * [AddMonths 메서드](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addmonths)
    * [AddNanoseconds 메서드](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addnanoseconds)
    * [AddPeriods 메서드](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addperiods)
    * [AddSeconds 메서드](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addseconds)
    * [AddWeeks 메서드](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addweeks)
    * [AddYears 메서드](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addyears)
    * [연대 속성](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.era)
    * [EraAsString 메서드](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.eraasstring)
    * [FirstYearInThisEra 속성](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.firstyearinthisera)
    * [LastEra 속성](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.lastera)
    * [LastYearInThisEra 속성](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.lastyearinthisera)
    * [NumberOfYearsInThisEra 속성](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.numberofyearsinthisera)
* [Windows.Globalization.DateTimeFormatting Namespace](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting)
  * [DateTimeFormatter 클래스](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting.datetimeformatter)
    * [Format 메서드](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting.datetimeformatter.format)

### <a name="net"></a>.NET

* [시스템 Namespace](https://docs.microsoft.com/dotnet/api/system)
  * [DateTime 구조체](https://docs.microsoft.com/dotnet/api/system.datetime)
  * [DateTimeOffset 구조체](https://docs.microsoft.com/dotnet/api/system.datetimeoffset)
* [System.Globalization Namespace](https://docs.microsoft.com/dotnet/api/system.globalization)
  * [달력 클래스](https://docs.microsoft.com/dotnet/api/system.globalization.calendar)
  * [DateTimeFormatInfo 클래스](https://docs.microsoft.com/dotnet/api/system.globalization.datetimeformatinfo)
  * [JapaneseCalendar 클래스](https://docs.microsoft.com/dotnet/api/system.globalization.japanesecalendar)
  * [JapaneseLunisolarCalendar 클래스](https://docs.microsoft.com/dotnet/api/system.globalization.japaneselunisolarcalendar)

### <a name="win32"></a>Win32

* [datetimeapi.h header](https://docs.microsoft.com/windows/desktop/api/datetimeapi/)
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

## <a name="see-also"></a>참조

* [연대 일본식 달력 처리](https://docs.microsoft.com/windows/desktop/Intl/era-handling-for-the-japanese-calendar)
* [일본식 달력 Y2K 순간](https://blogs.msdn.microsoft.com/shawnste/2018/04/12/the-japanese-calendars-y2k-moment/)
* [레지스트리를 사용 하 여 Windows에서 새 일본어 연대를 테스트 하려면](https://blogs.msdn.microsoft.com/shawnste/2018/08/07/using-the-registry-to-test-the-new-japanese-era-on-windows/)
* [넨 vs Ichinen](https://blogs.msdn.microsoft.com/shawnste/2018/11/12/gannen-vs-ichinen/)
* [2019 일본 연대 변경에 대 한 업데이트 될 수 있습니다.](https://support.microsoft.com/help/4470918/updates-for-may-2019-japan-era-change)
* [.NET에서 일본식 달력의 새로운 시대를 처리합니다.](https://devblogs.microsoft.com/dotnet/handling-a-new-era-in-the-japanese-calendar-in-net/)
