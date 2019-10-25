---
title: 관리 되는 Windows 런타임 구성 요소 배포
description: 파일 복사를 통해 Windows 런타임 구성 요소를 배포할 수 있습니다.
ms.assetid: 80262992-89FC-42FC-8298-5AABF58F8212
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1d306c02d4fd99acaa49ec59230181ac0a20c9f3
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690382"
---
# <a name="distributing-a-managed-windows-runtime-component"></a>관리 되는 Windows 런타임 구성 요소 배포

파일 복사를 통해 Windows 런타임 구성 요소를 배포할 수 있습니다. 그러나 구성 요소가 여러 파일로 구성 된 경우 사용자에 게 설치가 번거로울 수 있습니다. 또한 파일을 배치 하거나 참조를 설정 하지 못할 경우 오류가 발생할 수 있습니다. 간편 하 게 설치 하 고 사용할 수 있도록 복잡 한 구성 요소를 Visual Studio 확장 SDK로 패키지할 수 있습니다. 사용자는 전체 패키지에 대해 하나의 참조를 설정 하기만 하면 됩니다. [Visual Studio 확장 찾기 및 사용](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2015)에 설명 된 대로 **확장 및 업데이트** 대화 상자를 사용 하 여 구성 요소를 쉽게 찾고 설치할 수 있습니다.

## <a name="planning-a-distributable-windows-runtime-component"></a>배포 가능한 Windows 런타임 구성 요소 계획

파일 (예: winmd 파일)에 대해 고유한 이름을 선택 합니다. 고유성이 보장 되도록 하려면 다음 형식을 권장 합니다.

``` syntax
company.product.purpose.extension
For example: Microsoft.Cpp.Build.dll
```

이진 파일은 다른 개발자의 이진 파일을 비롯 하 여 앱 패키지에 설치 됩니다. [방법: 소프트웨어 개발 키트 만들기](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)에서 "확장 sdk"를 참조 하세요.

구성 요소를 배포 하는 방법을 결정 하려면 얼마나 복잡 한지 고려 하세요. 확장 SDK 또는 비슷한 패키지 관리자는 다음과 같은 경우에 권장 됩니다.

-   구성 요소는 여러 파일로 구성 됩니다.
-   여러 플랫폼 (예: x86 및 ARM)의 구성 요소 버전을 제공 합니다.
-   구성 요소의 디버그 버전과 릴리스 버전을 모두 제공 합니다.
-   구성 요소에는 디자인 타임에만 사용 되는 파일 및 어셈블리가 있습니다.

확장 SDK는 위의 둘 이상에 해당 하는 경우 특히 유용 합니다.

