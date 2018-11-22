---
author: muhsinking
Description: Use an inverted list to add new items at the bottom.
title: 반전된 목록
label: Inverted lists
template: detail.hbs
ms.author: mukin
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 52c1d63d-69c1-48d6-a234-6f39296e4bfd
pm-contact: predavid
design-contact: kimsea
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: d19d18d55c973644f5c3f99ae3d442e6dbf3a724
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7575961"
---
# <a name="inverted-lists"></a>반전된 목록

 

목록 보기를 사용하여 발신자/수신자를 나타내기 위해 시각적으로 구분된 항목으로 채팅 환경의 대화를 표시할 수 있습니다.  서로 다른 색과 가로 맞춤을 사용하여 발신자/수신자의 메시지를 구분하면 사용자가 빠르게 대화에 적응하는 데 도움이 됩니다.

> **중요 API**:  [ListView 클래스](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx), [ItemsStackPanel 클래스](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.aspx), [ItemsUpdatingScrollMode 속성](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.itemsupdatingscrollmode.aspx)
 
일반적으로 위에서 아래 대신 아래에서 위로 확장되는 것처럼 목록을 표시해야 합니다.  새 메시지가 도착하고 끝에 추가되면 이전 메시지가 위로 올라가서 사용자가 최신 메시지를 발견하기 쉽게 합니다.  그러나 사용자가 이전 회신을 보기 위해 위로 스크롤한 경우 새 메시지가 도착해도 화면이 이동하여 포커스를 방해하면 안 됩니다.

![반전된 목록이 있는 채팅 앱](images/listview-inverted.png)

## <a name="create-an-inverted-list"></a>반전된 목록 만들기

반전된 목록을 만들려면 [ItemsStackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.aspx)을 항목 패널로 지정하여 목록 보기를 사용합니다. ItemsStackPanel에서 [ItemsUpdatingScrollMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.itemsupdatingscrollmode.aspx)를 [KeepLastItemInView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsupdatingscrollmode.aspx)로 설정합니다.

> [!IMPORTANT]
> **KeepLastItemInView** 열거형 값은 Windows 10 버전 1607부터 사용할 수 있습니다. 이전 버전의 Windows 10에서 앱을 실행할 때는 이 값을 사용할 수 없습니다.

이 예제에서는 목록 보기의 항목을 아래쪽에 맞추고 항목이 변경될 때 마지막 항목이 보기에 유지되도록 지정하는 방법을 보여 줍니다.
 
 **XAML**
 ```xaml
<ListView>
    <ListView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsStackPanel VerticalAlignment="Bottom"
                             ItemsUpdatingScrollMode="KeepLastItemInView"/>
        </ItemsPanelTemplate>
    </ListView.ItemsPanel>
</ListView>
```

## <a name="dos-and-donts"></a>권장 사항 및 금지 사항

- 사용자가 대화 흐름을 파악하기 쉽도록 발신자/수신자의 메시지를 반대쪽에 정렬합니다.
- 사용자가 이미 대화 끝에서 다음 메시지를 기다리고 있는 경우 최신 메시지 표시에 방해가 되지 않도록 기존 메시지에 애니메이션 효과를 줍니다.
- 대화 끝을 읽고 있지 않은 경우 항목을 이동하여 사용자 포커스를 방해하지 않습니다.

## <a name="get-the-sample-code"></a>샘플 코드 다운로드

- [XAML 상향식 목록 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlBottomUpList)