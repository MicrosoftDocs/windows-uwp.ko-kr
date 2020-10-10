---
title: 게임의 수익 창출
description: Windows 10에서 유니버설 Windows 플랫폼 (UWP) 게임의 배너 광고, 중간 비디오 광고 및 앱 내 구매를 구현 합니다.
ms.assetid: 79f4e177-d8e7-45d3-8a78-31d4c2fe298a
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 수익 화
ms.localizationpriority: medium
ms.openlocfilehash: c827c257947ea0f365bafe497e627841b501d40d
ms.sourcegitcommit: 5d84d8fe60e83647fa363b710916cf8b92c6e331
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91878586"
---
#  <a name="monetization-for-games"></a>게임의 수익 창출

게임 개발자는 비즈니스를 유지 하 고 열정적인에 대 한 정보를 유지 하기 위해 수익 화 옵션을 알아야 합니다. 우수한 게임을 만들어 볼 수 있습니다. 이 문서에서는 유니버설 Windows 플랫폼 (UWP) 게임의 수익 화 메서드 및 구현 방법에 대 한 개요를 제공 합니다.

과거에는 게임에 가격을 추가한 다음 사용자가 매장에서 구매할 때까지 기다립니다. 그러나 현재는 옵션이 있습니다. 게임을 "brick 및 소매" 매장에 배포 하거나, 게임을 온라인 (물리적 또는 소프트 복사본)으로 판매 하거나, 모든 사람이 구매할 수 있는 일부 광고 또는 게임 내 항목을 통합 하도록 선택할 수 있습니다. 또한 게임은 더 이상 독립 실행형 제품 일 수 없습니다. 주요 게임 외에도 구매할 수 있는 추가 콘텐츠가 제공 되는 경우가 많습니다.

