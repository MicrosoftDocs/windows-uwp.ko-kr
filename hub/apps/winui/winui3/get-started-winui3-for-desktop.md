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
ms.openlocfilehash: ab67507153e0ff7065baffa92ea6ec35aee5b132
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580770"
---
# <a name="get-started-with-winui-30-for-desktop-apps"></a>데스크톱 앱용 WinUI 3.0 시작

WinUI 3.0 Preview 1에는 전적으로 WinUI 기반 사용자 인터페이스를 사용하여 관리형 데스크톱 C#/.NET 및 기본 C++/Win32 데스크톱 앱을 만들 수 있는 새로운 프로젝트 템플릿이 도입되었습니다. 이 프로젝트 템플릿을 사용하여 앱을 만들면 애플리케이션의 사용자 인터페이스 전체가 창, 컨트롤 및 WinUI 3.0에서 제공하는 기타 유형의 UI를 사용하여 구현됩니다. 

WinUI 3.0 Preview 1에는 Visual Studio 2019의 다음과 같은 **데스크톱의 WinUI**가 추가되었습니다.

* .NET 5를 대상으로 하는 C# 앱 및 라이브러리:
  * 비어 있는 앱, 패키지됨(데스크톱의 WinUI)
  * 클래스 라이브러리(데스크톱의 WinUI)

* C++/Win32 앱:
  * 비어 있는 앱, 패키지됨(데스크톱의 WinUI)

