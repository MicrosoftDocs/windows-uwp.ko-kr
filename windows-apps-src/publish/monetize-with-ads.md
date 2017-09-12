---
author: jnHs
Description: "앱에서 광고 조정을 사용하거나 Microsoft Store Services SDK를 사용해 배너 또는 중간 광고를 표시하는 경우, 광고를 통한 수익 창출 페이지로 광고 사용을 관리하세요."
title: "광고를 통한 수익 창출"
ms.assetid: 09970DE3-461A-4E2A-88E3-68F2399BBCC8
ms.author: wdg-dev-content
ms.date: 07/06/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.openlocfilehash: 6ecd37e54de266764570606ceaa575614dfb050c
ms.sourcegitcommit: 10f8dcf69d37cdb61562fc9f4d268ccb499c368f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/07/2017
---
# <a name="monetize-with-ads"></a>광고를 통한 수익 창출

대시보드의 각 앱에는 **수익 창출** &gt; **광고로 수익 창출** 페이지가 포함되어 있습니다. 이 페이지를 사용하여 앱에서 다음과 같은 개발 시나리오에 대한 광고 사용을 관리합니다.

* UWP 앱은 [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp)에서 [AdControl](https://msdn.microsoft.com/en-us/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx), [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 또는 [NativeAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.aspx)를 사용합니다.
* Windows 8.x 또는 Windows Phone 8.x 앱은 [Windows 및 Windows Phone 8.x용 Microsoft Advertising SDK](http://aka.ms/store-8-sdk)에서 **AdControl** 또는 **InterstitialAd**를 사용합니다.
* Windows 8.x 또는 Windows Phone 8.x 앱은 [Windows 및 Windows Phone 8.x용 Microsoft Advertising SDK](http://aka.ms/store-8-sdk)에서 **AdMediatorControl**을 사용합니다.

이러한 개발 시나리오에 대한 자세한 내용은 [Microsoft Advertising SDK를 사용하여 앱에 광고 표시](../monetize/display-ads-in-your-app.md)를 참조하세요.

<span id="create-ad-unit" />
## <a name="create-ad-units"></a>광고 단위 생성

이 섹션을 사용하여 다음과 같은 시나리오에 대한 광고 단위를 만듭니다.

* 앱이 **AdControl**을 사용하여 배너 광고를 표시합니다. 자세한 내용은 [XAML 및 .NET의 AdControl](../monetize/adcontrol-in-xaml-and--net.md) 및 [HTML5 및 Javascript의 AdControl](../monetize/adcontrol-in-html-5-and-javascript.md)을 참조하세요.
* 앱이 **InterstitialAd**를 사용하여 동영상 중간 광고나 배너 중간 광고를 표시합니다. 자세한 내용은 [중간 광고](../monetize/interstitial-ads.md)를 참조하세요.
* 앱이 **NativeAd**를 사용하여 기본 광고를 표시합니다. 자세한 내용은 [기본 광고](../monetize/native-ads.md)를 참조하세요.

광고 단위를 만들려면

1.  **광고 단위 이름** 필드에 광고 단위의 이름을 입력합니다. 보고용으로 광고 단위를 식별할 때 사용할 수 있는 설명 문자열입니다.
2.  **광고 단위 유형** 드롭다운 목록에서 컨트롤에 표시 중인 광고에 해당하는 광고 단위 유형을 선택합니다. 사용 가능한 옵션으로는 **배너**, **배너 중간 광고**, **동영상 중간 광고** 및 **기본 광고**가 있습니다.
    > [!NOTE]
    > 현재 **기본** 광고 단위를 생성하는 기능은 파일럿 프로그램에 참여하고 있는 일부 개발자만 사용할 수 입습니다. 그러나 조만간 모든 개발자가 이 기능을 사용할 수 있도록 지원할 계획입니다. 파일럿 프로그램에 참여하고 싶다면 aiacare@microsoft.com에 문의하세요.

3.  **장치 패밀리** 드롭다운 목록에서 광고 단위가 사용될 앱의 대상이 되는 장치 패밀리를 선택합니다. 사용 가능한 옵션은 **UWP(Windows 10)**, **PC/태블릿(Windows 8.1)** 또는 **모바일(Windows Phone 8.x)**입니다.
4.  **광고 단위 만들기**를 클릭합니다.

이 페이지의 **사용 가능한 광고 단위** 섹션에 있는 목록 맨 위에 새로운 광고 단위가 표시됩니다. 앱에서의 광고 단위 사용에 대한 자세한 내용은 [앱에서 광고 단위 설정](../monetize/set-up-ad-units-in-your-app.md)을 참조하세요.

> [!NOTE]
> Windows 8 또는 Windows Phone 8.x 앱에서 **AdMediatorControl**을 사용해 배너 광고를 표시하는 경우, 광고 단위를 요청할 필요가 없습니다. 이 시나리오에서는 광고 단위가 자동 생성됩니다.

<span id="available-ad-units" />
## <a name="available-ad-units"></a>사용 가능한 광고 단위

광고 단위가 이 섹션의 맨 아래에 있는 표에 표시됩니다. 각 광고 단위에 대해 **응용 프로그램 ID** 및 **광고 단위 ID**가 표시됩니다. 앱에 광고를 표시하려면 다음과 같이 코드에 이러한 값을 사용해야 합니다.

-   앱에서 배너 광고를 표시하는 경우 [AdControl](https://msdn.microsoft.com/library/mt313154.aspx) 개체의 [ApplicationId](https://msdn.microsoft.com/library/mt313174.aspx) 및 [AdUnitId](https://msdn.microsoft.com/library/mt313171.aspx) 속성에 이러한 값을 할당합니다. 자세한 내용은 [XAML 및 .NET의 AdControl](../monetize/adcontrol-in-xaml-and--net.md) 및 [HTML5 및 Javascript의 AdControl](../monetize/adcontrol-in-html-5-and-javascript.md)을 참조하세요.
-   앱에서 중간 광고를 표시하는 경우 [InterstitialAd](https://msdn.microsoft.com/library/mt313192.aspx) 개체의 [RequestAd](https://msdn.microsoft.com/library/mt313189.aspx) 메서드에 이러한 값을 전달합니다. 자세한 내용은 [중간 광고](../monetize/interstitial-ads.md)를 참조하세요.
-   앱에서 기본 광고를 표시하는 경우 이러한 값을 [NativeAdsManager](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.nativeadsmanager.aspx) 생성자의 *applicationId* 및 *adUnitId* 매개 변수로 전달합니다. 자세한 내용은 [기본 광고](../monetize/native-ads.md)를 참조하세요.

> [!IMPORTANT]
> 앱별로 하나의 광고 단위만 사용할 수 있습니다. 특정 광고 단위를 하나 이상의 앱에 사용하면, 해당 광고 단위에 광고가 제공되지 않습니다.

> [!NOTE]
> 단일 앱에서 여러 배너, 중간, 기본 광고 컨트롤을 사용할 수 있습니다. 이 경우, 각 컨트롤에 다른 광고 단위를 지정하는 것이 좋습니다. 각 컨트롤에 다른 광고 단위를 사용하면 별도로 [조정 설정을 구성](../publish/monetize-with-ads.md#mediation)할 수 있고, 각 컨트롤에 대해 별개의 [보고 데이터](../publish/advertising-performance-report.md)를 가져올 수 있습니다. 이렇게 하면 서비스를 통해 앱에 지원하는 광고를 더 효과적으로 최적화할 수 있습니다.

<span id="mediation" />
## <a name="ad-mediation"></a>광고 조정

앱이 Windows 10용 UWP 앱인 경우, 이 섹션의 옵션을 사용하여 앱의 배너, 중간 광고 또는 기본 광고에 연결되는 UWP 광고 단위에 대한 광고 조정을 지원할 수 있습니다. 광고 조정을 통해 기타 유료 광고 네트워크의 광고와 Microsoft 앱 프로모션 캠페인 비수익 광고 등 여러 광고 네트워크의 광고를 표시하여 광고 수익과 앱 홍보 기능을 최대화할 수 있습니다. Microsoft는 사용자가 선택한 광고 네트워크의 배너 광고 요청을 조정합니다.

앱에서 UWP 광고 단위가 배너, 중간 광고 또는 기본 광고에 이미 연결되어 있는 경우에는 광고 조정을 지원하기 위해 앱에서 코드를 변경할 필요가 없습니다.

> [!NOTE]
> 이 섹션은 UWP 앱 패키지의 **광고 조정** 옵션에 대해 설명합니다. 앱 패키지가 Windows 8 또는 Windows Phone 8.x를 대상으로 하고 [Windows 8 및 Windows Phone 8.x용 Microsoft Advertising SDK](http://aka.ms/store-8-sdk)의 **AdMediatorControl**을 사용하고 있다면, 대시보드의 **광고 조정** 섹션에 여러 옵션들이 표시됩니다. **AdMediatorControl**을 사용하는 Windows 8.x 및 Windows Phone 8.x 앱 패키지의 광고 조정 설정 구성에 대한 자세한 내용은 [이 문서](https://msdn.microsoft.com/library/windows/apps/mt219689)를 참조하세요.

앱에서 UWP 광고 단위에 대한 광고 조정 설정을 구성하려면

1. **광고 조정 구성 대상** 드롭다운 목록에서 구성하고 싶은 배너, 중간 광고 또는 기본 광고가 포함된 UWP 앱 패키지를 선택합니다.

2. **광고 장치 유형** 드롭다운 목록에서 구성하고 싶은 광고 단위 유형(**배너**, **배너 중간 광고**, **동영상 중간 광고** 또는 **기본 광고**)을 선택합니다.

3. **광고 단위** 드롭다운 목록에서 구성하고 싶은 UWP 광고 단위의 이름을 선택합니다.
    > [!NOTE]
    > UWP 광고 단위에 대해 광고 조정을 지원하면 타사 광고 네트워크에서 광고 단위를 가져올 필요가 없습니다. Microsoft의 광고 조정 서비스가 자동으로 필요한 타사 광고 단위를 생성합니다.

4. 기본값으로 **Microsoft가 앱에 가장 좋은 조정 설정을 선택** 확인란이 선택되어 있습니다. 이 옵션은 기계 학습 알고리즘을 사용, 자동으로 앱이 지원하는 지역/국가에서 광고 수익을 극대화 하는 데 도움이 되는 앱의 광고 조정 설정을 선택합니다. 이 옵션을 사용하는 것이 좋습니다. 고유의 광고 조정 설정을 선택하고 싶다면 확인란의 선택을 취소합니다.
    > [!NOTE]
    > 이 섹션의 나머지 단계는 확인란 선택을 취소하고 고유의 광고 조정 설정을 선택한 경우에만 적용됩니다.

5. **대상** 드롭다운 목록에서 **기본**을 선택해 광고 조정 설정을 기본값으로 구성할 수 있습니다. 이 기본 설정이 지역/국가에 특정적인 구성을 적용한 지역/국가를 제외한 모든 지역/국가에 적용됩니다.

6. 다음은 유료 네트워크(광고 컨트롤을 유료 네트워크 (노출에 대한 수익을 지급하는)와 다른 광고 네트워크(노출에 대한 수익을 지급하지 않는)의 컨트롤에 표시할 광고의 비율을 지정합니다. **유료 광고 네트워크** 및 **다른 광고 네트워크**의 **가중치** 필드에 0~100의 값을 입력해야 합니다.  

7. **유료 광고 네트워크** 섹션에서 사용하고 싶은 각 유료 네트워크의 **활성** 열의 확인란을 선택한 후, **순위** 열 옆 화살표를 사용하여 순위 별로 네트워크를 주문합니다(컨트롤이 각 네트워크를 얼마나 많이 사용할지 지정).

    현재 다음 유료 네트워크를 지원하고 있습니다. [일부 지역/국가에서 사용할 수 없는](#network-markets) 네트워크도 있습니다.

    -   **AOL 및 AppNexus**. Microsoft가 관리하는 광고 네트워크로 파트너 네트워크, AOL, AppNexus를 통해 광고를 제공합니다. 이 네트워크는 **배너** 및 **동영상 중간** 광고 단위에 지원됩니다.
        > [!NOTE]
        > **AOL 및 AppNexus**는 항상 **배너** 광고 단위의 **유료 광고 네트워크** 목록 맨 위에 표시되며, 이런 유형의 광고에서는 순위를 낮게 조정할 수 없습니다.

    -   **AppNexus(직접)**: [AppNexus](https://www.appnexus.com)에서 동영상 중간 광고를 지원하려면 이 옵션을 선택합니다. 이 네트워크는 **동영상 중간** 및 **기본**  광고 단위에서 지원됩니다.

    -   **Microsoft 앱 설치 광고**. Windows 에코시스템에서 [앱에 대해 홍보 광고 캠페인을 생성](create-an-ad-campaign-for-your-app.md)하는 다른 개발자가 생성한 앱 설치 광고 또는 앱 Re-engagement 광고를 지원하려면 이 옵션을 선택합니다. 이 네트워크는 **배너**, **배너 중간** 및 **기본** 광고 단위에 지원됩니다.

    -   **Smaato**: [Smaato](https://www.smaato.com/)에서 광고를 지원하려면 이 옵션을 선택합니다. 이 네트워크는 **배너** 광고 단위에 지원됩니다.

    -   **smartclip**: [smartclip](http://www.smartclip.com/)에서 광고를 지원하려면 이 옵션을 선택합니다. 이 네트워크는 **동영상 중간** 광고 단위에서 지원됩니다.

    -   **SpotX**: [SpotX](https://www.spotx.tv/)에서 광고를 지원하려면 이 옵션을 선택합니다. 이 네트워크는 **동영상 중간** 광고 단위에서 지원됩니다.

    -   **Taboola**: [Taboola](https://www.taboola.com/)에서 광고를 지원하려면 이 옵션을 선택합니다. 이 네트워크는 **배너** 광고 단위에 지원됩니다.

8. **배너** 또는 **배너 중간** 광고 단위를 선택하면 **다른 광고 네트워크**라는 섹션이 나타납니다. 이 섹션의 네트워크는 광고 노출에 대한 광고 수익을 획득할 수 없습니다. 대신 이러한 네트워크는 앱 홍보 캠페인 등 소스의 광고를 표시합니다. 

    **기타 광고 네트워크** 섹션에서 사용하고 싶은 각 기타 네트워크의 **활성** 열 확인란을 선택한 후, **순위** 열 옆 화살표를 사용하여 순위 별로 네트워크를 주문합니다(컨트롤이 각 네트워크를 얼마나 많이 사용할지 지정). 현재 다음과 같은 기타 네트워크가 지원되고 있습니다.

    -   **Microsoft 커뮤니티 광고**. [앱 중 하나에 대해 홍보 광고 캠페인을 생성](create-an-ad-campaign-for-your-app.md)하고, 이 캠페인을 [커뮤니티 광고 캠페인](about-community-ads.md)으로 구성할 경우, 이 옵션을 사용하여 이 캠페인의 광고를 표시합니다. 

    -   **Microsoft 하우스 광고** [앱 중 하나에 대해 홍보 광고 캠페인을 생성](create-an-ad-campaign-for-your-app.md)하고, 이 캠페인을 [하우스 광고 캠페인](about-house-ads.md)으로 구성할 경우, 이 옵션을 사용하여 이 캠페인의 광고를 표시합니다.

9. 기본 조정 구성을 재구성하려는 각 지역/국가의 경우, **대상** 드롭다운 목록에서 지역/국가를 선택한 후 광고 네트워크 선택 및 순위를 업데이트합니다.

10. **저장**을 클릭합니다.

<span id="network-markets" />
### <a name="supported-markets-for-ad-networks"></a>광고 네트워크 지원 지역/국가

사용 가능한 광고 네트워크가 (다음을 제외한)UWP 앱 [지원 지역/국가](define-pricing-and-market-selection.md#windows-store-consumer-markets)에 광고를 서비스합니다.

|  광고 네트워크  |  지원되는 지역/국가  |
|--------------|---------------------|
| Smaato | 브라질, 캐나다, 프랑스, 독일, 이탈리아, 일본, 스페인, 영국, 미국 |
| smartclip | 오스트리아, 벨기에, 덴마크, 핀란드, 독일, 이탈리아, 네덜란드, 노르웨이, 스웨덴, 스위스  |


## <a name="microsoft-affiliate-ads"></a>Microsoft 계열사 광고

앱에서 Microsoft 계열사 광고를 표시하려는 경우 이 섹션에 있는 확인란을 선택합니다. 이 확인란을 선택하면 다른 광고 네트워크의 광고를 사용할 수 없는 경우에도 음악, 게임, 영화, 앱, 하드웨어 및 소프트웨어를 비롯한 스토어의 제품 광고가 앱에 제공됩니다. 고객이 지정된 기여 기간 내에 스토어에서 광고를 클릭하고 제품을 구입하면 승인된 구매에 대해 수수료를 받게 됩니다.

이 선택을 변경하는 경우 변경 내용을 적용하기 위해 앱을 다시 게시할 필요가 없습니다. Microsoft 계열사 광고에 대한 자세한 내용은 [계열사 광고 정보](about-affiliate-ads.md)를 참조하세요.

## <a name="coppa-compliance"></a>COPPA 준수

“COPPA”(아동에 관한 온라인 개인 정보 보호법)를 위해 13세 미만 아동을 대상으로 하는 앱인 경우 Microsoft에 이를 알려야 합니다. 개발자 센터를 사용하여 앱이 13세 미만 아동을 대상으로 한다는 것을 Microsoft에 알리는 경우 Microsoft에서 앱에 광고를 제공할 때 동작 광고 서비스를 사용하지 않도록 설정하는 단계를 진행합니다. 앱이 13세 미만 아동을 대상으로 하는 경우 COPPA에 따른 특정한 의무 조항이 있습니다.

COPPA에 따른 의무 조항에 대한 자세한 내용은 [이 페이지](http://go.microsoft.com/fwlink/p/?linkid=536558)를 참조하세요.
