---
title: Microsoft PowerToys
description: Microsoft PowerToys는 Windows 10을 사용자 지정하기 위한 유틸리티 세트입니다. 유틸리티에는 ColorPicker(색상 값을 얻으려면 아무 곳이나 클릭), FancyZones(창을 모눈 레이아웃에 배치하는 바로 가기), 파일 탐색기 추가 기능(SVG 또는 Markdown 파일 미리 보기), Image Resizer(오른쪽 단추를 간단히 클릭하여 하나 이상의 이미지 크기 조정), Keyboard Manager(키를 다시 매핑하거나 자신만의 바로 가기 만들기), PowerRename(검색 및 바꾸기를 사용하여 대량 이름 바꾸기), PowerToys Run(Alt + Space로 앱 실행), Shortcut Guide 등이 있으며, 더 추가될 예정입니다.
ms.date: 12/02/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 1f441c7ef9fc4268b35c041f1100cb7f116318a5
ms.sourcegitcommit: 5dac88ad541b71ebe85b78951e6b357a3db176cc
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/16/2020
ms.locfileid: "97611887"
---
# <a name="microsoft-powertoys-utilities-to-customize-windows-10"></a>Microsoft PowerToys: Windows 10을 사용자 지정하는 유틸리티

Microsoft PowerToys는 고급 사용자가 생산성을 높이기 위해 Windows 10 환경을 조정하고 간소화하는 데 사용할 수 있는 유틸리티 세트입니다.

> [!div class="nextstepaction"]
> [PowerToys 설치](install.md)

## <a name="processor-support"></a>프로세서 지원

