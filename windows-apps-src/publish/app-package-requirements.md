---
author: jnHs
Description: Follow these guidelines to prepare your app's packages for submission to the Microsoft Store.
title: 앱 패키지 요구 사항
ms.assetid: 651B82BA-9D0C-45AC-8997-88CD93DC903C
ms.author: wdg-dev-content
ms.date: 10/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 패키지 요구 사항, 패키지, 패키지 형식, 지원되는 버전, 제출
ms.localizationpriority: medium
ms.openlocfilehash: f3e294fdf5a9b2d98f09d839fa62499b556de3a5
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/19/2018
ms.locfileid: "5157362"
---
# <a name="app-package-requirements"></a>앱 패키지 요구 사항

Microsoft Store에 제출할 앱 패키지를 준비하려면 다음 지침을 따르세요.

## <a name="before-you-build-your-apps-package-for-the-microsoft-store"></a>Microsoft Store용으로 앱 패키지를 빌드하기 전에

[Windows 앱 인증 키트를 사용하여 앱을 테스트](../debug-test-perf/windows-app-certification-kit.md)해야 합니다. 또한 다양한 유형의 하드웨어에서 앱을 테스트하는 것이 좋습니다. 앱을 인증하고 Microsoft Store에서 사용할 수 있도록 할 때까지 개발자 라이선스가 있는 컴퓨터에서만 앱을 설치하고 실행할 수 있습니다.

## <a name="building-the-app-package-using-microsoft-visual-studio"></a>Microsoft Visual Studio를 사용하여 앱 패키지 빌드

Microsoft Visual Studio를 개발 환경으로 사용 중인 경우 빠르고 쉽게 앱 패키지를 만들 수 있는 도구가 기본 제공됩니다. 자세한 내용은 [앱 패키징](../packaging/index.md)을 참조하세요.

> [!NOTE]
> 모든 파일 이름에는 ANSI를 사용해야 합니다. 

Visual Studio에서 패키지를 만들 때 개발자 계정과 연결된 동일한 계정을 사용하여 로그인해야 합니다. 패키지 매니페스트의 일부분은 계정과 관련된 고유한 정보입니다. 이 정보는 자동으로 검색 및 추가됩니다. 매니페스트에 정보를 추가하지 않으면 패키지 업로드 오류가 발생할 수 있습니다. 

앱의 UWP 패키지를 빌드할 때 Visual Studio는.msix appx 파일 또는.msixupload 또는.appxupload 파일을 만들 수 있습니다. UWP 앱의 경우 항상 [패키지](upload-app-packages.md) 페이지에서.msixupload 또는.appxupload 파일을 업로드 하는 것이 좋습니다. Microsoft Store용 UWP 앱 패키징에 대한 자세한 내용은 [Visual Studio를 사용하여 UWP 앱 패키징](../packaging/packaging-uwp-apps.md)을 참조하세요.

앱의 패키지는 신뢰할 수 있는 인증 기관에서 발급한 인증서로 서명할 필요가 없습니다.


### <a name="app-bundles"></a>앱 번들

UWP 앱에 대 한 Visual Studio (.msixbundle 또는.appxbundle) 사용자가 다운로드 하는 앱의 크기를 줄이기 위해 앱 번들을 생성할 수 있습니다. 언어별 자산, 다양한 이미지 스케일 자산 또는 특정 Microsoft DirectX 버전에 적용되는 리소스를 정의한 경우 이렇게 하면 유용할 수 있습니다.

> [!NOTE]
> 하나의 앱 번들에 모든 아키텍처에 대한 패키지가 포함될 수 있습니다.

앱 번들을 제공할 경우 사용자는 가능한 모든 리소스가 아닌 관련 파일만 다운로드할 수 있습니다. 앱 번들에 대한 자세한 내용은 [앱 패키징](../packaging/index.md) 및 [Visual Studio를 사용하여 UWP 앱 패키징](../packaging/packaging-uwp-apps.md)을 참조하세요.


## <a name="building-the-app-package-manually"></a>수동으로 앱 패키지 빌드

