---
Description: 대화 상자 및 플라이아웃은 사용자가 요청할 경우나 알림 또는 승인이 필요한 문제가 발생할 경우 나타나는 임시 UI 요소를 표시합니다.
title: 대화 상자 및 플라이아웃
template: detail.hbs
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: ad6affd9-a3c0-481f-a237-9a1ecd561be8
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: c61d1478c38df315a3fe3c20151de8c2bfbca4e2
ms.sourcegitcommit: 23c5d8dfaeb6edbca780637ffd26fe892db27519
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123583"
---
# <a name="dialogs-and-flyouts"></a>대화 상자 및 플라이아웃

대화 상자 및 플라이아웃은 사용자로부터 알림, 승인 또는 추가 정보를 요청하는 문제가 발생할 경우 나타나는 임시 UI 요소입니다.

> **플랫폼 API:** [ContentDialog 클래스](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog), [Flyout 클래스](/uwp/api/Windows.UI.Xaml.Controls.Flyout)

**대화 상자**

![대화 상자 예제](../images/dialogs/dialog_RS2_delete_file.png)

대화 상자는 상황에 맞는 앱 정보를 제공하는 모달 UI 오버레이입니다. 대화 상자는 명시적으로 닫을 때까지 앱 창의 조작을 차단합니다. 종종 사용자의 작업을 요청하기도 합니다.

**플라이아웃**

![플라이아웃의 예제](../images/flyout-example2.png)

플라이아웃은 사용자가 수행하는 작업과 관련된 UI를 표시하는 경량의 상황에 맞는 팝업입니다. 이 기능은 배치 및 크기 조정 논리를 포함하며, 보조 컨트롤을 표시하거나 항목에 대한 세부 정보를 표시하는 데 사용할 수 있습니다.

대화 상자와 달리 플라이아웃은 플라이아웃 바깥쪽 아무 곳이나 탭 또는 클릭하거나, Esc 키 또는 뒤로 단추를 누르거나, 앱 창 크기를 조정하거나, 디바이스의 방향을 변경하여 신속하게 해제할 수 있습니다.

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

대화 상자 및 플라이아웃을 통해 사용자가 중요한 정보를 알 수 있지만 사용자 환경을 방해할 수도 있습니다. 대화 상자는 모달(차단)이므로 대화 상자와 상호 작용할 때까지 다른 작업을 수행하지 못하도록 사용자를 차단합니다. 플라이아웃은 덜 방해되는 환경을 제공하지만 너무 많은 플라이아웃이 표시되면 주의가 분산될 수 있습니다.

대화 상자 또는 플라이아웃을 사용하기로 결정했으면 무엇을 사용할 것인지 선택해야 합니다.

대화 상자는 상호 작용을 차단하고 플라이아웃은 그러지 않으므로 질문에 대답하거나 소량의 특정 정보에 중점을 두는 상황에서는 대화 상자를 예약해야 합니다. 반면 무언가에 주의를 집중시키지만 사용자가 무시해도 되는 경우 플라이아웃을 사용할 수 있습니다.

   <p><b>대화 상자의 용도</b> <br/>
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


   <p><b>플라이아웃의 용도</b> <br/>
<ul>
<li>작업을 완료할 수 있기 전에 필요한 추가 정보 수집</li>
<li>특정 시간에만 관련된 정보 표시 예를 들어 사진 갤러리 앱에서 이미지 미리 보기를 클릭할 때 플라이아웃을 사용하여 큰 버전의 이미지를 표시할 수 있습니다.</li>
<li>페이지에 항목에 대한 세부 정보 또는 긴 설명과 같은 추가 정보 표시</li>
</ul></p>

## <a name="ways-to-avoid-using-dialogs-and-flyouts"></a>대화 상자 및 플라이아웃을 사용하지 않도록 방지하는 방법

공유하려는 정보가 사용자를 차단할 만큼 중요한 정보인지 고려합니다. 또한 정보를 표시해야 하는 빈도도 고려해야 합니다. 대화 상자나 알림을 몇 분 마다 표시할 경우에는 기본 UI에 이 정보에 대한 공간을 대신에 할당할 수 있습니다. 예를 들어 채팅 클라이언트에서 친구가 로그인 할 때마다 플라이아웃을 표시하지 않고 온라인 상태인 친구의 목록을 표시하고 친구가 로그온할 때 강조 표시할 수 있습니다.

대화 상자는 파일 삭제와 같은 작업을 실행하기 전에 확인하는 데 자주 사용됩니다. 사용자가 특정 작업을 자주 수행할 경우에는 매번 작업을 확인하지 않고 실수하면 작업을 취소할 수 있는 방법을 제공하는 것이 좋습니다.

## <a name="how-to-create-a-dialog"></a>대화 상자를 만드는 방법

[대화 상자 문서](dialogs.md)를 참조하세요. 

## <a name="how-to-create-a-flyout"></a>플라이아웃을 만드는 방법

[플라이아웃 문서](flyouts.md)를 참조하세요. 

## <a name="examples"></a>예

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="../images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML Controls Gallery</strong> 앱이 설치되어 있는 경우 여기를 클릭하여 앱을 열고 <a href="xamlcontrolsgallery:/item/ContentDialog">ContentDialog</a> 또는 <a href="xamlcontrolsgallery:/item/Flyout">Flyout</a>이 작동되는지 확인합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

