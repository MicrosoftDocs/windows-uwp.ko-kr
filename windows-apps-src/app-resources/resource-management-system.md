---
Description: At build time, the Resource Management System creates an index of all the different variants of the resources that are packaged up with your app. At run-time, the system detects the user and machine settings that are in effect and loads the resources that are the best match for those settings.
title: 리소스 관리 시스템
template: detail.hbs
ms.date: 10/20/2017
ms.topic: article
keywords: Windows 10, uwp, 리소스, 이미지, 자산, MRT, 한정자
ms.localizationpriority: medium
ms.openlocfilehash: bedbad9e4de22ee098863d013a1e4ad16d86543e
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8736833"
---
# <a name="resource-management-system"></a>리소스 관리 시스템
리소스 관리 시스템에는 빌드 시 및 런타임 시 기능이 모두 있습니다. 빌드 시 시스템은 앱과 함께 패키지되는 모든 여러 변형의 리소스의 인덱스를 만듭니다. 이 인덱스는 패키지 리소스 색인, 즉 PRI이며 앱 패키지에 포함되어 있습니다. 실행 시 시스템은 적용 중인 사용자와 시스템 설정을 감지하고 PRI의 정보를 확인하고 자동으로 이러한 설정에 대한 최적의 일치인 리소스를 로드합니다.

## <a name="package-resource-index-pri-file"></a>PRI(Package Resource Index) 파일
모든 앱 패키지는 앱 리소스의 이진 인덱스를 포함해야 합니다. 이 인덱스는 빌드 시 만들어지며 하나 또는 여러 개의 PRI(Package Resource Index) 파일에 포함되어 있습니다.

- PRI 파일에는 실제 문자열 리소스와 패키지의 다양한 파일을 참조하는 인덱스된 파일 경로 집합이 포함되어 있습니다.
- 일반적으로 패키지에는 언어마다 resources.pri라는 하나의 PRI 파일이 포함되어 있습니다.
- 각 패키지 루트의 resources.pri 파일은 [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live)가 인스턴스화될 때 자동으로 로드됩니다.
- [MakePRI.exe](compile-resources-manually-with-makepri.md) 도구로 PRI 파일을 만들고 덤프할 수 있습니다.
- 일반적인 앱 개발에서는 이미 Visual Studio 컴파일 워크플로에 통합되어 있으므로 MakePRI.exe가 필요 없습니다. 그리고 Visual Studio는 전용 UI에서 PRI 파일 편집을 지원합니다. 그러나 로컬라이저 및 그들이 사용하는 도구가 MakePRI.exe에 의존할 수도 있습니다.
- 각 PRI 파일에는 리소스 맵이라고 하는 명명된 리소스 컬렉션이 포함되어 있습니다. 패키지에서 PRI 파일을 로드할 때 리소스 맵 이름이 패키지 ID 이름과 일치하는지 확인합니다.
- PRI 파일에는 데이터가 포함되므로 PE(이식 가능한 실행 파일) 형식을 사용하지 않습니다. Windows용 리소스 형식으로 데이터 전용으로 설계됩니다. Win32 앱 모델에서 DLL에 포함된 리소스를 대체합니다.
- PRI 파일의 크기 제한은 64킬로바이트입니다.

## <a name="uwp-api-access-to-app-resources"></a>앱 리소스에 대한 UWP API 액세스

### <a name="basic-functionality-resourceloader"></a>기본 기능(ResourceLoader)
프로그래밍 방식으로 앱 리소스에 액세스하는 가장 간단한 방법은 [**Windows.ApplicationModel.Resources**](/uwp/api/windows.applicationmodel.resources?branch=live) 네임스페이스 및 [**ResourceLoader**](/uwp/api/windows.applicationmodel.resources.resourceloader?branch=live) 클래스를 사용하는 것입니다. **ResourceLoader**는 리소스 파일, 참조 라이브러리 또는 기타 패키지 집합에서 문자열 리소스로의 기본 액세스를 제공합니다.

### <a name="advanced-functionality-resourcemanager"></a>고급 기능(ResourceManager)
[**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) 클래스([**Windows.ApplicationModel.Resources.Core**](/uwp/api/windows.applicationmodel.resources.core?branch=live) 네임스페이스에 있음)는 열거 및 검사 등 리소스에 대한 추가 정보를 제공합니다. 이는 **ResourceLoader** 클래스가 제공하는 범위를 벗어납니다.

[**NamedResource**](/uwp/api/windows.applicationmodel.resources.core.namedresource?branch=live) 개체는 여러 언어 또는 기타 정규화된 변형의 개별 논리 리소스를 나타냅니다. `Header1`과 문자열 리소스 식별자 또는 `logo.jpg`와 같은 리소스 파일 이름이 있는 자산 또는 리소스의 논리적 보기를 설명합니다.

