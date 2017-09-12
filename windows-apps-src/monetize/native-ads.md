---
author: mcleanbyron
description: "UWP 앱에 기본 광고를 추가하는 방법을 알아봅니다."
title: "기본 광고"
ms.author: mcleans
ms.date: 06/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 광고, 광고, 광고 관리, 기본 광고"
ms.openlocfilehash: 47a69e48f04c670a462c34083af1117d2c7908d8
ms.sourcegitcommit: 8c4d50ef819ed1a2f8cac4eebefb5ccdaf3fa898
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/27/2017
---
# <a name="native-ads"></a>기본 광고

기본 광고는 제목, 이미지, 설명, 행동 촉구 텍스트 등의 각 광고 크리에이티브 조각이 앱과 통합할 수 있는 개별 요소로 앱에 제공되는 구성 요소 기반 광고입니다. 이러한 요소를 고유의 폰트, 색상, 애니메이션, 기타 UI 구성 요소를 사용해 앱에 통합, 앱의 모양과 느낌에 부합하는 동시에 높은 광고 수익이 창출되도록 간결한 사용자 환경으로 통합할 수 있습니다.

기본 광고는 광고주에게 성과가 높은 배치 형태를 제공합니다. 광고 환경이 앱과 깊이 통합되어 있고, 따라서 사용자는 이런 유형의 광고에 더 많이 상호작용을 하는 경향이 있습니다.

> [!NOTE]
> 스토어의 앱 공개 버전에 기본 광고를 제공하려면, 개발자 센터 대시보드의 **앱으로 수익 창출** 페이지에서 **기본** 광고 단위를 생성해야 합니다. 현재 **기본** 광고 단위를 생성하는 기능은 파일럿 프로그램에 참여하고 있는 일부 개발자만 사용할 수 있습니다. 그러나 조만간 모든 개발자가 이 기능을 사용할 수 있도록 지원할 계획입니다. 파일럿 프로그램에 참여하고 싶다면 aiacare@microsoft.com에 문의하세요.

> [!NOTE]
> 현재 Windows 10용 XAML 기반 UWP 앱에서만 기본 광고를 지원하고 있습니다. HTML과 JavaScript를 사용해 작성한 UWP 앱은 향후 Microsoft Advertising SK 릴리스에서 지원할 예정입니다.

## <a name="prerequisites"></a>필수 구성 요소

* Visual Studio 2015 이후 릴리스와 함께 [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp)(10.0.4 버전 이상)를 설치합니다. 설치 지침은 [이 문서](install-the-microsoft-advertising-libraries.md)를 참조하세요. MSI 설치 관리자로 개발자 컴퓨터에 SDK를 설치할 수 있습니다. 또는 NuGet 패키지를 통해 특정 프로젝트에 사용할 SDK를 설치하는 방법도 있습니다.

## <a name="integrate-a-native-ad-into-your-app"></a>앱에 기본 광고를 통합

이 지침을 따라 앱에 기본 광고를 통합하고, 기본 광고가 테스트 광고를 표시하는지 확인하세요.

1. Visual Studio에서 프로젝트를 열거나 새 프로젝트를 만듭니다.

