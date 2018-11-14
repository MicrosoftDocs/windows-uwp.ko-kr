---
author: Xansky
ms.assetid: 141900dd-f1d3-4432-ac8b-b98eaa0b0da2
description: XAML 앱에서 Microsoft Advertising 라이브러리를 사용할 때 발생하는 일반적인 개발 문제에 대한 해결 방법을 알아봅니다.
title: XAML과 C# 문제 해결 가이드
ms.author: mhopkins
ms.date: 08/23/2017
ms.topic: article
keywords: windows 10, uwp, 광고, 광고, AdControl, 문제 해결, XAML, c#
ms.localizationpriority: medium
ms.openlocfilehash: 12789767694e4ab3fa13efec4a31c8db4acd5420
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6656210"
---
# <a name="xaml-and-c-troubleshooting-guide"></a>XAML과 C# 문제 해결 가이드

이 항목에서는 XAML 앱에서 Microsoft Advertising 라이브러리를 사용할 때 발생하는 일반적인 개발 문제에 대한 해결 방법을 알아봅니다.

* [XAML](#xaml)
  * [AdControl이 표시되지 않음](#xaml-notappearing)
  * [블랙 박스가 깜박거리다가 사라짐](#xaml-blackboxblinksdisappears)
  * [광고가 새로 고쳐지지 않음](#xaml-adsnotrefreshing)

* [C#](#csharp)
  * [AdControl이 표시되지 않음](#csharp-adcontrolnotappearing)
  * [블랙 박스가 깜박거리다가 사라짐](#csharp-blackboxblinksdisappears)
  * [광고가 새로 고쳐지지 않음](#csharp-adsnotrefreshing)

<span id="xaml"/>

## <a name="xaml"></a>XAML

<span id="xaml-notappearing"/>

### <a name="adcontrol-not-appearing"></a>AdControl이 표시되지 않음

1.  Package.appxmanifest에서**인터넷(클라이언트)** 기능이 선택되어 있는지 확인합니다.

2.  응용 프로그램 ID 및 광고 단위 ID를 확인합니다. 이러한 Id는 응용 프로그램 ID와 광고 단위 ID는 파트너 센터에서 가져온 일치 해야 합니다. 자세한 내용은 [앱에서 광고 단위 설정](set-up-ad-units-in-your-app.md#live-ad-units)을 참조하세요.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}" ApplicationId="{ApplicationID}"
                  Width="728" Height="90" />
    ```

3.  **Height** 및 **Width** 속성을 확인합니다. 이러한 속성은 [배너 광고에 지원되는 광고 크기](supported-ad-sizes-for-banner-ads.md) 중 하나로 설정되어야 합니다.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90" />
    ```

4.  요소 위치를 확인합니다. [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol)은 볼 수 있는 영역 내에 있어야 합니다.

5.  **visibility** 속성을 확인합니다. 선택적 **Visibility** 속성은 collapsed 또는 hidden으로 설정되면 안 됩니다. 이 속성은 인라인으로(아래 참조) 또는 외부 스타일 시트에 설정할 수 있습니다.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Visibility="Visible"
                  Width="728" Height="90" />
    ```

6.  **AdControl**의 부모를 확인합니다. **AdControl** 요소가 부모 요소에 있으면 부모 요소가 활성화되고 표시되어야 합니다.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <StackPanel>
        <UI:AdControl AdUnitId="{AdUnitID}"
                      ApplicationId="{ApplicationID}"
                      Width="728" Height="90" />
    </StackPanel>
    ```

7.  **AdControl**이 뷰포트에서 숨겨져 있지 않은지 확인합니다. 광고가 제대로 표시되려면 **AdControl**이 표시되어야 합니다.

8.  **ApplicationId** 및 **AdUnitId**의 라이브 값은 에뮬레이터에서 테스트하지 말아야 합니다. **AdControl**이 예상대로 작동하는지 확인하려면 **ApplicationId** 및 **AdUnitId**에 대한 [테스트 값](set-up-ad-units-in-your-app.md#test-ad-units)을 모두 사용합니다.

<span id="xaml-blackboxblinksdisappears"/>

### <a name="black-box-blinks-and-disappears"></a>블랙 박스가 깜박거리다가 사라짐

1.  이전에 나온 [AdControl이 표시되지 않음](#xaml-notappearing) 섹션의 모든 단계를 한 번 더 확인합니다.

2.  **ErrorOccurred** 이벤트를 처리하고 이벤트 처리기에 전달된 메시지를 사용하여 오류가 발생했는지와 발생한 오류 유형을 확인합니다. 자세한 내용은 [XAML/C#에서 오류 처리 연습](error-handling-in-xamlc-walkthrough.md)을 참조하세요.

    이 예제에서는 **ErrorOccurred** 이벤트 처리기를 보여 줍니다. 첫 번째 조각은 XAML UI 태그입니다.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  ErrorOccurred="adControl_ErrorOccurred" />
    <TextBlock x:Name="TextBlock1" TextWrapping="Wrap" Width="500" Height="250" />
    ```

    이 예제에서는 해당 C# 코드를 보여 줍니다.

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    private void adControl_ErrorOccurred(object sender,               
        Microsoft.Advertising.WinRT.UI.AdErrorEventArgs e)
    {
        TextBlock1.Text = e.ErrorMessage;
    }
    ```

    블랙 박스를 유발하는 가장 일반적인 오류는 "사용할 수 있는 광고가 없음”입니다. 이 오류는 요청에서 반환할 수 있는 광고가 없음을 의미합니다.

3.  [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol)은 정상적으로 동작하고 있습니다.

    기본적으로 **AdControl**은 광고를 표시할 수 없을 때 축소됩니다. 다른 요소가 같은 부모의 자식인 경우 축소된 **AdControl**의 간격을 채우기 위해 이동된 후 다음 요청이 있을 때 확장될 수 있습니다.

<span id="xaml-adsnotrefreshing"/>

### <a name="ads-not-refreshing"></a>광고가 새로 고쳐지지 않음

1.  [IsAutoRefreshEnabled](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.isautorefreshenabled) 속성을 확인합니다. 기본적으로 이 선택적 속성은 **True**로 설정됩니다. **False**로 설정된 경우 [Refresh](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.refresh) 메서드를 사용해서 다른 광고를 검색해야 합니다.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  IsAutoRefreshEnabled="True" />
    ```

2.  **Refresh** 메서드에 대한 호출을 확인합니다. 자동 새로 고침을 사용하는 경우 **Refresh**를 사용해서 다른 광고를 검색할 수 없습니다. 수동 새로 고침을 사용하는 경우 디바이스의 현재 데이터 연결에 따라 30-60초경과된 후에만 **Refresh**가 호출됩니다.

    다음 코드 조각에서는 **Refresh** 메서드를 사용하는 방법의 예를 보여 줍니다. 첫 번째 조각은 XAML UI 태그입니다.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl x:Name="adControl1"
                  AdUnitId="{AdUnit_ID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  IsAutoRefreshEnabled="False" />
    ```

    이 코드 조각은 UI 태그 뒤에 있는 C# 코드의 예를 보여 줍니다.

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    public RefreshAds()
    {
        var timer = new DispatcherTimer() { Interval = TimeSpan.FromSeconds(60) };
        timer.Tick += (s, e) => adControl1.Refresh();
        timer.Start();
    }
    ```

3.  **AdControl**은 정상적으로 동작하고 있습니다. 동일한 광고가 차례대로 두 번 이상 나타나면서 광고는 새로 고쳐지지 않는 경우가 있습니다.

<span id="csharp"/>

## <a name="c"></a>C\# #

<span id="csharp-adcontrolnotappearing"/>

### <a name="adcontrol-not-appearing"></a>AdControl이 표시되지 않음

1.  Package.appxmanifest에서**인터넷(클라이언트)** 기능이 선택되어 있는지 확인합니다.

2.  **AdControl**이 인스턴스화되었는지 확인합니다. **AdControl**이 인스턴스화되지 않으면 사용할 수 없습니다.

    > [!div class="tabbedCodeSnippets"]
    [!code-cs[AdControl](./code/AdvertisingSamples/AdControlSamples/cs/MiscellaneousSnippets.cs#Snippet1)]

3.  응용 프로그램 ID 및 광고 단위 ID를 확인합니다. 이러한 Id는 응용 프로그램 ID와 광고 단위 ID는 파트너 센터에서 가져온 일치 해야 합니다. 자세한 내용은 [앱에서 광고 단위 설정](set-up-ad-units-in-your-app.md#live-ad-units)을 참조하세요.

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    ```

4.  **Height** 및 **Width** 매개 변수를 확인합니다. 이러한 속성은 [배너 광고에 지원되는 광고 크기](supported-ad-sizes-for-banner-ads.md) 중 하나로 설정되어야 합니다.

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;adControl.Width = 728;
    ```

5.  **AdControl**이 부모 요소에 추가되었는지 확인합니다. 표시하려면 **AdControl**이 부모 컨트롤에 자식으로 추가되어야 합니다(예: **StackPanel** 또는 **Grid**).

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    ContentPanel.Children.Add(adControl);
    ```

6.  **Margin** 매개 변수를 확인합니다. **AdControl**은 볼 수 있는 영역 내에 있어야 합니다.

7.  **visibility** 속성을 확인합니다. 선택적 **Visibility** 속성은 **Visible**로 설정되어야 합니다.

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    adControl.Visibility = System.Windows.Visibility.Visible;
    ```

8.  **AdControl**의 부모를 확인합니다. 부모는 활성화되고 표시되어야 합니다.

9. **ApplicationId** 및 **AdUnitId**의 라이브 값은 에뮬레이터에서 테스트하지 말아야 합니다. **AdControl**이 예상대로 작동하는지 확인하려면 **ApplicationId** 및 **AdUnitId**에 대한 [테스트 값](set-up-ad-units-in-your-app.md#test-ad-units)을 모두 사용합니다.

<span id="csharp-blackboxblinksdisappears"/>

### <a name="black-box-blinks-and-disappears"></a>블랙 박스가 깜박거리다가 사라짐

1.  [AdControl이 표시되지 않음](#csharp-adcontrolnotappearing) 섹션의 위의 모든 단계를 한 번 더 확인합니다.

2.  **ErrorOccurred** 이벤트를 처리하고 이벤트 처리기에 전달된 메시지를 사용하여 오류가 발생했는지와 발생한 오류 유형을 확인합니다. 자세한 내용은 [XAML/C#에서 오류 처리 연습](error-handling-in-xamlc-walkthrough.md)을 참조하세요.

    다음 예제에서는 오류 호출을 구현하는 데 필요한 기본 코드를 보여 줍니다. 이 XAML 코드는 오류 메시지를 표시하는 데 사용되는 **TextBlock**을 정의합니다.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <TextBlock x:Name="TextBlock1" TextWrapping="Wrap" Width="500" Height="250" />
    ```

    이 C# 코드는 오류 메시지를 검색하고 **TextBlock**에 표시합니다.

    > [!div class="tabbedCodeSnippets"]
    [!code-cs[AdControl](./code/AdvertisingSamples/AdControlSamples/cs/MiscellaneousSnippets.cs#Snippet2)]

    블랙 박스를 발생하는 가장 일반적인 오류는 "사용할 수 있는 광고가 없음”입니다. 이 오류는 요청에서 반환할 수 있는 광고가 없음을 의미합니다.

3.  **AdControl**은 정상적으로 동작하고 있습니다. 동일한 광고가 차례대로 두 번 이상 나타나면서 광고는 새로 고쳐지지 않는 경우가 있습니다.

<span id="csharp-adsnotrefreshing"/>

### <a name="ads-not-refreshing"></a>광고가 새로 고쳐지지 않음

1.  **AdControl**의 [IsAutoRefreshEnabled](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.isautorefreshenabled.aspx) 속성이 false로 설정되어 있는지 확인하세요. 기본적으로 이 선택적 속성은 **true**로 설정되어 있습니다. **false**로 설정된 경우 **Refresh** 메서드를 사용하여 다른 광고를 검색해야 합니다.

2.  [Refresh](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.refresh.aspx) 메서드에 대한 호출을 확인합니다. 자동 새로 고침을 사용하는 경우(**IsAutoRefreshEnabled**가 **true**로 설정된 경우) **Refresh**를 사용하여 다른 광고를 검색할 수 없습니다. 수동 새로 고침을 사용하는 경우(**IsAutoRefreshEnabled**가 **false**로 설정된 경우) 디바이스의 현재 데이터 연결에 따라 최소 30-60초가 경과된 후에만 **Refresh**가 호출됩니다.

    다음 예제에서는 **Refresh** 메서드를 호출하는 방법을 보여 줍니다.

    > [!div class="tabbedCodeSnippets"]
    [!code-cs[AdControl](./code/AdvertisingSamples/AdControlSamples/cs/MiscellaneousSnippets.cs#Snippet3)]

3.  **AdControl**은 정상적으로 동작하고 있습니다. 동일한 광고가 차례대로 두 번 이상 나타나면서 광고는 새로 고쳐지지 않는 경우가 있습니다.

 

 
