---
author: DelfCo
Description: "날짜, 시간, 숫자, 전화 번호 및 통화의 형식을 적절하게 지정하여 세계화를 대비한 앱을 개발하세요."
title: "세계화를 대비한 형식 사용"
ms.assetid: 6ECE8BA4-9A7D-49A6-81EE-AB2BE7F0254F
label: Use global-ready formats
template: detail.hbs
ms.author: bobdel
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: a733f3d87f2e6598e49d8926a7b10797e335c79c
ms.lasthandoff: 02/07/2017

---

# <a name="use-global-ready-formats"></a>세계화를 대비한 형식 사용

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

날짜, 시간, 숫자, 전화 번호 및 통화의 형식을 적절하게 지정하여 세계화를 대비한 앱을 개발하세요. 그러면 나중에 전 세계 지역/국가의 다른 문화, 지역 및 언어에 맞춰 앱을 조정할 수 있습니다.

<div class="important-apis" >
<b>중요 API</b><br/>
<ul>
<li>[**Windows.Globalization.Calendar**](https://msdn.microsoft.com/library/windows/apps/br206724)</li>
<li>[**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)</li>
<li>[**Windows.Globalization.NumberFormatting**](https://msdn.microsoft.com/library/windows/apps/br226136)</li>
<li>[**Windows.Globalization.PhoneNumberFormatting**](https://msdn.microsoft.com/library/windows/apps/Windows.Globalization.PhoneNumberFormatting)</li>
</ul>
</div>


## <a name="introduction"></a>소개

많은 앱 개발자들은 기본적으로 자신의 언어 및 문화만을 고려하여 앱을 만듭니다. 그러나 앱이 다른 시장으로 확대되면 새 언어 및 지역에 맞게 앱을 조정하는 것이 예상치 않게 어려울 수 있습니다. 예를 들어 날짜, 시간, 숫자, 달력, 통화, 전화 번호, 측정 단위 및 용지 크기는 문화 또는 언어마다 다르게 표시될 수 있는 항목입니다.

앱을 개발할 때 몇 가지 사항만 고려하면 간단한 절차를 통해 새로운 지역/국가에 맞게 조정할 수 있습니다.

## <a name="format-dates-and-times-appropriately"></a>날짜 및 시간의 형식을 적절하게 지정

날짜 및 시간을 적절하게 표시하는 방법에는 여러 가지가 있습니다. 날짜의 일과 월의 순서, 시간 및 분 분리, 구분 기호로 사용되는 문자 부호 등에 대한 규칙은 지역과 문화마다 다릅니다. 또한 날짜의 경우 문화마다 다양한 긴 형식("2012년 3월 28일, 수요일") 또는 짧은 형식("12/03/28")으로 표시할 수 있습니다. 요일과 월의 이름 및 약어도 언어마다 다릅니다.

사용자가 날짜 또는 시간을 선택하도록 해야 할 경우에는 표준 [날짜 및 시간 선택기](https://msdn.microsoft.com/library/windows/apps/hh465466) 컨트롤을 사용하세요. 이러한 컨트롤은 자동으로 사용자의 기본 설정 언어와 지역에 맞는 날짜 및 시간 형식을 사용합니다.

날짜 또는 시간을 자동으로 표시해야 할 경우에는 [**Date/Time**](https://msdn.microsoft.com/library/windows/apps/br206859) 및 [**Number**](https://msdn.microsoft.com/library/windows/apps/br226136) 포맷터를 사용하여 날짜, 시간 및 숫자에 대한 사용자의 기본 설정 형식을 자동으로 표시합니다. 아래 코드는 기본 설정 언어/지역을 사용하여 특정 DateTime의 형식을 지정합니다. 예를 들어 현재 날짜가 2012년 6월 3일인 경우 포맷터는 사용자의 기본 설정 언어가 영어(미국)이면 "6/3/2012"를 표시하고 독일어(독일)이면 "03.06.2012"를 표시합니다.

```CSharp
    // Use the Windows.Globalization.DateTimeFormatting.DateTimeFormatter class
    // to display dates and times using basic formatters.

    // Formatters for dates and times, using shortdate format.
    var sdatefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shortdate");
    var stimefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shorttime");

    // Obtain the date that will be formatted.
    var dateToFormat = DateTime.Now;

    // Perform the actual formatting.
    var sdate = sdatefmt.Format(dateToFormat);
    var stime = stimefmt.Format(dateToFormat);

    // Results for display.
    var results = "Short Date: " + sdate + "\n" +
                  "Short Time: " + stime;
```

## <a name="format-numbers-and-currencies-appropriately"></a>숫자 및 통화의 형식을 적절하게 지정

각 문화에는 저마다의 숫자 형식이 있습니다. 표시할 소수 자릿수, 소수점 구분 기호로 사용할 문자 및 사용할 통화 기호 등이 문화별 숫자 형식 차이점에 포함될 수 있습니다. [**NumberFormatting**](https://msdn.microsoft.com/library/windows/apps/br226136)을 사용하여 소수점, 백분율 또는 천분율 숫자, 통화를 표시합니다. 대부분의 경우는 사용자의 현재 기본 설정에 따라 간단하게 숫자 또는 통화를 표시합니다. 그러나 포맷터를 사용하여 특정 지역 또는 형식의 통화를 표시할 수도 있습니다.

아래 코드는 사용자의 기본 설정 언어 및 지역에 맞게 통화를 표시하거나 지정된 특정 통화 시스템에 맞게 통화를 표시하는 방법의 예를 보여 줍니다.

```CSharp
    // This scenario uses the Windows.Globalization.NumberFormatting.CurrencyFormatter class
    // to format a number as a currency.

    // Determine the current user's default currency.
    var userCurrency = Windows.System.UserProfile.GlobalizationPreferences.Currencies[0];

    // Number to be formatted.
    var fractionalNumber = 12345.67;

    // Currency formatter using the current user's preference settings for number formatting.
    var userCurrencyFormat = new Windows.Globalization.NumberFormatting.CurrencyFormatter(userCurrency);
    var currencyDefault = userCurrencyFormat.Format(fractionalNumber);

    // Create a formatter initialized to a specific currency,
    // in this case US Dollar (specified as an ISO 4217 code) 
    // but with the default number formatting for the current user.
    var currencyFormatUSD = new Windows.Globalization.NumberFormatting.CurrencyFormatter("USD"); 
    var currencyUSD = currencyFormatUSD.Format(fractionalNumber);

    // Create a formatter initialized to a specific currency.
    // In this case it's the Euro with the default number formatting for France.
    var currencyFormatEuroFR = new Windows.Globalization.NumberFormatting.CurrencyFormatter("EUR", new[] { "fr-FR" }, "FR");
    var currencyEuroFR = currencyFormatEuroFR.Format(fractionalNumber);

    // Results for display.
    var results = "Fixed number (" + fractionalNumber + ")\n" +
                  "With user's default currency: " + currencyDefault + "\n" +
                  "Formatted US Dollar: " + currencyUSD + "\n" +
                  "Formatted Euro (fr-FR defaults): " + currencyEuroFR;
```

## <a name="use-a-culturally-appropriate-calendar"></a>문화적으로 적절한 달력 사용

달력은 지역과 언어에 따라 다릅니다. 모든 문화에서 양력이 기본 설정인 것은 아닙니다. 일부 지역의 사용자는 일본 연도 또는 아랍 음력 같은 대체 달력을 선택하기도 합니다. 달력의 날짜 및 시간 또한 서로 다른 표준 시간대 및 일광 절약 시간을 따릅니다.

표준 [날짜 및 시간 선택기](https://msdn.microsoft.com/library/windows/apps/hh465466) 컨트롤을 사용하면 사용자가 날짜를 선택할 때 기본 설정 달력 형식이 사용됩니다. 달력 날짜로 직접 작업해야 하는 더 복잡한 시나리오의 경우 Windows.Globalization은 지정된 문화, 지역 및 달력 유형에 적절한 달력 표시를 제공하는 [**Calendar**](https://msdn.microsoft.com/library/windows/apps/br206724) 클래스를 제공합니다.

## <a name="format-phone-numbers-appropriately"></a>적절한 전화 번호 형식 지정
전화 번호는 지역에 따라 형식이 다르게 지정됩니다. 자릿수, 숫자 그룹화 방법, 전화 번호 중 특정 부분의 의미는 국가마다 다릅니다. Windows 10 버전 1607부터 [**PhoneNumberFormatting**](https://msdn.microsoft.com/library/windows/apps/Windows.Globalization.PhoneNumberFormatting)을 사용하여 전화 번호의 형식을 현재 지역에 맞게 설정할 수 있습니다.

[**PhoneNumberInfo**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.globalization.phonenumberformatting.phonenumberinfo.aspx)는 숫자 문자열의 구문을 분석하여 숫자가 현재 지역의 전화 번호에 유효한지 확인하고, 두 번호가 동일한지 비교하고, 국가 코드 또는 지리적 영역 코드 등 전화 번호의 다양한 기능 부분을 추출할 수 있도록 해줍니다.

[**PhoneNumberFormatter**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.globalization.phonenumberformatting.phonenumberformatter.aspx)는 숫자 문자열이 전화 번호의 일부를 나타내는 경우에도 숫자 문자열 또는 PhoneNumberInfo의 표시 형식을 설정합니다. 이 부분 숫자 형식을 사용하여 사용자가 숫자를 입력할 때 숫자의 형식을 설정할 수 있습니다. 

아래 코드에서는 전화 번호를 입력할 때 PhoneNumberFormatter를 사용하여 전화 번호의 형식을 설정하는 방법을 보여 줍니다. gradualInput라는 이름의 TextBox에서 텍스트가 변경될 때마다 입력란의 내용이 현재 기본 지역을 사용하여 형식이 설정되고 outBox라는 TextBlock에 표시됩니다. 여기서는 설명을 위해 뉴질랜드 지역을 사용하여 문자열의 형식이 설정되고 NZOutBox라는 TextBlock에 표시됩니다.
    
```csharp
    using Windows.Globalization;

    PhoneNumberFormatter currentFormatter, NZFormatter;

    public MainPage()
    {
        this.InitializeComponent();

        // Use the default formatter for the current region
        currentFormatter = new PhoneNumberFormatter();

        // Create an explicit formatter for New Zealand. 
        // Note that you must check the results of TryCreate before you use the formatter.
        PhoneNumberFormatter.TryCreate("NZ", out NZFormatter);

    }

    private void gradualInput_TextChanged(object sender, TextChangedEventArgs e)
    {
        // Format for the default region into outBox.
        outBox.Text = currentFormatter.FormatPartialString(gradualInput.Text);

        // If the NZFormatter was created successfully, format the partial string for the NZOutBox.
        if(NZFormatter != null)
        {
            NZOutBox.Text = NZFormatter.FormatPartialString(gradualInput.Text);
        }
    }
```    

## <a name="respect-the-users-language-and-cultural-preferences"></a>사용자의 언어 및 문화 기본 설정 준수

사용자의 언어, 지역 또는 문화 기본 설정에 따라 다른 기능을 제공하는 시나리오의 경우 Windows는 [**Windows.System.UserProfile.GlobalizationPreferences**](https://msdn.microsoft.com/library/windows/apps/br241825)를 통해 이러한 기본 설정에 액세스할 수 있는 방법을 제공합니다. 필요한 경우 **GlobalizationPreferences** 클래스를 사용하여 사용자의 현재 지역, 기본 설정 언어, 기본 설정 통화 등의 값을 가져옵니다.

## <a name="related-topics"></a>관련 항목

* [세계 시장에 대한 계획](https://msdn.microsoft.com/library/windows/apps/hh465405)
* [날짜 및 시간 컨트롤에 대한 지침](https://msdn.microsoft.com/library/windows/apps/hh465466)

**참조**
* [**Windows.Globalization.Calendar**](https://msdn.microsoft.com/library/windows/apps/br206724)
* [**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)
* [**Windows.Globalization.NumberFormatting**](https://msdn.microsoft.com/library/windows/apps/br226136)
* [**Windows.System.UserProfile.GlobalizationPreferences**](https://msdn.microsoft.com/library/windows/apps/br241825)

**샘플**
* [달력 세부 정보 및 수학 샘플](http://go.microsoft.com/fwlink/p/?linkid=231636)
* [날짜 및 시간 형식 지정 샘플](http://go.microsoft.com/fwlink/p/?linkid=231618)
* [세계화 기본 설정 샘플](http://go.microsoft.com/fwlink/p/?linkid=231608)
* [숫자 형식 지정 및 구문 분석 샘플](http://go.microsoft.com/fwlink/p/?linkid=231620)

