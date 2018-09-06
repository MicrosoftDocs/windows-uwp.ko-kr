---
author: jnHs
Description: If your app displays ads using the Microsoft Advertising SDK, use the In-app ads page of the Dev Center dashboard to manage your use of ads.
title: 인앱 광고
ms.assetid: 09970DE3-461A-4E2A-88E3-68F2399BBCC8
ms.author: wdg-dev-content
ms.date: 06/28/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 83c4645a09a38a76dfd230436e858e222d817eab
ms.sourcegitcommit: 914b38559852aaefe7e9468f6f53a7465bf36e30
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2018
ms.locfileid: "3393631"
---
# <a name="in-app-ads"></a>인앱 광고

Windows 개발자 센터 대시보드의 **수익 창출** &gt; **인앱 광고** 페이지에서 다음에 대한 광고 단위를 만들고 관리합니다.

* [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp)를 사용하는 UWP(유니버설 Windows 플랫폼) 앱.
* [Microsoft Advertising SDK for Windows 및 Windows Phone 8.x](http://aka.ms/store-8-sdk)를 사용하는 Windows8.1 및 Windows Phone 8.x 앱.

이러한 SDK를 앱과 통합하여 광고를 표시하는 방법에 대한 자세한 내용은 [Microsoft Advertising SDK를 사용하여 앱에 광고 표시](../monetize/display-ads-in-your-app.md)를 참조하세요.

<span id="create-ad-unit" />

## <a name="create-ad-units"></a>광고 단위 생성

앱에서 [배너](../monetize/banner-ads.md), [중간 광고](../monetize/interstitial-ads.md) 또는 [기본 광고](../monetize/native-ads.md)에 대한 광고 단위를 생성하려면:

1.  대시보드에서 **수익 창출** &gt; **인앱 광고** 페이지로 이동하고 **광고 단위 만들기**를 클릭합니다.
2.  **앱 이름** 드롭다운 목록에서 광고 단위가 사용될 앱을 선택합니다.
3.  **광고 단위 이름** 필드에 광고 단위의 이름을 입력합니다. 보고용으로 광고 단위를 식별할 때 사용할 수 있는 설명 문자열입니다.
4.  **광고 단위 유형** 드롭다운 목록에서 광고 유형을 선택합니다.

    * 앱에서 배너 광고를 표시 하는 **배너**를 선택 합니다.
    * 앱에서 동영상 중간 광고 또는 배너 중간 광고를 표시 하는, **동영상 중간 광고** 또는 **배너 중간 광고** (표시 하려는 중간 광고 유형에 대 한 적절 한 옵션을 선택 하세요)를 선택 합니다.
    * 앱에 기본 광고를 표시 하는 경우 **기본**선택 합니다.

5. **장치 패밀리** 드롭다운 목록에서 광고 단위가 사용될 앱의 대상이 되는 장치 패밀리를 선택합니다. 사용 가능한 옵션은 **UWP(Windows 10)**, **PC/태블릿(Windows 8.1)** 또는 **모바일(Windows Phone 8.x)** 입니다.

6. 필요에 따라 다음과 같은 추가 설정을 구성합니다.

    * 광고 단위에 대해 **UWP(Windows 10)** 장치 패밀리를 선택하는 경우 광고 단위에 대한 [조정 설정](#mediation)을 선택적으로 구성할 수 있습니다.
    * 배너 단위에 대해 **PC/태블릿(Windows 8.1)** 또는 **모바일(Windows Phone 8.x)** 장치 패밀리를 선택하는 경우 **내 앱에 커뮤니티 광고 표시**를 선택하여 [커뮤니티 광고](about-community-ads.md)에 옵트인(opt in)합니다.

7.  선택한 앱에 대해 COPPA 규정 준수를 아직 설정하지 않은 경우, [COPPA 규정 준수](#coppa) 섹션의 옵션을 선택합니다.
8.  **광고 단위 만들기**를 클릭합니다.

새 광고 단위를 만든 후에는 **수익 창출** &gt; **인앱 광고** 페이지에 사용 가능한 광고 단위 표가 표시됩니다.

<span id="available-ad-units" />

## <a name="review-and-edit-ad-units"></a>광고 단위 검토 및 편집

계정에서 하나 이상의 앱에 대한 광고 단위를 만든 후에는 **수익 창출** &gt; **인앱 광고** 페이지 하단의 표에 해당 광고 단위가 표시됩니다. 이 표에는 각 광고 단위에 대한 **응용 프로그램 ID** 및 **광고 단위 ID**와 기타 정보가 표시됩니다. 앱에 광고를 표시하려면 다음과 같이 코드에 이러한 값을 사용해야 합니다. 자세한 내용은 [앱에서 광고 단위 설정](../monetize/set-up-ad-units-in-your-app.md)을 참조하세요.

* 앱에서 [배너 광고](../monetize/banner-ads.md)를 표시하는 경우 [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 개체의 [ApplicationId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) 및 [AdUnitId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.adunitid) 속성에 이러한 값을 할당합니다.
* 앱에서 [중간 광고](../monetize/interstitial-ads.md)를 표시하는 경우 [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) 개체의 [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) 메서드에 이러한 값을 전달합니다.
* 앱에서 [기본 광고](../monetize/native-ads.md)를 표시 하는 경우 이러한 값을 **NativeAdsManagerV2** 생성자에 전달 합니다.
  > [!IMPORTANT]
  > 앱별로 하나의 광고 단위만 사용할 수 있습니다. 특정 광고 단위를 하나 이상의 앱에 사용하면, 해당 광고 단위에 광고가 제공되지 않습니다.

  > [!NOTE]
  > 단일 앱에서 여러 배너, 중간, 기본 광고 컨트롤을 사용할 수 있습니다. 이 경우, 각 컨트롤에 다른 광고 단위를 지정하는 것이 좋습니다. 각 컨트롤에 다른 광고 단위를 사용하면 별도로 [조정 설정을 구성](../publish/in-app-ads.md#mediation)할 수 있고, 각 컨트롤에 대해 별개의 [보고 데이터](../publish/advertising-performance-report.md)를 가져올 수 있습니다. 이렇게 하면 서비스를 통해 앱에 지원하는 광고를 더 효과적으로 최적화할 수 있습니다.

UWP 광고 단위에 대해 [조정 설정](#mediation) 또는 광고 단위가 사용되는 앱에 대해 [COPPA 규정 준수](#coppa)를 편집하려면 광고 단위 이름을 클릭합니다.

> [!NOTE]
> 광고 단위에 지난 6 개월 활동이 없는 경우 **비활성**지정 하 고 결국에 대시보드에서 제거 하겠습니다. 필터를 사용하여 **활성** 또는 **비활성** 광고 단위만 표시할 수 있습니다. 잘못 **비활성**으로 표시된 광고 단위가 있는 경우 [고객 지원에 문의](http://aka.ms/storesupport)합니다.

<span id="mediation" />

## <a name="mediation-settings"></a>조정 설정

경우 [새 UWP 광고 단위를 생성](#create-ad-unit) 하거나 [기존 UWP 광고 단위를 편집](#available-ad-units)옵션을 사용이 섹션의 광고 단위에 대 한 [광고 조정](../monetize/ad-mediation-service.md) 을 구성 합니다. 광고 조정을 통해 기타 유료 광고 네트워크의 광고와 Microsoft 앱 프로모션 캠페인 비수익 광고 등 여러 광고 네트워크의 광고를 표시하여 광고 수익과 앱 홍보 기능을 최대화할 수 있습니다. Microsoft는 사용자가 선택한 광고 네트워크의 배너 광고 요청을 조정합니다. 앱에서 UWP 광고 단위가 배너, 중간 광고 또는 기본 광고에 이미 연결되어 있는 경우에는 광고 조정을 지원하기 위해 앱에서 코드를 변경할 필요가 없습니다.

> [!NOTE]
> UWP 광고 단위에 대해 광고 조정을 지원하면 타사 광고 네트워크에서 광고 단위를 가져올 필요가 없습니다. Microsoft의 광고 조정 서비스가 자동으로 필요한 타사 광고 단위를 생성합니다.

앱에서 UWP 광고 단위에 대한 광고 조정 설정을 구성하려면:

1. [광고 단위를 만들거나](#create-ad-unit) [기존 광고 단위를 선택](#available-ad-units)합니다.
2. **인 앱 광고** 페이지에서 설정을 구성 및 **조정 설정** 섹션으로 이동 합니다.

    * 기본값으로 **Microsoft가 앱에 가장 좋은 조정 설정을 선택** 확인란이 선택되어 있습니다. 이 옵션을 사용하는 것이 좋습니다. 이 옵션은 기계 학습 알고리즘을 사용, 자동으로 앱이 지원하는 지역/국가에서 광고 수익을 극대화 하는 데 도움이 되는 앱의 광고 조정 설정을 선택합니다. 이 옵션을 사용 하는 경우에 구성에 사용 하려는 광고 네트워크를 선택할 수 있습니다. 구성의 일부로 하지 않으려면 앱 선택한 광고 네트워크에서 광고를 수신 하기만 하는 알고리즘은 보장 하는 광고 네트워크의 선택을 취소 합니다.
    * 고유의 광고 조정 설정을 선택 하려는 경우 **수정 기본 설정**을 선택 합니다.

    > [!NOTE]
    > 이 섹션의 나머지 단계는만 **수정 기본 설정**을 선택 하는 경우 적용 됩니다.

4. **대상** 드롭다운 목록에서 **기본**을 선택해 광고 조정 설정을 기본값으로 구성할 수 있습니다. 이 기본 설정이 지역/국가에 특정적인 구성을 적용한 지역/국가를 제외한 모든 지역/국가에 적용됩니다.
6. 다음은 유료 네트워크(광고 컨트롤을 유료 네트워크(노출에 대한 수익을 지급하는)와 다른 광고 네트워크(노출에 대한 수익을 지급하지 않는)의 컨트롤에 표시할 광고의 비율을 지정합니다. **유료 광고 네트워크** 및 **다른 광고 네트워크**의 **가중치** 필드에 0~100의 값을 입력해야 합니다.  
7. **유료 광고 네트워크** 섹션에서 사용하고 싶은 각 [유료 네트워크](#paid-networks)의 **활성** 열의 확인란을 선택한 후, **순위** 열 옆 화살표를 사용하여 순위 별로 네트워크를 주문합니다(컨트롤이 각 네트워크를 얼마나 많이 사용할지 지정).
8. **배너** 또는 **배너 중간** 광고 단위를 선택하면 **다른 광고 네트워크**라는 섹션이 나타납니다. 이 섹션의 네트워크는 광고 노출에 대한 광고 수익을 획득할 수 없습니다. 대신 이러한 네트워크는 앱 홍보 캠페인 등 소스의 광고를 표시합니다.

    **기타 광고 네트워크** 섹션에서 사용하고 싶은 각 [기타 네트워크](#other-networks)의 **활성** 열 확인란을 선택한 후, 순위 열 옆 화살표를 사용하여 **순위** 별로 네트워크를 주문합니다(컨트롤이 각 네트워크를 얼마나 많이 사용할지 지정). 현재 다음과 같은 기타 네트워크가 지원되고 있습니다.

9. 기본 조정 구성을 재구성하려는 각 지역/국가의 경우, **대상** 드롭다운 목록에서 지역/국가를 선택한 후 광고 네트워크 선택 및 순위를 업데이트합니다.
10. **광고 단위 만들기**(새로운 광고 단위를 만드는 경우) 또는 **저장**(기존 광고 단위를 편집하는 경우)을 클릭합니다.

<span id="paid-networks" />

### <a name="supported-paid-ad-networks"></a>지원되는 유료 광고 네트워크

다음 표에는 각 광고 유형에 대해 현재 지원되는 유료 네트워크가 나와 있습니다. [일부 지역/국가에서 사용할 수 없는](#network-markets) 네트워크도 있습니다.

|  광고 네트워크  |  설명  |  지원되는 광고 유형  |
|--------------|---------------|---------------------|
| Oath 및 AppNexus |  네트워크, Oath, AppNexus 파트너를 통해 광고를 제공 하는 Microsoft에서 관리 하는 광고 네트워크입니다.<p/>**참고**: Oath 및 AppNexus는 항상 순위 먼저 **유료 광고 네트워크** 목록에서 배너 광고 단위에 대 한 및 낮은 이런이 유형의 광고에서는 순위를 변경할 수 없습니다. | 배너, 동영상 중간 광고 |
| AppNexus(직접) | [AppNexus](https://www.appnexus.com)에서 광고를 지원 하려면이 옵션을 선택 합니다. | 동영상 중간 광고, 기본  |
| Microsoft 앱 설치 광고 | Windows 에코시스템에서 [앱에 대해 홍보 광고 캠페인을 생성](create-an-ad-campaign-for-your-app.md)하는 다른 개발자가 생성한 앱 설치 광고 또는 앱 Re-engagement 광고를 지원하려면 이 옵션을 선택합니다.  |  배너, 배너 중간 광고, 기본  |
| MSN 콘텐츠 권장 사항 |  MSN 콘텐츠 권장 사항에서 광고를 지원 하려면이 옵션을 선택 합니다. |  배너, 배너 중간 광고  |
| Outbrain |  [Outbrain](https://www.outbrain.com/)에서 광고를 지원하려면 이 옵션을 선택합니다. |  배너, 배너 중간 광고  |
| Revcontent |  [Revcontent](http://www.revcontent.com/)에서 광고를 지원하려면 이 옵션을 선택합니다. |  기본 배너  |
| Smaato |  [Smaato](https://www.smaato.com/)에서 광고를 지원하려면 이 옵션을 선택합니다. |  배너  |
| smartclip |  [smartclip](http://www.smartclip.com/)에서 광고를 지원하려면 이 옵션을 선택합니다. |  동영상 중간 광고  |
| SpotX |  [SpotX](https://www.spotx.tv/)에서 광고를 지원하려면 이 옵션을 선택합니다. |  동영상 중간 광고  |
| Taboola |  [Taboola](https://www.taboola.com/)에서 광고를 지원하려면 이 옵션을 선택합니다. |  배너  |
| Undertone | [Undertone](https://www.undertone.com/)에서 광고를 지원 하려면이 옵션을 선택 합니다. | 배너 중간 광고 |


<span id="other-networks" />

### <a name="other-ad-networks"></a>기타 광고 네트워크

다음 표에는 각 광고 유형에 대해 현재 지원되는 기타 네트워크가 나와 있습니다.

|  광고 네트워크  |  설명  |  지원되는 광고 유형  |
|--------------|---------------|---------------------|
| Microsoft 커뮤니티 광고 |  [앱 중 하나에 대해 홍보 광고 캠페인을 생성](create-an-ad-campaign-for-your-app.md)하고, 이 캠페인을 [커뮤니티 광고 캠페인](about-community-ads.md)으로 구성할 경우, 이 옵션을 사용하여 이 캠페인의 광고를 표시합니다. | 배너, 배너 중간 광고 |
| Microsoft 하우스 광고 | [앱 중 하나에 대해 홍보 광고 캠페인을 생성](create-an-ad-campaign-for-your-app.md)하고, 이 캠페인을 [하우스 광고 캠페인](about-house-ads.md)으로 구성할 경우, 이 옵션을 사용하여 이 캠페인의 광고를 표시합니다. | 배너, 배너 중간 광고  |


<span id="network-markets" />

### <a name="supported-markets-for-ad-networks"></a>광고 네트워크 지원 지역/국가

사용 가능한 광고 네트워크가 (다음을 제외한) [지원 지역/국가](define-pricing-and-market-selection.md#microsoft-store-consumer-markets)에 광고를 서비스합니다.

|  광고 네트워크  |  지원되는 지역/국가  |
|--------------|---------------------|
| Revcontent | 브라질, 캐나다, 프랑스, 독일, 이탈리아, 일본, 스페인, 영국, 미국  |
| Smaato | 브라질, 캐나다, 프랑스, 독일, 이탈리아, 일본, 스페인, 영국, 미국 |
| smartclip | 오스트리아, 벨기에, 덴마크, 핀란드, 독일, 이탈리아, 네덜란드, 노르웨이, 스웨덴, 스위스  |
| Undertone | 미국 |

<span id="coppa" />

## <a name="coppa-compliance"></a>COPPA 규정 준수

[광고 단위 만들거나](#create-ad-unit) [기존 광고 단위를 선택](#available-ad-units)할 때 광고 단위에 대해 선택된 앱에 앱 인증 과정의 [Store 내](../publish/the-app-certification-process.md#in-the-store) 단계에 도달한 제출이 하나 이상 있는 경우 **COPPA 규정 준수** 섹션이 대시보드 페이지의 맨 아래에 나타납니다.

“COPPA”(아동에 관한 온라인 개인 정보 보호법)를 위해 귀하의 앱이 13세 미만 아동을 대상으로 하는 경우 이 섹션의 **이 응용 프로그램은 13세 미만 아동을 대상으로 합니다**를 선택해야 합니다. 이 옵션을 선택하면 Microsoft는 앱에 광고를 제공할 때 동작 광고 서비스를 사용하지 않도록 설정하는 단계를 진행합니다.

선택한 **COPPA 규정 준수** 설정은 선택한 앱에 대한 모든 광고 단위에 자동으로 적용됩니다.

> [!IMPORTANT]
> 앱이 13세 미만 아동을 대상으로 하는 경우 COPPA에 따른 특정한 의무 조항이 있습니다. 의무 조항에 대한 자세한 내용은 [이 페이지](http://go.microsoft.com/fwlink/p/?linkid=536558)를 참조하세요.
