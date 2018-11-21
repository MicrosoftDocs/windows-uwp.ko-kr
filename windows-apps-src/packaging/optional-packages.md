---
author: laurenhughes
ms.assetid: 3a59ff5e-f491-491c-81b1-6aff15886aad
title: 선택적 패키지 및 관련 집합 제작
description: 선택적 패키지에는 주 패키지에 통합할 수 있는 콘텐츠가 포함되어 있습니다. 다운로드 가능한 콘텐츠(DLC)에서 크기 제한을 위해 대형 앱을 분할하거나, 원래 앱과의 분리를 위해 추가 콘텐츠를 배송하는 경우에 유용합니다.
ms.author: lahugh
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, uwp, 선택적 패키지, 관련된 집합, 패키지 확장, visual studio
ms.localizationpriority: medium
ms.openlocfilehash: c21b84467151493836186d1d55ab5e4e542899ec
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7559993"
---
# <a name="optional-packages-and-related-set-authoring"></a>선택적 패키지 및 관련 집합 제작
선택적 패키지에는 주 패키지에 통합할 수 있는 콘텐츠가 포함되어 있습니다. 이러한 크기 제한에 대 한 대규모 앱 분할 다운로드 가능한 콘텐츠 (DLC)에 유용 하거나 추가 콘텐츠를 전달 하는 것에 대 한 원래 앱에서 분리 합니다.

관련된 집합은 선택적 패키지의 확장-주 및 선택적 패키지 간에 엄격한 버전 집합을 적용할 수 있도록 합니다. 또한 선택적 패키지에서 네이티브 코드 (c + +)를 로드할 수 수 있습니다. 

## <a name="prerequisites"></a>사전 요구 사항

- Visual Studio 2017 버전 15.1
- Windows 10 버전 1703
- Windows 10, 버전 1703, SDK

