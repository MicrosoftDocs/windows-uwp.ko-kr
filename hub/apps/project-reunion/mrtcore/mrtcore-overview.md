---
description: MRT.LOG 핵심 구성 요소 개요 및 응용 프로그램 리소스를 로드 하는 방법 (프로젝트 리유니언)
title: MRT.LOG Core 소개 (프로젝트 리유니언)
ms.topic: article
ms.date: 12/11/2020
keywords: MRT.LOG, MRTCore, pri, makepri, 리소스, 리소스 로드
ms.author: hickeys
author: hickeys
ms.localizationpriority: medium
ms.openlocfilehash: 0039d2a585f850bd7a15afc619d3500c092de50b
ms.sourcegitcommit: cddc595969c658ce30fbc94ded92db4a8ad1bf66
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2020
ms.locfileid: "97349387"
---
# <a name="introduction-to-mrt-core-project-reunion"></a>MRT.LOG Core 소개 (프로젝트 리유니언)

MRT.LOG Core는 [프로젝트 리유니언](../index.md)의 일부로 배포 되는 최신 Windows [리소스 관리 시스템](/windows/uwp/app-resources/resource-management-system) 의 간소화 된 버전입니다.

MRT.LOG Core에는 빌드 시간과 런타임 기능이 모두 포함 되어 있습니다. 빌드 시 시스템은 앱과 함께 패키지 되는 리소스의 모든 다른 변형에 대 한 인덱스를 만듭니다. 이 인덱스는 패키지 리소스 인덱스 또는 PRI 이며 앱 패키지에도 포함 됩니다. 런타임에 시스템은 적용 되는 사용자 및 컴퓨터 설정을 검색 하 고, PRI의 정보를 확인 하 고, 해당 설정에 가장 일치 하는 리소스를 자동으로 로드 합니다.

## <a name="package-resource-index-pri-file"></a>PRI (패키지 리소스 인덱스) 파일

모든 앱 패키지는 앱의 리소스에 대 한 이진 인덱스를 포함 해야 합니다. 이 인덱스는 빌드 시에 생성 되 고 하나 이상의 리소스 (. resw) 파일에 포함 됩니다.

Resources.resw 파일에는 실제 문자열 리소스와 패키지의 다양 한 파일을 참조 하는 인덱싱된 파일 경로 집합이 포함 되어 있습니다.
패키지에는 일반적으로 리소스. resources.resw 라는 언어별로 단일 resources.resw 파일이 포함 되어 있습니다. 각 패키지의 루트에 있는 리소스. resw 파일은 ResourceManager가 인스턴스화될 때 자동으로 로드 됩니다.

각 resources.resw 파일에는 리소스 맵 이라고 하는 명명 된 리소스 컬렉션이 포함 되어 있습니다. 패키지에서 resources.resw 파일이 로드 되 면 리소스 맵 이름이 패키지 id 이름과 일치 하는지 확인 됩니다.

Resw 파일은 데이터만 포함 하므로 PE (이식 가능한 실행 파일) 형식을 사용 하지 않습니다. 특별히 데이터 전용으로 설계 되었습니다.

## <a name="using-mrt-core-to-access-app-resources"></a>MRT.LOG Core를 사용 하 여 앱 리소스 액세스

### <a name="resource-loader-basic-functionality"></a>리소스 로더 (기본 기능)

프로그래밍 방식으로 앱 리소스에 액세스 하는 가장 간단한 방법은 [Microsoft ApplicationModel .resources](/windows/winui/api/microsoft.applicationmodel.resources) 네임 스페이스와 resourceloader 클래스를 사용 하는 것입니다. ResourceLoader는 리소스 파일, 참조 된 라이브러리 또는 기타 패키지 집합에서 문자열 리소스에 대 한 기본 액세스를 제공 합니다.

### <a name="resource-manager-advanced-functionality"></a>리소스 관리자 (고급 기능)

[ResourceManager](/windows/winui/api/microsoft.applicationmodel.resources.resourcemanager) 클래스는 열거 및 검사와 같은 리소스에 대 한 추가 정보를 제공 합니다. 이는 **Resourceloader** 클래스에서 제공 하는 것 이상으로 진행 됩니다.

[ResourceCandidate](/windows/winui/api/microsoft.applicationmodel.resources.resourcecandidate) 개체는 단일 구체적 리소스 값과 해당 한정자 (예: 영어의 경우 "Hello World") 또는 "logo.scale-100.jpg"를 100 해상도와 관련 된 정규화 된 이미지 리소스로 나타냅니다.

