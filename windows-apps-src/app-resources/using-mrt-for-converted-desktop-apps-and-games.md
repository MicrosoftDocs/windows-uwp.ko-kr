---
title: 변환된 데스크톱 앱과 게임에 MRT 사용
description: .NET 또는 Win32 앱 또는 게임을 AppX 패키지로 패키징하면 리소스 관리 시스템을 사용하여 실행 시 컨텍스트에 맞게 조정된 앱 리소스를 로드할 수 있습니다. 이 항목에서는 그 기술에 대해 자세히 설명합니다.
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp, mrt, pri. 리소스, 게임, centennial, Desktop App Converter, mui, 위성 어셈블리
ms.localizationpriority: medium
ms.openlocfilehash: 3367cfafb2f3a8e307fd26dc6d6c19f1ece0d17e
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74254756"
---
# <a name="use-the-windows-10-resource-management-system-in-a-legacy-app-or-game"></a>레거시 앱 또는 게임에서 Windows 10 리소스 관리 시스템 사용

.NET 및 Win32 응용 프로그램 및 게임은 종종 다른 언어로 지역화되어 전체 대상 시장을 확장합니다. 앱 지역화의 가치 제안에 대한 자세한 내용은 [세계화 및 지역화](../design/globalizing/globalizing-portal.md)를 참조하세요. .NET 또는 Win32 앱 또는 게임을 MSIX 또는 AppX 패키지로 패키지화 하 여 리소스 관리 시스템을 활용 하 여 런타임 컨텍스트에 맞는 앱 리소스를 로드할 수 있습니다. 이 항목에서는 그 기술에 대해 자세히 설명합니다.

레거시 Win32 응용 프로그램을 지역화하는 방법은 여러 가지가 있지만 Windows 8에는 여러 프로그래밍 언어와 응용 프로그램 유형에서 사용할 수 있으며 간편한 지역화 이상의 기능을 제공하는 [새로운 리소스 관리 시스템](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10))이 도입되었습니다. 이 시스템은 이 항목에서 "MRT"라고 부르겠습니다. 이는 지금까지 "최신 리소스 기술"이라고 불렀지만 "최신"이라는 용어는 이제 사용되지 않습니다. 리소스 관리자는 MRM(최신 리소스 관리자) 또는 PRI(패키지 리소스 인덱스)라고도 알려져 있습니다.

MSIX 기반 또는 AppX 기반 배포와 함께 (예: Microsoft Store에서) MRT.LOG는 응용 프로그램의 다운로드 및 설치 크기를 최소화 하는 지정 된 사용자/장치에 대해 가장 적합 한 리소스를 자동으로 제공할 수 있습니다. 이 크기 감소는 지역화된 콘텐츠가 포함된 응용 프로그램에 매우 큰 것으로, AAA 게임의 경우 *기가바이트*의 자릿수가 변경되는 수준입니다. MRT의 추가 장점에는 Windows 셸과 Microsoft Store의 지역화된 목록, 사용자의 원하는 언어가 사용할 수 있는 리소스가 일치하지 않을 때 자동 대체 논리 등이 포함됩니다.

이 문서는 MRT의 간략한 아키텍처를 설명하고, 기존 Win32 응용 프로그램을 최소의 코드 변경으로 MRT로 이식할 수 있는 지침을 제공합니다. MRT로 이전하면, 개발자가 추가 장점(예: 배율 인수나 시스템 테마로 리소스를 분할하는 기능)을 사용할 수 있게 됩니다. MRT 기반 지역화는 데스크톱 브리지(일명 "Centennial")에 의해 처리되는 UWP 응용 프로그램 및 Win32 응용 프로그램 모두와 사용할 수 있습니다.

대부분의 경우 기존 지역화 형식과 소스 코드를 계속 사용할 수 있으며, 런타임에 리소스를 분석하고 다운로드 크기를 최소화하기 위해 MRT와 통합할 수 있습니다. 모두 가지거나 모두 포기하는 접근 방식이 아닙니다. 다음 표는 각 단계의 작업과 예상되는 비용/장점을 요약한 것입니다. 이 표는 고해상도 또는 고대비 응용 프로그램 아이콘과 같은 지역화되지 않는 작업을 포함하지 않습니다. 타일, 아이콘이 등에 대한 여러 자산 제공에 대한 자세한 내용은 [언어, 배율, 고대비 및 기타 한정자에 맞게 리소스 조정](tailor-resources-lang-scale-contrast.md)을 참조하세요.

<table>
<tr>
<th>Work</th>
<th>장점</th>
<th>예상 비용</th>
</tr>
<tr>
<td>패키지 매니페스트 지역화</td>
<td>Windows 셸과 Microsoft Store에 지역화된 콘텐츠를 표시하는 데 필요한 작업이 최소화됨</td>
<td>소형</td>
</tr>
<tr>
<td>MRT를 사용하여 리소스를 식별하고 찾기</td>
<td>다운로드 및 설치 크기 최소화를 위한 필수 조건, 자동 언어 대체</td>
<td>보통</td>
</tr>
<tr>
<td>리소스 팩 빌드</td>
<td>다운로드 및 설치 크기를 최소화하는 최종 단계</td>
<td>소형</td>
</tr>
<tr>
<td>MRT 리소스 형식과 API로 마이그레이션</td>
<td>훨씬 더 작은 파일 크기(기존 리소스 기술에 따름)</td>
<td>대형</td>
</tr>
</table>

## <a name="introduction"></a>소개

대부분의 중요 응용 프로그램에는 응용 프로그램의 코드에서 분리된(소스 코드 자체에서 작성된 *하드 코드된 값*과 반대) *리소스*라고 알려진 사용자 인터페이스 요소가 포함되어 있습니다. 하드 코드된 값을 통한 리소스를 선호하는 이유는 개발자가 아닌 사람에 의한 손쉬운 편집 등 여러 가지가 있지만 핵심적인 하나의 이유는 응용 프로그램이 런타임에 동일 논리적 리소스의 다른 표현을 선택할 수 있는 능력에 있습니다. 예를 들어, 단추에 표시되는 텍스트(또는 아이콘에 표시되는 이미지)는 사용자가 이해하는 언어, 디스플레이 디바이스의 특성 또는 사용자가 보조 기술을 사용하는지에 따라 달라질 수 있습니다.

따라서 모든 리소스 관리 기술의 주 목적은 논리적 또는 기호 *리소스 이름*(예: `SAVE_BUTTON_LABEL`)을 가능한 *후보*(예: "Save", "Speichern" 또는 "저장")에서 런타임에 가장 적절한 실제 *값*(예: "Save")으로 번역하는 것입니다. MRT는 이러한 기능을 제공하며, 응용 프로그램이 사용자의 언어, 디스플레이 배율 인수, 사용자가 선택한 테마 및 기타 환경 요인과 같은 *한정자*로 불리는 다양한 특성을 사용하여 리소스 후보를 식별할 수 있도록 합니다. MRT는 응용 프로그램이 필요할 경우 사용자 지정 한정자도 지원합니다. 예를 들어, 사용자가 계정으로 로그인했는지 게스트 사용자인지 확인하는 내용을 응용 프로그램의 모든 부분에 추가하지 않고도 다른 그래픽 자산을 제공할 수 있습니다. MRT는 문자열 리소스와 파일 기반 리소스 모두 사용할 수 있습니다. 파일 기반 리소스는 외부 데이터(파일 자체)에 대한 참조로 구현됩니다.

### <a name="example"></a>예제

여기 두 단추(`openButton` 및 `saveButton`)의 텍스트 레이블과 로고에 사용되는 PNG 파일(`logoImage`)이 있는 간단한 응용 프로그램의 예를 보겠습니다. 텍스트 레이블은 영어와 독일어로 지역화되어 있고, 로고는 일반 데스크톱 디스플레이(100% 배율 인수) 및 고해상도 휴대폰(300% 배율 인수)에 최적화되어 있습니다. 이 다이어그램은 모델의 개략적인 개념을 보여주는 것으로, 구현을 정확히 표현하지는 않습니다.

<p><img src="images\conceptual-resource-model.png"/></p>

이 그래픽에서 응용 프로그램 코드는 세 개의 논리 리소스 이름을 참조합니다. 런타임에서 `GetResource` 의사 함수는 MRT를 사용하여 리소스 테이블(PRI 파일)에서 리소스 이름을 조회하고 사용자의 언어와 디스플레이의 배율 인수와 같은 주변 조건에 따라 가장 적합한 후보를 찾습니다. 레이블의 경우 문자열이 직접 사용됩니다. 로고 이미지의 경우 문자열이 파일 이름으로 해석되고 파일을 디스크에서 읽습니다. 

