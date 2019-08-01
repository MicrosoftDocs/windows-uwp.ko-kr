---
description: C++/WinRT 프로젝트 내에서 WinUI를 위해 간단한 지원을 추가하는 과정을 안내합니다.
title: 간단한 C++/WinRT Windows UI 라이브러리 예제
ms.date: 07/12/2019
ms.topic: article
keywords: Windows 10, UWP, 표준, C++, cpp, WinRT, Windows UI 라이브러리, WinUI
ms.localizationpriority: medium
ms.openlocfilehash: 5d0066abb2a6eb15f1d31aaf930ed2c0f0faf81a
ms.sourcegitcommit: 4e74c920f1fef507c5cdf874975003702d37bcbb
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68372713"
---
# <a name="a-simple-cwinrt-windows-ui-library-example"></a>간단한 C++/WinRT Windows UI 라이브러리 예제

이 항목에서는 간단한 [WinUI(Windows UI) 라이브내러리](https://github.com/Microsoft/microsoft-ui-xaml) 지원을 C++/WinRT 프로젝트에 추가하는 과정을 안내합니다. 덧붙여 말해, Windows UI 라이브러리 자체는 C++/WinRT로 작성됩니다.

> [!NOTE]
> 이 항목에서 알 수 있듯이 WinUI(Windows UI) 라이브러리 도구 키트는 Visual Studio를 사용하여 기존 또는 새 프로젝트에 추가할 수 있는 NuGet 패키지로 사용할 수 있습니다. 자세한 배경, 설정 및 지원 정보는 [Windows UI 라이브러리 시작](/uwp/toolkits/winui/getting-started)을 참조하세요.

## <a name="create-a-blank-app-hellowinuicppwinrt"></a>비어 있는 앱(HelloWinUICppWinRT) 만들기

Visual Studio에서 **비어 있는 앱(C++/WinRT)** 프로젝트 템플릿을 사용하여 새 프로젝트를 만들고, 이름을 *HelloWinUICppWinRT*로 지정합니다.

## <a name="install-the-microsoftuixaml-nuget-package"></a>Microsoft.UI.Xaml NuGet 패키지 설치

**프로젝트** \> **NuGet 패키지 관리...** \> **찾아보기**를 차례로 클릭하고, 검색 상자에서 **Microsoft.UI.Xaml**을 입력하거나 붙여넣고, 검색 결과에서 해당 항목을 선택한 다음, **설치**를 클릭하여 패키지를 프로젝트에 설치합니다(사용권 계약 프롬프트도 표시됨). **Microsoft.UI.Xaml.Core.Direct**가 아니라 **Microsoft.UI.Xaml** 패키지만 설치하도록 주의하세요.

## <a name="declare-winui-application-resources"></a>WinUI 애플리케이션 리소스 선언

`App.xaml`을 열고, 기존의 열고 닫는 **Application** 태그 사이에 다음 태그를 붙여넣습니다.

```xaml
<Application.Resources>
    <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>
</Application.Resources>
```

## <a name="add-a-winui-control-to-mainpage"></a>MainPage에 WinUI 컨트롤 추가

다음으로 `MainPage.xaml`을 엽니다. 기존의 여는 **Application** 태그에는 몇 가지 xml 네임스페이스 선언이 있습니다. `xmlns:muxc="using:Microsoft.UI.Xaml.Controls"` xml 네임스페이스 선언을 추가합니다. 그런 다음, 기존의 열고 닫는 **Page** 태그 사이에서 기존 **StackPanel** 요소를 덮어쓰면서 다음 태그를 붙여넣습니다.

```xaml
<muxc:NavigationView PaneTitle="Welcome">
    <TextBlock Text="Hello, World!" VerticalAlignment="Center" HorizontalAlignment="Center" Style="{StaticResource TitleTextBlockStyle}"/>
</muxc:NavigationView>
```

## <a name="edit-mainpageh-and-cpp-as-necessary"></a>필요에 따라 MainPage.h 및 .cpp를 편집합니다.

NuGet 패키지(예: 이전에 추가한 **Microsoft.UI.Xaml** 패키지)를 C++/WinRT 프로젝트에 추가하면 도구에서 일단의 프로젝션 헤더를 프로젝트의 `\Generated Files\winrt` 폴더에 생성합니다. 이러한 헤더 파일을 프로젝트에 가져와서 이러한 새 형식의 참조가 해결되도록 하려면 해당 헤더 파일이 포함되어야 합니다.

따라서 `MainPage.h`에서 include를 아래 목록과 같이 편집합니다. 둘 이상의 XAML 페이지에서 WinUI를 사용하는 경우 미리 컴파일된 헤더 파일(일반적으로 `pch.h`)로 이동하여 대신 포함시킬 수 있습니다.

```cppwinrt
#include "MainPage.g.h"
#include "winrt/Microsoft.UI.Xaml.Controls.h"
#include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"
```

마지막으로, *myButton*이 더 이상 XAML 태그에 없으므로 `MainPage.cpp`에서 **MainPage::ClickHandler** 구현 내의 코드를 삭제합니다.

이제 프로젝트를 빌드하고 실행할 수 있습니다.

![간단한 C++/WinRT Windows UI 라이브러리의 스크린샷](images/winui.png)

## <a name="related-topics"></a>관련 항목
* [Windows UI 라이브러리 시작](/uwp/toolkits/winui/getting-started)