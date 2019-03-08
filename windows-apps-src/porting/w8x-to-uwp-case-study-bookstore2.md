---
ms.assetid: 333f67f5-f012-4981-917f-c6fd271267c6
description: 이 사례 연구는 SemanticZoom 컨트롤에서 그룹화된 데이터를 표시하는 유니버설 8.1 앱으로 시작하는 Bookstore1에 제공된 정보를 기반으로 합니다.
title: Windows 런타임 8.x에서 UWP로 이동 사례 연구 Bookstore2
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a81980bc03a272cb2be0e66772591f4e395d7722
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662208"
---
# <a name="windows-runtime-8x-to-uwp-case-study-bookstore2"></a>Windows 런타임 8.x UWP 사례 연구: Bookstore2


[Bookstore1](w8x-to-uwp-case-study-bookstore1.md)에 제공된 정보를 기반으로 하는 이 사례 연구는 [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) 컨트롤에서 그룹화된 데이터를 표시하는 유니버설 8.1 앱으로 시작합니다. 보기 모델에서 **Author** 클래스의 각 인스턴스는 해당 저자가 쓴 책의 그룹을 나타내며, **SemanticZoom**에서 저자가 그룹화한 책 목록을 보거나 저자의 점프 목록을 축소할 수 있습니다. 점프 목록은 책 목록을 스크롤할 때보다 훨씬 더 빠른 탐색이 가능케 합니다. Windows 10 유니버설 Windows 플랫폼 (UWP) 앱에 앱을 이식 하는 단계를 안내 합니다.

**참고**    Bookstore2Universal 열면\_10 메시지가 "Visual Studio 업데이트 필요"를 표시 하는 경우 Visual Studio에서 다음의 단계에 따라 [TargetPlatformVersion](w8x-to-uwp-troubleshooting.md)합니다.

## <a name="downloads"></a>다운로드

