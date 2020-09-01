---
title: 관리 되는 Windows 런타임 구성 요소 배포
description: 배포 가능한 Windows 런타임 구성 요소를 계획 하 고 파일 복사 또는 확장명 SDK를 사용 하 여 배포 하는 방법을 알아봅니다.
ms.assetid: 80262992-89FC-42FC-8298-5AABF58F8212
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e959ee17eadfca6b65ccbad2c7a6087622fe1a01
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174257"
---
# <a name="distributing-a-managed-windows-runtime-component"></a>관리 되는 Windows 런타임 구성 요소 배포

파일 복사를 통해 Windows 런타임 구성 요소를 배포할 수 있습니다. 그러나 구성 요소를 구성하는 파일 수가 많으면 사용자가 설치할 때 번거로울 수 있습니다. 또한 파일을 배치할 때나 참조를 설정할 때 발생하는 오류로 인해 파일이나 참조에 문제가 발생할 수 있습니다. 복잡한 구성 요소를 Visual Studio Extension SDK로 패키징하여 쉽게 설치 및 사용하도록 할 수 있습니다. 사용자는 전체 패키지에 대해 하나의 참조만 설정하면 됩니다. [Visual Studio 확장 찾기 및 사용](/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2015)에 설명 된 대로 **확장 및 업데이트** 대화 상자를 사용 하 여 구성 요소를 쉽게 찾고 설치할 수 있습니다.

## <a name="planning-a-distributable-windows-runtime-component"></a>배포 가능한 Windows 런타임 구성 요소 계획

.winmd 파일과 같은 이진 파일의 고유 이름을 선택하십시오. 다음과 같은 형식을 사용하여 고유성을 보장하는 것이 좋습니다.

``` syntax
company.product.purpose.extension
For example: Microsoft.Cpp.Build.dll
```

이진 파일은 앱 패키지에 설치됩니다. 이 패키지에는 다른 개발자의 이진 파일이 포함될 수 있습니다. [방법: 소프트웨어 개발 키트 만들기](/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)에서 "확장 sdk"를 참조 하세요.

구성 요소를 배포하는 방법을 결정하려면 구성 요소의 복잡한 정도를 고려하세요. 다음과 같은 경우에는 확장명 SDK 또는 비슷한 패키지 관리자를 사용하는 것이 좋습니다.

-   구성 요소가 여러 개의 파일로 구성되어 있습니다.
-   여러 플랫폼(예: x86 및 ARM)에 대한 구성 요소 버전을 제공합니다.
-   구성 요소의 디버그 버전과 릴리스 버전을 제공합니다.
-   구성 요소에 디자인 타임에만 사용되는 파일 및 어셈블리가 있습니다.

확장 SDK는 위 내용 중 두 가지 이상에 해당하는 경우 특히 유용합니다.

> **참고**    복잡 한 구성 요소의 경우 NuGet 패키지 관리 시스템 확장 Sdk에 대 한 오픈 소스 대안을 제공 합니다. 확장명 SDK와 마찬가지로 NuGet을 사용하면 복잡한 구성 요소의 설치를 단순화하는 패키지를 만들 수 있습니다. NuGet 패키지와 Visual Studio 확장 Sdk를 비교 하려면 [nuget을 사용한 참조 추가와 확장명 sdk를 사용한 참조 추가](/visualstudio/ide/adding-references-using-nuget-versus-an-extension-sdk?view=vs-2015)를 참조 하세요.

## <a name="distribution-by-file-copy"></a>파일 복사를 통한 배포

구성 요소가 단일 .winmd 파일 또는 .winmd 파일과 리소스 인덱스 파일(.pri)로 구성되어 있는 경우 사용자가 .winmd 파일을 복사할 수 있도록 간단하게 만들 수 있습니다. 사용자는 프로젝트에서 원하는 위치에 파일을 배치 하 고 **기존 항목 추가** 대화 상자를 사용 하 여 프로젝트에 winmd 파일을 추가한 다음 참조 관리자 대화 상자를 사용 하 여 참조를 만들 수 있습니다. .pri 파일 또는 .xml 파일을 포함하고 있는 경우에는 해당 파일을 .winmd 파일과 함께 배치하도록 사용자에게 지시하세요.

> **참고**    프로젝트에 리소스가 포함 되어 있지 않더라도 Windows 런타임 구성 요소를 빌드할 때 Visual Studio에서 항상 .pri 파일을 생성 합니다. 구성 요소에 대 한 테스트 앱이 있는 경우 bin \\ debug AppX 폴더에 있는 앱 패키지의 내용을 검사 하 여 .pri 파일을 사용할지 여부를 결정할 수 있습니다 \\ . 구성 요소의 .pri 파일이 여기에 없으면 배포할 필요가 없습니다. 또는 [MakePRI.exe](/previous-versions/windows/apps/jj552945(v=win.10)) 도구를 사용 하 여 Windows 런타임 구성 요소 프로젝트에서 리소스 파일을 덤프할 수 있습니다. 예를 들어 Visual Studio 명령 프롬프트 창에서 makepri.exe dump/if MyComponent.pri.xml MyComponent를 입력 하 고 [리소스 관리 시스템 (Windows)](/previous-versions/windows/apps/jj552947(v=win.10))에서 .pri 파일에 대 한 자세한 내용을 읽을 수 있습니다.