앱 프로젝트 템플릿은 앱을 배포용 [MSIX 패키지](https://docs.microsoft.com/windows/msix/overview)로 빌드하도록 구성되는 WinUI 앱 프로젝트 및 [Windows 애플리케이션 패키징 프로젝트](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)를 생성합니다.

## <a name="prerequisites"></a>필수 구성 요소

이 문서에서 설명하는 데스크톱 프로젝트 템플릿에 WinUI 3을 사용하려면 다음 지침에 따라 개발용 컴퓨터를 구성하세요.

1. 개발용 컴퓨터에 Windows 10, 버전 1803(빌드 17134) 이상이 설치되어 있는지 확인합니다. 데스크톱 앱용 WinUI 3을 사용하려면 OS 버전 1803 이상이 필요합니다.

2. Visual Studio 2019, 버전 16.7 Preview 1을 설치합니다. 자세한 내용은 [다음 지침](index.md#configure-your-dev-environment)을 참조하세요.

3. .NET 5 Preview 4의 x64 및 x86 버전을 모두 설치합니다.
    * x64: [https://aka.ms/dotnet/net5/preview4/Sdk/dotnet-sdk-win-x64.exe](https://aka.ms/dotnet/net5/preview4/Sdk/dotnet-sdk-win-x64.exe)
    * x86: [https://aka.ms/dotnet/net5/preview4/Sdk/dotnet-sdk-win-x86.exe](https://aka.ms/dotnet/net5/preview4/Sdk/dotnet-sdk-win-x86.exe)

4. Visual Studio 2019용 WinUI 3.0 Preview 1 프로젝트 템플릿이 들어 있는 VSIX 확장을 설치합니다. 자세한 내용은 [다음 지침](index.md#visual-studio-project-templates)을 참조하세요.

## <a name="create-a-winui-3-desktop-app-for-c-and-net-5"></a>C# 및 .NET 5용 WinUI 3 데스크톱 앱 만들기

1. Visual Studio 2019에서 **파일** -> **새로 만들기** -> **프로젝트**를 선택합니다.

2. 각각 프로젝트 드롭다운 필터에서 **C#** , **Windows**, **WinUI**를 선택합니다.

3. 프로젝트 유형으로 **비어 있는 앱, 패키지됨(데스크톱의 WinUI)** 을 선택하고 **다음**을 클릭합니다.

    ![비어 있는 앱 프로젝트 템플릿](images/WinUI-csharp-newproject.png)

4. 프로젝트 이름을 입력하고 원하는 대로 다른 옵션을 선택한 다음, **만들기**를 클릭합니다.

5. 다음 대화 상자에서 **대상 버전**을 Windows 10, 버전 1903(빌드 18362)으로 설정하고 **최소 버전**을 Windows 10, 버전 1803(빌드 17134)으로 설정한 다음, **확인**을 클릭합니다.

    ![대상 버전 및 최소 버전](images/WinUI-min-target-version.png)

6. 이제 Visual Studio는 다음과 같은 두 개의 프로젝트를 생성합니다.

    * ***프로젝트 이름*(데스크톱)** : 이 프로젝트에는 앱의 코드가 포함됩니다. **App.xaml.cs** 코드 파일은 앱 인스턴스를 나타내는 `Application` 클래스를 정의하고, **MainWindow.xaml.cs** 코드 파일은 앱에서 표시하는 주 창을 나타내는 `MainWindow` 클래스를 정의합니다. 이러한 클래스는 WinUI에서 제공하는 **Microsoft.UI.Xaml** 네임스페이스의 형식에서 파생됩니다.

        ![앱 프로젝트](images/WinUI-csharp-appproject.png)

    * ***프로젝트 이름*(패키지)** : 앱 프로젝트 템플릿은 앱을 배포용 MSIX 패키지로 빌드하도록 구성되는 [Windows 애플리케이션 패키징 프로젝트](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)입니다. 이 프로젝트는 앱의 [패키지 매니페스트](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root)를 포함하고 있으며, 기본적으로 솔루션의 시작 프로젝트입니다.

        ![앱 프로젝트](images/WinUI-csharp-packageproject.png)

7. 앱 프로젝트에 새 항목을 추가하려면 **솔루션 탐색기**에서 ***프로젝트 이름*(데스크톱)** 프로젝트 노드를 마우스 오른쪽 단추로 클릭하고 **추가** -> **새 항목**을 선택합니다. **새 항목 추가** 대화 상자에서 **WinUI** 탭을 선택하고, 추가하려는 항목을 선택한 다음, **추가**를 클릭합니다. 다음과 같은 유형의 항목 중에서 선택할 수 있습니다.

    * **빈 페이지**
    * **빈 창**
    * **사용자 지정 컨트롤**
    * **리소스 사전**
    * **리소스 파일**
    * **사용자 컨트롤**

    ![새 항목](images/WinUI-csharp-newitem.png)

8. 솔루션을 빌드하고 실행하여 앱이 오류 없이 실행되는지 확인합니다.

## <a name="create-a-winui-3-desktop-app-for-cwin32"></a>C++/Win32용 WinUI 3 데스크톱 앱 만들기

1. Visual Studio 2019에서 **파일** -> **새로 만들기** -> **프로젝트**를 선택합니다.

2. 각각 프로젝트 드롭다운 필터에서 **C++** , **Windows**, **WinUI**를 선택합니다.

3. 프로젝트 유형으로 **비어 있는 앱, 패키지됨(데스크톱의 WinUI)** 을 선택하고 **다음**을 클릭합니다.

    ![비어 있는 앱 프로젝트 템플릿](images/WinUI-cpp-newproject.png)

4. 프로젝트 이름을 입력하고 원하는 대로 다른 옵션을 선택한 다음, **만들기**를 클릭합니다.

5. 다음 대화 상자에서 **대상 버전**을 Windows 10, 버전 1903(빌드 18362)으로 설정하고 **최소 버전**을 Windows 10, 버전 1803(빌드 17134)으로 설정한 다음, **확인**을 클릭합니다.

    ![대상 버전 및 최소 버전](images/WinUI-min-target-version.png)

6. 이제 Visual Studio는 다음과 같은 두 개의 프로젝트를 생성합니다.

    * ***프로젝트 이름*(데스크톱)** : 이 프로젝트에는 앱의 코드가 포함됩니다. **App.xaml** 및 다양한 **앱** 코드 파일은 앱 인스턴스를 나타내는 `Application` 클래스를 정의하고, **MainWindow.xaml** 및 다양한 **MainWindow** 코드 파일은 앱에서 표시하는 주 창을 나타내는 `MainWindow` 클래스를 정의합니다. 이러한 클래스는 WinUI에서 제공하는 **Microsoft.UI.Xaml** 네임스페이스의 형식에서 파생됩니다.

        ![앱 프로젝트](images/WinUI-cpp-appproject.png)

    * ***프로젝트 이름*(패키지)** : 앱 프로젝트 템플릿은 앱을 배포용 MSIX 패키지로 빌드하도록 구성되는 [Windows 애플리케이션 패키징 프로젝트](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)입니다. 이 프로젝트는 앱의 [패키지 매니페스트](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root)를 포함하고 있으며, 기본적으로 솔루션의 시작 프로젝트입니다.

        ![패키지 프로젝트](images/WinUI-cpp-packageproject.png)

7. 앱 프로젝트에 새 항목을 추가하려면 **솔루션 탐색기**에서 ***프로젝트 이름*(데스크톱)** 프로젝트 노드를 마우스 오른쪽 단추로 클릭하고 **추가** -> **새 항목**을 선택합니다. **새 항목 추가** 대화 상자에서 **WinUI** 탭을 선택하고, 추가하려는 항목을 선택한 다음, **추가**를 클릭합니다. 다음과 같은 유형의 항목 중에서 선택할 수 있습니다.

    * **빈 페이지**
    * **빈 창**
    * **사용자 지정 컨트롤**
    * **리소스 사전**
    * **리소스 파일**
    * **사용자 컨트롤**

    ![새 항목](images/WinUI-cpp-newitem.png)

8. 솔루션을 빌드하고 실행하여 앱이 오류 없이 실행되는지 확인합니다.

## <a name="known-issues-and-limitations"></a>알려진 문제 및 제한 사항

Preview 1의 알려진 문제 및 제한 사항 목록은 [이 섹션](index.md#preview-1-limitations-and-known-issues)을 참조하세요.

## <a name="related-topics"></a>관련 항목

* [WinUI 3.0](index.md)