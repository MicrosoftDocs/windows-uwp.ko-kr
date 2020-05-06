---
description: Windows 앱에 컨트롤 &amp; 패턴을 추가하는 방법에 대한 디자인 지침과 코딩 지침을 확인하세요. 앱에서 사용할 45가지 이상의 강력한 컨트롤을 찾습니다.
title: Windows 컨트롤 및 패턴 - Windows 앱 개발
keywords: uwp 컨트롤, 사용자 인터페이스, 앱 컨트롤, windows 컨트롤
label: Controls & patterns
template: detail.hbs
ms.date: 03/23/2020
ms.topic: article
ms.assetid: ce2e611c-c419-4a14-9095-b88ac711d1b8
ms.localizationpriority: medium
ms.openlocfilehash: 63907b3bfe3fc6dece900f1a7c09ac535e859471
ms.sourcegitcommit: f727b68e86a86c94eff00f67ed79a1c12666e7bc
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80218453"
---
# <a name="controls-for-windows-apps"></a>Windows 앱용 컨트롤

![컨트롤](../images/controls-2x.png)

Windows 앱 개발에서 <i>컨트롤</i>은 콘텐츠를 표시하거나 조작을 가능하게 하는 UI 요소입니다. 컨트롤은 사용자 인터페이스의 구성 요소입니다. <i>패턴</i>은 새로운 것을 만들기 위해 여러 컨트롤을 조합하는 방법입니다.

간단한 단추에서 그리드 보기처럼 강력한 데이터 컨트롤까지 45개 이상의 사용할 수 있는 컨트롤을 제공합니다.  이러한 컨트롤은 Fluent 디자인 시스템의 일부이며, 모든 디바이스와 화면 크기에서 멋지게 보이는 대담하고 확장 가능한 UI를 만드는 데 도움이 됩니다.

이 섹션의 문서에서는 Windows 앱에 컨트롤 및 패턴을 추가하는 방법에 대한 디자인 지침과 코딩 지침을 제공합니다.

## <a name="intro"></a>소개

XAML 및 C#으로 컨트롤을 추가하고 스타일을 지정하는 방법에 대한 일반 지침과 코드 예제입니다.

:::row:::
    :::column:::
      <p><b><a href="controls-and-events-intro.md">컨트롤 추가 및 이벤트 처리</a></b> <br/>
앱에 컨트롤을 추가하는 세 가지 주요 단계는 앱 UI에 컨트롤 추가, 컨트롤의 속성 설정, 특정 작업을 수행하도록 컨트롤의 이벤트 처리기에 코드 추가입니다.</p>
    :::column-end:::
    :::column:::
      <p><b><a href="xaml-styles.md">컨트롤 스타일 지정</a></b> <br/>
XAML 프레임워크를 사용하여 다양한 방법으로 앱 모양을 사용자 지정할 수 있습니다. 스타일을 사용하면 컨트롤 속성을 설정하고 이 설정을 재사용하여 여러 컨트롤에서 일관된 모양을 얻을 수 있습니다.</p>
    :::column-end:::
:::row-end:::

## <a name="get-the-windows-ui-library"></a>Windows UI Library 가져오기

|  |  |
| - | - |
| ![WinUI 로고](images/winui-logo-64x64.png) | 일부 컨트롤은 새 컨트롤과 UI 기능을 포함하는 NuGet 패키지인 Windows UI(WinUI) 라이브러리에만 제공됩니다. 라이브러리를 가져오려면 [Windows UI Library 개요 및 설치 지침](/uwp/toolkits/winui/)을 참조하세요.<br/>WinUI 2.2부터 많은 컨트롤의 기본 스타일이 둥근 모서리를 사용하도록 업데이트되었습니다. 자세한 내용은 [모서리 반경](/windows/uwp/design/style/rounded-corner)을 참조하세요. |

## <a name="alphabetical-index"></a>사전순 인덱스

특정 컨트롤 및 패턴에 대한 자세한 정보입니다. 기능별로 정렬된 목록을 보려면 [기능별 컨트롤 인덱스](controls-by-function.md)를 참조하세요.

:::row:::
    :::column:::

