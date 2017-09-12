---
author: mcleanbyron
ms.assetid: 7a38a352-6e54-4949-87b1-992395a959fd
description: "앱 내 광고의 UI 및 사용자 환경 지침을 알아봅니다."
title: "광고의 UI 및 사용자 환경 지침"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 광고, 광고, 지침, 모범 사례"
ms.openlocfilehash: 8dc9c00bdeb47b5f07af0b9b27ef843e2afd4456
ms.sourcegitcommit: 9d1ca16a7edcbbcae03fad50a4a10183a319c63a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2017
---
# <a name="ui-and-user-experience-guidelines-for-ads"></a>광고의 UI 및 사용자 환경 지침

이 문서에서는 앱 내 배너 광고와 중간 광고로 뛰어난 환경을 제공하기 위한 지침을 제시합니다. 앱의 모양과 느낌을 디자인하는 방법에 대한 일반적인 지침은 [디자인 및 UI](https://developer.microsoft.com/windows/apps/design)를 참조하세요.

> [!IMPORTANT]
> 앱 내 광고를 사용할 때는 [정책 10.10](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_10)(광고 수행 및 콘텐츠)을 포함하되 이에 국한되지 않는 Windows 스토어 정책을 준수해야 합니다. 특히 앱의 배너 광고 또는 중간 광고 구현은 Windows 스토어 정책의 [정책 10.10.1](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_10) 요구 사항을 충족해야 합니다. 이 문서에는 이 정책을 위반하는 구현의 예가 포함되어 있습니다. 이러한 예는 정책을 더 잘 이해하는 데 도움을 드리기 위한 하나의 방법으로서 정보 제공의 목적으로만 제공됩니다. 일부만 예로 든 것이며 이 문서에는 열거되지 않았지만 Windows 스토어 정책을 위반하는 다른 여러 가지 예가 있을 수 있습니다.

## <a name="guidelines-for-banner-ads"></a>배너 광고에 대한 지침

다음 섹션에서는 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx)을 사용하여 앱 내 배너 광고를 구현하는 방법에 대한 권장 사항과 Windows 스토어 정책의 [정책 10.10.1](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_10)을 위반하는 구현의 예를 제시합니다.

### <a name="best-practices"></a>모범 사례

앱 내 배너 광고를 구현할 때는 이러한 모범 사례를 따르는 것이 좋습니다.

* 앱 UI의 대부분을 기능 컨트롤 및 콘텐츠에 할애합니다.

* 사용 환경에 맞게 광고를 디자인합니다. 디자이너에게 광고 형태를 계획하기 위한 샘플 광고를 제공합니다. 앱의 잘 계획된 광고 예제 2가지는 콘텐츠 형태 광고 레이아웃 및 분할 레이아웃입니다.

  개발 및 테스트 단계에서 다양한 크기의 광고가 앱 내에서 표시되고 작동하는 방식을 보려면 [테스트 모드 광고 단위](test-mode-values.md)를 활용할 수 있습니다. 테스트를 완료한 경우 인증을 위해 앱을 제출하기 전에 [실제 광고 단위 ID로 앱을 업데이트](set-up-ad-units-in-your-app.md)해야 합니다.

* 실행 중인 디바이스의 레이아웃에 잘 맞는 [광고 크기](supported-ad-sizes-for-banner-ads.md)를 사용하세요.

* 광고를 사용할 수 없는 시간을 계획합니다. 광고가 앱으로 전송되지 않는 시간이 있을 수 있습니다. 페이지에 광고가 표시되는지 여부에 따라 페이지가 돋보이는 방식으로 레이아웃을 지정합니다. 자세한 내용은 [오류 처리](error-handling-with-advertising-libraries.md)를 참조하세요.

* 오버레이로 처리하는 것이 가장 효과적인 사용자 경고 시나리오가 있을 경우 오버레이를 표시한 상태로 [AdControl.Suspend](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.suspend.aspx)를 호출하고 경고 시나리오가 완료되면 [AdControl.Resume](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.resume.aspx)을 호출하세요.

<span />
### <a name="practices-to-avoid"></a>피해야 할 사례

앱 내 배너 광고를 구현할 때는 이러한 사례를 피하는 것이 좋습니다.

* 열린 공간에 광고를 고정하지 않습니다. 광고 공간을 찾을 수 있는 첫 번째 열린 공간에 배치하면 안 됩니다. 대신, 앱의 전체 디자인에 통합되어야 합니다.

* 지나친 광고로 앱을 포화시키지 않습니다. 앱에 광고가 너무 많으면 모양이 정신 없고 유용성이 떨어집니다. 광고로 수익을 창출할 수 있지만 앱 자체를 희생해서는 안 됩니다.

* 사용자가 핵심 작업이 아닌 다른 부분에 정신이 팔리게 하면 안 됩니다. 일차적인 포커스는 항상 앱에 있어야 합니다. 광고 공간은 이차적인 포커스가 유지되도록 통합해야 합니다.

<span />
### <a name="examples-of-policy-violations"></a>정책 위반의 예

