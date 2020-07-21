---
Description: 날짜, 시간, 숫자, 전화 번호 및 통화를 적절 하 게 지정 하 여 전역적으로 사용할 수 있도록 앱을 디자인 합니다. 그런 다음 나중에 글로벌 시장에서 추가 문화권, 지역 및 언어용 앱을 적용할 수 있습니다.
title: 날짜/시간/숫자 형식 세계화
ms.assetid: 6ECE8BA4-9A7D-49A6-81EE-AB2BE7F0254F
template: detail.hbs
ms.date: 11/07/2017
ms.topic: article
keywords: windows 10, uwp, 세계화, 지역화 가능성, 지역화
ms.localizationpriority: medium
ms.openlocfilehash: 798199269a4fd02eebef7dcd46cd5781ba561250
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493428"
---
# <a name="globalize-your-datetimenumber-formats"></a>날짜/시간/숫자 형식 세계화

날짜, 시간, 숫자, 전화 번호 및 통화를 적절 하 게 지정 하 여 전역적으로 사용할 수 있도록 앱을 디자인 합니다. 그런 다음 나중에 글로벌 시장에서 추가 문화권, 지역 및 언어용 앱을 적용할 수 있습니다.

## <a name="introduction"></a>소개

앱을 만들 때 단일 언어 및 문화권 보다 더 광범위 하 게 생각 되는 경우 앱이 새 시장으로 확장 될 때 예기치 않은 문제가 발생할 수 있습니다 (있는 경우). 예를 들어 날짜, 시간, 숫자, 달력, 통화, 전화 번호, 측정 단위 및 용지 크기는 문화권 또는 언어 마다 다르게 표시 될 수 있는 모든 항목입니다.

서로 다른 지역과 문화권은 서로 다른 날짜 및 시간 형식을 사용 합니다. 여기에는 날짜의 날짜 및 월에 대 한 규칙, 시간 및 분의 구분 및 구분 기호로 사용 되는 문장 부호에 대 한 규칙이 포함 됩니다. 또한 날짜는 다양 한 긴 형식 ("수요일, 3 월 28 일 2012") 또는 짧은 형식 ("3/28/12")으로 표시 될 수 있습니다 .이는 문화권에 따라 다릅니다. 물론 요일과 월의 이름과 월의 이름과 약어는 언어 마다 다릅니다.

다른 언어에 사용 되는 형식을 미리 볼 수 있습니다. **설정**  >  **시간 & 언어**  >  **지역 & 언어**로 이동 하 고 **추가 날짜, 시간 & 지역 설정**  >  **날짜, 시간 또는 숫자 형식 변경**을 클릭 합니다. **서식 탭의** **서식** 드롭다운에서 언어를 선택 하 고 **예제**에서 형식을 미리 봅니다.

이 항목에서는 "사용자 프로필 언어 목록", "앱 매니페스트 언어 목록" 및 "app runtime language list" 라는 용어를 사용 합니다. 이러한 용어의 의미와 해당 값에 액세스 하는 방법에 대 한 자세한 내용은 [사용자 프로필 언어 및 앱 매니페스트 언어 이해](manage-language-and-region.md)를 참조 하세요.

## <a name="format-dates-and-times-for-the-app-runtime-language-list"></a>앱 런타임 언어 목록에 대 한 날짜 및 시간 형식 지정

사용자가 날짜를 선택 하도록 허용 하거나 시간을 선택 해야 하는 경우 표준 [달력, 날짜 및 시간 컨트롤](../controls-and-patterns/date-and-time.md)을 사용 합니다. 앱 런타임 언어 목록에 대해 가장 적합 한 날짜 및 시간 형식을 자동으로 사용 합니다.

날짜나 시간을 직접 표시 해야 하는 경우 [**DateTimeFormatter**](/uwp/api/windows.globalization.datetimeformatting?branch=live) 클래스를 사용할 수 있습니다. 기본적으로 **DateTimeFormatter** 는 앱 런타임 언어 목록에 대해 가장 적합 한 날짜 및 시간 형식을 자동으로 사용 합니다. 따라서 아래 코드는 해당 목록에 대 한 가장 좋은 방법으로 지정 된 **날짜/시간** 을 지정 합니다. 예를 들어, 응용 프로그램 매니페스트 언어 목록에 영어 (미국) (기본값) 및 독일어 (독일)가 포함 되어 있다고 가정 합니다. 현재 날짜가 11 월 6 2017이 고 사용자 프로필 언어 목록에 독일어 (독일)를 먼저 포함 하는 경우 포맷터는 "06.11.2017"을 제공 합니다. 사용자 프로필 언어 목록에 영어 (미국)를 먼저 포함 하는 경우 (또는 영어와 독일어가 모두 포함 되어 있지 않은 경우) 포맷터는 "en-us"가 일치 하거나 기본값으로 사용 되기 때문에 "11/6/2017"을 제공 합니다.

