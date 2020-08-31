---
Description: Microsoft Store에 제출할 수 있도록 앱 패키지를 준비 하려면 다음 지침을 따르세요.
title: 앱 패키지 요구 사항
ms.assetid: 651B82BA-9D0C-45AC-8997-88CD93DC903C
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, 패키지 요구 사항, 패키지, 패키지 형식, 지원 되는 버전, 제출
ms.localizationpriority: medium
ms.openlocfilehash: 851aaa28a7c42d395a16ee78a49a7e8bc5712f62
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158107"
---
# <a name="app-package-requirements"></a>앱 패키지 요구 사항

Microsoft Store에 제출할 수 있도록 앱 패키지를 준비 하려면 다음 지침을 따르세요.

## <a name="before-you-build-your-apps-package-for-the-microsoft-store"></a>Microsoft Store에 대 한 앱 패키지를 빌드하기 전에

[Windows 앱 인증 키트를 사용](../debug-test-perf/windows-app-certification-kit.md)하 여 앱을 테스트 해야 합니다. 또한 다양 한 유형의 하드웨어에서 앱을 테스트 하는 것이 좋습니다. 앱을 인증 하 고 Microsoft Store에서 사용할 수 있게 될 때까지 개발자 라이선스가 있는 컴퓨터에만 설치 하 고 실행할 수 있습니다.

## <a name="building-the-app-package-using-microsoft-visual-studio"></a>Microsoft Visual Studio를 사용 하 여 앱 패키지 빌드

Microsoft Visual Studio를 개발 환경으로 사용 하는 경우 응용 프로그램 패키지를 만들 수 있는 기본 제공 도구를 빠르고 쉽게 처리할 수 있습니다. 자세한 내용은 [앱 패키징](../packaging/index.md)을 참조 하세요.

> [!NOTE]
> 모든 파일 이름에 ANSI를 사용 해야 합니다. 

Visual Studio에서 패키지를 만들 때 개발자 계정과 연결 된 동일한 계정으로 로그인 했는지 확인 합니다. 패키지 매니페스트의 일부에는 계정과 관련 된 특정 정보가 있습니다. 이 정보는 자동으로 검색 되어 추가 됩니다. 매니페스트에 추가 정보를 추가 하지 않으면 패키지 업로드 오류가 발생할 수 있습니다. 

앱의 UWP 패키지를 빌드할 때 Visual Studio는. m 6 또는 appx 파일 또는. msixupload 또는 .appxupload 파일을 만들 수 있습니다. UWP 앱의 경우 [패키지](upload-app-packages.md) 페이지에서 항상. msixupload 또는. .appxupload 파일을 업로드 하는 것이 좋습니다. 스토어 용 UWP 앱을 패키징하는 방법에 대 한 자세한 내용은 [Visual Studio를 사용 하 여 uwp 앱 패키지](/windows/msix/package/packaging-uwp-apps)를 참조 하세요.

응용 프로그램의 패키지는 신뢰할 수 있는 인증 기관에서 루트 된 인증서로 서명할 필요가 없습니다.


### <a name="app-bundles"></a>앱 번들

UWP 앱의 경우 Visual Studio는 사용자가 다운로드 하는 앱의 크기를 줄이기 위해 앱 번들 (. msixbundle 또는 .appxbundle)을 생성할 수 있습니다. 이는 언어별 자산, 다양 한 이미지 배율 자산 또는 특정 버전의 Microsoft DirectX에 적용 되는 리소스를 정의한 경우에 유용할 수 있습니다.

> [!NOTE]
> 한 앱 번들에 모든 아키텍처에 대한 사용자 패키지를 포함할 수 있습니다.

사용자는 앱 번들을 사용 하 여 가능한 모든 리소스가 아닌 관련 파일만 다운로드 합니다. 앱 번들에 대 한 자세한 내용은 [앱](../packaging/index.md) 패키지 및 [Visual Studio를 사용 하 여 UWP 앱 패키지](/windows/msix/package/packaging-uwp-apps)를 참조 하세요.


## <a name="building-the-app-package-manually"></a>수동으로 앱 패키지 빌드

Visual Studio를 사용 하 여 패키지를 만들지 않는 경우 [패키지 매니페스트를 수동으로 만들어야](/uwp/schemas/appxpackage/how-to-create-a-package-manifest-manually)합니다.