[**ResourceCandidate**](/uwp/api/windows.applicationmodel.resources.core.resourcecandidate?branch=live) 개체는 영어의 경우 "Hello World" 문자열, 또는 **scale-100** 해상도에 해당하는 정규화된 이미지 리소스로서의 "logo.scale-100.jpg"와 같은 구체적인 단일 리소스 값 및 한정자를 나타냅니다.

앱에서 사용할 수 있는 리소스는 [**ResourceMap**](/uwp/api/windows.applicationmodel.resources.core.resourcemap?branch=live) 개체로 액세스할 수 있는 계층 구조 컬렉션에 저장됩니다. **ResourceManager** 클래스는 앱에 대한 다양한 패키지에 해당하는 앱에서 사용되는 다양한 최상위 **ResourceMap** 인스턴스에 대한 액세스를 제공합니다. [**MainResourceMap**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager.MainResourceMap) 값은 현재 앱 패키지에 대한 리소스 맵에 해당하며 참조된 모든 프레임워크 패키지를 제외합니다. 각 **ResourceMap**은 패키지의 매니페스트에 지정된 패키지 이름에 대해 지정됩니다. **ResourceMap** 내에는 **NamedResource** 개체를 포함하는 하위 트리가 있습니다([**ResourceMap.GetSubtree**](/uwp/api/windows.applicationmodel.resources.core.resourcemap.getsubtree?branch=live) 참조). 하위 트리는 일반적으로 리소스를 포함하는 리소스 파일에 해당합니다. 자세한 내용은 [UI 및 앱 패키지 매니페스트의 문자열 지역화](localize-strings-ui-manifest.md) 및 [배율, 테마, 고대비 등에 맞춘 이미지 및 자산 로드](images-tailored-for-scale-theme-contrast.md)를 참조하세요.

예를 들면 다음과 같습니다.

```csharp
// using Windows.ApplicationModel.Resources.Core;
ResourceMap resourceMap =  ResourceManager.Current.MainResourceMap.GetSubtree("Resources");
ResourceContext resourceContext = ResourceContext.GetForCurrentView()
var str = resourceMap.GetValue("String1", resourceContext).ValueAsString;
```

**참고** 리소스 식별자는 URI(Uniform Resource Identifier) 의미 체계에 따라 URI 조각으로 처리됩니다. 예를 들어 `GetValue("Caption%20")`은 `GetValue("Caption ")`로 처리됩니다. "?" 또는 "#" 기호를 리소스 식별자에서 사용하면 리소스 경로 평가가 종료되므로 사용하지 마십시오. 예를 들어 "MyResource?3"은 "MyResource"로 처리됩니다.

**ResourceManager**는 앱의 문자열 리소스에 대한 액세스를 지원할 뿐만 아니라 다양한 파일 리소스를 열거 및 검사하는 기능 또한 가지고 있습니다. 파일 및 파일 내에서 시작하는 기타 리소스 간에 충돌을 방지하기 위해 인덱스된 파일 경로는 모두 예약된 "Files" **ResourceMap** 하위 트리에 위치합니다. 예를 들어 `\Images\logo.png`는 `Files/images/logo.png` 리소스 이름에 해당합니다.

[**StorageFile**](/uwp/api/Windows.Storage.StorageFile?branch=live) API는 파일에 대한 참조를 투명하게 리소스로 처리하므로 일반 사용 시나리오에 적합합니다. **ResourceManager**는 현재 컨텍스트를 재정의하거나 하는 경우 등의 고급 시나리오에서만 사용해야 합니다.

### <a name="resourcecontext"></a>ResourceContext
리소스 후보는 리소스 한정자 값(언어, 배율, 대비 등) 의 컬렉션인 특정 [**ResourceContext**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceContext?branch=live)에 따라 선택됩니다. 기본 컨텍스트는 재정의되지 않는 한 각 한정자 값에 대한 앱의 현재 구성을 사용합니다. 예를 들어 이미지 등의 리소스는 배율에 대해 정규화될 수 있으며 이는 모니터에 따라 다르므로 응용 프로그램 보기마다 다릅니다. 이러한 이유로 각 응용 프로그램 보기에는 고유한 기본 컨텍스트가 있습니다. 지정된 보기에 대한 기본 컨텍스트는 [**ResourceContext.GetForCurrentView**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.GetForCurrentView)를 사용하여 가져올 수 있습니다. 리소스 후보 검색할 때마다 **ResourceContext** 인스턴스를 전달해야 지정된 보기에 대해 가장 적합한 값을 얻을 수 있습니다.

## <a name="important-apis"></a>중요 API
* [ResourceLoader](/uwp/api/windows.applicationmodel.resources.resourceloader?branch=live)
* [ResourceManager](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live)
* [ResourceContext](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live)

## <a name="related-topics"></a>관련 항목
* [UI와 앱 패키지 매니페스트에 문자열 지역화](localize-strings-ui-manifest.md)
* [배율, 테마, 고대비 등에 맞춘 이미지 및 자산 로드](images-tailored-for-scale-theme-contrast.md)