이 섹션에서는 Windows 스토어 정책의 [정책 10.10.1](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_10)을 위반하는 배너 광고 시나리오의 예를 제시합니다. 이러한 예는 정책을 더 잘 이해하는 데 도움을 드리기 위한 하나의 방법으로서 설명의 목적으로만 제공됩니다. 일부만 예로 든 것이며 여기에는 열거되지 않았지만 정책 10.10.1을 위반하는 다른 여러 가지 예가 있을 수 있습니다.

* [AdControl.Suspend](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.suspend.aspx)를 먼저 호출하지 않고 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx)의 불투명도를 변경하거나 **AdControl** 위에 다른 컨트롤을 배치하는 등 사용자의 배너 광고 보기 기능을 방해하는 모든 행위.

* 사용자가 배너 광고를 클릭해야 앱에서 작업을 수행할 수 있거나 사용자가 앱 디자인 때문에 어쩔 수 없이 배너 광고를 클릭해야 하는 경우.

* 사용자 개입 없이 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 개체를 바꾸거나 페이지를 강제로 새로 고치는 등 어떤 식으로든 배너 광고의 기본 제공되는 최소 새로 고침 타이머를 무시하는 경우.

* 개발 및 테스트 중에, 또는 에뮬레이터에서 라이브 광고 단위(즉, Windows 개발자 센터 대시보드에서 가져온 광고 단위)를 사용하는 경우.

* 앱 컨텍스트에서 실행되는 Microsoft 광고 라이브러리 이외의 다른 방식을 통해 광고 서비스를 호출하는 코드를 작성하거나 배포하는 경우.

* Microsoft 광고 라이브러리로 생성된 문서화되지 않은 인터페이스 또는 자식 개체(예: **WebView** 또는 **MediaElement**)를 조작하는 경우.

<span id="interstitialbestpractices10">
## <a name="guidelines-for-interstitial-ads"></a>중간 광고에 대한 지침

과하지 않게 사용될 경우 중간 광고는 사용자 만족도에 부정적인 영향을 주지 않으면서 앱 수익을 크게 높일 수 있습니다. 그렇지만 잘못 사용하면 이러한 광고는 정반대의 효과를 가져올 수 있습니다.

다음 섹션에서는 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx)를 사용하여 앱 내 동영상 및 배너 중간 광고를 구현하는 방법에 대한 권장 사항과 Windows 스토어 정책의 [정책 10.10.1](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_10)을 위반하는 구현의 예를 제시합니다. 정책이 관련되는 경우를 제외하고, 개발자가 어느 누구보다 자신의 앱에 대해 잘 알고 있으므로 가장 적절한 결정은 본인이 내리는 것이 좋습니다. 그렇지만 앱 평점 및 수익이 긴밀하게 결합되어 있다는 사실을 명심해야 합니다.

### <a name="best-practices"></a>모범 사례

앱 내 중간 광고를 구현할 때는 이러한 모범 사례를 따르는 것이 좋습니다.

* 게임 레벨 중간과 같이 앱의 자연스러운 흐름 도중에 중간 광고를 표시합니다.

* 광고를 다음과 같은 구체적인 혜택과 연결합니다.

    * 레벨 완료를 위한 힌트

    * 레벨을 다시 시도하기 위한 추가 시간

    * 사용자 지정 아바타 특징(예: 문신 또는 모자)

* 앱이 동영상 중간 광고를 시청해야 완료되는 경우 닫기 단추를 누를 때 오류 메시지가 나타나도 놀라지 않도록 해당 규칙을 미리 언급합니다.

* 이상적으로 광고를 표시해야 하는 시간보다 30-60초 전쯤에 광고를 미리 가져옵니다([InterstitialAd.RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) 호출).