모든 최신 개발 도구를 가져오려면 [Windows 10 용 다운로드 및 도구를](https://developer.microsoft.com/windows/downloads)참조 하세요.

> [!NOTE]
> Microsoft Store에 선택적 패키지 및 관련된 집합을 사용 하는 앱을 제출 하려면 권한이 필요 합니다. 선택적 패키지 및 관련된 집합을 스토어에 제출 되지 않은 경우 파트너 센터 권한 없이 줄의 lob (기간 업무) 나 엔터프라이즈 앱에 사용할 수 있습니다. 선택적 패키지 및 관련 집합을 사용한 앱을 제출할 권한을 얻는 방법은 [Windows 개발자 지원](https://developer.microsoft.com/windows/support)을 참조하세요.

### <a name="code-sample"></a>코드 샘플
이 문서를 읽고 하는 동안에 어떻게 선택적 패키지 실습 이해 하기 위해 GitHub의 [코드 샘플 선택적 패키지와](https://github.com/AppInstaller/OptionalPackageSample) 함께 수행 하 고 Visual Studio 내에서 설정 작업 관련 좋습니다.

## <a name="optional-packages"></a>선택적 패키지
Visual Studio에서 선택적 패키지를 만들려면 해야 합니다.
1. 앱의 **대상 플랫폼 최소 버전** 설정 되어 있는지 확인: 10.0.15063.0 합니다.
2. **주 패키지** 프로젝트에서 열고는 `Package.appxmanifest` 파일입니다. "패키징" 탭으로 이동한 기록해 둡니다 **패키지 패밀리 이름**에 "_" 문자 하기 전에 모든 것입니다.
3. **선택적 패키지** 프로젝트에서 마우스 오른쪽 단추로 클릭 합니다 `Package.appxmanifest` 선택 **다른 프로그램으로 열기 > XML (텍스트) 편집기**합니다.
4. 찾아는 `<Dependencies>` 파일에서 요소입니다. 다음을 추가 합니다.

```XML
<uap3:MainPackageDependency Name="[MainPackageDependency]"/>
```

대체 `[MainPackageDependency]` 2 단계에서에서 **패키지 패밀리 이름** 으로 합니다. 이 **선택적 패키지가** **주 패키지**에 종속 되어 있는지를 지정 합니다.

1 단계에서에서 4를 통해 설정 패키지 종속성에 있으면 계속 수 있습니다 개발 평소와 같이. 주 패키지에 선택적 패키지에서 코드를 로드 하려는 경우에 관련된 집합을 작성 해야 합니다. 자세한 내용은 [관련 설정](#related_sets) 섹션을 참조 하세요.

Visual Studio는 다시 선택적 패키지를 배포할 때마다 주 패키지를 배포 하도록 구성할 수 있습니다. Visual Studio에서 빌드 종속성을 설정 하려면 다음을 수행 해야 합니다.

- 선택적 패키지 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **종속성 빌드 > 프로젝트 종속성...**
- 주 패키지 프로젝트를 확인 하 고 "확인"를 선택 합니다. 

이제 F5, 선택적 패키지 프로젝트를 빌드할 때마다 Visual Studio가 프로젝트를 빌드합니다 주 패키지 먼저. 이렇게 하면 기본 프로젝트 및 선택적 프로젝트 동기화 됩니다.

## 관련된 집합<a name="related_sets"></a>

선택적 패키지에서 코드를 주 패키지 로드 하려는 경우 관련된 집합을 작성 해야 합니다. 관련된 집합을 작성 하려면 주 패키지와 선택적 패키지 긴밀 하 게 결합 해야 합니다. 관련된 집합에 대 한 메타 데이터는 주 패키지의.msixbundle 또는.appxbundle 파일에 지정 됩니다. Visual Studio를 사용 하면 파일의 올바른 메타 데이터를 얻을 수 있습니다. 관련된 집합에 대 한 앱의 솔루션을 구성 하려면 다음 단계를 사용 합니다.

1. 주 패키지 프로젝트를 마우스 오른쪽 단추로 클릭 **추가 > 새 항목...**
2. 창에서 ".txt"에 대 한 설치 된 템플릿을 검색 하 고 새 텍스트 파일을 추가 합니다.
> [!IMPORTANT]
> 새 텍스트 파일 이름: `Bundle.Mapping.txt`.
3. 에 `Bundle.Mapping.txt` 파일을 외부 패키지 또는 선택적 패키지 프로젝트에 대 한 상대 경로 지정 합니다. 샘플 `Bundle.Mapping.txt` 파일은 다음과 같이 보입니다.

```syntax
[OptionalProjects]
"..\ActivatableOptionalPackage1\ActivatableOptionalPackage1.vcxproj"
"..\ActivatableOptionalPackage2\ActivatableOptionalPackage2.vcxproj"

[ExternalPackages]
"..\ActivatableOptionalPackage1\x86\Release\ActivatableOptionalPackage3_1.1.1.0\ ActivatableOptionalPackage3_1.1.1.0.appx"
```

솔루션이 방식이으로 구성 되 면 Visual Studio는 번들 매니페스트를 주 패키지에 대 한 모든 관련된 집합에 대 한 필수 메타 데이터를 사용 하 여 만듭니다. 

선택적 패키지를 참고는 `Bundle.Mapping.txt` Windows 10 버전 1703에서 관련된 집합에 대 한 파일 에서만 작동 합니다. 또한 앱의 대상 플랫폼 최소 버전 10.0.15063.0를 설정 해야 합니다.

## 알려진 문제<a name="known_issues"></a>

현재 Visual Studio에서 관련된 집합의 선택적 프로젝트 디버깅 지원 되지 않습니다. 이 문제를 해결 하려면 배포 및 시작 활성화 (Ctrl + F5) 및 프로세스에 디버거를 수동으로 연결할 수 있습니다. 디버거를 연결할 "연결을 프로세스...", "Debug" 메뉴를 Visual Studio에서 선택한 **메인 앱 프로세스**에 디버거를 연결 합니다.