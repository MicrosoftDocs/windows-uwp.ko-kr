---
ms.assetid: 141900dd-f1d3-4432-ac8b-b98eaa0b0da2
description: XAML 앱의 Microsoft 광고 라이브러리와 관련 된 일반적인 개발 문제에 대 한 솔루션에 대해 알아봅니다.
title: XAML과 C# 문제 해결 가이드
ms.date: 02/18/2020
ms.topic: article
keywords: 'windows 10, uwp, 광고, 광고, AdControl, 문제 해결, XAML, c #'
ms.localizationpriority: medium
ms.openlocfilehash: 719e05d67d68627fcd631edfd6c688b17f8507bd
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164387"
---
# <a name="xaml-and-c-troubleshooting-guide"></a>XAML과 C# 문제 해결 가이드

>[!WARNING]
> 2020 년 6 월 1 일부 터 Windows UWP 앱 용 Microsoft Ad 수익 화 플랫폼이 종료 됩니다. [자세한 정보](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

이 항목에는 XAML 앱의 Microsoft 광고 라이브러리와 관련 된 일반적인 개발 문제에 대 한 해결 방법이 포함 되어 있습니다.

* [XAML](#xaml)
  * [AdControl이 나타나지 않음](#xaml-notappearing)
  * [검은색 상자가 깜박이 고 사라집니다.](#xaml-blackboxblinksdisappears)
  * [광고 새로 고침 안 함](#xaml-adsnotrefreshing)

* [C#](#csharp)
  * [AdControl이 나타나지 않음](#csharp-adcontrolnotappearing)
  * [검은색 상자가 깜박이 고 사라집니다.](#csharp-blackboxblinksdisappears)
  * [광고 새로 고침 안 함](#csharp-adsnotrefreshing)

<span id="xaml"/>

## <a name="xaml"></a>XAML

<span id="xaml-notappearing"/>

### <a name="adcontrol-not-appearing"></a>AdControl이 나타나지 않음

1.  Appxmanifest.xml에서 **인터넷 (클라이언트)** 기능이 선택 되어 있는지 확인 합니다.

2.  응용 프로그램 ID 및 ad 단위 ID를 확인 합니다. 이러한 Id는 파트너 센터에서 가져온 응용 프로그램 ID 및 ad 단위 ID와 일치 해야 합니다. 자세한 내용은 [앱에서 ad 단위 설정](set-up-ad-units-in-your-app.md#live-ad-units)을 참조 하세요.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}" ApplicationId="{ApplicationID}"
                  Width="728" Height="90" />
    ```

3.  **Height** 및 **Width** 속성을 확인 하십시오. [배너 광고에 대해 지원 되는 ad 크기](supported-ad-sizes-for-banner-ads.md)중 하나로 설정 해야 합니다.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90" />
    ```

4.  요소 위치를 확인 합니다. [Adcontrol](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 은 볼 수 있는 영역 내에 있어야 합니다.

5.  **표시 유형** 속성을 선택 합니다. 선택적 **표시 유형** 속성은 축소 또는 숨기기로 설정 해서는 안 됩니다. 이 속성은 아래와 같이 인라인으로 설정 하거나 외부 스타일 시트에서 설정할 수 있습니다.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Visibility="Visible"
                  Width="728" Height="90" />
    ```

6.  **Adcontrol**의 부모를 확인 합니다. **Adcontrol** 요소가 부모 요소에 있는 경우 부모는 활성 상태이 고 표시 되어야 합니다.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <StackPanel>
        <UI:AdControl AdUnitId="{AdUnitID}"
                      ApplicationId="{ApplicationID}"
                      Width="728" Height="90" />
    </StackPanel>
    ```

7.  뷰포트에 **Adcontrol** 이 숨겨지지 않았는지 확인 합니다. 광고가 제대로 표시 되려면 **Adcontrol** 이 표시 되어야 합니다.

8.  **ApplicationId** 및 **ad단위 id** 의 라이브 값은 에뮬레이터에서 테스트 하면 안 됩니다. **Adcontrol** 이 예상 대로 작동 하는지 확인 하려면 **ApplicationId** 및 **adcontrol id**모두에 대해 [테스트 값](set-up-ad-units-in-your-app.md#test-ad-units) 을 사용 합니다.

<span id="xaml-blackboxblinksdisappears"/>

### <a name="black-box-blinks-and-disappears"></a>검은색 상자가 깜박이 고 사라집니다.

1.  이전 [Adcontrol](#xaml-notappearing) 이 나타나지 않음 섹션의 모든 단계를 두 번 확인 합니다.

2.  **Erroroccurred** 이벤트를 처리 하 고 이벤트 처리기에 전달 되는 메시지를 사용 하 여 오류가 발생 했는지 여부와 throw 된 오류의 유형을 확인 합니다. 자세한 내용은 [XAML/c #의 오류 처리 연습](error-handling-in-xamlc-walkthrough.md) 을 참조 하세요.

    이 예제에서는 **erroroccurred** 한 이벤트 처리기를 보여 줍니다. 첫 번째 코드 조각은 XAML UI 태그입니다.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  ErrorOccurred="adControl_ErrorOccurred" />
    <TextBlock x:Name="TextBlock1" TextWrapping="Wrap" Width="500" Height="250" />
    ```

    이 예제에서는 해당 c # 코드를 보여 줍니다.

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    private void adControl_ErrorOccurred(object sender,               
        Microsoft.Advertising.WinRT.UI.AdErrorEventArgs e)
    {
        TextBlock1.Text = e.ErrorMessage;
    }
    ```

    블랙 박스를 발생 시키는 가장 일반적인 오류는 "ad를 사용할 수 없습니다."입니다. 이 오류는 요청에서 반환할 수 있는 광고가 없음을 의미 합니다.

3.  [Adcontrol](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 은 정상적으로 동작 합니다.

    기본적으로 **Adcontrol** 은 광고를 표시할 수 없을 때 축소 됩니다. 다른 요소가 동일한 부모의 자식인 경우 축소 된 **Adcontrol** 의 차이를 채우도록 이동 하 고 다음 요청이 수행 될 때 확장할 수 있습니다.

<span id="xaml-adsnotrefreshing"/>

### <a name="ads-not-refreshing"></a>광고 새로 고침 안 함

1.  [IsAutoRefreshEnabled](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.isautorefreshenabled) 속성을 확인 합니다. 기본적으로이 선택적 속성은 **True**로 설정 됩니다. **False**로 설정 하면 [Refresh](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.refresh) 메서드를 사용 하 여 다른 광고를 검색 해야 합니다.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  IsAutoRefreshEnabled="True" />
    ```

2.  **Refresh** 메서드 호출을 확인 합니다. 자동 새로 고침을 사용 하는 경우 **새로 고침** 을 사용 하 여 다른 광고를 검색할 수 없습니다. 수동 새로 고침을 사용 하는 경우 **새로 고침** 은 장치의 현재 데이터 연결에 따라 최소 30 ~ 60 초 후에만 호출 해야 합니다.

    다음 코드 조각에서는 **Refresh** 메서드를 사용 하는 방법의 예를 보여 줍니다. 첫 번째 코드 조각은 XAML UI 태그입니다.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl x:Name="adControl1"
                  AdUnitId="{AdUnit_ID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  IsAutoRefreshEnabled="False" />
    ```

    이 코드 조각은 UI 태그 뒤에 있는 c # 코드의 예를 보여 줍니다.

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    public RefreshAds()
    {
        var timer = new DispatcherTimer() { Interval = TimeSpan.FromSeconds(60) };
        timer.Tick += (s, e) => adControl1.Refresh();
        timer.Start();
    }
    ```

3.  **Adcontrol** 은 정상적으로 동작 합니다. 경우에 따라 광고가 새로 고쳐지지 않는 모양을 제공 하는 동일한 광고가 한 행에 두 번 이상 표시 됩니다.

<span id="csharp"/>

## <a name="c"></a>C\# #

<span id="csharp-adcontrolnotappearing"/>

### <a name="adcontrol-not-appearing"></a>AdControl이 나타나지 않음

1.  Appxmanifest.xml에서 **인터넷 (클라이언트)** 기능이 선택 되어 있는지 확인 합니다.

2.  **Adcontrol** 이 인스턴스화되어 있는지 확인 합니다. **Adcontrol** 이 인스턴스화되지 않은 경우에는 사용할 수 없습니다.

    > [!div class="tabbedCodeSnippets"]
    [!code-csharp[AdControl](./code/AdvertisingSamples/AdControlSamples/cs/MiscellaneousSnippets.cs#Snippet1)]

3.  응용 프로그램 ID 및 ad 단위 ID를 확인 합니다. 이러한 Id는 파트너 센터에서 가져온 응용 프로그램 ID 및 ad 단위 ID와 일치 해야 합니다. 자세한 내용은 [앱에서 ad 단위 설정](set-up-ad-units-in-your-app.md#live-ad-units)을 참조 하세요.

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    ```

4.  **Height** 및 **Width** 매개 변수를 확인 합니다. [배너 광고에 대해 지원 되는 ad 크기](supported-ad-sizes-for-banner-ads.md)중 하나로 설정 해야 합니다.

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;adControl.Width = 728;
    ```

5.  **Adcontrol** 이 부모 요소에 추가 되었는지 확인 합니다. 을 표시 하려면 **Adcontrol** 을 부모 컨트롤 (예: **StackPanel** 또는 **Grid**)에 자식으로 추가 해야 합니다.

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    ContentPanel.Children.Add(adControl);
    ```

6.  **Margin** 매개 변수를 확인 합니다. **Adcontrol** 은 볼 수 있는 영역 내에 있어야 합니다.

7.  **표시 유형** 속성을 선택 합니다. 선택적 **표시 유형** 속성은 **표시**로 설정 되어야 합니다.

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    adControl.Visibility = System.Windows.Visibility.Visible;
    ```

8.  **Adcontrol**의 부모를 확인 합니다. 부모는 활성 상태이 고 표시 되어야 합니다.

9. **ApplicationId** 및 **ad단위 id** 의 라이브 값은 에뮬레이터에서 테스트 하면 안 됩니다. **Adcontrol** 이 예상 대로 작동 하는지 확인 하려면 **ApplicationId** 및 **adcontrol id**모두에 대해 [테스트 값](set-up-ad-units-in-your-app.md#test-ad-units) 을 사용 합니다.

<span id="csharp-blackboxblinksdisappears"/>

### <a name="black-box-blinks-and-disappears"></a>검은색 상자가 깜박이 고 사라집니다.

1.  위의 [Adcontrol](#csharp-adcontrolnotappearing) 이 나타나지 않음 섹션에서 모든 단계를 두 번 확인 합니다.

2.  **Erroroccurred** 이벤트를 처리 하 고 이벤트 처리기에 전달 되는 메시지를 사용 하 여 오류가 발생 했는지 여부와 throw 된 오류의 유형을 확인 합니다. 자세한 내용은 [XAML/c #의 오류 처리 연습](error-handling-in-xamlc-walkthrough.md) 을 참조 하세요.

    다음 예제에서는 오류 호출을 구현 하는 데 필요한 기본 코드를 보여 줍니다. 이 XAML 코드는 오류 메시지를 표시하는 데 사용되는 **TextBlock**을 정의합니다.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <TextBlock x:Name="TextBlock1" TextWrapping="Wrap" Width="500" Height="250" />
    ```

    이 c # 코드는 오류 메시지를 검색 하 여 **TextBlock**에 표시 합니다.

    > [!div class="tabbedCodeSnippets"]
    [!code-csharp[AdControl](./code/AdvertisingSamples/AdControlSamples/cs/MiscellaneousSnippets.cs#Snippet2)]

    블랙 박스를 발생 시키는 가장 일반적인 오류는 "ad를 사용할 수 없습니다."입니다. 이 오류는 요청에서 반환할 수 있는 광고가 없음을 의미 합니다.

3.  **Adcontrol** 은 정상적으로 동작 합니다. 경우에 따라 광고가 새로 고쳐지지 않는 모양을 제공 하는 동일한 광고가 한 행에 두 번 이상 표시 됩니다.

<span id="csharp-adsnotrefreshing"/>

### <a name="ads-not-refreshing"></a>광고 새로 고침 안 함

1.  **Adcontrol** 의 [IsAutoRefreshEnabled](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.isautorefreshenabled.aspx) 속성이 false로 설정 되어 있는지 여부를 확인 합니다. 기본적으로이 선택적 속성은 **true**로 설정 됩니다. **False**로 설정 하면 **Refresh** 메서드를 사용 하 여 다른 광고를 검색 해야 합니다.

2.  [Refresh](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.refresh.aspx) 메서드 호출을 확인 합니다. 자동 새로 고침을 사용 하는 경우 (**IsAutoRefreshEnabled** 가 **true**인 경우) **새로 고침** 을 사용 하 여 다른 광고를 검색할 수 없습니다. 수동 새로 고침을 사용 하는 경우 (**IsAutoRefreshEnabled** 가 **false**인 경우), 장치의 현재 데이터 연결에 따라 최소 30 ~ 60 초 후에만 **새로 고침이** 호출 되어야 합니다.

    다음 예제에서는 **Refresh** 메서드를 호출 하는 방법을 보여 줍니다.

    > [!div class="tabbedCodeSnippets"]
    [!code-csharp[AdControl](./code/AdvertisingSamples/AdControlSamples/cs/MiscellaneousSnippets.cs#Snippet3)]

3.  **Adcontrol** 은 정상적으로 동작 합니다. 경우에 따라 광고가 새로 고쳐지지 않는 모양을 제공 하는 동일한 광고가 한 행에 두 번 이상 표시 됩니다.

 

 