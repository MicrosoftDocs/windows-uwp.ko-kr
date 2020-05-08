---
Description: 빌드 시 리소스 관리 시스템은 앱과 함께 패키지되는 다양한 모든 리소스 변형에 대한 인덱스를 만듭니다. 런타임 시 시스템에서 적용되는 사용자 및 머신 설정을 검색하고 이러한 설정에 가장 적합한 리소스를 로드합니다.
title: 리소스 관리 시스템
template: detail.hbs
ms.date: 10/20/2017
ms.topic: article
keywords: Windows 10, UWP, 리소스, 이미지, 자산, MRT, 한정자
ms.localizationpriority: medium
ms.openlocfilehash: afe538292fe804dcf042c969005978c3161ec6b6
ms.sourcegitcommit: 963316e065cf36c17b6360c3f89fba93a1a94827
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82868883"
---
# <a name="resource-management-system"></a>리소스 관리 시스템
리소스 관리 시스템에는 빌드 시간과 런타임 기능이 모두 포함 되어 있습니다. 빌드 시 시스템은 앱과 함께 패키지 되는 리소스의 모든 다른 변형에 대 한 인덱스를 만듭니다. 이 인덱스는 패키지 리소스 인덱스 또는 PRI 이며 앱 패키지에도 포함 됩니다. 런타임에 시스템은 적용 되는 사용자 및 컴퓨터 설정을 검색 하 고, PRI의 정보를 확인 하 고, 해당 설정에 가장 일치 하는 리소스를 자동으로 로드 합니다.

## <a name="package-resource-index-pri-file"></a>PRI (패키지 리소스 인덱스) 파일
모든 앱 패키지는 앱의 리소스에 대 한 이진 인덱스를 포함 해야 합니다. 이 인덱스는 빌드 시에 생성 되 고 하나 이상의 PRI (패키지 리소스 인덱스) 파일에 포함 됩니다.

- PRI 파일에는 실제 문자열 리소스와 패키지의 다양 한 파일을 참조 하는 인덱싱된 파일 경로 집합이 포함 되어 있습니다.
- 패키지에는 일반적으로 리소스 pri 라는 언어별로 단일 PRI 파일이 포함 되어 있습니다.
- 각 패키지의 루트에 있는 resources. pri 파일은 [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) 가 인스턴스화될 때 자동으로 로드 됩니다.
- PRI 파일은 [makepri .exe](compile-resources-manually-with-makepri.md)도구를 사용 하 여 만들고 덤프할 수 있습니다.
- 일반적인 앱 개발에는 Visual Studio 컴파일 워크플로에 이미 통합 되어 있으므로 MakePRI가 필요 하지 않습니다. 및 Visual Studio는 전용 UI에서 PRI 파일을 편집할 수 있도록 지원 합니다. 그러나 지역화 담당자와 사용 하는 도구는 MakePRI를 사용 합니다.
- 각 PRI 파일에는 리소스 맵이라고 하는 이름이 지정된 리소스 컬렉션이 들어 있습니다. 패키지의 PRI 파일이 로드 되 면 리소스 맵 이름이 패키지 id 이름과 일치 하는지 확인 됩니다.
- PRI 파일은 데이터만 포함 하므로 PE (이식 가능한 실행 파일) 형식을 사용 하지 않습니다. 특별히 Windows의 리소스 형식으로 데이터 전용으로 설계 되었습니다. Win32 앱 모델의 Dll에 포함 된 리소스를 대체 합니다.

## <a name="uwp-api-access-to-app-resources"></a>앱 리소스에 대 한 UWP API 액세스

### <a name="basic-functionality-resourceloader"></a>기본 기능 (ResourceLoader)
응용 프로그램 리소스에 프로그래밍 방식으로 액세스 하는 가장 간단한 방법은 [**Windows ApplicationModel .resources**](/uwp/api/windows.applicationmodel.resources?branch=live) 네임 스페이스와 [**resourceloader**](/uwp/api/windows.applicationmodel.resources.resourceloader?branch=live) 클래스를 사용 하는 것입니다. **Resourceloader** 는 리소스 파일, 참조 된 라이브러리 또는 기타 패키지 집합에서 문자열 리소스에 대 한 기본 액세스를 제공 합니다.

