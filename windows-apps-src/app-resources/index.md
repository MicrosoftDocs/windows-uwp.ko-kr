---
Description: 이 섹션에서는 앱의 문자열, 이미지 및 파일 리소스를 작성, 패키징 및 사용하는 방법을 보여 줍니다.
title: 앱 리소스 및 리소스 관리 시스템
label: Intro
template: detail.hbs
ms.date: 10/20/2017
ms.topic: article
keywords: Windows 10, UWP, 리소스, 이미지, 자산, MRT, 한정자
ms.localizationpriority: medium
ms.openlocfilehash: a5af904c099b92e399f169221cae3122f358be19
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57583009"
---
# <a name="app-resources-and-the-resource-management-system"></a>앱 리소스 및 리소스 관리 시스템


이 섹션에서는 앱의 문자열, 이미지 및 파일 리소스를 작성, 패키징 및 사용하는 방법을 보여 줍니다. 예를 들어 게임의 수준에 대한 정의가 포함된 가벼운 게임과 함께 파일을 패키지하고 런타임 시 파일을 로드할 수 있습니다. 또한 앱의 논리와 별도로 리소스를 유지 관리하여 다양한 로캘, 디바이스 디스플레이, 접근성 설정, 기타 사용자 및 머신 컨텍스트에 맞게 앱을 쉽게 지역화하고 사용자 지정하는 방법을 보여 줍니다. 문자열 및 이미지와 같은 리소스는 일반적으로 여러 언어, 배율 및 대비 변형으로 존재해야 합니다. 이러한 리소스의 경우 [리소스 관리 시스템](resource-management-system.md)에서 지원할 수 있습니다.

앱 리소스에는 다음 두 가지 종류가 있습니다.
- 파일 리소스는 디스크에 파일로 저장된 리소스입니다. 파일 리소스에는 비트맵 이미지, XAML, XML, HTML, 또는 다른 종류의 데이터가 포함될 수 있습니다.
- 포함된 리소스는 이를 포함하는 일부 리소스 파일에 포함된 리소스입니다. 가장 일반적인 예는 리소스 파일(.resw 또는.resjson)에 포함된 문자열 리소스입니다.

앱 지역화의 가치 제안에 대한 자세한 내용은 [세계화 및 지역화](../design/globalizing/globalizing-portal.md)를 참조하세요.

| 문서 | 설명 |
|---------|-------------|
| [리소스 관리 시스템](resource-management-system.md) | 빌드 시 리소스 관리 시스템은 앱과 함께 패키지되는 다양한 모든 리소스 변형에 대한 인덱스를 만듭니다. 런타임 시 시스템에서 적용되는 사용자 및 머신 설정을 검색하고 이러한 설정에 가장 적합한 리소스를 로드합니다. |
| [리소스 관리 시스템에서 리소스를 일치시키고 선택하는 방법](how-rms-matches-and-chooses-resources.md) | 리소스가 요청되면 현재 리소스 컨텍스트와 어느 정도 일치하는 몇 가지 후보가 있을 수 있습니다. 리소스 관리 시스템은 모든 후보를 분석하여 반환할 가장 적합한 후보를 결정합니다. 이 문서에서는 이 프로세스를 자세히 설명하고 예제를 제공합니다. |
| [리소스 관리 시스템에서 언어 태그를 일치시키는 방법](how-rms-matches-lang-tags.md) | 앞의 문서([리소스 관리 시스템에서 리소스를 일치시키고 선택하는 방법](how-rms-matches-and-chooses-resources.md))에서는 일반적으로 한정자 일치를 살펴보지만, 여기서는 언어 태그 일치에 집중하고 있습니다. |
| [언어, 배율, 고대비 및 기타 한정자에 맞게 리소스 조정](tailor-resources-lang-scale-contrast.md) | 이 문서에서는 리소스 한정자의 일반적인 개념, 사용 방법 및 각 한정자 이름의 목적에 대해 설명합니다. |
| [UI 및 앱 패키지 매니페스트의 문자열 지역화](localize-strings-ui-manifest.md) | 앱에서 다른 표시 언어를 지원하도록 하고 코드, XAML 태그 또는 앱 패키지 매니페스트에 문자열 리터럴이 있으면 해당 문자열을 리소스 파일(.resw)로 이동합니다. 그러면 앱에서 지원하는 언어별로 해당 리소스 파일의 변환된 복사본을 만들 수 있습니다. |
| [배율, 테마, 고대비 등에 맞춘 이미지 및 자산 로드](images-tailored-for-scale-theme-contrast.md) | 앱은 디스플레이 배율 인수, 테마, 고대비 및 기타 런타임 컨텍스트에 맞게 조정된 이미지를 포함하는 이미지 리소스 파일을 로드할 수 있습니다. |
| [URI 체계](uri-schemes.md) | 앱의 패키지, 앱의 데이터 폴더 또는 클라우드에서 제공하는 파일을 참조하는 데 사용할 수 있는 몇 가지 URI(Uniform Resource Identifier) 체계가 있습니다. URI 체계를 사용하여 앱의 리소스 파일(.resw)에서 로드되는 문자열을 참조할 수도 있습니다. |
| [앱에서 사용하는 기본 리소스 지정](specify-default-resources-installed.md) | 앱에 고객 디바이스의 특정 설정과 일치하는 리소스가 없으면 앱의 기본 리소스가 사용됩니다. 이 문서에서는 이러한 기본 리소스를 지정하는 방법에 대해 설명합니다. |
| [리소스 팩 대신 앱 패키지에 리소스 빌드](build-resources-into-app-package.md) | 몇 가지 종류의 앱(다국어 사전, 번역 도구 등)은 앱 번들의 기본 동작을 재정의하고, 리소스를 별도의 리소스 패키지에 포함하지 않고 앱 패키지에 빌드해야 합니다. 이 문서에서는 이러한 작업을 수행하는 방법에 대해 설명합니다. |
| [PRI(패키지 리소스 인덱싱) API 및 사용자 지정 빌드 시스템](pri-apis-custom-build-systems.md) | [PRI(패키지 리소스 인덱싱) API](https://msdn.microsoft.com/library/windows/desktop/mt845690)를 사용하여 UWP 앱의 리소스에 대한 사용자 지정 빌드 시스템을 개발할 수 있습니다. 빌드 시스템은 UWP 앱에 필요한 복잡도 수준에 관계없이 PRI(패키지 리소스 인덱스) 파일을 만들고, 버전 관리하고, 덤프할 수 있습니다(XML로). |
| [MakePri.exe를 사용하여 수동으로 리소스 컴파일](compile-resources-manually-with-makepri.md) | MakePri.exe는 PRI 파일을 만들고 덤프하는 데 사용할 수 있는 명령줄 도구입니다. Microsoft Visual Studio 내에서 MSBuild의 일부로 통합되었지만 패키지를 수동으로 또는 사용자 지정 빌드 시스템을 통해 만드는 데 유용할 수 있습니다. |
| [레거시 앱 또는 게임에서 Windows 10 리소스 관리 시스템 사용](using-mrt-for-converted-desktop-apps-and-games.md) | .NET 또는 Win32 앱 또는 게임을 AppX 패키지로 패키지하면 리소스 관리 시스템을 활용하여 런타임 컨텍스트에 맞는 앱 리소스를 로드할 수 있습니다. 이 문서에서는 해당 기술에 대해 자세히 설명합니다. |

[언어, 배율, 고대비에 대한 타일 및 알림 메시지 지원](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md)도 참조하세요.
