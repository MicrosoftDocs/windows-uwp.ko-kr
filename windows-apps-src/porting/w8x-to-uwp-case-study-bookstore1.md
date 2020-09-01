---
title: Windows 런타임 .x에서 UWP 사례 연구, Bookstore1
ms.assetid: e4582717-afb5-4cde-86bb-31fb1c5fc8f3
description: 이 항목에서는 매우 간단한 유니버설 8.1 앱을 UWP (Windows 10 유니버설 Windows 플랫폼) 앱으로 이식 하는 사례 연구를 제공 합니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 98c74ac688707c6c80b9f3098760328fea0f852a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174857"
---
# <a name="windows-runtime-8x-to-uwp-case-study-bookstore1"></a>Windows 런타임 .x에서 UWP 사례 연구: Bookstore1


이 항목에서는 매우 간단한 유니버설 8.1 앱을 UWP (Windows 10 유니버설 Windows 플랫폼) 앱으로 이식 하는 사례 연구를 제공 합니다. 유니버설 8.1 앱은 Windows 8.1용 앱 패키지와 Windows Phone 8.1용 앱 패키지를 각각 빌드한 앱입니다. Windows 10에서는 고객이 다양 한 장치에 설치할 수 있는 단일 앱 패키지를 만들 수 있으며,이 사례 연구에서이 작업을 수행할 수 있습니다. [UWP 앱에 대 한 가이드를](../get-started/universal-application-platform-guide.md)참조 하세요.

앱은 뷰 모델에 바인딩된 **ListBox** 로 구성 됩니다. 보기 모델에는 제목, 저자 및 책 커버를 보여 주는 책 목록이 있습니다. 서적 커버 이미지는 **빌드 작업** 을 **콘텐츠** 로 설정 하 고 **출력 디렉터리로 복사** 를 **복사 안 함**으로 설정 합니다.

이 단원의 이전 항목에서는 플랫폼 간의 차이점에 대해 설명 하 고, 뷰 모델에 대 한 바인딩을 통해 데이터에 액세스 하는 것을 통해 XAML 태그에서 앱의 다양 한 측면에 대 한 이식 프로세스에 대 한 세부 정보 및 지침을 제공 합니다. 사례 연구는 실제 예제에서 작업을 수행 하 여 해당 지침을 보완 하는 것을 목표로 합니다. 사례 연구에서는 반복 되지 않는 지침을 읽은 것으로 가정 합니다.

**참고**    \_Visual studio에서 Bookstore1Universal 10을 열 때 "Visual studio 업데이트 필요" 메시지가 표시 되 면 [Targetplatformversion](w8x-to-uwp-troubleshooting.md)의 단계를 따릅니다.

## <a name="downloads"></a>다운로드

[Bookstore1 \_ 81 Universal 8.1 앱을 다운로드](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore1_81)합니다.

[Bookstore1Universal \_ 10 Windows 10 앱을 다운로드](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore1Universal_10)합니다.

## <a name="the-universal-81-app"></a>유니버설 8.1 앱

Bookstore1 \_ 81은 다음과 같으며, 포트를 사용할 앱은 다음과 같습니다. 앱의 이름 및 페이지 제목 아래에 있는 책의 세로 스크롤 목록 상자 일 뿐입니다.

![\-windows에서 bookstore1 81를 찾는 방법](images/w8x-to-uwp-case-studies/c01-01-win81-how-the-app-looks.png)

\_Windows의 Bookstore1 81

![\-windows phone에서 bookstore1 81를 찾는 방법](images/w8x-to-uwp-case-studies/c01-02-wp81-how-the-app-looks.png)

\_Windows Phone Bookstore1 81

##  <a name="porting-to-a-windows10-project"></a>Windows 10 프로젝트로 포팅

Bookstore1 \_ 81 솔루션은 8.1 유니버설 앱 프로젝트 이며, 이러한 프로젝트를 포함 합니다.

-   Bookstore1 \_ 81. Windows 8.1에 대 한 앱 패키지를 빌드하는 프로젝트입니다.
-   Bookstore1 \_ 81. appname.windowsphone. Windows Phone 8.1에 대 한 앱 패키지를 빌드하는 프로젝트입니다.
-   Bookstore1 \_ 81. 이는 다른 두 프로젝트 모두에서 사용 되는 소스 코드, 태그 파일 및 기타 자산 및 리소스를 포함 하는 프로젝트입니다.