사용자가 영어 또는 독일어 이외의 언어를 사용 하거나 100% 또는 300% 이외의 표시 배율를 사용 하는 경우에는는 대체 규칙 집합을 기준으로 일치 하는 "가장 가까운" 후보를 선택 합니다 (자세한 배경 정보는 [리소스 관리 시스템](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10)) 참조).

참고로 MRT는 하나 이상의 한정자에 맞춤화된 리소스를 지원합니다. 예를 들어 로고 이미지에 지역화될 텍스트가 포함되어 있는 경우, 로고는 영어/배율-100, 독일어/배율-100, 영어/배율-300, 독일어/배율-300의 네 후보를 가집니다.

### <a name="sections-in-this-document"></a>이 문서의 섹션

다음 섹션에서는 MRT를 응용 프로그램에 통합하는 데 필요한 대략적인 작업을 설명합니다.

#### <a name="phase-0-build-an-application-package"></a>0단계: 응용 프로그램 패키지 제작

이 섹션에서는 기존 응용 프로그램 빌드를 응용 프로그램을 패키지로 가져오는 방법을 설명합니다. 이 단계에서 MRT 기능은 사용되지 않습니다.

#### <a name="phase-1-localize-the-application-manifest"></a>1단계: 응용 프로그램 매니페스트 지역화

이 섹션에서는 응용 프로그램의 매니페스트를 지역화하여 Windows 셸에서 올바르게 표시되도록 하고, 기존 리소스 형식과 API를 그대로 사용하여 리소스를 패키징하고 찾는 방법을 알아봅니다. 

#### <a name="phase-2-use-mrt-to-identify-and-locate-resources"></a>2단계: MRT를 사용하여 리소스를 식별하고 찾기

이 섹션에서는 응용 프로그램 코드(리소스 레이아웃도 가능)를 수정하여 MRT를 사용해 리소스를 찾고, 기존 리소스 형식와 API를 그대로 사용하여 리소스를 로드하고 소비하는 방법을 설명합니다. 

#### <a name="phase-3-build-resource-packs"></a>3단계: 리소스 팩 빌드

이 섹션에서는 리소스를 별도의 *리소스 팩*으로 분리하여, 앱의 다운로드 및 설치 크기를 최소화하는 데 필요한 최종 변경 사항을 설명합니다.

### <a name="not-covered-in-this-document"></a>이 문서에서 다루지 않는 내용

위의 0-3 단계를 완료 한 후에는 Microsoft Store에 제출할 수 있는 "번들" 응용 프로그램이 필요 하지 않은 리소스 (예: 말하는 언어가 아닌 언어)를 생략 하 여 사용자에 대 한 다운로드 및 설치 크기를 최소화 하 게 됩니다. 최종 단계를 수행하여 응용 프로그램 크기와 기능을 더 향상시킬 수 있습니다.

#### <a name="phase-4-migrate-to-mrt-resource-formats-and-apis"></a>4단계: MRT 리소스 형식과 API로 마이그레이션

이 단계는 이 문서의 범위를 벗어납니다. 이 단계에서는 MUI DLL 또는 .NET 리소스 어셈블리와 같은 기존 형식에서 리소스(특히 문자열)를 PRI 파일로 전환합니다. 이를 통해 다운로드와 설치 크기를 위한 추가 공간을 절약할 수 있습니다. 또한 이를 통해 다른 MRT 기능도 사용할 수 있습니다. 여기에는 배율 인수, 접근성 설정 등에 따라 이미지 파일의 다운로드 및 설치 최소화 등이 포함됩니다.

## <a name="phase-0-build-an-application-package"></a>0단계: 응용 프로그램 패키지 제작

응용 프로그램의 리소스를 변경하기 전에, 현재 패키징과 설치 기술을 표준 UWP 패키징 및 배포 기술로 바꿔야 합니다. 이렇게 하는 데는 다음과 같은 세 가지 방법이 있습니다.

* 복잡 한 설치 관리자를 사용 하는 대용량 데스크톱 응용 프로그램이 있거나 많은 OS 확장성 지점이 사용 되는 경우 데스크톱 앱 변환기 도구를 사용 하 여 기존 앱 설치 관리자 (예: MSI)에서 UWP 파일 레이아웃 및 매니페스트 정보를 생성할 수 있습니다.
* 비교적 적은 수의 파일이 나 간단한 설치 관리자가 있고 확장성 후크가 없는 소규모 데스크톱 응용 프로그램을 사용 하는 경우 파일 레이아웃 및 매니페스트 정보를 수동으로 만들 수 있습니다.
* 원본에서 다시 작성 하는 경우 응용 프로그램을 순수 UWP 응용 프로그램으로 업데이트 하려면 Visual Studio에서 새 프로젝트를 만들고 IDE를 사용 하 여 많은 작업을 수행 합니다.