다음 방법 중 하나 이상으로 UWP 게임의 수준을 올리거나 수익 창출 수 있습니다.
* [전 세계 배포](#worldwide-distribution-channel)를 제공 하는 안전한 온라인 매장 인 Microsoft Store에 게임을 배치 합니다. 전 세계의 게이머는 [설정 된 가격](#set-a-price-for-your-game)으로 게임을 온라인으로 구매할 수 있습니다.
* Windows SDK Api를 사용 하 여 [게임 내 구매](#in-game-purchases)를 만듭니다. 게이머는 게임 내에서 항목을 구입 하거나 추가 장비, 스킨, 지도, 게임 수준 등의 추가 콘텐츠를 구입할 수 있습니다.
* [MICROSOFT ADVERTISING SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) 의 api를 사용 하 여 ad 네트워크에서 광고를 표시 합니다. [게임에서 광고를 표시](#display-ads-in-your-game) 하 고 게임 내 보상에 대해 exchange의 비디오 광고를 시청 하는 게이머를 위한 옵션을 제공할 수 있습니다.
* [광고 캠페인을 통해 게임 잠재력을 극대화](#maximize-your-games-potential-through-ad-campaigns)하세요. 유료, 커뮤니티 (무료) 또는 집 (무료) 광고 캠페인을 사용 하 여 게임을 홍보 하 여 해당 사용자 기반을 확장 하세요.

## <a name="worldwide-distribution-channel"></a>전 세계 배포 채널

Microsoft Store를 통해 전 세계 200 개 이상의 국가 및 지역에서 게임을 다운로드할 수 있습니다 .이를 통해 MasterCard, PayPal을 비롯 한 다양 한 지불 형식을 통해 요금 청구를 지원할 수 있습니다. 국가 및 지역의 전체 목록은 [시장 선택 정의](../publish/define-market-selection.md)를 참조 하세요.

## <a name="set-a-price-for-your-game"></a>게임 가격 설정

스토어에 게시 된 UWP 게임은 _유료_ 이거나 _무료로_제공 될 수 있습니다. 유료 게임을 사용 하면 사용자가 설정한 가격으로 게임에 대 한 게임을 바로 시작할 수 있으며, 무료 게임을 통해 사용자는 지불 하지 않고도 게임을 다운로드 하 고 재생할 수 있습니다.

다음은 스토어에서 게임의 가격에 대 한 몇 가지 중요 한 개념입니다.

### <a name="base-price"></a>기본 가격

게임의 기본 가격은 게임이 _유료_ 또는 _무료_인지 여부를 결정 하는 것입니다. [파트너 센터](https://partner.microsoft.com/dashboard) 를 사용 하 여 국가 및 지역에 따라 기본 가격을 구성할 수 있습니다.
가격을 결정 하는 프로세스에는 [특정 시장에 대 한](../publish/define-market-selection.md)다양 한 국가 및 비용 고려 사항 [에 판매 될 때의 세금 책임이](../publish/tax-details-for-paid-apps.md) 포함 될 수 있습니다. [특정 시장의 사용자 지정 가격을 설정할](../publish/set-and-schedule-app-pricing.md#override-base-price-for-specific-markets)수도 있습니다.

### <a name="sale-price"></a>판매 가격

게임을 홍보 하는 한 가지 방법은 제한 된 시간에 대 한 가격을 줄이는 것입니다. 판매 가격을 __무료__ 로 설정 하 여 게임을 지불 하지 않고 다운로드 하도록 할 수도 있습니다.
판매의 시작 날짜와 종료 날짜를 설정 하 여 판매 캠페인을 미리 예약할 수 있습니다. 자세한 내용은 [판매에 앱 및 추가 기능 배치](../publish/put-apps-and-add-ons-on-sale.md)를 참조 하세요.

## <a name="in-game-purchases"></a>게임 내 구입

게임 내 구입은 게임 내에서 구매한 제품입니다. 일반적으로 _앱에서 바로 구매_라고도 합니다. Microsoft Store에서 이러한 제품을 _추가_기능 이라고 합니다. [추가 기능은](../publish/add-on-submissions.md) 파트너 센터를 통해 게시 됩니다. 또한 게임 코드에서 추가 기능을 사용 하도록 설정 해야 합니다.

### <a name="types-of-add-ons"></a>추가 기능 유형

저장소에서 _내구_ 또는 _소모품_이라는 두 가지 유형의 추가 기능을 만들 수 있습니다. 내구는 지정 된 시간 동안 지속 되는 항목이 며 만료 될 때까지 한 번만 구매할 수 있습니다. 소모품은 다시 구매 하 여 다시 사용할 수 있는 항목입니다.

소모품을 만들 때 &mdash; _개발자 관리_ 또는 _스토어 관리_ 여부에 관계 없이 이러한 항목을 추적 하는 방법을 결정 합니다 .이 기능은 Windows 10 버전 1607부터 사용할 수 있습니다. 개발자가 관리 하는 기능을 사용 하 여 게이머의 항목 잔액을 추적 해야 합니다. 저장소 관리 기능을 사용 하면 Microsoft Store는 항목의 잔액을 계속 추적 합니다. 자세한 내용은 사용할 때 사용할 [추가 기능 개요](../monetize/enable-consumable-add-on-purchases.md)를 참조 하세요.

### <a name="create-in-game-purchases"></a>게임 내 구매 만들기

최신 앱 내 구매 및 라이선스 정보 Api는 Windows SDK (Windows 10 버전 1607부터)에 있는 [windows. Store](/uwp/api/windows.services.store) 네임 스페이스의 일부입니다. 1607 이상 버전을 대상으로 하는 새 게임을 개발 하는 경우 최신 추가 기능 형식을 지원 하 고 성능이 향상 되기 때문에 __Windows. Store__ 네임 스페이스를 사용 하는 것이 좋습니다.
또한 파트너 센터 및 스토어에서 지원 되는 이후 유형의 제품 및 기능과 호환 되도록 설계 되었습니다. 이전 버전의 Windows 10 용으로 개발 하는 경우 대신에 [windows.](/uwp/api/windows.applicationmodel.store)

자세한 내용은 [앱에서 바로 구매 및 평가판](../monetize/in-app-purchases-and-trials.md)으로 이동 하세요.

#### <a name="simplified-purchase-example"></a>단순화 된 구매 예제

이 섹션에서는 간단한 구매 예제를 사용 하 여 구매 흐름을 구현 하는 다양 한 메서드 호출을 사용 하는 방법을 보여 줍니다.

|게임 내 작업/작업                                                | 게임 백그라운드 작업                  |
|--------------------------------------------------------------------------|----------------------------------------|
|게이머는 상점에 들어갑니다. 사용 가능한 추가 기능 및 구매 가격을 표시 하는 둘러보기 메뉴 팝업 |  게임은 추가 기능의 [제품 정보를 검색](../monetize/get-product-info-for-apps-and-add-ons.md) 하 고, [추가 기능에 적절 한 라이선스가 있는지 여부를 확인](../monetize/get-license-info-for-apps-and-add-ons.md)하 고, 상점 메뉴의 게이머가 구매할 수 있는 추가 기능을 표시 합니다.                           |
|게이머는 __구매__ 를 클릭 하 여 항목 구매             |__구매__ 작업은 항목 구매 요청을 보내고 지불 프로세스를 시작 하 여 해당 항목을 획득 합니다. 구현은 항목 유형에 따라 달라 집니다. [내구성이 있거나 일회성 구매 항목인](../monetize/enable-in-app-purchases-of-apps-and-add-ons.md)경우 고객은 만료 될 때까지 단일 항목만 소유할 수 있습니다. 항목을 사용할 수 있는 경우 고객은 하나 이상의 [항목을 소유할](../monetize/enable-consumable-add-on-purchases.md)수 있습니다. |

### <a name="test-in-game-purchases-during-game-development"></a>게임 개발 중 게임 내 구매 시험

게임과 연결 하 여 추가 기능을 만들어야 하므로 스토어에서 게임을 게시 하 고 사용할 수 있어야 합니다. 이 섹션의 단계에서는 게임이 아직 개발 중인 동안 추가 기능을 만드는 방법을 보여 줍니다.
(완성 된 게임이 이미 스토어에 있는 경우 처음 세 단계를 건너뛰고 [스토어에서 추가 기능 만들기](#create-an-add-on-in-the-store)로 바로 이동할 수 있습니다.)

게임이 아직 개발 중인 동안 추가 기능을 만들려면 다음을 수행 합니다.
1. [패키지 만들기](#create-a-package)
2. [게임을 숨김으로 게시](#publish-the-game-as-hidden)
3. [Visual Studio의 게임 솔루션을 스토어에 연결](#associate-your-game-solution-with-the-store)
4. [저장소에서 추가 기능 만들기](#create-an-add-on-in-the-store)

#### <a name="create-a-package"></a>패키지 만들기

게임을 게시 하려면 최소 Windows 앱 인증 요구 사항을 충족 해야 합니다. Windows 10 SDK의 일부인 [Windows 앱 인증 키트](../debug-test-perf/windows-app-certification-kit.md)를 사용 하 여 게임에서 테스트를 실행 하 여 스토어에 게시할 준비가 되었는지 확인할 수 있습니다. Windows 앱 인증 키트를 포함 하는 Windows 10 SDK를 아직 다운로드 하지 않은 경우 [windows 10 sdk](https://developer.microsoft.com/windows/downloads/windows-10-sdk)로 이동 합니다.

저장소에 업로드할 수 있는 패키지를 만들려면 다음을 수행 합니다.

1. Visual Studio에서 게임 솔루션을 엽니다.
2. Visual Studio 내에서 __프로젝트__  >  __저장소__  >  __앱 패키지 만들기__ ...로 이동 합니다.
3. Microsoft Store에 __업로드할 패키지를 작성 하 시겠습니까?__ 옵션에서 __예__를 선택 합니다.
4. [파트너 센터](https://partner.microsoft.com/dashboard) 개발자 계정에 로그인 합니다. 또는 개발자 계정이 없는 경우 [등록](https://developer.microsoft.com/store/register) 합니다.
5. 업로드 패키지를 만들 앱을 선택 합니다. 앱 제출을 아직 만들지 않은 경우 새 앱 이름을 제공 하 여 새 제출을 만드세요. 자세한 내용은 [이름을 예약 하 여 앱 만들기](../publish/create-your-app-by-reserving-a-name.md)를 참조 하세요.
6. 패키지가 성공적으로 만들어진 후에 __Windows 앱 인증 키트 시작__을 클릭하여 테스트 프로세스를 시작합니다.
7. 게임 패키지를 만들기 위해 오류를 수정 합니다.

#### <a name="publish-the-game-as-hidden"></a>게임을 숨김으로 게시

1. [파트너 센터](https://partner.microsoft.com/dashboard) 로 이동 하 여 로그인 합니다.
2. __대시보드 개요__ 또는 __모든 앱__ 페이지에서 사용 하려는 앱을 클릭 합니다. 앱 제출을 아직 만들지 않은 경우 __새 앱 만들기__ 및 이름 예약을 클릭 합니다.
3. __앱 개요__ 페이지에서 __제출 시작__을 클릭 합니다.
4. 이 새 제출을 구성 합니다. 제출 페이지에서 다음을 수행 합니다.
    * __가격 책정 및 가용성을__클릭 합니다. __표시 유형__ 섹션에서 '__이 앱 숨기기 및 획득 방지__... '를 선택 하 여 개발 팀만 게임에 액세스할 수 있도록 합니다. 자세한 내용은 [배포 및 표시 유형](../publish/set-app-pricing-and-availability.md)을 참조 하세요.
    * __속성__을 클릭합니다. __범주 및 하위__ 범주 섹션 __에서 게임을 선택 하__ 고 게임에 적합 한 하위 범주를 선택 합니다.
    * __연령별 등급__을 클릭 합니다. 질문을 정확 하 게 작성 합니다.
    * __패키지__를 클릭 합니다. 이전 단계에서 만든 게임 패키지를 업로드 합니다.
5. 대시보드에 숨겨진 상태로 남아 있는이 게임을 성공적으로 게시할 수 있도록 대시보드의 다른 제출 프롬프트를 따르세요.
6. __스토어에 제출을__클릭 합니다.

자세한 내용은 [앱 제출](../publish/app-submissions.md)을 참조 하세요.

게임을 스토어에 제출 하면 [앱 인증 프로세스](../publish/the-app-certification-process.md)에 들어갑니다. 이 프로세스는 게임이 나열 되기까지 최대 16 시간이 걸릴 수 있습니다.

#### <a name="associate-your-game-solution-with-the-store"></a>게임 솔루션을 스토어에 연결

Visual Studio에서 게임 솔루션을 연 상태로 다음 작업을 수행합니다.

1. __프로젝트__  >  __저장소__로 이동  >  __앱을 스토어에 연결__ ...
2. 파트너 센터 개발자 계정에 로그인 하 고이 솔루션을 연결할 앱 이름을 선택 합니다.
3. __Package.appxmanifest.xml 파일__ 을 두 번 클릭 하 고 __패키징__ 탭으로 이동 하 여 게임이 올바르게 연결 되었는지 확인 합니다.

라이브 상태 이며 스토어에 나열 된 게시 된 게임에 솔루션을 연결한 경우 솔루션은 활성 라이선스를 갖게 되며 게임에 대 한 추가 기능을 만드는 데 한 단계가 더 가깝습니다. 자세한 내용은 [앱 패키징](../packaging/index.md)을 참조 하세요.

#### <a name="create-an-add-on-in-the-store"></a>저장소에서 추가 기능 만들기

추가 기능을 만들 때 적절 한 게임 전송과 연결 하 고 있는지 확인 합니다. 추가 기능에 연결 된 다양 한 정보를 구성 하는 방법에 대 한 자세한 내용은 [추가 기능 제출](../publish/add-on-submissions.md)을 참조 하세요.

1. [파트너 센터](https://partner.microsoft.com/dashboard) 로 이동 하 여 로그인 합니다.
2. __대시보드 개요__ 또는 __모든 앱__ 페이지에서 추가 기능을 만들려는 앱을 클릭 합니다.
3. __앱 개요__ 페이지의 __추가 기능__ 섹션에서 __새 추가 기능 만들기__를 선택 합니다.
4. 추가 기능에 대 한 제품 유형 ( __개발자 관리 되는 소비__가능, __매장 관리 가능__또는 __내구성__)을 선택 합니다.
5. 이 추가 기능을 게임 코드에 통합할 때 문자열 변수로 사용할 고유한 제품 ID를 입력 합니다. 이 ID는 소비자에 게 표시 되지 않습니다. 자세한 내용은 [앱 제품 유형 및 제품 ID 설정](../publish/set-your-add-on-product-id.md)을 참조 하세요.

추가 기능에 대 한 기타 구성에는 다음이 포함 됩니다.
* [속성](../publish/enter-add-on-properties.md)
* [가격 책정 및 가용성](../publish/set-add-on-pricing-and-availability.md)
* [매장 목록](../publish/create-add-on-store-listings.md)

게임에 많은 추가 기능이 있는 경우 __Microsoft Store 제출 API__를 사용 하 여 프로그래밍 방식으로 만들 수 있습니다. 자세한 내용은 [Microsoft Store 서비스를 사용 하 여 전송 만들기 및 관리](../monetize/create-and-manage-submissions-using-windows-store-services.md)를 참조 하세요.

## <a name="display-ads-in-your-game"></a>게임에서 광고 표시

Microsoft Advertising SDK의 라이브러리 및 도구를 통해 ad 네트워크에서 광고를 받도록 게임에서 서비스를 설정할 수 있습니다. 게이머는 라이브 광고를 표시 하 고, 게이머는 게이머를 보거나 표시 된 광고와 상호 작용할 때 광고주 로부터 돈을 획득 합니다.
자세한 내용은 [앱에서 광고 표시](../monetize/display-ads-in-your-app.md)를 참조 하세요.

### <a name="ad-formats"></a>Ad 형식

Microsoft Advertising SDK를 사용 하 여 여러 유형의 광고를 표시할 수 있습니다.

* 게임 &mdash; 화면에 포함 되 고 일반적으로 게임 내에 배치 되는 배너 광고 광고입니다.
* 중간 비디오 광고 &mdash; 전체 화면 광고-수준 간에 사용 될 때 매우 효과적일 수 있습니다. 제대로 구현 된 경우 배너 광고 보다 간섭 적을 수 있습니다.
* 기본 광고 &mdash; 구성 요소 기반 광고 (제목, 이미지, 설명 및 동작에 대 한 호출 텍스트 등)는 앱에 앱에 통합할 수 있는 개별 요소로 앱에 배달 됩니다.

### <a name="which-ads-are-displayed"></a>표시 되는 광고

기본적으로 앱은 유료 광고에 대해 Microsoft 네트워크의 광고를 표시 합니다. Ad 수익을 최대화 하기 위해 광고를 추가 하 여 유료 ad 네트워크에서 광고를 표시할 수 있습니다. 현재 제품에 대 한 자세한 내용은 [ad 중재](../publish/in-app-ads.md#mediation) 지침을 참조 하세요.

### <a name="which-markets-allow-ads-to-be-displayed"></a>광고를 표시할 수 있는 시장

광고를 지 원하는 국가 및 지역의 전체 목록은 [ad 네트워크에 대해 지원 되는 시장](../publish/in-app-ads.md#network-markets)을 참조 하세요.

### <a name="apis-for-displaying-ads"></a>광고 표시를 위한 Api

Microsoft Advertising SDK의 [Adcontrol](/uwp/api/microsoft.advertising.winrt.ui.adcontrol), [interstitialad](/uwp/api/microsoft.advertising.winrt.ui.interstitialad)및 [NativeAd](/uwp/api/microsoft.advertising.winrt.ui.nativead) 클래스는 게임의 광고를 표시 하는 데 사용 됩니다.

시작 하려면 Visual Studio 2015 이상 버전을 사용 하 여 [MICROSOFT ADVERTISING SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) 를 다운로드 하 여 설치 하세요. 자세한 내용은 [MICROSOFT ADVERTISING SDK 설치](../monetize/install-the-microsoft-advertising-libraries.md)를 참조 하세요.

#### <a name="implementation-guides"></a>구현 가이드

다음 연습은 __Adcontrol__, __interstitialad__및 __NativeAd__를 사용 하 여 광고를 구현 하는 방법을 보여 줍니다.

* [XAML 및 .NET에서 배너 광고 만들기](../monetize/adcontrol-in-xaml-and--net.md)
* [HTML5 및 JavaScript에서 배너 광고 만들기](../monetize/adcontrol-in-html-5-and-javascript.md)
* [중간 광고 만들기](../monetize/interstitial-ads.md)
* [네이티브 광고 만들기](../monetize/native-ads.md)

개발 하는 동안 [테스트 ad 단위 값](../monetize/set-up-ad-units-in-your-app.md) 을 사용 하 여 광고가 렌더링 되는 방식을 확인할 수 있습니다. 이러한 테스트 ad 단위 값은 위의 연습 에서도 사용 됩니다.

디자인 및 구현 프로세스에 도움이 되는 몇 가지 모범 사례는 다음과 같습니다.

* [배너 광고에 대 한 모범 사례](../monetize/ui-and-user-experience-guidelines.md)
* [중간 광고에 대 한 모범 사례](../monetize/ui-and-user-experience-guidelines.md)

광고 표시 안 함과 같은 일반적인 개발 문제에 대 한 해결 방법, 검은색 상자 깜박임 및 사라짐 또는 광고를 새로 고치지 않는 경우 [문제 해결 가이드](../monetize/known-issues-for-the-advertising-libraries.md)를 참조 하세요.

### <a name="prepare-for-release-by-replacing-ad-unit-test-values"></a>Ad 단위 테스트 값을 바꿔서 릴리스 준비

라이브 테스트로 이동 하거나 게시 된 게임에서 광고를 받을 준비가 되 면 테스트 ad 단위 값을 게임에 제공 된 실제 값으로 업데이트 해야 합니다. 게임의 ad 단위를 만들려면 [앱에서 ad 단위 설정](../monetize/set-up-ad-units-in-your-app.md)을 참조 하세요.

### <a name="other-ad-networks"></a>기타 ad 네트워크

UWP 앱 및 게임에 광고를 제공 하기 위한 Sdk를 제공 하는 다른 ad 네트워크입니다.

#### <a name="vungle"></a>Vungle

Windows 용 Vungle SDK는 앱 및 게임의 비디오 광고를 제공 합니다. SDK를 다운로드 하려면 [Vungle sdk](https://publisher.vungle.com/sdk/)로 이동 합니다.

#### <a name="smaato"></a>Smaato

Smaato를 사용 하면 배너 광고를 UWP 앱 및 게임에 통합할 수 있습니다. [SDK](https://www.smaato.com/resources/sdks/) 를 다운로드 하 고 자세한 내용은 [설명서](https://wiki.smaato.com/display/SPX/Windows+Phone)를 참조 하세요.

#### <a name="adduplex"></a>AdDuplex

AdDuplex를 사용 하 여 게임에서 배너 또는 중간 광고를 구현할 수 있습니다.

AdDuplex를 Windows 10 XAML 프로젝트에 직접 통합 하는 방법에 대해 자세히 알아보려면 AdDuplex 웹 사이트로 이동 합니다.
* 배너 광고: [XAML 용 Windows 10 SDK](https://adduplex.zendesk.com/hc/articles/204849031-Windows-10-SDK-for-XAML-apps-installation-and-usage)
* 중간 광고: [Windows 10 XAML AdDuplex 중간 광고 설치 및 사용](https://adduplex.zendesk.com/hc/articles/204849091-Windows-10-XAML-AdDuplex-Interstitial-Ad-Installation-and-Usage)

Unity를 사용 하 여 만든 Windows 10 UWP 게임에 AdDuplex SDK를 통합 하는 방법에 대 한 정보는 [unity 앱 용 windows 10 SDK 설치 및 사용](https://adduplex.zendesk.com/hc/articles/207279435-Windows-10-SDK-for-Unity-apps-installation-and-usage)을 참조 하세요.

## <a name="maximize-your-games-potential-through-ad-campaigns"></a>광고 캠페인을 통해 게임 잠재력 극대화

광고를 사용 하 여 게임을 홍보 하는 다음 단계를 수행 합니다. 게임에 대 한 [광고 캠페인을 만들](../monetize/index.md) 때 다른 앱 및 게임은 게임을 홍보 하는 광고를 표시 합니다.

게이머 기반을 높이는 데 도움이 될 수 있는 여러 유형의 캠페인 중에서 선택 합니다.

|캠페인 유형             | 게임의 광고가 다음에 표시 됩니다.                                                                                                                                                                   |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|유료                      |게임의 장치 또는 범주와 일치 하는 앱                                                                                                                                                   |
|무료 커뮤니티            |커뮤니티 ad 캠페인이 옵트인 된 다른 개발자가 게시 한 앱입니다. 자세한 내용은 [커뮤니티 광고 정보](../monetize/index.md)를 참조 하세요.|
|무료 집                |게시 한 앱만 자세한 내용은 [사내 광고 정보](../monetize/index.md)를 참조 하세요.                                                            |

## <a name="related-links"></a>관련 링크

* [지급 받기](../publish/getting-paid-apps.md)
* [계정 유형, 위치 및 수수료](../publish/account-types-locations-and-fees.md)
* [분석](../publish/analytics.md)
* [세계화 및 지역화](../design/globalizing/globalizing-portal.md)
* [앱의 평가판 구현](../monetize/implement-a-trial-version-of-your-app.md)
* [A/B 테스트로 앱 실험 실행](../monetize/run-app-experiments-with-a-b-testing.md)