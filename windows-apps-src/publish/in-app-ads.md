---
Description: 앱이 Microsoft Advertising SDK를 사용 하 여 광고를 표시 하는 경우 파트너 센터의 앱 내 광고 페이지를 사용 하 여 광고 사용을 관리 합니다.
title: 앱 내 광고
ms.assetid: 09970DE3-461A-4E2A-88E3-68F2399BBCC8
ms.date: 03/25/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a1683cdad189b9e369700e25b47a6f0bf0796702
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171027"
---
# <a name="in-app-ads"></a>앱 내 광고

>[!WARNING]
> 2020 년 6 월 1 일부 터 Windows UWP 앱 용 Microsoft Ad 수익 화 플랫폼이 종료 됩니다. [자세한 정보](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

**Monetize** &gt; [파트너 센터](https://partner.microsoft.com/dashboard) 의 수익 창출 ad **앱 광고** 페이지를 사용 하 여 다음에 대 한 ad 단위를 만들고 관리할 수 있습니다.

* [MICROSOFT ADVERTISING SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK)를 사용 하는 UWP (유니버설 Windows 플랫폼) 앱입니다.
* [Windows 용 MICROSOFT ADVERTISING SDK 및 Windows Phone](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDKforWindowsandWindowsPhone8x).x를 사용 하는 이전에 게시 된 windows 8.x 및 Windows Phone .x 앱입니다.

> [!IMPORTANT]
> Windows Phone .x SDK를 사용 하 여 빌드된 새 XAP 패키지는 더 이상 업로드할 수 없습니다. 이미 XAP 패키지를 사용 하 여 저장 된 앱은 Windows 10 Mobile 장치에서 계속 작동 합니다. 자세한 내용은이 [블로그 게시물](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store)을 참조 하세요.

이러한 Sdk를 앱과 통합 하 여 광고를 표시 하는 방법에 대 한 자세한 내용은 [MICROSOFT ADVERTISING SDK를 사용 하 여 앱에 광고 표시](../monetize/display-ads-in-your-app.md)를 참조 하세요.

<span id="create-ad-unit" />

## <a name="create-ad-units"></a>Ad 단위 만들기

앱에서 [배너 광고](../monetize/banner-ads.md), [중간 광고](../monetize/interstitial-ads.md)또는 [기본 광고](../monetize/native-ads.md) 의 ad 단위를 만들려면 다음을 수행 합니다.

1.  파트너 센터에서 **수익 창출** &gt; **앱 광고** 페이지로 이동 하 고 **ad 유닛 만들기**를 클릭 합니다.
2.  **앱 이름** 드롭다운에서 ad 단위가 사용 될 앱을 선택 합니다.
3.  **Ad 단위 이름** 필드에 ad 단위의 이름을 입력 합니다. 이는 보고 목적으로 ad 단위를 식별 하는 데 사용 하려는 설명 문자열 일 수 있습니다.
4.  **Ad 단위 유형** 드롭다운에서 ad 유형을 선택 합니다.

    * 앱에 배너 광고를 표시 하는 경우 **배너**를 선택 합니다.
    * 앱에서 중간 비디오 광고 또는 중간 배너 광고를 표시 하는 경우 **video 중간** 또는 **배너 중간** 을 선택 합니다 (표시 하려는 중간 광고의 유형에 적절 한 옵션을 선택 해야 합니다).
    * 앱에서 네이티브 ad를 표시 하는 경우 **네이티브**를 선택 합니다.

5. **장치 제품군** 드롭다운에서 ad 단위를 사용할 앱의 대상으로 지정 된 장치 패밀리를 선택 합니다. 사용할 수 있는 옵션은 **UWP (Windows 10)**, **PC/태블릿 (Windows 8.1)** 또는 **모바일 (Windows Phone 8gb)** 입니다.

6. 필요에 따라 다음과 같은 추가 설정을 구성 합니다.

    * Ad 장치에 대 한 **UWP (Windows 10)** 장치 패밀리를 선택 하는 경우 필요에 따라 ad 단위에 대 한 [중재 설정을](#mediation) 구성할 수 있습니다.
    * 배너 광고 단위에 대 한 **PC/태블릿 (Windows 8.1)** 또는 **모바일 (Windows Phone .x)** 장치 제품군을 선택 하는 경우 **앱에서 커뮤니티 광고 표시** 를 선택 하 여 [커뮤니티 광고](about-community-ads.md)를 옵트인 할 수 있습니다.

7.  선택한 앱에 대 한 COPPA 준수를 아직 설정 하지 않은 경우 [COPPA 준수](#coppa) 섹션에서 옵션을 선택 합니다.
8.  **Ad 단위 만들기**를 클릭 합니다.

새 ad 단위를 만든 후에는 **수익 창출** &gt; **의 앱 내 광고** 페이지에서 사용 가능한 ad 단위 표에 표시 됩니다.

<span id="available-ad-units" />

## <a name="review-and-edit-ad-units"></a>Ad 단위 검토 및 편집

계정에서 하나 이상의 앱에 대 한 ad 단위를 만든 후 이러한 ad 단위는 **수익 창출** &gt; **앱 내 광고** 페이지의 아래쪽에 있는 테이블에 표시 됩니다. 이 테이블에는 각 ad 단위의 **응용 프로그램 id** 및 **ad 단위 id** 와 기타 정보가 표시 됩니다. 앱에 광고를 표시 하려면 코드에서 이러한 값을 사용 해야 합니다. 자세한 내용은 [앱에서 ad 단위 설정](../monetize/set-up-ad-units-in-your-app.md)을 참조 하세요.

* 앱이 [배너 광고](../monetize/banner-ads.md)를 표시 하는 경우 이러한 값을 [adcontrol](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 개체의 [ApplicationId](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) 및 [ad담당자 id](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.adunitid) 속성에 할당 합니다.
* 앱이 [중간 광고](../monetize/interstitial-ads.md)를 표시 하는 경우 이러한 값을 [Interstitialad](/uwp/api/microsoft.advertising.winrt.ui.interstitialad) 개체의 [requestad](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) 메서드에 전달 합니다.
* 앱이 [네이티브 광고](../monetize/native-ads.md)를 표시 하는 경우 이러한 값을 **NativeAdsManagerV2** 생성자에 전달 합니다.
  > [!IMPORTANT]
  > 하나의 앱 에서만 각 ad 단위를 사용할 수 있습니다. 둘 이상의 앱에서 ad 단위를 사용 하는 경우 해당 ad 단위에 대 한 광고가 제공 되지 않습니다.

  > [!NOTE]
  > 단일 앱에서 여러 배너, 중간 및 네이티브 ad 컨트롤을 사용할 수 있습니다. 이 시나리오에서는 각 컨트롤에 다른 ad 단위를 할당 하는 것이 좋습니다. 각 컨트롤에 대해 서로 다른 ad 단위를 사용 하면 [중재 설정을](../publish/in-app-ads.md#mediation) 개별적으로 구성 하 고 각 컨트롤에 대 한 개별 [보고 데이터](../publish/advertising-performance-report.md) 를 가져올 수 있습니다. 이를 통해 서비스는 앱에 제공 하는 광고를 더 효율적으로 최적화할 수 있습니다.

UWP ad 단위에 대 한 [중재 설정](#mediation) 또는 ad 단위가 사용 되는 앱에 대 한 [COPPA 준수](#coppa) 를 편집 하려면 ad 단위 이름을 클릭 합니다.

> [!NOTE]
> Ad 단위가 지난 6 개월 동안 활동이 없는 경우 **비활성**으로 레이블을 만들고 결국 파트너 센터에서이를 제거 합니다. 필터를 사용 하 여 **활성** 또는 **비활성** ad 장치만 표시할 수 있습니다. **비활성**으로 표시 된 것으로 예상 되는 ad 단위가 표시 되 면 [지원 담당자에 게 문의 하세요](https://developer.microsoft.com/windows/support).

<span id="mediation" />

## <a name="mediation-settings"></a>중재 설정

[새 uwp ad 단위를 만들거나](#create-ad-unit) [기존 uwp ad](#available-ad-units)단위를 편집 하는 경우이 섹션의 옵션을 사용 하 여 ad 장치에 대 한 [ad 중재](../monetize/ad-mediation-service.md) 를 구성 합니다. Ad 중재를 사용 하면 다른 유료 ad 네트워크의 광고 및 Microsoft 앱 홍보 행사에 대 한 비 수익 생성 광고를 비롯 하 여 여러 ad 네트워크의 광고를 표시 하 여 ad 수익과 앱 승격 기능을 최대화할 수 있습니다. 선택한 ad 네트워크에서 mediating 배너 광고 요청을 처리 합니다. 앱에서 배너, 중간 또는 기본 광고와 이미 연결 된 UWP ad 단위가 있는 경우 ad 중재를 사용 하도록 설정 하려면 앱에서 코드를 변경 하지 않아도 됩니다.

> [!NOTE]
> UWP ad 유닛에 대해 ad 중재를 사용 하도록 설정 하면 타사 ad 네트워크에서 ad 단위를 가져올 필요가 없습니다. Ad 중재 서비스는 필요한 타사 ad 단위를 자동으로 만듭니다.

앱에서 UWP ad 장치에 대 한 ad 중재 설정을 구성 하려면:

1. [Ad 단위를 만들거나](#create-ad-unit) [기존 ad 단위를 선택](#available-ad-units)합니다.
2. **앱 내 광고** 페이지에서 **중재 설정** 섹션으로 이동 하 여 설정을 구성 합니다.

    * 기본적으로 Microsoft에서 **설정 최적화** 확인란을 선택 합니다. 이 옵션을 사용 하는 것이 좋습니다. 이 옵션은 기계 학습 알고리즘을 사용 하 여 앱이 지 원하는 시장에서 광고 수익을 극대화 하는 데 도움이 되도록 앱에 대 한 ad 중재 설정을 자동으로 선택 합니다. 이 옵션을 사용 하는 경우 구성에서 사용 하려는 ad 네트워크를 선택할 수도 있습니다. 구성에 포함 하지 않을 ad 네트워크를 선택 취소 하면 앱이 선택한 ad 네트워크에서 광고를 받도록 할 수 있습니다.
    * 고유한 광고 중재 설정을 선택 하려면 **기본 설정 수정**을 선택 합니다.

    > [!NOTE]
    > 이 섹션의 나머지 단계는 **기본 설정 수정**을 선택 하는 경우에만 적용 됩니다.

3. **대상** 드롭다운에서 ad 중재 설정에 대 한 기본 구성을 구성 하려면 **기준** 을 선택 합니다. 이 기본 구성은 시장의 특정 구성을 정의 하는 시장을 제외 하 고 모든 시장에 적용 됩니다.
4. 다음으로 유료 네트워크에서 컨트롤에 표시 하려는 광고의 비율을 지정 합니다 (상대에 대 한 수익을 지불 하는) 및 기타 ad 네트워크 (상대에 대 한 수익을 지불 하지 않음)를 지정 합니다. 이렇게 하려면 **유료 ad 네트워크** 및 **기타 ad 네트워크**에 대 한 **가중치** 필드에 0에서 100 사이의 값을 입력 합니다.  
5. **유료 ad 네트워크** 섹션에서 사용 하려는 각 [유료 네트워크](#paid-networks) 에 대 한 **활성** 열의 확인란을 선택 하 고 **순위** 열의 화살표를 사용 하 여 순위별로 네트워크 순서를 지정 합니다 .이는 컨트롤에서 각 네트워크를 사용 해야 하는 빈도를 지정 합니다.
6. **배너** 또는 **배너 중간** 광고 단위를 선택한 경우 **다른 ad 네트워크**라는 섹션도 표시 됩니다. 이 섹션의 네트워크에서는 광고 광고를 받을 수 없습니다. 대신 이러한 네트워크는 앱 판촉 캠페인과 같은 원본의 광고를 표시 합니다.

    **다른 ad 네트워크** 섹션에서 사용 하려는 각 [네트워크](#other-networks) 에 대해 **활성** 열의 확인란을 선택 하 고 **순위** 열의 화살표를 사용 하 여 순위 별로 네트워크 순서를 지정 합니다 .이는 컨트롤에서 각 네트워크를 사용 해야 하는 빈도를 지정 합니다. 현재 지원 되는 기타 네트워크는 다음과 같습니다.

7. 기본 중재 구성을 재정의 하려는 각 시장에 대해 **대상** 드롭다운에서 시장을 선택 하 고 ad 네트워크 선택 및 순위를 업데이트 합니다.
8. Ad 단위 **만들기** (새 광고 단위를 만드는 경우)를 클릭 하거나 기존 광고 단위를 편집 하는 경우 **저장** 을 클릭 합니다.

<span id="paid-networks" />

### <a name="supported-paid-ad-networks"></a>지원 되는 유료 ad 네트워크

다음 표에서는 현재 각 ad 형식에 대해 지원 되는 유료 네트워크를 보여 줍니다. 일부 지역에서는 이러한 네트워크 중 일부를 [사용할 수 없습니다](#network-markets).

|  Ad 네트워크  |  Description  |  지원 되는 ad 유형  |
|--------------|---------------|---------------------|
| Oath 및 AppNexus |  파트너 네트워크, Oath 및 AppNexus를 통해 광고를 제공 하는 Microsoft 관리 ad 네트워크입니다.<p/>**참고**: Oath 및 appnexus는 항상 배너 광고 단위에 대 한 **유료 ad 네트워크** 목록에서 순위가 지정 되며, 이러한 유형의 광고에 대해 더 낮은 순위를 변경할 수 없습니다. | 배너, 비디오 중간 |
| AppNexus (직접) | [Appnexus](https://www.appnexus.com)에서 광고를 제공 하려면이 옵션을 선택 합니다. | 비디오 중간, 기본  |
| Microsoft 앱 설치 광고 | 앱 [에 대 한 프로 모션 광고 캠페인을 만드는](create-an-ad-campaign-for-your-app.md)Windows 에코 시스템의 다른 개발자가 만든 앱 설치 광고 또는 앱 재 engagement 광고를 제공 하려면이 옵션을 선택 합니다.  |  배너, 배너 중간, 네이티브  |
| MSN 콘텐츠 권장 사항 |  MSN 콘텐츠 권장 사항의 광고를 제공 하려면이 옵션을 선택 합니다. |  배너, 배너 중간  |
| Outbrain |  [Outbrain](https://www.outbrain.com/)의 광고를 제공 하려면이 옵션을 선택 합니다. |  배너, 배너 중간  |
| Revcontent |  [Revcontent](https://www.revcontent.com/)에서 광고를 제공 하려면이 옵션을 선택 합니다. |  배너, 네이티브  |
| Smaato |  [Smaato](https://www.smaato.com/)에서 광고를 제공 하려면이 옵션을 선택 합니다. |  배너  |
| smartclip |  [Smartclip](http://www.smartclip.com/)의 광고를 제공 하려면이 옵션을 선택 합니다. |  비디오 중간  |
| SpotX |  [SpotX](https://www.spotx.tv/)에서 광고를 제공 하려면이 옵션을 선택 합니다. |  비디오 중간  |
| Taboola |  [Taboola](https://www.taboola.com/)에서 광고를 제공 하려면이 옵션을 선택 합니다. |  배너  |
| Vungle | [Vungle](https://vungle.com/) 에서 광고를 제공 하려면이 옵션을 선택 합니다. | 비디오 중간 |
| 저 음 | 광고 [의 광고를 제공](https://www.undertone.com/)하려면이 옵션을 선택 합니다. | 배너 중간 |


<span id="other-networks" />

### <a name="other-ad-networks"></a>기타 ad 네트워크

다음 표에서는 현재 각 ad 형식에 대해 지원 되는 다른 네트워크를 보여 줍니다.

|  Ad 네트워크  |  Description  |  지원 되는 ad 유형  |
|--------------|---------------|---------------------|
| Microsoft 커뮤니티 광고 |  앱 중 [하나에 대 한 판촉 광고 캠페인을 만들고](create-an-ad-campaign-for-your-app.md) 이 캠페인을 [커뮤니티 ad 캠페인](about-community-ads.md)으로 구성 하는 경우이 캠페인의 광고를 표시 하려면이 옵션을 선택 합니다. | 배너, 배너 중간 |
| Microsoft 사내 광고 | [앱 중 하나에 대 한 판촉 광고 캠페인을 만들고](create-an-ad-campaign-for-your-app.md) 이 캠페인을 [집 광고 캠페인](about-house-ads.md)으로 구성 하는 경우이 옵션을 선택 하 여이 캠페인의 광고를 표시 합니다. | 배너, 배너 중간  |


<span id="network-markets" />

### <a name="supported-markets-for-ad-networks"></a>Ad 네트워크에 대해 지원 되는 시장

사용 가능한 ad 네트워크는 다음과 같은 예외를 제외 하 고 [지원 되](define-market-selection.md#microsoft-store-consumer-markets)는 모든 시장의 광고를 제공 합니다.

|  Ad 네트워크  |  지원 되는 시장  |
|--------------|---------------------|
| Revcontent | 브라질, 캐나다, 프랑스, 독일, 이탈리아, 일본, 스페인, 영국, 미국  |
| Smaato | 브라질, 캐나다, 프랑스, 독일, 이탈리아, 일본, 스페인, 영국, 미국 |
| smartclip | 오스트리아, 벨기에, 덴마크, 핀란드, 독일, 이탈리아, 네덜란드, 노르웨이, 스웨덴, 스위스  |
| 저 음 | 미국 |

<span id="coppa" />

## <a name="coppa-compliance"></a>COPPA 규정 준수

Ad 단위를 [만들거나](#create-ad-unit) [기존 ad 단위를 선택](#available-ad-units)하면 ad 단위의 선택 된 앱에 앱 인증 프로세스의 저장소 단계 [에서](../publish/the-app-certification-process.md#in-the-store) 에 도달한 제출이 하나 이상 있는 경우 **COPPA 준수** 섹션이 페이지 아래쪽에 표시 됩니다.

어린이의 온라인 개인 정보 보호 기능 ("COPPA")을 위해 앱이 13 세 미만의 자식으로 전달 되는 경우이 섹션에서 **13의 기간에 해당 하는 자식에이 응용 프로그램을 전달** 하도록 선택 해야 합니다. 이 옵션을 선택 하는 경우 Microsoft는 응용 프로그램에 광고를 제공할 때 동작 광고 서비스를 사용 하지 않도록 설정 하는 단계를 수행 합니다.

선택한 앱의 모든 ad 단위에 선택한 **COPPA 준수** 설정이 자동으로 적용 됩니다.

> [!IMPORTANT]
> 앱이 13 세 미만의 자식으로 전달 되는 경우 COPPA 아래에 특정 의무가 있습니다. 의무에 대 한 자세한 내용은 [이 페이지](https://www.ftc.gov/enforcement/rules/rulemaking-regulatory-reform-proceedings/childrens-online-privacy-protection-rule)를 참조 하세요.