---
ms.assetid: 333f67f5-f012-4981-917f-c6fd271267c6
description: 이 사례 연구는 SemanticZoom 컨트롤에 그룹화 된 데이터를 표시 하는 Universal 8.1 앱으로 시작 하는 Bookstore1에 제공 된 정보를 기반으로 합니다.
title: Windows 런타임 .x에서 UWP 사례 연구 Bookstore2
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b2681eb1fd14ed7e86c00054c5f5c7ba3efb97c6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167617"
---
# <a name="windows-runtime-8x-to-uwp-case-study-bookstore2"></a>Windows 런타임 .x에서 UWP 사례 연구: Bookstore2


[Bookstore1](w8x-to-uwp-case-study-bookstore1.md)에 제공 된 정보를 기반으로 하는이 사례 연구는 [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 컨트롤에 그룹화 된 데이터를 표시 하는 Universal 8.1 앱으로 시작 합니다. 뷰 모델에서 클래스 **작성자** 의 각 인스턴스는 해당 저자가 작성 한 책의 그룹을 나타내며, **SemanticZoom**에서 저자 별로 그룹화 된 책의 목록을 볼 수도 있고, 축소 하 여 저자의 점프 목록을 볼 수도 있습니다. 점프 목록은 책 목록을 스크롤할 때보다 훨씬 더 빠른 탐색이 가능케 합니다. 앱을 UWP (Windows 10 유니버설 Windows 플랫폼) 앱으로 이식 하는 단계를 안내 합니다.

**참고**    \_Visual studio에서 Bookstore2Universal 10을 열 때 "Visual studio 업데이트 필요" 메시지가 표시 되 면 [Targetplatformversion](w8x-to-uwp-troubleshooting.md)의 단계를 따릅니다.

## <a name="downloads"></a>다운로드

[Bookstore2 \_ 81 Universal 8.1 앱을 다운로드](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore2_81)합니다.

[Bookstore2Universal \_ 10 Windows 10 앱을 다운로드](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore2Universal_10)합니다.

## <a name="the-universal-81-app"></a>유니버설 8.1 앱

Bookstore2 \_ 81은 다음과 같으며, 포트를 사용할 앱은 다음과 같습니다. 작성자 별로 그룹화 된 책을 보여 주는 가로 스크롤 (Windows Phone에서 세로 스크롤) [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) . 점프 목록을 축소 하 고 원하는 그룹으로 다시 이동할 수 있습니다. 이 앱에는 그룹화 된 데이터 원본을 제공 하는 뷰 모델과 해당 뷰 모델에 바인딩되는 사용자 인터페이스가 있습니다. 여기에서 볼 수 있듯이 이러한 두 부분은 모두 WinRT 8.1 기술에서 Windows 10으로 쉽게 이식할 수 있습니다.

![\-windows의 bookstore2 81, 확대 보기](images/w8x-to-uwp-case-studies/c02-01-win81-zi-how-the-app-looks.png)

\_Windows의 Bookstore2 81, 확대 보기
 

![\-windows의 bookstore2 81, 축소 보기](images/w8x-to-uwp-case-studies/c02-02-win81-zo-how-the-app-looks.png)

\_Windows의 Bookstore2 81, 축소 보기

![\-windows phone의 bookstore2 81, 확대 보기](images/w8x-to-uwp-case-studies/c02-03-wp81-zi-how-the-app-looks.png)

Bookstore2 \_ 81 on Windows Phone, 확대 보기

![\-windows phone의 bookstore2 81, 축소 보기](images/w8x-to-uwp-case-studies/c02-04-wp81-zo-how-the-app-looks.png)

Bookstore2 \_ 81 on Windows Phone, 확대 보기

##  <a name="porting-to-a-windows10-project"></a>Windows 10 프로젝트로 포팅

Bookstore2 \_ 81 솔루션은 8.1 유니버설 앱 프로젝트입니다. Bookstore2 \_ 프로젝트는 Windows 8.1에 대 한 앱 패키지를 빌드하고 Bookstore2 \_ appname.windowsphone 프로젝트는 8.1 Windows Phone에 대 한 앱 패키지를 빌드합니다. Bookstore2 \_ 81. Shared는 다른 두 프로젝트 모두에서 사용 되는 소스 코드, 태그 파일 및 기타 자산 및 리소스를 포함 하는 프로젝트입니다.

이전 사례 연구와 마찬가지로이 옵션 ( [유니버설 8.1 앱](w8x-to-uwp-root.md)이 있는 경우에 설명 된 항목)은 공유 프로젝트의 콘텐츠를 유니버설 장치 제품군을 대상으로 하는 Windows 10으로 이식 하는 것입니다.

새 빈 응용 프로그램 (Windows 유니버설) 프로젝트를 만들어 시작 합니다. 이름을 Bookstore2Universal \_ 10으로 합니다. Bookstore2 \_ 81에서 Bookstore2Universal 10으로 복사할 파일 \_ 입니다.

**공유 프로젝트에서**

-   책 표지 이미지를 포함 하는 폴더를 복사 합니다. PNG 파일 \\ ( \\ CoverImages 폴더)입니다. 폴더를 복사한 후 **솔루션 탐색기**에서 **모든 파일 표시** 가 설정 되어 있는지 확인 합니다. 복사한 폴더를 마우스 오른쪽 단추로 클릭 하 고 **프로젝트에 포함**을 클릭 합니다. 이 명령은 프로젝트의 파일 또는 폴더를 "포함" 하 여 의미 합니다. 파일이 나 폴더를 복사할 때마다 각 복사본은 **솔루션 탐색기** 에서 **새로 고침** 을 클릭 한 다음 프로젝트에 파일 또는 폴더를 포함 합니다. 대상에서 대체 하는 파일에 대해서는이 작업을 수행할 필요가 없습니다.
-   뷰 모델 원본 파일이 포함 된 폴더를 복사 \\ 합니다 (ViewModel).
-   MainPage을 복사 하 고 대상의 파일을 바꿉니다.

**Windows 프로젝트에서**

-   BookstoreStyles을 복사 합니다. 이 파일의 모든 리소스 키가 Windows 10 앱에서 확인 되기 때문에이를 적절 한 시작 지점으로 사용 합니다. 해당 하는 Appname.windowsphone 파일에 있는 일부는 그렇지 않습니다.
-   SeZoUC. .xaml 및 SeZoUC.xaml.cs을 복사 합니다. 이 보기의 Windows 버전부터 시작 합니다 .이 보기는 와이드 창에 적합 하 고, 나중에 작은 창 및 그 보다 작은 장치에 맞게 조정 됩니다.

방금 복사한 소스 코드 및 태그 파일을 편집 하 고 Bookstore2 81 네임 스페이스에 대 한 참조를 \_ Bookstore2Universal 10으로 변경 합니다 \_ . 이 작업을 수행 하는 빠른 방법은 **파일에서 바꾸기** 기능을 사용 하는 것입니다. 뷰 모델에는 코드 변경이 필요 하지 않으며 다른 명령적 코드에도 필요 하지 않습니다. 그러나 실행 중인 앱의 버전을 보다 쉽게 확인할 수 있도록 하려면 **Bookstore2Universal \_ ** 속성에서 반환 하는 값을 "Bookstore2 \_ 81"에서 "Bookstore2Universal 10"으로 변경 합니다 \_ .

이제를 빌드하고 실행할 수 있습니다. 다음은 아직 작업을 수행 하지 않고 Windows 10으로 이식할 수 있는 새 UWP 앱의 모습입니다.

![바탕 화면 장치에서 실행 되는 초기 소스 코드 변경 내용이 포함 된 windows 10 앱, 확대 된 보기](images/w8x-to-uwp-case-studies/c02-05-desk10-zi-initial-source-code-changes.png)

바탕 화면 장치에서 실행 되는 초기 소스 코드 변경 내용이 포함 된 Windows 10 앱, 확대 된 보기

![데스크톱 장치에서 실행 되는 초기 소스 코드 변경 내용이 포함 된 windows 10 앱 (축소 된 보기)](images/w8x-to-uwp-case-studies/c02-06-desk10-zo-initial-source-code-changes.png)

데스크톱 장치에서 실행 되는 초기 소스 코드 변경 내용이 포함 된 Windows 10 앱 (축소 된 보기)

보기 모델 및 확대 된 뷰 및 확대 뷰가 제대로 작동 하는 것은 아니지만이를 확인 하는 문제가 있습니다. 한 가지 문제는 [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 스크롤되지 않는다는 것입니다. 이는 Windows 10에서 [**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView) 의 기본 스타일이 세로로 배치 되기 때문입니다. 즉, windows 10 디자인 지침에서는 새로운 및 이식 된 앱에서 해당 방법을 사용 하는 것이 좋습니다. 하지만, Bookstore2 81 프로젝트 (8.1 앱 용으로 설계 됨)에서 복사한 사용자 지정 항목 패널 템플릿의 가로 스크롤 설정은 \_ windows 10 앱으로 이식 된 결과로 적용 되는 windows 10 기본 스타일의 세로 스크롤 설정과 충돌 합니다. 두 번째는 앱이 사용자 인터페이스를 아직 조정 하지 않아 크기가 다양 한 windows 및 소형 장치에서 최상의 환경을 제공 하지 않는다는 것입니다. 그리고, thirdly는 올바른 스타일 및 브러시를 아직 사용 하지 않아 텍스트를 많이 표시 하지 않습니다 (축소 하기 위해 클릭할 수 있는 그룹 머리글 포함). 따라서 다음 세 섹션 ([SemanticZoom 및 GridView 디자인 변경 내용](#semanticzoom-and-gridview-design-changes), [적응 UI](#adaptive-ui)및 [범용 스타일](#universal-styling))에서는 이러한 세 가지 문제를 해결 합니다.

## <a name="semanticzoom-and-gridview-design-changes"></a>SemanticZoom 및 GridView 디자인 변경 내용

[**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 컨트롤에 대 한 Windows 10의 디자인 변경 내용은 [SemanticZoom changes](w8x-to-uwp-porting-xaml-and-ui.md)섹션에 설명 되어 있습니다. 이러한 변경에 대 한 응답으로이 섹션에서 수행할 작업은 없습니다.

[**Gridview**](/uwp/api/Windows.UI.Xaml.Controls.GridView) 에 대 한 변경 내용은 [gridview/ListView 변경 내용](w8x-to-uwp-porting-xaml-and-ui.md)섹션에 설명 되어 있습니다. 아래에 설명 된 대로 이러한 변경 사항에 맞게 조정 하기 위해 몇 가지 사소한 조정이 있습니다.

-   SeZoUC .xaml의에서 `ZoomedInItemsPanelTemplate` 및를 설정 `Orientation="Horizontal"` `GroupPadding="0,0,0,20"` 합니다.
-   Sezouc.xaml에서 `ZoomedOutItemsPanelTemplate` `ItemsPanel` 축소 된 뷰에서 특성을 삭제 하 고 제거 합니다.

정말 간단하죠!

## <a name="adaptive-ui"></a>적응 UI

이렇게 변경한 후에는 응용 프로그램이 와이드 창에서 실행 되는 경우 (큰 화면을 사용 하는 장치 에서만 가능),이를 위해 SeZoUC. 앱의 창이 좁은 경우 (작은 장치에서 발생 하 고 큰 장치 에서도 발생할 수 있음) Windows Phone 스토어 앱에 있던 UI는 가장 적합 합니다.

적응 시각적 상태 관리자 기능을 사용 하 여이를 달성할 수 있습니다. 기본적으로 Windows Phone 스토어 앱에서 사용 하는 더 작은 템플릿을 사용 하 여 UI가 좁은 상태로 배치 되도록 시각적 요소에 대 한 속성을 설정 합니다. 그런 다음 앱의 창이 특정 크기 ( [유효 픽셀](w8x-to-uwp-porting-xaml-and-ui.md)단위로 측정 됨) 보다 크거나 같은 경우를 검색 하 고,이에 대 한 응답으로 시각적 요소의 속성을 변경 하 여 더 크고 더 넓은 레이아웃을 가져옵니다. 이러한 속성 변경 내용을 시각적 상태로 설정 하 고, 적응 트리거를 사용 하 여 해당 시각적 상태를 적용 하는지 여부를 지속적으로 모니터링 하 고 해당 시각적 상태를 적용 하는지 여부를 결정 합니다. 이 경우 창 너비를 트리거하기는 하지만 창 높이를 트리거할 수도 있습니다.

최소 창 너비 548 window.epx.codesnippet는이 사용 사례에 적합 합니다. 가장 작은 장치의 크기 이므로 넓은 레이아웃을 표시 하고자 합니다. 휴대폰은 일반적으로 548 window.epx.codesnippet 보다 작으므로 기본 좁은 레이아웃을 유지 하는 것 처럼 작은 장치에 있습니다. PC에서 창은 전체 상태로 전환 하기에 충분 하 게 기본적으로 시작 됩니다. 여기에서 250x250 크기 항목의 두 열을 표시 하기에 충분히 좁게 창을 끌 수 있습니다. 이 보다 약간 좁아서, 트리거는 비활성화 되 고, 가로 시각적 상태는 제거 되 고, 기본 좁은 레이아웃은 적용 됩니다.

따라서 이러한 두 가지 레이아웃을 얻으려면 설정 하 고 변경 해야 하는 속성은 무엇 인가요? 두 가지 대안이 있으며 각각 다른 방법을 사용 합니다.

1.  태그에 두 개의 [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 컨트롤을 배치할 수 있습니다. 하나는 Windows 런타임 8.x 앱에서 사용 중인 태그의 복사본 ( [**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView) 컨트롤을 사용 하 여)이 고 기본적으로 축소 되어 있습니다. 다른는 Windows Phone 스토어 앱에서 사용 하 고 있는 태그의 복사본이 며, 기본적으로 표시 되는 [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) 컨트롤을 사용 합니다. 시각적 상태는 두 **SemanticZoom** 컨트롤의 표시 유형 속성을 전환 합니다. 이 작업을 수행 하는 데는 거의 노력 하지 않아도 되지만 일반적으로 고성능 기술이 아닙니다. 따라서 사용 하는 경우 앱을 프로 파일링 하 고 여전히 성능 목표를 충족 하 고 있는지 확인 해야 합니다.
2.  [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) 컨트롤을 포함 하는 단일 [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 를 사용할 수 있습니다. 넓은 시각적 상태에서 두 가지 레이아웃을 얻기 위해 적용 되는 템플릿을 포함 하 여 **ListView** 컨트롤의 속성을 변경 하 여 [**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView) 와 동일한 방식으로 레이아웃을 지정할 수 있습니다. 이렇게 하면 성능이 향상 될 수 있지만 **GridView** 와 **ListView** 의 다양 한 스타일과 템플릿과 다양 한 항목 유형 사이에서 많은 작은 차이가 있습니다. 이 솔루션은이 시점에서 기본 스타일 및 템플릿이 설계 되는 방식과 긴밀 하 게 결합 되어 있으며,이를 통해 나중에 기본값을 변경 하는 것에 영향을 주는 문제를 해결 하는 솔루션을 제공 합니다.

이 사례 연구에서는 첫 번째 대안을 살펴보겠습니다. 하지만 원하는 경우 두 번째 작업을 시도 하 여 더 잘 작동 하는지 확인할 수 있습니다. 첫 번째 대안을 구현 하기 위해 수행 해야 하는 단계는 다음과 같습니다.

-   새 프로젝트의 태그에 있는 [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 에서 및를 설정 합니다 `x:Name="wideSeZo"` `Visibility="Collapsed"` .
-   Bookstore2 \_ appname.windowsphone 프로젝트로 돌아가서 SeZoUC를 엽니다. 해당 파일에서 [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 요소 태그를 복사 하 여 `wideSeZo` 새 프로젝트의 바로 뒤에 붙여 넣습니다. `x:Name="narrowSeZo"`방금 붙여 넣은 요소에 설정 합니다.
-   그러나 `narrowSeZo` 아직 복사 하지 않은 몇 가지 스타일이 필요 합니다. Bookstore2 Appname.windowsphone에서 다시 \_ , 두 스타일 ( `AuthorGroupHeaderContainerStyle` 및 `ZoomedOutAuthorItemContainerStyle` )을 SeZoUC에 복사 하 고 새 프로젝트의 BookstoreStyles에 붙여넣습니다.
-   이제 새 SeZoUC에 두 개의 [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 요소가 있습니다. 이러한 두 요소를 **표로**래핑합니다.
-   새 프로젝트의 BookstoreStyles에서 `Wide` 이 세 가지 리소스 키에 단어를 추가 하 고 (내부 참조에만 해당 `wideSeZo` ) `AuthorGroupHeaderTemplate` , 및를 참조 `ZoomedOutAuthorTemplate` `BookTemplate` 하세요.
-   Bookstore2 \_ appname.windowsphone 프로젝트에서 BookstoreStyles를 엽니다. 이 파일에서 위의 세 가지 리소스 (위에서 언급 한 것으로 설명)와 두 개의 점프 목록 항목 변환기를 복사 하 고, 네임 스페이스 접두사 선언 Windows \_ UI \_ Xaml은 \_ \_ 기본 형식을 제어 하 고 새 프로젝트의 BookstoreStyles에 모두 붙여넣습니다.
-   마지막으로 새 프로젝트의 Sezouc.xaml에서 위에 추가한 **표에** 적절 한 시각적 상태 관리자 태그를 추가 합니다.

```xml
    <Grid>
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState x:Name="WideState">
                    <VisualState.StateTriggers>
                        <AdaptiveTrigger MinWindowWidth="548"/>
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Target="wideSeZo.Visibility" Value="Visible"/>
                        <Setter Target="narrowSeZo.Visibility" Value="Collapsed"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

    ...

    </Grid>
```

## <a name="universal-styling"></a>유니버설 스타일 지정

이제 이전 프로젝트에서 복사 하는 동안 위에서 소개한 스타일 문제를 포함 하 여 일부 스타일 문제를 수정 하겠습니다.

-   MainPage에서 `LayoutRoot` 의 배경을로 `"{ThemeResource ApplicationPageBackgroundThemeBrush}"` 변경 합니다.
-   BookstoreStyles에서 리소스의 값을 `TitlePanelMargin` 로 설정 `0` 하거나 적절 한 값을 표시 합니다.
-   Sezouc.xaml에서의 여백을로 설정 하거나, `wideSeZo` 적절 한 값을 표시 `0` 합니다.
-   BookstoreStyles에서의 Margin 특성을 제거 합니다 `AuthorGroupHeaderTemplateWide` .
-   에서 FontFamily 특성을 제거 `AuthorGroupHeaderTemplate` `ZoomedOutAuthorTemplate` 합니다.
-   Bookstore2 \_ 81는 `BookTemplateTitleTextBlockStyle` , `BookTemplateAuthorTextBlockStyle` 및 `PageTitleTextBlockStyle` 리소스 키를 간접 참조로 사용 하 여 단일 키가 두 앱에서 서로 다른 구현을 갖도록 했습니다. 해당 간접 참조가 필요 하지 않습니다. 시스템 스타일을 직접 참조할 수 있습니다. 따라서 앱 전체에서, 및로 참조를 `TitleTextBlockStyle` 대체 `CaptionTextBlockStyle` `HeaderTextBlockStyle` 합니다. Visual Studio **파일에서 바꾸기** 기능을 사용 하 여 빠르고 정확 하 게이 작업을 수행할 수 있습니다. 그런 다음 사용 하지 않는 세 리소스를 삭제할 수 있습니다.
-   에서를로 바꾸고,를 사용 하 여 `AuthorGroupHeaderTemplate` `PhoneAccentBrush` `SystemControlBackgroundAccentBrush` TextBlock에 설정 하 고, `Foreground="White"` 모바일 장치 제품군에서 실행 될 때 올바르게 표시 되도록 합니다. **TextBlock**
-   에서 `BookTemplateWide` 두 번째 TextBlock의 전경 특성을 첫 번째 **TextBlock** 에 복사 합니다.
-   에서에 대 한 참조를에 대 한 `ZoomedOutAuthorTemplateWide` `SubheaderTextBlockStyle` 참조 (이제는 조금 큼)로 변경 `SubtitleTextBlockStyle` 합니다.
-   확대 된 보기 (점프 목록)는 더 이상 새 플랫폼에서 확대 된 보기를 겹쳐서 표시 하지 않으므로 `Background` 의 축소 된 보기에서 특성을 제거할 수 있습니다 `narrowSeZo` .
-   모든 스타일 및 템플릿이 하나의 파일에 있도록 하려면 `ZoomedInItemsPanelTemplate` SeZoUC. .xaml에서 BookstoreStyles로 이동 합니다.

스타일 지정 작업의 마지막 시퀀스는 앱을 다음과 같이 그대로 둡니다.

![데스크톱 장치에서 실행 되는 이식 된 windows 10 앱, 확대 된 보기, 2 가지 크기의 창](images/w8x-to-uwp-case-studies/c02-07-desk10-zi-ported.png)

데스크톱 장치에서 실행 되는 이식 된 Windows 10 앱, 확대 된 보기, 2 가지 크기의 창

![데스크톱 장치에서 실행 되는 이식 된 windows 10 앱, 축소 된 보기, 2 가지 크기의 창](images/w8x-to-uwp-case-studies/c02-08-desk10-zo-ported.png)

데스크톱 장치에서 실행 되는 이식 된 Windows 10 앱, 축소 된 보기, 2 가지 크기의 창

![모바일 장치에서 실행 되는 이식 된 windows 10 앱, 확대 된 보기](images/w8x-to-uwp-case-studies/c02-09-mob10-zi-ported.png)

모바일 장치에서 실행 되는 이식 된 Windows 10 앱, 확대 된 보기

![모바일 장치에서 실행 되는 이식 된 windows 10 앱, 축소 된 보기](images/w8x-to-uwp-case-studies/c02-10-mob10-zo-ported.png)

모바일 장치에서 실행 되는 이식 된 Windows 10 앱, 축소 된 보기

## <a name="conclusion"></a>결론

이 사례 연구는 이전 보다 더 원대한 사용자 인터페이스를 포함 합니다. 이전 사례 연구와 마찬가지로이 특정 뷰 모델은 전혀 작동 하지 않으며 주로 사용자 인터페이스를 리팩터링 하는 데 사용 됩니다. 일부 변경 내용은 여러 폼 팩터를 지 원하는 동안 두 프로젝트를 하나로 결합 하는 데 필요한 결과 였습니다 (실제로는 이전 보다 훨씬 많은 형태). 플랫폼에 적용 된 변경 내용과 관련 하 여 몇 가지 사항이 변경 되었습니다.

다음 사례 연구는 그룹화 된 데이터에 액세스 하 고 표시 하는 것을 [QuizGame](w8x-to-uwp-case-study-quizgame.md).