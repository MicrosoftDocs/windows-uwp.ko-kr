---
author: mijacobs
Description: Purposeful, well-designed motion brings your app to life and makes the experience feel crafted and polished. Help users understand context changes, and tie experiences together with visual transitions.
title: UWP 앱의 동작 및 애니메이션
ms.assetid: 21AA1335-765E-433A-85D8-560B340AE966
label: Motion
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 1423aeff139758c780dcecb079141744931cdd7b
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/30/2018
ms.locfileid: "1816538"
---
# <a name="motion-for-uwp-apps"></a>UWP 앱의 동작

체계적으로 잘 디자인된 동작은 앱에 생명을 불어넣고 환경을 전문적이고 돋보이게 합니다. 동작은 사용자가 컨텍스트의 변화 그리고 앱의 탐색 계층 내에서 현재 위치를 이해하는 데 도움을 줍니다. 동작은 환경을 시각적 전환과 연결합니다. 동작은 환경에 속도감 및 차원성을 추가합니다.

## <a name="benefits-of-motion"></a>동작의 이점

동작은 사물이 이동하도록 만드는 것에서 그치지 않습니다. 동작은 사용자가 그 안에 살면서 마우스, 키보드, 터치, 펜 등의 다양한 입력 유형을 통해 조작할 수 있는 실제 에코시스템 만드는 도구입니다. 경험의 질은 사용자에 대한 앱 응답성 정도, UI가 통신하는 특성의 종류 등에 따라 달라집니다.

동작이 앱에서 목적에 맞게 작동하는지 확인하세요. 최상의 UWP(유니버설 Windows 플랫폼) 앱은 동작을 사용하여 UI에 생명을 불어넣습니다. 동작은 다음을 수행해야 합니다.

- 사용자의 동작에 따라 피드백을 제공합니다.
- 사용자에게 UI를 조작하는 방법을 설명합니다.
- 이전 또는 다음 보기로 이동하는 방법을 나타냅니다.

사용자가 앱 내부에서 보내는 시간이 많아질수록 또는 앱 내부 작업이 더욱 정교해질수록 고품질 동작의 중요성이 점점 커집니다. 동작을 사용하여 사용자가 인지적 부하를 인식하는 방식 및 앱의 사용 편의성을 바꿀 수 있습니다. 동작은 다른 직접적인 이점도 많이 갖고 있습니다.

- **동작은 상호 작용 및 길찾기 동작을 지원합니다.**

    동작에는 방향성이 있습니다. 콘텐츠의 앞이나 뒤, 내부나 외부로 이동하여 사용자가 현재 보기에 도달한 방법과 관련하여 정신적 "이동 경로" 단서를 남깁니다. 전환은 사용자에게 이미 익숙한 작업에 비유함으로써 사용자가 새 응용 프로그램의 작동 방식을 배울 수 있도록 도와줍니다.

- **동작은 성능이 향상된 것 같은 인상을 줄 수 있습니다.**

    네트워크 속도가 떨어지거나 시스템의 작동이 일시 중지되는 경우 애니메이션은 사용자가 대기 시간을 짧게 느끼도록 만들 수 있습니다. 애니메이션을 사용하여 사용자에게 앱이 동결 상태가 아니라 처리 중이라는 것을 알릴 수 있으며 휴대폰이 사용자가 관심을 가질 만한 새 정보를 수동적으로 노출할 수 있습니다.

- **동작은 퍼스낼리티를 더합니다.**

    동작은 종종 사용자가 환경 내에서 이동할 때 앱 퍼스낼리티를 전달하는 공통 스레드입니다.

- **동작은 정교함을 더합니다.**

    유연하고 신속한 동작은 환경에 자연스러운 느낌을 주어 환경과 감성적으로 연결합니다.

## <a name="examples-of-motion"></a>동작의 예

다음은 앱 내에서 이루어지는 동작의 예입니다.

여기에 나오는 앱은 항목 이미지가 "계속해서" 다음 페이지의 머리글이 되도록 연결된 애니메이션을 사용하여 항목 이미지에 애니메이션 효과를 줍니다. 이 효과는 전환 시 사용자 컨텍스트를 유지하는 데 도움이 됩니다.

