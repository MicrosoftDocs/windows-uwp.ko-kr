---
author: msatranjr
title: 관리되는 Windows 런타임 구성 요소 배포
description: Windows 런타임 구성 요소를 파일 복사로 배포할 수 있습니다.
ms.assetid: 80262992-89FC-42FC-8298-5AABF58F8212
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6461b6889f110bde8929e1f370f9197caa33e5f3
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7166385"
---
# <a name="distributing-a-managed-windows-runtime-component"></a>관리되는 Windows 런타임 구성 요소 배포



Windows 런타임 구성 요소를 파일 복사로 배포할 수 있습니다. 그러나 구성 요소가 여러 개의 파일로 구성된 경우 사용자가 설치하는 데 번거로울 수 있습니다. 또한 파일 배치 또는 참조 설정 시 발생하는 오류로 인해 문제가 발생할 수 있습니다. 설치와 사용이 쉽도록 복잡한 구성 요소를 Visual Studio 확장 SDK로 패키지할 수 있습니다. 사용자는 전체 패키지에 대해 하나의 참조만 설정하면 됩니다. MSDN 라이브러리의 [Visual Studio 확장 찾기 및 사용](https://msdn.microsoft.com/library/vstudio/dd293638.aspx)에서 설명한 대로 **확장 및 업데이트** 대화 상자를 사용하여 구성 요소를 쉽게 찾고 설치할 수 있습니다.

## <a name="planning-a-distributable-windows-runtime-component"></a>배포 가능한 Windows 런타임 구성 요소 계획

.winmd 파일과 같은 이진 파일에 대한 고유 이름을 선택합니다. 고유성을 보장하기 위해 다음 형식을 사용하는 것이 좋습니다.

``` syntax
company.product.purpose.extension
For example: Microsoft.Cpp.Build.dll
```

이진 파일이 다른 개발자의 이진 파일과 함께 앱 패키지에 설치됩니다. MSDN 라이브러리의 [방법: 소프트웨어 개발자 키트 만들기](https://msdn.microsoft.com/library/hh768146.aspx)에서 “확장 SDK”를 참조하세요.

구성 요소를 배포하는 방법을 결정하려면 복잡성을 고려해야 합니다. 확장 SDK 또는 유사한 패키지 관리자는 다음 경우에 권장됩니다.

-   구성 요소가 여러 파일로 구성됩니다.
-   여러 플랫폼에 대해 구성 요소의 버전을 제공합니다(예: x86 및 ARM).
-   구성 요소의 디버그 및 릴리스 버전을 모두 제공합니다.
-   구성 요소에 디자인 타임에만 사용하는 파일 및 어셈블리가 있습니다.

위의 둘 이상에 해당하는 경우 확장 SDK가 특히 유용합니다.

> **참고**복잡 한 구성 요소의 경우 NuGet 패키지 관리 시스템이 확장 Sdk 대신 오픈 소스를 제공 합니다. 확장 SDK와 마찬가지로 NuGet을 사용하면 복잡한 구성 요소의 설치를 단순화하는 패키지를 만들 수 있습니다. NuGet 패키지 및 Visual Studio 확장 SDK의 비교를 위해 MSDN 라이브러리에서 [NuGet 및 확장 SDK를 사용하여 참조 추가](https://msdn.microsoft.com/library/jj161096.aspx)를 참조하세요.

## <a name="distribution-by-file-copy"></a>파일 복사로 배포

구성 요소가 단일 .winmd 파일 또는 .winmd 파일 및 리소스 인덱스(.pri) 파일로 구성된 경우 단순히 사용자가 .winmd 파일을 복사하도록 할 수 있습니다. 사용자는 프로젝트에서 원하는 곳 어디에든 파일을 둘 수 있고 **기존 항목 추가** 대화 상자를 사용하여 프로젝트에 .winmd 파일을 추가할 수 있으며 참조 관리자 대화 상자를 사용하여 참조를 만들 수 있습니다. .pri 파일 또는 .xml 파일을 포함하는 경우 사용자에게 해당 파일을 .winmd 파일과 함께 두도록 지시합니다.

> **참고**Visual Studio 항상.pri 파일을 생성 Windows 런타임 구성 요소를 빌드할 때 프로젝트에 리소스가 포함 되지 않더라도 합니다. 구성 요소에 대한 테스트 앱이 있는 경우 bin\\debug\\AppX 폴더에 있는 앱 패키지의 콘텐츠를 검사하여 .pri 파일이 사용되는지 여부를 확인할 수 있습니다. 구성 요소의 .pri 파일이 여기에 없으면 배포할 필요가 없습니다. 대신 [MakePRI.exe](https://msdn.microsoft.com/library/windows/apps/jj552945.aspx) 도구를 사용하여 Windows 런타임 구성 요소 프로젝트에서 리소스 파일을 덤프할 수 있습니다. 예를 들어 Visual Studio 명령 프롬프트 창에서 다음을 입력합니다. makepri dump /if MyComponent.pri /of MyComponent.pri.xml .pri 파일에 대한 자세한 내용은 [리소스 관리 시스템(Windows)](https://msdn.microsoft.com/library/windows/apps/jj552947.aspx)에서 확인할 수 있습니다.

## <a name="distribution-by-extension-sdk"></a>확장 SDK로 배포

복잡한 구성 요소에는 일반적으로 Windows 리소스가 포함되지만, 이전 섹션에서 빈 .pri 파일 검색에 대한 참고 사항을 참조하세요.

**확장 SDK를 만들려면**

1.  Visual Studio SDK가 설치되어 있는지 확인합니다. [Visual Studio 다운로드](https://www.visualstudio.com/downloads/download-visual-studio-vs) 페이지에서 Visual Studio SDK를 다운로드할 수 있습니다.
2.  VSIX 프로젝트 템플릿을 사용하여 새 프로젝트를 만듭니다. Visual C# 또는 Visual Basic 아래의 확장성 범주에서 템플릿을 찾을 수 있습니다. 이 템플릿은 Visual Studio SDK의 일부로 설치됩니다. ([연습: C# 또는 Visual Basic을 사용하여 SDK 만들기](https://msdn.microsoft.com/library/jj127119.aspx) 또는 [연습: C++를 사용하여 SDK 만들기](https://msdn.microsoft.com/library/jj127117.aspx)는 매우 간단한 시나리오로 이 템플릿의 사용 방법을 보여 줍니다. )
3.  SDK에 대한 폴더 구조를 확인합니다. 폴더 구조는 VSIX 프로젝트의 루트 수준에서 시작하여 **References**, **Redist** 및 **DesignTime** 폴더가 있습니다.

    -   **References**는 사용자가 프로그래밍할 수 있는 이진 파일의 위치입니다. 확장 SDK는 사용자의 Visual Studio 프로젝트에 이러한 파일에 대한 참조를 만듭니다.
    -   **Redist**는 구성 요소를 사용하여 만들어지는 앱에서 이진 파일로 배포해야 하는 다른 파일의 위치입니다.
    -   **DesignTime**은 개발자가 구성 요소를 사용하는 앱을 만들고 있는 경우에만 사용하는 파일의 위치입니다.

    이러한 각각의 폴더에서 구성 폴더를 만들 수 있습니다. 허용된 이름은 debug, retail 및 CommonConfiguration입니다. CommonConfiguration 폴더는 정품 빌드에서 사용하든 디버그 빌드에서 사용하든 동일한 파일을 위해 사용됩니다. 구성 요소의 정품 빌드만 배포하는 경우 모두 CommonConfiguration에 넣고 다른 두 폴더는 생략할 수 있습니다.

    각 구성 폴더에서 플랫폼별 파일에 대한 아키텍처 폴더를 제공할 수 있습니다. 모든 플랫폼에 대해 동일한 파일을 사용하는 경우 neutral이라는 단일 폴더를 제공할 수 있습니다. 다른 아키텍처 폴더 이름을 포함하여 폴더 구조에 대한 세부 정보는 MSDN 라이브러리의 [방법: 소프트웨어 개발자 키트 만들기](https://msdn.microsoft.com/library/hh768146.aspx)에서 확인할 수 있습니다. (이 문서에서는 플랫폼 SDK 및 확장 SDK를 모두 설명합니다. 혼동을 피하기 위해 플랫폼 SDK에 대한 섹션을 축소하는 것이 좋습니다. )

4.  SDK 매니페스트 파일을 만듭니다. 매니페스트는 이름 및 버전 정보, SDK가 지원하는 아키텍처, .NET Framework 버전 및 Visual Studio가 SDK를 사용하는 방법에 대한 기타 정보를 지정합니다. 세부 정보 및 예제는 [방법: 소프트웨어 개발자 키트 만들기](https://msdn.microsoft.com/library/hh768146.aspx)에서 확인할 수 있습니다.
5.  확장 SDK를 빌드 및 배포합니다. VSIX 패키지 지역화 및 서명을 비롯한 자세한 내용은 MSDN 라이브러리의 VSIX 배포를 참조하세요.

## <a name="related-topics"></a>관련 항목

* [소프트웨어 개발자 키트 만들기](https://msdn.microsoft.com/library/hh768146.aspx)
* [NuGet 패키지 관리 시스템](https://github.com/NuGet/Home)
* [리소스 관리 시스템(Windows)](https://msdn.microsoft.com/library/windows/apps/jj552947.aspx)
* [Visual Studio 확장 찾기 및 사용](https://msdn.microsoft.com/library/dd293638.aspx)
* [MakePRI.exe 명령 옵션](https://msdn.microsoft.com/library/windows/apps/jj552945.aspx)
