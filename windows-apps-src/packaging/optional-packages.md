---
author: laurenhughes
ms.assetid: 3a59ff5e-f491-491c-81b1-6aff15886aad
title: 선택적 패키지 및 관련 집합 제작
description: 선택적 패키지에는 주 패키지에 통합할 수 있는 콘텐츠가 포함되어 있습니다. 다운로드 가능한 콘텐츠(DLC)에서 크기 제한을 위해 대형 앱을 분할하거나, 원래 앱과의 분리를 위해 추가 콘텐츠를 배송하는 경우에 유용합니다.
ms.author: lahugh
ms.date: 04/05/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 선택적 패키지, 관련된 집합, 패키지 확장, visual studio
ms.localizationpriority: medium
ms.openlocfilehash: d66a511211396190393e31bfd553149a1e89fad0
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/13/2018
ms.locfileid: "406080"
---
# <a name="optional-packages-and-related-set-authoring"></a>선택적 패키지 및 관련 집합 제작
선택적 패키지에는 주 패키지에 통합할 수 있는 콘텐츠가 포함되어 있습니다. 이 다운로드 가능한 콘텐츠 (DLC) 나눈 크기 제한에 대 한 대규모 응용 프로그램에 대 한 유용한 또는 추가 콘텐츠를 전달 하는 것에 대 한 원본 응용 프로그램에서 분리 합니다.

관련된 집합은 선택적 패키지의 확장--하면 주 및 선택적 패키지 간에 버전의 집합을 엄격 하 게 적용할 수 있도록 합니다. 또한 선택적 패키지에서 네이티브 코드 (c + +)를 로드 하기 수 있습니다. 

## <a name="prerequisites"></a>필수 구성 요소

- Visual Studio 2017, 15.1 버전
- Windows 10 버전 1703
- Windows 10, 버전 1703 sdk (영문)

