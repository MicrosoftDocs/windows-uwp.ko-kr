---
author: mcleanbyron
ms.assetid: 4e7c2388-b94e-4828-a104-14fa33f6eb2d
description: Windows 10(UWP), Windows 8.1 또는 Windows Phone 8.1용 XAML 앱에서 AdControl 클래스를 사용하여 배너 광고를 표시하는 방법을 알아봅니다.
title: XAML 및 .NET의 AdControl
---

# XAML 및 .NET의 AdControl


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

이 연습에서는 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 클래스를 사용하여 Windows 10(UWP), Windows 8.1 또는 Windows Phone 8.1용 XAML 앱에서 배너 광고를 표시하는 방법을 보여 줍니다. 이 연습에서는 **AdMediatorControl** 또는 광고 조정을 사용하지 않습니다.

C# 및 C++를 사용하여 XAML 앱에 배너 광고를 추가하는 방법을 보여 주는 전체 샘플 프로젝트에 대해서는 [GitHub의 광고 샘플](http://aka.ms/githubads)을 참조하세요.

## 필수 조건

* Visual Studio 2015 또는 Visual Studio 2013과 함께 [Microsoft 스토어 참여 및 수익 창출 SDK](http://aka.ms/store-em-sdk)를 설치합니다.

## 코드 개발

1. Visual Studio에서 프로젝트를 열거나 새 프로젝트를 만듭니다.

2. 프로젝트의 대상이 **모든 CPU**인 경우 아키텍처별 빌드 출력(예: **x86**)을 사용하도록 프로젝트를 업데이트합니다. 프로젝트의 대상이 **모든 CPU**인 경우 다음 단계에 따라 Microsoft Advertising 라이브러리에 대한 참조를 성공적으로 추가하지 못할 수 있습니다. 자세한 내용은 [프로젝트에서 모든 CPU를 대상으로 할 경우 발생하는 참조 오류](known-issues-for-the-advertising-libraries.md#reference_errors)를 참조하세요.

1.  **솔루션 탐색기** 창에서 **참조**를 마우스 오른쪽 단추로 클릭한 다음 **참조 추가...**를 선택합니다.

2.  **참조 관리자**에서 프로젝트 유형에 따라 다음 참조 중 하나를 선택합니다.

    -   UWP(유니버설 Windows 플랫폼) 프로젝트: **유니버설 Windows**를 확장하고 **확장**을 클릭한 후 **Microsoft Advertising SDK for XAML**(버전 10.0) 옆의 확인란을 선택합니다.

    -   Windows 8.1 프로젝트: **Windows 8.1**을 확장하고 **확장**을 클릭한 후 **Ad Mediator SDK for Windows 8.1 XAML** 옆의 확인란을 선택합니다. 이 옵션은 Microsoft Advertising 및 광고 중재자 라이브러리를 프로젝트에 추가하지만 광고 중재자 라이브러리는 무시해도 됩니다.

    -   Windows Phone 8.1 프로젝트: **Windows Phone 8.1**을 확장하고 **확장**을 클릭한 후 **Ad Mediator SDK for Windows Phone 8.1 XAML** 옆의 확인란을 선택합니다. 이 옵션은 Microsoft Advertising 및 광고 중재자 라이브러리를 프로젝트에 추가하지만 광고 중재자 라이브러리는 무시해도 됩니다.

  ![addreferences](images/13-a84c026e-b283-44f2-8816-f950a1ef89aa.png)

    > **참고** 이 이미지는 Visual Studio 2015에서 Windows 10용 UWP 프로젝트를 빌드하는 방법을 보여 줍니다. Windows 8.1 또는 Windows Phone 8.1 앱을 빌드하거나 Visual Studio 2013을 사용하는 경우 화면이 다르게 나타납니다.

3.  **참조 관리자**에서 확인을 클릭합니다.
4.  광고를 포함하는 페이지의 XAML을 **Microsoft.Advertising.WinRT.UI** 네임스페이스를 포함하도록 수정합니다. 예를 들어 Visual Studio에서 생성된 기본 샘플 앱(이 앱에서는 MyAdFundedWindows10AppXAML)에서 XAML 페이지는 **MainPage.XAML**입니다.

    Visual Studio에서 생성된 MainPage.xaml 파일의 **Page** 섹션에는 다음 코드가 있습니다.

    ``` syntax
    <Page
        x:Class="MyAdFundedWindows10AppXAML.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:MyAdFundedWindows10AppXAML"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

        </Grid>
    </Page>
    ```

    MainPage.xaml 파일의 **Page** 섹션에 다음 코드가 포함되도록 **Microsoft.Advertising.WinRT.UI**에 네임스페이스 참조를 추가합니다.

    ``` syntax
    <Page
        x:Class="MyAdFundedWindows10AppXAML.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:MyAdFundedWindows10AppXAML"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:UI="using:Microsoft.Advertising.WinRT.UI"
        mc:Ignorable="d">

        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

        </Grid>
    </Page>
    ```

5.  **Grid** 태그에 **AdControl**에 대한 코드를 추가합니다.

    1.  **Page**의 [ApplicationId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.applicationid.aspx) 및 [AdUnitId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.adunitid.aspx) 속성을 [테스트 모드 값](test-mode-values.md)에 제공된 테스트 값에 할당합니다.

        > **참고** 앱을 제출하기 전에 테스트 값을 라이브 값으로 바꿉니다.

    2.  [배너 광고에 지원되는 광고 크기](supported-ad-sizes-for-banner-ads.md) 중 하나가 되도록 컨트롤의 높이와 너비를 조정합니다.

    전체 **Grid** 태그는 다음 코드와 같습니다.

    ``` syntax
    <Grid Background="{StaticResource ApplicationPageBackgroundThemeBrush}">

            <UI:AdControl ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
                          AdUnitId="10865270"
                          HorizontalAlignment="Left"
                          Height="250"
                          VerticalAlignment="Top"
                          Width="300"/>
    </Grid>
    ```

    MainPage.xaml 파일에 대한 전체 코드는 다음과 같습니다.

    ``` syntax
    <Page
        x:Class="MyAdFundedWindows10AppXAML.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:MyAdFundedWindows10AppXAML"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:UI="using:Microsoft.Advertising.WinRT.UI"
        mc:Ignorable="d">

        <Grid Background="{StaticResource ApplicationPageBackgroundThemeBrush}">

            <UI:AdControl ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
                          AdUnitId="10865270"
                          HorizontalAlignment="Left"
                          Height="250"
                          VerticalAlignment="Top"
                          Width="300"/>
        </Grid>
    </Page>
    ```

6.  앱을 컴파일하고 실행하여 광고와 함께 표시합니다.

## Windows 개발자 센터를 사용하여 라이브 광고와 함께 앱 출시


1.  개발자 센터 대시보드에서 앱의 **수익 창출**&gt;**광고로 수익 창출** 페이지로 이동한 후 [독립 실행형 Microsoft 광고 단위를 만듭니다](../publish/monetize-with-ads.md). 광고 단위 유형으로 **배너**를 지정합니다. 광고 단위 ID와 응용 프로그램 ID를 적어둡니다.

2.  코드에서 테스트 광고 단위 값(**ApplicationId** 및 **AdUnitId**)을 개발자 센터에서 생성한 라이브 값으로 바꿉니다.

3.  개발자 센터 대시보드를 사용하여 스토어에 [앱을 제출](../publish/app-submissions.md)합니다.

4.  개발자 센터 대시보드에서 [광고 성과 보고서](../publish/advertising-performance-report.md)를 검토합니다.

## 참고

C#: **AdControl** 이벤트에 이벤트 처리기를 할당하는 방법의 예제를 보려면 [XAML 속성 예제](xaml-properties-example.md)를 참조하세요. 그런 다음 [C#의 AdControl 이벤트](adcontrol-events-in-c.md)에서 C#으로 작성된 이벤트 처리기를 보여 주는 샘플 코드를 참조하세요.

Visual Basic: **AdControl** 이벤트에 이벤트 처리기를 할당하는 방법의 예제를 보려면 [XAML 속성 예제](xaml-properties-example.md)를 참조하세요.

C++: Microsoft Advertising 라이브러리의 현재 릴리스에서는 C++를 지원합니다. **AdControl**은 CLR을 로드하고 관리되는 C++를 사용합니다.

오류 처리: 오류를 처리 하는 방법에 대해 알아보려면 [AdControl 오류 처리](adcontrol-error-handling.md)를 참조하세요.

## 관련 항목

* [GitHub의 광고 샘플](http://aka.ms/githubads)

 


<!--HONumber=May16_HO2-->