- **x64**: 지원함
- **x86**: 개발 시([문제 #602](https://github.com/microsoft/PowerToys/issues/602) 참조)
- **ARM**: 개발 시([문제 #490](https://github.com/microsoft/PowerToys/issues/490) 참조)

## <a name="current-powertoy-utilities"></a>현재 PowerToy 유틸리티

현재 사용할 수 있는 유틸리티는 다음과 같습니다.

### <a name="color-picker"></a>Color Picker

:::row:::
    :::column:::
        [![ColorPicker 스크린샷](../images/pt-color-picker.png)](color-picker.md)
    :::column-end:::
    :::column span="2":::
        [ColorPicker](color-picker.md)는 <kbd>Win</kbd>+<kbd>Shift</kbd>+<kbd>C</kbd>로 활성화되는 시스템 수준의 색상 선택 유틸리티입니다. 현재 실행 중인 애플리케이션에서 색상을 선택하면 선택기가 구성 가능한 형식으로 색상을 클립보드에 자동으로 복사합니다. Color Picker에는 이전에 선택한 색상의 내역을 표시하는 편집기도 포함되어 있어 선택한 색상을 미세 조정하고 다른 문자열 표현을 복사할 수 있습니다. 이 코드는 [Martin Chrzan의 Color Picker](https://github.com/martinchrzan/ColorPicker)를 기반으로 합니다.
    :::column-end:::
:::row-end:::

### <a name="fancy-zones"></a>Fancy Zones

:::row:::
    :::column:::
        [![FancyZones 스크린샷](../images/pt-fancy-zones.png)](fancyzones.md)
    :::column-end:::
    :::column span="2":::
        [FancyZones](fancyzones.md)는 복잡한 창 레이아웃을 쉽게 만들고 해당 레이아웃으로 창을 신속하게 배치할 수 있게 해주는 창 관리자입니다.
    :::column-end:::
:::row-end:::

### <a name="file-explorer-add-ons"></a>파일 탐색기 추가 기능

:::row:::
    :::column:::
        [![파일 탐색기 스크린샷](../images/pt-file-explorer.png)](file-explorer.md)
    :::column-end:::
    :::column span="2":::
        [파일 탐색기](file-explorer.md) 추가 기능을 사용하면 파일 탐색기에서 미리 보기 창 렌더링을 사용하여 SVG 아이콘(svg) 및 Markdown(md) 파일 미리 보기를 표시할 수 있습니다. 미리 보기 창을 사용하려면 파일 탐색기에서 "보기" 탭을 선택하고 "미리 보기 창"을 선택합니다.
    :::column-end:::
:::row-end:::

### <a name="image-resizer"></a>Image Resizer

:::row:::
    :::column:::
        [![Image Resizer 스크린샷](../images/pt-image-resizer.png)](image-resizer.md)
    :::column-end:::
    :::column span="2":::
        [Image Resizer](image-resizer.md)는 이미지의 크기를 신속하게 조정하는 Windows 셸 확장입니다.  파일 탐색기에서 마우스 오른쪽 단추를 클릭하여 하나 이상의 이미지를 즉시 크기 조정합니다. 이 코드는 [Brice Lambson의 Image Resizer](https://github.com/bricelam/ImageResizer)를 기반으로 합니다.
    :::column-end:::
:::row-end:::

### <a name="keyboard-manager"></a>Keyboard Manager

:::row:::
    :::column:::
        [![Keyboard Manager 스크린샷](../images/pt-keyboard-manager.png)](keyboard-manager.md)
    :::column-end:::
    :::column span="2":::
        [Keyboard Manager](keyboard-manager.md)를 사용하면 키를 다시 매핑하여 사용자 고유의 바로 가기 키를 만들어 키보드의 생산성을 높일 수 있습니다. 이 PowerToy에는 Windows 10 1903(빌드 18362) 이상이 필요합니다.
    :::column-end:::
:::row-end:::

### <a name="powerrename"></a>PowerRename

:::row:::
    :::column:::
        [![PowerRename 스크린샷](../images/pt-rename.png)](powerrename.md)
    :::column-end:::
    :::column span="2":::
        [PowerRename](powerrename.md)를 사용하면 파일 이름을 대량으로 바꾸고, 검색하고, 바꿀 수 있습니다. 여기에는 정규 표현식 사용, 특정 파일 형식 대상 지정, 예상 결과 미리 보기 및 변경 내용 취소 기능과 같은 고급 기능이 포함됩니다. 이 코드는 [Chris Davis의 SmartRename](https://github.com/chrdavis/SmartRename)를 기반으로 합니다.
    :::column-end:::
:::row-end:::

### <a name="powertoys-run"></a>PowerToys Run

:::row:::
    :::column:::
        [![PowerToys Run 스크린샷](../images/pt-run.png)](run.md)
    :::column-end:::
    :::column span="2":::
        [PowerToys Run](run.md)를 통해 즉시 앱을 검색하고 시작할 수 있습니다. 바로 가기 <kbd>Alt</kbd>+<kbd>Space</kbd>를 눌러 입력을 시작하면 됩니다. 오픈 소스이며 추가 플러그 인을 위한 모듈식입니다. 이제 Window Walker도 포함됩니다. 이 PowerToy에는 Windows 10 1903(빌드 18362) 이상이 필요합니다.
    :::column-end:::
:::row-end:::

### <a name="shortcut-guide"></a>Shortcut Guide

:::row:::
    :::column:::
        [![Shortcut Guide 스크린샷](../images/pt-shortcut-guide.png)](shortcut-guide.md)
    :::column-end:::
    :::column span="2":::
        [Windows 키 Shortcut Guide](shortcut-guide.md)는 사용자가 Windows 키를 1초 이상 누르고 있으면 표시되며 현재 바탕 화면에서 사용 가능한 단축키를 보여줍니다.
    :::column-end:::
:::row-end:::

## <a name="powertoys-video-walk-through"></a>PowerToys 비디오 연습

이 비디오에서 Clint Rutkas(PowerToys의 PM)가 사용 가능한 다양한 유틸리티를 설치 및 사용하는 방법을 안내하고 몇 가지 팁, 기여 방법에 대한 정보 등을 공유합니다.

> [!VIDEO https://channel9.msdn.com/Shows/Tabs-vs-Spaces/PowerToys-Utilities-to-customize-Windows-10/player?format=ny]

## <a name="future-powertoy-utilities"></a>향후 PowerToy 유틸리티

### <a name="experimental-powertoys"></a>실험용 PowerToys

PowerToys의 시험판 실험 버전을 설치하여 다음과 같은 최신 실험 유틸리티를 사용해보세요.

#### <a name="video-conference-mute-experimental"></a>Video Conference Mute(실험)

:::row:::
    :::column:::
        [![Video Conference Mute 스크린샷](../images/pt-video-conference-mute.png)](video-conference-mute.md)
    :::column-end:::
    :::column span="2":::
        [Video Conference Mute](video-conference-mute.md)는 전화 회의 중에 현재 어떤 애플리케이션을 사용하고 있든 관계없이 <kbd>⊞ Win</kbd>+<kbd>N</kbd>을 사용하여 마이크와 카메라 전체를 빠르게 "음소거"할 수 있습니다. [PowerToys의 시험판/실험 버전](https://github.com/microsoft/PowerToys/releases/)에만 포함되어 있으며 Windows 10 1903(빌드 18362) 이상이 필요합니다.
    :::column-end:::
:::row-end:::

## <a name="known-issues"></a>알려진 문제

GitHub의 PowerToys 리포지토리에 있는 [Issues](https://github.com/microsoft/PowerToys/issues) 탭에서 알려진 문제를 검색하거나 새로운 문제를 제출하세요.

## <a name="contribute-to-powertoys-open-source"></a>PowerToys에 기여(오픈 소스)

PowerToys는 여러분의 기여를 환영합니다. PowerToys 개발 팀은 사용자가 Windows를 최대한 활용하는 데 도움이 되는 도구를 빌드하기 위해 파워 유저 커뮤니티와 협력하게 된 것을 기쁘게 생각합니다. 다음과 같은 다양한 방법으로 기여할 수 있습니다.

- [기술 사양](https://codeburst.io/on-writing-tech-specs-6404c9791159) 작성
- [디자인 개념 또는 권장 사항](https://www.microsoft.com/design/inclusive/) 제출
- [문서에 기고](https://docs.microsoft.com/contribute/)
- [소스 코드](https://github.com/microsoft/PowerToys/tree/master/src)의 버그 식별 및 해결
- [새 기능 및 PowerToy 유틸리티 코딩](https://github.com/microsoft/PowerToys/tree/master/doc/devdocs)

기여하려는 기능에 대한 작업을 시작하기 전에 **[기여자 가이드](https://github.com/microsoft/PowerToys/blob/master/CONTRIBUTING.md)** 를 읽으세요. PowerToys 팀은 모범 사례를 파악하고, 기능 개발 전반에서 지침과 멘토십을 제공하고, 낭비되거나 중복된 작업을 방지하는 데 도움을 드릴 것입니다.

## <a name="powertoys-release-notes"></a>PowerToys 릴리스 정보

PowerToys [릴리스 정보](https://github.com/microsoft/PowerToys/releases/)는 GitHub 리포지토리의 설치 페이지에 나열되어 있습니다. 참고로 PowerToys wiki에서 [릴리스 검사 목록](https://github.com/microsoft/PowerToys/wiki/Release-check-list)을 확인할 수도 있습니다.

## <a name="powertoys-history"></a>PowerToys의 역사

[Windows 95 시대의 PowerToys 프로젝트](https://en.wikipedia.org/wiki/Microsoft_PowerToys)에서 영감을 받아 다시 제작한 이 유틸리티는 파워 유저에게 Windows 10 셸에서 효율성을 높이고 개별 워크플로에 맞게 사용자 지정할 수 있는 방법을 제공합니다.  Windows 95 PowerToys에 대한 자세한 개요는 [여기](https://socket3.wordpress.com/2016/10/22/using-windows-95-powertoys/)에서 확인할 수 있습니다.

## <a name="powertoys-roadmap"></a>PowerToys의 로드맵

PowerToys는 속성 개발 오픈 소스 팀으로, 파워 유저에게 Windows 10 셸에서 효율성을 높이고 개별 워크플로에 맞게 사용자 지정할 수 있는 방법을 제공하는 것을 목표로 합니다. 작업 우선 순위는 사용자 생산성 향상을 목표로 지속적으로 검사, 재평가 및 조정됩니다.

- [가능한 PowerToys의 새로운 사양](https://github.com/microsoft/PowerToys/wiki/Specs)
- [백로그 우선 순위 목록](https://github.com/microsoft/PowerToys/wiki/Roadmap#backlog-priority-list-in-order)
- [버전 1.0 전략 사양](https://github.com/microsoft/PowerToys/wiki/Version-1.0-Strategy), 2020년 2월