## <a name="distribution-by-extension-sdk"></a>확장 SDK를 통한 배포

복잡한 구성 요소에는 일반적으로 Windows 리소스가 포함되어 있지만 이전 단원에서 빈 .pri 파일을 검색하는 방법에 대한 참고 사항을 참조하십시오.

**확장명 SDK를 만들려면**

1.  Visual Studio SDK가 설치 되어 있는지 확인 합니다. Visual studio [다운로드](https://visualstudio.microsoft.com/downloads/download-visual-studio-vs) 페이지에서 VISUAL studio SDK를 다운로드할 수 있습니다.
2.  VSIX 프로젝트 템플릿을 사용하여 새 프로젝트를 만듭니다. 확장성 범주의 Visual C# 또는 Visual Basic 아래에서 템플릿을 찾을 수 있습니다. 이 템플릿은 Visual Studio SDK의 일부로 설치 됩니다. ([연습: c # 또는 Visual Basic를 사용 하 여 Sdk 만들기](/visualstudio/extensibility/walkthrough-creating-an-sdk-using-csharp-or-visual-basic?view=vs-2015) 또는 [연습: c + +를 사용 하 여 sdk](/visualstudio/extensibility/walkthrough-creating-an-sdk-using-cpp?view=vs-2015)만들기에서는 매우 간단한 시나리오에서이 템플릿을 사용 하는 방법을 보여 줍니다. )
3.  해당 SDK에 대한 폴더 구조를 확인합니다. 폴더 구조는 **참조**, **Redist**및 **designtime** 폴더를 사용 하 여 VSIX 프로젝트의 루트 수준에서 시작 됩니다.

    -   **참조** 는 사용자가 프로그래밍할 수 있는 이진 파일의 위치입니다. 확장명 SDK는 사용자의 Visual Studio 프로젝트에 있는 이러한 파일에 대한 참조를 만듭니다.
    -   **Redist** 는 구성 요소를 사용 하 여 만든 앱에서 이진 파일과 함께 배포 해야 하는 다른 파일의 위치입니다.
    -   **Designtime** 은 개발자가 구성 요소를 사용 하는 앱을 만드는 경우에만 사용 되는 파일의 위치입니다.

    이 폴더 각각에서 구성 폴더를 만들 수 있습니다. 허용되는 이름은 debug, retail 및 CommonConfiguration입니다. CommonConfiguration 폴더는 일반 정품 빌드에 사용되는지 디버그 빌드에 사용되는지와 관계없이 동일한 파일에 사용되는 폴더입니다. 구성 요소의 일반 정품 빌드만 배포하는 경우 CommonConfiguration에 모두 넣고 다른 두 폴더는 생략할 수 있습니다.

    각 구성 폴더에서 플랫폼 관련 파일에 대한 아키텍처 폴더를 제공할 수 있습니다. 모든 플랫폼에 대해 동일한 파일을 사용하는 경우 neutral이라는 단일 폴더를 제공할 수 있습니다. [방법: 소프트웨어 개발 키트 만들기](/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)에서 다른 아키텍처 폴더 이름을 포함 한 폴더 구조에 대 한 세부 정보를 찾을 수 있습니다. (이 문서에서는 플랫폼 Sdk와 확장 Sdk 모두에 대해 설명 합니다. 혼동을 피하기 위해 플랫폼 SDK에 대한 섹션을 축소하는 것이 유용할 수 있습니다. )

4.  SDK 매니페스트 파일을 만듭니다. 매니페스트는 이름 및 버전 정보, SDK에서 지 원하는 아키텍처, .NET 버전 및 Visual Studio에서 SDK를 사용 하는 방법에 대 한 기타 정보를 지정 합니다. [방법: 소프트웨어 개발 키트 만들기](/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)에서 세부 정보 및 예제를 찾을 수 있습니다.
5.  확장 SDK를 빌드 및 배포합니다. VSIX 패키지 지역화 및 서명을 포함 한 자세한 내용은 [Vsix 배포](/visualstudio/misc/how-to-manually-package-an-extension-vsix-deployment?view=vs-2015)를 참조 하세요.

## <a name="related-topics"></a>관련 항목

* [소프트웨어 개발 키트 만들기](/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)
* [NuGet 패키지 관리 시스템](https://github.com/NuGet/Home)
* [자원 관리 시스템(Windows)](/previous-versions/windows/apps/jj552947(v=win.10))
* [Visual Studio 확장명 찾기 및 사용](/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2015)
* [MakePRI.exe 명령 옵션](/previous-versions/windows/apps/jj552945(v=win.10))