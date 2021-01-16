---
ms.assetid: 2b63a4c8-b1c0-4c77-95ab-0b9549ba3c0e
description: 이 항목에서는 매우 간단한 Windows Phone Silverlight 앱을 UWP (Windows 10 유니버설 Windows 플랫폼) 앱으로 이식 하는 사례 연구를 제공 합니다.
title: Windows Phone Silverlight에서 UWP 사례 연구, Bookstore1
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8c771de3f5ad0d042d278c0851849c306e7edb4d
ms.sourcegitcommit: b0a82c2a132212eb5fb72b67f0789cac1014642f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254238"
---
# <a name="windows-phone-silverlight-to-uwp-case-study-bookstore1"></a>Windows Phone Silverlight에서 UWP 사례 연구: Bookstore1


이 항목에서는 매우 간단한 Windows Phone Silverlight 앱을 UWP (Windows 10 유니버설 Windows 플랫폼) 앱으로 이식 하는 사례 연구를 제공 합니다. Windows 10에서는 고객이 다양 한 장치에 설치할 수 있는 단일 앱 패키지를 만들 수 있으며,이 사례 연구에서이 작업을 수행할 수 있습니다. [UWP 앱에 대 한 가이드를](../get-started/universal-application-platform-guide.md)참조 하세요.

앱은 뷰 모델에 바인딩된 **ListBox** 로 구성 됩니다. 보기 모델에는 제목, 저자 및 책 커버를 보여 주는 책 목록이 있습니다. 서적 커버 이미지는 **빌드 작업** 을 **콘텐츠** 로 설정 하 고 **출력 디렉터리로 복사** 를 **복사 안 함** 으로 설정 합니다.

이 단원의 이전 항목에서는 플랫폼 간의 차이점에 대해 설명 하 고, 뷰 모델에 대 한 바인딩을 통해 데이터에 액세스 하는 것을 통해 XAML 태그에서 앱의 다양 한 측면에 대 한 이식 프로세스에 대 한 세부 정보 및 지침을 제공 합니다. 사례 연구는 실제 예제에서 작업을 수행 하 여 해당 지침을 보완 하는 것을 목표로 합니다. 사례 연구에서는 반복 되지 않는 지침을 읽은 것으로 가정 합니다.

**참고**   \_Visual studio에서 Bookstore1Universal 10을 열 때 "Visual studio 업데이트 필요" 메시지가 표시 되 면 [Targetplatformversion](wpsl-to-uwp-troubleshooting.md)에서 대상 플랫폼 버전 관리를 선택 하는 단계를 수행 합니다.

## <a name="downloads"></a>다운로드

[Bookstore1WPSL8 Windows Phone Silverlight 앱을 다운로드](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore1WPSL8)합니다.

[Bookstore1Universal \_ 10 Windows 10 앱을 다운로드](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore1Universal_10)합니다.

## <a name="the-windows-phone-silverlight-app"></a>Windows Phone Silverlight 앱

Bookstore1WPSL8 하는 앱은 다음과 같이 표시 됩니다. 앱의 이름 및 페이지 제목 아래에 있는 책의 세로 스크롤 목록 상자 일 뿐입니다.

![bookstore1wpsl8 모양](images/wpsl-to-uwp-case-studies/c01-01-wpsl-how-the-app-looks.png)

## <a name="porting-to-a-windows-10-project"></a>Windows 10 프로젝트로 포팅

Visual Studio에서 새 프로젝트를 만들고, Bookstore1WPSL8에서 파일에 파일을 복사 하 고, 복사한 파일을 새 프로젝트에 포함 하는 것이 매우 빠른 작업입니다. 새 빈 응용 프로그램 (Windows 유니버설) 프로젝트를 만들어 시작 합니다. 이름을 Bookstore1Universal \_ 10으로 합니다. Bookstore1WPSL8에서 Bookstore1Universal 10으로 복사할 파일 \_ 입니다.