[데스크톱 앱 변환기](https://www.microsoft.com/store/p/desktopappconverter/9nblggh4skzw)를 사용 하려는 경우 변환 프로세스에 대 한 자세한 내용은 데스크톱 [앱 변환기를 사용 하 여 데스크톱 응용 프로그램 패키지](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-run-desktop-app-converter) 를 참조 하세요. 데스크톱 변환기 샘플의 전체 집합은 [UWP 샘플 GitHub 리포지토리에 대 한 데스크톱 브리지](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)에서 찾을 수 있습니다.

패키지를 수동으로 만들려면 모든 응용 프로그램 파일 (실행 파일 및 내용, 소스 코드 제외) 및 패키지 매니페스트 파일 (. appxmanifest.xml)을 포함 하는 디렉터리 구조를 만들어야 합니다. 예제는 [Hello, 세계 GitHub 샘플](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/blob/master/Samples/HelloWorldSample/CentennialPackage/AppxManifest.xml)에서 확인할 수 있지만, `ContosoDemo.exe` 라는 데스크톱 실행 파일을 실행 하는 기본 패키지 매니페스트 파일은 다음과 같습니다. 여기서 <span style="background-color: yellow">강조 표시 된 텍스트</span> 는 사용자 고유의 값으로 대체 됩니다.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
         xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
         xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
         IgnorableNamespaces="uap mp rescap">
    <Identity Name="Contoso.Demo"
              Publisher="CN=Contoso.Demo"
              Version="1.0.0.0" />
    <Properties>
    <DisplayName>Contoso App</DisplayName>
    <PublisherDisplayName>Contoso, Inc</PublisherDisplayName>
    <Logo>Assets\StoreLogo.png</Logo>
  </Properties>
    <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.0" 
                        MaxVersionTested="10.0.14393.0" />
  </Dependencies>
    <Resources>
    <Resource Language="en-US" />
  </Resources>
    <Applications>
    <Application Id="ContosoDemo" Executable="ContosoDemo.exe" 
                 EntryPoint="Windows.FullTrustApplication">
    <uap:VisualElements DisplayName="Contoso Demo" BackgroundColor="#777777" 
                        Square150x150Logo="Assets\Square150x150Logo.png" 
                        Square44x44Logo="Assets\Square44x44Logo.png" 
        Description="Contoso Demo">
      </uap:VisualElements>
    </Application>
  </Applications>
    <Capabilities>
    <rescap:Capability Name="runFullTrust" />
  </Capabilities>
</Package>
```

패키지 매니페스트 파일 및 패키지 레이아웃에 대 한 자세한 내용은 [앱 패키지 매니페스트](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appx-package-manifest)를 참조 하세요.

마지막으로 Visual Studio를 사용 하 여 새 프로젝트를 만들고 기존 코드를로 마이그레이션하 [는 경우 "Hello, 세계" 앱 만들기](https://docs.microsoft.com/windows/uwp/get-started/create-a-hello-world-app-xaml-universal)를 참조 하세요. 기존 코드를 새 프로젝트에 포함할 수 있지만 순수 UWP 앱으로 실행 하기 위해 중요 한 코드를 변경 해야 할 수 있습니다 (특히 사용자 인터페이스에서). 이러한 변경 내용은 이 문서의 범위를 벗어납니다.

## <a name="phase-1-localize-the-manifest"></a>1 단계: 매니페스트 지역화

### <a name="step-11-update-strings--assets-in-the-manifest"></a>1\.1 단계: 매니페스트에서 자산 & 업데이트

0 단계에서 응용 프로그램에 대 한 기본 패키지 매니페스트 (appxmanifest.xml) 파일을 만들었습니다 (변환기에 제공 된 값에 따라, MSI에서 추출 되었거나 수동으로 매니페스트에 입력 됨). 하지만 지역화 된 정보를 포함 하지 않거나 지원 하지 않습니다. 고해상도 시작 타일 자산과 같은 추가 기능

응용 프로그램의 이름과 설명이 올바르게 지역화 되었는지 확인 하려면 리소스 파일 집합에 일부 리소스를 정의 하 고 패키지 매니페스트를 업데이트 하 여 참조 해야 합니다.

#### <a name="creating-a-default-resource-file"></a>기본 리소스 파일 만들기

첫 번째 단계 기본 언어(예: 미국 영어)로 기본 리소스를 만드는 것입니다. 이는 텍스트 편집기를 사용하여 수동으로 편집하거나, Visual Studio에서 리소스 디자이너를 통해 편집할 수 있습니다.

수동으로 메시지를 만들 경우:

1. `resources.resw`라는 이름의 XML 파일을 만들고 프로젝트의 `Strings\en-us` 하위 폴더에 배치합니다. 기본 언어가 영어 (미국)가 아닌 경우 적절 한 BCP-47 코드를 사용 합니다.
2. XML 파일에 다음과 같은 내용을 추가합니다. <span style="background-color: yellow">강조 표시된 텍스트</span>는 기본 언어로 앱에 적절한 텍스트로 교체합니다.

> [!NOTE]
> 이러한 문자열의 길이에는 제한이 있습니다. 자세한 내용은 [VisualElements](/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements)를 참조하세요.

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
  <data name="ApplicationDescription">
    <value>Contoso Demo app with localized resources (English)</value>
  </data>
  <data name="ApplicationDisplayName">
    <value>Contoso Demo Sample (English)</value>
  </data>
  <data name="PackageDisplayName">
    <value>Contoso Demo Package (English)</value>
  </data>
  <data name="PublisherDisplayName">
    <value>Contoso Samples, USA</value>
  </data>
  <data name="TileShortName">
    <value>Contoso (EN)</value>
  </data>
</root>
```

Visual Studio에서 디자이너를 사용하려면:

1. 프로젝트에서 `Strings\en-us` 폴더 (또는 다른 언어)를 만들고 기본 이름인 `resources.resw`를 사용 하 여 프로젝트의 루트 폴더에 **새 항목** 을 추가 합니다. 리소스 **사전이** 아닌 리소스 **파일 (. resw)** 을 선택 해야 합니다. 리소스 사전은 XAML 응용 프로그램에서 사용 되는 파일입니다.
2. 디자이너를 사용하여 다음 문자열을 입력합니다. 같은 `Names`를 사용하지만 `Values`는 응용 프로그램에 대한 적절한 텍스트로 교체합니다.

<img src="images\editing-resources-resw.png"/>

> [!NOTE]
> Visual Studio designer를 사용 하 여 시작 하는 경우 언제 든 지 `F7`을 눌러 XML을 직접 편집할 수 있습니다. 하지만 최소의 XML 파일로 시작할 경우, 많은 추가 메타데이터가 누락되어 있으므로 *디자이너는 파일을 인식하지 않습니다.* 이를 해결하려면 디자이너가 생성한 파일에서 상용구 XML 정보를 직접 편집한 XML 파일에 복사하면 됩니다.

#### <a name="update-the-manifest-to-reference-the-resources"></a>리소스를 참조하도록 매니페스트 업데이트

`.resw` 파일에 정의 된 값이 있는 후 다음 단계는 리소스 문자열을 참조 하도록 매니페스트를 업데이트 하는 것입니다. 마찬가지로 XML 파일을 직접 편집하거나 Visual Studio 매니페스트 디자이너를 사용할 수 있습니다.

XML을 직접 편집할 경우, `AppxManifest.xml` 파일을 열고 <span style="background-color: lightgreen">강조 표시된 값</span>을 다음과 같이 변경합니다. 응용 프로그램에 대한 텍스트가 아니라 *정확한* 이 텍스트를 사용합니다. 이러한 정확한 리소스 이름을 사용할 필요는 없으며, 자체적으로 선택하면 됩니다. 하지만 선택한 이름은 &mdash; 파일의 이름과 정확히 일치해야 합니다. 이러한 이름은 `Names` 파일에서 만든 `.resw`와 일치해야 합니다. 접두사로 `ms-resource:` 스키마와 `Resources/` 네임스페이스가 붙습니다. 

> [!NOTE]
> 이 코드 조각에서 매니페스트의 많은 요소를 생략 했습니다. 아무것도 삭제 하지 마세요.

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package>
  <Properties>
    <DisplayName>ms-resource:Resources/PackageDisplayName</DisplayName>
    <PublisherDisplayName>ms-resource:Resources/PublisherDisplayName</PublisherDisplayName>
  </Properties>
  <Applications>
    <Application>
      <uap:VisualElements DisplayName="ms-resource:Resources/ApplicationDisplayName"
        Description="ms-resource:Resources/ApplicationDescription">
        <uap:DefaultTile ShortName="ms-resource:Resources/TileShortName">
          <uap:ShowNameOnTiles>
            <uap:ShowOn Tile="square150x150Logo" />
          </uap:ShowNameOnTiles>
        </uap:DefaultTile>
      </uap:VisualElements>
    </Application>
  </Applications>
</Package>
```

Visual Studio 매니페스트 디자이너를 사용 하는 경우 appxmanifest.xml 파일을 열고 **응용 프로그램* 탭 및 *패키징* 탭에서 <span style="background-color: lightgreen">강조 표시 된 값</span> 값을 변경 합니다.

<img src="images\editing-application-info.png"/>
<img src="images\editing-packaging-info.png"/>

### <a name="step-12-build-pri-file-make-an-msix-package-and-verify-its-working"></a>1\.2 단계: PRI 파일을 빌드하고, MSIX 패키지를 만들고 작동 하는지 확인

이제 `.pri` 파일을 빌드하여 응용 프로그램을 배포, 기본 언어로 올바른 정보가 시작 메뉴에 표시되는지 확인할 수 있습니다.

Visual Studio에서 빌드하는 경우 `Ctrl+Shift+B`를 눌러 프로젝트를 빌드하고 프로젝트를 마우스 오른쪽 단추로 클릭한 다음 상황에 맞는 메뉴에서 `Deploy`를 선택합니다.

수동으로 빌드하는 경우 다음 단계에 따라 `MakePRI` 도구의 구성 파일을 만들고 `.pri` 파일 자체를 생성 합니다 (추가 정보는 [수동 앱 포장](/windows/msix/package/manual-packaging-root)에서 확인할 수 있음).

1. 시작 메뉴의 **Visual studio 2017** 또는 **visual studio 2019** 폴더에서 개발자 명령 프롬프트를 엽니다.
2. 프로젝트 루트 디렉터리 (appxmanifest.xml 파일 및 **Strings** 폴더가 포함 된 디렉터리)로 전환 합니다.
3. 다음 명령을 입력합니다. "contoso_demo.xml"은 프로젝트에 적절한 이름으로, "en-US"를 앱의 기본 언어로 변경(또는 해당하는 경우 en-US 그대로 유지)합니다. XML 파일은 응용 프로그램에 포함 되지 않으므로 부모 디렉터리 (프로젝트 디렉터리가**아님** )에 생성 됩니다. 원하는 다른 디렉터리를 선택할 수 있지만 이후 명령에서 대체 해야 합니다.

    ```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US /pv 10.0 /o
    ```

    `makepri createconfig /?`를 입력하면 각 매개 변수가 무슨 용도인지 볼 수 있지만, 요약하면 다음과 같습니다.
      * `/cf` 구성 파일 이름 (이 명령의 출력)을 설정 합니다.
      * `/dq` 기본 한정자를 설정 합니다 .이 경우에는 언어 `en-US`
      * 플랫폼 버전 (이 경우 Windows 10)을 설정 `/pv`
      * `/o` 출력 파일이 있는 경우이 파일을 덮어쓰도록 설정 합니다.

4. 이제 구성 파일이 있으므로 `MakePRI`를 다시 실행하여 리소스를 실제로 검색하고, PRI 파일로 패키징합니다. "contoso_demop.xml"을 이전 단계에서 사용한 XML 파일 이름으로 바꾸고, 입력과 출력 모두에 대해 부모 디렉터리를 지정합니다. 

    ```CMD
    makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
    ```

    `makepri new /?`를 입력하면 각 매개 변수가 무슨 용도인지 볼 수 있지만, 요약하면 다음과 같습니다.
      * `/pr`는 프로젝트 루트 (이 경우 현재 디렉터리)를 설정 합니다.
      * `/cf`는 이전 단계에서 만든 구성 파일 이름을 설정 합니다.
      * 출력 파일을 설정 하 `/of` 
      * `/mf` 매핑 파일을 만듭니다. 따라서 이후 단계에서 패키지의 파일을 제외할 수 있습니다.
      * `/o` 출력 파일이 있는 경우이 파일을 덮어쓰도록 설정 합니다.

5. 이제 기본 언어 리소스(예: en-US)가 있는 `.pri` 파일이 준비되었습니다. 제대로 작동하는지 확인하려면 다음 명령을 실행합니다.

    ```CMD
    makepri dump /if ..\resources.pri /of ..\resources /o
    ```

    `makepri dump /?`를 입력하면 각 매개 변수가 무슨 용도인지 볼 수 있지만, 요약하면 다음과 같습니다.
      * 입력 파일 이름을 설정 `/if` 
      * `/of` 출력 파일 이름을 설정 합니다 (`.xml` 자동으로 추가 됨).
      * `/o` 출력 파일이 있는 경우이 파일을 덮어쓰도록 설정 합니다.

6. 마지막으로, 텍스트 편집기에서 `..\resources.xml`을 열고 `<NamedResource>` 값(`ApplicationDescription`과 `PublisherDisplayName`과 같은)과 선택한 기본 언어에 대한 `<Candidate>` 값이 나열되어 있는지 확인합니다. 파일 시작 부분에 다른 내용이 있지만 여기서는 무시하면 됩니다.

매핑 파일 `..\resources.map.txt`를 열어 프로젝트에 필요한 파일이 포함 되어 있는지 확인할 수 있습니다 (프로젝트 디렉터리의 일부가 아닌 PRI 파일 포함). 중요한 것은 매핑 파일의 콘텐츠가 PRI 파일에 포함되어 있기 때문에 매핑 파일에는 *파일에 대한 참조가 포함되어 있지*않다는`resources.resw` 것입니다. 그러나 이미지의 파일 이름과 같은 다른 리소스는 포함하고 있습니다.

#### <a name="building-and-signing-the-package"></a>패키지 빌드 및 서명 

이제 PRI 파일이 빌드되었으므로 패키지를 빌드하고 서명합니다.

1. 앱 패키지를 만들려면 다음 명령을 실행 하 여 만들려는 MSIX/AppX 파일의 이름으로 `contoso_demo.appx`을 바꾸고 파일에 대해 다른 디렉터리를 선택 해야 합니다 .이 샘플은 부모 디렉터리를 사용 하는 것은 물론 어디에 나 프로젝트 **디렉터리가 될 수** 있습니다.

    ```CMD
    makeappx pack /m AppXManifest.xml /f ..\resources.map.txt /p ..\contoso_demo.appx /o
    ```

    `makeappx pack /?`를 입력하면 각 매개 변수가 무슨 용도인지 볼 수 있지만, 요약하면 다음과 같습니다.
      * 사용할 매니페스트 파일을 설정 `/m`
      * `/f`는 사용할 매핑 파일을 설정 합니다 (이전 단계에서 만들어짐). 
      * 출력 패키지 이름을 설정 하 `/p`
      * `/o` 출력 파일이 있는 경우이 파일을 덮어쓰도록 설정 합니다.

2. 패키지를 만든 후에는 서명 해야 합니다. 서명 인증서를 가져오는 가장 쉬운 방법은 Visual Studio에서 빈 유니버설 Windows 프로젝트를 만들고이 프로젝트에서 만드는 `.pfx` 파일을 복사 하는 것입니다. 하지만 [앱 패키지 서명 인증서를 만드는 방법](https://docs.microsoft.com/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate)에 설명 된 대로 `MakeCert` 및 `Pvk2Pfx` 유틸리티를 사용 하 여 수동으로 만들 수 있습니다.

    > [!IMPORTANT]
    > 서명 인증서를 수동으로 만드는 경우 원본 프로젝트 또는 패키지 원본과 다른 디렉터리에 파일을 저장 해야 합니다. 그렇지 않으면 개인 키를 포함 하 여 패키지의 일부로 포함 될 수 있습니다.

3. 패키지에 서명하려면 다음 명령을 사용합니다. `Publisher`의 `Identity` 요소에 지정된 `AppxManifest.xml`는 인증서의 `Subject`와 일치해야 합니다. 이는 사용자에게 표시되는 지역화된 표시 이름인 **요소가**아닙니다`<PublisherDisplayName>`. 늘 그렇듯 `contoso_demo...` 파일 이름을 프로젝트에 적절한 이름으로 바꾸고(**매우 중요**) `.pfx` 파일이 현재 디렉터리에 있지 않도록 합니다. 그렇지 않으면 개인 서명 키를 포함하여 패키지의 일부로 생성됩니다.

    ```CMD
    signtool sign /fd SHA256 /a /f ..\contoso_demo_key.pfx ..\contoso_demo.appx
    ```

    `signtool sign /?`를 입력하면 각 매개 변수가 무슨 용도인지 볼 수 있지만, 요약하면 다음과 같습니다.
      * 파일 다이제스트 알고리즘을 설정 하는 `/fd` (SHA256의 기본값)
      * `/a`에서 자동으로 최상의 인증서를 선택 합니다.
      * `/f`는 서명 인증서를 포함 하는 입력 파일을 지정 합니다.

마지막으로, `.appx` 파일을 두 번 클릭하여 설치합니다. 명령줄을 선호하는 경우 PowerShell 프롬프트를 열고 패키지가 포함된 디렉터리로 이동한 다음, 다음 명령(`contoso_demo.appx`를 패키지 이름으로 대체)을 입력합니다.

```CMD
add-appxpackage contoso_demo.appx
```

인증서를 신뢰할 수 없다는 오류가 발생할 경우, 인증서가 사용자 저장소가 **아닌** 컴퓨터 저장소에 추가되어 있는지 되며 있는지 확인하십시오. 인증서에 컴퓨터 저장소에 추가하려면 명령줄 또는 Windows 탐색기를 사용합니다.

명령줄을 사용하려면:

1. 관리자 권한으로 Visual Studio 2017 또는 Visual Studio 2019 명령 프롬프트를 실행 합니다.
2. `.cer` 파일(소스 또는 프로젝트 디렉터리 외에 있어야 함을 기억하십시오.)이 포함된 디렉터리로 전환합니다.
3. `contoso_demo.cer`을 파일 이름으로 바꾸어 다음 명령을 입력합니다.
    ```CMD
    certutil -addstore TrustedPeople contoso_demo.cer
    ```
    
    `certutil -addstore /?`를 실행하면 각 매개 변수가 무슨 용도인지 볼 수 있지만, 요약하면 다음과 같습니다.
      * 인증서 저장소에 인증서를 추가 `-addstore`
      * `TrustedPeople`는 인증서가 배치 된 저장소를 나타냅니다.

Windows 탐색기를 사용하려면:

1. `.pfx` 파일이 있는 폴더로 이동합니다.
2. `.pfx` 파일을 두 번 클릭하면 **인증서 가져오기 마법사**가 표시됩니다.
3. `Local Machine` 선택 하 고 `Next`를 클릭 합니다.
4. 사용자 계정 컨트롤 관리자 권한 상승 프롬프트 (표시 되는 경우)를 수락 하 고 `Next`를 클릭 합니다.
5. 개인 키에 대 한 암호 (있는 경우)를 입력 하 고 `Next`를 클릭 합니다.
6. `Place all certificates in the following store` 선택
7. `Browse`를 클릭하고 `Trusted People` 폴더("신뢰할 수 있는 게시자"가 **아님**)를 선택합니다.
8. `Next` 클릭 하 고 `Finish`

`Trusted People` 저장소에 인증서를 추가한 다음, 패키지를 다시 설치합니다.

이제 시작 메뉴의 "모든 앱" 목록에 `.resw` / `.pri` 파일의 올바른 정보로 앱이 표시될 것입니다. 빈 문자열이나 `ms-resource:...` 문자열이 표시될 경우 무언가 잘못된 것입니다. 편집한 내용이 올바른지 다시 확인하십시오. 시작 메뉴에서 앱을 마우스 오른쪽 단추로 클릭하면 타일로 고정하고 올바른 정보가 표시되는지 확인할 수 있습니다.

### <a name="step-13-add-more-supported-languages"></a>1\.3단계: 지원되는 언어 더 추가

패키지 매니페스트를 변경 하 고 초기 `resources.resw` 파일을 만든 후에는 다른 언어를 추가 하는 것이 쉽습니다.

#### <a name="create-additional-localized-resources"></a>지역화된 추가 리소스 만들기

먼저, 지역화된 추가 리소스 값을 만듭니다. 

`Strings` 폴더 내에 적절한 BCP-47 코드(예: `Strings\de-DE`)를 사용하여 지원하려는 각 언어에 대한 추가 폴더를 만듭니다. 이러한 각 폴더 내에서 번역된 리소스 값이 포함된 `resources.resw` 파일(XML 편집기 또는 Visual Studio 디자이너 사용)을 만듭니다. 이미 지역화된 문자열을 가지고 있다고 가정하며, `.resw` 파일에 복사하면 됩니다. 이 문서는 번역 단계를 자체를 다루지는 않습니다. 

예를 들어, `Strings\de-DE\resources.resw` 파일은 <span style="background-color: yellow">강조 표시된 텍스트</span>가 `en-US`에서 변경된 상태로 다음과 같이 표시될 것입니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
  <data name="ApplicationDescription">
    <value>Contoso Demo app with localized resources (German)</value>
  </data>
  <data name="ApplicationDisplayName">
    <value>Contoso Demo Sample (German)</value>
  </data>
  <data name="PackageDisplayName">
    <value>Contoso Demo Package (German)</value>
  </data>
  <data name="PublisherDisplayName">
    <value>Contoso Samples, DE</value>
  </data>
  <data name="TileShortName">
    <value>Contoso (DE)</value>
  </data>
</root>
```

다음 단계는 `de-DE` 및 `fr-FR` 모두에 대한 리소스를 추가했다고 가정하지만, 모든 언어에 동일한 패턴을 사용할 수 있습니다.

#### <a name="update-the-package-manifest-to-list-supported-languages"></a>패키지 매니페스트를 업데이트 하 여 지원 되는 언어 나열

앱에서 지 원하는 언어를 나열 하려면 패키지 매니페스트를 업데이트 해야 합니다. Desktop App Converter에서 기본 언어를 추가하지만, 다른 언어는 명시적으로 추가해야 합니다. `AppxManifest.xml` 파일을 직접 편집할 경우, `Resources` 노드를 다음과 같이 업데이트합니다. 필요한 만큼 요소를 많이 추가하고, <span style="background-color: yellow">지원하는 해당 언어</span>로 대체하고, 목록의 첫 항목이 기본(대체) 언어가 되어야 합니다. 이 예에서 기본 언어는 영어(미국)이며, 독일어(독일)와 프랑스어(프랑스) 모두에 대한 지원이 추가되었습니다.

```xml
<Resources>
  <Resource Language="EN-US" />
  <Resource Language="DE-DE" />
  <Resource Language="FR-FR" />
</Resources>
```

Visual Studio를 사용하는 경우 아무것도 할 필요가 없습니다. `Package.appxmanifest`를 보면 특별한 <span style="background-color: yellow">x-generate</span> 값이 있는데, 이는 빌드 프로세스 중 프로젝트에서 찾은 언어를 BCP-47 코드를 따른 폴더 이름을 기반으로 삽입하도록 하는 것입니다. 실제 패키지 매니페스트에 대 한 올바른 값이 아닙니다. Visual Studio 프로젝트에 대해서만 작동 합니다.

```xml
<Resources>
  <Resource Language="x-generate" />
</Resources>
```

#### <a name="re-build-with-the-localized-values"></a>지역화된 값을 사용하여 다시 빌드

이제 응용 프로그램을 다시 빌드하고 배포할 수 있습니다. Windows에서 언어 기본 설정을 변경하려는 경우 시작 메뉴에 표시되는 새로 지역화된 값을 확인해야 합니다. 언어를 변경하는 방법에 대한 지침은 다음을 참조하십시오.

Visual Studio의 경우 `Ctrl+Shift+B`를 사용하여 빌드하고, 프로젝트를 마우스 오른쪽 단추로 클릭하여 `Deploy`를 선택합니다.

수동으로 프로젝트를 빌드하는 경우 위의 단계를 그대로 수행하지만, 구성 파일을 만들 때 기본 한정자 목록(`/dq`)에 밑줄로 구분된 추가 언어를 추가합니다. 예를 들어, 이전 단계에서 추가한 영어, 독일어, 프랑스어 리소스를 지원하려면 다음과 같이 합니다.

```CMD
makepri createconfig /cf ..\contoso_demo.xml /dq en-US_de-DE_fr-FR /pv 10.0 /o
```

이렇게 하면 지정된 언어 모두가 포함된 PRI 파일이 생성되어 쉽게 테스팅에 사용할 수 있습니다. 리소스의 전체 크기가 작거나 약간의 언어만 지원할 경우는 배송 앱에 적절할 수 있습니다. 별도의 언어 팩을 만드는 추가 작업을 할 필요가 있는 리소스를 위해 설치/다운로드 크기를 최소화하는 장점을 활용할 경우만 이렇게 합니다.

#### <a name="test-with-the-localized-values"></a>지역화 값으로 테스트

새롭게 지역화된 변경 내용을 테스트하려면, Windows에 새로운 기본 UI 언어를 추가 합니다. 언어 팩을 다운로드하거나, 시스템을 다시 부팅 하거나, 전체 Windows UI를 다른 언어로 표시할 필요가 없습니다. 

1. 앱 `Settings`(`Windows + I`)를 실행
2. `Time & language`로 이동
3. `Region & language`로 이동
4. `Add a language` 클릭
5. 원하는 언어를 입력(또는 선택)(예: `Deutsch`또는 `German`)
 * 하위 언어가 있을 경우, 선택(예: `Deutsch / Deutschland`)
6. 언어 목록에서 새 언어 선택
7. `Set as default` 클릭

이제 시작 메뉴를 열고 응용 프로그램을 검색하면, 선택한 언어에 대한 지역화된 값(다른 앱도 지역화되어 표시됨)이 표시됩니다. 지역화된 이름이 바로 표시되지 않으면, 시작 메뉴의 캐시가 새로 고쳐질 때까지 몇 분만 기다립니다. 원래 언어로 돌아가려면 언어 목록에서 기본 언어로 돌리면 됩니다. 

### <a name="step-14-localizing-more-parts-of-the-package-manifest-optional"></a>1\.4 단계: 패키지 매니페스트의 추가 파트 지역화 (선택 사항)

패키지 매니페스트의 다른 섹션을 지역화할 수 있습니다. 예를 들어, 응용 프로그램이 파일 확장명을 처리하는 경우, 매니페스트에 `windows.fileTypeAssociation` 확장명이 있어야 하며 <span style="background-color: lightgreen">녹색으로 강조 표시된 텍스트</span>(리소스를 참조)를 응용 프로그램에 대한 정보가 있는 <span style="background-color: yellow">노란색 텍스트로 강조 표시</span>된 내용으로 바꿉니다.

```xml
<Extensions>
  <uap:Extension Category="windows.fileTypeAssociation">
    <uap:FileTypeAssociation Name="default">
      <uap:DisplayName>ms-resource:Resources/FileTypeDisplayName</uap:DisplayName>
      <uap:Logo>Assets\StoreLogo.png</uap:Logo>
      <uap:InfoTip>ms-resource:Resources/FileTypeInfoTip</uap:InfoTip>
      <uap:SupportedFileTypes>
        <uap:FileType ContentType="application/x-contoso">.contoso</uap:FileType>
      </uap:SupportedFileTypes>
    </uap:FileTypeAssociation>
  </uap:Extension>
</Extensions>
```

Visual Studio 매니페스트 디자이너의 `Declarations` 탭의 <span style="background-color: lightgreen">강조 표시된 값</span>을 사용하여 이 정보를 추가할 수도 있습니다.

<p><img src="images\editing-declarations-info.png"/></p>

이제 해당 리소스 이름을 각각의 `.resw` 파일에 추가합니다. <span style="background-color: yellow">강조 표시된 텍스트</span>는 앱에 대한 적절한 텍스트(이 작업은 *지원되는 각 언어*에 대해 수행해야 함):

```xml
... existing content...
<data name="FileTypeDisplayName">
  <value>Contoso Demo File</value>
</data>
<data name="FileTypeInfoTip">
  <value>Files used by Contoso Demo App</value>
</data>
```

이렇게 하면 파일 탐색기와 같은 Windows 셸의 일부가 표시됩니다.

<p><img src="images\file-type-tool-tip.png"/></p>

전과 같이 패키지를 빌드하고 테스트하여 새 UI 문자열을 표시하는 새 시나리오 패키지를 테스트합니다.

## <a name="phase-2-use-mrt-to-identify-and-locate-resources"></a>2단계: MRT를 사용하여 리소스를 식별하고 찾기

이전 섹션에서 MRT를 사용하여 앱의 매니페스트를 지역화하여 Windows 셸이 앱의 이름과 기타 메타데이터를 올바르게 표시할 수 있도록 하는 방법을 설명했습니다. 이를 위해 코드 변경은 필요하지 않습니다. `.resw` 파일과 몇 가지 추가 도구만 있으면 됩니다. 이 섹션에서는 MRT를 사용하여 기존 리소스 형식에서 리소스를 찾고, 최소한의 변경으로 기존 리소스 처리 코드를 사용하는 방법을 설명합니다.

### <a name="assumptions-about-existing-file-layout--application-code"></a>기존 파일 레이아웃 및 응용 프로그램 코드에 대한 가정

Win32 데스크톱 앱을 지역화하는 많은 방법이 있기 때문에, 이 백서에서는 특정 환경을 매핑하는 데 필요한 기존 응용 프로그램의 구조를 간소화한다고 가정합니다. MRT의 요구 사항에 맞게 기존 코드 기반이나 리소스 레이아웃 일부를 변경해야 할 수 있으며, 이러한 내용은 이 문서의 범위에서 벗어납니다.

#### <a name="resource-file-layout"></a>리소스 파일 레이아웃

이 문서에서는 지역화 된 리소스의 파일 이름 (예: `contoso_demo.exe.mui` 또는 `contoso_strings.dll` 또는 `contoso.strings.xml`)이 동일 하지만 BCP-47 이름 (`en-US`, `de-DE`등)이 있는 다른 폴더에 배치 되어 있다고 가정 합니다. 리소스 파일의 개수, 이름, 파일 형식, 관련된 API 등은 상관이 없습니다. 중요한 유일한 내용은 모든 *논리적* 리소스가 동일한 파일 이름(하지만 다른 *물리적* 디렉터리에 배치)을 가져야 한다는 것입니다. 

반대되는 예로, 응용 프로그램이 `Resources` 및 `english_strings.dll` 파일이 포함된 하나의 `french_strings.dll` 디렉터리 파일이 포함된 플랫 파일 구조를 사용하는 경우, MRT에 잘 매핑되지는 않습니다. 더 나은 구조는 하위 디렉터리와 `Resources` 및 `en\strings.dll` 파일이 있는 `fr\strings.dll` 디렉터리입니다. `strings.lang-en.dll`과 `strings.lang-fr.dll`과 같은 포함 한정자를 가진 같은 기본 파일을 사용하는 것도 가능하지만, 언어 코드를 가진 디렉터리를 사용하는 것이 개념적으로 더 간단하므로 이에 집중합니다.

>[!NOTE]
> 이 파일 명명 규칙을 따르지 않는 경우에도 MRT.LOG 및 패키징을 사용할 수 있습니다. 더 많은 작업이 필요 합니다.

예를 들어, 응용 프로그램이 <span style="background-color: yellow">UICommands</span> 폴더에 <span style="background-color: yellow">ui.txt</span>라는 이름의 간단한 텍스트 파일에 단추 등에 사용되는 사용자 지정 UI 명령 집합이 있을 수 있습니다.

<blockquote>
<pre>
+ ProjectRoot
|--+ Strings
|  |--+ en-US
|  |  \--- resources.resw
|  \--+ de-DE
|     \--- resources.resw
|--+ <span style="background-color: yellow">UICommands</span>
|  |--+ en-US
|  |  \--- <span style="background-color: yellow">ui.txt</span>
|  \--+ de-DE
|     \--- <span style="background-color: yellow">ui.txt</span>
|--- AppxManifest.xml
|--- ...rest of project...
</pre>
</blockquote>

#### <a name="resource-loading-code"></a>리소스 로드 코드

이 문서에서는 코드의 특정 지점에서 지역화 된 리소스를 포함 하는 파일을 찾고 로드 한 다음 사용 한다고 가정 합니다. 리소스를 로드하는 데 사용되는 API, 리소스를 추출하는 데 사용되는 API는 중요하지 않습니다. 의사 코드에서 기본적으로 세 단계가 있습니다.

<blockquote>
<pre>
set userLanguage = GetUsersPreferredLanguage()
set resourceFile = FindResourceFileForLanguage(MY_RESOURCE_NAME, userLanguage)
set resource = LoadResource(resourceFile) 
    
// now use 'resource' however you want
</pre>
</blockquote>

MRT는 이 프로세스에서 첫 두 단계만 변경하면 됩니다. 가장 좋은 후보 리소스를 판단하는 것과 찾는 방법입니다. 장점을 활용하기 위해 사용할 수 있는 기능을 제공하지만, 리소스를 로드하거나 사용하는 방법을 변경할 필요는 없습니다.

예를 들어, 응용 프로그램에서 Win32 API `GetUserPreferredUILanguages`, CRT 함수 `sprintf`, Win32 API `CreateFile`을 사용하여 위 세 개의 의사 코드 함수를 변경하고, 수동으로 `name=value` 쌍을 찾는 텍스트 파일을 구문 분석할 수 있습니다.의 (세부 정보는 중요하지 않습니다. MRT가 이미 찾은 리소스를 처리하는 기술에 영향을 주지 않는다는 것을 설명하는 것이기 때문입니다.)

### <a name="step-21-code-changes-to-use-mrt-to-locate-files"></a>2\.1단계: 파일 찾기를 위해 MRT를 사용하는 코드 변경

리소스를 찾기 위해 MRT를 사용하도록 코드를 바꾸는 것은 어렵지 않습니다. 이를 위해서는 유용한 WinRT 형식과 몇 줄의 코드를 사용해야 합니다. 사용할 주요 형식은 다음과 같습니다.

* [ResourceContext](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceContext), 현재 사용 중인 한정자 값(언어, 배율 인수 등)을 캡슐화합니다.
* [ResourceManager](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.resources.core.resourcemanager)(.NET 버전이 아닌 WinRT 버전), PRI에서 모든 리소스에 액세스할 수 있도록 합니다.
* [ResourceMap](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.resources.core.resourcemap), PRI 파일의 특정 리소스 하위 집합(이 예에서는 파일 기반 리소스 대 문자열 리소스)을 나타냅니다.
* [NamedResource](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Resources.Core.NamedResource), 논리 리소스와 가능한 모든 후보를 나타냅니다.
* [ResourceCandidate](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.resources.core.resourcecandidate), 하나의 구체적 후보 리소스를 나타냅니다. 

의사 코드에서, 지정된 파일 이름(위 샘플의 `UICommands\ui.txt`와 같은)을 해석하는 방법은 다음과 같습니다.

<blockquote>
<pre>
// Get the ResourceContext that applies to this app
set resourceContext = ResourceContext.GetForViewIndependentUse()
    
// Get the current ResourceManager (there's one per app)
set resourceManager = ResourceManager.Current
    
// Get the "Files" ResourceMap from the ResourceManager
set fileResources = resourceManager.MainResourceMap.GetSubtree("Files")
    
// Find the NamedResource with the logical filename we're looking for,
// by indexing into the ResourceMap
set desiredResource = fileResources["UICommands\ui.txt"]
    
// Get the ResourceCandidate that best matches our ResourceContext
set bestCandidate = desiredResource.Resolve(resourceContext)
   
// Get the string value (the filename) from the ResourceCandidate
set absoluteFileName = bestCandidate.ValueAsString
</blockquote>
</pre>

코드에서 **와 같은 특정 언어 폴더가 디스크에 어떻게 존재하는지 관계없이 이를 요청하지** 않는`UICommands\en-US\ui.txt`다는 점을 유의하십시오. 대신, 코드는 *논리적* 파일 이름인 `UICommands\ui.txt`를 요청하며 MRT를 사용하여 언어 디렉터리 중 하나에서 해당 디스크 내 파일을 찾습니다.

여기에서, 샘플 앱은 계속 `CreateFile`을 사용하여 `absoluteFileName`을 로드하고 전과 같이 `name=value` 쌍을 구문 분석합니다. 앱에서 변경이 필요한 논리는 없습니다. C# 또는 C++/CX에서 코드를 작성하는 경우, 실제 코드는 이보다 복잡할 수 있습니다(실제로 많은 매개 변수가 생략될 수 있음). 아래 **.NET 리소스 로드** 섹션을 참조하세요. C++/WRL 기반 응용 프로그램은 WinRT API를 호출하는 데 하위 수준 COM 기반 API를 사용하기 때문에 더 복잡할 수 있지만, 기본 단계는 동일합니다. 아래 **Win32 MUI 리소스 로드** 섹션을 참조하세요.

#### <a name="loading-net-resources"></a>.NET 리소스 로드

.NET에는 리소스를 찾고 로드하는 기본 제공 메커니즘("위성 어셈블리"라고 함)이 있기 때문에, 위 코드 샘플을 굳이 바꿀 필요가 없습니다. .NET에서는 해당 디렉터리에 리소스 DLL이 있으면 자동으로 이를 찾습니다. 앱이 리소스 팩을 사용 하 여 MSIX 또는 AppX로 패키지 되는 경우 디렉터리 구조는 리소스 디렉터리가 주 응용 프로그램 디렉터리의 하위 디렉터리가 되도록 하는 것이 아니라 약간 다릅니다. 은 (는) 해당 기본 설정에 나열 된 언어가 없습니다.) 

