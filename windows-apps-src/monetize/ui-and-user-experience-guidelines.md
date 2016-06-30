---
author: mcleanbyron
ms.assetid: 7a38a352-6e54-4949-87b1-992395a959fd
description: "앱에 광고에 대한 UI 및 사용자 환경 지침을 알아봅니다."
title: "앱에 광고에 대한 UI 및 사용자 환경 지침"
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 87be9f0f0a19094085d5c4ecbcfb8d40ceb20d2a


---

# 앱에 광고에 대한 UI 및 사용자 환경 지침


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

## Windows 앱용 일반 UI 리소스

[디자인 및 UI](https://developer.microsoft.com/windows/design)에서 앱의 모양과 느낌을 디자인하는 방법에 대한 정보를 찾을 수 있습니다.

## AdControl 모범 사례

* [AdControl 모범 사례: 할 일](#adcontrolbestpracticesdo10)
* [AdControl 모범 사례: 하지 않을 일](#adcontrolbestpracticesdont10)

<span id="adcontrolbestpracticesdo10"/>
### AdControl 모범 사례: 할 일

* 사용 환경에 맞게 광고를 디자인합니다. 디자이너에게 광고 형태를 계획하기 위한 샘플 광고를 제공합니다. 앱의 잘 계획된 광고 예제 2가지는 콘텐츠 형태 광고 레이아웃 및 분할 레이아웃입니다.

  다양한 크기의 광고가 앱 내에서 표시되고 작동하는 방식을 보려면 Windows Phone, Windows 8.1 및 Windows 10용 테스트 모드 광고 단위를 활용할 수 있습니다. 테스트 모드 광고 단위 사용을 완료한 경우 인증을 위해 앱을 제출하기 전에 [실제 광고 단위 ID로 앱을 업데이트](set-up-ad-units-in-your-app.md)해야 합니다.

* 광고를 사용할 수 없는 시간을 계획합니다. 광고가 앱으로 전송되지 않는 시간이 있을 수 있습니다. 페이지에 광고가 표시되는지 여부에 따라 페이지가 돋보이는 방식으로 레이아웃을 지정합니다. 자세한 내용은 [오류 처리](error-handling-with-advertising-libraries.md)를 참조하세요.

<span id="adcontrolbestpracticesdont10"/>
### AdControl 모범 사례: 하지 않을 일

* 열린 공간에 광고를 고정하지 않습니다. 광고 공간을 찾을 수 있는 첫 번째 열린 공간에 배치하면 안 됩니다. 대신, 앱의 전체 디자인에 통합되어야 합니다.

* 지나치게 광고하여 광고로 앱을 포화시키지 않습니다. 앱에 광고가 너무 많으면 모양이 정신 없고 유용성이 떨어집니다. 광고로 수익을 창출할 수 있지만 앱 자체를 희생해서는 안 됩니다.

* 사용자가 핵심 작업이 아닌 다른 부분에 정신이 팔리게 하면 안 됩니다. 일차적인 포커스는 항상 앱에 있어야 합니다. 광고 공간은 이차적인 포커스가 유지되도록 통합해야 합니다.

<span id="interstitialbestpractices10"/>
## 중간 광고 모범 사례

* [중간 광고 모범 사례: 할 일](#interstitialbestpracticesdo10)
* [중간 광고 모범 사례: 피할 일](#interstitialbestpracticesavoid10)
* [중간 광고 모범 사례: 절대 금지(정책 적용)](#interstitialbestpracticesnever10)

과하지 않게 사용될 경우 동영상 중간 광고는 사용자 만족도에 부정적인 영향을 주지 않으면서 앱 수익을 크게 높일 수 있습니다. 그렇지만 잘못 사용하면 이러한 광고는 정반대의 효과를 가져올 수 있습니다.

과하지 않은 수준을 유지하는 것이 중요합니다. 정책이 관련되는 경우를 제외하고, 사용자가 어느 누구보다 자신의 앱에 대해 잘 알고 있으므로 가장 적절한 결정은 본인이 내리는 것이 좋습니다. 그렇지만 앱 등급 및 수익이 긴밀하게 결합되어 있다는 사실을 명심해야 합니다.

<span id="interstitialbestpracticesdo10"/>
### 중간 광고 모범 사례: 할 일

* 게임 레벨 중간과 같이 앱의 자연스러운 흐름 도중에 중간 광고를 표시합니다.

* 광고를 다음과 같은 구체적인 혜택과 연결합니다.

    * 레벨 완료를 위한 힌트

    * 레벨을 다시 시도하기 위한 추가 시간

    * 사용자 지정 아바타 특징(예: 문신 또는 모자)

* 앱이 완료를 위해 동영상 광고를 시청하도록 요구하는 경우 닫기 단추를 누를 때 오류 메시지가 나타나도 놀라지 않도록 해당 규칙을 미리 언급합니다.

* 이상적으로 광고를 표시해야 하는 시간보다 30-60초 전쯤에 광고를 미리 가져옵니다([RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) 호출).

* [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 클래스에 노출된 4개의 모든 이벤트(**Canceled**, **Completed**, **AdReady** 및 **ErrorOccurred**)를 구독한 후 앱에 적절한 결정을 내리는 데 사용합니다.

* 서버 일치 광고 대신 일부 기본 제공 환경이 사용되도록 합니다. 이렇게 하면 다음과 같은 시나리오에서 유용할 수 있습니다.

    * 광고 서버에 연결할 수 없는 오프라인 모드

    * **ErrorOccurred** 이벤트가 발생하는 경우

    * [ConnectionProfile](https://msdn.microsoft.com/library/windows/apps/windows.networking.connectivity.connectionprofile.aspx)을 기준으로 사용자 대역폭을 저장하도록 선택하는 경우 **ConnectionProfile** 클래스에 도움이 될 수 있는 API가 있습니다.

* 10초 이내에 이동할 수 없는 경우와 같은 타당한 이유가 있는 경우가 아니면 기본(30초) 시간 제한을 사용합니다.

    * 동영상 광고는 특히, 고속 연결이 제공되지 않는 시장에서 배너보다 다운로드하는 데 더 오래 걸립니다.


* 사용자의 데이터 요금제를 염두에 둡니다. 예를 들어 데이터 제한에 가까워졌거나 제한을 초과하는 모바일 디바이스에서는 동영상 광고를 표시하지 않거나 광고를 제공하기 전에 사용자에게 경고를 합니다. [ConnectionProfile](https://msdn.microsoft.com/library/windows/apps/windows.networking.connectivity.connectionprofile.aspx) 클래스에 도움이 될 수 있는 API가 있습니다.

* 초기 전송 후 앱을 지속적으로 향상시킵니다. 광고 보고서를 살펴보고 채우기 및 동영상 완료 속도 향상에 도움이 되도록 디자인을 변경합니다.

<span id="interstitialbestpracticesavoid10"/>
### 중간 광고 모범 사례: 피할 일

* 너무 많은 광고 사용을 피함. 사용자가 게임을 하는 것 외에 분명하게 선택적인 구체적 혜택을 받는 경우가 아니면 5분 이상마다 광고를 제공하지 않도록 합니다.

* 사용자가 잘못된 타일을 클릭한 것으로 오인할 수 있으므로 앱을 시작할 때 동영상 중간 광고를 사용하지 않음

* 종료할 때 동영상 중간 광고를 표시하지 않음. 완료 속도가 거의 제로에 가까워지므로 잘못된 인벤토리입니다.

    * 두 개 이상의 동영상 광고를 연속으로 표시하지 않음

    * 진행률 표시줄이 시작점으로 다시 설정되게 되어 사용자에게 혼란을 줍니다.

    * 많은 사람들이 코딩 버그나 광고 제공 버그 정도로 생각합니다.

* 동영상 광고를 가져오고 5분 이상 경과한 후 [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx) 호출

    * 좋은 인벤토리는 미리 가져온 광고가 청구 가능한 광고 노출로 최대한 많이 변환되도록 합니다.


* 광고 제공이 실패할 경우 광고를 사용할 수 없는 것과 같은 벌칙 부과. 예를 들어 "광고를 시청하여 *xxx* 받기" UI 옵션을 표시하는 경우 사용자가 자신의 역할을 해낼 경우 *xxx*를 제공해야 합니다. 다음 두 가지 옵션을 고려해야 합니다.

    * [AdReady](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.adready.aspx) 이벤트가 발생하지 않는 한, 이 옵션을 포함하지 마세요.

    * 앱에 실제 광고와 동일한 이점을 가져오는 기본 제공 환경을 포함합니다.

* 멀티 플레이어 게임을 하는 사용자에게 경쟁적 우위 제공

    * 1인칭 슈팅 게임에서 더 나은 총이 이 버킷에 제공됩니다.

    * 플레이어의 아바타에 적용된 사용자 지정 셔츠는 위장 효과를 제공하지 않기만 하면 괜찮습니다.

<span id="interstitialbestpracticesnever10"/>
### 중간 광고 모범 사례: 절대 금지(정책 적용)

* 광고 컨테이너에 모든 UI 요소를 배치합니다.

    * 광고주는 전체 화면에 대해 비용을 지불했습니다.


* 사용자가 앱에 연결된 동안 **Show**를 호출합니다.

    * **InterstitialAd**는 전체 화면 오버레이를 만들기 때문에 사용자에게 거슬릴 수 있습니다.

    * 또한 광고 클릭률이 부풀려질 수 있습니다.

* 광고를 사용하여 통화로 사용 가능하거나 다른 사용자와 거래할 수 있는 항목을 받을 수 있습니다.

 

 



<!--HONumber=Jun16_HO4-->