앱에 사용할 수 있는 리소스는 계층 컬렉션에 저장 되며,이는 [resourcemap](/windows/winui/api/microsoft.applicationmodel.resources.resourcemap) 를 사용 하 여 액세스할 수 있습니다. **ResourceManager** 클래스를 사용 하면 앱에서 사용 하는 다양 한 최상위 **resourcemap** 에 대 한 액세스를 제공 하며, 앱에 대 한 다양 한 패키지에 해당 합니다. [ResourceManager](/windows/winui/api/microsoft.applicationmodel.resources.resourcemanager.mainresourcemap) 는 현재 앱 패키지의 리소스 맵에 해당 하며 참조 된 프레임 워크 패키지를 제외 합니다. 각 **resourcemap** 패키지의 매니페스트에 지정 된 패키지 이름으로 이름이 지정 됩니다. **Resourcemap** 하위 트리 ([resourcemap] 참조)로, **namedresource** 개체를 추가로 포함 합니다. 하위 트리는 일반적으로 리소스를 포함 하는 리소스 파일에 해당 합니다.

참고 리소스 식별자는 URI 의미 체계가 적용 되는 URI (Uniform Resource Identifier) 조각으로 취급 됩니다. 예를 들어 `GetValue("Caption%20")` 는로 처리 됩니다 `GetValue("Caption ")` . 리소스 식별자의 "?" 또는 "#"은 리소스 경로 평가를 종료 하므로 사용 하지 마십시오. 예를 들어 "MyResource? 3"은 "MyResource"로 처리 됩니다.

**ResourceManager** 는 앱의 문자열 리소스에 대 한 액세스를 지원할 뿐만 아니라 다양 한 파일 리소스를 열거 하 고 검사 하는 기능도 유지 관리 합니다. 파일 및 파일 내에서 시작 되는 다른 리소스 간에 충돌을 피하기 위해 인덱싱된 파일 경로는 모두 예약 된 "파일" **Resourcemap** 하위 트리 내에 상주 합니다. 예를 들어 ' \Images\logo.png ' 파일은 ' Files/images/logo.png ' 리소스 이름에 해당 합니다.

[StorageFile](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) api는 파일에 대 한 참조를 리소스로 투명 하 게 처리 하며 일반적인 사용 시나리오에 적합 합니다. **ResourceManager** 는 현재 컨텍스트를 재정의 하려는 경우와 같은 고급 시나리오에만 사용 해야 합니다.

### <a name="resourcecontext"></a>ResourceContext

리소스 후보는 리소스 한정자 값 (언어, 배율, 대비 등) 컬렉션인 특정 [Resourcecontext](/windows/winui/api/microsoft.applicationmodel.resources.resourcecontext)를 기반으로 선택 됩니다. 기본 컨텍스트는 재정의 되지 않는 한 각 한정자 값에 대해 앱의 현재 구성을 사용 합니다. 예를 들어 이미지와 같은 리소스는 한 모니터에서 다른 모니터로 다양 한 응용 프로그램 보기에서 다른 모니터로 다양 하 게 확장할 수 있습니다. 이러한 이유로 각 응용 프로그램 보기에는 고유한 기본 컨텍스트가 있습니다. 지정 된 뷰에 대 한 기본 컨텍스트는 [GetForCurrentView](/windows/winui/api/microsoft.applicationmodel.resources.resourcecontext)을 사용 하 여 가져올 수 있습니다. 리소스 후보를 검색할 때마다 지정 된 뷰에 대해 가장 적합 한 값을 얻기 위해 **Resourcecontext** 인스턴스를 전달 해야 합니다.

### <a name="important-apis"></a>중요 API

- [ResourceLoader](/windows/winui/api/microsoft.applicationmodel.resources.resourceloader)
- [ResourceManager](/windows/winui/api/microsoft.applicationmodel.resources.resourcemanager)
- [ResourceContext](/windows/winui/api/microsoft.applicationmodel.resources.resourcecontext)

## <a name="sample"></a>샘플

MRT.LOG Core API를 사용 하는 방법을 보여 주는 샘플은 [Mrt.log core 샘플](https://github.com/microsoft/Project-Reunion-Samples/tree/main/MrtCore)을 참조 하세요.