Visual Studio를 사용하여 패키지를 만들지 않는 경우에는 [패키지 매니페스트를 직접 만들어야](https://docs.microsoft.com/uwp/schemas/appxpackage/how-to-create-a-package-manifest-manually) 합니다.

전체 매니페스트 세부 정보 및 요구 사항은 [앱 패키지 매니페스트](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest) 설명서를 검토해야 합니다. 인증을 통과하려면 매니페스트가 패키지 매니페스트 스키마를 따라야 합니다.

매니페스트에 계정 및 앱에 대한 몇 가지 관련 정보가 포함되어야 합니다. 대시보드에서 앱 개요 페이지의 **앱 관리** 섹션에 있는 [앱 ID 세부 정보 보기](view-app-identity-details.md)를 확인하여 이 정보를 찾을 수 있습니다.

> [!NOTE]
> 매니페스트의 값은 대/소문자를 구분합니다. 공백과 기타 문장 부호도 일치해야 합니다. 값을 주의해서 입력하고 검토하여 올바른지 확인합니다.


앱 번들 (.msixbundle 또는.appxbundle)은 다른 매니페스트를 사용합니다. 앱 번들 매니페스트에 대한 세부 정보와 요구 사항은 [번들 매니페스트](https://docs.microsoft.com/uwp/schemas/bundlemanifestschema/bundle-manifest) 설명서를 검토하세요. .Msixbundle 또는.appxbundle에서 각각의 매니페스트에 포함 패키지는 동일한 요소 및 특성 [Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) 요소의 **ProcessorArchitecture** 특성을 제외 하 고 사용 해야 합니다.

> [!TIP]
> 패키지를 제출하기 전에 반드시 [Windows 앱 인증 키트](../debug-test-perf/windows-app-certification-kit.md)를 실행합니다. 그러면 매니페스트에 인증 또는 제출 오류를 발생시킬 수 있는 문제가 있는지 확인하는 데 도움이 될 수 있습니다.


## <a name="package-format-requirements"></a>패키지 형식 요구 사항

앱 패키지는 다음 요구 사항을 준수해야 합니다.

| 앱 패키지 속성 | 요구 사항                                                          |
|----------------------|----------------------------------------------------------------------|
| 패키지 크기         | .msixbundle 또는.appxbundle: 번들 당 최대 25GB <br>Windows 10시 25분 대상으로 하는.msix 또는.appx 패키지 패키지당 최대<br>Windows 8.1을 대상으로 하는 .appx 패키지: 패키지당 최대 8GB <br> Windows 8을 대상으로 하는 .appx 패키지: 패키지당 최대 2GB <br> Windows Phone 8.1을 대상으로 하는 .appx 패키지: 패키지당 최대 4GB <br> .xap 패키지: 패키지당 최대 1GB                                                                           |
| 블록 맵 해시     | SHA2-256 알고리즘                                                   |


## <a name="supported-versions"></a>지원되는 버전

UWP 앱의 경우 모든 패키지는 Microsoft Store에서 지원하는 버전의 Windows 10을 대상으로 지정해야 합니다. 패키지에서 지원되는 버전은 앱 매니페스트의 [TargetDeviceFamily](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) 요소의 **MinVersion** 및 **MaxVersionTested** 특성에 나타나야 합니다.

현재 지원되는 버전 범위는 다음과 같습니다. 
- 최소: 10.0.10240.0
- 최대: 10.0.17763.1


## <a name="storemanifest-xml-file"></a>StoreManifest XML 파일

StoreManifest.xml은 앱 패키지에 포함될 수 있는 선택적 구성 파일입니다. 이 구성 파일은 앱을 Microsoft Store 장치 앱으로 선언하거나 패키지를 장치에 적용하려면 필요한 요구 사항을 선언하는 등과 같이 패키지 매니페스트에서 다루지 않는 기능을 사용하도록 설정하기 위한 것입니다. 를 사용 하는 경우 StoreManifest.xml은 앱 패키지를 사용 하 여와 제출 앱의 기본 프로젝트의 루트 폴더에 있어야 합니다. 자세한 내용은 [StoreManifest 스키마](https://docs.microsoft.com/uwp/schemas/storemanifest/store-manifest-schema-portal)를 참조하세요.

 

 




