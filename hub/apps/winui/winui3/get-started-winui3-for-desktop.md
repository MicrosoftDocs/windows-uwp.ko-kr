---
description: 이 가이드에서는 WinUI 3 UI를 사용하여 .NET 및 C++/Win32 데스크톱 앱 만들기를 시작하는 방법을 보여줍니다.
title: 데스크톱 앱용 WinUI 3 시작
ms.date: 05/19/2020
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, xaml island
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 164ae035d3b9dda24137bcb09dd208e718db0319
ms.sourcegitcommit: 53c00939b20d4b0a294936df3d395adb0c13e231
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91933104"
---
# <a name="get-started-with-winui-3-for-desktop-apps"></a>데스크톱 앱용 WinUI 3 시작

WinUI 3 Preview 2에는 완전한 WinUI 기반 사용자 인터페이스를 사용하여 관리형 데스크톱 C#/.NET Core 및 네이티브 C++/Win32 데스크톱 앱을 만들 수 있도록 하는 새로운 프로젝트 템플릿이 도입되었습니다. 이러한 프로젝트 템플릿을 사용하여 앱을 만들면 애플리케이션의 전체 사용자 인터페이스가 WinUI 3에서 제공하는 창, 컨트롤 및 기타 UI 유형을 사용하여 구현됩니다. 프로젝트 템플릿의 전체 목록은 [이 섹션](index.md#project-templates-for-winui-3)을 참조하세요.

## <a name="prerequisites"></a>필수 구성 요소

