---
title: 게임의 수익 창출
description: Windows 10에서 UWP(유니버설 Windows 플랫폼) 게임에 대한 배너 광고, 중간 동영상 광고 및 앱에서 바로 구매를 구현합니다.
ms.assetid: 79f4e177-d8e7-45d3-8a78-31d4c2fe298a
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 게임, 수익 창출
ms.localizationpriority: medium
ms.openlocfilehash: 4c84c42da0e2653f0e5958583ad0ee9b9babe3bc
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318949"
---
#  <a name="monetization-for-games"></a>게임의 수익 창출

게임 개발자라면 비즈니스를 지속하고 열정을 느낄 수 있는 일을 계속하기 위해 멋진 게임을 만드는 것과 같은 수익 창출 옵션을 알아야 할 필요가 있습니다. 이 문서에서는 UWP(유니버설 Windows 플랫폼) 게임의 수익 창출 방법 및 구현 방법을 대략적으로 설명합니다.

과거에는 게임에 가격을 지정하고 사람들이 스토어에서 구입하기만 기다리면 되었습니다. 하지만 요즘에는 옵션이 있습니다. "오프라인" 스토어에 게임을 배포하거나, 온라인으로 게임을 판매하거나(실제 또는 소프트 카피), 모든 사람이 무료로 게임을 즐길 수 있도록 하면서 구매 가능한 일종의 광고나 게임 내 항목을 포함하도록 선택할 수 있습니다. 게임 또한 더 이상 독립 실행형 제품이 아닙니다. 주 게임 외에 구매할 수 있는 추가 콘텐츠가 함께 제공되기도 합니다.

