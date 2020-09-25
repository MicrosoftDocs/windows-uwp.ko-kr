---
Description: 목록 애니메이션을 사용 하면 사진 앨범이 나 검색 결과 목록과 같이 컬렉션에서 단일 항목 또는 여러 항목을 삽입 하거나 제거할 수 있습니다.
title: 애니메이션 추가 및 삭제
ms.assetid: A85006AE-4992-457a-B514-500B8BEF5DC8
label: Motion--add and delete animations
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 105b2fe5f7f267d8a5a82473332747584b02316c
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220376"
---
# <a name="add-and-delete-animations"></a>애니메이션 추가 및 삭제



목록 애니메이션을 사용 하면 사진 앨범이 나 검색 결과 목록과 같이 컬렉션에서 단일 항목 또는 여러 항목을 삽입 하거나 제거할 수 있습니다.

> **중요 한 api**: [ **AddDeleteThemeTransition 클래스**](/uwp/api/windows.ui.xaml.media.animation.adddeletethemetransition)


## <a name="dos-and-donts"></a>권장 사항 및 금지 사항


-   목록 애니메이션을 사용 하 여 기존 항목 집합에 새 항목 하나를 추가 합니다. 예를 들어 새 전자 메일이 도착 하거나 새 사진을 기존 집합으로 가져올 때이를 사용 합니다.
-   목록 애니메이션을 사용 하 여 한 번에 여러 항목을 집합에 추가 합니다. 예를 들어 새 사진 집합을 기존 컬렉션으로 가져올 때 이러한 항목을 사용 합니다. 여러 항목을 추가 하거나 삭제 하는 작업은 개별 개체에 대 한 작업 사이에서 지연 없이 동시에 수행 되어야 합니다.
-   목록 애니메이션 추가 및 삭제를 쌍으로 사용 합니다. 이러한 애니메이션 중 하나를 사용할 때마다 반대의 동작에 해당 하는 애니메이션을 사용 합니다.
-   하나 이상의 요소 또는 요소 그룹을 한 번에 추가 하거나 삭제할 수 있는 항목 목록과 함께 목록 애니메이션을 사용 합니다.
-   컨테이너를 표시 하거나 제거 하는 데 목록 애니메이션을 사용 하지 않습니다. 이러한 애니메이션은 이미 표시 된 컬렉션 또는 집합의 멤버를 위한 것입니다. 팝업 애니메이션을 사용 하 여 앱 화면 위에 임시 컨테이너를 표시 하거나 숨깁니다. 콘텐츠 전환 애니메이션을 사용 하 여 앱 표면의 일부인 컨테이너를 표시 하거나 바꿉니다.
-   전체 항목 집합에서 목록 애니메이션을 사용 하지 마세요. 콘텐츠 전환 애니메이션을 사용 하 여 컨테이너 내에서 전체 컬렉션을 추가 하거나 제거 합니다.



## <a name="related-articles"></a>관련된 문서

* [애니메이션 개요](./xaml-animation.md)
* [목록 추가 및 삭제에 애니메이션 적용](/previous-versions/windows/apps/jj649430(v=win.10))
* [빠른 시작: 라이브러리 애니메이션을 사용 하 여 UI에 애니메이션 효과 주기](/previous-versions/windows/apps/hh452703(v=win.10))
* [**AddDeleteThemeTransition 클래스**](/uwp/api/windows.ui.xaml.media.animation.adddeletethemetransition)

 

 
