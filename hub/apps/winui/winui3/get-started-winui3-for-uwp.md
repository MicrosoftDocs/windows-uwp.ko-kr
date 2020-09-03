---
description: 이 가이드에서는 WinUI 3 UI를 사용하여 UWP 앱 만들기를 시작하는 방법을 보여 줍니다.
title: UWP 앱용 WinUI 3 시작
ms.date: 07/13/2020
ms.topic: article
keywords: Windows 10, UWP, WinUI
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 88b17500527b5f52d7e020e1c37a72e932ec225b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157737"
---
# <a name="get-started-with-winui-3-for-uwp-apps"></a>UWP 앱용 WinUI 3 시작

WinUI 3 Preview 2에는 WinUI에서 완전하게 빌드된 사용자 인터페이스를 사용하여 UWP(유니버설 Windows 플랫폼) 앱을 만들 수 있는 새 프로젝트 템플릿이 도입되었습니다. 이러한 프로젝트 템플릿을 사용하여 앱을 만들면 애플리케이션의 전체 사용자 인터페이스가 WinUI 3에서 제공하는 창, 컨트롤 및 스타일을 사용하여 구현됩니다. 지원되는 WinUI 3 프로젝트 템플릿의 전체 목록은 [WinUI 3용 프로젝트 템플릿](index.md#project-templates-for-winui-3)을 참조하세요.

## <a name="prerequisites"></a>필수 구성 요소

이 문서에서 설명하는 UWP 프로젝트 템플릿용 WinUI 3을 사용하려면 개발 컴퓨터를 구성하고 [WinUI 3 Preview 2를 설치](index.md#install-winui-3-preview-2)합니다.

## <a name="create-a-winui-3-app-in-uwp-for-c"></a>C#용 "UWP의 WinUI 3 앱" 만들기

1. Visual Studio 2019를 사용하여 새 프로젝트를 만듭니다.
   - Visual Studio가 이미 실행되고 있으면 **파일** -> **새로 만들기** -> **프로젝트**를 차례로 선택합니다.

   :::image type="content" source="images/WinUI-and-UWP/vs2019-menu-file-new-project.png" alt-text="Visual Studio 2019 - 파일 -> 새로 만들기 -> 프로젝트 메뉴":::

   - 그렇지 않으면 Visual Studio를 시작하고, **새 프로젝트 만들기**를 선택합니다.

   :::image type="content" source="images/WinUI-and-UWP/vs2019-splash-new-project.png" alt-text="Visual Studio 2019 - 새 프로젝트 만들기":::

2. **새 프로젝트 만들기** 대화 상자의 프로젝트 드롭다운 필터에서 **C#** , **Windows** 및 **WinUI**를 각각 선택합니다.

3. **빈 앱(UWP의 WinUI)** 프로젝트 형식을 선택하고, **다음**을 클릭합니다.

:::image type="content" source="images/WinUI-and-UWP/vs2019-create-new-project-dialog.png" alt-text="Visual Studio 2019 - 새 프로젝트 만들기 대화 상자":::

4. 프로젝트 이름을 입력하고 원하는 대로 다른 옵션을 선택한 다음, **만들기**를 클릭합니다.

:::image type="content" source="images/WinUI-and-UWP/vs2019-configure-new-project-dialog.png" alt-text="Visual Studio 2019 - 새 프로젝트 구성 대화 상자":::

5. 다음 대화 상자에서 **대상 버전**을 Windows 10, 버전 1903(빌드 18362)으로 설정하고 **최소 버전**을 Windows 10, 버전 1803(빌드 17134)으로 설정한 다음, **확인**을 클릭합니다.

:::image type="content" source="images/WinUI-min-target-version.png" alt-text="대상 버전 및 최소 버전 대화 상자":::

6. Visual Studio에서 다음 개체를 사용하여 **UWP의 WinUI** 프로젝트를 생성합니다.

    - ***프로젝트 이름*(유니버설 Windows)** : 애플리케이션 코드를 포함합니다. 프로젝트 솔루션에 대한 기본 시작 프로젝트입니다.

    :::image type="content" source="images/WinUI-and-UWP/vs2019-project.png" alt-text="Visual Studio 2019 - 새 프로젝트 구성 대화 상자":::

    - **Package.appxmanifest**: 시스템에서 앱을 배포, 표시 또는 업데이트하는 데 필요한 정보를 포함합니다. 자세한 내용은 [앱 패키지 매니페스트](/uwp/schemas/appxpackage/appx-package-manifest)를 참조하세요.

    :::image type="content" source="images/WinUI-and-UWP/vs2019-file-package-manifest.png" alt-text="Visual Studio 2019 - 앱 패키지 매니페스트":::

    - **App.xaml/App.xaml.cs**: 앱 인스턴스를 나타내는 `Application` 클래스를 정의하는 코드 파일입니다.

    :::image type="content" source="images/WinUI-and-UWP/vs2019-file-app-xaml.png" alt-text="Visual Studio 2019 - App.xaml 파일":::

    :::image type="content" source="images/WinUI-and-UWP/vs2019-file-app-xaml-cs.png" alt-text="Visual Studio 2019 - App.xaml.cs 파일":::

    - **MainPage.xaml/MainPage.xaml.cs**: 앱에서 표시하는 기본 창을 나타내는 코드 파일입니다. 이러한 클래스는 WinUI에서 제공하는 **Microsoft.UI.Xaml** 네임스페이스의 형식에서 파생됩니다.

    :::image type="content" source="images/WinUI-and-UWP/vs2019-file-mainpage-xaml.png" alt-text="Visual Studio 2019 - MainPage.xaml 파일":::

    :::image type="content" source="images/WinUI-and-UWP/vs2019-file-mainpage-xaml-cs.png" alt-text="Visual Studio 2019 - MainPage.xaml.cs 파일":::

7. 새 항목을 앱 프로젝트에 추가하려면 **솔루션 탐색기**에서 마우스 오른쪽 단추로 ***프로젝트 이름*(유니버설 Windows)** 프로젝트 노드를 클릭하고, **추가** -> **새 항목**을 차례로 선택합니다. **새 항목 추가** 대화 상자에서 **WinUI** 탭을 선택하고, 추가하려는 항목을 선택한 다음, **추가**를 클릭합니다. 사용 가능한 항목에 대한 자세한 내용은 [WinUI 3용 항목 템플릿](index.md#item-templates-for-winui-3)을 참조하세요.

    :::image type="content" source="images/WinUI-and-UWP/vs2019-add-new-item-dialog.png" alt-text="Visual Studio 2019 - 새 항목 추가 대화 상자":::

8. 앱을 빌드, 배포 및 실행하여 앱이 표시되는 모양을 확인합니다.

    1. 로컬 컴퓨터, 시뮬레이터, 에뮬레이터 또는 원격 디바이스에서 앱을 디버그할 수 있습니다. 드롭다운에서 대상 디바이스를 선택합니다.

        :::image type="content" source="images/WinUI-and-UWP/vs2019-menu-target-device.png" alt-text="Visual Studio 2019 - 대상 디바이스 메뉴":::

    1. F5 키를 누르거나, **빌드** 단추를 클릭하거나, **디버그 -> 디버깅 시작**을 차례로 선택하여 솔루션을 빌드 및 실행하고, 앱이 오류 없이 실행되는지 확인합니다.

        :::image type="content" source="images/WinUI-and-UWP/vs2019-project-running.png" alt-text="Visual Studio 2019 - 대상 디바이스 메뉴":::

## <a name="known-issues-and-limitations"></a>알려진 문제 및 제한 사항

알려진 문제 및 제한 사항에 대한 목록은 [이 섹션](index.md#preview-2-limitations-and-known-issues)을 참조하세요.

## <a name="related-topics"></a>관련 항목

- [WinUI 3](index.md)
- [첫 번째 앱 만들기](/windows/uwp/get-started/your-first-app)