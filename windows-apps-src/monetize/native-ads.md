---
author: Xansky
description: UWP 앱에 기본 광고를 추가하는 방법을 알아봅니다.
title: 기본 광고
ms.author: mhopkins
ms.date: 05/11/2018
ms.topic: article
keywords: windows 10, uwp, 광고, 광고, 광고 관리, 기본 광고
ms.localizationpriority: medium
ms.openlocfilehash: 123934c911f342dd57033c8e204e58bc00a5f18f
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5861412"
---
# <a name="native-ads"></a>기본 광고

기본 광고는 제목, 이미지, 설명, 행동 촉구 텍스트 등의 각 광고 크리에이티브 조각이 앱과 통합할 수 있는 개별 요소로 앱에 제공되는 구성 요소 기반 광고입니다. 이러한 요소를 고유의 폰트, 색상, 애니메이션, 기타 UI 구성 요소를 사용해 앱에 통합, 앱의 모양과 느낌에 부합하는 동시에 높은 광고 수익이 창출되도록 간결한 사용자 환경으로 통합할 수 있습니다.

기본 광고는 광고주에게 성과가 높은 배치 형태를 제공합니다. 광고 환경이 앱과 깊이 통합되어 있고, 따라서 사용자는 이런 유형의 광고에 더 많이 상호작용을 하는 경향이 있습니다.

> [!NOTE]
> 현재 Windows 10용 XAML 기반 UWP 앱에서만 기본 광고를 지원하고 있습니다. HTML과 JavaScript를 사용해 작성한 UWP 앱은 향후 Microsoft Advertising SK 릴리스에서 지원할 예정입니다.

## <a name="prerequisites"></a>필수 구성 요소

* Visual Studio 2015 이상 릴리스와 함께 [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp)를 설치합니다. 설치 지침은 [이 문서](install-the-microsoft-advertising-libraries.md)를 참조하세요.

## <a name="integrate-a-native-ad-into-your-app"></a>앱에 기본 광고를 통합

이 지침을 따라 앱에 기본 광고를 통합하고, 기본 광고가 테스트 광고를 표시하는지 확인하세요.

1. Visual Studio에서 프로젝트를 열거나 새 프로젝트를 만듭니다.
    > [!NOTE]
    > 기존 프로젝트를 사용하고 있는 경우 프로젝트에서 Package.appxmanifest 파일을 열고 **인터넷(클라이언트)** 기능이 선택되었는지 확인합니다. 앱에서 테스트 광고와 라이브 광고를 수신하려면 이 기능이 필요합니다.