가져올 모든 최신 개발 도구를 [다운로드 및 Windows 10에 대 한 도구를](https://developer.microsoft.com/windows/downloads)참조 하십시오.

> [!NOTE]
> Microsoft 저장소에 선택적 패키지 및/또는 관련된 집합을 사용 하는 앱을 전송 하려면 권한이 필요 합니다. Microsoft Store에 제출되지 않은 경우에는 선택적 패키지 및 관련 집합을 개발자 센터 권한 없이 기간 업무(LOB)나 엔터프라이즈 앱에서 사용할 수 있습니다. 선택적 패키지 및 관련 집합을 사용한 앱을 제출할 권한을 얻는 방법은 [Windows 개발자 지원](https://developer.microsoft.com/windows/support)을 참조하세요.

### <a name="code-sample"></a>코드 샘플
이 문서를 보는 동안 방법을 선택적 패키지에 대 한 실무 이해에 대 한 [선택적 패키지 코드 예제](https://github.com/AppInstaller/OptionalPackageSample) 와 함께 GitHub에 따라 Visual Studio 내에서 집합 작업 관련 하는 것이 좋습니다.

## <a name="optional-packages"></a>선택적 패키지
Visual Studio에서 선택적 패키지를 만들려면 해야 합니다.
1. 앱의 **대상 플랫폼 최소 버전** 을 설정 되었는지 확인: 10.0.15063.0 합니다.
2. **주 패키지** 프로젝트를 엽니다는 `Package.appxmanifest` 파일입니다. "패키징" 탭으로 이동한 후 기록해둡니다 **패키지 제품군 이름**"_" 문자 앞에 있는 모든 항목은 합니다.
3. 사용자가 **선택적 패키지** 프로젝트에서 마우스 오른쪽 단추로 클릭은 `Package.appxmanifest` 을 선택 하 고 **사용 하 여 열기 > XML (텍스트) 편집기**합니다.
4. 찾습니다는 `<Dependencies>` 파일의 요소입니다. 다음을 추가 합니다.

```XML
<uap3:MainPackageDependency Name="[MainPackageDependency]"/>
```

바꾸기 `[MainPackageDependency]` 2 단계에서에서 **패키지 제품군 이름** 입니다. 이 **선택적 패키지** **주 패키지**에 종속 인지를 지정 합니다.

-4 단계 1에서 설정 하 여 패키지 의존 관계를 만든 후 계속할 수 있습니다 개발 (영문) 평소와 같이. 주 패키지에 패키지 옵션에서 코드를 로드 하려는 경우에 관련된 집합을 구축 해야 합니다. 자세한 내용은 [관련 설정](#related_sets) 섹션을 참조 하십시오.

선택적 패키지를 배포 하는 각 시간 주 패키지를 다시 배포 하려면 visual Studio는 구성할 수 있습니다. Visual Studio에서 빌드 의존 관계를 설정 하려면 다음을 수행 해야 합니다.

- 선택적 패키지 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **종속성 빌드 > 프로젝트 의존 관계...**
- 주 패키지 프로젝트를 확인 하 고 "확인"을 선택 합니다. 

이제 f5 키를 입력 하거나 선택적 패키지 프로젝트를 빌드할 때마다 Visual Studio 작성 주 패키지 프로젝트 먼저 합니다. 이렇게 하면 주 프로젝트 및 선택적 프로젝트 동기화 됩니다.

## 관련된 설정<a name="related_sets"></a>

주 패키지에는 선택적 패키지에서 코드를 로드 하려는 경우에 관련된 집합을 구축 해야 합니다. 관련된 된 집합을 작성 하려면 주 패키지 및 패키지 옵션 해야 수 밀접 하 게 관련 됩니다. 관련된 집합에 대 한 메타 데이터에 지정 된는 `.appxbundle` 주 패키지의 파일입니다. Visual Studio를 사용 하면 파일에서 올바른 메타 데이터를 가져올 수 있습니다. 관련된 집합에 대 한 앱의 솔루션을 구성 하려면 다음 단계를 사용 합니다.

1. 주 패키지 프로젝트를 마우스 오른쪽 단추로 클릭, 선택 **추가 > 새 항목...**
2. 창에서 ".txt"에 대 한 설치 된 서식 파일을 검색 하 고 새 텍스트 파일을 추가 합니다.
> [!IMPORTANT]
> 새 텍스트 파일을 지정 해야 합니다: `Bundle.Mapping.txt`합니다.
3. `Bundle.Mapping.txt` 파일 패키지 옵션 프로젝트 또는 외부 패키지 상대 경로 지정 합니다. 샘플 `Bundle.Mapping.txt` 파일 형식은 다음과 같습니다.

```syntax
[OptionalProjects]
"..\ActivatableOptionalPackage1\ActivatableOptionalPackage1.vcxproj"
"..\ActivatableOptionalPackage2\ActivatableOptionalPackage2.vcxproj"

[ExternalPackages]
"..\ActivatableOptionalPackage1\x86\Release\ActivatableOptionalPackage3_1.1.1.0\ ActivatableOptionalPackage3_1.1.1.0.appx"
```

솔루션에는 이러한 방식으로 구성 되 면 Visual Studio와 관련 된 집합에 필요한 메타 데이터의 모든 주요 패키지에 대 한 번들 매니페스트를 만들어집니다. 

패키지 옵션을 선택 하는 내용의 `Bundle.Mapping.txt` 관련된 집합에 대 한 파일 Windows 10, 1703 버전에만 적용 됩니다. 또한 앱의 대상 플랫폼 Min 버전 10.0.15063.0로 설정 해야 합니다.

## 알려진 문제<a name="known_issues"></a>

현재 Visual Studio에서 관련 된 집합 선택 사항 프로젝트를 디버깅 지원 되지 않습니다. 이 문제를 해결 하려면 배포 하 고 (Ctrl + f5 키) 정품 인증을 실행 및 수동으로 프로세스에 디버거를 연결할 수 있습니다. 디버거를 연결할 "연결을 프로세스...", Visual Studio에서 "Debug" 메뉴를 이동 선택한 **주 응용 프로그램 프로세스**에 디버거를 연결 합니다.