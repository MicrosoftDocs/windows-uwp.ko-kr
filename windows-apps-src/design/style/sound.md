---
Description: Sound helps complete an application's user experience, and gives them that extra audio edge they need to match the feel of Windows across all platforms.
label: Sound
title: 소리
template: detail.hbs
ms.assetid: 9fa77494-2525-4491-8f26-dc733b6a18f6
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: mattben
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 74d1d5b04b13795a075e7111ed898243ed59e7b7
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8334096"
---
# <a name="sound"></a>소리

![영웅 이미지](images/header-sound.svg)

소리를 사용하여 앱을 향상시키는 여러 가지 방법이 있습니다. 사용자가 이벤트를 경고음으로 인식할 수 있도록 소리를 사용하여 다른 UI 요소를 보완할 수 있습니다. 소리는 시각 장애가 있는 사용자에게 효과적인 사용자 인터페이스 요소일 수 있습니다. 소리를 사용하여 사용자를 몰입시키는 분위기를 만들 수 있습니다. 예를 들어 퍼즐 게임의 백그라운드에서 기발한 사운드트랙을 재생하거나 공포/서바이벌 게임에 음울한 사운드 효과를 사용할 수 있습니다.

## <a name="sound-global-api"></a>소리 전역 API

UWP는 단순히 "스위치를 플리핑"하여 전체 앱에서 몰입형 오디오 환경을 얻을 수 있는, 쉽게 액세스할 수 있는 사운드 시스템을 제공합니다.

[**ElementSoundPlayer**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.elementsoundplayer)는 XAML 내의 통합 사운드 시스템이며, 켤 경우 모든 기본 컨트롤이 자동으로 소리를 재생합니다.
```C#
ElementSoundPlayer.State = ElementSoundPlayerState.On;
```
**ElementSoundPlayer**에는 **켜짐**, **꺼짐** 및 **자동**의 세 가지 상태가 있습니다.

**꺼짐**으로 설정된 경우 앱을 실행하는 위치에 관계없이 소리가 재생되지 않습니다. **켜짐**으로 설정된 경우 앱의 소리가 모든 플랫폼에서 재생됩니다.

Enabling ElementSoundPlayer는 공간 오디오(3D 사운드)도 자동으로 활성화합니다. 소리를 켜 놓은 상태에서 3D 사운드를 비활성화하려면 ElementSoundPlayer의 **SpatialAudioMode**를 비활성화합니다. 

```C#
ElementSoundPlayer.SpatialAudioMode = ElementSpatialAudioMode.Off
```

**SpatialAudioMode** 속성은 다음 값을 취할 수 있습니다. 
- **Auto**: 소리가 켜져 있으면 공간 오디오를 켭니다. 
- **Off**: 소리가 켜져 있더라도 공간 오디오를 항상 끕니다.
- **On**: 공간 오디오를 항상 재생합니다.

