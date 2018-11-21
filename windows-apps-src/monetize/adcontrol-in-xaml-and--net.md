---
author: Xansky
ms.assetid: 4e7c2388-b94e-4828-a104-14fa33f6eb2d
description: Windows10(UWP)용 XAML 앱에서 AdControl 클래스를 사용하여 배너 광고를 표시하는 방법을 알아봅니다.
title: XAML 및 .NET의 AdControl
ms.author: mhopkins
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp, 광고, 광고, AdControl, 광고 관리, XAML, .net, 연습
ms.localizationpriority: medium
ms.openlocfilehash: b2346fceaae3996de8aa54640c206b5fcd8c4a87
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7433008"
---
# <a name="adcontrol-in-xaml-and-net"></a>XAML 및 .NET의 AdControl


이 연습에서는 [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 클래스를 사용하여 C#를 사용해 구현되는 Windows 10용 UWP(유니버설 Windows 플랫폼) XAML 앱에서 배너 광고를 표시하는 방법을 보여줍니다.

> [!NOTE]
> Microsoft Advertising SDK는 C ++를 사용하여 구현되는 XAML 앱도 지원합니다. 전체 샘플 프로젝트는 [GitHub의 광고 샘플](http://aka.ms/githubads)을 참조하세요.

## <a name="prerequisites"></a>필수 구성 요소

* Visual Studio 2015 이상 릴리스와 함께 [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp)를 설치합니다. 설치 지침은 [이 문서](install-the-microsoft-advertising-libraries.md)를 참조하세요.

## <a name="integrate-a-banner-ad-into-your-app"></a>앱에 배너 광고를 통합

1. Visual Studio에서 프로젝트를 열거나 새 프로젝트를 만듭니다.

    > [!NOTE]
    > 기존 프로젝트를 사용하고 있는 경우 프로젝트에서 Package.appxmanifest 파일을 열고 **인터넷(클라이언트)** 기능이 선택되었는지 확인합니다. 앱에서 테스트 광고와 라이브 광고를 수신하려면 이 기능이 필요합니다.

2. 프로젝트의 대상이 **모든 CPU**인 경우 아키텍처별 빌드 출력(예: **x86**)을 사용하도록 프로젝트를 업데이트합니다. 프로젝트의 대상이 **모든 CPU**인 경우 다음 단계에 따라 Microsoft Advertising 라이브러리에 대한 참조를 성공적으로 추가하지 못할 수 있습니다. 자세한 내용은 [프로젝트에서 모든 CPU를 대상으로 할 경우 발생하는 참조 오류](known-issues-for-the-advertising-libraries.md#reference_errors)를 참조하세요.

3. 프로젝트에 Microsoft Advertising SDK 참조를 추가합니다.

    1. **솔루션 탐색기** 창에서 **참조**를 마우스 오른쪽 단추로 클릭한 다음 **참조 추가**를 선택합니다.
    2.  **참조 관리자**에서 **유니버설 Windows**를 확장하고 **확장**을 클릭한 후 **Microsoft Advertising SDK for XAML**(버전 10.0) 옆의 확인란을 선택합니다.
    3.  **참조 관리자**에서 확인을 클릭합니다.

4.  광고를 포함하는 페이지의 XAML을 **Microsoft.Advertising.WinRT.UI** 네임스페이스를 포함하도록 수정합니다. 예를 들어 Visual Studio에서 생성된 기본 샘플 앱(이 앱에서는 MyAdFundedWindows10AppXAML)에서 XAML 페이지는 **MainPage.XAML**입니다.

    Visual Studio에서 생성된 MainPage.xaml 파일의 **Page** 섹션에는 다음 코드가 있습니다.

    ``` xml
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

    ``` xml
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

5. **Grid** 태그에 **AdControl**에 대한 코드를 추가합니다. [테스트 광고 단위 값](set-up-ad-units-in-your-app.md#test-ad-units)에 [AdUnitId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.adunitid) 및 [ApplicationId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) 속성을 할당합니다. 또한 [배너 광고에 지원되는 광고 크기](supported-ad-sizes-for-banner-ads.md) 중 하나가 되도록 컨트롤의 **높이** 와 **너비**를 조정합니다.

    > [!NOTE]
    > 모든 **AdControl**에는 컨트롤할 광고를 지원하는 서비스가 사용하는 *광고 단위*가 있고, 모든 광고 단위는 *광고 단위 ID*와 *응용 프로그램 ID*로 구성되어 있습니다. 이 단계에서 컨트롤에 테스트 광고 단위 ID와 응용 프로그램 ID 값을 할당하세요. 이 테스트 값은 앱 테스트 버전에서만 사용할 수 있습니다. 해야 스토어에 앱을 게시 하기 전에 [대체 이러한 테스트 값을 라이브 값](#release) 파트너 센터에서.

    전체 **Grid** 태그는 다음 코드와 같습니다.

    ``` xml
    <Grid Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
        <UI:AdControl ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
            AdUnitId="test"
            HorizontalAlignment="Left"
            Height="250"
            VerticalAlignment="Top"
            Width="300"/>
    </Grid>
    ```

    MainPage.xaml 파일에 대한 전체 코드는 다음과 같습니다.

    ``` xml
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
                  AdUnitId="test"
                  HorizontalAlignment="Left"
                  Height="250"
                  VerticalAlignment="Top"
                  Width="300"/>
      </Grid>
    </Page>
    ```

6.  앱을 컴파일하고 실행하여 광고와 함께 표시합니다.

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>라이브 광고와 앱 릴리스

1. 앱에 사용할 배너 광고는 [배너 광고에 대한 지침](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads)을 따라야 합니다.

2.  파트너 센터에서 이동 [인 앱 광고](../publish/in-app-ads.md) 페이지와 [광고 단위를 생성](set-up-ad-units-in-your-app.md#live-ad-units)합니다. 광고 단위 유형으로 **배너**를 지정합니다. 광고 단위 ID와 응용 프로그램 ID를 적어둡니다.
    > [!NOTE]
    > 테스트 광고 단위와 라이브 UWP 광고 단위의 응용 프로그램 ID 값은 형식이 서로 다릅니다. 테스트 응용 프로그램 ID 값은 GUID입니다. 파트너 센터에서 라이브 UWP 광고 단위를 만들 때 광고 단위에 대 한 응용 프로그램 ID 값에 항상 (예: Store ID 값은 9NBLGGH4R315와) 앱에 대 한 스토어 ID와 일치 합니다.

3. [인앱 광고](../publish/in-app-ads.md) 페이지의 [조정 설정](../publish/in-app-ads.md#mediation) 섹션에서 설정을 구성하여, **AdControl**의 광고 조정을 사용하는 방법도 있습니다. 광고 조정을 통해 Taboola 및 Smaato 같은 기타 유료 광고 네트워크와 Microsoft 앱 프로모션 캠페인에 대한 광고를 포함하여 여러 광고 네트워크의 광고를 표시하여 광고 수익과 앱 프로모션 기능을 최대화할 수 있습니다.

4.  코드에서 테스트 광고 단위 값을 (**ApplicationId** 및 **AdUnitId**) 파트너 센터에서 생성 한 라이브 값으로 바꿉니다.

5.  파트너 센터를 사용 하 여 스토어에 [앱 제출](../publish/app-submissions.md)

6.  파트너 센터에서 [광고 성과 보고서](../publish/advertising-performance-report.md) 를 검토 합니다.

<span id="manage" />

## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>앱에서 여러 광고 컨트롤에 대한 광고 단위 관리

단일 앱에서 여러 **AdControl** 개체를 사용할 수 있습니다(예를 들어, 앱의 각 페이지를 여러 **AdControl** 개체에 호스트할 수 있습니다). 이 경우, 각 컨트롤에 다른 광고 단위를 지정하는 것이 좋습니다. 각 컨트롤에 다른 광고 단위를 사용하면 별도로 [조정 설정을 구성](../publish/in-app-ads.md#mediation)할 수 있고, 각 컨트롤에 대해 별개의 [보고 데이터](../publish/advertising-performance-report.md)를 가져올 수 있습니다. 또 이렇게 하면 서비스가 지원하는 앱에서 더 효과적으로 광고를 최적화할 수 있습니다.

> [!IMPORTANT]
> 앱별로 하나의 광고 단위만 사용할 수 있습니다. 특정 광고 단위를 하나 이상의 앱에 사용하면, 해당 광고 단위에 광고가 제공되지 않습니다.

## <a name="related-topics"></a>관련 항목

* [배너 광고에 대한 지침](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads)
* [XAML/C#에서 오류 처리 연습](error-handling-in-xamlc-walkthrough.md).
* [GitHub의 광고 샘플](http://aka.ms/githubads)
* [앱에서 광고 단위 설정](set-up-ad-units-in-your-app.md)