```csharp
    // Use the DateTimeFormatter class to display dates and times using basic formatters.

    var shortDateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shortdate");
    var shortTimeFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shorttime");

    var dateTimeToFormat = DateTime.Now;

    var shortDate = shortDateFormatter.Format(dateTimeToFormat);
    var shortTime = shortTimeFormatter.Format(dateTimeToFormat);

    var results = "Short Date: " + shortDate + "\n" +
                  "Short Time: " + shortTime;
```

다음과 같이 사용자 고유의 PC에서 위의 코드를 테스트할 수 있습니다.

- "En-us" 및 "de-de" 모두에 대 한 리소스 파일이 프로젝트에 포함 되어 있는지 확인 합니다 ( [언어, 크기 조정, 고대비 및 기타 한정자를 위해 리소스를 조정](../../app-resources/tailor-resources-lang-scale-contrast.md)하는 방법 참조).
- **설정**  >  **시간 & 언어**  >  **지역 & 언어**  >  **언어로**사용자 프로필 언어 목록을 변경 합니다. 독일어 (독일)를 추가 하 고이를 기본값으로 설정 하 고 코드를 다시 실행 합니다.

## <a name="format-dates-and-times-for-the-user-profile-language-list"></a>사용자 프로필 언어 목록에 대 한 날짜 및 시간 형식 지정

기본적으로 **DateTimeFormatter** 는 앱 런타임 언어 목록과 일치 합니다. 이렇게 하면 "date is date"와 같은 문자열을 표시 하는 경우 &lt; &gt; 언어는 날짜 형식과 일치 하 게 됩니다.

사용자 프로필 언어 목록에 따라서만 날짜 및/또는 시간 형식을 지정 하려는 어떤 이유로 든, 아래 예제와 같은 코드를 사용 하 여이 작업을 수행할 수 있습니다. 그러나 이렇게 하면 사용자가 앱에서 번역 된 문자열을 사용 하지 않는 언어를 선택할 수 있음을 이해 해야 합니다. 예를 들어 앱이 독일어 (독일)로 지역화 되지 않지만 사용자가 기본 설정 언어로 선택 하는 경우에는 "the date is 06.11.2017"와 같이 매우 이상한 문자열이 표시 될 수 있습니다.

```csharp
    // Use the DateTimeFormatter class to display dates and times using basic formatters.

    var userLanguages = Windows.System.UserProfile.GlobalizationPreferences.Languages;

    var shortDateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shortdate", userLanguages);

    var results = "Short Date: " + shortDateFormatter.Format(DateTime.Now);
```

## <a name="format-numbers-and-currencies-appropriately"></a>숫자 및 통화를 적절 하 게 서식 지정

문화권 마다 숫자가 다르게 지정 됩니다. 표시할 소수 자릿수, 소수점 구분 기호로 사용할 문자 및 사용할 통화 기호 등이 문화별 숫자 형식 차이점에 포함될 수 있습니다. 숫자 [**형식 지정**](/uwp/api/windows.globalization.numberformatting?branch=live) 네임 스페이스의 클래스를 사용 하 여 decimal, percent 또는 permille 숫자와 통화를 표시 합니다. 대부분의 경우 이러한 포맷터 클래스는 사용자 프로필에 가장 적합 한 형식을 사용 하려고 합니다. 그러나 포맷터를 사용 하 여 지역 또는 형식에 대 한 통화를 표시할 수 있습니다.

이 예에서는 사용자 프로필 및 지정 된 특정 통화 시스템에 대해 통화를 표시 하는 방법을 보여 줍니다.