공간 오디오 및 XAML 처리 방법에 대한 자세한 내용은 [AudioGraph - 공간 오디오](/windows/uwp/audio-video-camera/audio-graphs#spatial-audio)를 참조하세요.

### <a name="sound-for-tv-and-xbox"></a>TV 및 Xbox의 소리

소리는 10피트 환경의 주요 부분이며, **ElementSoundPlayer**의 상태는 기본적으로 **자동**이므로 Xbox에서 앱을 실행하는 경우에만 소리가 재생됩니다.
Xbox 및 TV용 디자인 방식에 대한 자세한 내용은 [Xbox 및 TV용 디자인](http://go.microsoft.com/fwlink/?LinkId=760736)을 참조하세요.

## <a name="sound-volume-override"></a>소리 볼륨 재정의

**볼륨** 컨트롤을 사용하여 앱 내의 모든 소리를 낮출 수 있습니다. 그러나 앱 내의 소리는 *시스템 볼륨보다 더 클* 수 없습니다.

앱 볼륨 수준을 설정하려면 다음을 호출합니다.
```C#
ElementSoundPlayer.Volume = 0.5;
```
여기서 최대 볼륨(시스템 볼륨 기준)은 1.0이고 최소 볼륨은 0.0(기본적으로 무음)입니다.

## <a name="control-level-state"></a>컨트롤 수준 상태

컨트롤의 기본 소리가 부적절한 경우 사용하지 않도록 설정할 수 있습니다. 이 작업은 컨트롤의 **ElementSoundMode**를 통해 수행합니다.

**ElementSoundMode**에는 **꺼짐**과 **기본값**의 두 가지 상태가 있습니다. 설정하지 않으면 **기본값**입니다. **꺼짐**으로 설정하면 *포커스를 제외*하고 컨트롤이 재생하는 모든 소리가 음소거됩니다.

```XAML
<Button Name="ButtonName" Content="More Info" ElementSoundMode="Off"/>
```

```C#
ButtonName.ElementSoundState = ElementSoundMode.Off;
```

## <a name="is-this-the-right-sound"></a>올바른 소리인가요?

사용자 지정 컨트롤을 만들거나 기존 컨트롤의 소리를 변경하는 경우 시스템에서 제공하는 모든 소리의 사용을 이해하는 것이 중요합니다.

각 소리는 특정 기본 사용자 조작과 관련이 있으며, 임의 조작 시 재생되도록 소리를 사용자 지정할 수 있지만 이 섹션은 모든 UWP 앱에서 일관된 환경을 유지하도록 소리를 사용해야 하는 시나리오를 설명합니다.

### <a name="invoking-an-element"></a>요소 호출

오늘날 컨트롤에 의해 트리거되는 가장 일반적인 시스템 소리는 **Invoke** 소리입니다. 이 소리는 사용자가 탭/클릭/Enter/스페이스 또는 게임 패드의 'A' 단추 누르기를 통해 컨트롤을 호출할 때 재생됩니다.

일반적으로 이 소리는 사용자가 [입력 장치](../input/index.md)를 통해 단순 컨트롤이나 컨트롤 부분의 대상을 명시적으로 지정하는 경우에만 재생됩니다.

&lt;여기에 SelectButtonClick.mp3 사운드 클립 포함&gt;

컨트롤 이벤트에서 이 소리를 재생하려면 **ElementSoundPlayer**에서 Play 메서드를 호출하고 **ElementSound.Invoke**를 전달합니다.
```C#
ElementSoundPlayer.Play(ElementSoundKind.Invoke);
```

### <a name="showing--hiding-content"></a>콘텐츠 표시 및 숨기기

XAML에는 많은 플라이아웃, 대화 상자 및 해제 가능한 UI가 있으며, 이러한 오버레이 중 하나를 트리거하는 작업은 **Show** 또는 **Hide** 소리를 호출해야 합니다.

오버레이 콘텐츠 창이 표시되는 경우 **Show** 소리를 호출해야 합니다.

&lt;여기에 OverlayIn.mp3 사운드 클립 포함&gt;

```C#
ElementSoundPlayer.Play(ElementSoundKind.Show);
```
반대로, 오버레이 콘텐츠 창이 닫히거나 빠른 해제되는 경우 **숨기기** 소리를 호출해야 합니다.

&lt;여기에 OverlayOut.mp3 사운드 클립 포함&gt;

```C#
ElementSoundPlayer.Play(ElementSoundKind.Hide);
```
### <a name="navigation-within-a-page"></a>페이지 내의 탐색

앱 페이지 내의 패널 또는 보기 간에 탐색하는 경우([허브](../controls-and-patterns/hub.md) 또는 [탭 및 피벗](../controls-and-patterns/tabs-pivot.md) 참조) 일반적으로 양방향 이동이 있습니다. 따라서 현재 앱 페이지를 나가지 않고도 다음 보기/패널 또는 이전 보기/패널로 이동할 수 있습니다.

이 탐색 개념을 중심으로 하는 오디오 환경은 **MovePrevious** 및 **MoveNext** 소리로 나타냅니다.

목록의 *다음 항목*으로 간주되는 보기/패널로 이동하는 경우 다음을 호출합니다.

&lt;여기에 PageTransitionRight.mp3 사운드 클립 포함&gt;1

```C#
ElementSoundPlayer.Play(ElementSoundKind.MoveNext);
```
또한 목록에서 *이전 항목*으로 간주되는 이전 보기/패널로 이동하는 경우 다음을 호출합니다.

&lt;여기에 PageTransitionLeft.mp3 사운드 클립 포함&gt;1

```C#
ElementSoundPlayer.Play(ElementSoundKind.MovePrevious);
```
### <a name="back-navigation"></a>뒤로 탐색

현재 페이지에서 앱 내의 이전 페이지로 이동하는 경우 **GoBack** 소리를 호출해야 합니다.

&lt;여기에 BackButtonClick.mp3 사운드 클립 포함&gt;

```C#
ElementSoundPlayer.Play(ElementSoundKind.GoBack);
```
### <a name="focusing-on-an-element"></a>요소에 집중

**Focus** 소리는 시스템에서 유일한 암시적 소리입니다. 따라서 사용자가 아무것도 직접 조작하지 않아도 소리가 들립니다.

사용자가 앱을 탐색할 때 포커스 지정이 발생하며, 게임 패드/키보드/리모컨 또는 Kinect를 사용할 수 있습니다. 일반적으로 **Focus** 소리는 *PointerEntered 또는 마우스 가리키기 이벤트 시에는 재생되지 않습니다*.

컨트롤이 포커스를 받을 때 **Focus** 소리를 재생하도록 컨트롤을 설정하려면 다음을 호출합니다.

&lt;여기에 ElementFocus1.mp3 사운드 클립 포함&gt;

```C#
ElementSoundPlayer.Play(ElementSoundKind.Focus);
```
### <a name="cycling-focus-sounds"></a>포커스 소리 순환

**ElementSound.Focus** 호출에 추가되는 기능으로, 사운드 시스템은 기본적으로 각 탐색 트리거 시 4가지 다른 소리를 순환합니다. 따라서 동일한 포커스 소리가 두 번 연속해서 재생되지 않습니다.

이 순환 기능의 목적은 포커스 소리가 단조롭게 반복되고 사용자가 알아차릴 수 없게 하기 위한 것입니다. 포커스 소리는 가장 자주 재생되므로 가장 감지하기 힘든 소리여야 합니다.

## <a name="related-articles"></a>관련 문서

* [Xbox 및 TV용 디자인](http://go.microsoft.com/fwlink/?LinkId=760736)
* [ElementSoundPlayer 클래스 설명서](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.elementsoundplayer)