예를 들어. 모든 파일이 `MainApp` 폴더에 있는 다음과 같은 레이아웃의 .NET 응용 프로그램을 가정해 보겠습니다.

<blockquote>
<pre>
+ MainApp
|--+ en-us
|  \--- MainApp.resources.dll
|--+ de-de
|  \--- MainApp.resources.dll
|--+ fr-fr
|  \--- MainApp.resources.dll
\--- MainApp.exe
</pre>
</blockquote>

AppX로 변환한 후, 레이아웃은 다음과 유사하게 됩니다. `en-US`가 기본 언어라고 가정했으며, 사용자가 언어 목록에 독일어와 프랑스어를 선택했다고 가정했습니다.

<blockquote>
<pre>
+ WindowsAppsRoot
|--+ MainApp_neutral
|  |--+ en-us
|  |  \--- <span style="background-color: yellow">MainApp.resources.dll</span>
|  \--- MainApp.exe
|--+ MainApp_neutral_resources.language_de
|  \--+ de-de
|     \--- <span style="background-color: yellow">MainApp.resources.dll</span>
\--+ MainApp_neutral_resources.language_fr
   \--+ fr-fr
      \--- <span style="background-color: yellow">MainApp.resources.dll</span>
</pre>
</blockquote>

기본 실행 파일 설치 위치 아래의 하위 디렉터리에 지역화된 리소스가 없기 때문에 기본 제공 .NET 리소스 확인이 실패합니다. 다행히 .NET에는 실패한 어셈블리 로드 시도를 처리하는 잘 정의된 메커니즘인 `AssemblyResolve` 이벤트가 있습니다. MRT를 사용하는 .NET 앱은 이 이벤트를 등록해야 하며, .NET 리소스 하위 시스템에 대한 누락 어셈블리를 제공해야 합니다. 

