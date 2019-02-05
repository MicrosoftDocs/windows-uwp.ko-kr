---
Description: Design your app to be global-ready by appropriately formatting dates, times, numbers, phone numbers, and currencies. You'll then be able later to adapt your app for additional cultures, regions, and languages in the global market.
title: 날짜/시간 숫자 형식 세계화
ms.assetid: 6ECE8BA4-9A7D-49A6-81EE-AB2BE7F0254F
template: detail.hbs
ms.date: 11/07/2017
ms.topic: article
keywords: windows 10, uwp, 세계화, 현지화, 지역화
ms.localizationpriority: medium
ms.openlocfilehash: d641bcff48b830c56a1d03ee861ec2a4c5f433b6
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/05/2019
ms.locfileid: "9048610"
---
# <a name="globalize-your-datetimenumber-formats"></a>날짜/시간 숫자 형식 세계화

날짜, 시간, 숫자, 전화 번호 및 통화의 형식을 적절하게 지정하여 세계화를 대비한 앱을 디자인합니다. 그러면 나중에 전 세계 지역/국가의 다른 문화, 지역 및 언어에 맞춰 앱을 조정할 수 있습니다.

## <a name="introduction"></a>소개

앱을 만들 때 하나의 언어 및 문화가 아니라 더 광범위하게 생각하면 앱이 신흥 시장에서 성장함에 따라 예기치 않게 직면하게 될 문제가 적어집니다. 예를 들어 날짜, 시간, 숫자, 달력, 통화, 전화 번호, 측정 단위 및 용지 크기는 문화 또는 언어마다 다르게 표시될 수 있는 항목입니다.

지역 및 문화에 따라 다른 언어 및 시간 형식을 사용합니다. 날짜의 일과 월의 순서, 시간 및 분 분리, 구분 기호로 사용되는 문자 부호 등에 대한 규칙이 여기에 포함됩니다. 또한 날짜의 경우 문화마다 다양한 긴 형식("2012년 3월 28일, 수요일") 또는 짧은 형식("12/03/28")으로 표시할 수 있습니다. 요일과 월의 이름 및 약어도 언어마다 다릅니다.

다른 언어에 사용되는 형식을 미리 볼 수 있습니다. **설정** > **시간 및 언어** > **지역 및 언어**로 이동하고 **추가 날짜, 시간 및 국가별 설정** > **날짜, 시간 또는 숫자 형식 변경**을 클릭합니다. **형식** 탭의 **형식** 드롭다운에서 언어를 선택하고 **예**에서 서식을 미리봅니다.

이 항목에서는 "사용자 프로필 언어 목록", "앱 매니페스트 언어 목록" 및 "앱 런타임 언어 목록"이라는 용어를 사용합니다. 이러한 용어의 정확한 의미 및 값에 액세스하는 방법에 대한 자세한 내용은 [사용자 프로필 언어 및 앱 매니페스트 언어 이해](manage-language-and-region.md)를 참조하세요.

## <a name="format-dates-and-times-for-the-app-runtime-language-list"></a>앱 런타임 언어 목록에 대한 날짜 및 시간 형식

사용자가 날짜 또는 시간을 선택하도록 해야 할 경우에는 표준 [달력, 날짜 및 시간 컨트롤](../controls-and-patterns/date-and-time.md)을 사용하세요. 이러한 컨트롤은 앱 런타임 언어 목록에 가장 적합한 날짜 및 시간 형식을 자동으로 사용합니다.

날짜 또는 시간을 사용자에 맞게 표시해야 하는 경우 [**DateTimeFormatter**](/uwp/api/windows.globalization.datetimeformatting?branch=live) 클래스를 사용할 수 있습니다. 기본적으로 **DateTimeFormatter**는 앱 런타임 언어 목록에 가장 적합한 날짜 및 시간 형식을 자동으로 사용합니다. 따라서 아래의 코드는 특정 **DateTime**을 해당 목록에 가장 적합한 방식으로 형식을 지정합니다. 예를 들어 앱 매니페스트 언어 목록에 기본 언어인 영어(미국)와 독일어(독일)가 포함되어 있다고 가정합니다. 현재 날짜는 2017년 11월 6일이고 사용자 프로필 언어 목록에 독일어(독일)이 먼저 포함된 경우 포맷터에서는 06.11.2017"을 제공합니다. 사용자 프로필 언어 목록에 영어(미국)이 먼저 포함된 경우(또는 영어 및 독일어가 모두 포함되지 않은 경우) 포맷터는 "11/6/2017"을 제공합니다("en-US"가 일치하거나 기본적으로 사용되는 경우).

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

