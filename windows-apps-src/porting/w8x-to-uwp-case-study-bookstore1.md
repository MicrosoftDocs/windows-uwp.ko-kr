---
title: Windows 런타임 8.x에서 UWP로 이동 사례 연구, Bookstore1
ms.assetid: e4582717-afb5-4cde-86bb-31fb1c5fc8f3
description: This topic presents a case study of porting a very simple Universal 8.1 app to a Windows 10 Universal Windows Platform (UWP) app.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ecbc4d3fcdff9d06469e7a274fd56ef453a81e02
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260127"
---
# <a name="windows-runtime-8x-to-uwp-case-study-bookstore1"></a>Windows 런타임 8.x에서 UWP로 이동 사례 연구: Bookstore1


This topic presents a case study of porting a very simple Universal 8.1 app to a Windows 10 Universal Windows Platform (UWP) app. A Universal 8.1 app is one that builds one app package for Windows 8.1, and a different app package for Windows Phone 8.1. With Windows 10, you can create a single app package that your customers can install onto a wide range of devices, and that's what we'll do in this case study. [UWP 앱 지침](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)을 참조하세요.

포팅할 앱은 보기 모델에 바인딩되는 **ListBox**로 구성됩니다. 보기 모델에는 제목, 저자 및 책 표지를 보여주는 책 목록이 있습니다. 책 표지 이미지에는 **빌드 작업**이 **콘텐츠**로 설정되어 있고 **출력 디렉터리로 복사**가 **복사 안 함**으로 설정되어 있습니다.

이 섹션의 이전 항목에서는 플랫폼 간의 차이점에 대해 설명하고 XAML 태그, 보기 모델에 바인딩, 데이터 액세스 등 앱의 다양한 측면에 대한 포팅 프로세스와 관련된 세부 정보와 지침을 제공했습니다. 사용 사례는 실제 작업 사례를 제공하여 지침을 보완하는 데 목표를 두고 있습니다. 사례 연구에서는 지침을 이미 읽었다고 가정하고 더 이상 반복하지 않습니다.

**Note**   When opening Bookstore1Universal\_10 in Visual Studio, if you see the message "Visual Studio update required", then follow the steps in [TargetPlatformVersion](w8x-to-uwp-troubleshooting.md).

## <a name="downloads"></a>다운로드

[Download the Bookstore1\_81 Universal 8.1 app](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore1_81).

[Download the Bookstore1Universal\_10 Windows 10 app](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore1Universal_10).

## <a name="the-universal-81-app"></a>유니버설 8.1 앱

Here’s what Bookstore1\_81—the app that we're going to port—looks like. 앱의 이름과 페이지 제목 아래에는 세로로 스크롤되는 책의 목록 상자가 있습니다.

![how bookstore1\-81 looks on windows](images/w8x-to-uwp-case-studies/c01-01-win81-how-the-app-looks.png)

Bookstore1\_81 on Windows

![how bookstore1\-81 looks on windows phone](images/w8x-to-uwp-case-studies/c01-02-wp81-how-the-app-looks.png)

Bookstore1\_81 on Windows Phone

##  <a name="porting-to-a-windows10-project"></a>Porting to a Windows 10 project

The Bookstore1\_81 solution is an 8.1 Universal App project, and it contains these projects.

-   Bookstore1\_81.Windows. This is the project that builds the app package for Windows 8.1.
-   Bookstore1\_81.WindowsPhone. Windows Phone 8.1용 앱 패키지를 빌드하는 프로젝트입니다.
-   Bookstore1\_81.Shared. 두 프로젝트 모두에서 사용되는 소스 코드, 태그 파일, 기타 자산 및 리소스가 포함된 프로젝트입니다.

이 사례 연구를 위해 지원할 장치와 관련하여 [유니버설 8.1 앱이 있는 경우](w8x-to-uwp-root.md)에 설명된 일반적인 옵션을 제공합니다. The decision here is a simple one: this app has the same features, and does so mostly with the same code, in both its Windows 8.1 and Windows Phone 8.1 forms. So, we'll port the contents of the Shared project (and anything else we need from the other projects) to a Windows 10 that targets the Universal device family (one that you can install onto the widest range of devices).

