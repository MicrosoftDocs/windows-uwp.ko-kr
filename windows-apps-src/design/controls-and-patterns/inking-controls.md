---
author: Karl-Bridge-Microsoft
Description: Ink tools described
title: 수동 입력 컨트롤
label: Inking Controls
template: detail.hbs
ms.author: kbridge
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 97eae5f3-c16b-4aa5-b4a1-dd892cf32ead
ms.localizationpriority: medium
ms.openlocfilehash: ed358f88470dfe1ba1c48cd3daf1ed54135ed987
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/14/2018
ms.locfileid: "6834643"
---
# <a name="inking-controls"></a>수동 입력 컨트롤



UWP(유니버설 Windows 플랫폼) 앱에서 수동 입력을 간편하게 하는 두 가지 컨트롤은 [InkCanvas](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx) 및 [InkToolbar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx)입니다.

InkCanvas 컨트롤은 펜 입력을 잉크 스트로크(색과 두께에 기본 설정 사용) 또는 지우기 스트로크로 렌더링합니다. 이 컨트롤은 기본 잉크 스트로크 속성을 변경하기 위한 기본 제공 UI를 포함하지 않는 투명 오버레이입니다.

> [!NOTE]
> 마우스 및 터치식 입력 둘 다에 대해 유사한 기능을 지원하도록 InkCanvas를 구성할 수 있습니다.

InkCanvas 컨트롤은 기본 잉크 스트로크 설정 변경 지원을 포함하지 않으므로 InkToolbar 컨트롤과 연결할 수 있습니다. InkToolbar에는 연결된 InkCanvas에서 잉크 관련 기능을 활성화하는, 사용자 지정 및 확장이 가능한 단추 컬렉션이 포함됩니다.

기본적으로 InkToolbar에는 그리기, 지우기, 강조 표시 및 눈금자 표시 단추가 포함되어 있습니다. 기능에 따라 잉크 색, 스트로크 두께, 모든 잉크 지우기 등의 기타 설정 및 명령이 플라이아웃에 제공됩니다.

> [!NOTE]
> InkToolbar는 펜 및 마우스 입력을 지원하며 터치식 입력을 인식하도록 구성할 수 있습니다.

<img src="images/ink-tools-invoked-toolbar.png" width="300" alt="InkToolbar palette flyout">

