---
description: "특정 유형의 입력 및 디바이스에 대해 UWP 앱을 사용자 지정합니다. 터치 및 음성 명령을 활용합니다. Xbox, 전화 및 TV에서도 앱을 실행합니다."
title: "UWP 앱 입력 및 디바이스 디자인 - Windows 앱 개발"
author: mijacobs
keywords: "디바이스 입문서, 앱 입력, UWP 응용 프로그램 사용자 지정"
translationtype: Human Translation
ms.sourcegitcommit: e3eb6d7cf1c8aa045b2a89b4e20827daec07680c
ms.openlocfilehash: e975eec0af37915a848e757638d32d73413fd3ca

---
# 입력 및 디바이스

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

UWP 앱은 다양한 입력을 자동으로 처리하고 다양한 디바이스에서 실행됩니다. 터치 입력을 사용하거나 휴대폰에서 앱을 실행하기 위해 수행해야 할 추가 작업이 없습니다.

그러나 특정 유형의 입력 또는 디바이스에 대해 앱을 최적화해야 하는 경우는 있습니다. 예를 들어 그리기 앱을 만드는 경우 펜 입력을 처리하는 방식을 사용자 지정할 수 있습니다.

이 섹션의 디자인 및 코딩 지침은 특정 유형의 입력 및 디바이스에 대해 UWP 앱을 사용자 지정하는 데 도움이 됩니다.

## 입력 지침서

특정 폼 팩터와 짝을 이룰 경우 각 입력 디바이스 유형 및 동작, 기능 및 제한 사항을 알아보려면 <b>[입력 지침서](input-primer.md)</b>를 참조하세요.

## 입력 및 조작