```csharp
    // This scenario uses the CurrencyFormatter class to format a number as a currency.

    var userCurrency = Windows.System.UserProfile.GlobalizationPreferences.Currencies[0];

    var valueToBeFormatted = 12345.67;

    var userCurrencyFormatter = new Windows.Globalization.NumberFormatting.CurrencyFormatter(userCurrency);
    var userCurrencyValue = userCurrencyFormatter.Format(valueToBeFormatted);

    // Create a formatter initialized to a specific currency,
    // in this case US Dollar (specified as an ISO 4217 code) 
    // but with the default number formatting for the current user.
    var currencyFormatUSD = new Windows.Globalization.NumberFormatting.CurrencyFormatter("USD");
    var currencyValueUSD = currencyFormatUSD.Format(valueToBeFormatted);

    // Create a formatter initialized to a specific currency.
    // In this case it's the Euro with the default number formatting for France.
    var currencyFormatEuroFR = new Windows.Globalization.NumberFormatting.CurrencyFormatter("EUR", new[] { "fr-FR" }, "FR");
    var currencyValueEuroFR = currencyFormatEuroFR.Format(valueToBeFormatted);

    // Results for display.
    var results = "Fixed number (" + valueToBeFormatted + ")\n" +
                    "With user's default currency: " + userCurrencyValue + "\n" +
                    "Formatted US Dollar: " + currencyValueUSD + "\n" +
                    "Formatted Euro (fr-FR defaults): " + currencyValueEuroFR;
```

**설정**  >  **시간 & 언어**  >  **지역 & 언어**  >  **국가 또는 지역**에서 국가 또는 지역을 변경 하 여 위의 코드를 사용자 PC에서 테스트할 수 있습니다. 국가 또는 지역 (아이슬란드)을 선택 하 고 코드를 다시 실행 합니다.

## <a name="use-a-culturally-appropriate-calendar"></a>문화권에 맞는 달력 사용

달력은 지역 및 언어에 따라 다릅니다. 양력 달력은 모든 지역에 대 한 기본값이 아닙니다. 일부 지역의 사용자는 일본 연대 달력 또는 아랍어 음력 달력과 같은 대체 달력을 선택할 수 있습니다. 달력의 날짜와 시간은 다른 표준 시간대 및 일광 절약 시간에도 중요 합니다.

기본 달력 형식이 사용 되도록 하려면 표준 [달력, 날짜 및 시간 컨트롤](../controls-and-patterns/date-and-time.md)을 사용 하면 됩니다. 일정 날짜에 대 한 작업을 직접 수행 해야 하는 더 복잡 한 시나리오의 경우, **Windows 전역화** 는 지정 된 문화권, 지역 및 달력 유형에 대 한 적절 한 달력 표현을 제공 하는 [**달력**](/uwp/api/windows.globalization.calendar?branch=live) 클래스를 제공 합니다.

## <a name="format-phone-numbers-appropriately"></a>전화 번호 형식 적절

전화 번호는 지역에 따라 형식이 다르게 지정됩니다. 숫자의 수, 숫자를 그룹화 하는 방법 및 전화 번호의 특정 부분에 대 한 중요도는 국가 마다 다릅니다. Windows 10 버전 1607부터 [**PhoneNumberFormatting**](/uwp/api/windows.globalization.phonenumberformatting?branch=live) 네임 스페이스의 클래스를 사용 하 여 현재 지역에 맞게 전화 번호의 형식을 지정할 수 있습니다.

[**PhoneNumberInfo**](/uwp/api/windows.globalization.phonenumberformatting.phonenumberinfo?branch=live) 는 숫자 문자열을 구문 분석 하 여 숫자가 현재 지역에서 유효한 전화 번호 인지 여부를 확인 하는 데 사용할 수 있습니다. 두 숫자가 같은지 비교 합니다. 전화 번호의 다양 한 기능 부분 (예: 국가 코드 또는 지역 번호)을 추출 합니다.

[**PhoneNumberFormatter**](/uwp/api/windows.globalization.phonenumberformatting.phonenumberformatter?branch=live) 는 숫자 문자열이 부분 전화 번호를 나타내는 경우에도 숫자 문자열이 나 표시에 대 한 **PhoneNumberInfo** 형식을 지정 합니다. 사용자가 숫자를 입력 하는 경우이 부분 숫자 서식 지정을 사용 하 여 숫자의 형식을 지정할 수 있습니다.