이 사례 연구를 위해 지원 되는 장치에 대해 [Universal 8.1 앱](w8x-to-uwp-root.md) 이 있는 경우에 설명 된 일반적인 옵션을 사용할 수 있습니다. 여기에서 결정 한 사항은 간단 합니다 .이 앱은 동일한 기능을 가지 며 대부분의 Windows 8.1 및 Windows Phone 8.1 폼에서 같은 코드를 사용 하 여 수행 합니다. 따라서 공유 프로젝트의 콘텐츠 (및 다른 프로젝트에서 필요한 모든 항목)를 유니버설 장치 제품군을 대상으로 하는 Windows 10 (가장 넓은 범위의 장치에 설치할 수 있음)으로 이식 합니다.

Visual Studio에서 새 프로젝트를 만들고, Bookstore1 81에서 파일에 파일을 복사 하 \_ 고, 복사한 파일을 새 프로젝트에 포함 하는 것이 매우 빠른 작업입니다. 새 빈 응용 프로그램 (Windows 유니버설) 프로젝트를 만들어 시작 합니다. 이름을 Bookstore1Universal \_ 10으로 합니다. Bookstore1 \_ 81에서 Bookstore1Universal 10으로 복사할 파일 \_ 입니다.

**공유 프로젝트에서**

-   책 표지 이미지를 포함 하는 폴더를 복사 합니다. PNG 파일 \\ ( \\ CoverImages 폴더)입니다. 폴더를 복사한 후 **솔루션 탐색기**에서 **모든 파일 표시** 가 설정 되어 있는지 확인 합니다. 복사한 폴더를 마우스 오른쪽 단추로 클릭 하 고 **프로젝트에 포함**을 클릭 합니다. 이 명령은 프로젝트의 파일 또는 폴더를 "포함" 하 여 의미 합니다. 파일이 나 폴더를 복사할 때마다 각 복사본은 **솔루션 탐색기** 에서 **새로 고침** 을 클릭 한 다음 프로젝트에 파일 또는 폴더를 포함 합니다. 대상에서 대체 하는 파일에 대해서는이 작업을 수행할 필요가 없습니다.
-   뷰 모델 원본 파일이 포함 된 폴더를 복사 \\ 합니다 (ViewModel).
-   MainPage을 복사 하 고 대상의 파일을 바꿉니다.

**Windows 프로젝트에서**

-   BookstoreStyles을 복사 합니다. 이 파일의 모든 리소스 키가 Windows 10 앱에서 확인 되기 때문에이를 적절 한 시작 지점으로 사용 합니다. 해당 하는 Appname.windowsphone 파일에 있는 일부는 그렇지 않습니다.

방금 복사한 소스 코드 및 태그 파일을 편집 하 고 Bookstore1 81 네임 스페이스에 대 한 참조를 \_ Bookstore1Universal 10으로 변경 합니다 \_ . 이 작업을 수행 하는 빠른 방법은 **파일에서 바꾸기** 기능을 사용 하는 것입니다. 뷰 모델에는 코드 변경이 필요 하지 않으며 다른 명령적 코드에도 필요 하지 않습니다. 그러나 실행 중인 앱의 버전을 보다 쉽게 확인할 수 있도록 하려면 **Bookstore1Universal \_ ** 속성에서 반환 하는 값을 "BOOKSTORE1 \_ 81"에서 "Bookstore1Universal 10"으로 변경 합니다 \_ .

이제를 빌드하고 실행할 수 있습니다. 새 UWP 앱이 아직 Windows 10으로 이식할 수 있는 명시적인 작업을 수행 하지 않은 후에 표시 되는 방법은 다음과 같습니다.

![초기 소스 코드가 변경 된 windows 10 앱](images/w8x-to-uwp-case-studies/c01-03-desk10-initial-source-code-changes.png)

데스크톱 장치에서 실행 되는 초기 소스 코드 변경 내용이 포함 된 Windows 10 앱

![초기 소스 코드가 변경 된 windows 10 앱](images/w8x-to-uwp-case-studies/c01-04-mob10-initial-source-code-changes.png)

모바일 장치에서 실행 되는 초기 소스 코드 변경 내용이 포함 된 Windows 10 앱

뷰 및 뷰 모델이 제대로 작동 하 고 **ListBox** 가 작동 합니다. 스타일을 수정 하기만 하면 됩니다. 밝은 테마의 모바일 장치에서는 목록 상자의 테두리를 볼 수 있지만이는 쉽게 숨길 수 있습니다. 그리고 입력 체계가 너무 커서 사용 중인 스타일을 변경 합니다. 또한 앱은 데스크톱 장치에서 실행 되는 경우 색으로 표시 됩니다 (기본값). 따라서 변경 될 예정입니다.