.NET에서 위성 어셈블리를 찾는 데 WinRT API를 사용하는 방법을 설명하는 간략한 예는 다음과 같습니다. 코드가 최소 구현을 보여주기 위해 의도적으로 축소되어 있지만, 찾을 어셈블리의 이름을 제공하기 위해 `ResolveEventArgs`를 전달한 위 의사 코드와 유사함을 알 수 있을 것입니다. 이 코드의 실행 가능한 버전(상세한 주석과 오류 처리 포함)은 `PriResourceRsolver.cs`GitHub의 [.NET 어셈블리 확인자 **샘플**의 ](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DotNetSatelliteAssemblyDemo) 파일에서 찾을 수 있습니다.

```csharp
static class PriResourceResolver
{
  internal static Assembly ResolveResourceDll(object sender, ResolveEventArgs args)
  {
    var fullAssemblyName = new AssemblyName(args.Name);
    var fileName = string.Format(@"{0}.dll", fullAssemblyName.Name);

    var resourceContext = ResourceContext.GetForViewIndependentUse();
    resourceContext.Languages = new[] { fullAssemblyName.CultureName };

    var resource = ResourceManager.Current.MainResourceMap.GetSubtree("Files")[fileName];

    // Note use of 'UnsafeLoadFrom' - this is required for apps installed with AppX, but
    // in general is discouraged. The full sample provides a safer wrapper of this method
    return Assembly.UnsafeLoadFrom(resource.Resolve(resourceContext).ValueAsString);
  }
}
```