-   책 표지 이미지를 포함 하는 폴더를 복사 합니다. PNG 파일 \\ ( \\ CoverImages 폴더)입니다. 폴더를 복사한 후 **솔루션 탐색기** 에서 **모든 파일 표시** 가 설정 되어 있는지 확인 합니다. 복사한 폴더를 마우스 오른쪽 단추로 클릭 하 고 **프로젝트에 포함** 을 클릭 합니다. 이 명령은 프로젝트의 파일 또는 폴더를 "포함" 하 여 의미 합니다. 파일이 나 폴더를 복사할 때마다 **솔루션 탐색기** 에서 **새로 고침** 을 클릭 한 다음 프로젝트에 파일 또는 폴더를 포함 합니다. 대상에서 대체 하는 파일에 대해서는이 작업을 수행할 필요가 없습니다.
-   뷰 모델 원본 파일이 포함 된 폴더를 복사 \\ 합니다 (ViewModel).
-   MainPage을 복사 하 고 대상의 파일을 바꿉니다.

Windows 10 프로젝트에서 Visual Studio가 생성 한 App.xaml.cs 및 Visual Studio를 유지할 수 있습니다.

방금 복사한 소스 코드 및 태그 파일을 편집 하 고 Bookstore1WPSL8 네임 스페이스에 대 한 참조를 Bookstore1Universal 10으로 변경 \_ 합니다. 이 작업을 수행 하는 빠른 방법은 **파일에서 바꾸기** 기능을 사용 하는 것입니다. 뷰 모델 소스 파일의 명령적 코드에서 다음과 같은 포팅 변경이 필요 합니다.

-   `System.ComponentModel.DesignerProperties`을로 변경 하 `DesignMode` 고이에 대해 **Resolve** 명령을 사용 합니다. 속성을 삭제 하 `IsInDesignTool` 고 IntelliSense를 사용 하 여 올바른 속성 이름를 추가 `DesignModeEnabled` 합니다.
-   에서 **Resolve** 명령을 사용 `ImageSource` 합니다.
-   에서 **Resolve** 명령을 사용 `BitmapImage` 합니다.
-   및를 사용 하 여 삭제 `System.Windows.Media;` `using System.Windows.Media.Imaging;` 합니다.
-   **Bookstore1Universal \_** 속성에서 반환 하는 값을 "BOOKSTORE1WPSL8"에서 "Bookstore1Universal"로 변경 합니다.

MainPage에서 다음과 같은 이식 변경이 필요 합니다.

-   `phone:PhoneApplicationPage`를로 변경 `Page` 합니다 (속성 요소 구문에서 발생 하는 것을 잊지 마세요).
-   `phone`및 `shell` 네임 스페이스 접두사 선언을 삭제 합니다.
-   나머지 네임 스페이스 접두사 선언에서 "clr-namespace"를 "using"으로 변경 합니다.

태그가 일시적으로 제거 되는 것을 의미 하는 경우에도 결과 제거를 확인 하려는 경우 태그 컴파일 오류를 수정 하도록 선택할 수 있습니다. 그러나 이렇게 하면 계산 된 부채에 대 한 기록을 유지 하겠습니다. 이 경우에는 여기에 있습니다.

1.  **Mainpage** 의 root **page** 요소에서를 삭제 `SupportedOrientations="Portrait"` 합니다.
2.  **Mainpage** 의 root **page** 요소에서를 삭제 `Orientation="Portrait"` 합니다.
3.  **Mainpage** 의 root **page** 요소에서를 삭제 `shell:SystemTray.IsVisible="True"` 합니다.
4.  `BookTemplate`데이터 템플릿에서 및 TextBlock 스타일에 대 한 참조를 `PhoneTextExtraLargeStyle` 삭제 `PhoneTextSubtleStyle`  **** 합니다.
5.  `TitlePanel`  **StackPanel** 에서 및 TextBlock 스타일에 대 한 참조를 삭제 `PhoneTextNormalStyle` `PhoneTextTitle1Style`  **** 합니다.

먼저 모바일 장치 제품군에 대 한 UI를 살펴보겠습니다. 그 후에는 다른 폼 팩터를 고려할 수 있습니다. 이제 앱을 빌드하고 실행할 수 있습니다. 모바일 에뮬레이터에서 표시 되는 방법은 다음과 같습니다.

![초기 소스 코드가 변경 된 모바일의 uwp 앱](images/wpsl-to-uwp-case-studies/c01-02-mob10-initial-source-code-changes.png)