### <a name="advanced-functionality-resourcemanager"></a>고급 기능 (ResourceManager)
[**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) 클래스 ( [**Windows.**](/uwp/api/windows.applicationmodel.resources.core?branch=live) r e c. c u r e s. c u l. c u r e s. 이는 **Resourceloader** 클래스에서 제공 하는 것 이상으로 진행 됩니다.

[**Namedresource**](/uwp/api/windows.applicationmodel.resources.core.namedresource?branch=live) 개체는 여러 언어 또는 기타 정규화 된 변형이 있는 개별 논리 리소스를 나타냅니다. 또는와 같은 문자열 리소스 식별자 `Header1`또는와 같은 리소스 파일 이름을 사용 하 여 자산이 나 리소스의 논리적 보기에 대해 설명 합니다. `logo.jpg`

[**ResourceCandidate**](/uwp/api/windows.applicationmodel.resources.core.resourcecandidate?branch=live) 개체는 단일 구체적 리소스 값과 해당 한정자 (예: 영어의 경우 "Hello World") 또는 "scale-100"를 **100** 해상도와 관련 된 정규화 된 이미지 리소스로 나타냅니다.

앱에 사용할 수 있는 리소스는 계층 컬렉션에 저장 되며,이는 [**resourcemap**](/uwp/api/windows.applicationmodel.resources.core.resourcemap?branch=live) 를 사용 하 여 액세스할 수 있습니다. **ResourceManager** 클래스를 사용 하면 앱에서 사용 하는 다양 한 최상위 **resourcemap** 에 대 한 액세스를 제공 하며, 앱에 대 한 다양 한 패키지에 해당 합니다. [**Mainresourcemap**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager.MainResourceMap) 값은 현재 앱 패키지의 리소스 맵에 해당 하며 참조 된 프레임 워크 패키지를 제외 합니다. 각 **resourcemap** 패키지의 매니페스트에 지정 된 패키지 이름으로 이름이 지정 됩니다. **Resourcemap** 는 하위 트리 (예를 들어, **namedresource** 개체를 포함 하는 [**resourcemap**](/uwp/api/windows.applicationmodel.resources.core.resourcemap.getsubtree?branch=live)가 있습니다. 하위 트리는 일반적으로 리소스를 포함 하는 리소스 파일에 해당 합니다. 자세한 내용은 [UI의 문자열 및 앱 패키지 매니페스트](localize-strings-ui-manifest.md) 및 [크기 조정, 테마, 고대비 등에 맞게 조정 된 이미지 및 자산 로드](images-tailored-for-scale-theme-contrast.md)를 참조 하세요.

예를 들면 다음과 같습니다.

```csharp
// using Windows.ApplicationModel.Resources.Core;
ResourceMap resourceMap =  ResourceManager.Current.MainResourceMap.GetSubtree("Resources");
ResourceContext resourceContext = ResourceContext.GetForCurrentView()
var str = resourceMap.GetValue("String1", resourceContext).ValueAsString;
```

**참고** 리소스 식별자는 URI 의미 체계가 적용 되는 URI (Uniform Resource Identifier) 조각으로 취급 됩니다. 예를 들어 `GetValue("Caption%20")` 는로 `GetValue("Caption ")`처리 됩니다. 리소스 식별자의 "?" 또는 "#"은 리소스 경로 평가를 종료 하므로 사용 하지 마십시오. 예를 들어 "MyResource? 3"은 "MyResource"로 처리 됩니다.

**ResourceManager** 는 앱의 문자열 리소스에 대 한 액세스를 지원할 뿐만 아니라 다양 한 파일 리소스를 열거 하 고 검사 하는 기능도 유지 관리 합니다. 파일 및 파일 내에서 시작 되는 다른 리소스 간에 충돌을 피하기 위해 인덱싱된 파일 경로는 모두 예약 된 "파일" **Resourcemap** 하위 트리 내에 상주 합니다. 예를 들어 파일 `\Images\logo.png` 은 리소스 이름 `Files/images/logo.png`에 해당 합니다.

[**StorageFile**](/uwp/api/Windows.Storage.StorageFile?branch=live) api는 파일에 대 한 참조를 리소스로 투명 하 게 처리 하며 일반적인 사용 시나리오에 적합 합니다. **ResourceManager** 는 현재 컨텍스트를 재정의 하려는 경우와 같은 고급 시나리오에만 사용 해야 합니다.

### <a name="resourcecontext"></a>ResourceContext
리소스 후보는 리소스 한정자 값 (언어, 배율, 대비 등) 컬렉션인 특정 [**Resourcecontext**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceContext?branch=live)를 기반으로 선택 됩니다. 기본 컨텍스트는 재정의 되지 않는 한 각 한정자 값에 대해 앱의 현재 구성을 사용 합니다. 예를 들어 이미지와 같은 리소스는 한 모니터에서 다른 모니터로 다양 한 응용 프로그램 보기에서 다른 모니터로 다양 하 게 확장할 수 있습니다. 이러한 이유로 각 응용 프로그램 보기에는 고유한 기본 컨텍스트가 있습니다. 지정 된 뷰에 대 한 기본 컨텍스트는 [**GetForCurrentView**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.GetForCurrentView)을 사용 하 여 가져올 수 있습니다. 리소스 후보를 검색할 때마다 지정 된 뷰에 대해 가장 적합 한 값을 얻기 위해 **Resourcecontext** 인스턴스를 전달 해야 합니다.

## <a name="important-apis"></a>중요 API
* [ResourceLoader](/uwp/api/windows.applicationmodel.resources.resourceloader?branch=live)
* [ResourceManager](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live)
* [ResourceContext](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live)

## <a name="related-topics"></a>관련 항목
* [UI 및 앱 패키지 매니페스트의 문자열 지역화](localize-strings-ui-manifest.md)
* [배율, 테마, 고대비 등에 맞춘 이미지 및 자산 로드](images-tailored-for-scale-theme-contrast.md)