2. 프로젝트의 대상이 **모든 CPU**인 경우 아키텍처별 빌드 출력(예: **x86**)을 사용하도록 프로젝트를 업데이트합니다. 프로젝트의 대상이 **모든 CPU**인 경우 다음 단계에 따라 Microsoft Advertising SDK에 대한 참조를 성공적으로 추가하지 못할 수 있습니다. 자세한 내용은 [프로젝트에서 모든 CPU를 대상으로 할 경우 발생하는 참조 오류](known-issues-for-the-advertising-libraries.md#reference_errors)를 참조하세요.

3.  **솔루션 탐색기** 창에서 **참조**를 마우스 오른쪽 단추로 클릭한 다음 **참조 추가**를 선택합니다.

4.  **참조 관리자**에서 **유니버설 Windows**를 확장하고 **확장**을 클릭한 후 **Microsoft Advertising SDK for XAML**(버전 10.0) 옆의 확인란을 선택합니다.

5.  **참조 관리자**에서 확인을 클릭합니다.

6. 앱의 적절한 코드 파일(예: MainPage.xaml.cs 또는 다른 페이지의 코드 파일)에 다음 네임스페이스 참조를 추가합니다.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#Namespaces)]

7.  앱의 적절한 위치(예: ```MainPage``` 또는 다른 페이지)에서 [NativeAdsManager](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.aspx) 개체를 비롯하여 중간 광고 응용 프로그램 ID 및 광고 단위 ID를 나타내는 여러 문자열 필드를 선언합니다. 다음 코드 예제에서는 `myAppId` 및 `myAdUnitId` 필드를 기본 광고에 대한 [테스트 값](test-mode-values.md)에 할당합니다.

    > [!NOTE]
    > 모든 **NativeAdsManager**에는 컨트롤할 기본 광고를 지원하는 서비스가 사용하는 *광고 단위*가 있고, 모든 광고 단위는 *광고 단위 ID*와 *응용 프로그램 ID*로 구성되어 있습니다. 이 단계에서 컨트롤에 테스트 광고 단위 ID와 응용 프로그램 ID 값을 할당하세요. 이 테스트 값은 앱 테스트 버전에서만 사용할 수 있습니다. 앱을 스토어에 게시하기 전, Windows 개발자 센터에서 [이 테스트 값을 라이브 값으로](#release) 변경하세요.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#Variables)]

8.  시작할 때 실행되는 코드에서(예를 들면 해당 페이지에 대한 생성자에서), **NativeAdsManager** 개체를 인스턴스화하고, 개체 [AdRedady](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.adready.aspx) 및 [ErroOccurred](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.erroroccurred.aspx) 이벤트에 이벤트 처리기를 연결합니다.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#ConfigureNativeAd)]

9.  기본 광고를 표시할 준비가 되면 광고를 가져올 [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.requestad.aspx) 메서드를 호출합니다.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#RequestAd)]

10.  앱에 기본 광고가 준비되면, **AdReady** 이벤트 처리기를 호출하고, 기본 광고를 표시하는 [NativeAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.aspx) 개를 *e* 매개 변수로 전달합니다. **NativeAd** 속성을 사용해 기본 광고의 각 요소를 가져오고, 이 요소들을 페이지에 표시합니다. 기본 광고의 컨테이너로 동작하는 UI 요소를 등록하기 위해 [RegisterAdContainer](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.registeradcontainer.aspx) 메서드를 호출해야 합니다. 광고 노출과 클릭을 올바르게 추적하기 위해 필요합니다.
  > [!NOTE]
  > 반드시 필요한 기본 광고 요소들이 있습니다. 이러한 요소들은 항상 앱에 표시되어야 합니다. 자세한 내용은 [필수 구성 요소와 지침](#requirements-and-guidelines)을 참조하세요.

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

11.  **ErrorOccurred** 이벤트에 대한 이벤트 처리기를 정의해 기본 광고와 관련이 있는 오류를 처리합니다. 다음 예제는 테스트 동안 오류 정보를 Visual Studio **출력** 창에 씁니다.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#ErrorOccurred)]

12.  앱을 컴파일하고 실행하여 테스트 광고와 함께 표시합니다.

<span id="release" />
## <a name="release-your-app-with-live-ads"></a>라이브 광고와 앱 릴리스

기본 광고가 테스트 광고를 성공적으로 표시하는지 확인한 후, 다음 지침에 따라 앱이 진짜 광고를 표시하도록 구성하고, 업데이트한 앱을 스토어에 제출합니다.

1.  기본 광고에 기본 광고에 대한 [요구 사항 및 지침](#requirements-and-guidelines)을 적용했는지 확인합니다.

2.  개발자 센터 대시보드에서 [앱의 광고 수익 창출](../publish/monetize-with-ads.md) 페이지로 이동한 후 [광고 단위를 만듭니다](../monetize/set-up-ad-units-in-your-app.md). 광고 단위 유형으로 **기본**을 지정합니다. 광고 단위 ID와 응용 프로그램 ID를 적어둡니다.
    > [!IMPORTANT]
    > 앱별로 하나의 광고 단위만 사용할 수 있습니다. 특정 광고 단위를 하나 이상의 앱에 사용하면, 해당 광고 단위에 광고가 제공되지 않습니다.

3. [광고로 수익 창출](../publish/monetize-with-ads.md) 페이지의 [광고 조정](../publish/monetize-with-ads.md#mediation) 섹션에서 설정을 구성하여, 기본 광고의 광고 조정을 사용하는 방법도 있습니다. 광고 조정으로 여러 광고 네트워크에서 광고를 표시해 광고 수익과 앱 홍보 기능을 극대화 할 수 있습니다.

4.  코드에서 테스트 광고 단위 값([NativeAdsManager](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.nativeadsmanager.aspx) 생성자의 *applicationId* 및 *adUnitId* 매개 변수)을 개발자 센터에서 생성한 라이브 값으로 바꿉니다.

5.  개발자 센터 대시보드를 사용하여 스토어에 [앱을 제출](../publish/app-submissions.md)합니다.

6.  개발자 센터 대시보드에서 [광고 성과 보고서](../publish/advertising-performance-report.md)를 검토합니다.

<span id="requirements-and-guidelines" />
## <a name="requirements-and-guidelines"></a>요구 사항 및 지침

기본 광고는 사용자에게 광고 콘텐츠를 제시하는 방식을 많이 컨트롤할 수 있습니다. 이러한 요구 사항과 지침을 따르면 광고 메시지가 사용자에 도달하도록 만들고, 동시에 사용자에게 혼란을 주는 기본 광고 환경을 전달하는 것을 피할 수 있습니다.

### <a name="register-the-container-for-your-native-ad"></a>기본 광고 컨테이너 등록

코드에서 **NativeAd** 개체의 [RegisterAdContainer](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.registeradcontainer.aspx) 메서드를 호출, 기본 광고의 컨테이너로 작동하는 UI 요소를 등록해야 합니다. 또한 광고를 클릭 가능한 대상으로 등록하는 특정 컨트롤을 호출할 수도 있습니다. 이는 광고 노출과 클릭을 올바르게 추적하는 데 필요합니다.

사용할 수 있는 **RegisterAdContainer** 메서드의 오버로드 2개는 다음과 같습니다.

* 기본 광고 요소 모두에 대한 전체 컨테이너를 클릭할 수 있도록 [RegisterAdContainer(FrameworkElement)](https://msdn.microsoft.com/library/windows/apps/mt809188.aspx) 메서드를 호출하고, 컨테이너 컨트롤을 메서드로 전달합니다. 예를 들어, **StackPanel**에 모두 호스트된 별개 컨트롤의 기본 광고 요소 전부를 표시하고, 전체 **StackPanel**을 클릭할 수 있도록 하려면, **StackPanel**을 이 메서드로 전달합니다.

* 특정 기본 광고 요소만 클릭할 수 있도록 하려면, [RegisterAdContainer (FrameworkElement, IVector(FrameworkElement))](https://msdn.microsoft.com/library/windows/apps/mt809189.aspx) 메서드를 호출합니다. 두 번째 매개 변수로 전달한 컨트롤만 클릭할 수 있습니다.

### <a name="required-native-ad-elements"></a>필수 기본 광고 요소

최소한 기본 광고 디자인에서 사용자에게 다음 기본 광고 요소를 항상 표시해야 합니다. 이러한 요소를 포함하지 않으면, 광고 단위의 성과가 나빠지고 수익이 하락할 수 있습니다.

1. 항상 기본 광고 제목을 표시합니다(**NativeAd** 개체의 [제목](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.title.aspx) 속성에서 이용 가능). 최소 25자 이상을 표시할 수 있는 충분한 공간을 제공합니다. 제목이 너무 길면, 추가 텍스트를 줄임표로 바꿉니다.
2. 항상 다음 요소 중 하나 이상을 표시해 앱 나머지 부분과 기본 광고 환경을 차별화 하고, 광고주가 제공하는 콘텐츠를 명확히 설명합니다.
  * 구별할 수 있는 *광고* 아이콘(**NativeAd** 개체의 [AdIcon](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.adicon.aspx) 속성에서 사용 가능). 이 아이콘은 Microsoft에서 제공됩니다.
  * *sponsored by* 텍스트(**NativeAd** 개체의 [SponsoredBy](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.sponsoredby.aspx) 속성에서 사용 가능). 이 텍스트는 광고주가 제공합니다.
  * *sponsored by* 텍스트를 대신해 "후원 콘텐츠", "홍보 콘텐츠", "권장 콘텐츠" 등 앱 나머지 부분과 기본 광고 환경을 구분하는 데 도움을 주는 다른 텍스트를 표시할 수도 있습니다.

### <a name="user-experience"></a>사용자 환경

기본 광고가 앱 나머지 부분과 명확히 구분되어야 하고, 실수로 클릭하는 것을 방지하기 위해 주변에 공간이 있어야 합니다. 광고 콘텐츠를 앱 나머지와 분리하기 위해 경계, 다른 배경, 기타 UI를 사용합니다. 실수로 클릭을 하는 것은 장기적으로 최종 사용자의 경험이나 광고 기반 수익에 도움이 되지 않는다는 점을 유념하세요.

### <a name="description"></a>Description

광고에 대한 설명을 표시하려면(**NativeAd** 개체의 [설명](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.description.aspx) 속성에 사용 가능), 75자 이상을 표시할 수 있는 충분한 공간을 제공하세요. 애니메이션을 사용해 광고 설명의 콘텐츠를 전부 표시하는 것이 좋습니다.

### <a name="call-to-action"></a>Call to action

*call to action(행동 촉발)* 텍스트(**NativeAd** 개체의 [CallToAction](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.calltoaction.aspx) 속성에서 사용 가능)는 아주 중요한 광고 구성 요소입니다. 이러한 텍스트를 표시하려면, 다음 지침을 따릅니다.

* 항상 단추나 하이퍼링크 등 클릭할 수 있는 컨트롤로 *Call to action* 텍스트를 표시합니다.
* 항상 전체 *Call to action* 텍스트를 표시합니다.
* *Call to action* 텍스트를 광고주의 나머지 홍보 텍스트와 분리합니다.

### <a name="learn-and-optimize"></a>학습 및 최적화

앱의 여러 기본 광고 배치에 다른 광고 단위를 생성해 사용하는 것이 좋습니다. 그러면 각 기본 광고 배치 별로 별도 보고서 데이터를 얻을 수 있습니다. 또한 이 데이터를 사용해 각 기본 광고 배치의 성과를 최적화하도록 변경할 수 있습니다.

## <a name="related-topics"></a>관련 항목

* [광고로 수익 창출](../publish/monetize-with-ads.md)
* [앱에서 광고 단위 설정](../monetize/set-up-ad-units-in-your-app.md)
