---
ms.assetid: 7a38a352-6e54-4949-87b1-992395a959fd
description: 앱에서 배너 광고, 중간 광고 및 네이티브 광고를 사용 하 여 뛰어난 UI 및 사용자 환경을 제공 하기 위한 지침을 참조 하세요.
title: 광고에 대 한 UI 및 사용자 환경 지침
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, 광고, 광고, 지침, 모범 사례
ms.localizationpriority: medium
ms.openlocfilehash: 014cc5a7be370ed5fd579af9bc4ab7600cb96689
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158357"
---
# <a name="ui-and-user-experience-guidelines-for-ads"></a>광고에 대 한 UI 및 사용자 환경 지침

>[!WARNING]
> 2020 년 6 월 1 일부 터 Windows UWP 앱 용 Microsoft Ad 수익 화 플랫폼이 종료 됩니다. [자세한 정보](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

이 문서에서는 앱에서 배너 광고, 중간 광고 및 기본 광고를 사용 하 여 뛰어난 환경을 제공 하기 위한 지침을 제공 합니다. 앱에 대 한 모양과 느낌을 디자인 하는 방법에 대 한 일반적인 지침은 [& UI 디자인](https://developer.microsoft.com/windows/apps/design)을 참조 하세요.

> [!IMPORTANT]
> 앱에서 광고를 사용 하는 경우 [정책 10.10](/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) (광고 수행 및 콘텐츠)을 포함 하 여 Microsoft Store 정책을 준수 해야 합니다. 특히 응용 프로그램의 배너 광고 또는 중간 광고 구현은 Microsoft Store 정책 [정책 10.10.1](/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content)의 요구 사항을 충족 해야 합니다. 이 문서에는이 정책을 위반 하는 구현의 예가 포함 되어 있습니다. 이러한 예제는 정책을 보다 잘 이해 하는 데 도움이 되는 방법으로 정보를 제공 하기 위해서만 제공 됩니다. 이러한 예제는 포괄적이 지 않으며이 문서에 나열 되지 않은 Microsoft Store 정책을 위반 하는 다른 여러 가지 방법이 있을 수 있습니다.

## <a name="general-best-practices"></a>일반적인 유용한 정보

이 문서에서 다양 한 유형의 광고에 대 한 지침을 검토 하기 전에 먼저 이러한 일반적인 모범 사례를 검토 하 여 ad 수익을 개선 하세요.

* [광고 배치를 신중 하 게 계획](https://blogs.windows.com/buildingapps/2017/04/10/monetizing-app-advertisement-placement/)합니다. [Ad 단위의 viewability 최적화](optimize-ad-unit-viewability.md)에 대 한 관련 지침을 참조 하세요.
* 중간 [배너 광고를 중간 비디오 광고에 대 한 대체 (fallback)로 사용](https://blogs.windows.com/buildingapps/2017/04/17/monetizing-app-use-interstitial-banner-fallback-interstitial-video/)합니다.
* [더 나은 대상 광고를 제공 하기 위해 사용자](https://blogs.windows.com/buildingapps/2017/05/17/monetize-app-know-user-serve-better-targeted-ads/)에 게 알립니다.
* [최신 광고 라이브러리를 사용](https://blogs.windows.com/buildingapps/2017/05/22/earn-money-moving-latest-advertising-libraries/)합니다.
* [앱에 대 한 올바른 COPPA 설정을 설정](https://blogs.windows.com/buildingapps/2017/06/21/monetizing-app-set-coppa-settings-app/)합니다.


## <a name="guidelines-for-banner-ads"></a>배너 광고 지침

다음 섹션에서는 [Adcontrol](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 을 사용 하 여 앱에서 [배너 광고](banner-ads.md) 를 구현 하는 방법에 대 한 권장 사항 및 Microsoft Store 정책의 [정책 10.10.1](/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) 을 위반 하는 구현 예제를 제공 합니다.

### <a name="best-practices"></a>모범 사례

앱에서 배너 광고를 구현할 때 다음 모범 사례를 따르는 것이 좋습니다.

* 장치의 레이아웃과 잘 맞는 [대화형 광고 기관 크기를 사용](https://blogs.windows.com/buildingapps/2017/04/03/monetizing-app-use-interactive-advertising-bureau-ad-sizes/) 합니다.

* 기능 제어 및 콘텐츠에 대 한 대부분의 앱 UI를 활용 합니다.

* 사용자 환경에 대 한 광고를 디자인 합니다. 광고의 모양을 계획 하는 샘플 광고를 디자이너에 게 제공 합니다. 앱에서 잘 계획 된 광고의 두 가지 예는 광고 콘텐츠 레이아웃 및 분할 레이아웃입니다.

  개발 및 테스트 단계에서 앱 내에서 다양 한 ad 크기가 표시 되 고 작동 하는 방식을 확인 하기 위해 [테스트 ad 단위](set-up-ad-units-in-your-app.md#test-ad-units)를 활용할 수 있습니다. 테스트를 완료 한 후에는 인증을 위해 앱을 제출 하기 전에 [라이브 ad 단위를 사용 하 여 앱을 업데이트](set-up-ad-units-in-your-app.md#live-ad-units) 해야 합니다.

* 광고를 사용할 수 없는 시간을 계획 합니다. 광고가 앱에 전송 되지 않는 경우가 있을 수 있습니다. 광고를 표시 하는 것 처럼 보이는 방식으로 페이지를 레이아웃 합니다. 자세한 내용은 [ad 오류 처리](error-handling-with-advertising-libraries.md)를 참조 하세요.

* 오버레이를 사용 하 여 가장 잘 처리 되는 사용자에 게 경고 하는 시나리오가 있는 경우 오버레이를 표시 한 다음 adcontrol을 호출 [합니다. 경고](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.suspend) 시나리오가 완료 되 면 [다시 시작](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.resume) 합니다.

### <a name="practices-to-avoid"></a>예방 방법

앱에서 배너 광고를 구현 하는 경우 다음 방법을 사용 하지 않는 것이 좋습니다.

* 광고를 오픈 부동산으로 전환 하지 않습니다. 광고 공간을 찾을 수 있는 첫 번째 열린 공간에 배치하면 안 됩니다. 대신 앱의 전체 설계에 통합 되어야 합니다.

* 앱을 과도 하 게 광고 및 포화 하지 않습니다. 앱에 너무 많은 광고는 자신의 모양과 유용성을 저하 시킬 수 있습니다. 광고를 사용 하 여 비용을 만들려고 하지만 앱 자체를 사용 하는 것은 아닙니다.

* 사용자의 핵심 작업에서 사용자를 방해 마세요. 일차적인 포커스는 항상 앱에 있어야 합니다. Ad 공간이 통합 되어 보조 포커스를 유지 해야 합니다.

### <a name="examples-of-policy-violations"></a>정책 위반의 예

이 섹션에서는 Microsoft Store 정책의 [정책 10.10.1](/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) 를 위반 하는 배너 광고 시나리오의 예를 제공 합니다. 이러한 예제는 정책을 이해 하는 데 도움이 되는 방법 으로만 교육용 으로만 제공 됩니다. 이러한 예제는 포괄적이 지 않으며 여기에 나열 되지 않은 정책 10.10.1를 위반 하는 다른 여러 가지 방법이 있을 수 있습니다.

* 모든 작업을 수행 하 [여 adcontrol](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 을 먼저 호출 하지 않고 adcontrol의 불투명도를 변경 하거나 **adcontrol** 위에 다른 컨트롤을 배치 하는 등의 배너 광고를 보는 기능을 방해 [합니다.](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.suspend)

* 사용자가 배너 광고를 클릭 하 여 앱에서 작업을 수행 하도록 하거나 앱 디자인의 결과로 사용자가 배너 광고를 클릭 하도록 합니다.

* [Adcontrol](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 개체 교체 또는 사용자 개입 없이 페이지 새로 고침 적용을 포함 하 여 모든 방법으로 배너 광고에 대해 기본 제공 되는 최소 새로 고침 타이머를 무시 합니다.

* 개발 및 테스트 중 또는 에뮬레이터에서 live ad 단위 (파트너 센터에서 가져오는 ad 단위)를 사용 합니다.

* 응용 프로그램의 컨텍스트에서 실행 되는 Microsoft 광고 라이브러리 이외의 수단을 통해 광고 서비스를 호출 하는 코드를 작성 하거나 배포 합니다.

* **웹 보기** 또는 **MediaElement**와 같이 Microsoft 광고 라이브러리에서 만든 문서화 되지 않은 인터페이스 또는 자식 개체와 상호 작용

* 광고를 viewbox 하 여 광고의 크기를 줄이면 일반적인 페이지에서 광고를 더 많이 허용 합니다.

<span id="interstitialbestpractices10" />

## <a name="guidelines-for-interstitial-ads"></a>중간 광고에 대 한 지침

원활를 사용 하는 경우 [중간 광고](interstitial-ads.md) 는 사용자 만족도에 부정적인 영향을 주지 않고 앱 수익을 크게 증가 시킬 수 있습니다. 이러한 광고는 부적절 하 게 사용 될 경우 정확히 반대 효과가 있습니다.

다음 섹션에서는 [Interstitialad](/uwp/api/microsoft.advertising.winrt.ui.interstitialad)를 사용 하 여 앱에서 중간 비디오 광고 및 중간 배너 광고를 구현 하는 방법에 대 한 권장 사항을 제공 하 고 Microsoft Store 정책의 [정책 10.10.1](/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) 을 위반 하는 구현 예를 제공 합니다. 정책에 관심이 있는 경우를 제외 하 고 모든 사용자가 앱을 더 잘 알고 있기 때문에 최상의 최종 결정을 내릴 수 있습니다. 가장 중요 한 점은 앱 등급과 수익이 긴밀 하 게 결합 된다는 것입니다.

### <a name="best-practices"></a>모범 사례

앱에서 중간 광고를 구현할 때 다음 모범 사례를 따르는 것이 좋습니다.

* 게임 수준 간의 일반적인 앱 흐름 내에서 중간 광고에 맞춥니다.

* 다음을 비롯 하 여 광고를 유형 upsides 연결 합니다.

    * 수준 완료에 대 한 힌트입니다.

    * 수준을 다시 시도할 시간이 추가 됩니다.

    * 사용자 지정 아바타 기능 (예: 문신 또는 hat)

* 앱에서 중간 비디오 광고를 완료 해야 하는 경우 해당 규칙을 미리 언급 하 여 닫기 단추를 누르면 오류 메시지가 나타나지 않도록 합니다.

* Ad를 미리 인출 하려면 ( [Interstitialad. RequestAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad)를 호출 하 여) 30-60 초 정도를 표시 해야 합니다.

* [Interstitialad](/uwp/api/microsoft.advertising.winrt.ui.interstitialad) 클래스 (**취소 됨**, **완료 됨**, **adready**및 **erroroccurred**)에서 노출 된 네 개의 이벤트를 모두 구독 하 고이를 사용 하 여 앱에 대 한 적절 한 결정을 내립니다.

* 서버와 일치 하는 ad 대신를 사용 하는 몇 가지 기본 제공 환경이 있습니다. 이는 다음과 같은 몇 가지 시나리오에서 유용 합니다.

    * 오프 라인 모드, ad 서버에 연결할 수 없는 경우

    * **Erroroccurred** 이벤트가 발생 하면이 오류가 발생 합니다.

    * [Connectionprofile](/uwp/api/Windows.Networking.Connectivity.ConnectionProfile)에 따라 사용자 대역폭을 저장 하도록 선택 하는 경우 **connectionprofile** 클래스에는 도움이 될 수 있는 api가 있습니다.

* 그렇지 않은 경우에는 기본값 (30 초) 시간 제한을 사용 합니다. 그렇지 않으면 10 초 미만으로 이동 하지 않습니다. 중간 광고는 표준 배너 광고 보다 다운로드 하는 데 더 많은 시간이 걸립니다. 특히 고속 연결이 없는 시장에서 사용 합니다.

* 사용자의 데이터 요금제를 염두에 둘 수 있습니다. 예를 들어, 데이터 제한이 거의 발생 하지 않은 모바일 장치에서 중간 비디오 광고를 제공 하기 전에 사용자를 표시 하지 않거나 사용자에 게 경고 합니다. [Connectionprofile](/uwp/api/Windows.Networking.Connectivity.ConnectionProfile) 클래스에는 도움이 될 수 있는 api가 있습니다.

* 초기 제출 후 지속적으로 앱을 개선 하세요. [광고 보고서](../publish/advertising-performance-report.md) 를 확인 하 고 디자인을 변경 하 여 채우기 및 중간 비디오 완료 비율을 개선 합니다.

### <a name="practices-to-avoid"></a>예방 방법

앱에서 중간 광고를 구현할 때 이러한 방법을 사용 하지 않는 것이 좋습니다.

* 과도 하 게 수행 하지 마세요. 사용자가 게임을 재생 하는 것 외에도 선택적으로 분명 한 이점을 제공 하는 경우를 제외 하 고 5 분 마다 광고를 강제로 실행 하지 마세요.

* 사용자가 잘못 된 타일을 클릭 했을 수 있으므로 앱 시작 시 interstitials를 표시 하지 않습니다.

* Exit에서 interstitials를 표시 하지 않습니다. 이는 잘못 된 인벤토리 이므로 완료 요금은 0에 근접 하 게 됩니다.

* 두 개 이상의 중간 광고를 다시 표시 하지 않습니다. 사용자는 ad 진행률 표시줄이 시작 지점으로 다시 설정 되는 것을 볼 수 있습니다. 대부분은 코딩 또는 광고 제공 버그 라고 생각 합니다.

* [Interstitialad. Show](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show)를 호출 하기 전에는 중간 비디오 광고를 5 분 이상 인출 하지 마십시오. 좋은 인벤토리는 미리 인출 된 광고를 청구 가능한 대상으로 변환 하는 것을 최대화 합니다.

* Ad를 사용할 수 없는 경우와 같이 ad 서비스의 오류에 대해 사용자에 게 penalize 마세요. 예를 들어 "ad를 사용 하 여 *xxx*가져오기"에 대 한 UI 옵션을 표시 하는 경우 사용자가 해당 파트를 만든 경우 *xxx* 를 제공 해야 합니다. 고려해 야 할 두 가지 옵션은 다음과 같습니다.

    * [Interstitialad. AdReady](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.adready) 이벤트가 발생 하지 않는 한 옵션을 포함 하지 마세요.

    * 앱에 실제 ad와 동일한 혜택을 제공 하는 기본 제공 환경이 포함 되어 있어야 합니다.

* 사용자가 다중 플레이어 게임에서 경쟁력 있는 이점을 얻을 수 있도록 하려면 중간 광고를 사용 하지 마세요. 예를 들어 중간 광고를 확인 하는 경우 사용자가 첫 번째 사용자 슈팅 게임에서 더 나은를 사용 하도록 유도 하지 않습니다. 플레이어의 아바타에 게는 camouflage를 제공 하지 않는 사용자 지정 셔츠가 있습니다.

### <a name="examples-of-policy-violations"></a>정책 위반의 예

이 섹션에서는 Microsoft Store 정책의 [정책 10.10.1](/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) 를 위반 하는 중간 광고 시나리오의 예를 제공 합니다. 이러한 예제는 정책을 이해 하는 데 도움이 되는 방법 으로만 교육용 으로만 제공 됩니다. 이러한 예제는 포괄적이 지 않으며 여기에 나열 되지 않은 정책 10.10.1를 위반 하는 다른 여러 가지 방법이 있을 수 있습니다.

* 중간 ad 컨테이너에 UI 요소 배치

* [Interstitialad](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show) 를 호출 하 고 있습니다. 사용자가 앱을 사용 하는 동안 표시 됩니다.

* 중간 광고를 사용 하 여 다른 사용자와 통화 또는 거래로 사용 될 수 있는 모든 항목을 가져옵니다.

* [Interstitialad](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.erroroccurred) 에 대 한 이벤트 처리기의 컨텍스트에서 새 중간 광고를 요청 합니다. 이로 인해 무한 루프가 발생 하 여 광고 서비스에 대 한 운영 문제를 일으킬 수 있습니다.

* 중간 광고를 요청 하는 것은 폭포 광고 시퀀스에 대 한 백업 ad만 있으면 됩니다. 중간 광고를 요청 하 고 [Interstitialad. AdReady](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.adready) 이벤트를 수신 하는 경우 앱에 표시 되는 다음 중간 Ad가 [Interstitialad. Show](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show) 메서드를 통해 표시 될 준비가 된 ad 여야 합니다.

* 개발 및 테스트 중 또는 에뮬레이터에서 live ad 단위 (파트너 센터에서 가져오는 ad 단위)를 사용 합니다.

* 응용 프로그램의 컨텍스트에서 실행 되는 Microsoft 광고 라이브러리 이외의 수단을 통해 광고 서비스를 호출 하는 코드를 작성 하거나 배포 합니다.

* **웹 보기** 또는 **MediaElement**와 같이 Microsoft 광고 라이브러리에서 만든 문서화 되지 않은 인터페이스 또는 자식 개체와 상호 작용

## <a name="guidelines-for-native-ads"></a>네이티브 광고에 대 한 지침

[기본 광고](native-ads.md) 를 통해 사용자에 게 광고 콘텐츠를 제공 하는 방법을 제어할 수 있습니다. 이러한 요구 사항 및 지침에 따라 사용자에 게 혼란 스 럽 고 기본적인 광고 환경을 제공 하지 않고 광고주의 메시지가 사용자에 게 도달 하도록 합니다.

### <a name="register-the-container-for-your-native-ad"></a>네이티브 ad에 대 한 컨테이너 등록

코드에서 [NativeAdV2](/uwp/api/microsoft.advertising.winrt.ui.nativeadv2) 개체의 [registeradcontainer](/uwp/api/microsoft.advertising.winrt.ui.nativeadv2.registeradcontainer) 메서드를 호출 하 여 기본 광고의 컨테이너 역할을 하는 UI 요소를 등록 하 고 필요에 따라 광고에 대해 클릭 가능한 대상으로 등록 하려는 특정 컨트롤을 등록 해야 합니다. 광고 노출 및 클릭을 적절히 추적 하는 데 필요 합니다.

사용할 수 있는 **Registeradcontainer** 메서드에는 두 가지 오버 로드가 있습니다.

* 모든 개별 네이티브 ad 요소의 전체 컨테이너를 클릭 가능 하 게 하려면 **Registeradcontainer (FrameworkElement)** 메서드를 호출 하 고 컨테이너 컨트롤을 메서드에 전달 합니다. 예를 들어 모든 기본 ad 요소를 **StackPanel** 에 호스팅된 별도의 컨트롤에 표시 하 고 전체 **StackPanel** 을 클릭 가능 하 게 하려면 **stackpanel** 을이 메서드에 전달 합니다.

* 특정 네이티브 ad 요소만 클릭할 수 있도록 하려면 **Registeradcontainer (frameworkelement, IVector (frameworkelement))** 메서드를 호출 합니다. 두 번째 매개 변수에 전달 하는 컨트롤만 클릭할 수 있습니다.

### <a name="required-native-ad-elements"></a>필수 네이티브 ad 요소

최소한 [NativeAdV2](/uwp/api/microsoft.advertising.winrt.ui.nativeadv2) 개체의 속성에서 제공 하는 다음 네이티브 ad 요소를 네이티브 ad 디자인의 사용자에 게 항상 표시 해야 합니다. 이러한 요소를 포함 하지 않으면 ad 단위에 대 한 성능이 저하 되 고 낮은 결과가 표시 될 수 있습니다.

1. 항상 기본 광고 ( **title** 속성에서 사용 가능)의 제목을 표시 합니다. 25 자 이상을 표시할 수 있는 충분 한 공간을 제공 합니다. 제목이 더 길면 추가 텍스트를 줄임표로 바꿉니다.
2. 응용 프로그램의 나머지 부분과 네이티브 ad 환경을 구분 하는 데 도움이 되도록 다음 요소 중 하나를 항상 표시 하 고, 광고주에서 콘텐츠를 제공 한다는 것을 명확 하 게 호출 합니다.
    * **Adicon** 속성에서 사용할 수 있는 구별 가능한 *ad* 아이콘입니다. 이 아이콘은 Microsoft에서 제공 합니다.
    * **SponsoredBy** 속성에서 사용할 수 있는 지원 *되* 는 텍스트입니다. 이 텍스트는 광고주에서 제공 합니다.
    * 텍스트에 대 한 대 안으로, 앱의 나머지 부분 (예: "후원 콘텐츠", "판촉 콘텐츠", "추천 콘텐츠" 등)에서 기본 광고 환경을 구분 하는 데 도움이 되는 *다른 텍스트를* 표시 하도록 선택할 수 있습니다.

### <a name="user-experience"></a>사용자 환경

기본 광고는 앱의 나머지 부분에서 명확 하 게 구분 해야 하며 실수로 클릭 하지 않도록 하기 위한 공간을 확보 해야 합니다. 테두리, 다른 배경 또는 다른 UI를 사용 하 여 응용 프로그램의 나머지 부분과 광고 콘텐츠를 분리 합니다. 실수로 광고를 클릭 하는 것은 장기적으로 광고 기반 수익과 최종 사용자 환경에 유용 하지 않습니다.

### <a name="description"></a>Description

**NativeAdV2** 개체의 **description** 속성에서 사용할 수 있는 ad에 대 한 설명을 표시 하도록 선택 하는 경우 최소 75 자를 표시 하는 데 충분 한 공간을 제공 합니다. 애니메이션을 사용 하 여 광고 설명의 전체 콘텐츠를 표시 하는 것이 좋습니다.

### <a name="call-to-action"></a>조치 사항

*작업 텍스트에 대 한 호출* ( **NativeAdV2** 개체의 **calltoaction** 속성에서 사용 가능)은 광고의 중요 한 구성 요소입니다. 이 텍스트를 표시 하도록 선택 하는 경우 다음 지침을 따르세요.

* 단추 또는 하이퍼링크와 같은 클릭 가능한 컨트롤에 대해 사용자에 게 *작업 텍스트에* 대 한 호출을 항상 표시 합니다.
* *작업 텍스트에 대 한 호출은* 항상 전체로 표시 합니다.
* *작업 텍스트에 대 한 호출이* 광고주에서 프로 모션 텍스트의 나머지 부분과 구분 되는지 확인 합니다.