다음 방법 중 한 가지 이상으로 UWP 게임을 홍보하고 수익을 창출할 수 있습니다.
* 게임에는 보안이 적용 되는 Microsoft Store put, 온라인 제품을 상점 [전세계 배포](#worldwide-distribution-channel)합니다. 전 세계의 게이머가 [사용자가 설정한 가격](#set-a-price-for-your-game)으로 온라인으로 게임을 구입할 수 있습니다.
* Windows SDK의 API를 사용하여 [게임에서 바로 구매](#in-game-purchases)를 만듭니다. 게이머는 게임 내에서 항목을 구입하거나 추가 장비, 스킨, 지도 또는 게임 레벨과 같은 추가 콘텐츠를 구입할 수 있습니다.
* [Microsoft Advertising SDK](https://aka.ms/ads-sdk-uwp)의 API를 사용하여 광고 네트워크의 광고를 표시합니다. [게임에 광고를 표시](#display-ads-in-your-game)하고 게이머에게 게임 보상으로 비디오 광고를 볼 수 있는 옵션을 제공합니다.
* [광고 캠페인을 통해 게임 잠재력을 극대화](#maximize-your-games-potential-through-ad-campaigns)합니다. 유료, 커뮤니티(무료) 또는 하우스(무료) 광고 캠페인을 사용하여 게임을 홍보함으로써 사용자 기반을 넓힙니다.

## <a name="worldwide-distribution-channel"></a>전 세계 배포 채널

Microsoft Store 가능 게임 다운로드에 사용할 수 있는 200 개 이상의 국가 및 지역에서 전 세계 다양 한 형태의 결제 Visa, Mastercard, PayPal 등을 통해 청구 지원 합니다. 국가 및 지역의 전체 목록을 참조 하세요 [정의 시장 선택](https://docs.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection)합니다.

## <a name="set-a-price-for-your-game"></a>게임에 대한 가격 설정

스토어에 게시된 UWP 게임을 스토어는 _유료_ 또는 _무료_일 수 있습니다. 유료 게임의 경우 게임 플레이 전에 사용자가 설정한 가격을 게이머에게 부과할 수 있지만 무료 게임은 비용을 지불하지 않고 게임을 다운로드한 후 재생할 수 있습니다.

다음은 스토어의 게임 가격 책정과 관련된 몇 가지 중요한 개념입니다.

### <a name="base-price"></a>기본 가격

게임의 기본 가격은 게임이 _유료_로 분류되는지, _무료_로 분류되는지를 결정합니다. 사용할 수 있습니다 [파트너 센터](https://partner.microsoft.com/dashboard) 국가 및 지역을 기반으로 기본 가격을 구성 합니다.
가격을 결정하는 프로세스에는 [다른 국가로 판매할 때의 세금 책임](https://docs.microsoft.com/windows/uwp/publish/tax-details-for-paid-apps) 및 [특정 시장에 대한 가격 고려 사항](https://docs.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection)이 포함될 수 있습니다. [특정 시장에 대한 사용자 지정 가격을 설정](../publish/set-and-schedule-app-pricing.md#override-base-price-for-specific-markets)할 수도 있습니다.

### <a name="sale-price"></a>판매 가격

게임을 홍보하는 한 가지 방법은 제한된 시간 동안 가격을 할인하는 것입니다. 비용을 지불하지 않고 게임을 다운로드할 수 있도록 판매 가격을 __무료__로 설정할 수도 있습니다.
판매 시작 날짜와 종료 날짜를 설정하여 미리 판매 캠페인을 예약할 수 있습니다. 자세한 내용은 [할인 앱 및 추가 기능 판매](https://docs.microsoft.com/windows/uwp/publish/put-apps-and-add-ons-on-sale)를 참조하세요.

## <a name="in-game-purchases"></a>게임에서 바로 구매

게임에서 바로 구매는 게임 내에서 제품을 구매하는 것을 말합니다. 일반적으로 _앱에서 바로 구매_라고도 알려져 있습니다. Microsoft Store 이러한 제품 이라고 _추가 기능_합니다. [추가 기능 게시](https://docs.microsoft.com/windows/uwp/publish/add-on-submissions) 파트너 센터를 통해. 또한 게임 코드에서 추가 기능을 사용하도록 설정해야 합니다.

### <a name="types-of-add-ons"></a>추가 기능 유형

스토어에는 _지속성_ 또는 _소모성_의 두 가지 추가 기능 유형을 만들 수 있습니다. 지속성은 지정된 기간 동안 지속되며 만료될 때까지 한 번만 구입할 수 있는 항목을 나타냅니다. 소모성은 반복해서 구입하여 사용할 수 있는 항목을 나타냅니다.

소모성 항목을 만들 때는 이러한 항목을 추적하는 방법을 결정해야 합니다. 즉, _개발자가 관리하는지_ 또는 _스토어에서 관리하는지_ 여부를 추적해야 합니다(이 기능은 Windows 10 버전 1607부터 사용할 수 있음). 게이머;에 대 한 항목의 분산 된 추적에 대 한 책임은 개발자 관리 소모품을 사용 하 여 저장소 관리 소모품을 사용 하 여 Microsoft Store 추적 항목의 분산 있습니다. 자세한 내용은 [소모성 추가 기능 개요](https://docs.microsoft.com/windows/uwp/monetize/enable-consumable-add-on-purchases)를 참조하세요.

### <a name="create-in-game-purchases"></a>게임에서 바로 구매 만들기

최신 앱에서 바로 구매 및 라이선스 정보 API는 Windows SDK(Windows 10, 버전 1607부터)의 [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) 네임스페이스에 있습니다. 1607 이상 릴리스를 대상으로 하는 새 게임을 개발하는 경우 __Windows.Services.Store__ 네임스페이스가 최신 추가 기능 유형을 지원하 고 더 나은 성능을 제공하기 때문에 권장됩니다.
또한 향후 유형의 제품 및 파트너 센터와 저장소를 지 원하는 기능을 사용 하 여 호환 되도록 설계 되었습니다. 이전 버전의 Windows 10용으로 개발할 때는 [Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) 네임스페이스를 대신 사용합니다.

자세한 내용은 [앱에서 바로 구매 및 평가판](https://docs.microsoft.com/windows/uwp/monetize/in-app-purchases-and-trials)을 참조하세요.

#### <a name="simplified-purchase-example"></a>간단한 구매 예제

이 섹션에서는 간단한 구매 예제를 사용하여 다양한 메서드 호출을 통해 구매 흐름을 구현하는 방법을 보여 줍니다.

|게임 내 작업/활동                                                | 게임 백그라운드 작업                  |
|--------------------------------------------------------------------------|----------------------------------------|
|게이머가 가게에 들어갑니다. 쇼핑 메뉴가 팝업되면서 사용 가능한 추가 기능 및 구매 가격 표시 |  게임은 추가 기능의 [제품 정보를 검색](https://docs.microsoft.com/windows/uwp/monetize/get-product-info-for-apps-and-add-ons)하고, [추가 기능에 적절한 라이선스가 있는지 여부를 확인](https://docs.microsoft.com/windows/uwp/monetize/get-license-info-for-apps-and-add-ons)하고, 쇼핑 메뉴에서 게이머가 구매할 수 있는 추가 기능을 표시합니다.                           |
|게이머가 __구매__를 클릭하여 항목 구매             |__구매__ 작업이 항목 구매 요청을 보내고 구입을 위한 결제 프로세스를 시작합니다. 항목 유형에 따라 구현이 달라집니다. [지속형 또는 일회성 구매 항목](https://docs.microsoft.com/windows/uwp/monetize/enable-in-app-purchases-of-apps-and-add-ons)인 경우 해당 항목이 만료될 때까지 단일 항목만 소유할 수 있습니다. 항목이 [소모성](https://docs.microsoft.com/windows/uwp/monetize/enable-consumable-add-on-purchases)이면 하나 이상의 소유할 수 있습니다. |

### <a name="test-in-game-purchases-during-game-development"></a>게임 개발 중에 게임에서 바로 구매 테스트

추가 기능은 게임과 연계해서 만들어야 하므로 게임이 스토어에서 게시되고 사용할 수 있어야 합니다. 이 섹션의 단계는 게임 개발 동안 추가 기능을 만드는 방법을 보여 줍니다.
(완료된 게임이 스토어에 이미 라이브된 경우 처음 세 단계를 건너뛰고 [저장소에서 추가 기능 만들기](#create-an-add-on-in-the-store)로 바로 이동할 수 있습니다.)

게임 개발 동안 추가 기능을 만들려면
1. [패키지 만들기](#create-a-package)
2. [숨김으로 게임 게시](#publish-the-game-as-hidden)
3. [저장소를 사용 하 여 Visual Studio에서 게임 솔루션 연결](#associate-your-game-solution-with-the-store)
4. [저장소에 추가 기능 만들기](#create-an-add-on-in-the-store)

#### <a name="create-a-package"></a>패키지 만들기

게임이 게시되려면 최소 Windows 앱 인증 요구 사항을 충족해야 합니다. Windows 10 SDK의 일부인 [Windows 앱 인증 키트](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit)를 통해 게임 테스트를 실행하여 스토어에 게시할 준비가 되었는지 확인할 수 있습니다. Windows 앱 인증 키트를 포함하는 Windows 10 SDK를 아직 다운로드하지 않은 경우 [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)로 이동합니다.

스토어에 업로드할 수 있는 패키지를 만들려면

1. Visual Studio에서 게임 솔루션을 엽니다.
2. Visual Studio 내에서 __프로젝트__ > __스토어__ > __앱 패키지 만들기...__ 로 이동합니다.
3. 에 대 한 합니다 __Microsoft Store 업로드할 패키지를 작성 하 시겠습니까?__ 옵션을 선택 __예__합니다.
4. 에 로그인 하 여 [파트너 센터](https://partner.microsoft.com/dashboard) 개발자 계정. 또는 개발자 계정이 없으면 [등록](https://developer.microsoft.com/store/register)합니다.
5. 업로드 패키지를 만들 앱을 선택합니다. 앱 제출을 아직 만들지 않은 경우 새 앱 이름을 제공하여 새 제출을 만듭니다. 자세한 내용은 [이름을 예약하여 앱 만들기](https://docs.microsoft.com/windows/uwp/publish/create-your-app-by-reserving-a-name)를 참조하세요.
6. 패키지가 성공적으로 만들어진 후에 __Windows 앱 인증 키트 시작__을 클릭하여 테스트 프로세스를 시작합니다.
7. 모든 오류를 수정하여 게임 패키지를 만듭니다.

#### <a name="publish-the-game-as-hidden"></a>게임을 숨김으로 게시

1. 로 이동 [파트너 센터](https://partner.microsoft.com/dashboard) 에 로그인 합니다.
2. __대시보드 개요__ 또는 __모든 앱__ 페이지에서 사용하려는 앱을 클릭합니다. 앱 제출을 아직 만들지 않은 경우 __새 앱 만들기__를 클릭하고 이름을 예약합니다.
3. __앱 개요__ 페이지에서 __제출 시작__을 클릭합니다.
4. 새 제출을 구성합니다. 제출 페이지에서 다음을 수행합니다.
    * __가격 책정 및 가용성__을 클릭합니다. 에 __가시성__ 섹션을 선택 '__이 앱 숨기기 및 취득을 방지 하는 중...__ ' 확인만 개발 팀이 게임에 대 한 액세스. 자세한 내용은 [배포 및 표시 여부](https://docs.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability)를 참조하세요.
    * __속성__을 클릭합니다. __범주 및 하위 범주__ 섹션에서 __게임__을 선택한 다음 게임에 적합한 하위 범주를 선택합니다.
    * __연령별 등급__을 클릭합니다. 질문지를 정확하게 작성합니다.
    * __패키지__를 클릭합니다. 이전 단계에서 만든 게임 패키지를 업로드합니다.
5. 대시보드의 다른 제출 지침에 따라 이 게임을 일반 사용자에게 숨김 상태로 게시할 수 있습니다.
6. __스토어에 제출__을 클릭합니다.

자세한 내용은 [앱 제출](https://docs.microsoft.com/windows/uwp/publish/app-submissions)을 참조하세요.

게임이 스토어에 제출되면 [앱 인증 프로세스](https://docs.microsoft.com/windows/uwp/publish/the-app-certification-process)가 시작됩니다. 최대 16시간 동안 이 프로세스가 진행되면 게임이 목록에 표시됩니다.

#### <a name="associate-your-game-solution-with-the-store"></a>스토어에 게임 솔루션 연결

Visual Studio에서 게임 솔루션을 연 상태로 다음 작업을 수행합니다.

1. __프로젝트__ > __스토어__ > __응용 프로그램을 스토어에 연결...__ 로 이동합니다.
2. 파트너 센터 개발자 계정에 로그인 하 고 사용 하 여이 솔루션에 연결할 앱 이름을 선택 합니다.
3. __Package.appxmanifest.xml 파일__을 두 번 클릭하고 __패키징__으로 이동하여 게임이 올바르게 연결되었는지 확인합니다.

스토어에 게시하여 라이브 상태로 등록된 게임에 솔루션을 연결한 경우 활성 라이선스가 솔루션에 적용되므로 게임용 추가 기능을 더욱 쉽게 만들 수 있습니다. 자세한 내용은 [앱 패키징](https://docs.microsoft.com/windows/uwp/packaging/index)을 참조하세요.

#### <a name="create-an-add-on-in-the-store"></a>스토어에서 추가 기능 만들기

추가 기능을 만들 때 적절한 게임 제출에 연결하고 있는지 확인합니다. 추가 기능과 연결된 모든 다양한 정보를 구성하는 방법에 대한 자세한 내용은 [추가 기능 제출](https://docs.microsoft.com/windows/uwp/publish/add-on-submissions)을 참조하세요.

1. 로 이동 [파트너 센터](https://partner.microsoft.com/dashboard) 에 로그인 합니다.
2. __대시보드 개요__ 또는 __모든 앱__ 페이지에서 추가 기능을 구현하려는 앱을 클릭합니다.
3. __앱 개요__ 페이지의 __추가 기능__ 섹션에서 __새 추가 기능 만들기__를 선택합니다.
4. 추가 기능에 대한 제품 유형으로 __개발자 관리 소모__ , __스토어 관리 소모성__ 또는 __지속성__ 중 하나를 선택합니다.
5. 게임 코드에 이 추가 기능을 통합하는 경우 문자열 변수로 사용할 고유 제품 ID를 입력합니다. 이 ID는 소비자에게 표시되지 않습니다. 자세한 내용은 [앱 제품 유형 및 제품 ID 설정](https://docs.microsoft.com/windows/uwp/publish/set-your-add-on-product-id)을 참조하세요.

추가 기능에 대한 기타 구성은 다음과 같습니다.
* [Properties](https://docs.microsoft.com/windows/uwp/publish/enter-add-on-properties)
* [가격 책정 및 가용성](https://docs.microsoft.com/windows/uwp/publish/set-add-on-pricing-and-availability)
* [스토어 목록](https://docs.microsoft.com/windows/uwp/publish/create-add-on-store-listings)

게임 많은 추가 기능에 만들면 이러한 프로그래밍 방식으로 사용 하 여 합니다 __Microsoft Store 제출 API__합니다. 자세한 내용은 참조 하세요. [만들기 및 Microsoft Store 서비스를 사용 하 여 제출 관리](https://docs.microsoft.com/windows/uwp/monetize/create-and-manage-submissions-using-windows-store-services)합니다.

## <a name="display-ads-in-your-game"></a>게임에서 광고 표시

Microsoft Advertising SDK의 라이브러리 및 도구를 사용하면 광고 네트워크에서 광고를 받도록 게임의 서비스를 설정할 수 있습니다. 게이머에게는 라이브 광고가 표시되며, 게이머가 광고를 보거나 상호 작용을 수행하게 되면 광고주로부터 수익을 얻게 됩니다.
자세한 내용은 [앱에서 광고 표시](../monetize/display-ads-in-your-app.md)를 참조하세요.

### <a name="ad-formats"></a>광고 형식

Microsoft Advertising SDK를 사용하여 여러 유형의 광고를 표시할 수 있습니다.

* 광고 배너 &mdash; 게임 화면 일부를 차지하고 일반적으로 게임 내에 배치되는 광고입니다.
* 동영상 중간 광고 &mdash; 레벨 중간에 사용할 때 매우 효과적일 수 있는 전체 화면 광고입니다. 적절히 구현된 경우 배너 광고보다 게임을 덜 방해할 수 있습니다.
* 기본 광고 &mdash; 제목, 이미지, 설명, 행동 촉구 텍스트 등의 각 광고 크리에이티브 조각이 앱과 통합할 수 있는 개별 요소로 앱에 제공되는 구성 요소 기반 광고입니다.

### <a name="which-ads-are-displayed"></a>어떤 광고가 표시되나요?

기본적으로 앱에는 유료 광고에 대한 Microsoft 네트워크의 광고가 표시됩니다. 수익을 극대화하기 위해 추가 유료 광고 네트워크의 광고를 표시하도록 광고 단위에 대한 광고 조정을 사용할 수 있습니다. 현재 제공되는 서비스에 대한 자세한 내용은 [광고 조정](../publish/in-app-ads.md#mediation) 지침을 참조하세요.

### <a name="which-markets-allow-ads-to-be-displayed"></a>어떤 지역/국가에서 광고 표시를 허용하나요?

광고를 지원하는 국가 및 지역의 전체 목록을 보려면 [광고 네트워크 지원 지역/국가](../publish/in-app-ads.md#network-markets)를 참조하세요.

### <a name="apis-for-displaying-ads"></a>광고를 표시하기 위한 API

Microsoft Advertising SDK의 [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol), [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) 및 [NativeAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativead) 클래스는 게임에 광고를 표시하는 데 사용됩니다.

시작하려면 Visual Studio 2015 이상 버전과 함께 [Microsoft Advertising SDK](https://aka.ms/ads-sdk-uwp)를 다운로드하여 설치합니다. 자세한 내용은 [Microsoft Advertising SDK 설치](../monetize/install-the-microsoft-advertising-libraries.md)를 참조하세요.

#### <a name="implementation-guides"></a>구현 가이드

이 연습에서는 __AdControl__, __InterstitialAd__ 및 __NativeAd__를 사용하여 광고를 구현하는 방법을 보여 줍니다.

* [XAML 및.NET에서 배너 광고를 만듭니다](https://docs.microsoft.com/windows/uwp/monetize/adcontrol-in-xaml-and--net)
* [HTML5 및 JavaScript 배너 광고 만들기](https://docs.microsoft.com/windows/uwp/monetize/adcontrol-in-html-5-and-javascript)
* [중간 광고 만들기](https://docs.microsoft.com/windows/uwp/monetize/interstitial-ads)
* [네이티브 광고 만들기](https://docs.microsoft.com/windows/uwp/monetize/native-ads)

개발하는 동안 [테스트 광고 단위 값](../monetize/test-mode-values.md)을 사용하여 광고가 렌더링되는 방식을 확인할 수 있습니다. 이러한 테스트 광고 단위 값은 위의 연습에도 사용됩니다.

디자인 및 구현 프로세스에 도움이 되는 몇 가지 모범 사례는 다음과 같습니다.

* [배너 광고에 대 한 모범 사례](https://docs.microsoft.com/windows/uwp/monetize/ui-and-user-experience-guidelines)
* [중간 광고에 대한 모범 사례](https://docs.microsoft.com/windows/uwp/monetize/ui-and-user-experience-guidelines)

광고 표시 안 됨, 블랙 박스 깜박임 및 사라짐 또는 광고가 새로 고쳐지지 않음 등의 일반적인 개발 문제에 대한 해결 방법을 보려면 [문제 해결 가이드](https://docs.microsoft.com/windows/uwp/monetize/troubleshooting-guides)를 참조하세요.

### <a name="prepare-for-release-by-replacing-ad-unit-test-values"></a>광고 단위 테스트 값을 바꾸어 릴리스 준비

라이브 테스트로 이동하거나 게시된 게임에 광고를 수신할 준비가 되면 테스트 광고 단위 값을 게임용으로 제공된 실제 값으로 업데이트해야 합니다. 게임에 대한 광고 단위를 만들려면 [앱에서 광고 단위 설정](https://docs.microsoft.com/windows/uwp/monetize/set-up-ad-units-in-your-app)을 참조하세요.

### <a name="other-ad-networks"></a>기타 광고 네트워크

UWP 앱 및 게임에 광고 서비스를 제공하는 데 사용되는 SDK를 제공하는 기타 광고 네트워크입니다.

#### <a name="vungle"></a>Vungle

Vungle SDK for Windows에서 앱 및 게임에 비디오 광고를 제공합니다. SDK를 다운로드하려면 [Vungle SDK](https://publisher.vungle.com/sdk/)로 이동합니다.

#### <a name="smaato"></a>Smaato

Smaato에서는 UWP 앱 및 게임에 배너 광고를 통합할 수 있습니다. [SDK](https://www.smaato.com/resources/sdks/)를 다운로드하고, [설명서](https://wiki.smaato.com/display/SPX/Windows+Phone)에서 자세한 내용을 참조하세요.

#### <a name="adduplex"></a>AdDuplex

AdDuplex를 사용하여 게임에서 배너 또는 중간 광고를 구현할 수 있습니다.

Windows 10 XAML 프로젝트에 직접 AdDuplex를 통합하는 방법을 알아보려면 AdDuplex 웹 사이트로 이동합니다.
* 배너 광고: [Windows 10 XAML 용 SDK](https://adduplex.zendesk.com/hc/en-us/articles/204849031-Windows-10-SDK-for-XAML-apps-installation-and-usage)
* 중간 광고: [Windows 10 XAML AdDuplex 중간 Ad 설치 및 사용](https://adduplex.zendesk.com/hc/en-us/articles/204849091-Windows-10-XAML-AdDuplex-Interstitial-Ad-Installation-and-Usage)

Unity를 사용하여 만든 Windows 10 UWP 게임에 AdDuplex SDK를 통합하는 방법에 대한 자세한 내용은 [Unity 앱 설치 및 사용에 대한 Windows 10 SDK](https://adduplex.zendesk.com/hc/en-us/articles/207279435-Windows-10-SDK-for-Unity-apps-installation-and-usage)를 참조하세요.

## <a name="maximize-your-games-potential-through-ad-campaigns"></a>광고 캠페인을 통해 게임 잠재력 극대화

광고를 사용하여 게임을 홍보할 때는 다음 단계를 수행합니다. 게임에 대한 [광고 캠페인을 만들면](https://docs.microsoft.com/windows/uwp/publish/create-an-ad-campaign-for-your-app) 다른 앱 및 게임에서 게임을 홍보하는 광고를 표시합니다.

게이머 기반을 늘리는 데 도움이 되는 여러 유형의 캠페인에서 선택합니다.

|캠페인 유형             | 게임 광고가 표시되는 위치...                                                                                                                                                                   |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|유료                      |게임의 디바이스 또는 범주에 해당하는 앱입니다.                                                                                                                                                   |
|무료 커뮤니티            |커뮤니티 광고 캠페인도 옵트인(opt in)한 다른 개발자가 게시하는 앱입니다. 자세한 내용은 [커뮤니티 광고 정보](https://docs.microsoft.com/windows/uwp/publish/about-community-ads)를 참조하세요.|
|무료 하우스                |게시한 앱만 무료인 경우입니다. 자세한 내용은 [하우스 광고 정보](https://docs.microsoft.com/windows/uwp/publish/about-house-ads)를 참조하세요.                                                            |

## <a name="related-links"></a>관련 링크

* [지급 받기](https://docs.microsoft.com/windows/uwp/publish/getting-paid-apps)
* [계정 유형, 위치 및 수수료](https://docs.microsoft.com/windows/uwp/publish/account-types-locations-and-fees)
* [분석](https://docs.microsoft.com/windows/uwp/publish/analytics)
* [세계화 및 지역화](https://docs.microsoft.com/windows/uwp/globalizing/globalizing-portal)
* [앱의 평가판 버전을 구현 합니다.](https://docs.microsoft.com/windows/uwp/monetize/implement-a-trial-version-of-your-app)
* [사용 하 여 앱 실험 실행 / B 테스트](https://docs.microsoft.com/windows/uwp/monetize/run-app-experiments-with-a-b-testing)
