---
author: stevewhims
ms.assetid: 2b63a4c8-b1c0-4c77-95ab-0b9549ba3c0e
description: 이 항목에서는 매우 간단한 WindowsPhone Silverlight 앱을 Windows10 유니버설 Windows 플랫폼 (UWP) 앱으로 포팅하는 사례 연구를 제공 합니다.
title: WindowsPhone Silverlight에서 UWP 사례 연구, Bookstore1
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c335f607eb1897f79035850cd6a5af9e7a7a56dc
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7156699"
---
# <a name="windowsphone-silverlight-to-uwp-case-study-bookstore1"></a>WindowsPhone Silverlight를 UWP로 이동 사례 연구: Bookstore1


이 항목에서는 매우 간단한 WindowsPhone Silverlight 앱은 Windows10Universal Windows 플랫폼 (UWP) 앱을 포팅하는 사례 연구를 제공 합니다. Windows10를 만들 수 있습니다 단일 앱 패키지는이 사례 연구에서 작업을 어떻게 수행 되 고 고객이 다양 한 장치에 설치할 수 있습니다. [UWP 앱 지침](https://msdn.microsoft.com/library/windows/apps/dn894631)을 참조하세요.

포팅할 앱은 보기 모델에 바인딩되는 **ListBox**로 구성됩니다. 보기 모델에는 제목, 저자 및 책 표지를 보여주는 책 목록이 있습니다. 책 표지 이미지에는 **빌드 작업**이 **콘텐츠**로 설정되어 있고 **출력 디렉터리로 복사**가 **복사 안 함**으로 설정되어 있습니다.

이 섹션의 이전 항목에서는 플랫폼 간의 차이점에 대해 설명하고 XAML 태그, 보기 모델에 바인딩, 데이터 액세스 등 앱의 다양한 측면에 대한 포팅 프로세스와 관련된 세부 정보와 지침을 제공했습니다. 사용 사례는 실제 작업 사례를 제공하여 지침을 보완하는 데 목표를 두고 있습니다. 사례 연구에서는 지침을 이미 읽었다고 가정하고 더 이상 반복하지 않습니다.

**참고**  [TargetPlatformVersion](wpsl-to-uwp-troubleshooting.md)에서 대상 플랫폼 버전 선택 단계를 따르세요 다음 때 "Visual Studio 업데이트 필요" 메시지가 표시 되 면 Bookstore1Universal\_10 Visual Studio에서 열기.

## <a name="downloads"></a>다운로드

[WindowsPhone Silverlight 앱을 다운로드 Bookstore1WPSL8 합니다](http://go.microsoft.com/fwlink/?linkid=517053).

[Bookstore1Universal\_10 Windows10 앱을 다운로드](http://go.microsoft.com/fwlink/?linkid=532950)합니다.

## <a name="the-windowsphone-silverlight-app"></a>WindowsPhone Silverlight 앱

포팅할 Bookstore1WPSL8 앱은 다음과 같습니다. 앱의 이름과 페이지 제목 아래에는 세로로 스크롤되는 책의 목록 상자가 있습니다.

![bookstore1wpsl8 모양](images/wpsl-to-uwp-case-studies/c01-01-wpsl-how-the-app-looks.png)

## <a name="porting-to-a-windows10-project"></a>Windows10 프로젝트로 포팅

Visual Studio에서 새 프로젝트를 만들고 Bookstore1WPSL8의 파일을 이 프로젝트로 복사한 후 복사한 파일을 새 프로젝트에 포함하는 과정은 매우 빠르게 진행되는 작업입니다. 비어 있는 응용 프로그램(Windows 유니버설) 프로젝트를 새로 만들어 시작합니다. Bookstore1Universal\_10이라고 이름을 지정합니다. 다음은 Bookstore1WPSL8에서 Bookstore1Universal\_10으로 복사할 파일입니다.

-   책 표지 이미지 PNG 파일이 들어 있는 폴더를 복사합니다(폴더는 \\Assets\\CoverImages임) 폴더를 복사한 후에 **솔루션 탐색기**에서 **모든 파일 표시**가 설정되어 있는지 확인합니다. 복사한 폴더를 마우스 오른쪽 단추로 클릭하고 **프로젝트에 포함**을 클릭합니다. 이 명령은 파일이나 폴더를 프로젝트에 "포함"하여 우리가 의도한 작업을 진행합니다. 파일이나 폴더를 복사할 때마다 **솔루션 탐색기**에서 **새로 고침**을 클릭한 다음 프로젝트에 파일 또는 폴더를 포함합니다. 대상에서 바꾸려는 파일에 대해서는 이 작업을 수행하지 않아도 됩니다.
-   보기 모델 소스 파일이 포함된 폴더(\\ViewModel)를 복사합니다.
-   MainPage.xaml을 복사한 후 대상의 파일을 바꿉니다.

App.xaml 및 App.xaml.cs Windows10 프로젝트에서 Visual Studio에서 생성 우리는 하 게 유지할 수 있습니다.

방금 복사한 소스 코드 및 태그 파일을 편집하고 Bookstore1WPSL8 네임스페이스에 대한 참조를 Bookstore1Universal\_10으로 변경합니다. **파일에서 바꾸기** 기능을 사용하면 이 작업을 빠르게 수행할 수 있습니다. 보기 모델 소스 파일의 명령적 코드에서 다음과 같은 포팅 변경이 필요합니다.

-   `System.ComponentModel.DesignerProperties`을(를) `DesignMode`(으)로 변경한 다음 **확인** 명령을 사용합니다. `IsInDesignTool` 속성을 삭제하고 IntelliSense를 사용하여 올바른 속성 이름(`DesignModeEnabled`)을 추가합니다.
-   `ImageSource`에서 **확인** 명령을 사용합니다.
-   `BitmapImage`에서 **확인** 명령을 사용합니다.
-   `System.Windows.Media;` 및 `using System.Windows.Media.Imaging;`을 사용하여 삭제합니다.
-   **Bookstore1Universal\_10.BookstoreViewModel.AppName** 속성에 의해 반환된 값을 "BOOKSTORE1WPSL8"에서 "BOOKSTORE1UNIVERSAL"로 변경합니다.

MainPage.xaml에서 다음과 같은 변경 내용이 필요합니다.

-   `phone:PhoneApplicationPage`에서 `Page`(으)로 변경합니다(속성 요소 구문에서의 발생 횟수를 잊지 말 것).
-   `phone` 및 `shell` 네임스페이스 접두사 선언을 삭제합니다.
-   나머지 네임스페이스 접두사 선언에서 ‘clr 네임스페이스’를 ‘사용’으로 변경합니다.

태그를 일시적으로 제거하더라도 결과를 최대한 빠르게 표시하려면 태그 컴파일 오류를 매우 저렴하게 수정할 수 있습니다. 하지만 이렇게 하여 증가하는 부채를 기록해 보겠습니다. 이 경우는 다음과 같습니다.

1.  **MainPage.xaml**의 루트 **Page** 요소에서 `SupportedOrientations="Portrait"`를 삭제합니다.
2.  **MainPage.xaml**의 루트 **Page** 요소에서 `Orientation="Portrait"`를 삭제합니다.
3.  **MainPage.xaml**의 루트 **Page** 요소에서 `shell:SystemTray.IsVisible="True"`를 삭제합니다.
4.  `BookTemplate` 데이터 템플릿에서 `PhoneTextExtraLargeStyle` 및 `PhoneTextSubtleStyle` **TextBlock** 스타일에 대한 참조를 삭제합니다.
5.  `TitlePanel` **StackPanel**에서 `PhoneTextNormalStyle` 및 `PhoneTextTitle1Style` **TextBlock** 스타일에 대한 참조를 삭제합니다.

먼저 모바일 디바이스 패밀리의 UI에 대해 작업하겠습니다. 그 후에야 다른 폼 팩터를 고려할 수 있습니다. 이제 앱을 빌드 및 실행할 수 있습니다. 모바일 에뮬레이터에서 모양은 다음과 같습니다.

![초기 소스 코드가 변경된 모바일의 UWP 앱](images/wpsl-to-uwp-case-studies/c01-02-mob10-initial-source-code-changes.png)

보기와 보기 모델이 함께 올바르게 작동하고 **ListBox**가 작동하고 있습니다. 대부분 스타일을 수정하고 표시할 이미지를 가져와야 합니다.

## <a name="paying-off-the-debt-items-and-some-initial-styling"></a>부채 항목 청산 및 일부 초기 스타일 지정

기본적으로 모든 방향이 지원됩니다. 명시적으로 제한 합니다 WindowsPhone Silverlight 앱 세로 전용 하지만, 따라서 부채 항목 \#1 및 \#2 지급 되 새 프로젝트에서 앱 패키지 매니페스트로 하 고 **지원 되는 방향**에서 **세로** 확인 하 여 합니다.

이 앱의 경우 상태 표시줄(이전의 시스템 트레이)이 기본적으로 표시되므로 항목 #\3은 부채가 아닙니다. 항목 \#4 및 \#5를 사용 하는 WindowsPhone Silverlight 스타일에 해당 하는 네 가지 유니버설 Windows 플랫폼 (UWP) **TextBlock** 스타일을 찾는 필요 합니다. 에뮬레이터에서 WindowsPhone Silverlight 앱을 실행 하 고 정렬 [텍스트](wpsl-to-uwp-porting-xaml-and-ui.md) 섹션의 그림과 비교 수 있습니다. 및 WindowsPhone Silverlight 시스템 스타일의 속성을 참조이 테이블을 만들 수 있습니다.

| Windows Phone Silverlight 스타일 키 | UWP 스타일 키          |
|-------------------------------------|------------------------|
| PhoneTextExtraLargeStyle            | TitleTextBlockStyle    |
| PhoneTextSubtleStyle                | SubtitleTextBlockStyle |
| PhoneTextNormalStyle                | CaptionTextBlockStyle  |
| PhoneTextTitle1Style                | HeaderTextBlockStyle   |
 
이러한 스타일을 설정하려면 태그 편집기에 스타일을 입력하거나 Visual Studio XAML 도구를 사용하여 별도로 입력하지 않고 스타일을 설정할 수 있습니다. 이렇게 하려면 **TextBlock**을 마우스 오른쪽 단추로 클릭하고 **스타일 편집** &gt; **리소스 적용**을 클릭합니다. 항목 템플릿의 **TextBlock**을 사용하려면 **ListBox**를 마우스 오른쪽 단추로 클릭하고 **추가 템플릿 편집** &gt; **생성된 항목 편집(ItemTemplate)** 을 클릭합니다.

**ListBox** 컨트롤의 기본 스타일에서는 배경을 `ListBoxBackgroundThemeBrush` 시스템 리소스로 설정하기 때문에 항목의 뒤에는 80% 불투명 흰색 배경이 있습니다. **ListBox**에서 `Background="Transparent"`를 설정하여 해당 배경의 선택을 취소합니다. 항목 템플릿의 **TextBlock**을 왼쪽 맞춤으로 설정하려면 위에 설명한 것과 동일한 방법으로 편집하고 두 **TextBlock** 모두에 대해 **Margin**을 `"9.6,0"`으로 설정합니다.

완료되면 [보기 픽셀 관련 변경](wpsl-to-uwp-porting-xaml-and-ui.md)으로 인해 아직 변경하지 않은 고정 크기 치수(여백, 너비, 높이 등)를 0.8로 곱해야 합니다. 예를 들어 이미지가 70x70px에서 56x56px로 변경되어야 합니다.

그러나 스타일 지정의 결과를 보여 주기 전에 렌더링할 해당 이미지를 가져오겠습니다.

## <a name="binding-an-image-to-a-view-model"></a>이미지를 보기 모델에 바인딩

Bookstore1WPSL8에서 다음과 같이 했습니다.

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(this.CoverImagePath, UriKind.Relative));
```

Bookstore1Universal에서 ms-appx [URI 스키마](https://msdn.microsoft.com/library/windows/apps/jj655406)를 사용합니다. 코드의 나머지 부분을 동일하게 유지할 수 있도록 **System.Uri** 생성자의 다른 오버로드를 사용하여 ms-appx URI 스키마를 기본 URI에 삽입하고 나머지 경로를 URI에 추가합니다. 다음과 같습니다.

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(new Uri("ms-appx://"), this.CoverImagePath));
```

## <a name="universal-styling"></a>범용 스타일 지정

이제 일부 최종 스타일을 조정하고 앱이 모바일뿐만 아니라 데스크톱(및 기타) 폼 팩터에서도 멋지게 표시되는지 확인해야 합니다. 단계는 다음과 같습니다. 이 항목 맨 위에 있는 링크를 사용하여 프로젝트를 다운로드하고, 여기서부터 사례 연구 끝까지 포함된 모든 변경의 결과를 확인할 수 있습니다.

-   항목 사이의 간격을 촘촘히 하려면 MainPage.xaml에서 `BookTemplate` 데이터 템플릿을 찾고 루트 **Grid**에서 `Margin` 특성을 삭제합니다.
-   페이지 제목에 안정감을 조금 더 부여하려는 경우 페이지 제목 **TextBlock**에서 아래쪽 여백 `-5.6`을 `0`으로 다시 설정할 수 있습니다.
-   이제 테마에 관계없이 앱이 모든 디바이스에서 실행될 때 적절해 보이도록 `LayoutRoot`의 Background를 올바른 기본값으로 설정해야 합니다. `"Transparent"`에서 `"{ThemeResource ApplicationPageBackgroundThemeBrush}"`로 변경합니다.

더 정교한 앱에서는 [폼 팩터 및 사용자 환경에 대한 포팅](wpsl-to-uwp-form-factors-and-ux.md)의 지침에 따라 현재 앱이 실행될 수 있는 여러 디바이스 각각의 폼 팩터를 최대한 활용하세요. 그러나 간단한 이 앱의 경우 여기에서 중지하고 스타일 지정 작업의 마지막 해당 시퀀스 후 앱이 어떻게 표시되는지 확인할 수 있습니다. 모바일과 데스크톱 장치에서 실제로 똑같이 표시되지만 와이드 폼 팩터에서는 공간을 효과적으로 사용하지 못하는 것입니다. 이후 사례 연구에서 효과적으로 활용하는 방법을 살펴보겠습니다.

앱의 테마를 제어하는 방법을 알아보려면 [테마 변경](wpsl-to-uwp-porting-xaml-and-ui.md)을 참조하세요.

![포팅된 Windows 10 앱](images/w8x-to-uwp-case-studies/c01-07-mob10-ported.png)

모바일 장치에서 실행 되는 포팅된 Windows10 앱

## <a name="an-optional-adjustment-to-the-list-box-for-mobile-devices"></a>모바일 장치에 대한 목록 상자를 선택적으로 조정

앱이 모바일 장치에서 실행되는 경우 기본적으로 두 테마에서 목록 상자의 배경이 밝습니다. 이러한 설정이 선호하는 스타일일 수 있으며, 그런 경우 다른 작업을 하지 않아도 됩니다. 그러나 컨트롤은 해당 동작에 영향을 주지 않으면서 모양을 사용자 지정할 수 있도록 디자인되었습니다. 원래 앱의 모양대로 어두운 테마에서 목록 상자를 어둡게 유지하려면 "선택적 조정" 아래에서 [해당 지침](w8x-to-uwp-case-study-bookstore1.md)을 따르세요.

## <a name="conclusion"></a>결론

이 사례 연구에서는 비현실적으로 간단한 앱을 포팅하는 프로세스에 대해 살펴보았습니다. 예를 들어 목록 컨트롤을 사용하여 선택하거나 탐색할 컨텍스트를 설정할 수 있습니다. 앱에서는 탭되는 항목에 대한 세부 정보가 나오는 페이지를 탐색합니다. 이 특정 앱에서는 사용자 선택과 관련하여 아무 작업도 수행하지 않고 탐색도 하지 않습니다. 하지만 이 사례 연구에서는 분위기를 쇄신하고 포팅 프로세스를 소개하고 실제 UWP 앱에서 사용할 수 있는 중요한 기술을 보여 주었습니다.

다음 사례 연구는 [Bookstore2](wpsl-to-uwp-case-study-bookstore2.md)이며, 여기에서는 그룹화된 데이터에 대한 액세스 및 표시에 대해 살펴봅니다.