다음과 같이 자신의 PC에서 위 코드를 테스트할 수 있습니다.

- 프로젝트에 "en-US" 및 "de-DE" 모두에 정규화된 리소스 파일이 있어야 합니다([언어, 배율, 고대비 및 기타 한정자에 맞게 리소스 조정](../../app-resources/tailor-resources-lang-scale-contrast.md)).
- **설정** > **시간 및 언어** > **지역 및 언어** > **언어**에서 사용자 프로필 언어 목록을 변경합니다. 독일어(독일)을 추가하고 이 언어를 기본 언어로 설정하고 코드를 다시 실행합니다.

## <a name="format-dates-and-times-for-the-user-profile-language-list"></a>사용자 프로필 언어 목록의 날짜 및 시간 형식

기본적으로 **DateTimeFormatter**는 앱 런타임 언어 목록과 일치합니다. 그러면 "날짜는 &lt;날짜&gt;입니다."와 같은 문자열을 표시하는 경우 언어가 날짜 형식에 일치하게 됩니다.

이유에 관계없이 사용자 프로필 언어 목록만 따르도록 날짜 및/또는 시간을 포맷하려는 경우 아래 예와 같은 코드를 사용하여 그렇게 할 수 있습니다. 그러나 이 경우 사용자는 번역되지 않은 문자열이 포함된 앱의 언어를 선택할 수 있습니다. 예들 들어, 앱이 독일어(독일)로 번역되지 않았지만 사용자가 기본 설정 언어로 선택하는 경우 "날짜는 06.11.2017입니다."와 같이 이상하게 표시될 수 있습니다.

```csharp
    // Use the DateTimeFormatter class to display dates and times using basic formatters.

    var userLanguages = Windows.System.UserProfile.GlobalizationPreferences.Languages;

    var shortDateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shortdate", userLanguages);

    var results = "Short Date: " + shortDateFormatter.Format(DateTime.Now);
```

## <a name="format-numbers-and-currencies-appropriately"></a>숫자 및 통화의 형식을 적절하게 지정

각 문화에는 저마다의 숫자 형식이 있습니다. 표시할 소수 자릿수, 소수점 구분 기호로 사용할 문자 및 사용할 통화 기호 등이 문화권별 숫자 형식 차이점에 포함될 수 있습니다. [**NumberFormatting**](/uwp/api/windows.globalization.numberformatting?branch=live) 네임스페이스에 클래스를 사용하여 소수점, 백분율 또는 천분율 숫자, 통화를 표시합니다. 대부분의 경우 사용자 프로필에 가장 적합한 형식을 포맷터 클래스에 사용하려고 할 것입니다. 그러나 포맷터를 사용하여 특정 지역 또는 형식의 통화를 표시할 수 있습니다.

다음 예는 사용자 프로필 및 특정 통화 체계에 따라 통화를 표시하는 방법을 보여 줍니다.

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

**설정** > **시간 및 언어** > **지역 및 언어** > **국가 및 지역**에서 국가 또는 지역을 변경하여 사용자의 PC에서 위의 코드를 테스트할 수 있습니다. 국가 또는 지역(예: 아이슬란드)을 선택하고 코드를 다시 실행합니다.

## <a name="use-a-culturally-appropriate-calendar"></a>문화적으로 적절한 달력 사용

달력은 지역과 언어에 따라 다릅니다. 모든 문화에서 양력이 기본 설정인 것은 아닙니다. 일부 지역의 사용자는 일본 연도 또는 아랍 음력 같은 대체 달력을 선택하기도 합니다. 달력의 날짜 및 시간 또한 서로 다른 표준 시간대 및 일광 절약 시간을 따릅니다.

기본 설정 달력 형식이 사용되도록 하려면 표준 [달력, 날짜 및 시간 컨트롤](../controls-and-patterns/date-and-time.md)을 사용할 수 있습니다. 달력 날짜로 직접 작업해야 하는 더 복잡한 시나리오의 경우 **Windows.Globalization**은 지정된 문화, 지역 및 달력 유형에 적절한 달력 표시를 제공하는 [**Calendar**](/uwp/api/windows.globalization.calendar?branch=live) 클래스를 제공합니다.

## <a name="format-phone-numbers-appropriately"></a>적절한 전화 번호 형식 지정

전화 번호는 지역에 따라 형식이 다르게 지정됩니다. 자릿수, 숫자 그룹화 방법, 전화 번호 중 특정 부분의 의미는 국가마다 다릅니다. Windows 10 버전 1607부터 [**PhoneNumberFormatting**](/uwp/api/windows.globalization.phonenumberformatting?branch=live)네임스페이스에 클래스를 사용하여 전화 번호의 형식을 현재 지역에 맞게 설정할 수 있습니다.

