---
Description: 앱이 Microsoft Advertising SDK를 사용 하 여 광고를 표시 하는 경우 파트너 센터의 앱 내 광고 페이지를 사용 하 여 광고 사용을 관리 합니다.
title: 인앱 광고
ms.assetid: 09970DE3-461A-4E2A-88E3-68F2399BBCC8
ms.date: 03/25/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e12641695dd72cddcfb6b51f6cd2f20fa66ddf41
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258996"
---
# <a name="in-app-ads"></a>인앱 광고

[파트너 센터](https://partner.microsoft.com/dashboard) 의 **수익 창출** &gt; **앱 내 광고** 페이지를 사용 하 여 다음에 대 한 ad 단위를 만들고 관리할 수 있습니다.

* [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK)를 사용하는 UWP(유니버설 Windows 플랫폼) 앱.
* [Windows 용 MICROSOFT ADVERTISING SDK 및 Windows Phone](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDKforWindowsandWindowsPhone8x).x를 사용 하는 이전에 게시 된 windows 8.x 및 Windows Phone .x 앱입니다.

> [!IMPORTANT]
> 2018 년 10 월 31 일까 지 새로 만든 제품은 Windows 8.x/Windows Phone 8.x이 하 버전을 대상으로 하는 패키지를 포함할 수 없습니다. 자세한 내용은이 [블로그 게시물](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store)을 참조 하세요.

이러한 SDK를 앱과 통합하여 광고를 표시하는 방법에 대한 자세한 내용은 [Microsoft Advertising SDK를 사용하여 앱에 광고 표시](../monetize/display-ads-in-your-app.md)를 참조하세요.

<span id="create-ad-unit" />

## <a name="create-ad-units"></a>광고 단위 생성

앱에서 [배너](../monetize/banner-ads.md), [중간 광고](../monetize/interstitial-ads.md) 또는 [기본 광고](../monetize/native-ads.md)에 대한 광고 단위를 생성하려면:

1.  파트너 센터에서 **수익 창출** &gt; **In-app ads** 페이지로 이동 하 고 **ad 유닛 만들기**를 클릭 합니다.
2.  **앱 이름** 드롭다운 목록에서 광고 단위가 사용될 앱을 선택합니다.
3.  **광고 단위 이름** 필드에 광고 단위의 이름을 입력합니다. 보고용으로 광고 단위를 식별할 때 사용할 수 있는 설명 문자열입니다.
4.  **광고 단위 유형** 드롭다운 목록에서 광고 유형을 선택합니다.

    * 앱에 배너 광고를 표시 하는 경우 **배너**를 선택 합니다.
    * 앱에서 중간 비디오 광고 또는 중간 배너 광고를 표시 하는 경우 **video 중간** 또는 **배너 중간** 을 선택 합니다 (표시 하려는 중간 광고의 유형에 적절 한 옵션을 선택 해야 합니다).
    * 앱에서 네이티브 ad를 표시 하는 경우 **네이티브**를 선택 합니다.

5. **장치 패밀리** 드롭다운 목록에서 광고 단위가 사용될 앱의 대상이 되는 장치 패밀리를 선택합니다. 사용 가능한 옵션은 **UWP(Windows 10)** , **PC/태블릿(Windows 8.1)** 또는 **모바일(Windows Phone 8.x)** 입니다.

6. 필요에 따라 다음과 같은 추가 설정을 구성합니다.

    * 광고 단위에 대해 **UWP(Windows 10)** 장치 패밀리를 선택하는 경우 광고 단위에 대한 [조정 설정](#mediation)을 선택적으로 구성할 수 있습니다.
    * 배너 단위에 대해 **PC/태블릿(Windows 8.1)** 또는 **모바일(Windows Phone 8.x)** 장치 패밀리를 선택하는 경우 **내 앱에 커뮤니티 광고 표시**를 선택하여 [커뮤니티 광고](about-community-ads.md)에 옵트인(opt in)합니다.

7.  선택한 앱에 대해 COPPA 규정 준수를 아직 설정하지 않은 경우, [COPPA 규정 준수](#coppa) 섹션의 옵션을 선택합니다.
8.  **광고 단위 만들기**를 클릭합니다.

새 ad 단위를 만든 후에는 **수익 창출** &gt; **앱 내 광고** 페이지에서 사용 가능한 ad 단위 표에 표시 됩니다.

<span id="available-ad-units" />

## <a name="review-and-edit-ad-units"></a>광고 단위 검토 및 편집

계정에 하나 이상의 앱에 대 한 ad 단위를 만든 후 이러한 ad 단위는 **수익 창출** &gt; **앱 내 광고** 페이지의 아래쪽에 있는 테이블에 표시 됩니다. 이 표에는 각 광고 단위에 대한 **응용 프로그램 ID** 및 **광고 단위 ID**와 기타 정보가 표시됩니다. 앱에 광고를 표시하려면 다음과 같이 코드에 이러한 값을 사용해야 합니다. 자세한 내용은 [앱에서 광고 단위 설정](../monetize/set-up-ad-units-in-your-app.md)을 참조하세요.

* 앱에서 [배너 광고](../monetize/banner-ads.md)를 표시하는 경우 [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) 개체의 [ApplicationId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.adunitid) 및 [AdUnitId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 속성에 이러한 값을 할당합니다.
* 앱에서 [중간 광고](../monetize/interstitial-ads.md)를 표시하는 경우 [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) 개체의 [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) 메서드에 이러한 값을 전달합니다.
* 앱이 [네이티브 광고](../monetize/native-ads.md)를 표시 하는 경우 이러한 값을 **NativeAdsManagerV2** 생성자에 전달 합니다.
  > [!IMPORTANT]
  > 앱별로 하나의 광고 단위만 사용할 수 있습니다. 특정 광고 단위를 하나 이상의 앱에 사용하면, 해당 광고 단위에 광고가 제공되지 않습니다.

  > [!NOTE]
  > 단일 앱에서 여러 배너, 중간, 기본 광고 컨트롤을 사용할 수 있습니다. 이 경우, 각 컨트롤에 다른 광고 단위를 지정하는 것이 좋습니다. 각 컨트롤에 다른 광고 단위를 사용하면 별도로 [조정 설정을 구성](../publish/in-app-ads.md#mediation)할 수 있고, 각 컨트롤에 대해 별개의 [보고 데이터](../publish/advertising-performance-report.md)를 가져올 수 있습니다. 이렇게 하면 서비스를 통해 앱에 지원하는 광고를 더 효과적으로 최적화할 수 있습니다.

UWP 광고 단위에 대해 [조정 설정](#mediation) 또는 광고 단위가 사용되는 앱에 대해 [COPPA 규정 준수](#coppa)를 편집하려면 광고 단위 이름을 클릭합니다.

> [!NOTE]
> Ad 단위가 지난 6 개월 동안 활동이 없는 경우 **비활성**으로 레이블을 만들고 결국 파트너 센터에서이를 제거 합니다. 필터를 사용하여 **활성** 또는 **비활성** 광고 단위만 표시할 수 있습니다. 잘못 **비활성**으로 표시된 광고 단위가 있는 경우 [고객 지원에 문의](https://developer.microsoft.com/windows/support)합니다.

<span id="mediation" />

## <a name="mediation-settings"></a>조정 설정

[새 uwp ad 단위를 만들거나](#create-ad-unit) [기존 uwp ad](#available-ad-units)단위를 편집 하는 경우이 섹션의 옵션을 사용 하 여 ad 장치에 대 한 [ad 중재](../monetize/ad-mediation-service.md) 를 구성 합니다. 광고 조정을 통해 기타 유료 광고 네트워크의 광고와 Microsoft 앱 프로모션 캠페인 비수익 광고 등 여러 광고 네트워크의 광고를 표시하여 광고 수익과 앱 홍보 기능을 최대화할 수 있습니다. Microsoft는 사용자가 선택한 광고 네트워크의 배너 광고 요청을 조정합니다. 앱에서 UWP 광고 단위가 배너, 중간 광고 또는 기본 광고에 이미 연결되어 있는 경우에는 광고 조정을 지원하기 위해 앱에서 코드를 변경할 필요가 없습니다.

> [!NOTE]
> UWP 광고 단위에 대해 광고 조정을 지원하면 타사 광고 네트워크에서 광고 단위를 가져올 필요가 없습니다. Microsoft의 광고 조정 서비스가 자동으로 필요한 타사 광고 단위를 생성합니다.

앱에서 UWP 광고 단위에 대한 광고 조정 설정을 구성하려면:

1. [광고 단위를 만들거나](#create-ad-unit)[기존 광고 단위를 선택](#available-ad-units)합니다.
2. **앱 내 광고** 페이지에서 **중재 설정** 섹션으로 이동 하 여 설정을 구성 합니다.

    * 기본적으로 Microsoft에서 **설정 최적화** 확인란을 선택 합니다. 이 옵션을 사용하는 것이 좋습니다. 이 옵션은 기계 학습 알고리즘을 사용, 자동으로 앱이 지원하는 지역/국가에서 광고 수익을 극대화 하는 데 도움이 되는 앱의 광고 조정 설정을 선택합니다. 이 옵션을 사용 하는 경우 구성에서 사용 하려는 ad 네트워크를 선택할 수도 있습니다. 구성에 포함 하지 않을 ad 네트워크를 선택 취소 하면 앱이 선택한 ad 네트워크에서 광고를 받도록 할 수 있습니다.
    * 고유한 광고 중재 설정을 선택 하려면 **기본 설정 수정**을 선택 합니다.

    > [!NOTE]
    > 이 섹션의 나머지 단계는 **기본 설정 수정**을 선택 하는 경우에만 적용 됩니다.

3. **대상** 드롭다운 목록에서 **기본**을 선택해 광고 조정 설정을 기본값으로 구성할 수 있습니다. 이 기본 설정이 지역/국가에 특정적인 구성을 적용한 지역/국가를 제외한 모든 지역/국가에 적용됩니다.
4. 다음은 유료 네트워크(광고 컨트롤을 유료 네트워크(노출에 대한 수익을 지급하는)와 다른 광고 네트워크(노출에 대한 수익을 지급하지 않는)의 컨트롤에 표시할 광고의 비율을 지정합니다. **유료 광고 네트워크** 및 **다른 광고 네트워크**의 **가중치** 필드에 0~100의 값을 입력해야 합니다.  
5. **유료 광고 네트워크** 섹션에서 사용하고 싶은 각 **유료 네트워크**의 [활성](#paid-networks) 열의 확인란을 선택한 후, **순위** 열 옆 화살표를 사용하여 순위 별로 네트워크를 주문합니다(컨트롤이 각 네트워크를 얼마나 많이 사용할지 지정).
6. **배너** 또는 **배너 중간** 광고 단위를 선택하면 **다른 광고 네트워크**라는 섹션이 나타납니다. 이 섹션의 네트워크는 광고 노출에 대한 광고 수익을 획득할 수 없습니다. 대신 이러한 네트워크는 앱 홍보 캠페인 등 소스의 광고를 표시합니다.

    **기타 광고 네트워크** 섹션에서 사용하고 싶은 각 **기타 네트워크**의 [활성](#other-networks) 열 확인란을 선택한 후, 순위 열 옆 화살표를 사용하여 **순위** 별로 네트워크를 주문합니다(컨트롤이 각 네트워크를 얼마나 많이 사용할지 지정). 현재 다음과 같은 기타 네트워크가 지원되고 있습니다.

7. 기본 조정 구성을 재구성하려는 각 지역/국가의 경우, **대상** 드롭다운 목록에서 지역/국가를 선택한 후 광고 네트워크 선택 및 순위를 업데이트합니다.
8. **광고 단위 만들기**(새로운 광고 단위를 만드는 경우) 또는 **저장**(기존 광고 단위를 편집하는 경우)을 클릭합니다.

<span id="paid-networks" />

### <a name="supported-paid-ad-networks"></a>지원되는 유료 광고 네트워크

다음 표에는 각 광고 유형에 대해 현재 지원되는 유료 네트워크가 나와 있습니다. [일부 지역/국가에서 사용할 수 없는](#network-markets) 네트워크도 있습니다.

|  광고 네트워크  |  설명  |  지원되는 광고 유형  |
|--------------|---------------|---------------------|
| Oath 및 AppNexus |  파트너 네트워크, Oath 및 AppNexus를 통해 광고를 제공 하는 Microsoft 관리 ad 네트워크입니다.<p/>**참고**: Oath 및 appnexus는 항상 배너 광고 단위에 대 한 **유료 ad 네트워크** 목록에서 순위가 지정 되며, 이러한 유형의 광고에 대해 더 낮은 순위를 변경할 수 없습니다. | 배너, 동영상 중간 광고 |
| AppNexus(직접) | [Appnexus](https://www.appnexus.com)에서 광고를 제공 하려면이 옵션을 선택 합니다. | 동영상 중간 광고, 기본  |
| Microsoft 앱 설치 광고 | Windows 에코시스템에서 [앱에 대해 홍보 광고 캠페인을 생성](create-an-ad-campaign-for-your-app.md)하는 다른 개발자가 생성한 앱 설치 광고 또는 앱 Re-engagement 광고를 지원하려면 이 옵션을 선택합니다.  |  배너, 배너 중간 광고, 기본  |
| MSN 콘텐츠 권장 사항 |  MSN 콘텐츠 권장 사항의 광고를 제공 하려면이 옵션을 선택 합니다. |  배너, 배너 중간 광고  |
| Outbrain |  [Outbrain](https://www.outbrain.com/)에서 광고를 지원하려면 이 옵션을 선택합니다. |  배너, 배너 중간 광고  |
| Revcontent |  [Revcontent](https://www.revcontent.com/)에서 광고를 지원하려면 이 옵션을 선택합니다. |  기본 배너  |
| Smaato |  [Smaato](https://www.smaato.com/)에서 광고를 지원하려면 이 옵션을 선택합니다. |  배너  |
| smartclip |  [smartclip](http://www.smartclip.com/)에서 광고를 지원하려면 이 옵션을 선택합니다. |  동영상 중간 광고  |
| SpotX |  [SpotX](https://www.spotx.tv/)에서 광고를 지원하려면 이 옵션을 선택합니다. |  동영상 중간 광고  |
| Taboola |  [Taboola](https://www.taboola.com/)에서 광고를 지원하려면 이 옵션을 선택합니다. |  배너  |
| Vungle | [Vungle](https://vungle.com/) 에서 광고를 제공 하려면이 옵션을 선택 합니다. | 동영상 중간 광고 |
| 저 음 | 광고 [의 광고를 제공](https://www.undertone.com/)하려면이 옵션을 선택 합니다. | 배너 중간 |


<span id="other-networks" />

### <a name="other-ad-networks"></a>기타 광고 네트워크

다음 표에는 각 광고 유형에 대해 현재 지원되는 기타 네트워크가 나와 있습니다.

|  광고 네트워크  |  설명  |  지원되는 광고 유형  |
|--------------|---------------|---------------------|
| Microsoft 커뮤니티 광고 |  [앱 중 하나에 대해 홍보 광고 캠페인을 생성](create-an-ad-campaign-for-your-app.md)하고, 이 캠페인을 [커뮤니티 광고 캠페인](about-community-ads.md)으로 구성할 경우, 이 옵션을 사용하여 이 캠페인의 광고를 표시합니다. | 배너, 배너 중간 광고 |
| Microsoft 하우스 광고 | [앱 중 하나에 대해 홍보 광고 캠페인을 생성](create-an-ad-campaign-for-your-app.md)하고, 이 캠페인을 [하우스 광고 캠페인](about-house-ads.md)으로 구성할 경우, 이 옵션을 사용하여 이 캠페인의 광고를 표시합니다. | 배너, 배너 중간 광고  |


<span id="network-markets" />

### <a name="supported-markets-for-ad-networks"></a>광고 네트워크 지원 지역/국가

사용 가능한 광고 네트워크가 (다음을 제외한) [지원 지역/국가](define-market-selection.md#microsoft-store-consumer-markets)에 광고를 서비스합니다.

|  광고 네트워크  |  지원되는 지역/국가  |
|--------------|---------------------|
| Revcontent | 브라질, 캐나다, 프랑스, 독일, 이탈리아, 일본, 스페인, 영국, 미국  |
| Smaato | 브라질, 캐나다, 프랑스, 독일, 이탈리아, 일본, 스페인, 영국, 미국 |
| smartclip | 오스트리아, 벨기에, 덴마크, 핀란드, 독일, 이탈리아, 네덜란드, 노르웨이, 스웨덴, 스위스  |
| 저 음 | 미국 |

<span id="coppa" />

## <a name="coppa-compliance"></a>COPPA 규정 준수

Ad 단위를 [만들거나](#create-ad-unit) [기존 ad 단위를 선택](#available-ad-units)하면 ad 단위의 선택 된 앱에 앱 인증 프로세스의 저장소 단계 [에서](../publish/the-app-certification-process.md#in-the-store) 에 도달한 제출이 하나 이상 있는 경우 **COPPA 준수** 섹션이 페이지 아래쪽에 표시 됩니다.

“COPPA”(아동에 관한 온라인 개인 정보 보호법)를 위해 귀하의 앱이 13세 미만 아동을 대상으로 하는 경우 이 섹션의 **이 응용 프로그램은 13세 미만 아동을 대상으로 합니다**를 선택해야 합니다. 이 옵션을 선택하면 Microsoft는 앱에 광고를 제공할 때 동작 광고 서비스를 사용하지 않도록 설정하는 단계를 진행합니다.

선택한 **COPPA 규정 준수** 설정은 선택한 앱에 대한 모든 광고 단위에 자동으로 적용됩니다.

> [!IMPORTANT]
> 앱이 13세 미만 아동을 대상으로 하는 경우 COPPA에 따른 특정한 의무 조항이 있습니다. 의무 조항에 대한 자세한 내용은 [이 페이지](https://www.ftc.gov/enforcement/rules/rulemaking-regulatory-reform-proceedings/childrens-online-privacy-protection-rule)를 참조하세요.
