---
title: 분할 보기
ms.assetid: E9E4537F-1160-4183-9A83-26602FCFDC9A
description: 분할 보기 컨트롤에는 확장/축소 가능한 창 및 콘텐츠 영역이 있습니다.
label: 분할 보기
template: detail.hbs
---

# 분할 보기 컨트롤에 대한 지침


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


**중요 API**

-   [**SplitView 클래스(XAML)**](https://msdn.microsoft.com/library/windows/apps/dn864360)
-   [**SplitView 개체(HTML)**](https://msdn.microsoft.com/library/windows/apps/dn919970)

분할 보기 컨트롤에는 확장/축소 가능한 창 및 콘텐츠 영역이 있습니다. 콘텐츠 영역은 항상 표시됩니다. 창은 확장하거나 축소할 수 있으며, 앱 창의 왼쪽 또는 오른쪽에 표시할 수 있습니다. 창에는 다음과 같은 세 가지 모드가 있습니다.

-   **오버레이**

    창이 열릴 때까지 표시되지 않습니다. 열린 창은 콘텐츠 영역을 오버레이합니다.

-   **인라인**

    창이 항상 표시되며 콘텐츠 영역을 오버레이하지 않습니다. 창 및 콘텐츠 영역이 사용 가능한 화면 공간을 나눕니다.

-   **컴팩트**

    창이 항상 이 모드로 표시되며 아이콘을 표시할 정도의 공간(보통 48epx의 너비)만 사용합니다. 창 및 콘텐츠 영역이 사용 가능한 화면 공간을 나눕니다. 표준 컴팩트 모드에서는 콘텐츠를 오버레이하지 않지만 더 많은 콘텐츠를 표시하는 더 넓은 창으로 변환할 수 있으며, 이 경우 콘텐츠 영역을 오버레이합니다.

## <span id="Is_this_the_right_control_"> </span> <span id="is_this_the_right_control_"> </span> <span id="IS_THIS_THE_RIGHT_CONTROL_"> </span>올바른 컨트롤인가요?


분할 보기 컨트롤은 [탐색 창 패턴](nav-pane.md)을 만드는 데 사용할 수 있습니다. 이 패턴을 구성하려면 확장/축소 단추("햄버거" 단추) 및 목록 보기를 분할 보기 컨트롤에 추가해야 합니다.

## <span id="Examples"> </span> <span id="examples"> </span> <span id="EXAMPLES"> </span>예제


기본 양식의 분할 보기 컨트롤은 기본 컨테이너입니다. 단추 및 목록 보기가 추가된 분할 보기 컨트롤을 탐색 메뉴로 사용할 수 있습니다. 확장 및 압축 모드에서 탐색 메뉴로 분할 보기를 활용한 예제는 다음과 같습니다.

![오버레이 모드 및 컴팩트 모드에서 분할 보기 메뉴의 예](images/controls-splitview-menu01.png)
## <span id="Recommendations"> </span> <span id="recommendations"> </span> <span id="RECOMMENDATIONS"> </span>권장 사항


-   탐색 메뉴에 분할 보기를 사용하는 경우 앱의 다른 영역에 액세스하는 탐색 컨트롤을 창에 배치하는 것이 좋습니다. 탐색에 창을 사용하면 일관된 사용자 환경이 제공됩니다. 또한, 메뉴를 이렇게 구현하면 사용자가 앱의 모든 부분에 익숙해지는 데 도움이 되고 앱의 홈페이지에 빨리 액세스할 수 있으며 앱의 더 많은 영역을 더 쉽게 탐색할 수 있습니다.

\[이 문서에는 UWP(유니버설 Windows 플랫폼) 앱 및 Windows 10과 관련된 정보가 있습니다. Windows 8.1 참고 자료는 [Windows 8.1 지침 PDF](https://go.microsoft.com/fwlink/p/?linkid=258743)를 다운로드하세요.\]

## <span id="related_topics"> </span>관련 항목


* [탐색 창 패턴](nav-pane.md)
* [목록 보기](lists.md)
 

 






<!--HONumber=Mar16_HO1-->


