---
author: mijacobs
Description: Dialogs and flyouts display transient UI elements that appear when the user requests them or when something happens that requires notification or approval.
title: 대화 상자 및 플라이아웃
template: detail.hbs
ms.author: mijacobs
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: ad6affd9-a3c0-481f-a237-9a1ecd561be8
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: d4ff66e988634cf1ba48809688ea6535e6e95b03
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5871542"
---
# <a name="dialogs-and-flyouts"></a>대화 상자 및 플라이아웃



대화 상자 및 플라이아웃은 사용자로부터 알림, 승인 또는 추가 정보를 요청하는 문제가 발생할 경우 나타나는 임시 UI 요소입니다.

> **중요 API**: [ContentDialog 클래스](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog), [Flyout 클래스](/uwp/api/Windows.UI.Xaml.Controls.Flyout)


:::row:::
    :::column:::
        **Dialogs**
        
        ![Example of a dialog](../images/dialogs/dialog_RS2_delete_file.png)

        Dialogs are modal UI overlays that provide contextual app information. Dialogs block interactions with the app window until being explicitly dismissed. They often request some kind of action from the user.
    :::column-end:::
    :::column::: 
        **Flyouts**

        ![Example of a flyout](../images/flyout-example2.png)

        A flyout is a lightweight contextual popup that displays UI related to what the user is doing. It includes placement and sizing logic, and can be used to reveal a secondary control or show more detail about an item.

        Unlike a dialog, a flyout can be quickly dismissed by tapping or clicking somewhere outside the flyout, pressing the Escape key or Back button, resizing the app window, or changing the device's orientation.
    :::column-end:::
:::row-end:::


## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

대화 상자 및 플라이아웃을 통해 사용자가 중요한 정보를 알 수 있지만 사용자 환경을 방해할 수도 있습니다. 대화 상자는 모달(차단)이므로 대화 상자를 조작할 때까지 아무 작업도 수행하지 못하도록 하여 사용자를 중단시킵니다. 플라이아웃은 덜 방해되는 환경을 제공하지만 너무 많은 플라이아웃이 표시되면 주의가 분산될 수 있습니다.

대화 상자 또는 플라이아웃을 사용하기로 결정했으면 무엇을 사용할 것인지 선택해야 합니다.

대화 상자는 상호 작용을 차단하고 플라이아웃은 그러지 않으므로 질문에 대답하거나 소량의 특정 정보에 중점을 두는 상황에서는 대화 상자를 예약해야 합니다. 반면 무언가에 주의를 집중시키지만 사용자가 무시해도 되는 경우 플라이아웃을 사용할 수 있습니다.

:::row:::
    :::column:::
   <p><b>대화 상자는 다음과 같은 작업에 사용됩니다.</b> <br/>
<ul>
<li>사용자가 계속하기 전에 읽고 <b>반드시</b> 승인해야 하는 중요한 정보 표시 예를 들면 다음과 같습니다.
<ul>
  <li>사용자의 보안이 손상될 수 있는 경우</li>
  <li>사용자가 중요한 자산을 영구적으로 변경하려는 경우</li>
  <li>사용자가 중요한 자산을 삭제하려는 경우</li>
  <li>앱에서 바로 구매를 확인하려면</li>
</ul>

</li>
<li>연결 오류 등 전체 앱 상황에 적용되는 오류 메시지입니다.</li>
<li>앱에서 사용자 대신 선택할 수 없는 경우와 같이 앱에서 사용자에게 차단 질문을 해야 하는 경우의 질문 차단 질문은 무시하거나 연기할 수 없으며 사용자에게 잘 정의된 선택 항목을 제공해야 합니다.</li>
</ul>
</p>
    :::column-end:::
    :::column:::
   <p><b>플라이아웃은 다음과 같은 작업에 사용됩니다.</b> <br/>
<ul>
<li>작업을 완료할 수 있기 전에 필요한 추가 정보 수집</li>
<li>특정 시간에만 관련되는 정보 표시 예를 들어 사진 갤러리 앱에서 이미지 미리 보기를 클릭할 때 플라이아웃을 사용하여 큰 버전의 이미지를 표시할 수 있습니다.</li>
<li>페이지에 항목에 대한 세부 정보 또는 긴 설명과 같은 추가 정보 표시</li>
</ul></p>
    :::column-end:::
:::row-end:::


## <a name="ways-to-avoid-using-dialogs-and-flyouts"></a>대화 상자 및 플라이 아웃을 사용 하지 않도록 하는 방법

공유하려는 정보가 사용자를 중단시킬 만큼 중요한 정보인지 고려해야 합니다. 또한 정보를 표시해야 하는 빈도도 고려해야 합니다. 대화 상자나 알림을 몇 분 마다 표시할 경우에는 기본 UI에 이 정보에 대한 공간을 대신에 할당할 수 있습니다. 예를 들어 채팅 클라이언트에서 친구가 로그인 할 때마다 플라이아웃을 표시하지 않고 온라인 상태인 친구의 목록을 표시하고 친구가 로그온할 때 강조 표시할 수 있습니다.

대화 상자는 흔히 파일 삭제 등과 같이 작업을 실행하기 전에 확인하는 데 사용됩니다. 사용자가 특정 작업을 자주 수행할 경우에는 매번 작업을 확인하지 않고 실수하면 작업을 취소할 수 있는 방법을 제공하는 것이 좋습니다.

## <a name="how-to-create-a-dialog"></a>대화 상자를 만드는 방법

[문서 대화 상자](dialogs.md)를 참조 하세요. 

## <a name="how-to-create-a-flyout"></a>플라이 아웃을 만드는 방법

[플라이 아웃 문서](flyouts.md)를 참조 하세요. 

## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="../images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 앱을 열고 작동 중인 <a href="xamlcontrolsgallery:/item/ContentDialog">ContentDialog</a> 또는 <a href="xamlcontrolsgallery:/item/Flyout">플라이아웃</a>을 확인합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML 컨트롤 갤러리 앱 다운로드(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">소스 코드 다운로드(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