## <a name="universal-styling"></a>유니버설 스타일 지정

Bookstore1 \_ 81 앱은 두 가지 리소스 사전 (BookstoreStyles)을 사용 하 여 Windows 8.1 및 Windows Phone 8.1 운영 체제에 맞게 스타일을 조정 합니다. 이러한 두 BookstoreStyles 파일은 Windows 10 앱에 필요한 스타일을 정확히 포함 하지 않습니다. 하지만 원하는 것은 원하는 것 보다 훨씬 더 간단 합니다. 따라서 다음 단계에서는 주로 프로젝트 파일 및 태그를 제거 하 고 단순화 합니다. 단계는 다음과 같습니다. 이 항목의 맨 위에 있는 링크를 사용 하 여 프로젝트를 다운로드 하 고 여기와 사례 연구의 끝 사이에 있는 모든 변경 내용에 대 한 결과를 볼 수 있습니다.

-   항목 사이의 간격을 강화 하려면 `BookTemplate` mainpage에서 데이터 템플릿을 찾고 `Margin="0,0,0,8"` 루트 **표에서**를 삭제 합니다.
-   또한 `BookTemplate`에는 `BookTemplateTitleTextBlockStyle` 및 `BookTemplateAuthorTextBlockStyle`에 대한 참조가 있습니다. Bookstore1 \_ 81는 두 앱에서 단일 키의 구현이 서로 다르므로 해당 키를 간접 참조로 사용 했습니다. 해당 간접 참조가 필요 하지 않습니다. 시스템 스타일을 직접 참조할 수 있습니다. 따라서 이러한 참조를 `TitleTextBlockStyle` 각각 및로 대체 `SubtitleTextBlockStyle` 합니다.
-   이제 `LayoutRoot` 테마의 내용에 관계 없이 모든 장치에서 실행 될 때 앱이 적절 하 게 보이도록 올바른 기본값으로를 설정 해야 합니다. 에서로 변경 `"Transparent"` `"{ThemeResource ApplicationPageBackgroundThemeBrush}"` 합니다.
-   에서에 대 한 참조를에 대 한 `TitlePanel` `TitleTextBlockStyle` 참조 (이제는 조금 큼)로 변경 `CaptionTextBlockStyle` 합니다. `PageTitleTextBlockStyle` 는 \_ 더 이상 필요 하지 않은 또 다른 Bookstore1 81 간접 참조입니다. 대신 참조로 변경 `HeaderTextBlockStyle` 합니다.
-   **ListBox**에 대해 더 이상 특수한 Background, Style, ItemContainerStyle을 설정할 필요가 없으므로 태그에서 이러한 세 특성과 해당 값을 삭제합니다. 그러나 **ListBox**의 테두리를 숨기 려 하므로 추가 `BorderBrush="{x:Null}"` 합니다.
-   더 이상 BookstoreStyles **ResourceDictionary** 파일의 리소스를 참조 하 고 있지 않습니다. 이러한 모든 리소스를 삭제할 수 있습니다. 그러나 BookstoreStyles 파일 자체는 삭제 하지 마세요. 다음 섹션에서 볼 수 있듯이이 파일은 마지막으로 사용 됩니다.

스타일 지정 작업의 마지막 시퀀스는 앱을 다음과 같이 그대로 둡니다.

![거의 포팅 된 windows 10 앱](images/w8x-to-uwp-case-studies/c01-05-desk10-almost-ported.png)

데스크톱 장치에서 실행 되는 거의 포팅 된 Windows 10 앱

![거의 포팅 된 windows 10 앱](images/w8x-to-uwp-case-studies/c01-06-mob10-almost-ported.png)

모바일 장치에서 실행 되는 거의 포팅 된 Windows 10 앱

## <a name="an-optional-adjustment-to-the-list-box-for-mobile-devices"></a>모바일 장치의 목록 상자에 대 한 선택적 조정

앱이 모바일 장치에서 실행 되는 경우 목록 상자의 배경은 기본적으로 두 테마에 모두 표시 됩니다. 원하는 스타일 일 수 있으며,이 경우에는 정리를 제외 하 고 더 이상 수행할 작업이 없습니다. 프로젝트에서 BookstoreStyles 리소스 사전 파일을 삭제 하 고이를 MainPage에 병합 하는 태그를 제거 합니다.