위 클래스가 제공되면, 다음 코드를 지역화된 리소스를 로드하기 전에 응용 프로그램 시작 코드 어디에서든 추가할 수 있습니다.

```csharp
void EnableMrtResourceLookup()
{
  AppDomain.CurrentDomain.AssemblyResolve += PriResourceResolver.ResolveResourceDll;
}
```

.NET 런타임은 리소스 DLL을 찾을 수 없을 경우 `AssemblyResolve` 이벤트를 발생시키며, 이때 제공된 이벤트 처리기가 MRT를 통해 원하는 파일을 찾아 어셈블리를 반환합니다.

> [!NOTE]
> 앱에 다른 용도에 대 한 `AssemblyResolve` 처리기가 이미 있는 경우 리소스 확인 코드를 기존 코드와 통합 해야 합니다.

#### <a name="loading-win32-mui-resources"></a>Win32 MUI 리소스 로딩

Win32 MUI 리소스 로딩은 기본적으로 .NET 위성 어셈블리 로딩과 같지만 C++/CX 또는 C++/WRL 코드를 대신 사용합니다. C++/CX를 사용하면 위 C# 코드와 아주 비슷하지만, C++ 언어 확장, 컴파일러 스위치, 추가 런타임 오버헤드를 사용하므로 피하고 싶을 것입니다. 이런 경우, C++/WRL을 사용하는 것이 더 복잡한 코드를 피하고 영향을 줄이는 훨씬 좋은 방법입니다. 하지만 ATL 프로그래밍(또는 일반적인 COM)에 익숙할 경우 WRL이 친숙할 것입니다. 