It's a very quick task to create a new project in Visual Studio, copy files over to it from Bookstore1\_81, and include the copied files in the new project. 비어 있는 응용 프로그램(Windows 유니버설) 프로젝트를 새로 만들어 시작합니다. Name it Bookstore1Universal\_10. These are the files to copy over from Bookstore1\_81 to Bookstore1Universal\_10.

**From the Shared project**

-   Copy the folder containing the book cover image PNG files (the folder is \\Assets\\CoverImages). 폴더를 복사한 후에 **솔루션 탐색기**에서 **모든 파일 표시**가 설정되어 있는지 확인합니다. 복사한 폴더를 마우스 오른쪽 단추로 클릭하고 **프로젝트에 포함**을 클릭합니다. 이 명령은 파일이나 폴더를 프로젝트에 "포함"하여 우리가 의도한 작업을 진행합니다. 파일이나 폴더를 복사할 때마다 **솔루션 탐색기**에서 **새로 고침**을 클릭한 다음 프로젝트에 파일 또는 폴더를 포함합니다. 대상에서 바꾸려는 파일에 대해서는 이 작업을 수행하지 않아도 됩니다.
-   Copy the folder containing the view model source file (the folder is \\ViewModel).
-   MainPage.xaml을 복사한 후 대상의 파일을 바꿉니다.

**From the Windows project**

-   BookstoreStyles.xaml을 복사합니다. We'll use this one as a good starting-point because all the resource keys in this file will resolve in a Windows 10 app; some of those in the equivalent WindowsPhone file will not.

Edit the source code and markup files that you just copied and change any references to the Bookstore1\_81 namespace to Bookstore1Universal\_10. **파일에서 바꾸기** 기능을 사용하면 이 작업을 빠르게 수행할 수 있습니다. 보기 모델과 다른 명령적 코드에서 코드를 변경할 필요가 없습니다. But, just to make it easier to see which version of the app is running, change the value returned by the **Bookstore1Universal\_10.BookstoreViewModel.AppName** property from "BOOKSTORE1\_81" to "BOOKSTORE1UNIVERSAL\_10".

이제 앱을 빌드 및 실행할 수 있습니다. Here's how our new UWP app looks after having done no explicit work yet to port it to Windows 10.

![초기 소스 코드가 변경된 Windows 10 앱](images/w8x-to-uwp-case-studies/c01-03-desk10-initial-source-code-changes.png)

The Windows 10 app with initial source code changes running on a Desktop device

![초기 소스 코드가 변경된 Windows 10 앱](images/w8x-to-uwp-case-studies/c01-04-mob10-initial-source-code-changes.png)

The Windows 10 app with initial source code changes running on a Mobile device

보기와 보기 모델이 함께 올바르게 작동하고 **ListBox**가 작동하고 있습니다. 스타일만 수정하면 됩니다. 밝은 테마의 모바일 장치에서는 목록 상자 테두리를 볼 수 있지만 쉽게 숨길 수 있습니다. 또한 서체가 너무 커서 사용 중인 스타일을 변경할 것입니다. 그뿐만 아니라 앱을 데스크톱 장치에서 실행할 경우 기본 색상으로 보이게 하려면 더 밝게 지정해야 합니다. 따라서 색상도 변경할 것입니다.

## <a name="universal-styling"></a>범용 스타일 지정

The Bookstore1\_81 app used two different resource dictionaries (BookstoreStyles.xaml) to tailor its styles to the Windows 8.1 and Windows Phone 8.1 operating systems. Neither of those two BookstoreStyles.xaml files contains exactly the styles we need for our Windows 10 app. 하지만 다행히도 우리가 원하는 결과는 실제로 이러한 두 파일의 스타일보다 훨씬 더 간단합니다. 따라서 다음 단계에서는 프로젝트 파일 및 태그를 제거하고 단순화하는 작업을 주로 진행할 것입니다. 단계는 다음과 같습니다. 이 항목 맨 위에 있는 링크를 사용하여 프로젝트를 다운로드하고, 여기서부터 사례 연구 끝까지 포함된 모든 변경의 결과를 확인할 수 있습니다.