![연결된 애니메이션](images/connected-animations/example.gif)

여기서는 깊이감, 원근감 및 운동성을 주기 위해 UI가 스크롤하거나 이동할 때 시각적 시차 효과를 통해 여러 개체를 서로 다른 속도로 움직입니다.

![목록 및 배경 이미지를 사용한 시차의 예](images/_Parallax_v2.gif)


## <a name="types-of-motion"></a>동작의 유형

<table>
    <tr>
        <th align="left">동작 유형</th>
        <th align="left">설명</th>
    </tr>
    <tr>
        <td><a href="motion-list.md">추가 및 삭제</a>
        </td>
        <td>목록 애니메이션을 사용하면 사진 앨범이나 검색 결과 목록 같은 컬렉션에서 단일 항목이나 여러 항목을 삽입하거나 제거할 수 있습니다.
        </td>
    </tr>
    <tr>
        <td><a href="connected-animation.md">연결된 애니메이션</a>
        </td>
        <td>연결된 애니메이션을 사용하면 두 가지 보기 간에 전환되는 동작에 애니메이션 효과를 적용하여 역동적이고 매력적인 탐색 환경을 만들 수 있습니다. 이렇게 하면 사용자가 컨텍스트를 유지하는 데 도움이 될 뿐 아니라 보기 간에 연속성이 보장됩니다. 연결된 애니메이션에서는 UI 콘텐츠가 바뀌는 동안 요소가 두 보기 간에 "계속 있는" 것처럼 보이고, 원본 보기의 원래 위치에서 새 보기의 대상 위치로 화면을 가로질러 날아갑니다. 이렇게 하면 보기 간의 공통 콘텐츠가 강조되며 전환 시 아름답고 역동적인 효과를 얻을 수 있습니다. 
        </td>
    </tr>
    <tr>
        <td><a href="content-transition-animations.md">콘텐츠 전환</a>
        </td>
        <td>콘텐츠 전환 애니메이션을 사용하면 컨테이너 또는 배경을 일정하게 유지하면서 화면 영역의 콘텐츠를 변경할 수 있습니다. 새 콘텐츠가 페이드 인됩니다. 바꿀 기존 콘텐츠가 있는 경우 해당 콘텐츠는 페이드 아웃됩니다. </td>
    </tr>
    <tr>
        <td><a href="motion-fade.md">페이드</a>
        </td>
        <td>항목을 보기로 가져오거나 보기 외부로 이동하려면 페이드 애니메이션을 사용합니다. 두 가지의 일반적인 페이드 애니메이션은 페이드 인 및 페이드 아웃입니다. </td>
    </tr>
    <tr>
        <td><a href="page-transitions.md">페이지 전환</a>
        </td>
        <td>페이지 전환은 앱 페이지 사이에서 사용자를 탐색하면서 페이지 사이의 관계로 피드백을 제공합니다.
        </td>
    </tr>
    <tr>
        <td><a href="parallax.md">시차</a>
        </td>
        <td>시각적 시차 효과는 깊이감, 원근감 및 운동성을 만드는 데 도움이 됩니다. UI가 스크롤하거나 이동할 때 여러 개체를 서로 다른 속도로 움직여서 이 효과를 얻습니다.
        </td>
    </tr> 
    <tr>
        <td><a href="motion-pointer.md">누르기 피드백</a>
        </td>
        <td>포인터 누르기 애니메이션은 사용자가 항목을 탭할 때 사용자에게 시각적 피드백을 제공합니다. 포인터 아래로 애니메이션은 눌린 항목을 약간 축소하고 기울이며 항목을 처음 탭할 때 재생됩니다. 포인터 위로 애니메이션은 항목을 원래 위치로 복원하며 사용자가 포인터를 놓을 때 재생됩니다.
        </td>
    </tr>
</table>

## <a name="animations-in-xaml"></a>XAML의 애니메이션

XAML에서 기본적으로 제공되는 애니메이션을 사용하거나, 혹은 자신의 애니메이션을 생성하는 방법에 대한 자세한 내용은 [XAML의 애니메이션](xaml-animation.md)을 참조하십시오. 