아래 예제에서는 **PhoneNumberFormatter** 를 사용 하 여 입력 하는 전화 번호의 형식을 지정 하는 방법을 보여 줍니다. PhoneNumberInputTextBox **라는 텍스트 상자에서** 매번 텍스트가 변경 될 때마다 텍스트 상자의 내용이 현재 기본 영역을 사용 하 여 서식이 지정 되 고 PhoneNumberOutputTextBlock 라는 **TextBlock** 에 표시 됩니다. 데모용으로 문자열은 뉴질랜드의 경우 지역을 사용 하 여 서식이 지정 되 고 phoneNumberOutputTextBlockNZ 라는 TextBlock에 표시 됩니다.
  
```csharp
    using Windows.Globalization.PhoneNumberFormatting;

    PhoneNumberFormatter currentFormatter, NZFormatter;

    public MainPage()
    {
        this.InitializeComponent();

        // Use the default formatter for the current region
        this.currentFormatter = new PhoneNumberFormatter();

        // Create an explicit formatter for New Zealand. 
        PhoneNumberFormatter.TryCreate("NZ", out this.NZFormatter);
    }

    private void phoneNumberInputTextBox_TextChanged(object sender, TextChangedEventArgs e)
    {
        // Format for the default region.
        this.phoneNumberOutputTextBlock.Text = currentFormatter.FormatPartialString(this.phoneNumberInputTextBox.Text);

        // If the NZFormatter was created successfully, format the partial string for the NZ TextBlock.
        if(this.NZFormatter != null)
        {
            this.phoneNumberOutputTextBlockNZ.Text = this.NZFormatter.FormatPartialString(this.phoneNumberInputTextBox.Text);
        }
    }
```    

**설정**  >  **시간 & 언어**  >  **지역 & 언어**  >  **국가 또는 지역**에서 국가 또는 지역을 변경 하 여 위의 코드를 사용자 PC에서 테스트할 수 있습니다. 국가 또는 지역 (뉴질랜드)을 선택 하 여 형식이 일치 하는지 확인 하 고 코드를 다시 실행 합니다. 테스트 데이터의 경우 뉴질랜드의 비즈니스 전화 번호에 대 한 웹 검색을 수행할 수 있습니다.

## <a name="the-users-language-and-cultural-preferences"></a>사용자의 언어 및 문화권 기본 설정

사용자의 언어, 지역 또는 문화권 기본 설정에 따라서만 다른 기능을 제공 하려는 시나리오의 경우 Windows에서는Windows.System를 통해 이러한 기본 설정에 액세스 하는 방법을 제공 [**합니다. GlobalizationPreferences**](/uwp/api/windows.system.userprofile.globalizationpreferences?branch=live). 필요한 경우 **GlobalizationPreferences** 클래스를 사용 하 여 사용자의 현재 지역, 기본 설정 언어, 기본 통화 등의 값을 가져옵니다. 하지만 앱의 문자열/이미지가 사용자의 기본 언어에 맞게 지역화 되지 않은 경우에는 날짜 및 시간 및 해당 기본 설정 언어에 대해 서식이 지정 된 다른 데이터가 표시 되는 문자열과 일치 하지 않습니다.

## <a name="important-apis"></a>중요 API

* [DateTimeFormatter](/uwp/api/windows.globalization.datetimeformatting?branch=live)
* [Windows.globalization.numberformatting](/uwp/api/windows.globalization.numberformatting?branch=live)
* [캘린더](/uwp/api/windows.globalization.calendar?branch=live)
* [PhoneNumberFormatting](/uwp/api/windows.globalization.phonenumberformatting?branch=live)
* [GlobalizationPreferences](/uwp/api/windows.system.userprofile.globalizationpreferences?branch=live)

## <a name="related-topics"></a>관련 항목

* [달력, 날짜 및 시간 컨트롤](../controls-and-patterns/date-and-time.md)
* [사용자 프로필 언어 및 앱 매니페스트 언어 이해](manage-language-and-region.md)
* [언어, 배율, 고대비 및 기타 한정자에 맞게 리소스 조정](../../app-resources/tailor-resources-lang-scale-contrast.md)

## <a name="samples"></a>샘플

* [달력 세부 정보 및 수학 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Calendar%20details%20and%20math%20sample%20(Windows%208))
* [날짜 및 시간 형식 지정 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Date%20and%20time%20formatting%20sample%20(Windows%208))
* [세계화 기본 설정 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Globalization%20preferences%20sample%20(Windows%208))
* [숫자 형식 지정 및 구문 분석 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Number%20formatting%20and%20parsing%20sample%20(Windows%208))