<div class="side-by-side">
<div class="side-by-side-content">
<p>
<b>[Surface Dial](windows-wheel-interactions.md)</b><br/>
이 새로운 범주의 입력 디바이스를 Windows 앱에 통합하는 방법을 알아봅니다.</br>
이 디바이스는 보조 다중 모달 입력 디바이스로 기본 디바이스의 입력을 보완하거나 수정하도록 고안되었습니다.
</p>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p>
<b>[Cortana](cortana-interactions.md)</b><br/>
외부 응용 프로그램을 시작하고 해당 응용 프로그램 내에서 단일 작업을 실행하는 음성 명령으로 Cortana의 기본 기능을 확장하세요.
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[음성](speech-interactions.md)</b><br/>
음성 인식과 텍스트 음성 변환(TTS 또는 음성 합성이라고도 함)을 앱의 사용자 환경에 직접 통합합니다.
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p>
<b>[펜](pen-and-stylus-interactions.md)</b><br/>
UWP 앱을 최적화하여 펜 입력을 통해 사용자에게 표준 포인터 디바이스 기능과 최적의 Windows Ink 환경을 제공하세요.
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[Keyboard](keyboard-interactions.md)</b><br/>
키보드 입력은 앱용 전체 사용자 조작 환경에서 중요한 부분을 차지합니다. 키보드가 앱을 조작하는 보다 효율적인 방법이라고 생각하는 사용자나 특정 장애를 가진 사람들에게는 키보드가 필수 도구입니다.
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p>
<b>[터치](touch-interactions.md)</b><br/>
UWP에는 터치식 입력을 처리하는 여러 다양한 메커니즘이 있으며 모든 메커니즘을 통해 사용자가 안심하고 이용할 수 있는 몰입형 작업 환경을 만들 수 있도록 합니다.
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[터치 패드](touchpad-interactions.md)</b><br/>
터치 패드는 간접 멀티 터치 입력을 마우스와 같은 포인팅 장치의 정밀도 입력과 결합합니다. 이러한 결합을 통해 터치 패드는 터치 최적화된 UI와 생산성 앱의 작은 대상에 모두 적합합니다.
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p>
<b>[마우스](mouse-interactions.md)</b><br/>
마우스 입력은 가리키고 클릭할 때 정밀도가 필요한 사용자 조작에 가장 적합합니다. 이러한 고유 정밀도는 터치의 부정확한 특성에 최적화된 Windows의 UI에서 자연스럽게 지원됩니다.
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[게임 패드 및 리모컨](gamepad-and-remote-interactions.md)</b><br/>
이제 UWP 앱에서 게임 패드 및 원격 제어 입력을 지원합니다. 게임 패드 및 원격 제어는 Xbox 및 TV 환경에 대한 기본 입력 디바이스입니다.
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<p>
<b>[여러 입력](multiple-input-design-guidelines.md)</b><br/>
가능한 한 많은 사용자와 디바이스를 수용하려면 가능한 한 많은 입력 유형(제스처, 음성, 터치, 터치 패드, 마우스 및 키보드)과 작동하도록 앱을 디자인하는 것이 좋습니다. 이렇게 하면 유연성, 유용성 및 접근성이 극대화됩니다.
</p>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p>
<b>[입력 디바이스 식별](identify-input-devices.md)</b><br/>
UWP(유니버설 Windows 플랫폼) 디바이스에 연결된 입력 디바이스를 식별하고 해당 기능 및 특성을 식별합니다.
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[포인터 입력 처리](handle-pointer-input.md)</b><br/>
UWP(유니버설 Windows 플랫폼) 앱에서 터치, 마우스, 펜/스타일러스, 터치 패드 등의 포인팅 디바이스에서 입력 데이터를 수신하고, 처리하고, 관리합니다.
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p><b>[사용자 지정 텍스트 입력](custom-text-input.md)</b><br/>
Windows.UI.Text.Core 네임스페이스의 코어 텍스트 API를 통해 UWP 앱은 Windows 디바이스에서 지원되는 모든 텍스트 서비스에서 텍스트 입력을 받을 수 있습니다. 이 API를 통해 앱은 모든 언어로 된 텍스트를 키보드, 음성 또는 펜과 같은 모든 입력 유형에서 받을 수 있습니다.
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[텍스트 및 이미지 선택](guidelines-for-textselection.md)</b><br/>
이 문서에서는 텍스트, 이미지 및 컨트롤을 선택하고 조작하는 방법을 설명하고 앱에서 이러한 메커니즘을 사용할 때 고려해야 할 사용자 환경 지침을 제공합니다.
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<p>
<b>[이동](guidelines-for-panning.md)</b><br/>
사용자는 이동 또는 스크롤을 통해 단일 보기 내에서 탐색하여 뷰포트 내에 맞지 않는 보기의 콘텐츠를 표시할 수 있습니다.
</p>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p>
<b>[광학 줌 및 크기 조정](guidelines-for-optical-zoom.md)</b><br/>
이 문서에서는 Windows 확대/축소 및 크기 조정 요소에 대해 설명하고, 앱에서 이러한 조작 메커니즘 사용을 위한 사용자 환경 지침을 제공합니다.
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[회전](guidelines-for-rotation.md)</b><br/>
이 문서에서는 회전하기 위한 새로운 Windows UI에 대해 설명하고 UWP 앱에서 이러한 새로운 조작 방식 메커니즘을 사용할 때 고려해야 할 사용자 환경 지침을 제공합니다.
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p><b>[대상 지정](guidelines-for-targeting.md)</b><br/>
Windows의 터치 타기팅은 터치 디지타이저로 감지되는 각 손가락의 전체 접촉 영역을 사용합니다. 사용자가 의도했거나 대상이 될 가능성 높은 대상을 결정할 때 정확도를 높이기 위해 디지타이저가 보고하는 좀 더 크고 좀 더 복잡한 입력 데이터 집합이 사용됩니다.
</p>
</div>
<div class="side-by-side-content-right">
<p><b>[시각적 피드백](guidelines-for-visualfeedback.md)</b><br/>
시각적 피드백을 사용하여 조작이 감지, 해석 및 처리될 때 사용자에게 표시할 수 있습니다. 시각적 피드백은 조작 의지를 북돋아 사용자에게 도움이 될 수 있습니다. 시각적 피드백은 조작이 성공했음을 표시하여 사용자의 제어 감각을 향상합니다. 또한 시스템 상태를 전달하고 오류를 줄여 줍니다.
</p>
</div>
</div>
</div>

## 디바이스

UWP 앱을 지원하는 디바이스에 대해 알고 있으면 각 폼 팩터에 최상의 사용자 환경을 제공하는 데 도움이 됩니다. 특정 디바이스를 디자인할 때의 주요 고려 사항은 해당 디바이스에 앱이 표시되는 방법, 해당 디바이스에서 앱이 사용되는 위치, 시간 및 방법, 사용자가 해당 디바이스를 조작하는 방법 등입니다.

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[디바이스 입문서](device-primer.md)</b><br/>UWP 앱을 지원하는 디바이스에 대해 알고 있으면 각 폼 팩터에 최상의 사용자 환경을 제공하는 데 도움이 됩니다.
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Xbox 및 TV용 디자인](designing-for-tv.md)</b><br/>Xbox One 및 TV 화면에서 멋지게 보이고 제대로 작동하도록 UWP(유니버설 Windows 플랫폼) 앱을 디자인합니다.
</p>
  </div>
</div>
</div>



<!--HONumber=Nov16_HO1-->