다음 샘플 함수는 C++/WRL을 사용하여 특정 리소스 DLL을 로드하고 일반적인 Win32 리소스 API를 사용하여 추가 리소스를 로드하는 데 사용되는 `HINSTANCE`를 반환하는 방법을 보여줍니다. 명시적으로 .NET 런타임에서 요청한 언어로 `ResourceContext`를 초기화한 C# 샘플과 달리, 이 코드는 사용자의 현재 언어를 사용함을 유의하십시오.

```cpp
#include <roapi.h>
#include <wrl\client.h>
#include <wrl\wrappers\corewrappers.h>
#include <Windows.ApplicationModel.resources.core.h>
#include <Windows.Foundation.h>
   
#define IF_FAIL_RETURN(hr) if (FAILED((hr))) return hr;
    
HRESULT GetMrtResourceHandle(LPCWSTR resourceFilePath,  HINSTANCE* resourceHandle)
{
  using namespace Microsoft::WRL;
  using namespace Microsoft::WRL::Wrappers;
  using namespace ABI::Windows::ApplicationModel::Resources::Core;
  using namespace ABI::Windows::Foundation;
    
  *resourceHandle = nullptr;
  HRESULT hr{ S_OK };
  RoInitializeWrapper roInit{ RO_INIT_SINGLETHREADED };
  IF_FAIL_RETURN(roInit);
    
  // Get Windows.ApplicationModel.Resources.Core.ResourceManager statics
  ComPtr<IResourceManagerStatics> resourceManagerStatics;
  IF_FAIL_RETURN(GetActivationFactory(
    HStringReference(
    RuntimeClass_Windows_ApplicationModel_Resources_Core_ResourceManager).Get(),
    &resourceManagerStatics));
    
  // Get .Current property
  ComPtr<IResourceManager> resourceManager;
  IF_FAIL_RETURN(resourceManagerStatics->get_Current(&resourceManager));
    
  // get .MainResourceMap property
  ComPtr<IResourceMap> resourceMap;
  IF_FAIL_RETURN(resourceManager->get_MainResourceMap(&resourceMap));
    
  // Call .GetValue with supplied filename
  ComPtr<IResourceCandidate> resourceCandidate;
  IF_FAIL_RETURN(resourceMap->GetValue(HStringReference(resourceFilePath).Get(),
    &resourceCandidate));
    
  // Get .ValueAsString property
  HString resolvedResourceFilePath;
  IF_FAIL_RETURN(resourceCandidate->get_ValueAsString(
    resolvedResourceFilePath.GetAddressOf()));
    
  // Finally, load the DLL and return the hInst.
  *resourceHandle = LoadLibraryEx(resolvedResourceFilePath.GetRawBuffer(nullptr),
    nullptr, LOAD_LIBRARY_AS_DATAFILE | LOAD_LIBRARY_AS_IMAGE_RESOURCE);
    
  return S_OK;
}
```