이 문서에서 설명하는 데스크톱 프로젝트 템플릿에 WinUI 3을 사용하려면 개발 컴퓨터를 구성하고 [여기](index.md#install-winui-3-preview-2)의 지침에 따라 WinUI 3 Preview 2를 설치합니다.

## <a name="create-a-winui-3-desktop-app-for-c-and-net-5"></a>C# 및 .NET 5용 WinUI 3 데스크톱 앱 만들기

1. Visual Studio 2019에서 **파일** -> **새로 만들기** -> **프로젝트**를 선택합니다.

2. 각각 프로젝트 드롭다운 필터에서 **C#** , **Windows**, **WinUI**를 선택합니다.

3. 프로젝트 유형으로 **비어 있는 앱, 패키지됨(데스크톱의 WinUI)** 을 선택하고 **다음**을 클릭합니다.

    ![비어 있는 앱, 패키지됨(데스크톱의 WinUI) 옵션이 강조 표시된 새 프로젝트 만들기 마법사의 스크린샷](images/WinUI-csharp-newproject.png)

4. 프로젝트 이름을 입력하고 원하는 대로 다른 옵션을 선택한 다음, **만들기**를 클릭합니다.

5. 다음 대화 상자에서 **대상 버전**을 Windows 10, 버전 1903(빌드 18362)으로 설정하고 **최소 버전**을 Windows 10, 버전 1803(빌드 17134)으로 설정한 다음, **확인**을 클릭합니다.

    ![대상 버전 및 최소 버전](images/WinUI-min-target-version.png)

6. 이제 Visual Studio는 다음과 같은 두 개의 프로젝트를 생성합니다.

    * ***프로젝트 이름*(데스크톱)** : 이 프로젝트에는 앱의 코드가 포함됩니다. **App.xaml.cs** 코드 파일은 앱 인스턴스를 나타내는 `Application` 클래스를 정의하고, **MainWindow.xaml.cs** 코드 파일은 앱에서 표시하는 주 창을 나타내는 `MainWindow` 클래스를 정의합니다. 이러한 클래스는 WinUI에서 제공하는 **Microsoft.UI.Xaml** 네임스페이스의 형식에서 파생됩니다.

        ![솔루션 탐색기 창 및 기본 Windows XAML .CS 파일의 내용을 보여주는 Visual Studio의 스크린샷](images/WinUI-csharp-appproject.png)

    * ***프로젝트 이름*(패키지)** : 앱을 [MSIX 패키지](/windows/msix/overview)로 빌드하도록 구성된 [Windows 애플리케이션 패키징 프로젝트](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)입니다. 이는 최신 배포 환경, 패키지 확장을 통해 Windows 10 기능과 통합하는 기능 등을 제공합니다. 이 프로젝트는 앱의 [패키지 매니페스트](/uwp/schemas/appxpackage/uapmanifestschema/schema-root)를 포함하고 있으며, 기본적으로 솔루션의 시작 프로젝트입니다.

        ![솔루션 탐색기 창 및 패키지 앱 x 매니페스트 파일의 내용을 보여주는 Visual Studio의 스크린샷](images/WinUI-csharp-packageproject.png)

7. 앱 프로젝트에 새 항목을 추가하려면 **솔루션 탐색기**에서 ***프로젝트 이름*(데스크톱)** 프로젝트 노드를 마우스 오른쪽 단추로 클릭하고 **추가** -> **새 항목**을 선택합니다. **새 항목 추가** 대화 상자에서 **WinUI** 탭을 선택하고, 추가하려는 항목을 선택한 다음, **추가**를 클릭합니다. 사용 가능한 항목에 대한 자세한 내용은 [이 섹션](index.md#item-templates-for-winui-3)을 참조하세요.

    ![설치됨 > Visual C# 항목 > WinUI가 선택되고 빈 페이지 옵션이 강조 표시된 새 항목 추가 대화 상자의 스크린샷](images/WinUI-csharp-newitem.png)

8. 솔루션을 빌드하고 실행하여 앱이 오류 없이 실행되는지 확인합니다.

## <a name="create-a-winui-3-desktop-app-for-cwin32"></a>C++/Win32용 WinUI 3 데스크톱 앱 만들기

1. Visual Studio 2019에서 **파일** -> **새로 만들기** -> **프로젝트**를 선택합니다.

2. 각각 프로젝트 드롭다운 필터에서 **C++** , **Windows**, **WinUI**를 선택합니다.

3. 프로젝트 유형으로 **비어 있는 앱, 패키지됨(데스크톱의 WinUI)** 을 선택하고 **다음**을 클릭합니다.

    ![비어 있는 앱, 패키지됨(데스크톱의 WinUI) 옵션이 강조 표시된 새 프로젝트 만들기 마법사의 또 다른 스크린샷](images/WinUI-cpp-newproject.png)

4. 프로젝트 이름을 입력하고 원하는 대로 다른 옵션을 선택한 다음, **만들기**를 클릭합니다.

5. 다음 대화 상자에서 **대상 버전**을 Windows 10, 버전 1903(빌드 18362)으로 설정하고 **최소 버전**을 Windows 10, 버전 1803(빌드 17134)으로 설정한 다음, **확인**을 클릭합니다.

    ![대상 버전 및 최소 버전](images/WinUI-min-target-version.png)

6. 이제 Visual Studio는 다음과 같은 두 개의 프로젝트를 생성합니다.

    * ***프로젝트 이름*(데스크톱)** : 이 프로젝트에는 앱의 코드가 포함됩니다. **App.xaml** 및 다양한 **앱** 코드 파일은 앱 인스턴스를 나타내는 `Application` 클래스를 정의하고, **MainWindow.xaml** 및 다양한 **MainWindow** 코드 파일은 앱에서 표시하는 주 창을 나타내는 `MainWindow` 클래스를 정의합니다. 이러한 클래스는 WinUI에서 제공하는 **Microsoft.UI.Xaml** 네임스페이스의 형식에서 파생됩니다.

        ![솔루션 탐색기 창 및 기본 Windows XAML 파일의 내용을 보여주는 Visual Studio의 스크린샷](images/WinUI-cpp-appproject.png)

    * ***프로젝트 이름*(패키지)** : 앱을 [MSIX 패키지](/windows/msix/overview)로 빌드하도록 구성된 [Windows 애플리케이션 패키징 프로젝트](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)입니다. 이는 최신 배포 환경, 패키지 확장을 통해 Windows 10 기능과 통합하는 기능 등을 제공합니다. 이 프로젝트는 앱의 [패키지 매니페스트](/uwp/schemas/appxpackage/uapmanifestschema/schema-root)를 포함하고 있으며, 기본적으로 솔루션의 시작 프로젝트입니다.

        ![솔루션 탐색기 창 및 패키지 앱 x 매니페스트 파일의 내용을 보여주는 Visual Studio의 또 다른 스크린샷](images/WinUI-cpp-packageproject.png)

7. 앱 프로젝트에 새 항목을 추가하려면 **솔루션 탐색기**에서 ***프로젝트 이름*(데스크톱)** 프로젝트 노드를 마우스 오른쪽 단추로 클릭하고 **추가** -> **새 항목**을 선택합니다. **새 항목 추가** 대화 상자에서 **WinUI** 탭을 선택하고, 추가하려는 항목을 선택한 다음, **추가**를 클릭합니다. 사용 가능한 항목에 대한 자세한 내용은 [이 섹션](index.md#item-templates-for-winui-3)을 참조하세요.

    ![새 항목](images/WinUI-cpp-newitem.png)

8. 솔루션을 빌드하고 실행하여 앱이 오류 없이 실행되는지 확인합니다.

## <a name="known-issues-and-limitations"></a>알려진 문제 및 제한 사항

알려진 문제 및 제한 사항에 대한 목록은 [이 섹션](index.md#preview-2-limitations-and-known-issues)을 참조하세요.

## <a name="related-topics"></a>관련 항목

* [Windows UI 라이브러리 3](index.md)