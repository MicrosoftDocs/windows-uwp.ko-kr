---
Description: 사용자가 목록 내에서 항목을 이동 하거나 다른 항목의 맨 위에 있는 항목을 삭제 하는 등의 방법으로 개체를 이동 하는 경우 끌어서 놓기 애니메이션을 사용 합니다.
title: 애니메이션 끌기
ms.assetid: 6064755F-6E24-4901-A4FF-263F05F0DFD6
label: Motion--Drag and drop
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4d346d286b6a874ff62e63ecc59a102bcb68fa73
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156857"
---
# <a name="drag-animations"></a>애니메이션 끌기




사용자가 목록 내에서 항목을 이동 하거나 다른 항목의 맨 위에 있는 항목을 삭제 하는 등의 방법으로 개체를 이동 하는 경우 끌어서 놓기 애니메이션을 사용 합니다.

> **중요 한 api**: [ **DragItemThemeAnimation 클래스**](/uwp/api/windows.ui.xaml.media.animation.dragitemthemeanimation)


## <a name="dos-and-donts"></a>권장 사항 및 금지 사항


**시작 애니메이션 끌기**

-   사용자가 개체를 이동 하기 시작할 때 끌기 시작 애니메이션을 사용 합니다.
-   끌어서 놓기 작업의 영향을 받을 수 있는 다른 개체가 있는 경우에만 애니메이션에 영향을 받는 개체를 포함 합니다.
-   끌기 시작 애니메이션으로 시작 하는 애니메이션 시퀀스를 완료 하려면 끌기 끝 애니메이션을 사용 합니다. 이렇게 하면 끌기 시작 애니메이션에 의해 발생 한 끌어 온 개체의 크기 변경이 취소 됩니다.

**끌기 종료 애니메이션**

-   사용자가 끌어 온 개체를 놓을 때 끌기 끝 애니메이션을 사용 합니다.
-   목록에 대 한 애니메이션 추가 및 삭제와 함께 끌기 끝 애니메이션을 사용 합니다.
-   끌기 시작 애니메이션에서 영향을 받는 개체를 포함 한 경우에만 끌기 끝 애니메이션에 영향을 받는 개체를 포함 합니다.
-   끌기 시작 애니메이션을 처음 사용 하지 않은 경우에는 끌기 끝 애니메이션을 사용 하지 마세요. 끌기 시퀀스가 완료되면 모든 개체가 원래의 크기로 돌아갈 수 있도록 두 애니메이션을 모두 사용해야 합니다.

**Enter 애니메이션 간 끌기**

-   사용자가 끌기 소스를 다른 두 개체 사이에 놓을 수 있는 끌어 놓기 영역으로 끌 때 enter 키 애니메이션을 사용 합니다.
-   적절 한 놓기 대상 영역을 선택 하세요. 사용자가 끌어서 놓기에 대 한 끌기 원본을 배치 하기 어려운 경우에는이 영역을 작게 지정 하면 안 됩니다.
-   끌어 놓기 영역을 표시 하기 위해 영향을 받는 개체를 이동 하는 권장 방향은 서로 직접적입니다. 세로 또는 가로로 이동 하는지 여부는 영향을 받는 개체의 방향에 따라 좌우 됩니다.
-   끌기 소스를 영역에서 삭제할 수 없는 경우에는 enter 키 애니메이션을 사용 하지 마세요. Enter 키를 끌어서 놓기 애니메이션은 영향을 받는 개체 사이에 끌기 소스를 놓을 수 있음을 사용자에 게 알립니다.

**나가기 애니메이션 사이를 끌기**

-   사용자가 개체를 다른 두 개체 사이에 끌어 놓을 수 있는 영역 밖으로 끌 때 leave 애니메이션 사이에 끌기를 사용 합니다.
-   Enter 키 애니메이션을 처음으로 사용 하지 않은 경우에는 나가기 애니메이션을 사용 하지 마세요.


## <a name="related-articles"></a>관련된 문서

**개발자용**
* [애니메이션 개요](./xaml-animation.md)
* [끌어서 놓기 시퀀스에 애니메이션 적용](/previous-versions/windows/apps/jj649427(v=win.10))
* [빠른 시작: 라이브러리 애니메이션을 사용 하 여 UI에 애니메이션 효과 주기](/previous-versions/windows/apps/hh452703(v=win.10))
* [**DragItemThemeAnimation 클래스**](/uwp/api/windows.ui.xaml.media.animation.dragitemthemeanimation)
* [**DropTargetItemThemeAnimation 클래스**](/uwp/api/windows.ui.xaml.media.animation.droptargetitemthemeanimation)
* [**DragOverThemeAnimation 클래스**](/uwp/api/windows.ui.xaml.media.animation.dragoverthemeanimation)


 