## <a name="phase-3-building-resource-packs"></a>3단계: 리소스 팩 빌드

이제 모든 리소스가 포함된 "큰 팩"이 있고, 다운로드 및 설치 크기를 최소화하기 위해 주 패키지와 리소스 패키지를 분리하는 두 가지 방법이 마련되었습니다.

* 리소스 팩을 자동으로 만들기 위해 기존의 큰 팩을 사용하여 [번들 생성기 도구](https://www.microsoft.com/store/apps/9nblggh43pmq)를 실행해 보겠습니다. 이미 큰 팩을 생성한 시스템을 구축하였고, 리소스 팩을 생성하기 위해 사후 처리를 원하는 경우 선호되는 방법입니다.
* 개별 리소스 패키지를 직접 생성하고 번들로 빌드합니다. 빌드 시스템을 더 자세히 제어하고 패키지를 직접 빌드할 수 있는 경우 선호되는 접근 방식입니다.

### <a name="step-31-creating-the-bundle"></a>3\.1단계: 번들 만들기

#### <a name="using-the-bundle-generator-tool"></a>번들 생성기 도구 사용

번들 생성기 도구를 사용하기 위해서는 패키지를 위해 생성된 PRI 구성 파일에서 `<packaging>`섹션을 수동으로 제거하는 업데이트가 필요합니다.

Visual Studio를 사용 하는 경우, 파일 `priconfig.packaging.xml` 및 `priconfig.default.xml`를 만들어 주 패키지에 모든 언어를 빌드하는 방법에 대 한 정보를 제공 [해야 하는지 여부에 관계 없이 장치에 리소스가 설치](https://docs.microsoft.com/en-us/previous-versions/dn482043(v=vs.140)) 되어 있는지 확인 합니다.

파일을 수동으로 편집하는 경우 다음 단계를 따르십시오. 

1. 전과 마찬가지로 구성 파일을 만들고, 경로, 파일 이름, 언어를 바꿉니다.

    ```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US_de-DE_es-MX /pv 10.0 /o
    ```

2. 생성된 `.xml` 파일을 수동으로 열고 `&lt;packaging&rt;` 섹션 전체를 삭제(나머지는 그대로 유지)합니다.

    ```xml
    <?xml version="1.0" encoding="UTF-8" standalone="yes" ?> 
    <resources targetOsVersion="10.0.0" majorVersion="1">
      <!-- Packaging section has been deleted... -->
      <index root="\" startIndexAt="\">
        <default>
        ...
        ...
    ```

3. 전과 같이 업데이트된 구성 파일, 해당 디렉터리 및 파일 이름(이러한 명령에 대해서는 위 내용 참조)으로 `.pri` 파일과 `.appx` 패키지를 빌드합니다.

    ```CMD
    makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
    makeappx pack /m AppXManifest.xml /f ..\resources.map.txt /p ..\contoso_demo.appx /o
    ```

4. 패키지를 만든 후에는 다음 명령을 사용 하 여 적절 한 디렉터리와 파일 이름을 사용 하 여 번들을 만듭니다.

    ```CMD
    BundleGenerator.exe -Package ..\contoso_demo.appx -Destination ..\bundle -BundleName contoso_demo
    ```

이제 마지막 단계인 서명 (아래 참조)으로 이동할 수 있습니다.

#### <a name="manually-creating-resource-packages"></a>리소스 패키지 수동 생성

수동으로 리소스 패키지를 만들려면 별도의 `.pri`와 `.appx` 파일을 빌드하기 위한 약간 다른 명령을 실행해야 합니다. 위에서 큰 패키지를 만들기 위해 사용한 코드와 비슷하므로 설명은 최대한 간략하게 하겠습니다. 참고: 모든 명령은 현재 디렉터리가 `AppXManifest.xml` 파일을 포함한 디렉터리라고 가정하지만, 모든 파일은 부모 디렉터리에 위치합니다. 필요하면 다른 디렉터리를 사용해도 되지만, 프로젝트 디렉터리에 불필요한 파일을 넣어서는 안 됩니다. 언제나처럼 "Contoso" 파일 이름을 원하는 파일 이름으로 바꿉니다.

1. 기본 한정자로 기본 언어(이 경우 **)를 사용할** 경우만`en-US` 다음 명령을 사용하여 구성 파일을 만듭니다.

    ```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US /pv 10.0 /o
    ```

2. 다음 명령을 사용하여 기본 패키지와 프로젝트에서 발견되는 각 언어에 대한 파일 세트를 위한 기본 `.pri`와 `.map.txt` 파일을 만듭니다.

    ```CMD
    makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
    ```

3. 다음 명령을 사용하여 실행 코드와 기본 언어 리소스를 포함한 주 패키지를 만듭니다. 번들을 나중에 쉽게 만들기 위해(이 예에서는 `..\bundle` 디렉터리 사용) 패키지를 별도의 디렉터리에 넣겠지만, 이름을 알맞게 변경합니다.

    ```CMD
    makeappx pack /m .\AppXManifest.xml /f ..\resources.map.txt /p ..\bundle\contoso_demo.main.appx /o
    ```

4. 주 패키지가 생성되면, 다음 명령을 추가 언어당 한 번씩 사용합니다. 즉 이전 단계에서 생성한 각 언어 맵 파일에 대해 이 명령을 반복합니다. 마찬가지로 출력은 별도 디렉터리(주 패키지와 같은)에 있어야 합니다. 언어는 **옵션과** 옵션에 `/f`모두`/p` 지정되며, 원하는 리소스 패키지를 나타내는 새로운 `/r` 인수가 사용되었음을 유의하십시오.

    ```CMD
    makeappx pack /r /m .\AppXManifest.xml /f ..\resources.language-de.map.txt /p ..\bundle\contoso_demo.de.appx /o
    ```

5. 번들 디렉터리의 모든 패키지를 하나의 `.appxbundle` 파일로 결합합니다. 새 `/d` 옵션은 번들의 모든 파일에 사용되는 디렉터리를 지정합니다. 이전 단계에서 `.appx` 파일을 다른 위치에 저장한 이유가 여기에 있습니다.

    ```CMD
    makeappx bundle /d ..\bundle /p ..\contoso_demo.appxbundle /o
    ```

패키지 작성을 위한 마지막 단계는 서명입니다.

### <a name="step-32-signing-the-bundle"></a>3\.2단계: 번들에 서명

`.appxbundle` 파일을 만든 후에는(번들 생성기를 사용했거나 수동으로 만들었거나) 주 패키지와 모든 리소스 패키지를 포함하는 하나의 파일을 갖게 됩니다. 마지막 단계는 파일에 서명하여 Windows에서 이를 설치하도록 하는 것입니다.

```CMD
signtool sign /fd SHA256 /a /f ..\contoso_demo_key.pfx ..\contoso_demo.appxbundle
```

이렇게 하면 주 패키지와 모든 언어별 리소스 패키지가 포함된 서명된 `.appxbundle` 파일이 생성됩니다. 이 파일은 패키지 파일과 같이 두 번 클릭하여 설치할 수 있으며, 사용자의 Windows 언어 설정을 기반으로 적절한 언어로 설치됩니다.

## <a name="related-topics"></a>관련 항목

* [언어, 배율, 고대비 및 기타 한정자에 맞게 리소스 조정](tailor-resources-lang-scale-contrast.md)