---
ms.assetid: 3a59ff5e-f491-491c-81b1-6aff15886aad
title: 선택형 패키지 및 관련 집합 제작
description: 선택적 패키지에는 주 패키지에 통합할 수 있는 콘텐츠가 포함되어 있습니다. 다운로드 가능한 콘텐츠(DLC)에서 크기 제한을 위해 대형 앱을 분할하거나, 원래 앱과의 분리를 위해 추가 콘텐츠를 배송하는 경우에 유용합니다.
ms.date: 09/30/2018
ms.topic: article
keywords: Windows 10, uwp, 선택적 패키지, 관련 집합, 패키지 확장, visual studio
ms.localizationpriority: medium
ms.openlocfilehash: f62d6c99acc75033403fac7a498308cea6f7d3f8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594028"
---
# <a name="optional-packages-and-related-set-authoring"></a>선택형 패키지 및 관련 집합 제작
선택적 패키지에는 주 패키지에 통합할 수 있는 콘텐츠가 포함되어 있습니다. 다운로드 가능한 콘텐츠(DLC)에서 크기 제한을 위해 대형 앱을 분할하거나, 원래 앱과의 분리를 위해 추가 콘텐츠를 배송하는 경우에 유용합니다.

관련 집합은 선택적 패키지가 확장된 것으로, 주 패키지와 선택적 패키지에서 엄격한 버전 집합을 적용할 수 있도록 해줍니다. 또한 선택적 패키지에서 네이티브 코드 (C++)를 로드할 수 있도록 해줍니다. 

## <a name="prerequisites"></a>필수 구성 요소

- Visual Studio 2017, 버전 15.1
- Windows 10, 버전 1703
- Windows 10, 버전 1703 SDK

