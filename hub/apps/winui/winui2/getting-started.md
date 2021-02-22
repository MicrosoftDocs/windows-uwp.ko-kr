---
title: Windows UI 라이브러리 시작
description: Windows UI 라이브러리를 설치하고 사용하는 방법입니다.
ms.topic: article
ms.date: 07/15/2020
keywords: Windows 10, UWP, 도구 키트 SDK
ms.openlocfilehash: 801c1f578c08df627264f542cbe1496d275afc0a
ms.sourcegitcommit: 2b7f6fdb3c393f19a6ad448773126a053b860953
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/12/2021
ms.locfileid: "100334959"
---
# <a name="getting-started-with-the-windows-ui-2x-library"></a>Windows UI 2.x 라이브러리 시작

[WinUI 2.5](release-notes/winui-2.5.md)는 안정적인 최신 버전의 WinUI이며, 프로덕션 환경의 앱에 사용해야 합니다.

라이브러리는 신규 또는 기존 Visual Studio 프로젝트에 추가할 수 있는 NuGet 패키지로 제공됩니다.

> [!NOTE]
> WinUI 3 초기 미리 보기를 사용해 보는 방법에 대한 자세한 내용은 [Windows UI 라이브러리 3 Preview 4(2021년 2월)](../winui3/index.md)를 참조하세요.

## <a name="download-and-install-the-windows-ui-library"></a>Windows UI 라이브러리 다운로드 및 설치

