---
Description: 앱에서 광고 조정을 사용하거나 Microsoft Advertising의 배너 광고 또는 동영상 중간 광고를 표시하는 경우 수익 창출 &gt; 광고를 통한 수익 창출 페이지를 사용하여 광고 사용을 관리할 수 있습니다.
title: 광고를 통한 수익 창출
ms.assetid: 09970DE3-461A-4E2A-88E3-68F2399BBCC8
---

# 광고를 통한 수익 창출


앱에서 **AdMediatorControl**, **AdControl** 또는 **InterstitialAd** 컨트롤을 사용하여 배너 또는 동영상 중간 광고를 표시하는 경우 **수익 창출** &gt; **광고로 수익 창출** 페이지를 사용하여 광고 사용을 관리합니다.

## Windows 광고 조정


앱에서 광고 조정을 사용하는 경우 이 섹션을 사용하여 조정 설정을 구성하고 앱에서 사용한 각각의 광고 네트워크에 대한 필수 매개 변수를 추가할 수 있습니다. 이 섹션의 옵션에 관한 자세한 내용은 [앱 제출 및 광고 조정 구성](https://msdn.microsoft.com/library/windows/apps/mt219689)을 참조하세요.

광고 조정을 통해 여러 광고 네트워크의 배너 광고 요청을 조정하여 앱에서 바로 광고 수익을 최적화할 수 있습니다. 광고 조정에 대한 자세한 내용은 [광고 조정을 사용하여 수익 최대화](https://msdn.microsoft.com/library/windows/apps/mt219691)를 참조하세요.

## COPPA 준수

“COPPA”(아동에 관한 온라인 개인 정보 보호법)를 위해 13세 미만 아동을 대상으로 하는 앱인 경우 Microsoft에 이를 알려야 합니다. 개발자 센터를 사용하여 앱이 13세 미만 아동을 대상으로 한다는 것을 Microsoft에 알리는 경우 Microsoft에서 앱에 광고를 제공할 때 동작 광고 서비스를 사용하지 않도록 설정하는 단계를 진행합니다. 앱이 13세 미만 아동을 대상으로 하는 경우 COPPA에 따른 특정한 의무 조항이 있습니다.

COPPA에 따른 의무 조항에 대한 자세한 내용은 [이 페이지](http://go.microsoft.com/fwlink/p/?linkid=536558)를 참조하세요.

## Microsoft 계열사 광고

앱에서 Microsoft 계열사 광고를 표시하려는 경우 이 섹션에 있는 확인란을 선택합니다. 이 확인란을 선택하면 다른 광고 네트워크의 광고를 사용할 수 없는 경우에도 음악, 게임, 영화, 앱, 하드웨어 및 소프트웨어를 비롯한 스토어의 제품 광고가 앱에 제공됩니다. 사용자가 지정된 기여 기간 내에서 스토어의 광고 및 버스 제품을 클릭하면 승인된 구매에 대해 수수료를 받게 됩니다.

이 선택을 변경하는 경우 변경 내용을 적용하기 위해 앱을 다시 게시할 필요가 없습니다. Microsoft 계열사 광고에 대한 자세한 내용은 [계열사 광고 정보](about-affiliate-ads.md)를 참조하세요.

> **참고** 앱에서 광고 조정을 사용하는 경우(즉, **AdMediatorControl**을 사용하여 광고 표시) 앱은 광고 조정 설정이 Microsoft의 광고를 표시하도록 구성된 경우에만 계열사 광고를 표시할 수 있습니다.

## 커뮤니티 광고

사용자 앱과 다른 개발자의 앱을 교차 홍보하려는 경우 이 섹션에 있는 확인란을 선택합니다. 이 확인란을 선택한 다음 [커뮤니티 광고 캠페인을 만들면](create-an-ad-campaign-for-your-app.md) 역시 커뮤니티 광고 캠페인을 만드는 다른 개발자가 게시한 앱에 대한 광고가 사용자 앱에 표시됩니다. 커뮤니티 광고는 무료이며, 다른 광고 네트워크의 광고를 사용할 수 없는 경우에만 표시됩니다.

이 선택을 변경하는 경우 변경 내용을 적용하기 위해 앱을 다시 게시할 필요가 없습니다. 커뮤니티 광고에 대한 자세한 내용은 [커뮤니티 광고 정보](about-community-ads.md)를 참조하세요.

## Microsoft Advertising 광고 단위

이 섹션을 사용하여 Microsoft Advertising 광고 단위를 만들 수 있습니다. 다음 시나리오에서만 광고 단위를 만들어야 합니다.

-   앱에서 [AdControl](https://msdn.microsoft.com/library/mt313154.aspx) 개체를 사용하여 Microsoft Advertising의 배너 광고를 표시합니다.
-   앱에서 [InterstitialAd](https://msdn.microsoft.com/library/mt313189.aspx) 개체를 사용하여 Microsoft Advertising의 동영상 중간 광고를 표시합니다.

이러한 시나리오를 위한 광고 단위를 만들려면 다음을 수행합니다.

1.  광고 단위의 이름을 지정합니다.
2.  광고 단위 유형(**배너 광고** 또는 **동영상 중간 광고**)을 선택합니다.
3.  디바이스 유형(**모바일** 또는 **PC/태블릿**)을 선택합니다.
4.  **광고 단위 만들기**를 클릭합니다.

광고 단위가 이 섹션의 맨 아래에 있는 표에 표시됩니다. 각 광고 단위에 대해 **응용 프로그램 ID** 및 **광고 단위 ID**가 표시됩니다. 앱에 광고를 표시하려면 다음과 같이 코드에 이러한 값을 사용해야 합니다.

-   앱에서 배너 광고를 표시하는 경우 [AdControl](https://msdn.microsoft.com/library/mt313154.aspx) 개체의 [ApplicationId](https://msdn.microsoft.com/library/mt313174.aspx) 및 [AdUnitId](https://msdn.microsoft.com/library/mt313171.aspx) 속성에 이러한 값을 할당합니다.
-   앱에서 동영상 중간 광고를 표시하는 경우 [InterstitialAd](https://msdn.microsoft.com/library/mt313189.aspx) 개체의 [RequestAd](https://msdn.microsoft.com/library/mt313192.aspx) 메서드에 이러한 값을 전달합니다.

> **참고** 앱에서 광고 조정을 사용하여 Microsoft Advertising의 배너 광고를 표시하는 경우(즉, 앱에서 **AdMediatorControl** 개체를 사용하는 경우) 광고 단위를 요청하지 않아도 됩니다. 이 시나리오에서는 Microsoft Advertising 광고 단위가 자동으로 생성됩니다.

 

 

 


<!--HONumber=Mar16_HO5-->