그러나 컨트롤은 동작의 영향을 받지 않고 모양을 사용자 지정할 수 있도록 디자인 되었습니다. 따라서 원본 앱이 표시 되는 방식으로 목록 상자를 어두운 테마에 어둡게 표시 하려면이 섹션에서이 작업을 수행 하는 방법을 설명 합니다.

변경 내용은 모바일 장치에서 실행 되는 경우에만 앱에 영향을 미칩니다. 따라서 모바일 장치 제품군에서 실행 되는 경우 매우 약간 사용자 지정 된 목록 상자 스타일을 사용 하 고 다른 모든 위치에서 실행 하는 경우에는 기본 스타일을 계속 사용할 수 있습니다. 이렇게 하려면 BookstoreStyles의 복사본을 만들고,이를 특정 MRT.LOG 정규화 된 이름으로 지정 하면 모바일 장치에만 로드 됩니다.

새 **ResourceDictionary** 프로젝트 항목을 추가 하 고 이름을 BookstoreStyles로 추가 합니다. 이제 두 파일 모두 논리 이름이 BookstoreStyles.xaml입니다. 그리고 태그 및 코드에서 이 이름을 사용하게 됩니다. 그러나 파일은 서로 다른 실제 이름을 가지 므로 다른 태그를 포함할 수 있습니다. 모든 xaml 파일에서이 정규화 된 이름 지정 스키마를 사용할 수 있지만, 논리적 이름이 동일한 모든 xaml 파일은 단일 xaml.cs 코드 숨김이 아닌 파일 (있는 경우)을 공유 합니다.

목록 상자에 대 한 컨트롤 템플릿 복사본을 편집 하 고 `BookstoreListBoxStyle` 새 리소스 사전 BookstoreStyles에의 키를 사용 하 여 저장 합니다. 이제는 세 개의 setter를 간단히 변경할 것입니다.

-   전경 setter에서 값을로 변경 `"{x:Null}"` 합니다. 요소에 대 한 속성을 직접 설정 하는 것 `"{x:Null}"` 은 코드에서로 설정 하는 것과 같습니다 `null` . 그러나 setter에서 값을 사용 하면 `"{x:Null}"` 고유한 결과가 있습니다. 즉, 동일한 속성에 대 한 기본 스타일의 setter를 재정의 하 고 대상 요소에서 속성의 기본값을 복원 합니다.
-   배경 setter에서 값을로 변경 하 여 `"Transparent"` 이 밝은 배경을 제거 합니다.
-   템플릿 setter에서 라는 시각적 상태를 찾고 `Focused` 해당 Storyboard를 삭제 하 여 빈 태그로 만듭니다.
-   태그에서 다른 모든 setter를 삭제 합니다.

마지막으로 `BookstoreListBoxStyle` BookstoreStyles에 복사 하 고 세 개의 setter를 삭제 하 여 빈 태그로 만듭니다. 모바일 컴퓨터 이외의 장치에서 BookstoreStyles 및에 대 한 참조는 해결 되지만 아무런 영향을 주지 않습니다 `BookstoreListBoxStyle` .

![포팅 된 windows 10 앱](images/w8x-to-uwp-case-studies/c01-07-mob10-ported.png)

모바일 장치에서 실행 되는 이식 된 Windows 10 앱

## <a name="conclusion"></a>결론

이 사례 연구는 매우 간단한 앱을 이식 하는 프로세스를 보여 주었습니다. 통해 비현실적으로 단순 합니다. 예를 들어 목록 상자를 선택 하거나 탐색을 위한 컨텍스트를 설정 하는 데 사용할 수 있습니다. 앱은 탭 된 항목에 대 한 자세한 정보를 포함 하는 페이지로 이동 합니다. 이 특정 앱은 사용자가 선택 하는 것이 없으며 탐색이 없습니다. 그러나 사례 연구는 ice를 중단 하 고 포팅 프로세스를 소개 하며 실제 UWP 앱에서 사용할 수 있는 중요 한 기술을 보여 주기 위해 제공 됩니다.

또한 이식 가능한 뷰 모델을 일반적으로 처리 하는 증거를 살펴보았습니다. 사용자 인터페이스 및 폼 팩터 지원은 이식할 때 주의가 필요할 수 있는 측면입니다.

다음 사례 연구는 그룹화 된 데이터에 액세스 하 고 표시 하는 것을 [Bookstore2](w8x-to-uwp-case-study-bookstore2.md).