전체 매니페스트 세부 정보 및 요구 사항은 [앱 패키지 매니페스트](/uwp/schemas/appxpackage/appx-package-manifest) 설명서를 참조 하세요. 인증서를 전달 하려면 매니페스트가 패키지 매니페스트 스키마를 따라야 합니다.

매니페스트에는 계정 및 앱에 대 한 특정 정보가 포함 되어야 합니다. 이 정보는 대시보드의 앱 개요 페이지에 있는 앱 **관리** 섹션에서 [앱 Id 세부 정보 보기를 참조](view-app-identity-details.md) 하 여 찾을 수 있습니다.

> [!NOTE]
> 매니페스트의 값은 대/소문자를 구분 합니다. 공백과 기타 문장 부호도 일치 해야 합니다. 값을 신중 하 게 입력 하 고 검토 하 여 올바른지 확인 합니다.


앱 번들 (. msixbundle 또는 .appxbundle)은 다른 매니페스트를 사용 합니다. 앱 번들 매니페스트에 대 한 세부 정보 및 요구 사항은 [번들 매니페스트](/uwp/schemas/bundlemanifestschema/bundle-manifest) 설명서를 참조 하세요. . Msixbundle 또는 .appxbundle에서 포함 된 각 패키지의 매니페스트는 [Identity](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) 요소의 **ProcessorArchitecture** 특성을 제외 하 고 동일한 요소 및 특성을 사용 해야 합니다.

> [!TIP]
> 패키지를 제출 하기 전에 [Windows 앱 인증 키트](../debug-test-perf/windows-app-certification-kit.md) 를 실행 해야 합니다. 이를 통해 매니페스트에 인증 또는 전송 오류가 발생할 수 있는 문제가 있는지 확인할 수 있습니다.


## <a name="package-format-requirements"></a>패키지 형식 요구 사항

앱의 패키지는 이러한 요구 사항을 준수 해야 합니다.

| 앱 패키지 속성 | 요구 사항                                                          |
|----------------------|----------------------------------------------------------------------|
| 패키지 크기         | . msixbundle 또는 .appxbundle: 번들 당 최대 25gb <br>패키지 당 최대 Windows 10:25 GB를 대상으로 하는 msix 또는 .appx 패키지<br>Windows 8.1를 대상으로 하는 .appx 패키지: 패키지 당 8gb의 최대값 <br> 패키지 당 최대 Windows 8:2 GB를 대상으로 하는 .appx 패키지 <br> Windows Phone 8.1를 대상으로 하는 .appx 패키지: 패키지 당 최대 4gb <br> .xap 패키지: 패키지 당 최대 1gb                                                                           |
| 지도 해시 차단     | SHA2-256 알고리즘                                                   |

> [!IMPORTANT]
> Windows Phone .x SDK를 사용 하 여 빌드된 새 XAP 패키지는 더 이상 업로드할 수 없습니다. 이미 XAP 패키지를 사용 하 여 저장 된 앱은 Windows 10 Mobile 장치에서 계속 작동 합니다. 자세한 내용은이 [블로그 게시물](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store)을 참조 하세요.

## <a name="supported-versions"></a>지원되는 버전

UWP 앱의 경우 모든 패키지는 저장소에서 지 원하는 Windows 10 버전을 대상으로 해야 합니다. 패키지에서 지 원하는 버전은 응용 프로그램 매니페스트의 [Targetdevicefamily](/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) 요소의 **MinVersion** 및 **MaxVersionTested** 특성에 표시 되어야 합니다.

에서 현재 지원 되는 버전의 범위는 다음과 같습니다. 
- 최소: 10.0.10240.0
- 최대: 10.0.17763.1


## <a name="storemanifest-xml-file"></a>StoreManifest XML 파일

StoreManifest.xml은 앱 패키지에 포함 될 수 있는 선택적 구성 파일입니다. 응용 프로그램을 Microsoft Store 장치 앱으로 선언 하는 것과 같은 기능을 사용 하도록 설정 하는 것과 같은 기능을 사용 하도록 설정 하는 것이 고 패키지 매니페스트가 적용 되지 않는 장치에 적용 되는 패키지의 요구 사항을 선언 하는 것입니다 사용 되는 경우 앱 패키지를 사용 하 여 StoreManifest.xml 전송 되며 앱 주 프로젝트의 루트 폴더에 있어야 합니다. 자세한 내용은 [StoreManifest 스키마](/uwp/schemas/storemanifest/store-manifest-schema-portal)를 참조 하세요.

 

 