모든 최신 개발 도구를 다운로드하는 방법은 [Windows 10용 다운로드 및 도구](https://developer.microsoft.com/windows/downloads)를 참조하세요.

> [!NOTE]
> Microsoft Store 선택적 패키지 및/또는 관련된 집합을 사용 하는 앱을 제출 하려면 권한이 필요 합니다. 선택적 패키지 및 관련된 집합 스토어로 전송 되지 않습니다 하는 경우 파트너 센터의 허가 없이 업무 (LOB) 또는 엔터프라이즈 앱에 사용할 수 있습니다. 선택적 패키지 및 관련 집합을 사용한 앱을 제출할 권한을 얻는 방법은 [Windows 개발자 지원](https://developer.microsoft.com/windows/support)을 참조하세요.

### <a name="code-sample"></a>코드 샘플
이 문서를 읽을 때는 선택적 패키지 및 관련 집합이 Visual Studio 내에서 어떻게 작동하는지 실질적으로 이해할 수 있도록 GitHub의 [선택적 패키지 코드 샘플](https://github.com/AppInstaller/OptionalPackageSample)을 팔로우하는 것이 좋습니다.

## <a name="optional-packages"></a>선택적 패키지
Visual Studio에서 선택적 패키지를 만들려면 다음이 필요합니다.
1. 앱의 했는지 **대상 플랫폼 최소 버전** 로 설정 됩니다. 10.0.15063.0 이상.
2. **주 패키지** 프로젝트에서 `Package.appxmanifest` 파일을 엽니다. "패키징" 탭으로 이동해서 "_" 문자 앞에 있는 모든 것, 즉 **패키지 패밀리 이름**을 기록해 둡니다.
3. **선택적 패키지** 프로젝트에서 `Package.appxmanifest`를 마우스 오른쪽 단추로 클릭해서 **연결 프로그램 > XML (텍스트) 편집기**를 선택합니다.
4. 파일에서 `<Dependencies>` 요소를 찾습니다. 다음을 추가합니다.

```XML
<uap3:MainPackageDependency Name="[MainPackageDependency]"/>
```

2단계에서 `[MainPackageDependency]`를 **패키지 패밀리 이름**으로 교체합니다. 이렇게 하면 **선택적 패키지**가 **주 패키지**에 종속되도록 설정이 됩니다.

1~4단계를 거쳐 패키지 종속성이 설정되고 나면 평소 대로 개발을 계속할 수 있습니다. 선택적 패키지에서 주 패키지로 코드를 로드하고 싶은 경우에는 관련 집합을 빌드해야 합니다. 자세한 내용은 [관련 집합](#related_sets)을 참조하세요.

선택적 패키지가 배포될 때마다 주 패키지를 다시 배포하도록 Visual Studio를 설정할 수 있습니다. Visual Studio에서 빌드 종속성을 설정하려면 다음을 수행해야 합니다.

- 선택적 패키지 프로젝트를 마우스 오른쪽 단추로 클릭하고 **빌드 종속성 > 프로젝트 종속성...** 을 선택합니다.
- 주 패키지 프로젝트를 확인하고 "확인"을 선택합니다. 

이제, F5 키를 누르거나 선택적 패키지 프로젝트를 빌드할 때마다 Visual Studio가 주 패키지 프로젝트를 먼저 빌드합니다. 따라서 주 프로젝트와 선택적 프로젝트가 동기화됩니다.

## 관련 집합<a name="related_sets"></a>

선택적 패키지에서 주 패키지로 코드를 로드하고 싶은 경우에는 관련 집합을 빌드해야 합니다. 관련 집합을 빌드하려면 주 패키지와 선택적 패키지를 긴밀하게 결합해야 합니다. 관련된 집합에 대 한 메타 데이터는 기본 패키지의.appxbundle 또는.msixbundle 파일에 지정 됩니다. Visual Studio는 파일에서 올바른 메타 데이터를 가져오도록 도와줍니다. 관련 집합에 대한 앱 솔루션을 구성하려면 다음 단계를 거칩니다.

1. 주 패키지 프로젝트를 마우스 오른쪽 단추로 클릭하고 **추가 > 새 항목...** 을 선택합니다.
2. 해당 창에서 ".txt"에 대한 설치 템플릿을 검색하고 새 텍스트 파일을 추가 합니다.
> [!IMPORTANT]
> 새 텍스트 파일의 이름은 `Bundle.Mapping.txt`입니다.

3. `Bundle.Mapping.txt`파일에서 선택적 패키지 프로젝트나 외부 패키지로 상대 경로를 지정할 수 있습니다. 샘플인 `Bundle.Mapping.txt` 파일은 다음과 같은 모양을 갖게 됩니다.

```syntax
[OptionalProjects]
"..\ActivatableOptionalPackage1\ActivatableOptionalPackage1.vcxproj"
"..\ActivatableOptionalPackage2\ActivatableOptionalPackage2.vcxproj"

[ExternalPackages]
"..\ActivatableOptionalPackage1\x86\Release\ActivatableOptionalPackage3_1.1.1.0\ ActivatableOptionalPackage3_1.1.1.0.appx"
```

이런 식으로 솔루션이 구성되면, Visual Studio는 관련 집합에 필요한 모든 메타 데이터와 함께 주 패키지에 대한 번들 매니페스트를 생성하게 됩니다. 

선택적 패키지를 참고를 `Bundle.Mapping.txt` 관련된 집합에 대 한 파일은 Windows 10 버전 1703 이상 에서만 작동 합니다. 또한 앱의 대상 플랫폼 최소 버전 이상 10.0.15063.0으로 설정 되어야 합니다.

## 알려진 문제<a name="known_issues"></a>

관련 집합 선택적 프로젝트에 대한 디버깅은 현재 Visual Studio에서 지원되지 않습니다. 이 문제를 해결하기 위해 정품을 배포 및 실행해서(Ctrl + F5) 디버거를 프로세스에 직접 연결할 수 있습니다. 디버거를 연결하려면 Visual Studio에서 "디버그" 메뉴로 이동해서 "프로세스에 연결..."을 선택하고 디버거를 **주 앱 프로세스**에 연결합니다.