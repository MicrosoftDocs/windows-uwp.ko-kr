---
author: mijacobs
Description: Use drag-and-drop animations when users move objects, such as moving an item within a list, or dropping an item on top of another.
title: UWP 앱의 끌기 애니메이션
ms.assetid: 6064755F-6E24-4901-A4FF-263F05F0DFD6
label: Motion--Drag and drop
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d271d0b9c8e7ce73835457789aca3fa2cb5eda97
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5920178"
---
# <a name="drag-animations"></a>끌기 애니메이션




끌어서 놓기 애니메이션은 목록 내에서 항목 이동, 다른 항목 위에 항목 놓기 등 사용자가 개체를 이동할 때 사용합니다.

> **중요 API**: [**DragItemThemeAnimation 클래스**](https://msdn.microsoft.com/library/windows/apps/br243174)


## <a name="dos-and-donts"></a>권장 사항 및 금지 사항


**끌기 시작 애니메이션**

-   끌기 시작 애니메이션은 사용자가 개체를 이동하기 시작할 때 사용합니다.
-   애니메이션에 해당 개체를 포함하는 것은 끌어서 놓기 작업의 영향을 받는 다른 개체도 있는 경우에 한합니다.
-   끌기 시작 애니메이션으로 시작되는 모든 애니메이션 시퀀스를 완료하려면 끌기 끝 애니메이션을 사용합니다. 그러면 끌기 시작 애니메이션에 의해 끌어오는 개체의 크기가 반대로 변경됩니다.

**끌기 끝 애니메이션**

-   끌기 끝 애니메이션은 사용자가 끌어온 항목을 놓을 때 사용합니다.
-   끌기 끝 애니메이션은 목록의 추가 및 삭제 애니메이션과 결합하여 사용합니다.
-   영향을 받는 개체를 끌기 시작 애니메이션에 포함한 경우에만 해당 개체를 끌기 끝 애니메이션에 포함합니다.
-   끌기 끝 애니메이션은 끌기 시작 애니메이션을 먼저 사용한 경우에만 사용합니다. 끌기 시퀀스가 완료되면 모든 개체가 원래의 크기로 돌아갈 수 있도록 두 애니메이션을 모두 사용해야 합니다.

**들어가기 사이 끌기 애니메이션**

-   두 개의 다른 개체 사이에 놓을 수 있는 놓기 영역에 끌기 원본을 끌어올 경우 들어가기 사이 끌기 애니메이션을 사용합니다.
-   알맞은 놓기 대상 영역을 선택합니다. 이 영역은 사용자가 놓으려는 끌기 원본을 배치하기 힘들 정도로 작아서는 안 됩니다.
-   끌어놓기 영역을 표시하기 위해 영향을 받는 개체를 이동해야 하는 권장 방향은 서로 떨어진 방향입니다. 세로로 이동하는지 가로로 이동하는지 여부는 서로에 대해 영향을 받는 개체의 방향에 따라 다릅니다.
-   끌기 원본을 영역에 놓을 수 없는 경우 들어가기 사이 끌기 애니메이션을 사용하지 마세요. 들어가기 사이 끌기 애니메이션은 사용자에게 끌기 원본을 영향을 받는 개체 사이에 놓을 수 있음을 알려줍니다.

**나가기 사이 끌기 애니메이션**

-   두 개의 다른 개체 사이에 놓을 수 있는 영역에서 멀리 개체를 끌어갈 경우 나가기 사이 끌기 애니메이션을 사용합니다.
-   나가기 사이 끌기 애니메이션은 들어가기 사이 끌기 애니메이션을 먼저 사용한 경우에만 사용합니다.


## <a name="related-articles"></a>관련 문서

**개발자용**
* [애니메이션 개요](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [끌어서 놓기 시퀀스 애니메이션](https://msdn.microsoft.com/library/windows/apps/xaml/jj649427)
* [빠른 시작: 라이브러리 애니메이션을 사용하여 UI에 애니메이션 효과 주기](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**DragItemThemeAnimation 클래스**](https://msdn.microsoft.com/library/windows/apps/br243174)
* [**DropTargetItemThemeAnimation 클래스**](https://msdn.microsoft.com/library/windows/apps/br243186)
* [**DragOverThemeAnimation 클래스**](https://msdn.microsoft.com/library/windows/apps/br243180)


 




