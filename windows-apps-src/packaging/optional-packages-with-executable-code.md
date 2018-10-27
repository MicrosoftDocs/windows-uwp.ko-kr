---
author: laurenhughes
title: 실행 가능한 코드가 있는 선택적 패키지
description: Visual Studio를 사용하여 실행 코드로 선택적 패키지를 사용하는 방법을 알아보세요.
ms.author: lahugh
ms.date: 9/30/2018
ms.topic: article
keywords: Windows 10, uwp 앱 설치 관리자, AppInstaller, 사이드로드, 관련 집합, 선택적 패키지
ms.localizationpriority: medium
ms.openlocfilehash: 8f454c7e7386ba829cf6c99edc0b5b8f2d831f00
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5700852"
---
# <a name="optional-packages-with-executable-code"></a>실행 가능한 코드가 있는 선택적 패키지
 
실행 코드를 사용하는 선택적 패키지는 대용량 또는 복잡한 앱을 분할하거나 이미 게시된 앱에 추가하는 데 유용합니다. Visual Studio 2017 버전 15.7 및 .NET 네이티브 2.1을 사용하면 C++와 C# 선택적 패키지에서 실행 코드를 로드할 수 있습니다.

## <a name="prerequisites"></a>필수 구성 요소
- Visual Studio 2017, 버전 15.7
- Windows 10, 버전 1709
- Windows 10, 버전 1709 SDK

최신 개발 도구를 다운로드하는 방법은 [Windows 10용 다운로드 및 도구](https://developer.microsoft.com/windows/downloads)를 참조하세요. 

> [!NOTE]
> 선택적 패키지 및 관련 집합을 사용하는 앱을 Microsoft Store에 제출하려면 권한이 필요합니다. Microsoft Store에 제출되지 않은 경우에는 선택적 패키지 및 관련 집합을 개발자 센터 권한 없이 기간 업무(LOB)나 엔터프라이즈 앱에서 사용할 수 있습니다. 선택적 패키지 및 관련 집합을 사용한 앱을 제출할 권한을 얻는 방법은 [Windows 개발자 지원](https://developer.microsoft.com/windows/support)을 참조하세요.

## <a name="c-optional-packages-with-executable-code"></a>실행 가능한 코드가 있는 C++ 선택적 패키지

C++ 선택적 패키지에서 코드를 로드하려면 GitHub의 [OptionalPackageSample](https://github.com/AppInstaller/OptionalPackageSample) 리포지토리를 참조하세요. [OptionalPackageDLL](https://github.com/AppInstaller/OptionalPackageSample/tree/master/OptionalPackageDLL)은 주 패키지에서 실행할 수 있는 코드로 프로젝트를 만드는 방법을 보여 줍니다. MyMainApp 프로젝트는 OptionalPackageDLL.dll 파일에서 [코드를 로드](https://github.com/AppInstaller/OptionalPackageSample/blob/bf6b4915ff1f3b8abfdaacb1ad9e77184c49fe18/MyMainApp/MainPage.xaml.cpp#L182)하는 방법을 보여 줍니다.

## <a name="c-optional-packages-with-executable-code"></a>실행 가능한 코드가 있는 C# 선택적 패키지

C#로 선택적 코드 패키지 빌드를 시작하려면 다음 단계를 따라 솔루션을 구성하세요.

1. 최소 버전을 Windows 10 Fall Creators Update SDK(16299 빌드) 이상으로 설정하여 새 UWP 응용 프로그램을 만듭니다.

2. 솔루션에 새 **선택적 코드 패키지(유니버설 Windows)** 프로젝트를 추가합니다. **최소 버전** 및 **대상 버전**이 메인 앱과 일치하는지 확인합니다.

3. Microsoft Store에 앱을 제출하는 경우 두 프로젝트를 마우스 오른쪽 단추로 클릭하고 **Microsoft Store -> Microsoft Store에 앱 연결...** 을 선택합니다.

4. 메인 앱의 `Package.appxmanifest` 파일을 열고 `Identity Name` 값을 찾습니다. 다음 단계를 위해 이 값을 적어 둡니다.

5. 선택적 앱 패키지의 `Package.appxmanifest` 파일을 열고 `uap3:MainAppPackageDependency Name` 값을 찾습니다. `uap3:MainAppPackageDependency Name` 값을 이전 단계의 주 앱 패키지의 `Identity Name` 값과 일치하도록 업데이트합니다. 

    메인 앱의 `Package.appxmanifest`의 `Identity` 예는 다음과 같습니다.
    ```XML
    <Identity Name="12345.MainAppProject" Publisher="CN=PublisherName" Version="1.0.0.0" />
    ```

    선택적 앱 패키지의 `uap3:MainPackageDependency`를 메인 앱의 `Identity`에 맞게 업데이트해야 합니다.
    ```XML
    <uap3:MainPackageDependency Name="12345.MainAppProjectTest" />
    ```

6. 메인 앱에 `Bundle.mapping.txt` 파일을 추가합니다. [관련 설정](https://docs.microsoft.com/windows/uwp/packaging/optional-packages#related-sets) 섹션의 단계를 수행하여 두 앱을 포함하는 관련 설정을 만듭니다. 

7. 선택적 패키지 프로젝트를 빌드한 다음 `..\[PathToOptionalPackageProject]\bin\[architecture]\[configuration]\Reference`에서 찾은 빌드의 출력에 있는 패키지 참조 폴더로 이동합니다. `.winmd` 파일(8단계)은 아키텍처와 독립적이므로 참조 폴더에 대한 경로에서 모든 아키텍처를 선택할 수 있습니다.

8. 기본 앱 프로젝트의 참조를 추가 이 폴더에서 찾은 `.winmd` 파일에 추가합니다. 선택적 패키지 프로젝트에서 API 노출 영역을 변경할 때마다 이 `.winmd` 파일을 **반드시** 업데이트해야 합니다. 이 참조는 기본 앱 프로젝트에 컴파일하는 데 필요한 정보를 제공합니다.

9. 기본 앱 프로젝트에서 프로젝트 빌드 속성으로 이동하고 **.NET 네이티브 도구 체인으로 컴파일**을 선택합니다. 현재 C#로 선택적 코드 패키지 작성에 대해 .NET 네이티브에서의 디버깅만 지원됩니다. 프로젝트 디버그 속성으로 이동하고 **선택적 패키지 배포**를 선택합니다. 이렇게 하면 기본 앱 프로젝트를 배포할 때마다 패키지가 모두 동기화됩니다.

이러한 단계를 완료했으면 관리되는 WinRT 구성 요소 프로젝트처럼 코드를 선택적 패키지 프로젝트에 추가할 수 있습니다. 기본 앱 프로젝트에서 코드에 액세스하려면 선택적 패키지 프로젝트에 노출된 공개 메서드를 호출합니다.