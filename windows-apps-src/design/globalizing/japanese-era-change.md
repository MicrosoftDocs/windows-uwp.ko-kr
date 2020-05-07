---
title: 일본의 시대 변경을 대비한 애플리케이션 준비
description: 2019년 5월 일본의 연호 변경과 애플리케이션을 준비하는 방법에 대해 알아보세요.
ms.assetid: 5A945F9A-8632-4038-ADD6-C0568091EF27
ms.date: 04/26/2019
ms.topic: article
keywords: windows 10, uwp, 현지화 가능성, 현지화, 일본, 연호
ms.localizationpriority: high
ms.openlocfilehash: 7e8250ccae96ed835aba2a2a993fdde9ae31a884
ms.sourcegitcommit: f727b68e86a86c94eff00f67ed79a1c12666e7bc
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "67714115"
---
# <a name="prepare-your-application-for-the-japanese-era-change"></a>일본의 시대 변경을 대비한 애플리케이션 준비

> [!NOTE]
> 2019년 4월 1일에 새로운 연호 레이와(令和)가 발표되었습니다. 4월 25일에 Microsoft는 업데이트된 레지스트리 키와 새로운 연호가 포함된 다른 Windows 운영 체제에 대한 패키지를 출시했습니다. 디바이스를 업데이트하고 레지스트리를 검사하여 새 키가 있는지 확인하고 애플리케이션을 테스트하세요. [이 지원 문서](https://support.microsoft.com/help/4469068/summary-of-new-japanese-era-updates-kb4469068)를 검토하여 현재 사용하는 운영 체제에서 업데이트된 레지스트리 키를 받았는지 확인하세요.

일본력은 연호에 따라 구분되며, 현대 컴퓨팅의 대부분은 헤이세이 시대에 있었습니다. 그러나 2019년 5월 1일부로 새로운 연호가 사용됩니다. 연호가 바뀌는 것은 수십 년 만에 처음이기 때문에, 새 연호가 사용될 때 일본력을 지원하는 소프트웨어가 올바르게 작동하는지 테스트할 필요가 있습니다.

다음 섹션에서는 새 연호를 대비하여 애플리케이션을 준비하고 테스트하려면 어떻게 해야 하는지 알아보겠습니다.

> [!NOTE]
> 여기서 변경하는 내용은 컴퓨터 전체의 동작에 영향을 주기 때문에 테스트 컴퓨터를 사용하는 것이 좋습니다.

## <a name="add-a-registry-key-for-the-new-era"></a>새 연호를 위한 레지스트리 키 추가

> [!NOTE]
> 다음 지침은 아직 새 레지스트리 키로 업데이트되지 않은 디바이스를 위한 것입니다. 먼저 디바이스에 새 레지스트리 키가 있는지 확인하고, 없으면 다음 지침에 따라 테스트합니다.

연호가 바뀌기 전에 호환성 문제를 테스트해야 하는데, 이제 새 연호를 사용하여 호환성을 테스트할 수 있습니다. 그렇게 하려면 **레지스트리 편집기**를 사용하여 새 연호에 대한 레지스트리 키를 추가합니다.

1. **Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Nls\Calendars\Japanese\Eras**로 이동합니다.
2. **편집 > 새로 만들기 > 문자열 값**을 선택하고, 이름을 **2019 05 01**로 지정합니다.
3. 키를 마우스 오른쪽 단추로 클릭하고 **수정**을 선택합니다.
4. **값 데이터** 필드에 **令和_令_Reiwa_R**을 입력합니다. 여기서 복사하여 붙여넣으면 간편합니다.

이러한 레지스트리 키의 형식에 대한 자세한 내용은 [일본력의 연호 처리](https://docs.microsoft.com/windows/desktop/Intl/era-handling-for-the-japanese-calendar)를 참조하세요.

2019년 4월 1일에 새 연호가 발표되었습니다. 4월 25일에 이름을 포함하고 있는 지원되는 Windows 버전에 대한 새 레지스트리 키가 포함된 업데이트가 발표되었으며, 애플리케이션에서 이 업데이트를 올바르게 처리하는지 확인할 수 있습니다. 이 업데이트는 지원되는 이전 Windows 10 릴리스뿐 아니라 Windows 8 및 7에도 전파됩니다.

애플리케이션 테스트가 완료되면 자리 표시자 레지스트리 키를 삭제할 수 있습니다. 이렇게 하면 Windows가 업데이트될 때 추가될 새 레지스트리 키와 겹치지 않습니다.

## <a name="change-your-devices-calendar-format"></a>디바이스의 달력 형식 변경

새 연호의 레지스트리 키를 추가한 후에는 일본력을 사용하도록 디바이스를 구성해야 합니다. 이 작업을 수행하기 위해 일본어 디바이스가 필요한 것은 아닙니다. 철저한 테스트를 위해 일본어 팩을 설치할 수 있지만, 기본적인 테스트에는 없어도 괜찮습니다.

일본력을 사용하도록 디바이스를 구성하려면 다음을 수행합니다.

1. **intl.cpl** 파일을 엽니다(Windows 검색 창에서 검색).
2. **형식** 드롭다운에서 **일본어(일본)** 를 선택합니다.
3. **추가 설정**을 선택합니다.
4. **날짜** 탭을 선택합니다.
5. **달력 타입** 드롭다운에서 **和暦**(*와레키*, 화력)를 선택합니다. 두 번째 옵션입니다.
6. **확인**을 클릭합니다.
7. **지역** 창에서 **확인**을 클릭합니다.

이제 디바이스가 일본력을 사용하도록 구성되었으며, 레지스트리에 있는 연호가 반영됩니다. 아래는 화면의 오른쪽 아래 모서리에 보이는 내용의 예입니다.

![일본력 형식의 날짜 및 시간](images/japanese-calendar-format.png)

## <a name="adjust-your-devices-clock"></a>디바이스의 시계 조정

애플리케이션이 새 연호에서 작동하는지 테스트하려면 컴퓨터의 시계를 2019년 5월 1일 이후로 조정해야 합니다. 다음은 Windows 10 지침이지만, Windows 8 및 7에서도 비슷하게 작동합니다.

1. 화면의 오른쪽 아래 모서리에 있는 날짜 및 시간 영역을 마우스 오른쪽 단추로 클릭합니다.
2. **날짜/시간 조정**을 선택합니다.
3. 설정 앱의 **날짜 및 시간 변경**에서 **변경**을 선택합니다.
4. 날짜를 2019년 5월 1일 이후로 변경합니다.

> [!NOTE]
> 조직 설정에 따라 날짜를 변경할 수 없는 경우가 있습니다. 이 경우 관리자에게 문의하세요. 또는 자리 표시자 레지스트리 키를 편집하여 이미 지나간 날짜를 사용할 수도 있습니다.

## <a name="test-your-application"></a>애플리케이션 테스트

이제 애플리케이션이 새 연호를 어떻게 처리하는지 테스트합니다. 타임스탬프 및 날짜 선택기처럼 날짜가 표시되는 곳을 확인합니다. 2019년 5월 1일 전의 연호(Heisei, 平成)와 이후의 연호(Reiwa, 令和)가 올바른지 확인합니다.

### <a name="gannen-"></a>*간넨*(元年)

일본력의 형식은 일반적으로 **&lt;연호&gt; &lt;연도&gt;** 입니다. 예를 들어 2018년은 **헤이세이 30년**(平成30年)입니다.  단, 첫 번째 해는 특별합니다. **&lt;연호&gt; 1년 대신** **&lt;연호&gt; 元年**(*간넨*)을 사용합니다. 따라서 헤이세이 시대의 첫 번째 해는 平成元年(*헤이세이 간넨*)입니다. 애플리케이션이 새 시대의 첫 번째 해를 제대로 처리하여 令和元年을 출력하는지 확인합니다.

## <a name="related-apis"></a>관련 API

연호 변경을 처리하기 위해 여러 WinRT, .NET 및 Win32 API가 업데이트될 예정이므로 이러한 API를 사용하는 분들은 너무 걱정할 필요 없습니다. 그러나 이러한 API에 전적으로 의존하는 경우에도 애플리케이션을 테스트하여 원하는 대로 작동하는지 확인하는 것이 좋습니다. 특히 이러한 API로 구문 분석 같은 특별한 작업을 수행하는 경우에는 반드시 테스트해야 합니다.

[2019년 5월 일본 연호 변경을 위한 업데이트](https://support.microsoft.com/help/4470918/updates-for-may-2019-japan-era-change)에서 OS 및 SDK 업데이트를 추적할 수 있습니다.

다음 API가 영향을 받습니다.

### <a name="winrt"></a>WinRT

* [Windows.Globalization 네임스페이스](https://docs.microsoft.com/uwp/api/windows.globalization)
  * [Calendar 클래스](https://docs.microsoft.com/uwp/api/windows.globalization.calendar)
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
    * [Era 속성](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.era)
    * [EraAsString 메서드](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.eraasstring)
    * [FirstYearInThisEra 속성](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.firstyearinthisera)
    * [LastEra 속성](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.lastera)
    * [LastYearInThisEra 속성](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.lastyearinthisera)
    * [NumberOfYearsInThisEra 속성](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.numberofyearsinthisera)
* [Windows.Globalization.DateTimeFormatting 네임스페이스](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting)
  * [DateTimeFormatter 클래스](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting.datetimeformatter)
    * [Format 메서드](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting.datetimeformatter.format)

### <a name="net"></a>.NET

* [System 네임스페이스](https://docs.microsoft.com/dotnet/api/system)
  * [DateTime 구조](https://docs.microsoft.com/dotnet/api/system.datetime)
  * [DateTimeOffset 구조](https://docs.microsoft.com/dotnet/api/system.datetimeoffset)
* [System.Globalization 네임스페이스](https://docs.microsoft.com/dotnet/api/system.globalization)
  * [Calendar 클래스](https://docs.microsoft.com/dotnet/api/system.globalization.calendar)
  * [DateTimeFormatInfo 클래스](https://docs.microsoft.com/dotnet/api/system.globalization.datetimeformatinfo)
  * [JapaneseCalendar 클래스](https://docs.microsoft.com/dotnet/api/system.globalization.japanesecalendar)
  * [JapaneseLunisolarCalendar 클래스](https://docs.microsoft.com/dotnet/api/system.globalization.japaneselunisolarcalendar)

### <a name="win32"></a>Win32

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

## <a name="see-also"></a>참고 항목

* [일본력의 연호 처리](https://docs.microsoft.com/windows/desktop/Intl/era-handling-for-the-japanese-calendar)
* [일본력의 Y2K 순간](https://blogs.msdn.microsoft.com/shawnste/2018/04/12/the-japanese-calendars-y2k-moment/)
* [레지스트리를 사용하여 Windows에서 새 일본 연호 테스트](https://blogs.msdn.microsoft.com/shawnste/2018/08/07/using-the-registry-to-test-the-new-japanese-era-on-windows/)
* [간넨 대 이치넨](https://blogs.msdn.microsoft.com/shawnste/2018/11/12/gannen-vs-ichinen/)
* [2019년 5월 일본 연호 변경을 위한 업데이트](https://support.microsoft.com/help/4470918/updates-for-may-2019-japan-era-change)
* [.NET에서 일본력의 새 연호 처리](https://devblogs.microsoft.com/dotnet/handling-a-new-era-in-the-japanese-calendar-in-net/)