2. 프로젝트의 대상이 **모든 CPU**인 경우 아키텍처별 빌드 출력(예: **x86**)을 사용하도록 프로젝트를 업데이트합니다. 프로젝트의 대상이 **모든 CPU**인 경우 다음 단계에 따라 Microsoft Advertising SDK에 대한 참조를 성공적으로 추가하지 못할 수 있습니다. 자세한 내용은 [프로젝트에서 모든 CPU를 대상으로 할 경우 발생하는 참조 오류](known-issues-for-the-advertising-libraries.md#reference_errors)를 참조하세요.

3. 프로젝트에 Microsoft Advertising SDK 참조를 추가합니다.

    1. **솔루션 탐색기** 창에서 **참조**를 마우스 오른쪽 단추로 클릭한 다음 **참조 추가**를 선택합니다.
    2.  **참조 관리자**에서 **유니버설 Windows**를 확장하고 **확장**을 클릭한 후 **Microsoft Advertising SDK for XAML**(버전 10.0) 옆의 확인란을 선택합니다.
    3.  **참조 관리자**에서 확인을 클릭합니다.

4. 앱의 적절한 코드 파일(예: MainPage.xaml.cs 또는 다른 페이지의 코드 파일)에 다음 네임스페이스 참조를 추가합니다.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#Namespaces)]

5.  앱의 적절한 위치(예: ```MainPage``` 또는 다른 페이지)에서 [NativeAdsManagerV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2) 개체를 비롯하여 중간 광고 응용 프로그램 ID 및 광고 단위 ID를 나타내는 여러 문자열 필드를 선언합니다. 다음 코드 예제에서는 `myAppId` 및 `myAdUnitId` 필드를 기본 광고에 대한 [테스트 값](set-up-ad-units-in-your-app.md#test-ad-units)에 할당합니다.
    > [!NOTE]
    > 모든 **NativeAdsManagerV2**에는 컨트롤할 기본 광고를 지원하는 서비스가 사용하는 *광고 단위*가 있고, 모든 광고 단위는 *광고 단위 ID*와 *응용 프로그램 ID*로 구성되어 있습니다. 이 단계에서 컨트롤에 테스트 광고 단위 ID와 응용 프로그램 ID 값을 할당하세요. 이 테스트 값은 앱 테스트 버전에서만 사용할 수 있습니다. 앱을 Microsoft Store에 게시하기 전, Windows 개발자 센터에서 [이 테스트 값을 라이브 값으로](#release) 변경하세요.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#Variables)]

6.  시작할 때 실행되는 코드에서(예를 들면 해당 페이지에 대한 생성자에서), **NativeAdsManagerV2** 개체를 인스턴스화하고, 개체 **AdReady** 및 **ErrorOccurred** 이벤트에 이벤트 처리기를 연결합니다.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#ConfigureNativeAd)]

7.  기본 광고를 표시할 준비가 되면 광고를 가져올 **RequestAd** 메서드를 호출합니다.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#RequestAd)]

8.  앱에 기본 광고가 준비되면, [AdReady](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2.adready) 이벤트 처리기를 호출하고, 기본 광고를 표시하는 [NativeAdV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadv2) 개체를 *e* 매개 변수로 전달합니다. **NativeAdV2** 속성을 사용해 기본 광고의 각 요소를 가져오고, 이 요소들을 페이지에 표시합니다. 기본 광고의 컨테이너로 동작하는 UI 요소를 등록하기 위해 **RegisterAdContainer** 메서드를 호출해야 합니다. 광고 노출과 클릭을 올바르게 추적하기 위해 필요합니다.
    > [!NOTE]
    > 반드시 필요한 기본 광고 요소들이 있습니다. 이러한 요소들은 항상 앱에 표시되어야 합니다. 자세한 내용은 [기본 광고에 대한 지침](ui-and-user-experience-guidelines.md#guidelines-for-native-ads)을 참조하세요.

    예를 들어, 앱에 ```MainPage```(또는 다른 페이지)와 다음 **StackPanel**이 포함되어 있다고 가정하겠습니다. 이 **StackPanel**은 제목, 설명, 이미지, *sponsored by* 문자, *call to action* 문자를 표시할 단추 등 기본 광고의 여러 요소들을 표시하는 여러 컨트롤을 포함합니다.

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

    다음 코드 예제는 **StackPanel**의 컨트롤에서 각 기본 광고 요소를 표시하고, **StackPanel**을 등록하기 위해 **RegisterAdContainer**를 호출하는 **AdReady** 이벤트 처리기에 대해 설명합니다. 이 코드는 **StackPanel**이 포함된 페이지의 코드 백그라운드 파일에서 실행된다고 가정합니다.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#AdReady)]

9.  **ErrorOccurred** 이벤트에 대한 이벤트 처리기를 정의해 기본 광고와 관련이 있는 오류를 처리합니다. 다음 예제는 테스트 동안 오류 정보를 Visual Studio **출력** 창에 씁니다.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#ErrorOccurred)]

10.  앱을 컴파일하고 실행하여 테스트 광고와 함께 표시합니다.

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>라이브 광고와 앱 릴리스

기본 광고가 테스트 광고를 성공적으로 표시하는지 확인한 후, 다음 지침에 따라 앱이 진짜 광고를 표시하도록 구성하고, 업데이트한 앱을 스토어에 제출합니다.

1.  기본 광고를 구현하려면 [기본 광고에 대한 지침](ui-and-user-experience-guidelines.md#guidelines-for-native-ads)을 따라야 합니다.

2.  개발자 센터 대시보드에서 [인앱 광고](../publish/in-app-ads.md) 페이지로 이동하여 [광고 단위를 만듭니다](set-up-ad-units-in-your-app.md#live-ad-units). 광고 단위 유형으로 **기본**을 지정합니다. 광고 단위 ID와 응용 프로그램 ID를 적어둡니다.
    > [!NOTE]
    > 테스트 광고 단위와 라이브 UWP 광고 단위의 응용 프로그램 ID 값은 형식이 서로 다릅니다. 테스트 응용 프로그램 ID 값은 GUID입니다. 대시보드에서 라이브 UWP 광고 단위를 만들 때 광고 단위의 응용 프로그램 ID 값은 항상 앱의 Store ID와 일치합니다(예: Store ID 값은 9NBLGGH4R315와 비슷한 형식).

3. [인앱 광고](../publish/in-app-ads.md) 페이지의 [조정 설정](../publish/in-app-ads.md#mediation) 섹션에서 설정을 구성하여, 기본 광고의 광고 조정을 사용하는 방법도 있습니다. 광고 조정으로 여러 광고 네트워크에서 광고를 표시해 광고 수익과 앱 홍보 기능을 극대화 할 수 있습니다.

4.  코드에서 테스트 광고 단위 값([NativeAdsManagerV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2.-ctor) 생성자의 *applicationId* 및 *adUnitId* 매개 변수)을 개발자 센터에서 생성한 라이브 값으로 바꿉니다.

5.  개발자 센터 대시보드를 사용하여 스토어에 [앱을 제출](../publish/app-submissions.md)합니다.

6.  Windows 개발자 센터 대시보드에서 [광고 성과 보고서](../publish/advertising-performance-report.md)를 검토합니다.

## <a name="manage-ad-units-for-multiple-native-ads-in-your-app"></a>앱에서 여러 기본 광고에 대한 광고 단위 관리

하나의 앱에 여러 기본 광고 배치를 사용할 수 있습니다. 이 경우, 각 기본 광고 배치에 다른 광고 단위를 지정하는 것이 좋습니다. 기본 광고에 서로 다른 광고 단위를 사용하면 별도로 [조정 설정을 구성](../publish/in-app-ads.md#mediation)할 수 있고, 각 컨트롤에 대해 별개의 [보고 데이터](../publish/advertising-performance-report.md)를 가져올 수 있습니다. 또 이렇게 하면 서비스가 지원하는 앱에서 더 효과적으로 광고를 최적화할 수 있습니다.

> [!IMPORTANT]
> 앱별로 하나의 광고 단위만 사용할 수 있습니다. 특정 광고 단위를 하나 이상의 앱에 사용하면, 해당 광고 단위에 광고가 제공되지 않습니다.

## <a name="related-topics"></a>관련 항목

* [기본 광고에 대한 지침](ui-and-user-experience-guidelines.md#guidelines-for-native-ads)
* [인앱 광고](../publish/in-app-ads.md)
* [앱의 광고 단위 설정](set-up-ad-units-in-your-app.md)