뷰 및 뷰 모델이 제대로 작동 하 고 **ListBox** 가 작동 합니다. 대부분 스타일을 수정 하 고 이미지를 표시 해야 합니다.

## <a name="paying-off-the-debt-items-and-some-initial-styling"></a>부채 항목의 지불 및 몇 가지 초기 스타일 지정

기본적으로 모든 방향이 지원 됩니다. Windows Phone Silverlight 앱은 명시적으로 세로 전용으로 제한 되기 때문에, \# \# 새 프로젝트의 앱 패키지 매니페스트로 이동 하 여 **지원 되는 방향** 에서 **세로로** 확인 하 여 부채 항목 1과 2를 지불 합니다.

이 앱의 경우 \# 상태 표시줄 (이전에는 시스템 트레이 라고 함)이 기본적으로 표시 되므로 항목 3은 부채가 아닙니다. 항목 \# 4와 5의 경우 \# 사용 중인 Windows Phone Silverlight 스타일에 해당 하는 UWP (4 유니버설 Windows 플랫폼) **TextBlock** 스타일을 찾아야 합니다. 에뮬레이터에서 Windows Phone Silverlight 앱을 실행 하 고 [텍스트](wpsl-to-uwp-porting-xaml-and-ui.md) 섹션의 그림과 나란히 비교할 수 있습니다. 이 작업을 수행 하 고 Silverlight 시스템 스타일 Windows Phone의 속성을 살펴보면이 테이블을 만들 수 있습니다.

| Windows Phone Silverlight 스타일 키 | UWP 스타일 키          |
|-------------------------------------|------------------------|
| PhoneTextExtraLargeStyle            | TitleTextBlockStyle    |
| PhoneTextSubtleStyle                | SubtitleTextBlockStyle |
| PhoneTextNormalStyle                | CaptionTextBlockStyle  |
| PhoneTextTitle1Style                | HeaderTextBlockStyle   |

이러한 스타일을 설정 하려면 태그 편집기에 입력 하거나 Visual Studio XAML 도구를 사용 하 여 입력 하지 않고 설정할 수 있습니다. 이렇게 하려면 **TextBlock** 을 마우스 오른쪽 단추로 클릭 하 고 **스타일 편집** &gt; **리소스 적용** 을 클릭 합니다. 항목 템플릿에서 **TextBlock** s를 사용 하 여이 작업을 수행 하려면 **목록 상자** 를 마우스 오른쪽 단추로 클릭 하 고 **추가 템플릿 편집** &gt; **생성 된 항목 (ItemTemplate)** 을 클릭 합니다.

**ListBox** 컨트롤의 기본 스타일은 해당 배경을 시스템 리소스로 설정 하므로 항목 뒤에 80% 불투명 흰색 배경이 있습니다 `ListBoxBackgroundThemeBrush` . `Background="Transparent"`해당 배경을 지우려면 **ListBox** 에 설정 합니다. 항목 템플릿에서 **TextBlock** s를 왼쪽으로 정렬 하려면 위에서 설명한 것과 같은 방식으로 다시 편집 하 고 두 번째 Textblock에 **여백을** 설정 `"9.6,0"` 합니다. 

이 작업을 수행한 후에는 [보기 픽셀과 관련 된 변경 내용](wpsl-to-uwp-porting-xaml-and-ui.md)때문에 아직 변경 하지 않은 고정 크기 차원 (여백, 너비, 높이 등)을 0.8에 곱해 야 합니다. 예를 들어 이미지는 70x70px에서 56x56px로 변경 되어야 합니다.

하지만 스타일의 결과를 표시 하기 전에 렌더링할 이미지를 살펴보겠습니다.

## <a name="binding-an-image-to-a-view-model"></a>이미지를 뷰 모델에 바인딩

Bookstore1WPSL8에서는 다음을 수행 했습니다.

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(this.CoverImagePath, UriKind.Relative));
```

Bookstore1Universal에서는 ms appx [URI 체계](/previous-versions/windows/apps/jj655406(v=win.10))를 사용 합니다. 코드의 나머지 부분을 동일 하 게 유지할 수 있도록, **system.uri** 생성자의 다른 오버 로드를 사용 하 여 기본 uri에 MS appx uri 체계를 추가 하 고 나머지 경로를 해당 경로에 추가할 수 있습니다. 다음과 같이:

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(new Uri("ms-appx://"), this.CoverImagePath));
```

