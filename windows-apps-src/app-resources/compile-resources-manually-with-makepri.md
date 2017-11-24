---
author: stevewhims
Description: "MakePri.exe는 PRI 파일을 만들고 덤프하는 데 사용할 수 있는 명령줄 도구입니다. Microsoft Visual Studio 내에 MSBuild의 일부로 통합되어 있지만 수동으로 또는 사용자 지정 빌드 시스템으로 패키지를 만드는 데 도움이 될 수 있습니다."
title: "MakePri.exe를 사용하여 수동으로 리소스 컴파일"
template: detail.hbs
ms.author: stwhi
ms.date: 10/23/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, 리소스, 이미지, 자산, MRT, 한정자"
localizationpriority: medium
ms.openlocfilehash: 16d2a270a69497bc66f7b17109bc28b062f14b5e
ms.sourcegitcommit: d0c93d734639bd31f264424ae5b6fead903a951d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/03/2017
---
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

# <a name="compile-resources-manually-with-makepriexe"></a>MakePri.exe를 사용하여 수동으로 리소스 컴파일

MakePri.exe는 PRI 파일을 만들고 덤프하는 데 사용할 수 있는 명령줄 도구입니다. Microsoft Visual Studio 내에 MSBuild의 일부로 통합되어 있지만 수동으로 또는 사용자 지정 빌드 시스템으로 패키지를 만드는 데 도움이 될 수 있습니다.

## <a name="makepriexe-command-line-options"></a>MakePri.exe 명령줄 옵션

MakePri.exe에는 `createconfig`, `dump`, `new`, `resourcepack` 및 `versioned` 명령 집합이 있습니다. 이 사용에 대한 자세한 내용은 [MakePri.exe 명령줄 옵션](makepri-exe-command-options.md)을 참조하세요.

## <a name="makepriexe-configuration"></a>MakePri.exe 구성

PRI XML 구성 파일은 인덱싱하는 리소스와 그 방법을 제어합니다. 구성 XML의 스키마는 [MakePri.exe 구성](makepri-exe-configuration.md)에 설명되어 있습니다.

## <a name="format-specific-indexers"></a>형식별 인덱서

MakePri.exe는 일반적으로 `new`, `versioned`, `resourcepack` 옵션과 함께 사용됩니다. 이러한 경우 원본 파일을 인덱싱하여 리소스 인덱스를 생성합니다. MakePri.exe는 다른 소스 리소스 파일 또는 리소스 컨테이너를 읽기 위해 다양한 개별 인덱서를 사용합니다. 가장 단순한 인덱서는 폴더 인덱서이며 `.jpg`또는 `.png` 이미지 등의 리소스에 대한 폴더의 콘텐츠를 인덱싱합니다. 자세한 내용은 [MakePri.exe 형식별 인덱서](makepri-exe-format-specific-indexers.md)를 참조하세요.

## <a name="makepriexe-warnings-and-error-messages"></a>MakePri.exe 경고 및 오류 메시지

```
Resources found for language(s) '<language(s)>' but no resources found for default language(s): '<language(s)>'. Change the default language or qualify resources with the default language.
```

MakePri.exe 또는 MSBuild가 언어 한정자가 있는 것으로 표시되지만 기본 언어에 대한 후보를 찾을 수 없는 지정된 명명된 리소스에 대한 파일 또는 문자열 리소스를 발견했을 때 위의 경고가 표시됩니다. 파일 및 폴더 이름에 한정자를 사용하는 프로세스는 [언어, 규모 및 기타 한정자에 맞게 리소스 조정](tailor-resources-lang-scale-contrast.md)에 설명되어 있습니다. 파일 또는 폴더에는 언어 이름이 있을 수 있지만 정확한 기본 언어에 대해 정규화된 리소스가 검색되지 않습니다. 예를 들어 프로젝트가 "en-US"를 기본 언어로 사용하고 "de/logo.png"라는 파일이 있지만 기본 언어인 "en-US"가 표시된 파일이 없는 경우 이 경고가 표시됩니다. 이 경고를 제거하기 위해 파일 또는 문자열 리소스를 기본 언어로 정규화하거나 기본 언어를 변경해야 합니다. 기본 언어를 변경하려면 Visual Studio에서 솔루션을 연 채로 `Package.appxmanifest`를 엽니다. 응용 프로그램 탭에서 기본 언어가 제대로 설정되어 있는지 확인합니다(예: "en" 또는 "en-US").

