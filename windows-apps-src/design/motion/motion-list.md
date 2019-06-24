---
Description: 목록 애니메이션을 사용하면 사진 앨범이나 검색 결과 목록 같은 컬렉션에서 단일 항목이나 여러 항목을 삽입하거나 제거할 수 있습니다.
title: UWP 앱에서 애니메이션 추가 및 삭제
ms.assetid: A85006AE-4992-457a-B514-500B8BEF5DC8
label: Motion--add and delete animations
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d0dc449c0134799f3de675fff4bdbbda66046be8
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67317091"
---
# <a name="add-and-delete-animations"></a>추가 및 삭제 애니메이션



목록 애니메이션을 사용하면 사진 앨범이나 검색 결과 목록 같은 컬렉션에서 단일 항목이나 여러 항목을 삽입하거나 제거할 수 있습니다.

> **중요 API**: [**AddDeleteThemeTransition 클래스**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.adddeletethemetransition)


## <a name="dos-and-donts"></a>권장 사항 및 금지 사항


-   목록 애니메이션을 사용하여 기존 항목 집합에 단일 새 항목을 추가합니다. 예를 들어 새 메일이 도착하거나 새 사진을 기존 집합으로 가져올 때 사용합니다.
-   목록 애니메이션을 사용하여 한 번에 여러 항목을 집합에 추가합니다. 예를 들어 새 사진 집합을 기존 컬렉션으로 가져올 때 사용합니다. 여러 항목을 추가하거나 삭제할 경우 개별 개체에 대한 작업 간의 지연 없이 모두 동시에 이루어져야 합니다.
-   목록 추가 및 삭제 애니메이션은 쌍으로 사용합니다. 이러한 애니메이션 중 하나를 사용할 때마다 그 반대 작업에 다른 하나를 사용하면 됩니다.
-   요소 하나 또는 요소 그룹을 한꺼번에 추가하거나 삭제할 항목 목록에 목록 애니메이션을 사용하세요.
-   컨테이너를 표시하거나 제거할 때 목록 애니메이션을 사용하지 마세요. 이 애니메이션은 이미 표시되어 있는 컬렉션이나 집합의 멤버에 대해 사용해야 합니다. 앱 화면 위에 임시 컨테이너를 표시하거나 숨기려면 팝업 애니메이션을 사용합니다. 앱 화면의 일부인 컨테이너를 표시하거나 바꾸려면 콘텐츠 전환 애니메이션을 사용합니다.
-   전체 항목 집합을 대상으로 목록 애니메이션을 사용하지 마세요. 컨테이너 내에서 전체 컬렉션을 추가하거나 제거하려면 페이지 전환 애니메이션을 사용합니다.



## <a name="related-articles"></a>관련 문서

* [애니메이션 개요](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)
* [애니메이션 목록 추가 및 삭제](https://docs.microsoft.com/previous-versions/windows/apps/jj649430(v=win.10))
* [빠른 시작: 애니메이션 라이브러리 애니메이션을 사용 하 여 UI](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))
* [**AddDeleteThemeTransition 클래스**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.adddeletethemetransition)

 

 