## <a name="universal-styling"></a>유니버설 스타일 지정

이제 몇 가지 최종 스타일 지정을 수행 하 고 모바일 뿐만 아니라 데스크톱 (및 기타) 폼 팩터에서 앱이 잘 표시 되는지 확인 해야 합니다. 단계는 다음과 같습니다. 이 항목의 맨 위에 있는 링크를 사용 하 여 프로젝트를 다운로드 하 고 여기와 사례 연구의 끝 사이에 있는 모든 변경 내용에 대 한 결과를 볼 수 있습니다.

-   항목 사이의 간격을 강화 하려면 `BookTemplate` mainpage에서 데이터 템플릿을 찾고 `Margin` 루트 **표에서** 특성을 삭제 합니다.
-   페이지 제목에 좀 더 보냅니다 공간을 제공 하려면 `-5.6` `0` 페이지 제목 **TextBlock** 에서의 아래쪽 여백을로 다시 설정할 수 있습니다.
-   이제 `LayoutRoot` 테마의 내용에 관계 없이 모든 장치에서 실행 될 때 앱이 적절 하 게 보이도록 올바른 기본값으로를 설정 해야 합니다. 에서로 변경 `"Transparent"` `"{ThemeResource ApplicationPageBackgroundThemeBrush}"` 합니다.

더 복잡 한 앱을 사용 하면 [폼 팩터 및 사용자 경험을 위해 포팅](wpsl-to-uwp-form-factors-and-ux.md) 하는 지침을 사용 하 고 이제 앱이 실행 될 수 있는 여러 장치의 폼 팩터를 최적으로 사용 하 게 됩니다. 그러나이 간단한 응용 프로그램의 경우 여기에서 중지 하 고 스타일 지정 작업의 마지막 시퀀스 이후에 앱이 어떻게 표시 되는지 확인할 수 있습니다. 이는 와이드 폼 팩터를 최대한 활용 하는 것이 아니라 모바일 및 데스크톱 장치에서 동일 하 게 보입니다. 하지만 이후 사례 연구에서이 작업을 수행 하는 방법을 조사 합니다.

앱의 테마를 제어 하는 방법을 보려면 [테마 변경](wpsl-to-uwp-porting-xaml-and-ui.md) 을 참조 하세요.

![포팅 된 windows 10 앱](images/w8x-to-uwp-case-studies/c01-07-mob10-ported.png)

모바일 장치에서 실행 되는 이식 된 Windows 10 앱

## <a name="an-optional-adjustment-to-the-list-box-for-mobile-devices"></a>모바일 장치의 목록 상자에 대 한 선택적 조정

앱이 모바일 장치에서 실행 되는 경우 목록 상자의 배경은 기본적으로 두 테마에 모두 표시 됩니다. 원하는 스타일 일 수 있으며,이 경우 더 이상 수행할 작업이 없습니다. 그러나 컨트롤은 동작의 영향을 받지 않고 모양을 사용자 지정할 수 있도록 디자인 되었습니다. 따라서 원본 앱이 표시 되는 방식으로 목록 상자를 어두운 테마에 어둡게 표시 하려면 "선택적 조정"에서 다음 [지침](w8x-to-uwp-case-study-bookstore1.md) 을 따르세요.

## <a name="conclusion"></a>결론

이 사례 연구는 매우 간단한 앱을 이식 하는 프로세스를 보여 주었습니다. 통해 비현실적으로 단순 합니다. 예를 들어 목록 컨트롤을 선택 하거나 탐색을 위한 컨텍스트를 설정 하는 데 사용할 수 있습니다. 앱은 탭 된 항목에 대 한 자세한 정보를 포함 하는 페이지로 이동 합니다. 이 특정 앱은 사용자가 선택 하는 것이 없으며 탐색이 없습니다. 그러나 사례 연구는 ice를 중단 하 고 포팅 프로세스를 소개 하며 실제 UWP 앱에서 사용할 수 있는 중요 한 기술을 보여 주기 위해 제공 됩니다.

다음 사례 연구는 그룹화 된 데이터에 액세스 하 고 표시 하는 것을 [Bookstore2](wpsl-to-uwp-case-study-bookstore2.md).