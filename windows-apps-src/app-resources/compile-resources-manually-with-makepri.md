---
Description: MakePri.exe는 PRI 파일을 만들고 덤프하는 데 사용할 수 있는 명령줄 도구입니다. Microsoft Visual Studio 내에서 MSBuild의 일부로 통합되었지만 패키지를 수동으로 또는 사용자 지정 빌드 시스템을 통해 만드는 데 유용할 수 있습니다.
title: MakePri.exe를 사용하여 수동으로 리소스 컴파일
template: detail.hbs
ms.date: 10/23/2017
ms.topic: article
keywords: Windows 10, UWP, 리소스, 이미지, 자산, MRT, 한정자
ms.localizationpriority: medium
ms.openlocfilehash: c23eced05d7539c869a40db1f2b641282b75f503
ms.sourcegitcommit: 963316e065cf36c17b6360c3f89fba93a1a94827
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82868895"
---
# <a name="compile-resources-manually-with-makepriexe"></a>MakePri.exe를 사용하여 수동으로 리소스 컴파일

MakePri.exe는 PRI 파일을 만들고 덤프하는 데 사용할 수 있는 명령줄 도구입니다. Microsoft Visual Studio 내에서 MSBuild의 일부로 통합되었지만 패키지를 수동으로 또는 사용자 지정 빌드 시스템을 통해 만드는 데 유용할 수 있습니다.

> [!NOTE]
> MakePri .exe는 Windows 소프트웨어 개발 키트를 설치 하는 동안 **UWP 관리 앱에 대 한 Windows SDK** 옵션을 선택 하면 설치 됩니다. 경로 뿐만 아니라 `%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe` 다른 아키텍처에 대해 이름이 지정 된 폴더에도 설치 됩니다. `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`)을 입력합니다.

## <a name="in-this-section"></a>섹션 내용
|항목|Description|
|-|-|
| [MakePri.exe 명령줄 옵션](makepri-exe-command-options.md) | Makepri에는,,, 및 명령 집합이 있습니다 `createconfig` `dump` `new` `resourcepack` `versioned` . 이 항목에서는 사용을 위한 명령줄 옵션에 대해 자세히 설명 합니다. |
| [MakePri.exe 구성 파일](makepri-exe-configuration.md) | 이 항목에서는 MakePri .exe XML 구성 파일의 스키마에 대해 설명 합니다. |
| [MakePri.exe 형식별 인덱서](makepri-exe-format-specific-indexers.md) | 이 항목에서는 MakePri 도구에서 리소스의 인덱스를 생성 하는 데 사용 하는 형식별 인덱서를 설명 합니다. |

## <a name="makepriexe-command-line-options"></a>MakePri.exe 명령줄 옵션

Makepri에는,,, 및 명령 집합이 있습니다 `createconfig` `dump` `new` `resourcepack` `versioned` . 사용에 대 한 자세한 내용은 [Makepri 명령줄 옵션](makepri-exe-command-options.md)을 참조 하세요.

## <a name="makepriexe-configuration"></a>MakePri .exe 구성

PRI XML 구성 파일은 인덱싱되는 방법 및 리소스를 나타냅니다. 구성 XML의 스키마는 [Makepri .exe 구성](makepri-exe-configuration.md)에 설명 되어 있습니다.

## <a name="format-specific-indexers"></a>서식 지정 인덱서

MakePri는 일반적으로 `new` , `versioned` 및 `resourcepack` 옵션과 함께 사용 됩니다. 이러한 경우 원본 파일을 인덱싱하고 리소스의 인덱스를 생성 합니다. MakePri는 다양 한 개별 인덱서를 사용 하 여 리소스에 대 한 다른 소스 리소스 파일이 나 컨테이너를 읽습니다. 가장 간단한 인덱서는 폴더 인덱서로, 또는 이미지와 같은 리소스에 대 한 폴더의 내용을 인덱싱합니다 `.jpg` `.png` . 자세한 내용은 [Makepri .exe 형식 관련 인덱서](makepri-exe-format-specific-indexers.md)를 참조 하세요.

## <a name="makepriexe-warnings-and-error-messages"></a>MakePri 경고 및 오류 메시지

### <a name="resources-found-for-languages-languages-but-no-resources-found-for-default-languages-languages-change-the-default-language-or-qualify-resources-with-the-default-language"></a>언어 ' <언어 > '에 대 한 리소스가 있지만 기본 언어에 대 한 리소스를 찾을 수 없습니다. ' <언어 > '. 기본 언어를 변경 하거나 기본 언어로 리소스를 한정 합니다.