1. [Visual Studio 2019](https://developer.microsoft.com/windows/downloads)를 다운로드하고, Visual Studio 설치 관리자에서 **유니버설 Windows 플랫폼 개발** 워크로드를 선택해야 합니다.

2. 기존 프로젝트를 열거나, Visual C# -> Windows -> 유니버설에서 [빈 앱] 템플릿을 사용하거나 언어 프로젝션에 적절한 템플릿을 사용하여 새 프로젝트를 만듭니다.  

    > [!IMPORTANT]
    > WinUI 2.5를 사용하려면 프로젝트 속성에서 TargetPlatformVersion을 10.0.18362.0 이상으로, TargetPlatformMinVersion을 10.0.15063.0 이상으로 설정해야 합니다.

3. 솔루션 탐색기 창에서 마우스 오른쪽 단추로 프로젝트 이름을 클릭하고, **NuGet 패키지 관리** 를 선택합니다. 

    :::image type="content" source="images/ManageNugetPackages.png" alt-text="프로젝트를 마우스 오른쪽 단추로 클릭하고 NuGet 패키지 관리 옵션이 강조 표시된 솔루션 탐색기 패널의 스크린샷.":::<br/>*프로젝트를 마우스 오른쪽 단추로 클릭하고 NuGet 패키지 관리 옵션이 강조 표시된 솔루션 탐색기 패널*

4. **NuGet 패키지 관리자** 에서 **찾아보기** 탭을 선택하고 **Microsoft.UI.Xaml** 또는 **WinUI** 를 검색합니다. 사용할 [Windows UI 라이브러리 NuGet 패키지](nuget-packages.md)를 선택합니다(**Microsoft.UI.Xaml** 패키지에는 모든 앱에 적합한 Fluent 컨트롤 및 기능이 포함되어 있음). 설치를 클릭합니다. 

    "시험판 포함" 확인란을 선택하면 새로운 실험적 기능이 포함된 최신 시험판 버전을 볼 수 있습니다.

    :::image type="content" source="images/NugetPackages.png" alt-text="검색 필드에 winui를 입력하고 [시험판 포함] 확인란을 선택한 [찾아보기] 탭을 보여주는 NuGet 패키지 관리자 대화 상자의 스크린샷":::<br/>*검색 필드에 winui를 입력하고 [시험판 포함] 확인란을 선택한 [찾아보기] 탭을 보여주는 NuGet 패키지 관리자 대화 상자*

5. WinUI(Windows UI) 테마 리소스를 App.xaml 파일에 추가합니다.

    추가 애플리케이션 리소스가 있는지 여부에 따라 이를 수행하는 두 가지 방법이 있습니다.

    a. 다른 애플리케이션 리소스가 필요 없는 경우 다음 예제와 같이 WinUI 리소스 요소 `<XamlControlsResources`를 추가합니다.

    ``` XAML
    <Application
        x:Class="ExampleApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        RequestedTheme="Light">

        <Application.Resources>
            <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
        </Application.Resources>

    </Application>
    ```

    b. 애플리케이션 리소스가 여러 개 필요한 경우 다음과 같이 `<ResourceDictionary.MergedDictionaries>`에서 WinUI resources 요소 `<XamlControlsResources`를 추가합니다.

    ``` XAML
    <Application
        x:Class="ExampleApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        RequestedTheme="Light">

        <Application.Resources>
            <ResourceDictionary>
                <ResourceDictionary.MergedDictionaries>
                    <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
                    <ResourceDictionary Source="/Styles/Styles.xaml"/>
                </ResourceDictionary.MergedDictionaries>
            </ResourceDictionary>
        </Application.Resources>

    </Application>
    ```

    > [!IMPORTANT]
    > ResourceDictionary에 추가되는 리소스의 순서는 적용되는 순서에 영향을 줍니다. `XamlControlsResources` 사전은 많은 기본 리소스 키를 재정의하므로 앱의 다른 사용자 지정 스타일 또는 리소스를 재정의하지 않도록 먼저 `Application.Resources`에 추가해야 합니다. 리소스를 로드하는 방법에 대한 자세한 내용은 [ResourceDictionary 및 XAML 리소스 참조](/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references)를 참조하세요.

6. WinUI 패키지에 대한 참조를 XAML 페이지 및/또는 코드 숨김 페이지에 추가합니다.

    * XAML 페이지에서 참조를 페이지 위쪽에 추가합니다.

        ```xaml
        xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
        ```

    * 코드에서(형식 이름을 한정하지 않고 사용하려는 경우) using 지시문을 추가할 수 있습니다.

        ```csharp
        using MUXC = Microsoft.UI.Xaml.Controls;
        ```

## <a name="additional-steps-for-a-cwinrt-project"></a>C++/WinRT 프로젝트에 대한 추가 단계

NuGet 패키지를 C++/WinRT 프로젝트에 추가하면 도구에서 일단의 프로젝션 헤더를 프로젝트의 `\Generated Files\winrt` 폴더에 생성합니다. 이러한 헤더 파일을 프로젝트로 가져와서 새 형식에 대한 참조가 확인되도록 하려면 미리 컴파일된 헤더 파일(일반적으로 `pch.h`)로 이동하여 포함시킬 수 있습니다. 다음은 **Microsoft.UI.Xaml** 패키지에 대해 생성된 헤더 파일이 포함된 예제입니다.

```cppwinrt
// pch.h
...
#include "winrt/Microsoft.UI.Xaml.Automation.Peers.h"
#include "winrt/Microsoft.UI.Xaml.Controls.Primitives.h"
#include "winrt/Microsoft.UI.Xaml.Media.h"
#include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"
...
```

Windows UI 라이브러리에 대한 간단한 지원을 C++/WinRT 프로젝트에 추가하는 방법에 대한 전체 단계별 연습은 [간단한 C++/WinRT Windows UI 라이브러리 예제](/windows/uwp/cpp-and-winrt-apis/simple-winui-example)를 참조하세요.

## <a name="contributing-to-the-windows-ui-library"></a>Windows UI 라이브러리에 기여

WinUI는 GitHub에 호스팅되는 오픈 소스 프로젝트입니다.

[Windows UI 라이브러리 리포지토리](https://aka.ms/winui)에서 버그 보고서를 제출하고, 기능을 요청하고, 커뮤니티 코드를 제공할 수 있습니다.

## <a name="other-resources"></a>다른 리소스

UWP를 처음 사용하는 경우 개발자 포털의 [UWP 개발 시작](https://developer.microsoft.com/windows/getstarted) 페이지를 방문하는 것이 좋습니다.