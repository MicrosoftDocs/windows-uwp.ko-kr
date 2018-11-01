---
author: stevewhims
title: Windows 런타임 8.x에서 UWP로 이동 사례 연구, Bookstore1
ms.assetid: e4582717-afb5-4cde-86bb-31fb1c5fc8f3
description: 이 항목에서는 매우 간단한 유니버설 8.1 앱을 Windows10 유니버설 Windows 플랫폼 (UWP) 앱으로 포팅하는 사례 연구를 제공 합니다.
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: cec8171b381a607616e2054784fa888074d3f90e
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5870081"
---
# <a name="windows-runtime-8x-to-uwp-case-study-bookstore1"></a>Windows 런타임 8.x에서 UWP로 이동 사례 연구: Bookstore1


이 항목에서는 매우 간단한 유니버설 8.1 앱을 Windows10Universal Windows 플랫폼 (UWP) 앱으로 포팅하는 사례 연구를 제공 합니다. 유니버설 8.1 앱은 Windows8.1, 하나의 앱 패키지 및 Windows Phone 8.1 용 앱 패키지 빌드입니다. Windows10를 만들 수 있습니다 단일 앱 패키지는이 사례 연구에서 작업을 어떻게 수행 되는 및 고객이 다양 한 장치에 설치할 수 있습니다. [UWP 앱 지침](https://msdn.microsoft.com/library/windows/apps/dn894631)을 참조하세요.

포팅할 앱은 보기 모델에 바인딩되는 **ListBox**로 구성됩니다. 보기 모델에는 제목, 저자 및 책 표지를 보여주는 책 목록이 있습니다. 책 표지 이미지에는 **빌드 작업**이 **콘텐츠**로 설정되어 있고 **출력 디렉터리로 복사**가 **복사 안 함**으로 설정되어 있습니다.

이 섹션의 이전 항목에서는 플랫폼 간의 차이점에 대해 설명하고 XAML 태그, 보기 모델에 바인딩, 데이터 액세스 등 앱의 다양한 측면에 대한 포팅 프로세스와 관련된 세부 정보와 지침을 제공했습니다. 사용 사례는 실제 작업 사례를 제공하여 지침을 보완하는 데 목표를 두고 있습니다. 사례 연구에서는 지침을 이미 읽었다고 가정하고 더 이상 반복하지 않습니다.

**참고**  [TargetPlatformVersion](w8x-to-uwp-troubleshooting.md)의 단계에 따라 다음 때 "Visual Studio 업데이트 필요" 메시지가 표시 되 면 Bookstore1Universal\_10 Visual Studio에서 열기.

## <a name="downloads"></a>다운로드

[Bookstore1\_81 유니버설 8.1 앱을 다운로드합니다](http://go.microsoft.com/fwlink/?linkid=532946).

[Windows10 앱을 다운로드 Bookstore1Universal\_10 합니다](http://go.microsoft.com/fwlink/?linkid=532950).

## <a name="the-universal-81-app"></a>유니버설 8.1 앱

포팅할 Bookstore1\_81 앱은 다음과 같습니다. 앱의 이름과 페이지 제목 아래에는 세로로 스크롤되는 책의 목록 상자가 있습니다.

![Windows에서 bookstore1\-81의 모양](images/w8x-to-uwp-case-studies/c01-01-win81-how-the-app-looks.png)

Windows의 Bookstore1\_81

![Windows Phone에서 bookstore1\-81의 모양](images/w8x-to-uwp-case-studies/c01-02-wp81-how-the-app-looks.png)

Windows Phone의 Bookstore1\_81

##  <a name="porting-to-a-windows10-project"></a>Windows10 프로젝트로 포팅

Bookstore1\_81 솔루션은 8.1 유니버설 앱 프로젝트이며 다음 프로젝트를 포함합니다.

-   Bookstore1\_81.Windows. Windows8.1 용 앱 패키지를 빌드하는 프로젝트입니다.
-   Bookstore1\_81.WindowsPhone. Windows Phone 8.1용 앱 패키지를 빌드하는 프로젝트입니다.
-   Bookstore1\_81.Shared. 두 프로젝트 모두에서 사용되는 소스 코드, 태그 파일, 기타 자산 및 리소스가 포함된 프로젝트입니다.

이 사례 연구를 위해 지원할 장치와 관련하여 [유니버설 8.1 앱이 있는 경우](w8x-to-uwp-root.md)에 설명된 일반적인 옵션을 제공합니다. 다음 의사 결정은 간단한:이 앱에는 동일한 기능이 및 대부분의 Windows8.1 및 Windows Phone 8.1 양식에서 동일한 코드를 사용 하지 않습니다. 따라서 (광범위 한 디바이스에 설치할 수 하나) 유니버설 디바이스 패밀리를 대상으로 Windows10을 공유 프로젝트 (및 다른 프로젝트의 필요한 항목)의 내용을 포팅 합니다.

Visual Studio에서 새 프로젝트를 만들고 Bookstore1\_81의 파일을 이 프로젝트로 복사한 후 복사한 파일을 새 프로젝트에 포함하는 과정은 매우 빠르게 진행되는 작업입니다. 비어 있는 응용 프로그램(Windows 유니버설) 프로젝트를 새로 만들어 시작합니다. Bookstore1Universal\_10이라고 이름을 지정합니다. 다음은 Bookstore1\_81에서 Bookstore1Universal\_10으로 복사할 파일입니다.

**공유 프로젝트에서**

-   책 표지 이미지 PNG 파일이 들어 있는 폴더를 복사합니다(폴더는 \\Assets\\CoverImages임) 폴더를 복사한 후에 **솔루션 탐색기**에서 **모든 파일 표시**가 설정되어 있는지 확인합니다. 복사한 폴더를 마우스 오른쪽 단추로 클릭하고 **프로젝트에 포함**을 클릭합니다. 이 명령은 파일이나 폴더를 프로젝트에 "포함"하여 우리가 의도한 작업을 진행합니다. 파일이나 폴더를 복사할 때마다 **솔루션 탐색기**에서 **새로 고침**을 클릭한 다음 프로젝트에 파일 또는 폴더를 포함합니다. 대상에서 바꾸려는 파일에 대해서는 이 작업을 수행하지 않아도 됩니다.
-   보기 모델 소스 파일이 포함된 폴더(\\ViewModel)를 복사합니다.
-   MainPage.xaml을 복사한 후 대상의 파일을 바꿉니다.

**Windows 프로젝트에서**

-   BookstoreStyles.xaml을 복사합니다. 이 파일에 있는 모든 리소스 키를 Windows10 앱; 해결할 수 있으므로이 가장 좋은 시작 지점으로 사용 됩니다. 해당 하는 WindowsPhone 파일의 일부는 없습니다.

방금 복사한 소스 코드 및 태그 파일을 편집하고 Bookstore1\_81 네임스페이스에 대한 참조를 Bookstore1Universal\_10으로 변경합니다. **파일에서 바꾸기** 기능을 사용하면 이 작업을 빠르게 수행할 수 있습니다. 보기 모델과 다른 명령적 코드에서 코드를 변경할 필요가 없습니다. 그렇지만 실행 중인 앱 버전을 더 쉽게 식별하려면 **Bookstore1Universal\_10.BookstoreViewModel.AppName** 속성에 의해 반환된 값을 "BOOKSTORE1\_81"에서 "BOOKSTORE1UNIVERSAL\_10"으로 변경하세요.

이제 앱을 빌드 및 실행할 수 있습니다. 새 UWP 앱의 모양은 Windows10으로 포팅할을 아직 없는 명시적 작업 수행 다음과 같습니다.

![초기 소스 코드가 변경된 Windows 10 앱](images/w8x-to-uwp-case-studies/c01-03-desk10-initial-source-code-changes.png)

데스크톱 장치에서 실행 중인 초기 소스 코드가 변경 된 Windows10 앱

![초기 소스 코드가 변경된 Windows 10 앱](images/w8x-to-uwp-case-studies/c01-04-mob10-initial-source-code-changes.png)

모바일 장치에서 실행 중인 초기 소스 코드가 변경 된 Windows10 앱

보기와 보기 모델이 함께 올바르게 작동하고 **ListBox**가 작동하고 있습니다. 스타일만 수정하면 됩니다. 밝은 테마의 모바일 장치에서는 목록 상자 테두리를 볼 수 있지만 쉽게 숨길 수 있습니다. 또한 서체가 너무 커서 사용 중인 스타일을 변경할 것입니다. 그뿐만 아니라 앱을 데스크톱 장치에서 실행할 경우 기본 색상으로 보이게 하려면 더 밝게 지정해야 합니다. 따라서 색상도 변경할 것입니다.

## <a name="universal-styling"></a>범용 스타일 지정

Bookstore1\_81 앱은 스타일을 Windows8.1 및 Windows Phone 8.1 운영 체제에 맞게 두 개의 다른 리소스 사전 (BookstoreStyles.xaml)을 사용 합니다. 두 BookstoreStyles.xaml 파일 모두 Windows10 앱에 필요한 스타일을 정확히 포함 되어 있습니다. 하지만 다행히도 우리가 원하는 결과는 실제로 이러한 두 파일의 스타일보다 훨씬 더 간단합니다. 따라서 다음 단계에서는 프로젝트 파일 및 태그를 제거하고 단순화하는 작업을 주로 진행할 것입니다. 단계는 다음과 같습니다. 이 항목 맨 위에 있는 링크를 사용하여 프로젝트를 다운로드하고, 여기서부터 사례 연구 끝까지 포함된 모든 변경의 결과를 확인할 수 있습니다.

-   항목 사이의 간격을 촘촘히 하려면 MainPage.xaml에서 `BookTemplate` 데이터 템플릿을 찾고 **Grid** 루트에서 `Margin="0,0,0,8"`을 삭제합니다.
-   또한 `BookTemplate`에는 `BookTemplateTitleTextBlockStyle` 및 `BookTemplateAuthorTextBlockStyle`에 대한 참조가 있습니다. Bookstore1\_81은 단일 키가 두 앱에서 다르게 구현되도록 해당 키를 간접 참조로 사용했습니다. 해당 간접 참조는 더 이상 필요하지 않습니다. 시스템 스타일은 직접 참조할 수만 있습니다. 따라서 해당 참조를 `TitleTextBlockStyle` 및 `SubtitleTextBlockStyle`로 각각 바꿉니다.
-   이제 테마에 관계없이 앱이 모든 디바이스에서 실행될 때 적절해 보이도록 `LayoutRoot`의 Background를 올바른 기본값으로 설정해야 합니다. `"Transparent"`에서 `"{ThemeResource ApplicationPageBackgroundThemeBrush}"`로 변경합니다.
-   `TitlePanel`에서 `TitleTextBlockStyle`(현재 너무 큼)에 대한 참조를 `CaptionTextBlockStyle`에 대한 참조로 변경합니다. `PageTitleTextBlockStyle` 은 더 이상 필요 하지 않은 다른 Bookstore1\_81 간접 참조입니다. 대신 참조 `HeaderTextBlockStyle`로 변경합니다.
-   **ListBox**에 대해 더 이상 특수한 Background, Style, ItemContainerStyle을 설정할 필요가 없으므로 태그에서 이러한 세 특성과 해당 값을 삭제합니다. 그렇지만 **ListBox**의 테두리를 숨기려고 하므로 `BorderBrush="{x:Null}"`을 추가합니다.
-   더 이상 BookstoreStyles.xaml **ResourceDictionary** 파일에서 어떤 리소스도 참조하지 않을 것입니다. 모든 리소스를 삭제할 수 있습니다. 하지만 BookstoreStyles.xaml 파일 자체를 삭제하지는 않도록 합니다. 다음 섹션에서 확인할 수 있지만 한 가지 목적으로 더 사용해야 합니다.

스타일 작업의 마지막 시퀀스를 진행하면 앱이 다음과 같은 모양을 유지합니다.

![포팅 직전의 Windows 10 앱](images/w8x-to-uwp-case-studies/c01-05-desk10-almost-ported.png)

데스크톱 장치에서 실행 되는 직전 Windows10 앱

![포팅 직전의 Windows 10 앱](images/w8x-to-uwp-case-studies/c01-06-mob10-almost-ported.png)

모바일 장치에서 실행 되는 직전 Windows10 앱

## <a name="an-optional-adjustment-to-the-list-box-for-mobile-devices"></a>모바일 장치에 대한 목록 상자를 선택적으로 조정

앱이 모바일 장치에서 실행되는 경우 기본적으로 두 테마에서 목록 상자의 배경이 밝습니다. 이러한 스타일을 선호한다면 추가적인 정리 외에는 더 수행할 작업이 없습니다. 예를 들어 프로젝트에서 BookstoreStyles.xaml 리소스 사전 파일을 삭제하고 MainPage.xaml에 병합하는 태그를 제거합니다.

그러나 컨트롤은 해당 동작에 영향을 주지 않으면서 모양을 사용자 지정할 수 있도록 디자인되어 있습니다. 원래 앱의 모양대로 어두운 테마에서 목록 상자를 어둡게 유지하려면 이 섹션의 방법을 참조하세요.

즉, 모바일 디바이스에서 실행되는 앱에는 영향을 주도록 변경하면 됩니다. 따라서 모바일 디바이스 패밀리에서 실행할 경우 약간만 사용자 지정한 목록 상자 스타일을 사용하고 다른 위치에서 실행할 경우에는 기본 스타일을 계속 사용할 것입니다. 이렇게 하려면 BookstoreStyles.xaml의 복사본을 만들고, 모바일 디바이스에서만 로드되도록 하는 특별한 MRT 정규화된 이름을 복사본에 지정합니다.

새 **ResourceDictionary** 프로젝트 항목을 추가하고 BookstoreStyles.DeviceFamily-Mobile.xaml로 이름을 지정합니다. 이제 두 파일 모두 논리 이름이 BookstoreStyles.xaml입니다. 그리고 태그 및 코드에서 이 이름을 사용하게 됩니다. 그렇지만 다른 태그를 포함할 수 있도록 파일의 실제 이름은 다릅니다. xaml 파일에 이 MRT 정규화된 명명 스키마를 사용할 수 있지만 동일한 논리 이름의 모든 xaml 파일은 단일 xaml.cs 코드 숨김 파일을 공유해야 합니다(하나의 파일이 여기에 해당).

목록 상자에 대한 컨트롤 템플릿 복사본을 편집하고 새 리소스 사전인 BookstoreStyles.DeviceFamily Mobile.xaml에 `BookstoreListBoxStyle`의 키와 함께 저장합니다. 이제 세 개의 setter를 간단히 변경해 봅니다.

-   Foreground setter에서 값을 `"{x:Null}"`로 변경합니다. 속성을 요소에서 직접 `"{x:Null}"`로 설정하는 것은 코드에서 `null`로 설정하는 것과 같습니다. 그렇지만 setter에서 `"{x:Null}"` 값을 사용하면 특별한 효과가 나타납니다. (동일한 속성에 대해) 기본 스타일에서 setter가 재정의되고 대상 요소에서 속성의 기본값이 복원됩니다.
-   Background setter에서는 값을 `"Transparent"`로 변경하여 밝은 배경을 제거합니다.
-   Template setter에서는 `Focused`라는 시각적 상태를 찾은 후 해당 스토리보드를 삭제하여 빈 태그로 만듭니다.
-   태그의 다른 모든 setter를 삭제합니다.

마지막으로 `BookstoreListBoxStyle`을 BookstoreStyles.xaml에 복사하고 세 개의 해당 setter를 삭제하여 빈 태그로 만듭니다. 모바일 디바이스 이외의 디바이스에서 이 작업을 수행하면 BookstoreStyles.xaml 및 `BookstoreListBoxStyle`에 대한 참조가 확인되지만 효과는 없습니다.

![포팅된 Windows 10 앱](images/w8x-to-uwp-case-studies/c01-07-mob10-ported.png)

모바일 장치에서 실행 되는 포팅된 Windows10 앱

## <a name="conclusion"></a>결론

이 사례 연구에서는 비현실적으로 간단한 앱을 포팅하는 프로세스에 대해 살펴보았습니다. 예를 들어 목록 상자를 사용하여 선택하거나 탐색할 컨텍스트를 설정할 수 있습니다. 앱에서는 탭되는 항목에 대한 세부 정보가 나오는 페이지를 탐색합니다. 이 특정 앱에서는 사용자 선택과 관련하여 아무 작업도 수행하지 않고 탐색도 하지 않습니다. 하지만 이 사례 연구에서는 분위기를 쇄신하고 포팅 프로세스를 소개하고 실제 UWP 앱에서 사용할 수 있는 중요한 기술을 보여 주었습니다.

또한 포팅한 보기 모델이 일반적으로 매끄러운 프로세스라는 증거도 살펴보았습니다. 사용자 인터페이스 및 폼 팩터 지원은 포팅할 때 주의해야 하는 부분입니다.

다음 사례 연구는 [Bookstore2](w8x-to-uwp-case-study-bookstore2.md)이며, 여기에서는 그룹화된 데이터에 대한 액세스 및 표시에 대해 살펴봅니다.