-   항목 사이의 간격을 촘촘히 하려면 MainPage.xaml에서 `BookTemplate` 데이터 템플릿을 찾고 **Grid** 루트에서 `Margin="0,0,0,8"`을 삭제합니다.
-   또한 `BookTemplate`에는 `BookTemplateTitleTextBlockStyle` 및 `BookTemplateAuthorTextBlockStyle`에 대한 참조가 있습니다. Bookstore1\_81 used those keys as an indirection so that a single key had different implementations in the two apps. 해당 간접 참조는 더 이상 필요하지 않습니다. 시스템 스타일은 직접 참조할 수만 있습니다. 따라서 해당 참조를 `TitleTextBlockStyle` 및 `SubtitleTextBlockStyle`로 각각 바꿉니다.
-   이제 테마에 관계없이 앱이 모든 디바이스에서 실행될 때 적절해 보이도록 `LayoutRoot`의 Background를 올바른 기본값으로 설정해야 합니다. `"Transparent"`에서 `"{ThemeResource ApplicationPageBackgroundThemeBrush}"`로 변경합니다.
-   `TitlePanel`에서 `TitleTextBlockStyle`(현재 너무 큼)에 대한 참조를 `CaptionTextBlockStyle`에 대한 참조로 변경합니다. `PageTitleTextBlockStyle` is another Bookstore1\_81 indirection that we don't need any longer. 대신 참조 `HeaderTextBlockStyle`로 변경합니다.
-   **ListBox**에 대해 더 이상 특수한 Background, Style, ItemContainerStyle을 설정할 필요가 없으므로 태그에서 이러한 세 특성과 해당 값을 삭제합니다. 그렇지만 **ListBox**의 테두리를 숨기려고 하므로 `BorderBrush="{x:Null}"`을 추가합니다.
-   더 이상 BookstoreStyles.xaml **ResourceDictionary** 파일에서 어떤 리소스도 참조하지 않을 것입니다. 모든 리소스를 삭제할 수 있습니다. 하지만 BookstoreStyles.xaml 파일 자체를 삭제하지는 않도록 합니다. 다음 섹션에서 확인할 수 있지만 한 가지 목적으로 더 사용해야 합니다.

스타일 작업의 마지막 시퀀스를 진행하면 앱이 다음과 같은 모양을 유지합니다.

![포팅 직전의 Windows 10 앱](images/w8x-to-uwp-case-studies/c01-05-desk10-almost-ported.png)

The almost-ported Windows 10 app running on a Desktop device

![포팅 직전의 Windows 10 앱](images/w8x-to-uwp-case-studies/c01-06-mob10-almost-ported.png)

The almost-ported Windows 10 app running on a Mobile device

## <a name="an-optional-adjustment-to-the-list-box-for-mobile-devices"></a>모바일 장치에 대한 목록 상자를 선택적으로 조정

앱이 모바일 장치에서 실행되는 경우 기본적으로 두 테마에서 목록 상자의 배경이 밝습니다. 이러한 스타일을 선호한다면 추가적인 정리 외에는 더 수행할 작업이 없습니다. 예를 들어 프로젝트에서 BookstoreStyles.xaml 리소스 사전 파일을 삭제하고 MainPage.xaml에 병합하는 태그를 제거합니다.

그러나 컨트롤은 해당 동작에 영향을 주지 않으면서 모양을 사용자 지정할 수 있도록 디자인되었습니다. 원래 앱의 모양대로 어두운 테마에서 목록 상자를 어둡게 유지하려면 이 섹션의 방법을 참조하세요.