[다운로드는 Bookstore2\_81 유니버설 8.1 앱](https://go.microsoft.com/fwlink/?linkid=532951)합니다.

[다운로드는 Bookstore2Universal\_10 Windows 10 앱](https://go.microsoft.com/fwlink/?linkid=532952)합니다.

## <a name="the-universal-81-app"></a>유니버설 8.1 앱

어떤 Bookstore2 같습니다\_81-포트에는 앱-것 같습니다. 가로로 스크롤되는(Windows Phone에서는 세로로 스크롤됨)는 [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601)으로, 작성자로 그룹화된 책을 표시합니다. 점프 목록으로 축소할 수 있으며 점프 목록에서 어떤 그룹으로도 다시 이동할 수 있습니다. 이 앱에는 두 가지 주요 특징이 있습니다. 그룹화된 데이터 원본를 제공하는 보기 모델 및 보기 모델에 바인딩하는 사용자 인터페이스가 그것입니다. 앞으로 살펴보겠지만, Windows 10으로 WinRT 8.1 기술에서 쉽게 포트를 부분을 둘 다.

![bookstore2\-windows에서 확대 된 뷰는 81](images/w8x-to-uwp-case-studies/c02-01-win81-zi-how-the-app-looks.png)

Bookstore2\_Windows, 확대 된 보기에는 81
 

![bookstore2\-81 windows에서 축소 된 보기](images/w8x-to-uwp-case-studies/c02-02-win81-zo-how-the-app-looks.png)

Bookstore2\_Windows에서 축소 된 보기에는 81

![bookstore2\-81 windows phone, 확대 된 뷰](images/w8x-to-uwp-case-studies/c02-03-wp81-zi-how-the-app-looks.png)

Bookstore2\_확대 된 뷰, Windows Phone 81

![bookstore2\-81 windows phone, 축소 된 보기](images/w8x-to-uwp-case-studies/c02-04-wp81-zo-how-the-app-looks.png)

Bookstore2\_Windows Phone, 축소 된 보기에는 81

##  <a name="porting-to-a-windows10-project"></a>Windows 10 프로젝트에 이식

Bookstore2\_81 방법은 8.1 범용 앱 프로젝트를 합니다. Bookstore2\_81. Windows 프로젝트를 Bookstore2 및 Windows 8.1 대 한 앱 패키지를 빌드\_81. WindowsPhone 프로젝트 Windows Phone 8.1 용 앱 패키지를 빌드합니다. Bookstore2\_81. 공유는 소스 코드, 태그 파일 및 기타 자산 및 다른 두 프로젝트 모두에 의해 사용 되는 리소스를 포함 하는 프로젝트입니다.

마찬가지로 이전 사례 연구를 사용 하 여 옵션을 살펴보겠습니다 (에 설명 된 것입니다 [유니버설 8.1 앱이 있는 경우](w8x-to-uwp-root.md))를 Windows 10 유니버설 장치 제품군을 대상으로 하는 공유 프로젝트의 내용을 이식 하는 것입니다.

비어 있는 응용 프로그램(Windows 유니버설) 프로젝트를 새로 만들어 시작합니다. Bookstore2Universal 이름을\_10입니다. 이러한 파일은 Bookstore2에서 복사할 파일\_Bookstore2Universal 81\_10입니다.

**공유 프로젝트에서**

-   책 표지 이미지 PNG 파일을 포함 하는 폴더를 복사 합니다. (폴더가 \\자산\\CoverImages). 폴더를 복사한 후에 **솔루션 탐색기**에서 **모든 파일 표시**가 설정되어 있는지 확인합니다. 복사한 폴더를 마우스 오른쪽 단추로 클릭하고 **프로젝트에 포함**을 클릭합니다. 이 명령은 파일이나 폴더를 프로젝트에 "포함"하여 우리가 의도한 작업을 진행합니다. 파일이나 폴더를 복사할 때마다 **솔루션 탐색기**에서 **새로 고침**을 클릭한 다음 프로젝트에 파일 또는 폴더를 포함합니다. 대상에서 바꾸려는 파일에 대해서는 이 작업을 수행하지 않아도 됩니다.
-   보기 모델 원본 파일을 포함 하는 폴더에 복사 (폴더는 \\ViewModel).
-   MainPage.xaml을 복사한 후 대상의 파일을 바꿉니다.

**Windows 프로젝트에서**

-   BookstoreStyles.xaml을 복사합니다. Windows 10 앱에서이 파일에 있는 모든 리소스 키를 해결할 수 있으므로이 좋은 시작 지점으로 하나를 사용 하겠습니다. 동등한 WindowsPhone 파일의 일부 수신 되지 않습니다.
-   SeZoUC.xaml 및 SeZoUC.xaml.cs를 복사합니다. 넓은 창에 적합한 이 보기의 Windows 버전으로 시작한 후 나중에 작은 창에 맞게 그리고 결과적으로 더 작은 디바이스에 맞게 조정합니다.

방금 복사한 소스 코드와 태그 파일을 편집 하 고는 Bookstore2에 대 한 참조를 변경\_Bookstore2Universal 81 네임 스페이스\_10입니다. **파일에서 바꾸기** 기능을 사용하면 이 작업을 빠르게 수행할 수 있습니다. 보기 모델과 다른 명령적 코드에서 코드를 변경할 필요가 없습니다. 반환 된 값을 변경 하지만 실행 중인 앱의 버전을 보기 쉽게 확인 하기 위해 합니다 **Bookstore2Universal\_10.BookstoreViewModel.AppName** 속성을 "Bookstore2\_81"를 " BOOKSTORE2UNIVERSAL\_10"입니다.

이제 앱을 빌드 및 실행할 수 있습니다. Windows 10으로 포트를 아직 없는 작업 완료 후 새 UWP 앱의 모양을 다음과 같습니다.

![데스크톱 디바이스에서 실행 중인 초기 소스 코드가 변경된 Windows 10 앱, 확대 보기](images/w8x-to-uwp-case-studies/c02-05-desk10-zi-initial-source-code-changes.png)

초기 소스 코드를 사용 하 여 Windows 10 앱을 변경 확대 된 뷰는 데스크톱 장치에서 실행

![데스크톱 디바이스에서 실행 중인 초기 소스 코드가 변경된 Windows 10 앱, 축소 보기](images/w8x-to-uwp-case-studies/c02-06-desk10-zo-initial-source-code-changes.png)

축소 된 보기 데스크톱 장치에서 실행 되는 초기 소스 코드를 사용 하 여 Windows 10 앱 변경

보는 것이 약간 어려워지는 문제가 있긴 하지만 보기 모델, 확대 및 축소 보기는 다 함께 올바르게 작동됩니다. 한 가지 문제는 [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601)이 스크롤되지 않는 것입니다. 이므로 Windows 10에서의 기본 스타일을 [ **GridView** ](https://msdn.microsoft.com/library/windows/apps/br242705) 세로로 배치 되도록 (및 Windows 10 디자인 지침을 사용 하 여에서 이런 방식으로 새 및 가져올된 앱 권장). 하지만 가로 스크롤을 Bookstore2에서 복사한 사용자 지정 항목 패널 템플릿의 설정을\_81 프로젝트 (의 8.1 용으로 디자인 된 앱) 되는 Windows 10 기본 스타일의 세로 스크롤 설정과 충돌 Windows 10 앱에 이식 하지 우리 결과로 적용 합니다. 두 번째 문제는 앱이 다양한 크기의 창과 작은 디바이스에서 최상의 환경을 제공하도록 해당 사용자 인터페이스를 아직 조정하지 않은 것입니다. 세 번째 문제는 올바른 스타일 및 브러시가 사용되지 않아 텍스트의 상당 부분(축소하기 위해 클릭할 수 있는 그룹 헤더를 포함하여)이 표시되지 않는 것입니다. 따라서 다음 세 섹션([SemanticZoom 및 GridView 디자인 변경](#semanticzoom-and-gridview-design-changes), [적응 UI](#adaptive-ui), [범용 스타일 지정](#universal-styling))에서 이러한 세 가지 문제를 바로잡겠습니다.

## <a name="semanticzoom-and-gridview-design-changes"></a>SemanticZoom 및 GridView 디자인 변경

Windows 10의 디자인 변경 합니다 [ **SemanticZoom** ](https://msdn.microsoft.com/library/windows/apps/hh702601) 컨트롤의 섹션에 설명 된 [SemanticZoom 변경](w8x-to-uwp-porting-xaml-and-ui.md)합니다. 이러한 변경과 관련하여 이 섹션에서는 수행할 작업이 없습니다.

[  **GridView**](https://msdn.microsoft.com/library/windows/apps/br242705)에 대한 변경은 [GridView/ListView 변경](w8x-to-uwp-porting-xaml-and-ui.md) 섹션에서 설명합니다. 아래에 설명된 대로 해당 변경 내용에 맞춰 일부 사항을 매우 약간 조정했습니다.

-   SeZoUC.xaml의 `ZoomedInItemsPanelTemplate`에서 `Orientation="Horizontal"` 및 `GroupPadding="0,0,0,20"`을 설정합니다.
-   SeZoUC.xaml에서 `ZoomedOutItemsPanelTemplate`을 삭제하고 축소 보기에서 `ItemsPanel` 특성을 제거합니다.

정말 간단하죠!

## <a name="adaptive-ui"></a>적응 UI

해당 변경 후 SeZoUC.xaml에서 제공하는 UI 레이아웃은 앱이 넓은 창에서 실행되는 경우에 유용합니다(넓은 창은 큰 화면이 있는 장치에서만 가능). 그러나 앱의 창이 좁은 경우(좁은 창은 작은 장치에서 사용 가능하며 큰 장치에서도 사용 가능) Windows Phone 스토어 앱에서 사용한 UI가 거의 틀림없이 가장 적합합니다.

이를 위해 적응 Visual State Manager 기능을 사용할 수 있습니다. 기본적으로 Windows Phone 스토어 앱에서 사용하는 더 작은 템플릿을 사용하여 좁은 상태에 UI가 배치되도록 시각적 요소에서 속성을 설정합니다. 그런 다음 앱의 창이 특정 크기([유효 픽셀](w8x-to-uwp-porting-xaml-and-ui.md)의 단위로 측정)보다 넓거나 같은 경우를 감지하고 그에 따라 더 크고 넓은 레이아웃을 가져오도록 시각적 요소의 속성을 변경합니다. 시각적 상태에서 해당 속성 변경을 적용하고 적응 트리거를 사용하여 끊임없이 모니터링하고 유효 픽셀의 창 너비에 따라 해당 시각적 상태를 적용할지 여부를 결정합니다. 이 경우 창 너비에서 트리거하지만 창 높이에서도 트리거할 수 있습니다.

548epx의 최소 창 너비가 넓은 레이아웃을 표시하려는 가장 작은 장치의 크기이므로 548epx가 이 사용 사례에 적합합니다. 휴대폰은 일반적으로 548epx보다 작으므로 이와 같은 소형 장치에서는 기본적으로 좁은 레이아웃 상태를 유지합니다. PC에서는 기본적으로 넓은 상태로의 전환을 트리거하기에 충분히 넓은 상태로 창이 시작됩니다. 이러한 넓은 창에서, 창을 끌어 250x250 크기 항목의 두 열을 표시할 정도로 좁힐 수 있습니다. 그보다 너비를 더 좁히면 트리거가 비활성화되고 넓은 시각적 상태가 제거되며 기본 좁은 레이아웃이 적용됩니다.

따라서 이러한 두 가지 다른 레이아웃을 적용하려면 어떤 속성을 설정 및 변경해야 하나요? 두 가지 대안이 있으며 각 대안마다 다른 접근 방법을 사용합니다.

1.  태그에 두 가지 [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) 컨트롤을 사용할 수 있습니다. 하나는 Windows 런타임 8.x 앱에서 사용 하는 태그의 복사본이 됩니다 (사용 하 여 [ **GridView** ](https://msdn.microsoft.com/library/windows/apps/br242705) 내부 컨트롤)를 기본적으로 축소 합니다. 다른 하나는 Windows Phone 스토어 앱(이 내부에서 [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) 컨트롤 사용)에서 사용하는 태그의 복사본으로, 기본적으로 표시됩니다. 시각적 상태는 두 **SemanticZoom** 컨트롤의 표시 속성을 전환합니다. 이러한 작업을 수행하는 데는 약간의 노력이 필요할 수 있지만, 일반적으로 고성능 기술이 필요하지는 않습니다. 따라서 이 방법을 사용하는 경우 앱을 프로파일링하고 여전히 성능 목표를 충족하는지 확인해야 합니다.
2.  [  **ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) 컨트롤이 포함된 단일 [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601)을 사용할 수 있습니다. 두 가지 레이아웃을 적용하려면 넓은 시각적 상태에서, 적용된 템플릿을 비롯해 **ListView** 컨트롤의 속성을 변경하여 [**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705)에서 수행한 것과 동일한 방식으로 배치되도록 합니다. 그러면 성능은 더 좋지만, **GridView** 및 **ListView**의 다양한 스타일 및 템플릿 간에 그리고 다양한 항목 종류 간에 작은 차이가 너무 많아서 적용하기에는 더욱 어려운 솔루션입니다. 또한 이 솔루션은 현재 기본 스타일 및 템플릿이 디자인되는 방식과 긴밀하게 결합되어 있으며, 향후 기본값의 변화에 취약하고 민감한 솔루션을 제공합니다.

이 사례 연구에서는 첫 번째 대안을 사용하겠습니다. 그러나 두 번째 대안을 적용하고 싶은 경우 자신에게 그 방법이 더 맞는지 알아보세요. 다음은 첫 번째 대안을 구현하기 위해 수행할 단계입니다.

-   새 프로젝트 태그의 [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601)에서 `x:Name="wideSeZo"` 및 `Visibility="Collapsed"`를 설정합니다.
-   Bookstore2 돌아가서\_81. WindowsPhone SeZoUC.xaml 연 프로젝트입니다. 해당 파일에서 [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) 요소 태그를 복사하여 새 프로젝트의 `wideSeZo` 바로 뒤에 붙여넣습니다. 방금 붙여넣은 요소에서 `x:Name="narrowSeZo"`를 설정합니다.
-   그러나 `narrowSeZo`에는 아직 복사하지 않은 몇 가지의 스타일이 필요합니다. Bookstore2에서 다시\_81.WindowsPhone, 두 복사본 스타일 (`AuthorGroupHeaderContainerStyle` 고 `ZoomedOutAuthorItemContainerStyle`) SeZoUC.xaml의 out 및 BookstoreStyles.xaml에 새 프로젝트에 붙여 넣습니다.
-   이제 새 SeZoUC.xaml에 두 [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) 요소가 있습니다. **Grid**에서 이러한 두 요소를 래핑합니다.
-   새 프로젝트의 BookstoreStyles.xaml에서 이러한 세 리소스 키(그리고 `wideSeZo` 내부의 SeZoUC.xaml의 해당 참조에)인 `AuthorGroupHeaderTemplate`, `ZoomedOutAuthorTemplate`, `BookTemplate`에 `Wide` 단어를 추가합니다.
-   Bookstore2에서\_81. WindowsPhone 프로젝트 BookstoreStyles.xaml를 엽니다. 이 파일에서 (위에서 언급 한) 같은 세 가지 리소스를 복사 하 고 두 점프 목록 항목 변환기, 및 Windows의 네임 스페이스 접두사 선언만\_UI\_Xaml\_컨트롤\_기본 모든 붙여넣을 새 프로젝트에서 BookstoreStyles.xaml에.
-   마지막으로 새 프로젝트의 SeZoUC.xaml에서 적절한 Visual State Manager 태그를 방금 추가한 **Grid**에 추가합니다.

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

## <a name="universal-styling"></a>범용 스타일 지정

이제 이전 프로젝트에서 복사하는 동안 위에서 소개했던 문제를 비롯하여 일부 스타일 지정 문제를 수정하겠습니다.

-   MainPage.xaml에서 `LayoutRoot`의 배경을 `"{ThemeResource ApplicationPageBackgroundThemeBrush}"`로 변경합니다.
-   BookstoreStyles.xaml에서 리소스 `TitlePanelMargin`의 값을 `0`(또는 적절해 보이는 값)으로 설정합니다.
-   SeZoUC.xaml에서 `wideSeZo`의 여백을 `0`(또는 적절해 보이는 값)으로 설정합니다.
-   BookstoreStyles.xaml의 `AuthorGroupHeaderTemplateWide`에서 Margin 특성을 제거합니다.
-   `AuthorGroupHeaderTemplate` 및 `ZoomedOutAuthorTemplate`에서 FontFamily 특성을 제거합니다.
-   Bookstore2\_81을 사용 합니다 `BookTemplateTitleTextBlockStyle`를 `BookTemplateAuthorTextBlockStyle`, 및 `PageTitleTextBlockStyle` 리소스가 간접 참조로 키 단일 키에 게 두 앱의 다른 구현을 있도록 합니다. 해당 간접 참조는 더 이상 필요하지 않습니다. 시스템 스타일은 직접 참조할 수만 있습니다. 따라서 앱 전체에서 해당 참조를 각각 `TitleTextBlockStyle`, `CaptionTextBlockStyle` 및 `HeaderTextBlockStyle`로 바꿉니다. Visual Studio **파일에서 바꾸기** 기능을 사용하면 이 작업을 빠르고 정확하게 수행할 수 있습니다. 그런 다음 사용하지 않은 세 가지 해당 리소스를 삭제할 수 있습니다.
-   `AuthorGroupHeaderTemplate`에서 `PhoneAccentBrush`를 `SystemControlBackgroundAccentBrush`로 바꾸고, 모바일 디바이스 패밀리에서 실행될 때 올바르게 표시되도록 **TextBlock**에서 `Foreground="White"`를 설정합니다.
-   `BookTemplateWide`에서 두 번째 **TextBlock**의 Foreground 특성을 첫 번째 텍스트 블록에 복사합니다.
-   `ZoomedOutAuthorTemplateWide`에서 `SubheaderTextBlockStyle`(현재 너무 큼)에 대한 참조를 `SubtitleTextBlockStyle`에 대한 참조로 변경합니다.
-   축소 보기(점프 목록)가 새 플랫폼의 확대 보기를 더 이상 오버레이하지 않습니다. 따라서 `narrowSeZo`의 축소 보기에서 `Background` 특성을 제거할 수 있습니다.
-   모든 스타일과 템플릿이 하나의 파일에 있도록 `ZoomedInItemsPanelTemplate`을 SeZoUC.xaml에서 BookstoreStyles.xaml로 이동합니다.

스타일 작업의 마지막 시퀀스를 진행하면 앱이 다음과 같은 모양을 유지합니다.

![데스크톱 디바이스에서 실행되는 포팅된 Windows 10 앱, 확대 보기, 두 개의 창 크기](images/w8x-to-uwp-case-studies/c02-07-desk10-zi-ported.png)

창의 두 크기 확대 된 뷰를 선택한 다음 데스크톱 장치에서 실행 되는 가져오는 Windows 10 앱

![데스크톱 디바이스에서 실행되는 포팅된 Windows 10 앱, 축소 보기, 두 개의 창 크기](images/w8x-to-uwp-case-studies/c02-08-desk10-zo-ported.png)

창의 두 크기 축소 된 보기를 선택한 다음 데스크톱 장치에서 실행 되는 가져오는 Windows 10 앱

![모바일 디바이스에서 실행되는 포팅된 Windows 10 앱, 확대 보기](images/w8x-to-uwp-case-studies/c02-09-mob10-zi-ported.png)

확대 된 뷰를 모바일 장치에서 실행 되는 가져온된 Windows 10 앱

![모바일 디바이스에서 실행되는 포팅된 Windows 10 앱, 축소 보기](images/w8x-to-uwp-case-studies/c02-10-mob10-zo-ported.png)

축소 된 보기 모바일 장치에서 실행 중인 Windows 10 앱 이식된

## <a name="conclusion"></a>결론

이 사례 연구는 이전보다 더욱 복잡한 사용자 인터페이스를 포함합니다. 이전 사례 연구와 마찬가지로 이 특정 보기 모델에는 작업이 필요하지 않으며 주로 사용자 인터페이스를 리팩터링하는 데 노력을 기울였습니다. 일부 변경 내용은 계속해서 여러 폼 팩터(사실상 이전보다 훨씬 많음)를 지원하면서 두 프로젝트를 하나로 결합하는 데 따른 필연적인 결과였습니다. 몇 가지 변경은 플랫폼에 적용한 변경과 관계가 있었습니다.

다음 사례 연구는 [QuizGame](w8x-to-uwp-case-study-quizgame.md)이며, 여기에서는 그룹화된 데이터에 대한 액세스 및 표시에 대해 살펴봅니다.