> **중요 API**: [InkCanvas 클래스](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx), [InkToolbar 클래스](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx), [InkPresenter 클래스](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkpresenter.aspx), [Windows.UI.Input.Inking](https://msdn.microsoft.com/library/windows/apps/br208524)


## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

사용자에게 잉크 설정을 제공하지 않고 앱에서 기본 수동 입력 기능을 사용하도록 설정해야 하는 경우 InkCanvas를 사용합니다.

기본적으로 펜 팁(두께가 2픽셀인 검은색 볼펜)을 사용할 때는 스트로크가 잉크로 렌더링되고 지우개 팁을 사용할 때는 지우개로 렌더링됩니다. 지우개 팁이 없는 경우 펜 팁의 입력을 지우기 스트로크로 처리하도록 InkCanvas를 구성할 수 있습니다.

InkToolbar와 InkCanvas를 연결하여 잉크 기능을 활성화하고 스트로크 크기, 색, 펜 팁 모양 등의 기본적인 잉크 속성을 설정하기 위한 UI를 제공합니다.

> [!NOTE] 
> InkCanvas에서 잉크 스트로크 렌더링을 보다 광범위하게 사용자 지정하려면 기본 [InkPresenter](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkpresenter.aspx) 개체를 사용합니다.

## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/InkCanvas">앱을 열고 작동 중인 InkCanvas를 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML 컨트롤 갤러리 앱 다운로드(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">소스 코드 다운로드(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

**Microsoft Edge**

Microsoft Edge는 **웹 노트**에서 InkCanvas 및 InkToolbar를 사용합니다.  
![InkCanvas는 Microsoft Edge에서 수동 입력에 사용됩니다.](images/ink-tools-edge.png)

**Windows Ink 작업 영역**

InkCanvas 및 InkToolbar는 **Windows Ink 작업 영역**의 **스케치북**과 **화면 스케치**에서도 사용됩니다.  
![Windows Ink 작업 영역의 InkToolbar](images/ink-tools-ink-workspace.png)

## <a name="create-an-inkcanvas-and-inktoolbar"></a>InkCanvas 및 InkToolbar 만들기

앱에 InkCanvas를 추가하려면 다음과 같은 한 줄의 태그만 있으면 됩니다.

```xaml
<InkCanvas x:Name=“myInkCanvas”/>
```

> [!NOTE]
> InkPresenter를 사용하는 자세한 InkCanvas 사용자 지정은 ["UWP 앱에서 펜 및 스타일러스 조작"](http://windowsstyleguide/input/pen-and-stylus-interactions/) 문서를 참조하세요.

InkToolbar 컨트롤은 InkCanvas와 함께 사용해야 합니다. InkToolbar(모든 기본 제공 도구 포함)를 앱에 통합하려면 다음과 같은 한 줄의 태그가 추가로 필요합니다.

 ```xaml
<InkToolbar TargetInkCanvas=“{x:Bind myInkCanvas}”/>
 ```

이 경우 다음과 같은 InkToolbar가 표시됩니다.
<img src="images/ink-tools-uninvoked-toolbar.png" width="250" alt="Basic InkToolbar">

### <a name="built-in-buttons"></a>기본 제공 단추

InkToolbar에는 다음과 같은 기본 제공 단추가 포함되어 있습니다.

**펜**

- 볼펜 - 원형 펜 팁으로 단색의 불투명 스트로크를 그립니다. 스트로크 크기는 감지된 펜 압력에 따라 달라집니다.
- 연필 - 원형 펜 팁으로 가장자리가 부드러운, 텍스처 처리된 반투명 스트로크(계층적 음영 효과에 유용)를 그립니다. 스트로크 색(어둡기)은 감지된 펜 압력에 따라 달라집니다.
- 형광펜 – 사각형 펜 팁으로 반투명 스트로크를 그립니다.

각 펜에 대한 플라이아웃에서 색상표와 크기 특성(min, max, default)을 둘 다 사용자 지정할 수 있습니다.

**도구**

- 지우개 – 터치된 잉크 스트로크를 모두 삭제합니다. 지우개 스트로크 아래에 있는 부분뿐 아니라 전체 잉크 스트로크가 삭제됩니다.

**토글**

- 눈금자 – 눈금자를 표시하거나 숨깁니다. 눈금자 가장자리 근처에 그리면 잉크 스트로크가 눈금자에 맞춰집니다.  
 ![InkToolbar와 연결된 눈금자 화면 효과](images/inking-tools-ruler.png)

기본 구성이지만 앱의 InkToolbar에 포함되는 기본 제공 단추를 완전히 제어할 수 있습니다.

### <a name="custom-buttons"></a>사용자 지정 단추

InkToolbar는 다음 두 가지 그룹의 단추 유형으로 이루어져 있습니다.

1. 기본 제공 그리기, 지우기 및 강조 표시 단추를 포함하는 "도구" 단추 그룹. 사용자 지정 펜과 도구가 여기에 추가됩니다.
> [!NOTE]
> 기능 선택은 함께 사용할 수 없습니다.

2. 기본 제공 눈금자 단추를 포함하는 "토글" 단추 그룹. 사용자 지정 토글이 여기에 추가됩니다.
> [!NOTE]
> 기능은 함께 사용할 수 있으며 다른 활성 도구와 동시에 사용할 수 있습니다.

응용 프로그램 및 필요한 수동 입력 기능에 따라 사용자 지정 잉크 기능에 바인딩된 다음 단추를 InkToolbar에 추가할 수 있습니다.

- 사용자 지정 펜 – 호스트 앱에서 잉크 색상표와 펜 팁 속성(예: 모양, 회전, 크기)이 정의된 펜입니다.
- 사용자 지정 도구 - 호스트 앱에서 정의된 펜 이외의 도구입니다.
- 사용자 지정 토글 - 앱에서 정의된 기능의 상태를 켜짐 또는 꺼짐으로 설정합니다. 켜진 경우 기능이 활성 도구와 함께 작동합니다.

> [!NOTE]
> 기본 제공 단추의 표시 순서는 변경할 수 없습니다. 기본 표시 순서는 볼펜, 연필, 형광펜, 지우개, 눈금자 순입니다. 사용자 지정 펜은 마지막 기본 펜 뒤에 추가되고, 사용자 지정 도구 단추는 마지막 펜 단추와 지우개 단추 사이에 추가되고, 사용자 지정 토글 단추는 눈금자 단추 뒤에 추가됩니다. 사용자 지정 단추는 지정된 순서대로 추가됩니다.

InkToolbar는 최상위 항목일 수 있지만 일반적으로 "수동 입력" 단추 또는 명령을 통해 노출됩니다. Segoe MLD2 자산 글꼴의 EE56 문자 모양을 최상위 수준 아이콘으로 사용하는 것이 좋습니다.

## <a name="inktoolbar-interaction"></a>InkToolbar 조작

모든 기본 제공 펜 및 도구 단추에는 잉크 속성과 펜 팁 모양 및 크기를 설정할 수 있는 플라이아웃 메뉴가 포함되어 있습니다. "확장 문자 모양" ![InkToolbar 문자 모양](images/ink-tools-glyph.png) 이 단추에 표시되면 플라이아웃이 있음을 나타냅니다.

활성 도구의 단추를 다시 선택하면 플라이아웃이 표시됩니다. 색 또는 크기를 변경하면 플라이아웃이 자동으로 해제되고 수동 입력을 다시 시작할 수 있습니다. 사용자 지정 펜과 도구는 기본 플라이아웃을 사용하거나 사용자 지정 플라이아웃을 지정할 수 있습니다.

지우개에는 **모든 잉크 지우기** 명령을 제공하는 플라이아웃도 있습니다.  
![지우개 플라이아웃이 호출된 InkToolbar](images/ink-tools-erase-all-ink.png)

 사용자 지정 및 확장성에 대한 자세한 내용은 [SimpleInk 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)을 참조하세요.

## <a name="dos-and-donts"></a>권장 사항 및 금지 사항

- InkCanvas 및 일반적인 수동 입력은 활성 펜을 통해 가장 잘 작동합니다. 그러나 앱에 필요한 경우 마우스 및 터치식(수동 펜 포함) 입력을 사용한 수동 입력을 지원하는 것이 좋습니다.
- InkToolbar 컨트롤을 InkCanvas와 함께 사용하면 기본적인 수동 입력 기능과 설정을 제공할 수 있습니다. InkCanvas와 InkToolbar 모두 프로그래밍 방식으로 사용자 지정할 수 있습니다.
- InkToolbar 및 일반적인 수동 입력은 활성 펜을 통해 가장 잘 작동합니다. 그러나 앱에 필요한 경우 마우스와 터치를 사용한 수동 입력을 지원할 수 있습니다.
- 터치식 입력을 사용한 수동 입력을 지원하는 경우 “터치 쓰기” 도구 설명과 함께 Segoe MLD2 자산 글꼴의 ED5F 아이콘을 토글 단추에 사용하는 것이 좋습니다.
- 스트로크 선택을 입력할 때 “선택 도구” 도구 설명과 함께 Segoe MLD2 자산 글꼴의 EF20 아이콘을 도구 단추에 사용하는 것이 좋습니다.
- 둘 이상의 InkCanvas를 사용하는 경우 단일 InkToolbar를 사용하여 여러 캔버스의 수동 입력을 제어하는 것이 좋습니다.
- 최상의 성능을 얻으려면 기본 및 사용자 지정 도구 둘 다를 위한 사용자 지정 플라이아웃 하나를 만드는 대신 기본 플라이아웃을 변경하는 것이 좋습니다.

## <a name="get-the-sample-code"></a>샘플 코드 다운로드

- [SimpleInk 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk) - InkCanvas 및 InkToolbar 컨트롤의 사용자 지정 및 확장성 기능과 관련된 8가지 시나리오를 보여줍니다. 각 시나리오는 일반적인 수동 입력 상황 및 컨트롤 구현에 대한 기본 지침을 제공합니다.
- [ComplexInk 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk) - 보다 수준 높은 수동 입력 시나리오를 보여줍니다.
- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 대화형 형식의 모든 XAML 컨트롤을 보여줍니다.

## <a name="related-articles"></a>관련 문서

- [UWP 앱에서 펜 및 스타일러스 조작](http://windowsstyleguide/input/pen-and-stylus-interactions/)
- [잉크 스트로크 인식](http://windowsstyleguide/input/convert-ink-to-text/)
- [잉크 스트로크 저장 및 검색](http://windowsstyleguide/input/save-and-load-ink/)
