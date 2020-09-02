---
description: 광고의 각 조각이 개별 요소로 앱에 배달 되는 구성 요소 기반 ad 형식인 네이티브 광고를 사용 하는 방법에 대해 알아봅니다.
title: 기본 광고
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, 광고, 광고, ad 컨트롤, 기본 ad
ms.localizationpriority: medium
ms.openlocfilehash: 417560c9099937324b39a8cdfafb7d62ec7e64e6
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364086"
---
# <a name="native-ads"></a>기본 광고

>[!WARNING]
> 2020 년 6 월 1 일부 터 Windows UWP 앱 용 Microsoft Ad 수익 화 플랫폼이 종료 됩니다. [자세한 정보](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

기본 광고는 제목, 이미지, 설명, 행동 촉구 텍스트 등의 각 광고 크리에이티브 조각이 앱과 통합할 수 있는 개별 요소로 앱에 제공되는 구성 요소 기반 광고입니다. 사용자 고유의 글꼴, 색, 애니메이션 및 기타 UI 구성 요소를 사용 하 여 이러한 요소를 앱에 통합 하 여 응용 프로그램의 모양과 느낌에 맞고 광고에서 높은 수익률을 얻을 수 있는 사용자 환경을 원활 하 게 연결할 수 있습니다.

광고주의 경우, 광고 환경이 앱에 긴밀 하 게 통합 되어 사용자가 이러한 유형의 광고와 상호 작용 하는 경향이 있기 때문에 기본 광고는 고성능 배치를 제공 합니다.

> [!NOTE]
> 네이티브 광고는 현재 Windows 10 용 XAML 기반 UWP 앱에 대해서만 지원 됩니다. HTML 및 JavaScript를 사용 하 여 작성 된 UWP 앱에 대 한 지원은 향후 Microsoft Advertising SDK 릴리스를 위해 예정 되어 있습니다.

## <a name="prerequisites"></a>사전 요구 사항

* Visual studio 2015 이상 버전의 Visual Studio를 사용 하 여 [MICROSOFT ADVERTISING SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) 를 설치 합니다. 설치 지침은 [이 문서](install-the-microsoft-advertising-libraries.md)를 참조 하세요.

## <a name="integrate-a-native-ad-into-your-app"></a>네이티브 ad를 앱에 통합

다음 지침에 따라 네이티브 ad를 앱에 통합 하 고 네이티브 ad 구현에서 테스트 광고를 표시 하는지 확인 합니다.

1. Visual Studio에서 프로젝트를 열거나 새 프로젝트를 만듭니다.
    > [!NOTE]
    > 기존 프로젝트를 사용 하는 경우 프로젝트에서 appxmanifest.xml 파일을 열고 **인터넷 (클라이언트)** 기능이 선택 되어 있는지 확인 합니다. 앱에서 테스트 광고와 라이브 광고를 수신 하려면이 기능이 필요 합니다.

2. 프로젝트가 **모든 CPU**를 대상으로 하는 경우 프로젝트를 업데이트 하 여 아키텍처 관련 빌드 출력 (예: **x86**)을 사용 합니다. 프로젝트가 **모든 CPU**를 대상으로 하는 경우에는 다음 단계에서 Microsoft Advertising SDK에 대 한 참조를 성공적으로 추가할 수 없습니다. 자세한 내용은 [프로젝트의 모든 CPU를 대상으로 하 여 발생 한 참조 오류](known-issues-for-the-advertising-libraries.md#reference_errors)를 참조 하세요.

3. 프로젝트에서 Microsoft Advertising SDK에 대 한 참조를 추가 합니다.

    1. **솔루션 탐색기** 창에서 **참조**를 마우스 오른쪽 단추로 클릭 하 고 **참조 추가** ...를 선택 합니다.
    2.  **참조 관리자**에서 **유니버설 Windows**를 확장 하 고 **확장**을 클릭 한 후 **Microsoft Advertising SDK for XAML** (버전 10.0) 옆의 확인란을 선택 합니다.
    3.  **참조 관리자**에서 확인을 클릭 합니다.

4. 앱의 적절 한 코드 파일 (예: MainPage.xaml.cs 또는 다른 페이지의 코드 파일)에서 다음 네임 스페이스 참조를 추가 합니다.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs" id="Namespaces":::

5.  응용 프로그램의 적절 한 위치 (예: ```MainPage``` 또는 다른 페이지)에서 [NativeAdsManagerV2](/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2) 개체와 기본 광고의 응용 프로그램 id 및 ad 단위 id를 나타내는 여러 개의 문자열 필드를 선언 합니다. 다음 코드 예제에서는 `myAppId` 및 필드를 `myAdUnitId` 네이티브 광고의 [테스트 값](set-up-ad-units-in-your-app.md#test-ad-units) 에 할당 합니다.
    > [!NOTE]
    > 모든 **NativeAdsManagerV2** 에는 서비스에서 네이티브 ad 컨트롤에 대 한 광고를 제공 하는 데 사용 하는 해당 하는 *ad 단위가* 있으며 모든 ad 단위는 *ad 단위 id* 와 *응용 프로그램 id*로 구성 됩니다. 이러한 단계에서는 테스트 ad 단위 ID 및 응용 프로그램 ID 값을 컨트롤에 할당 합니다. 이러한 테스트 값은 응용 프로그램의 테스트 버전 에서만 사용할 수 있습니다. 스토어에 앱을 게시 하기 전에 이러한 테스트 값을 파트너 센터의 [라이브 값으로 바꾸어야](#release) 합니다.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs" id="Variables":::

6.  시작 시 실행 되는 코드 (예: 페이지에 대 한 생성자)에서 **NativeAdsManagerV2** 개체를 인스턴스화하고 개체의 **Adready** 및 **erroroccurred** 이벤트에 대 한 이벤트 처리기를 연결 합니다.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs" id="ConfigureNativeAd":::

7.  네이티브 광고를 표시할 준비가 되 면 **requestad** 메서드를 호출 하 여 광고를 인출 합니다.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs" id="RequestAd":::

8.  네이티브 ad가 앱에 대해 준비 되 면 [Adready](/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2.adready) 이벤트 처리기가 호출 되 고, 기본 광고를 나타내는 [NativeAdV2](/uwp/api/microsoft.advertising.winrt.ui.nativeadv2) 개체가 *e* 매개 변수로 전달 됩니다. **NativeAdV2** 속성을 사용 하 여 네이티브 광고의 각 요소를 가져오고 이러한 요소를 페이지에 표시 합니다. 또한 **Registeradcontainer** 메서드를 호출 하 여 네이티브 광고의 컨테이너 역할을 하는 UI 요소를 등록 해야 합니다. 광고 노출 및 클릭을 적절히 추적 하는 데 필요 합니다.
    > [!NOTE]
    > 기본 광고의 일부 요소는 필수 이며 항상 앱에 표시 되어야 합니다. 자세한 내용은 [기본 광고에 대 한 지침](ui-and-user-experience-guidelines.md#guidelines-for-native-ads)을 참조 하세요.

    예를 들어 앱에 다음 StackPanel이 포함 된 ```MainPage``` (또는 다른 페이지)가 포함 되어 **StackPanel**있다고 가정 합니다. 이 **StackPanel** 에는 제목, 설명, 이미지, 텍스트로 *후원* 및 *작업 텍스트에 대 한 호출* 을 표시 하는 단추를 포함 하 여 네이티브 광고의 여러 요소를 표시 하는 일련의 컨트롤이 포함 됩니다.

    ``` xml
    <StackPanel x:Name="NativeAdContainer" Background="#555555" Width="Auto" Height="Auto"
                Orientation="Vertical">
        <Image x:Name="AdIconImage" HorizontalAlignment="Left" VerticalAlignment="Center"
               Margin="20,20,20,20"/>
        <TextBlock x:Name="TitleTextBlock" HorizontalAlignment="Left" VerticalAlignment="Center"
               Text="The ad title will go here" FontSize="24" Foreground="White" Margin="20,0,0,10"/>
        <TextBlock x:Name="DescriptionTextBlock" HorizontalAlignment="Left" VerticalAlignment="Center"
                   Foreground="White" TextWrapping="Wrap" Text="The ad description will go here"
                   Margin="20,0,0,0" Visibility="Collapsed"/>
        <Image x:Name="MainImageImage" HorizontalAlignment="Left"
               VerticalAlignment="Center" Margin="20,20,20,20" Visibility="Collapsed"/>
        <Button x:Name="CallToActionButton" Background="Gray" Foreground="White"
                HorizontalAlignment="Left" VerticalAlignment="Center" Width="Auto" Height="Auto"
                Content="The call to action text will go here" Margin="20,20,20,20"
                Visibility="Collapsed"/>
        <StackPanel x:Name="SponsoredByStackPanel" Orientation="Horizontal" Margin="20,20,20,20">
            <TextBlock x:Name="SponsoredByTextBlock" Text="The ad sponsored by text will go here"
                       FontSize="24" Foreground="White" Margin="20,0,0,0" HorizontalAlignment="Left"
                       VerticalAlignment="Center" Visibility="Collapsed"/>
            <Image x:Name="IconImageImage" Margin="40,20,20,20" HorizontalAlignment="Left"
                   VerticalAlignment="Center" Visibility="Collapsed"/>
        </StackPanel>
    </StackPanel>
    ```

    다음 코드 예제에서는 **stackpanel** 의 컨트롤에 있는 네이티브 광고의 각 요소를 표시 한 다음 **registeradcontainer** 메서드를 호출 하 여 **Stackpanel**을 등록 하는 **adready** 이벤트 처리기를 보여 줍니다. 이 코드에서는 **StackPanel**을 포함 하는 페이지에 대 한 코드 숨겨진 파일에서 실행 된다고 가정 합니다.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs" id="AdReady":::

9.  네이티브 ad와 관련 된 오류를 처리 하기 위해 **erroroccurred** 이벤트에 대 한 이벤트 처리기를 정의 합니다. 다음 예제에서는 테스트 하는 동안 Visual Studio **출력** 창에 오류 정보를 기록 합니다.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs" id="ErrorOccurred":::

10.  응용 프로그램을 컴파일하고 실행 하 여 테스트 광고를 확인 합니다.

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>라이브 광고를 사용 하 여 앱 릴리스

네이티브 ad 구현에서 테스트 광고가 성공적으로 표시 되는 것을 확인 한 후에는 다음 지침에 따라 실제 광고를 표시 하 고 업데이트 된 앱을 스토어에 제출 하도록 앱을 구성 합니다.

1.  네이티브 광고 구현에 [대 한 지침](ui-and-user-experience-guidelines.md#guidelines-for-native-ads)을 준수 하는지 확인 합니다.

2.  파트너 센터에서 [앱 내 광고](../publish/in-app-ads.md) 페이지로 이동 하 여 [ad 단위를 만듭니다](set-up-ad-units-in-your-app.md#live-ad-units). Ad 단위 형식에 대해 **네이티브**를 지정 합니다. Ad 단위 ID와 응용 프로그램 ID를 모두 기록해 둡니다.
    > [!NOTE]
    > 테스트 ad 단위와 라이브 UWP ad 단위의 응용 프로그램 ID 값은 서로 다른 형식입니다. 테스트 응용 프로그램 ID 값은 Guid입니다. 파트너 센터에서 라이브 UWP ad 단위를 만들 때 ad 단위의 응용 프로그램 ID 값은 항상 앱에 대 한 저장소 ID와 일치 합니다. 예를 들어 매장 ID 값은 9NBLGGH4R315와 같습니다.

3. 필요 [에 따라 앱 내 광고](../publish/in-app-ads.md) 페이지의 [중재 설정](../publish/in-app-ads.md#mediation) 섹션에서 설정을 구성 하 여 기본 ad에 대 한 ad 중재를 사용 하도록 설정할 수 있습니다. Ad 중재를 사용 하면 여러 ad 네트워크에서 광고를 표시 하 여 ad 수익과 앱 승격 기능을 최대화할 수 있습니다.

4.  코드에서 테스트 ad 단위 값 (즉, [NativeAdsManagerV2](/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2.-ctor) 생성자의 *ApplicationId* 및 *adunit Id* 매개 변수)을 파트너 센터에서 생성 한 라이브 값으로 바꿉니다.

5.  파트너 센터를 사용 하 여 스토어에 [앱을 제출](../publish/app-submissions.md) 합니다.

6.  파트너 센터에서 [광고 성능 보고서](../publish/advertising-performance-report.md) 를 검토 합니다.

## <a name="manage-ad-units-for-multiple-native-ads-in-your-app"></a>앱에서 여러 네이티브 광고의 ad 단위 관리

단일 앱에서 여러 기본 광고 배치를 사용할 수 있습니다. 이 시나리오에서는 각 기본 광고 위치에 다른 ad 단위를 할당 하는 것이 좋습니다. 기본 ad에 다른 ad 단위를 사용 하면 [중재 설정을](../publish/in-app-ads.md#mediation) 개별적으로 구성 하 고 각 컨트롤에 대 한 개별 [보고 데이터](../publish/advertising-performance-report.md) 를 가져올 수 있습니다. 이를 통해 서비스는 앱에 제공 하는 광고를 더 효율적으로 최적화할 수 있습니다.

> [!IMPORTANT]
> 하나의 앱 에서만 각 ad 단위를 사용할 수 있습니다. 둘 이상의 앱에서 ad 단위를 사용 하는 경우 해당 ad 단위에 대 한 광고가 제공 되지 않습니다.

## <a name="related-topics"></a>관련 항목

* [네이티브 광고에 대 한 지침](ui-and-user-experience-guidelines.md#guidelines-for-native-ads)
* [앱 내 광고](../publish/in-app-ads.md)
* [앱에 대 한 ad 단위 설정](set-up-ad-units-in-your-app.md)