> **참고**  복잡 한 구성 요소의 경우 NuGet 패키지 관리 시스템는 확장 Sdk에 대 한 오픈 소스 대안을 제공 합니다. 확장 Sdk와 마찬가지로 NuGet을 사용 하면 복잡 한 구성 요소의 설치를 간소화 하는 패키지를 만들 수 있습니다. NuGet 패키지와 Visual Studio 확장 Sdk를 비교 하려면 [nuget을 사용한 참조 추가와 확장명 sdk를 사용한 참조 추가](https://docs.microsoft.com/visualstudio/ide/adding-references-using-nuget-versus-an-extension-sdk?view=vs-2015)를 참조 하세요.

## <a name="distribution-by-file-copy"></a>파일 복사를 통한 배포

구성 요소가 단일 winmd 파일이 나 winmd 파일 및 리소스 인덱스 (pri) 파일로 구성 된 경우 사용자가 파일을 복사할 수 있도록 합니다. 사용자는 프로젝트에서 원하는 위치에 파일을 배치 하 고 **기존 항목 추가** 대화 상자를 사용 하 여 프로젝트에 winmd 파일을 추가한 다음 참조 관리자 대화 상자를 사용 하 여 참조를 만들 수 있습니다. Pri 파일이 나 .xml 파일을 포함 하는 경우 사용자에 게 winmd 파일을 포함 하 여 해당 파일을 저장 하도록 지시 합니다.

> **참고**  Visual Studio는 프로젝트에 리소스가 포함 되지 않은 경우에도 Windows 런타임 구성 요소를 빌드할 때 항상 .pri 파일을 생성 합니다. 구성 요소에 대 한 테스트 앱이 있는 경우 bin\\AppX 폴더\\응용 프로그램 패키지의 내용을 검사 하 여 .pri 파일을 사용할지 여부를 결정할 수 있습니다. 구성 요소의 pri 파일이 표시 되지 않으면 배포할 필요가 없습니다. 또는 [Makepri](https://docs.microsoft.com/previous-versions/windows/apps/jj552945(v=win.10)) 도구를 사용 하 여 Windows 런타임 구성 요소 프로젝트에서 리소스 파일을 덤프할 수 있습니다. 예를 들어 Visual Studio 명령 프롬프트 창에서 makepri.exe dump/if MyComponent MyComponent를 입력 합니다. [즉, 리소스 관리 시스템 (Windows)](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10))에서 .pri 파일에 대 한 자세한 내용을 확인할 수 있습니다.

## <a name="distribution-by-extension-sdk"></a>확장 SDK에의 한 배포

복잡 한 구성 요소는 일반적으로 Windows 리소스를 포함 하지만 이전 섹션에서 빈 .pri 파일 검색에 대 한 참고를 참조 하세요.

**확장 SDK를 만들려면**

1.  Visual Studio SDK가 설치 되어 있는지 확인 합니다. Visual studio [다운로드](https://visualstudio.microsoft.com/downloads/download-visual-studio-vs) 페이지에서 VISUAL studio SDK를 다운로드할 수 있습니다.
2.  VSIX 프로젝트 템플릿을 사용 하 여 새 프로젝트를 만듭니다. 확장성 범주의 시각적 개체 C# 또는 Visual Basic에서 템플릿을 찾을 수 있습니다. 이 템플릿은 Visual Studio SDK의 일부로 설치 됩니다. ([연습: 또는 Visual Basic를 사용 C# 하 여 sdk 만들기](https://docs.microsoft.com/visualstudio/extensibility/walkthrough-creating-an-sdk-using-csharp-or-visual-basic?view=vs-2015) 또는 [연습:를 사용 C++하 여 sdk ](https://docs.microsoft.com/visualstudio/extensibility/walkthrough-creating-an-sdk-using-cpp?view=vs-2015)만들기에서는 매우 간단한 시나리오에서이 템플릿을 사용 하는 방법을 보여 줍니다. )
3.  SDK에 대 한 폴더 구조를 결정 합니다. 폴더 구조는 **참조**, **Redist**및 **designtime** 폴더를 사용 하 여 VSIX 프로젝트의 루트 수준에서 시작 됩니다.

    -   **참조** 는 사용자가 프로그래밍할 수 있는 이진 파일의 위치입니다. 확장 SDK는 사용자의 Visual Studio 프로젝트에서 이러한 파일에 대 한 참조를 만듭니다.
    -   **Redist** 는 구성 요소를 사용 하 여 만든 앱에서 이진 파일과 함께 배포 해야 하는 다른 파일의 위치입니다.
    -   **Designtime** 은 개발자가 구성 요소를 사용 하는 앱을 만드는 경우에만 사용 되는 파일의 위치입니다.

    이러한 각 폴더에서 구성 폴더를 만들 수 있습니다. 허용 되는 이름은 debug, retail 및 CommonConfiguration입니다. CommonConfiguration 폴더는 일반 정품 또는 디버그 빌드에서 사용 되는지에 관계 없이 동일한 파일에 사용 됩니다. 구성 요소의 정품 빌드만 배포 하는 경우 모든 항목을 CommonConfiguration에 추가 하 고 다른 두 폴더를 생략할 수 있습니다.

    각 구성 폴더에서 플랫폼별 파일의 아키텍처 폴더를 제공할 수 있습니다. 모든 플랫폼에 동일한 파일을 사용 하는 경우 중립 이라는 단일 폴더를 제공할 수 있습니다. [방법: 소프트웨어 개발 키트 만들기](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)에서 다른 아키텍처 폴더 이름을 포함 한 폴더 구조에 대 한 세부 정보를 찾을 수 있습니다. (이 문서에서는 플랫폼 Sdk와 확장 Sdk 모두에 대해 설명 합니다. 혼동을 피하기 위해 플랫폼 Sdk에 대 한 섹션을 축소 하는 것이 유용할 수 있습니다. )

4.  SDK 매니페스트 파일을 만듭니다. 매니페스트는 이름 및 버전 정보, SDK에서 지 원하는 아키텍처, .NET 버전 및 Visual Studio에서 SDK를 사용 하는 방법에 대 한 기타 정보를 지정 합니다. [방법: 소프트웨어 개발 키트 만들기](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)에서 세부 정보 및 예제를 찾을 수 있습니다.
5.  확장 SDK를 빌드하고 배포 합니다. VSIX 패키지 지역화 및 서명을 포함 한 자세한 내용은 [Vsix 배포](https://docs.microsoft.com/visualstudio/misc/how-to-manually-package-an-extension-vsix-deployment?view=vs-2015)를 참조 하세요.

## <a name="related-topics"></a>관련된 항목

* [소프트웨어 개발 키트 만들기](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)
* [NuGet 패키지 관리 시스템](https://github.com/NuGet/Home)
* [리소스 관리 시스템 (Windows)](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10))
* [Visual Studio 확장 찾기 및 사용](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2015)
* [MakePRI 명령 옵션](https://docs.microsoft.com/previous-versions/windows/apps/jj552945(v=win.10))