[**PhoneNumberInfo**](/uwp/api/windows.globalization.phonenumberformatting.phonenumberinfo?branch=live)는 숫자 문자열의 구문을 분석하여 숫자가 현재 지역의 전화 번호에 유효한지 확인하고, 두 번호가 동일한지 비교하고, 국가 코드 또는 지리적 영역 코드 등 전화 번호의 다양한 기능 부분을 추출할 수 있도록 해줍니다.

[**PhoneNumberFormatter**](/uwp/api/windows.globalization.phonenumberformatting.phonenumberformatter?branch=live)는 숫자 문자열이 전화 번호의 일부를 나타내는 경우에도 숫자 문자열 또는 **PhoneNumberInfo**의 표시 형식을 설정합니다. 이 부분 숫자 형식을 사용하여 사용자가 숫자를 입력할 때 숫자의 형식을 설정할 수 있습니다.

아래 예에서는 전화 번호를 입력할 때 **PhoneNumberFormatter**를 사용하여 전화 번호의 형식을 설정하는 방법을 보여 줍니다. phoneNumberInputTextBox라는 이름의 **TextBox**에서 텍스트가 변경될 때마다 입력란의 내용이 현재 기본 지역을 사용하여 형식이 설정되고 phoneNumberOutputTextBlock라는 **TextBlock**에 표시됩니다. 여기서는 설명을 위해 뉴질랜드 지역을 사용하여 문자열의 형식이 설정되고 phoneNumberOutputTextBlockNZ라는 TextBlock에 표시됩니다.
  
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

**설정** > **시간 및 언어** > **지역 및 언어** > **국가 및 지역**에서 국가 또는 지역을 변경하여 사용자의 PC에서 위의 코드를 테스트할 수 있습니다. 국가 또는 지역(예: 형식이 일치하는지 확인하기 위해 뉴질랜드 선택)을 선택하고 코드를 다시 실행합니다. 테스트 데이터의 경우 뉴질랜드의 회사 전화번호를 웹에서 검색할 수 있습니다.

## <a name="the-users-language-and-cultural-preferences"></a>사용자의 언어 및 문화 기본 설정

사용자의 언어, 지역 또는 문화 기본 설정에 따라서만 다른 기능을 제공하려는 시나리오의 경우 Windows는 [**Windows.System.UserProfile.GlobalizationPreferences**](/uwp/api/windows.system.userprofile.globalizationpreferences?branch=live)를 통해 이러한 기본 설정에 액세스할 수 있는 방법을 제공합니다. 필요한 경우 **GlobalizationPreferences** 클래스를 사용하여 사용자의 현재 지역, 기본 설정 언어, 기본 설정 통화 등의 값을 가져옵니다. 앱 문자열/이미지가 사용자의 기본 설정 언어에 맞게 지역화되지 않은 경우 기본 설정된 언어 형식으로 설정된 날짜, 시간 및 기타 데이터가 표시되는 문자열과 일치하지 않게 됩니다.

## <a name="important-apis"></a>중요 API

* [DateTimeFormatter](/uwp/api/windows.globalization.datetimeformatting?branch=live)
* [NumberFormatting](/uwp/api/windows.globalization.numberformatting?branch=live)
* [달력](/uwp/api/windows.globalization.calendar?branch=live)
* [PhoneNumberFormatting](/uwp/api/windows.globalization.phonenumberformatting?branch=live)
* [GlobalizationPreferences](/uwp/api/windows.system.userprofile.globalizationpreferences?branch=live)

## <a name="related-topics"></a>관련 항목

* [달력, 날짜 및 시간 컨트롤](../controls-and-patterns/date-and-time.md)
* [사용자 프로필 언어와 앱 매니페스트 언어 이해](manage-language-and-region.md)
* [언어, 배율, 고대비 및 기타 한정자에 맞게 리소스 조정](../../app-resources/tailor-resources-lang-scale-contrast.md)

## <a name="samples"></a>샘플

* [달력 세부 정보 및 수학 샘플](https://go.microsoft.com/fwlink/p/?linkid=231636)
* [날짜 및 시간 형식 지정 샘플](https://go.microsoft.com/fwlink/p/?linkid=231618)
* [세계화 기본 설정 샘플](https://go.microsoft.com/fwlink/p/?linkid=231608)
* [숫자 형식 지정 및 구문 분석 샘플](https://go.microsoft.com/fwlink/p/?linkid=231620)