* [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 클래스에 노출된 4개의 모든 이벤트(**Canceled**, **Completed**, **AdReady** 및 **ErrorOccurred**)를 구독한 후 앱에 적절한 결정을 내리는 데 사용합니다.

* 서버 일치 광고 대신 일부 기본 제공 환경이 사용되도록 합니다. 이렇게 하면 다음과 같은 시나리오에서 유용할 수 있습니다.

    * 광고 서버에 연결할 수 없는 오프라인 모드

    * **ErrorOccurred** 이벤트가 발생하는 경우

    * [ConnectionProfile](https://msdn.microsoft.com/library/windows/apps/windows.networking.connectivity.connectionprofile.aspx)을 기준으로 사용자 대역폭을 저장하도록 선택하는 경우 **ConnectionProfile** 클래스에 도움이 될 수 있는 API가 있습니다.

* 10초 이내에 이동할 수 없는 경우와 같은 타당한 이유가 있는 경우가 아니면 기본(30초) 시간 제한을 사용합니다. 중간 광고는 특히, 고속 연결이 제공되지 않는 시장에서 표준 배너 광고보다 다운로드하는 데 훨씬 더 오래 걸립니다.

<span/>

* 사용자의 데이터 요금제를 염두에 둡니다. 예를 들어 데이터 제한에 가까워졌거나 제한을 초과하는 모바일 디바이스에서는 동영상 중간 광고를 표시하지 않거나 광고를 제공하기 전에 사용자에게 경고를 합니다. [ConnectionProfile](https://msdn.microsoft.com/library/windows/apps/windows.networking.connectivity.connectionprofile.aspx) 클래스에 도움이 될 수 있는 API가 있습니다.

* 초기 전송 후 앱을 지속적으로 향상시킵니다. [광고 보고서](../publish/advertising-performance-report.md)를 살펴보고 채우기 및 동영상 중간 광고 완료 속도 향상에 도움이 되도록 디자인을 변경합니다.

<span />
### <a name="practices-to-avoid"></a>피해야 할 사례

앱 내 중간 광고를 구현할 때는 이러한 사례를 피하는 것이 좋습니다.

* 지나친 광고를 삼갑니다. 사용자가 게임을 하는 것 외에 분명하게 선택적인 구체적 혜택을 받는 경우가 아니면 5분 간격 이상으로 광고를 제공하지 않도록 합니다.

* 사용자가 잘못된 타일을 클릭한 것으로 오인할 수 있으므로 앱을 시작할 때는 중간 광고를 표시하지 않습니다.

* 종료할 때 중간 광고를 표시하지 않습니다. 완료 속도가 거의 제로에 가까워지므로 잘못된 인벤토리입니다.

* 두 개 이상의 중간 광고를 연속으로 표시하지 않습니다. 진행률 표시줄이 시작점으로 다시 설정되게 되어 사용자에게 혼란을 줍니다. 많은 사람들이 코딩 버그나 광고 제공 버그 정도로 생각합니다.

* 동영상 중간 광고를 가져오고 5분 이상 경과한 후 [InterstitialAd.Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx)를 호출하지 않습니다. 좋은 인벤토리는 미리 가져온 광고가 청구 가능한 광고 노출로 최대한 많이 변환되도록 합니다.

* 광고 제공이 실패할 경우 광고를 사용할 수 없는 것과 같은 벌칙을 부과하지 않습니다. 예를 들어 "광고를 시청하여 *xxx* 받기" UI 옵션을 표시한다면 사용자가 광고를 시청한 후 *xxx*를 제공해야 합니다. 다음 두 가지 옵션을 고려해야 합니다.

    * [InterstitialAd.AdReady](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.adready.aspx) 이벤트가 발생하지 않는 한 이 옵션을 포함하지 않습니다.

    * 앱에 실제 광고와 동일한 이점을 가져오는 기본 제공 환경을 포함합니다.

* 멀티 플레이어 게임을 하는 사용자에게 경쟁적 우위를 제공하기 위해 중간 광고를 사용하지 않습니다. 예를 들어 1인칭 슈팅 게임에서 사용자가 중간 광고를 볼 경우 더 좋은 총을 제공한다고 사용자를 유인하지 않습니다. 플레이어의 아바타에 적용된 사용자 지정 셔츠는 위장 효과를 제공하지 않으면 괜찮습니다.

<span />
### <a name="examples-of-policy-violations"></a>정책 위반의 예

이 섹션에서는 Windows 스토어 정책의 [정책 10.10.1](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_10)을 위반하는 중간 광고 시나리오의 예를 제시합니다. 이러한 예는 정책을 더 잘 이해하는 데 도움을 드리기 위한 하나의 방법으로서 설명의 목적으로만 제공됩니다. 일부만 예로 든 것이며 여기에는 열거되지 않았지만 정책 10.10.1을 위반하는 다른 여러 가지 예가 있을 수 있습니다.

* 중간 광고 컨테이너 위에 UI 요소 배치.

* 사용자가 앱에 연결된 동안 [InterstitialAd.Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx) 호출.

* 통화로 사용 가능하거나 다른 사용자와 거래할 수 있는 것을 얻기 위해 중간 광고 사용.

* [InterstitialAd.ErrorOccurred](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.erroroccurred.aspx) 이벤트에 대한 이벤트 처리기의 컨텍스트에서 새 중간 광고 요청. 이렇게 하면 무한 루프가 발생하고 광고 서비스에서 작동 문제가 발생할 수 있습니다.

* 단순히 일련의 광고에 대한 백업 광고를 위해 중간 광고 요청. 중간 광고를 요청한 후 [InterstitialAd.AdReady](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.adready.aspx) 이벤트를 받은 경우 앱에 표시되는 다음 중간 광고는 [InterstitialAd.Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx) 메서드를 통해 표시할 준비가 된 광고여야 합니다.

* 개발 및 테스트 중에, 또는 에뮬레이터에서 라이브 광고 단위(즉, Windows 개발자 센터 대시보드에서 가져온 광고 단위)를 사용하는 경우.

* 앱 컨텍스트에서 실행되는 Microsoft 광고 라이브러리 이외의 다른 방식을 통해 광고 서비스를 호출하는 코드를 작성하거나 배포하는 경우.

* Microsoft 광고 라이브러리로 생성된 문서화되지 않은 인터페이스 또는 자식 개체(예: **WebView** 또는 **MediaElement**)를 조작하는 경우.

 

 
