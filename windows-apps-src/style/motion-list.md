---
Description: 목록 애니메이션을 사용하면 사진 앨범이나 검색 결과 목록 같은 컬렉션에서 단일 항목이나 여러 항목을 삽입하거나 제거할 수 있습니다.
title: UWP 앱에서 애니메이션 추가 및 삭제
ms.assetid: A85006AE-4992-457a-B514-500B8BEF5DC8
label: Motion--add and delete animations
template: detail.hbs
---

# 추가 및 삭제 애니메이션


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

목록 애니메이션을 사용하면 사진 앨범이나 검색 결과 목록 같은 컬렉션에서 단일 항목이나 여러 항목을 삽입하거나 제거할 수 있습니다.

**중요 API**

-   [**AddDeleteThemeTransition 클래스**](https://msdn.microsoft.com/library/windows/apps/br243048)


## 권장 사항 및 금지 사항


-   목록 애니메이션을 사용하여 기존 항목 집합에 단일 새 항목을 추가합니다. 예를 들어 새 메일이 도착하거나 새 사진을 기존 집합으로 가져올 때 사용합니다.
-   목록 애니메이션을 사용하여 한 번에 여러 항목을 집합에 추가합니다. 예를 들어 새 사진 집합을 기존 컬렉션으로 가져올 때 사용합니다. 여러 항목을 추가하거나 삭제할 경우 개별 개체에 대한 작업 간의 지연 없이 모두 동시에 이루어져야 합니다.
-   목록 추가 및 삭제 애니메이션은 쌍으로 사용합니다. 이러한 애니메이션 중 하나를 사용할 때마다 그 반대 작업에 다른 하나를 사용하면 됩니다.
-   요소 하나 또는 요소 그룹을 한꺼번에 추가하거나 삭제할 항목 목록에 목록 애니메이션을 사용하세요.
-   컨테이너를 표시하거나 제거할 때 목록 애니메이션을 사용하지 마세요. 이 애니메이션은 이미 표시되어 있는 컬렉션이나 집합의 멤버에 대해 사용해야 합니다. 앱 화면 위에 임시 컨테이너를 표시하거나 숨기려면 팝업 애니메이션을 사용합니다. 앱 화면의 일부인 컨테이너를 표시하거나 바꾸려면 콘텐츠 전환 애니메이션을 사용합니다.
-   전체 항목 집합을 대상으로 목록 애니메이션을 사용하지 마세요. 컨테이너 내에서 전체 컬렉션을 추가하거나 제거하려면 페이지 전환 애니메이션을 사용합니다.

\[이 문서에는 UWP(유니버설 Windows 플랫폼) 앱 및 Windows 10과 관련된 정보가 있습니다. Windows 8.1 참고 자료는 [Windows 8.1 지침 PDF](https://go.microsoft.com/fwlink/p/?linkid=258743)를 다운로드하세요.\]

## 관련 문서


**개발자용(XAML)**
* [애니메이션 개요](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [목록 추가 및 삭제 애니메이션](https://msdn.microsoft.com/library/windows/apps/xaml/jj649430)
* [빠른 시작: 라이브러리 애니메이션을 사용하여 UI에 애니메이션 효과 주기](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**AddDeleteThemeTransition 클래스**](https://msdn.microsoft.com/library/windows/apps/br243048)

 

 






<!--HONumber=Mar16_HO3-->