- 애니메이션을 사용하는 시각적 플레이어([Lottie](/windows/communitytoolkit/animations/lottie) 참조) ![WinUI 로고](images/winui-logo-16x16.png)
- [자동 제안 상자](auto-suggest-box.md)
- [Button](buttons.md)
- [달력 날짜 선택](calendar-date-picker.md)
- [달력 보기](calendar-view.md)
- [확인란](checkbox.md)
- [색 선택](color-picker.md) ![WinUI 로고](images/winui-logo-16x16.png)
- [콤보 상자](combo-box.md)
- [명령 모음](app-bars.md)
- [명령 모음 플라이아웃](command-bar-flyout.md) ![WinUI 로고](images/winui-logo-16x16.png)
- [연락처 카드](contact-card.md)
- [콘텐츠 대화 상자](dialogs-and-flyouts/dialogs.md)
- [콘텐츠 링크](content-links.md)
- [상황에 맞는 메뉴](menus.md)
- [날짜 선택기](date-picker.md)
- [대화 상자 및 플라이아웃](dialogs-and-flyouts/index.md)
- [드롭다운 단추](buttons.md#create-a-drop-down-button) ![WinUI 로고](images/winui-logo-16x16.png)
- [대칭 이동 보기](flipview.md)
- [플라이아웃](dialogs-and-flyouts/flyouts.md)
- [양식](forms.md)(패턴)
- [그리드 보기](listview-and-gridview.md)
- [하이퍼링크](hyperlinks.md)
- [하이퍼링크 단추](hyperlinks.md#create-a-hyperlinkbutton)
- [이미지 및 이미지 브러시](images-imagebrushes.md)
- [잉크 입력 컨트롤](inking-controls.md)
- [목록 보기](listview-and-gridview.md)
- [지도 컨트롤](../../maps-and-location/controls-map.md)
- [마스터/세부 정보](master-details.md)(패턴)
- [미디어 재생](media-playback.md)
- [메뉴 모음](menus.md#create-a-menu-bar) ![WinUI 로고](images/winui-logo-16x16.png)
- [메뉴 플라이아웃](menus.md)
- [탐색 보기](navigationview.md) ![WinUI 로고](images/winui-logo-16x16.png)

    :::column-end:::
    :::column:::

- [숫자 상자](number-box.md) ![WinUI 로고](images/winui-logo-16x16.png)
- [시차 보기](..\motion\parallax.md) ![WinUI 로고](images/winui-logo-16x16.png)
- [암호 상자](password-box.md)
- [인물 사진](person-picture.md) ![WinUI 로고](images/winui-logo-16x16.png)
- [피벗](pivot.md)
- [진행률 표시줄](progress-controls.md) ![WinUI 로고](images/winui-logo-16x16.png)
- [진행률 링](progress-controls.md)
- [라디오 단추](radio-button.md)
- [평점 컨트롤](rating.md) ![WinUI 로고](images/winui-logo-16x16.png)
- [반복 단추](buttons.md#create-a-repeat-button)
- [서식 있는 편집 상자](rich-edit-box.md)
- [서식 있는 텍스트 블록](rich-text-block.md)
- [스크롤 뷰어](scroll-controls.md)
- [검색](search.md)(패턴)
- [시맨틱 줌](semantic-zoom.md)
- [셰이프](shapes.md)
- [슬라이더](slider.md)
- [분할 단추](buttons.md#create-a-split-button) ![WinUI 로고](images/winui-logo-16x16.png)
- [분할 보기](split-view.md)
- [살짝 밀기 컨트롤](swipe.md) ![WinUI 로고](images/winui-logo-16x16.png)
- [탭 보기](tab-view.md) ![WinUI 로고](images/winui-logo-16x16.png)
- [교육 팁](dialogs-and-flyouts/teaching-tip.md) ![WinUI 로고](images/winui-logo-16x16.png)
- [텍스트 블록](text-block.md)
- [입력란](text-box.md)
- [시간 선택기](time-picker.md)
- [토글 스위치](toggles.md)
- [토글 단추](buttons.md)
- [토글 분할 단추](buttons.md#create-a-toggle-split-button)
- [도구 설명](tooltips.md)
- [트리 보기](tree-view.md) ![WinUI 로고](images/winui-logo-16x16.png)
- [2창 보기](two-pane-view.md) ![WinUI 로고](images/winui-logo-16x16.png)
- [웹 뷰](web-view.md)

    :::column-end:::
:::row-end:::




## <a name="xaml-controls-gallery"></a>XAML Controls Gallery

Microsoft Store에서 _XAML Controls Gallery_ 앱을 가져와서 이러한 컨트롤과 Fluent 디자인 시스템이 실제로 작동하는지 확인합니다. 이 앱은 이 웹사이트의 대화형 컴패니언입니다. 앱이 설치되면 개별 컨트롤 페이지의 링크를 사용하여 앱을 시작하고 실제로 작동하는 컨트롤을 확인할 수 있습니다.

<a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a>

<a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a>

<img src="images/xaml-controls-gallery.png" alt="XAML Controls Gallery screen" />

## <a name="additional-controls"></a>추가 컨트롤

Windows 개발용 추가 컨트롤은<a href="https://www.telerik.com/">Telerik</a>, <a href="https://www.syncfusion.com/uwp-ui-controls">SyncFusion</a>, <a href="https://www.devexpress.com/Products/NET/Controls/Win10Apps/">DevExpress</a>, <a href="https://www.infragistics.com/products/universal-windows-platform">Infragistics</a>, <a href="https://www.componentone.com/Studio/Platform/UWP">ComponentOne</a> 및 <a href="https://www.actiprosoftware.com/products/controls/universal">ActiPro</a>와 같은 회사에서 사용할 수 있습니다. 이러한 컨트롤은 사용자 지정 컨트롤 및 서비스를 사용하여 표준 시스템 컨트롤을 보강함으로써 엔터프라이즈 및 .NET 개발자용 추가 지원을 제공합니다.