즉, 모바일 디바이스에서 실행되는 앱에는 영향을 주도록 변경하면 됩니다. 따라서 모바일 디바이스 패밀리에서 실행할 경우 약간만 사용자 지정한 목록 상자 스타일을 사용하고 다른 위치에서 실행할 경우에는 기본 스타일을 계속 사용할 것입니다. 이렇게 하려면 BookstoreStyles.xaml의 복사본을 만들고, 모바일 디바이스에서만 로드되도록 하는 특별한 MRT 정규화된 이름을 복사본에 지정합니다.

새 **ResourceDictionary** 프로젝트 항목을 추가하고 BookstoreStyles.DeviceFamily-Mobile.xaml로 이름을 지정합니다. 이제 두 파일 모두 논리 이름이 BookstoreStyles.xaml입니다. 그리고 태그 및 코드에서 이 이름을 사용하게 됩니다. 그렇지만 다른 태그를 포함할 수 있도록 파일의 실제 이름은 다릅니다. xaml 파일에 이 MRT 정규화된 명명 스키마를 사용할 수 있지만 동일한 논리 이름의 모든 xaml 파일은 단일 xaml.cs 코드 숨김 파일을 공유해야 합니다(하나의 파일이 여기에 해당).

목록 상자에 대한 컨트롤 템플릿 복사본을 편집하고 새 리소스 사전인 BookstoreStyles.DeviceFamily Mobile.xaml에 `BookstoreListBoxStyle`의 키와 함께 저장합니다. 이제 세 개의 setter를 간단히 변경해 봅니다.

-   Foreground setter에서 값을 `"{x:Null}"`로 변경합니다. 속성을 요소에서 직접 `"{x:Null}"`로 설정하는 것은 코드에서 `null`로 설정하는 것과 같습니다. 그렇지만 setter에서 `"{x:Null}"` 값을 사용하면 특별한 효과가 나타납니다. (동일한 속성에 대해) 기본 스타일에서 setter가 재정의되고 대상 요소에서 속성의 기본값이 복원됩니다.
-   Background setter에서는 값을 `"Transparent"`로 변경하여 밝은 배경을 제거합니다.
-   Template setter에서는 `Focused`라는 시각적 상태를 찾은 후 해당 스토리보드를 삭제하여 빈 태그로 만듭니다.
-   태그의 다른 모든 setter를 삭제합니다.

마지막으로 `BookstoreListBoxStyle`을 BookstoreStyles.xaml에 복사하고 세 개의 해당 setter를 삭제하여 빈 태그로 만듭니다. 모바일 디바이스 이외의 디바이스에서 이 작업을 수행하면 BookstoreStyles.xaml 및 `BookstoreListBoxStyle`에 대한 참조가 확인되지만 효과는 없습니다.

![포팅된 Windows 10 앱](images/w8x-to-uwp-case-studies/c01-07-mob10-ported.png)

The ported Windows 10 app running on a Mobile device

## <a name="conclusion"></a>결론

이 사례 연구에서는 비현실적으로 간단한 앱을 포팅하는 프로세스에 대해 살펴보았습니다. 예를 들어 목록 상자를 사용하여 선택하거나 탐색할 컨텍스트를 설정할 수 있습니다. 앱에서는 탭되는 항목에 대한 세부 정보가 나오는 페이지를 탐색합니다. 이 특정 앱에서는 사용자 선택과 관련하여 아무 작업도 수행하지 않고 탐색도 하지 않습니다. 하지만 이 사례 연구에서는 분위기를 쇄신하고 포팅 프로세스를 소개하고 실제 UWP 앱에서 사용할 수 있는 중요한 기술을 보여 주었습니다.

또한 포팅한 보기 모델이 일반적으로 매끄러운 프로세스라는 증거도 살펴보았습니다. 사용자 인터페이스 및 폼 팩터 지원은 포팅할 때 주의해야 하는 부분입니다.

다음 사례 연구는 [Bookstore2](w8x-to-uwp-case-study-bookstore2.md)이며, 여기에서는 그룹화된 데이터에 대한 액세스 및 표시에 대해 살펴봅니다.
