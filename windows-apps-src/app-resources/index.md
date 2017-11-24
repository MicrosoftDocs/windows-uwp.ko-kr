---
author: stevewhims
Description: "이 섹션은 앱의 문자열, 이미지 및 파일 리소스를 작성하고 패키징하고 사용하는 방법을 보여줍니다."
title: "앱 리소스 및 리소스 관리 시스템"
label: Intro
template: detail.hbs
ms.author: stwhi
ms.date: 10/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, 리소스, 이미지, 자산, MRT, 한정자"
localizationpriority: medium
ms.openlocfilehash: 38a131704bacbffdf89636aa70b405aa30861d27
ms.sourcegitcommit: d0c93d734639bd31f264424ae5b6fead903a951d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/03/2017
---
# <a name="app-resources-and-the-resource-management-system"></a>앱 리소스 및 리소스 관리 시스템
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

이 섹션은 앱의 문자열, 이미지 및 파일 리소스를 작성하고 패키징하고 사용하는 방법을 보여줍니다. 예를 들어 게임의 수준에 대한 정의를 포함하는 가벼운 게임과 함께 파일을 패키징하고 실행 시 파일을 로드할 수 있습니다. 또한 앱의 논리와 별도로 리소스를 유지하여 다른 로캘, 장치 디스플레이, 접근성 설정, 기타 사용자 및 시스템 컨텍스트에 대해 앱을 지역화 및 사용자 지정하는 쉬운 방법을 보여줍니다. 일반적으로 문자열 및 이미지 등의 리소스는 여러 언어, 배율 및 대비 변형으로 존재해야 합니다. 그러한 리소스에 대해 [리소스 관리 시스템](resource-management-system.md)에서 지원을 받을 수 있습니다.

앱 리소스에는 두 가지 유형이 있습니다.
- 파일 리소스는 디스크에 파일로 저장된 리소스입니다. 파일 리소스에는 비트맵 이미지, XAML, XML, HTML, 또는 다른 종류의 데이터가 포함될 수 있습니다.
- 포함된 리소스는 이를 포함하는 일부 리소스 파일에 포함된 리소스입니다. 가장 일반적인 예는 리소스 파일(.resw 또는.resjson)에 포함된 문자열 리소스입니다.

앱 지역화의 가치 제안에 대한 자세한 내용은 [세계화 및 지역화](../globalizing/globalizing-portal.md)를 참조하세요.

| 문서 | 설명 |
|---------|-------------|
| [리소스 관리 시스템](resource-management-system.md) | 빌드 시 리소스 관리 시스템은 앱과 함께 패키지되는 모든 여러 변형의 리소스의 인덱스를 만듭니다. 실행 시 시스템은 적용 중인 사용자와 시스템 설정을 감지하고 이러한 설정에 대한 최적의 일치인 리소스를 로드합니다. |
| [리소스 관리 시스템이 리소스를 일치시키고 선택하는 방법](how-rms-matches-and-chooses-resources.md) | 리소스를 요청하는 경우 현재 리소스 컨텍스트와 어느 정도 일치하는 몇 가지 후보가 있을 수 있습니다. 리소스 관리 시스템은 모든 후보를 분석하고 반환할 최적의 후보를 결정합니다. 이 항목은 이 프로세스를 자세히 설명하고 예를 제공합니다. |
| [리소스 관리 시스템이 언어 태그를 일치하는 방법](how-rms-matches-lang-tags.md) | 이전 항목([리소스 관리 시스템이 리소스를 일치시키고 선택하는 방법](how-rms-matches-and-chooses-resources.md))은 일반적으로 한정자 일치를 살펴봅니다. 이 항목은 언어-태그-일치에 대해 더 자세히 알아봅니다. |
| [언어, 배율, 고대비 및 기타 한정자에 맞게 리소스 조정](tailor-resources-lang-scale-contrast.md) | 이 항목에서는 리소스 한정자의 일반적인 개념, 사용 방법 및 각 한정자 이름의 목적에 대해 설명합니다. |
| [UI와 앱 패키지 매니페스트에 문자열 지역화](localize-strings-ui-manifest.md) | 앱에서 다른 표시 언어를 지원하길 원하고 코드 또는 XAML 태그 또는 앱 패키지 매니페스트에 문자열 리터럴이 있는 경우, 해당 문자열을 리소스 파일(.resw)로 이동합니다. 그런 다음 앱에서 지원하는 언어별로 해당 리소스 파일의 번역본을 만들 수 있습니다. |
| [배율, 테마, 고대비 등에 맞춘 이미지 및 자산 로드](images-tailored-for-scale-theme-contrast.md) | 앱은 디스플레이 배율 인수, 테마, 고대비 및 기타 런타임 컨텍스트에 맞게 조정된 이미지를 포함하는 이미지 리소스 파일을 로드할 수 있습니다. |
| [언어, 배율, 고대비에 대한 타일 및 알림 메시지](tile-toast-language-scale-contrast.md) | 타일 및 알림은 표시 언어, 디스플레이 배율 인수, 고대비 및 기타 런타임 컨텍스트에 맞게 조정된 문자열 및 이미지를 로드할 수 있습니다. |
| [URI 스키마](uri-schemes.md) | 앱 패키지, 앱의 데이터 폴더 또는 클라우드에서 파일을 참조하는 데 사용할 수 있는 몇 가지 URI (Uniform Resource Identifier) 스키마가 있습니다. URI 스키마를 사용하여 앱의 리소스 파일(.resw)에서 로드되는 문자열을 참조할 수 있습니다. |
| [MakePri.exe를 사용하여 수동으로 리소스 컴파일](compile-resources-manually-with-makepri.md) | MakePri.exe는 PRI 파일을 만들고 덤프하는 데 사용할 수 있는 명령줄 도구입니다. Microsoft Visual Studio 내에 MSBuild의 일부로 통합되어 있지만 수동으로 또는 사용자 지정 빌드 시스템으로 패키지를 만드는 데 도움이 될 수 있습니다. |
| [MakePri.exe 명령줄 옵션](makepri-exe-command-options.md) | MakePri.exe에는 `createconfig`, `dump`, `new`, `resourcepack` 및 `versioned` 명령 집합이 있습니다. 이 항목에서는 이들의 사용에 대한 명령줄 옵션을 자세히 설명합니다. |
| [MakePri.exe 구성 파일](makepri-exe-configuration.md) | 이 항목에서는 MakePri.exe XML 구성 파일 스키마를 설명합니다. |
| [MakePri.exe 형식별 인덱서](makepri-exe-format-specific-indexers.md) | 이 항목에서는 리소스 인덱스를 생성하기 위해 MakePri.exe 도구에 의해 사용되는 형식별 인덱서에 대해 설명합니다. |
| [레거시 앱 또는 게임에서 Windows 10 리소스 관리 시스템 사용](using-mrt-for-converted-desktop-apps-and-games.md) | .NET 또는 Win32 앱 또는 게임을 AppX 패키지로 패키징하면 리소스 관리 시스템을 사용하여 실행 시 컨텍스트에 맞게 조정된 앱 리소스를 로드할 수 있습니다. 이 항목에서는 그 기술에 대해 자세히 설명합니다. |

UWP(유니버설 Windows 플랫폼) 앱 및 Windows 10에도 여전히 적용되는 원래 Windows 8.x용으로 작성된 설명서도 참조하세요.

-   [앱 리소스 및 지역화](https://msdn.microsoft.com/library/windows/apps/xaml/hh710212.aspx)