```
No default or neutral resource given for '<resource identifier>'. The application may throw an exception for certain user configurations when retrieving the resources.
```

MakePri.exe 또는 MSBuild가 리소스가 명확하지 않은 언어 한정자가 표시된 것으로 보이는 파일 또는 리소스를 발견했을 때 위의 경고가 표시됩니다. 한정자가 있지만 실행 시 특정 리소스 후보가 해당 리소스 식별자에 대해 반환될 수 있다는 보장이 없습니다. 특정 언어, 소속 지역, 또는 그 외 한정자에 대해 기본값이거나 사용자의 컨텍스트에 항상 일치하는 리소스 후보를 찾을 수 없는 경우 이 경고가 표시됩니다. 런타임 시 사용자의 언어 기본 설정 또는 홈 위치와 같은 특정 사용자 구성에 대해(**설정** > **시간 및 언어** > **지역 및 언어**), 리소스를 검색하기 위해 사용되는 API는 예기치 않은 예외가 발생할 수 있습니다. 이 경고를 제거하려면 프로젝트의 기본 언어 또는 전 세계 소속 지역(homeregion-001)의 리소스와 같은 기본 리소스가 제공되어야 합니다.

## <a name="using-makepriexe-in-a-build-system"></a>빌드 시스템에서 MakePri.exe 사용

빌드 시스템은 빌드되는 프로젝트 유형에 따라 MakePri.exe `new`, `versioned`, 또는 `resourcepack` 명령을 사용해야 합니다. 새 PRI 파일을 만드는 빌드 시스템은 `new` 명령을 사용해야 합니다. 반복을 통해 내부 오프셋의 호환성을 사용할 수 있도록 해야 하는 시스템 빌드는 `versioned` 명령을 사용할 수 있습니다. 추가 리소스 변형을 포함하고 새 리소스가 해당 변형에 추가되지 않은지 유효성 검사를 해야 하는 PRI 파일을 만들어야 하는 빌드 시스템은 `resourcepack` 명령을 사용해야 합니다.

인덱스되는 원본 파일에 대해 명시적인 제어가 필요한 빌드 시스템은 폴더를 인덱싱하는 대신에 ResFiles 인덱서를 사용할 수 있습니다. 빌드 시스템은 다른 [형식별 인덱서](makepri-exe-format-specific-indexers.md)로 여러 인덱스 전달을 사용하여 단일 PRI 파일을 생성할 수도 있습니다.

또한 빌드 시스템은 PRI 형식별 인덱서를 사용하여 클래스 라이브러리, 어셈블리, SDK 및 DLL과 같은 다른 구성 요소에서 미리 작성된 PRI 파일을 패키지에 대한 PRI에 추가할 수도 있습니다.

다른 구성 요소, 클래스 라이브러리, 어셈블리, DLL 및 SDK에 대해 PRI 파일을 작성할 때 구성 요소 리소스가 포함된 앱과 충돌하지 않는 자체 하위 리소스 맵을 가지도록 **initialPath** 구성을 사용해야 합니다.

## <a name="related-topics"></a>관련 항목

* [MakePri.exe 명령줄 옵션](makepri-exe-command-options.md)
* [MakePri.exe 구성](makepri-exe-configuration.md)
* [MakePri.exe 형식별 인덱서](makepri-exe-format-specific-indexers.md)
* [언어, 규모 및 기타 한정자에 맞게 리소스 조정](tailor-resources-lang-scale-contrast.md)