이 경고는 MakePri 또는 MSBuild가 언어 한정자로 표시 된 것 처럼 보이지만 기본 언어에 대 한 후보를 찾을 수 없는 경우 지정 된 명명 된 리소스에 대 한 파일 또는 문자열 리소스를 검색 하는 경우에 표시 됩니다. 파일 및 폴더 이름에 한정자를 사용 하는 프로세스는 [언어, 크기 조정 및 기타 한정자에 맞게 리소스를 조정](tailor-resources-lang-scale-contrast.md)하는 방법에 설명 되어 있습니다. 파일이 나 폴더에 언어 이름이 있을 수 있지만 정확한 기본 언어에 대 한 정규화 된 리소스는 검색 되지 않습니다. 예를 들어, 프로젝트에서 "en-us"를 기본 언어로 사용 하 고 "de/로고 .png" 라는 파일이 있지만 기본 언어 "en-us"로 표시 된 파일이 없는 경우이 경고가 표시 됩니다. 이 경고를 제거 하려면 파일 또는 문자열 리소스를 기본 언어로 정규화 해야 합니다. 그렇지 않으면 기본 언어가 변경 되어야 합니다. Visual Studio에서 솔루션을 열어 기본 언어를 변경 하려면를 엽니다 `Package.appxmanifest` . 응용 프로그램 탭에서 기본 언어가 적절 하 게 설정 되었는지 확인 합니다 (예: "en" 또는 "en-us").

### <a name="no-default-or-neutral-resource-given-for-resource-identifier-the-application-may-throw-an-exception-for-certain-user-configurations-when-retrieving-the-resources"></a>' '에 대 한 기본 또는 중립 리소스가 제공 되지 않았습니다 <resource identifier> . 응용 프로그램은 리소스를 검색할 때 특정 사용자 구성에 대해 예외를 throw 할 수 있습니다.

이 경고는 MakePri 또는 MSBuild에서 리소스가 명확 하지 않은 언어 한정자로 표시 된 것으로 보이는 파일이 나 리소스를 검색 하는 경우에 표시 됩니다. 한정자가 있지만 런타임에 해당 리소스 식별자에 대 한 특정 리소스 후보가 반환 될 수 있다는 보장이 없습니다. 기본값 이거나 항상 사용자의 컨텍스트와 일치 하는 특정 언어, homeregion 또는 기타 한정자의 리소스 후보를 찾을 수 없는 경우이 경고가 표시 됩니다. 런타임에 사용자의 언어 기본 설정 또는 홈 위치 (**설정**  >  **시간 & 언어**& 언어)와 같은 특정 사용자 구성의 경우  >  **Region & language**리소스를 검색 하는 데 사용 되는 api가 예기치 않은 예외를 throw 할 수 있습니다. 이 경고를 제거 하려면 프로젝트의 기본 언어 또는 전역 홈 지역 (homeregion-001)의 리소스와 같은 기본 리소스를 제공 해야 합니다.

## <a name="using-makepriexe-in-a-build-system"></a>빌드 시스템에서 MakePri 사용

빌드 시스템은 빌드하는 `new` `versioned` `resourcepack` 프로젝트의 형식에 따라 makepri .exe, 또는 명령을 사용 해야 합니다. 새 PRI 파일을 만드는 빌드 시스템은 명령을 사용 해야 합니다 `new` . 반복을 통한 내부 오프셋과의 호환성을 보장 해야 하는 빌드 시스템은 명령을 사용할 수 있습니다 `versioned` . 추가 리소스를 포함 하는 PRI 파일을 만들고 해당 변형의 새 리소스가 추가 되지 않도록 유효성 검사를 수행 해야 하는 경우 명령을 사용 해야 합니다 `resourcepack` .

인덱싱된 원본 파일을 명시적으로 제어 해야 하는 빌드 시스템은 폴더를 인덱싱하지 않고 ResFiles 인덱서를 사용할 수 있습니다. 또한 빌드 시스템은 다양 한 [형식별 인덱서](makepri-exe-format-specific-indexers.md) 를 사용 하 여 여러 인덱스 패스를 사용 하 여 단일 PRI 파일을 생성할 수 있습니다.

또한 빌드 시스템은 PRI 형식별 인덱서를 사용 하 여 클래스 라이브러리, 어셈블리, Sdk 및 Dll 등의 다른 구성 요소에서 패키지의 PRI에 미리 작성 된 PRI 파일을 추가할 수 있습니다.

다른 구성 요소, 클래스 라이브러리, 어셈블리, Dll 및 Sdk에 대해 PRI 파일을 작성 하는 경우 **Initialpath** 구성을 사용 하 여 구성 요소 리소스에 포함 된 앱과 충돌 하지 않는 고유한 하위 리소스 맵을 갖도록 해야 합니다.

## <a name="related-topics"></a>관련 항목
* [MakePri.exe 명령줄 옵션](makepri-exe-command-options.md)
* [MakePri .exe 구성](makepri-exe-configuration.md)
* [MakePri.exe 형식별 인덱서](makepri-exe-format-specific-indexers.md)
* [언어, 크기 조정 및 기타 한정자에 맞게 리소스를 조정 합니다.](tailor-resources-lang-scale-contrast.md)
