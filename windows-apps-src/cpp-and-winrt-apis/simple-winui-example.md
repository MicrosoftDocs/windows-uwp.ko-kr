---
description: 이 토픽에서는 C++/WinRT 프로젝트 내에서 WinUI에 대한 간단한 지원을 추가하는 과정을 안내합니다.
title: 간단한 C++/WinRT Windows UI 라이브러리 예제
ms.date: 07/12/2019
ms.topic: article
keywords: Windows 10, UWP, 표준, C++, cpp, WinRT, Windows UI 라이브러리, WinUI
ms.localizationpriority: medium
ms.openlocfilehash: b7323ba63bedfc1ed560effb4da8ea61d1527897
ms.sourcegitcommit: 71701f5ffc540951f86d6f77a52416c6d75fe305
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100632684"
---
# <a name="a-basic-cwinrt-windows-ui-library-example"></a>기본 C++/WinRT Windows UI 라이브러리 예제

이 토픽에서는 C++/WinRT 프로젝트에 [WinUI(Windows UI) 라이브러리](https://github.com/Microsoft/microsoft-ui-xaml)에 대한 기본 지원을 추가하는 과정을 안내합니다. 덧붙여 말해, Windows UI 라이브러리 자체는 C++/WinRT로 작성됩니다.

> [!NOTE]
> 이 항목에서 알 수 있듯이 WinUI(Windows UI) 라이브러리 도구 키트는 Visual Studio를 사용하여 기존 또는 새 프로젝트에 추가할 수 있는 NuGet 패키지로 사용할 수 있습니다. 자세한 배경, 설정 및 지원 정보는 [Windows UI 라이브러리 시작](/uwp/toolkits/winui/getting-started)을 참조하세요.

## <a name="create-a-blank-app-hellowinuicppwinrt"></a>비어 있는 앱(HelloWinUICppWinRT) 만들기

Visual Studio에서 **비어 있는 앱(C++/WinRT)** 프로젝트 템플릿을 사용하여 새 프로젝트를 만듭니다. **(유니버설 Windows)** 템플릿이 아닌 **(C++/WinRT)** 템플릿을 사용하고 있는지 확인합니다.

새 프로젝트의 이름을 *HelloWinUICppWinRT* 로 설정하고 폴더 구조가 연습과 일치하도록 **솔루션 및 프로젝트를 같은 디렉터리에 배치** 를 선택 취소합니다.

## <a name="install-the-microsoftuixaml-nuget-package"></a>Microsoft.UI.Xaml NuGet 패키지 설치

**프로젝트** \> **NuGet 패키지 관리...** 를 클릭합니다. \> **찾아보기** 를 차례로 클릭하고, 검색 상자에서 **Microsoft.UI.Xaml** 을 입력하거나 붙여넣고, 검색 결과에서 해당 항목을 선택한 다음, **설치** 를 클릭하여 패키지를 프로젝트에 설치합니다(사용권 계약 프롬프트도 표시됨). **Microsoft.UI.Xaml.Core.Direct** 가 아니라 **Microsoft.UI.Xaml** 패키지만 설치하도록 주의하세요.

## <a name="declare-winui-application-resources"></a>WinUI 애플리케이션 리소스 선언

`App.xaml`을 열고, 기존의 열고 닫는 **Application** 태그 사이에 다음 태그를 붙여넣습니다.

```xaml
<Application.Resources>
    <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>
</Application.Resources>
```

## <a name="add-a-winui-control-to-mainpage"></a>MainPage에 WinUI 컨트롤 추가

다음으로 `MainPage.xaml`을 엽니다. 기존의 여는 **Page** 태그에는 몇 가지 xml 네임스페이스 선언이 있습니다. `xmlns:muxc="using:Microsoft.UI.Xaml.Controls"` xml 네임스페이스 선언을 추가합니다. 그런 다음, 기존의 열고 닫는 **Page** 태그 사이에서 기존 **StackPanel** 요소를 덮어쓰면서 다음 태그를 붙여넣습니다.

```xaml
<muxc:NavigationView PaneTitle="Welcome">
    <TextBlock Text="Hello, World!" VerticalAlignment="Center" HorizontalAlignment="Center" Style="{StaticResource TitleTextBlockStyle}"/>
</muxc:NavigationView>
```

## <a name="edit-pchh-as-necessary"></a>필요에 따라 pch.h 편집

NuGet 패키지(예: 이전에 추가한 **Microsoft.UI.Xaml** 패키지)를 C++/WinRT 프로젝트에 추가하고 프로젝트를 빌드하면 도구에서 일단의 프로젝션 헤더 파일을 프로젝트의 `\Generated Files\winrt` 폴더에 생성합니다. 연습을 수행하고 나면 이제 `\HelloWinUICppWinRT\HelloWinUICppWinRT\Generated Files\winrt` 폴더가 생깁니다. 이러한 헤더 파일을 프로젝트로 가져와서 새 형식에 대한 참조가 확인되도록 하려면 미리 컴파일된 헤더 파일(일반적으로 `pch.h`)로 이동하여 포함시킬 수 있습니다.

사용하는 형식에 해당하는 헤더만 포함해야 합니다. 하지만, 여기에 있는 것은 **Microsoft.UI.Xaml** 패키지에 대해 생성된 모든 헤더 파일이 포함된 예제입니다.

```cppwinrt
// pch.h
...
#include "winrt/Microsoft.UI.Xaml.Automation.Peers.h"
#include "winrt/Microsoft.UI.Xaml.Controls.h"
#include "winrt/Microsoft.UI.Xaml.Controls.Primitives.h"
#include "winrt/Microsoft.UI.Xaml.Media.h"
#include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"
...
```

## <a name="edit-mainpagecpp"></a>MainPage.cpp 편집

*myButton* 이 더 이상 XAML 태그에 없으므로 `MainPage.cpp`에서 **MainPage::ClickHandler** 구현 내의 코드를 삭제합니다.

이제 프로젝트를 빌드하고 실행할 수 있습니다.

![간단한 C++/WinRT Windows UI 라이브러리의 스크린샷](images/winui.png)

## <a name="related-topics"></a>관련 항목
* [Windows UI 라이브러리 시작](/uwp/toolkits/winui/getting-started)