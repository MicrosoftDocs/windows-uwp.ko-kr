---
Description: 응용 프로그램에서 흐름 동작의 기본을 함께 제공 하는 방법에 대해 알아봅니다.
title: 실제 동작-Windows 앱의 애니메이션
label: Motion in practice
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 45ab6c593b9e20f778e4b352a8b284cefe57c9a8
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970328"
---
# <a name="bringing-it-together"></a>함께 가져오기

타이밍, 감속/방향성 및 무게는 함께 작동 하 여 흐름 동작의 기반을 형성 합니다. 각는 다른 항목의 컨텍스트에서 고려 되 고 앱의 컨텍스트에서 적절 하 게 적용 되어야 합니다.

앱에서 흐름 동작 기본 사항을 적용 하는 세 가지 방법은 다음과 같습니다.

:::row:::
    :::column:::
**암시적 애니메이션** 표준화 된 값을 사용 하 여 매우 간단한 흐름 동작을 수행 하기 위해 매개 변수의 값 사이에 자동 트윈 및 시간이 변경 되었습니다.
    :::column-end:::
    :::column:::
**기본 제공 애니메이션** 시스템 구성 요소 (예: 공용 컨트롤 및 공유 동작)는 "기본적으로 흐름"입니다. 기본은 암시 된 사용량과 일관 된 방식으로 적용 됩니다.
    :::column-end:::
    :::column:::
**지침 권장 사항에 따라 사용자 지정 애니메이션** 시스템에서 시나리오에 대 한 정확한 동작 솔루션을 아직 제공 하지 않는 경우가 있을 수 있습니다. 이러한 경우 기본 권장 사항을 사용 환경에 대 한 시작 지점으로 사용 합니다.
    :::column-end:::
:::row-end:::

**전환 예제**

![함수 애니메이션](images/pageRefresh.gif)

:::row:::
    :::column:::
<b>방향 전달:</b><br>
페이드 아웃: 150m; 감속/가속: 기본 <b>에서 방향이 앞</b> 으로 빨라집니다.<br>
위로 밀기 150px: 300ms; 감속/가속: 기본 감속
    :::column-end:::
    :::column:::
<b>역방향 방향:</b><br>
Slide down 150px: 150ms; 감속/가속: 기본 <b>방향으로 뒤로</b> 이동 합니다.<br>
페이드 인: 300ms; 감속/가속: 기본 감속
    :::column-end:::
:::row-end:::

**개체 예**

 ![300ms 동작](images/control.gif)

:::row:::
    :::column:::
<b>방향 확장:</b><br>
증가: 300ms; 감속/가속: 표준
    :::column-end:::
    :::column:::
<b>방향 계약:</b><br>
증가: 150ms; 감속/가속: 기본 가속화
    :::column-end:::
:::row-end:::

## <a name="examples"></a>예

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치 되어 있는 경우 여기를 클릭 하 여 <a href="xamlcontrolsgallery:/item/ImplicitTransition">앱을 열고 동작의 암시적 전환을 확인</a>하세요.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="implicit-animations"></a>암시적 애니메이션

> 암시적 애니메이션에는 Windows 10, 버전 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 이상이 필요 합니다.

암시적 애니메이션은 매개 변수를 변경 하는 동안 이전 값과 새 값 사이를 자동으로 보간 하 여 흐름 동작을 달성할 수 있는 간단한 방법입니다.

다음 속성에 대 한 변경 내용에 암시적으로 애니메이션 효과를 적용할 수 있습니다.

- [UIElement](/uwp/api/windows.ui.xaml.uielement)
  - **불투명도**
  - **회전**
  - **크기 조정**
  - **Translation**

- [Border](/uwp/api/windows.ui.xaml.controls.border), [ContentPresenter](/uwp/api/windows.ui.xaml.controls.contentpresenter)또는 [Panel](/uwp/api/windows.ui.xaml.controls.panel)
  - **배경**

변경 내용이 암시적으로 적용 될 수 있는 각 속성에는 해당 하는 _전환_ 속성이 있습니다. 속성에 애니메이션 효과를 주려면 해당 _전환_ 속성에 전환 유형을 할당 합니다. 다음 표에서는 _전환_ 속성과 각 속성에 사용할 전환 형식을 보여 줍니다.

| 애니메이션 속성 | Transition 속성 | 암시적 전환 유형 |
| -- | -- | -- |
| [UIElement 불투명도](/uwp/api/windows.ui.xaml.uielement.opacity) | [OpacityTransition](/uwp/api/windows.ui.xaml.uielement.opacitytransition) | [ScalarTransition](/uwp/api/windows.ui.xaml.scalartransition) |
| [UIElement.Rotation](/uwp/api/windows.ui.xaml.uielement.rotation) | [RotationTransition](/uwp/api/windows.ui.xaml.uielement.rotationtransition) | [ScalarTransition](/uwp/api/windows.ui.xaml.scalartransition) |
| [UIElement.Scale](/uwp/api/windows.ui.xaml.uielement.scale) | [ScaleTransition](/uwp/api/windows.ui.xaml.uielement.scaletransition) | [Vector3Transition](/uwp/api/windows.ui.xaml.vector3transition) |
| [UIElement.Translation](/uwp/api/windows.ui.xaml.uielement.translation) | [TranslationTransition](/uwp/api/windows.ui.xaml.uielement.translationtransition) | [Vector3Transition](/uwp/api/windows.ui.xaml.vector3transition) |
| [테두리. 배경](/uwp/api/windows.ui.xaml.controls.border.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.border.backgroundtransition) | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |
| [ContentPresenter](/uwp/api/windows.ui.xaml.controls.contentpresenter.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.contentpresenter.backgroundtransition) | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |
| [Panel. 배경](/uwp/api/windows.ui.xaml.controls.panel.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.panel.backgroundtransition)  | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |

이 예제에서는 불투명도 속성 및 전환을 사용 하 여 컨트롤이 활성화 될 때 단추를 페이드 아웃 하 고 비활성화 된 경우 페이드 아웃 하는 방법을 보여 줍니다.

```xaml
<Button x:Name="SubmitButton"
        Content="Submit"
        Opacity="{x:Bind OpaqueIfEnabled(SubmitButton.IsEnabled), Mode=OneWay}">
    <Button.OpacityTransition>
        <ScalarTransition />
    </Button.OpacityTransition>
</Button>
```

```csharp
public double OpaqueIfEnabled(bool IsEnabled)
{
    return IsEnabled ? 1.0 : 0.2;
}
```

## <a name="related-articles"></a>관련된 문서

- [동작 개요](index.md)
- [타이밍 및 감속](timing-and-easing.md)
- [방향 및 무게](directionality-and-gravity.md)
