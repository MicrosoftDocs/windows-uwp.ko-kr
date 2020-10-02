---
title: 변환 된 데스크톱 앱 및 게임에 MRT.LOG 사용
description: .NET 또는 Win32 앱 또는 게임을 .msix 또는 .appx 패키지로 패키징하면 리소스 관리 시스템을 활용하여 런타임 컨텍스트에 맞는 앱 리소스를 로드할 수 있습니다. 이 문서에서는 해당 기술에 대해 자세히 설명합니다.
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp, mrt.log, pri. 리소스, 게임, centennial, 데스크톱 앱 변환기, mui, 위성 어셈블리
ms.localizationpriority: medium
ms.openlocfilehash: b86cbcfcc5a6c6284b993dcad1325b108b1ab353
ms.sourcegitcommit: 6cb20dca1cb60b4f6b894b95dcc2cc3a166165ad
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/01/2020
ms.locfileid: "91636493"
---
# <a name="use-the-windows-10-resource-management-system-in-a-legacy-app-or-game"></a>레거시 앱 또는 게임에서 Windows 10 리소스 관리 시스템 사용

.NET 및 Win32 앱과 게임은 종종 다양 한 언어로 지역화 되어 총 주소 지정 가능 시장을 확장 합니다. 앱 지역화의 가치 제안에 대한 자세한 내용은 [세계화 및 지역화](../design/globalizing/globalizing-portal.md)를 참조하세요. .NET 또는 Win32 앱 또는 게임을 .msix 또는 .appx 패키지로 패키징하면 리소스 관리 시스템을 활용하여 런타임 컨텍스트에 맞는 앱 리소스를 로드할 수 있습니다. 이 문서에서는 해당 기술에 대해 자세히 설명합니다.

기존 Win32 응용 프로그램을 지역화 하는 방법에는 여러 가지가 있지만 Windows 8에는 응용 프로그램 형식에 따라 여러 프로그래밍 언어에서 작동 하는 [새로운 리소스 관리 시스템이](/previous-versions/windows/apps/jj552947(v=win.10)) 도입 되었으며 간단한 지역화를 통해 기능을 제공 합니다. 이 시스템은이 항목에서 "MRT.LOG"로 지칭 됩니다. 지금 까지는 "현대 리소스 기술"을 구현 "최신" 이라는 용어는 중단 되었습니다. 리소스 관리자는 MRM (최신 리소스 관리자) 또는 PRI (패키지 리소스 인덱스) 라고도 할 수 있습니다.

MSIX 기반 또는 .appx 기반 배포와 함께 (예: Microsoft Store에서) MRT.LOG는 응용 프로그램의 다운로드 및 설치 크기를 최소화 하는 지정 된 사용자/장치에 대해 가장 적합 한 리소스를 자동으로 배달할 수 있습니다. 이 크기 감소는 AAA 게임의 경우 몇 *기가바이트* 의 순서로 지역화 된 콘텐츠가 많은 응용 프로그램에 매우 중요할 수 있습니다. MRT.LOG의 추가 혜택에는 사용자의 기본 언어가 사용 가능한 리소스와 일치 하지 않는 경우 Windows Shell의 지역화 된 목록과 Microsoft Store 자동 대체 (fallback) 논리가 포함 됩니다.

이 문서에서는 MRT.LOG의 개략적인 아키텍처에 대해 설명 하 고, 최소한의 코드 변경으로 레거시 Win32 응용 프로그램을 MRT.LOG로 이동 하는 데 도움이 되는 포팅 가이드를 제공 합니다. MRT.LOG로 이동 하는 경우에는 추가 혜택 (예: 크기 조정 요소 또는 시스템 테마로 리소스를 분할 하는 기능)이 개발자에 게 제공 됩니다. MRT.LOG 기반 지역화는 데스크톱 브리지 (즉, "Centennial")에서 처리 하는 UWP 응용 프로그램 및 Win32 응용 프로그램 모두에 대해 작동 합니다.

대부분의 경우에는 런타임에 리소스를 확인 하 고 다운로드 크기를 최소화 하기 위해 MRT.LOG와 통합 하는 경우 기존 지역화 형식 및 소스 코드를 계속 사용할 수 있습니다. 즉, 전체 또는 없음 방법이 아닙니다. 다음 표에는 각 단계의 작업 및 예상 비용/혜택이 요약 되어 있습니다. 이 표에는 고해상도 또는 고대비 응용 프로그램 아이콘을 제공 하는 것과 같은 비 지역화 작업이 포함 되어 있지 않습니다. 타일, 아이콘 등에 대해 여러 자산을 제공 하는 방법에 대 한 자세한 내용은 [언어, 크기 조정, 고대비 및 기타 한정자에 대 한 리소스를 조정](tailor-resources-lang-scale-contrast.md)하는 방법을 참조 하세요.

<table>
<tr>
<th>작업</th>
<th>이점</th>
<th>예상 비용</th>
</tr>
<tr>
<td>패키지 매니페스트 지역화</td>
<td>지역화 된 콘텐츠가 Windows Shell 및 Microsoft Store에 표시 되는 데 필요한 운영 체제의 최소 작업</td>
<td>소형</td>
</tr>
<tr>
<td>MRT.LOG를 사용 하 여 리소스 식별 및 찾기</td>
<td>다운로드 및 설치 크기를 최소화 하기 위한 필수 구성 요소 자동 언어 대체</td>
<td>중간</td>
</tr>
<tr>
<td>빌드 리소스 팩</td>
<td>다운로드 및 설치 크기를 최소화 하는 마지막 단계</td>
<td>소형</td>
</tr>
<tr>
<td>MRT.LOG 리소스 형식 및 Api로 마이그레이션</td>
<td>파일 크기가 훨씬 작음 (기존 리소스 기술에 따라)</td>
<td>대형</td>
</tr>
</table>

## <a name="introduction"></a>소개

대다수의 응용 프로그램에는 응용 프로그램 코드에서 분리 된 *리소스* 라는 사용자 인터페이스 요소가 포함 됩니다 (소스 코드 자체에서 작성 된 하드 코드 된 *값* 과 대조). 예를 들어, 개발자가 아닌 사람이 하드 코딩 된 값을 사용 하 여 리소스를 선호 하는 이유는 여러 가지가 있습니다. 하지만 주요 이유 중 하나는 응용 프로그램이 런타임에 동일한 논리 리소스의 다른 표현을 선택 하도록 하는 것입니다. 예를 들어 단추에 표시 되는 텍스트 (또는 아이콘에 표시 되는 이미지)는 사용자가 이해 하는 언어, 표시 장치의 특성 또는 사용자에 게 사용 가능한 보조 기술을 사용 하는지 여부에 따라 다를 수 있습니다.

따라서 리소스 관리 기술의 주된 목적은 런타임에는 가능한 후보 집합 (예: " *resource name* `SAVE_BUTTON_LABEL` save", "Speichern" 또는 "저장")에서 가능한 가장 적합 한 실제 *값* (예: "저장")으로의 논리적 또는 기호화 된 리소스 이름 *candidates* 에 대 한 요청을 변환 하는 것입니다. MRT.LOG는 이러한 기능을 제공 하며, 응용 프로그램에서 사용자의 언어, 표시 배율, 사용자가 선택한 테마 및 기타 환경 요소와 같은 *한정자*라는 다양 한 특성을 사용 하 여 리소스 후보를 식별할 수 있도록 합니다. MRT.LOG는이를 필요로 하는 응용 프로그램에 대 한 사용자 지정 한정자를 지원 합니다. 예를 들어 응용 프로그램은 응용 프로그램의 모든 부분에 명시적으로이 체크를 추가 하지 않고 계정과 게스트 사용자를 사용 하 여 로그인 한 사용자에 게 다른 그래픽 자산을 제공할 수 있습니다. MRT.LOG는 파일 기반 리소스가 외부 데이터 (파일 자체)에 대 한 참조로 구현 되는 문자열 리소스와 파일 기반 리소스 모두에서 작동 합니다.

### <a name="example"></a>예제

다음은 두 단추 (및)에 텍스트 레이블이 있고 `openButton` `saveButton` 로고 ()에 사용 되는 PNG 파일을 포함 하는 응용 프로그램의 간단한 예입니다 `logoImage` . 텍스트 레이블은 영어와 독일어로 지역화 되며, 로고는 일반 바탕 화면 표시 (100% 배율 비율) 및 고해상도 휴대폰 (300% 배율 비율)에 최적화 되어 있습니다. 이 다이어그램은 모델의 개략적인 개념 보기를 제공 합니다. 정확히 구현에 매핑되지 않습니다.

:::image type="content" source="images\conceptual-resource-model.png" alt-text="원본 코드 레이블, 조회 테이블 레이블 및 디스크의 파일 레이블에 대 한 스크린샷&quot;:::

그래픽에서 응용 프로그램 코드는 세 가지 논리적 리소스 이름을 참조 합니다. 런타임에 `GetResource` 의사 (pseudo) 함수는 mrt.log를 사용 하 여 리소스 테이블 (PRI 파일 이라고 함)에서 이러한 리소스 이름을 확인 하 고 앰비언트 조건 (사용자 언어 및 디스플레이의 배율 요소)에 따라 가장 적합 한 후보를 찾습니다. 레이블의 경우 문자열이 직접 사용 됩니다. 로고 이미지의 경우 문자열은 파일 이름으로 해석 되 고 파일은 디스크에서 읽혀집니다. 

사용자가 영어 또는 독일어 이외의 언어를 사용 하거나 100% 또는 300% 이외의 표시 배율를 사용 하는 경우에는는 대체 규칙 집합을 기준으로 일치 하는 &quot;가장 가까운" 후보를 선택 합니다 (자세한 배경 정보는 [리소스 관리 시스템](/previous-versions/windows/apps/jj552947(v=win.10)) 참조).

MRT.LOG는 둘 이상의 한정자에 맞게 조정 되는 리소스를 지원 합니다. 예를 들어 로고 이미지에 지역화 해야 하는 포함 된 텍스트가 포함 된 경우 로고에는 EN/Scale-100, u n/배율-100, EN/배율-300 및 DE-DE/배율-300 이라는 4 개의 후보가 있습니다.

### <a name="sections-in-this-document"></a>이 문서의 섹션

다음 섹션에서는 응용 프로그램에 MRT.LOG를 통합 하는 데 필요한 개략적인 작업을 간략하게 설명 합니다.

#### <a name="phase-0-build-an-application-package"></a>0 단계: 응용 프로그램 패키지 빌드

이 섹션에서는 기존 데스크톱 응용 프로그램을 응용 프로그램 패키지로 만드는 방법을 간략하게 설명 합니다. 이 단계에서는 MRT.LOG 기능이 사용 되지 않습니다.

#### <a name="phase-1-localize-the-application-manifest"></a>1 단계: 응용 프로그램 매니페스트 지역화

이 섹션에서는 레거시 리소스 형식 및 API를 사용 하 여 리소스를 패키지 하 고 찾는 데 응용 프로그램의 매니페스트를 지역화 하는 방법 (Windows 셸에 올바르게 표시 됨)을 설명 합니다. 

#### <a name="phase-2-use-mrt-to-identify-and-locate-resources"></a>2 단계: MRT.LOG를 사용 하 여 리소스 식별 및 찾기

이 섹션에서는 기존 리소스 형식 및 Api를 사용 하 여 리소스를 로드 하 고 사용 하는 동안 응용 프로그램 코드 (및 리소스 레이아웃)를 수정 하 여 리소스를 검색 하는 방법을 간략하게 설명 합니다. 

#### <a name="phase-3-build-resource-packs"></a>3 단계: 리소스 팩 빌드

이 섹션에서는 리소스를 별도의 *리소스 팩*으로 분리 하 여 앱의 다운로드 및 설치 크기를 최소화 하는 데 필요한 최종 변경 내용을 간략하게 설명 합니다.

### <a name="not-covered-in-this-document"></a>이 문서에서 다루지 않음

위의 0-3 단계를 완료 한 후에는 Microsoft Store에 제출할 수 있는 "번들" 응용 프로그램이 필요 하지 않은 리소스 (예: 말하는 언어가 아닌 언어)를 생략 하 여 사용자에 대 한 다운로드 및 설치 크기를 최소화 하 게 됩니다. 응용 프로그램 크기 및 기능의 추가 향상은 최종 단계를 수행 하 여 수행할 수 있습니다.

#### <a name="phase-4-migrate-to-mrt-resource-formats-and-apis"></a>4 단계: MRT.LOG 리소스 형식 및 Api로 마이그레이션

이 단계는이 문서의 범위를 벗어나는 것입니다. 여기에는 리소스 (특히 문자열)를 MUI Dll 또는 .NET 리소스 어셈블리와 같은 레거시 형식에서 PRI 파일로 이동 하는 것이 수반 됩니다. 이로 인해 다운로드 & 설치 크기를 줄일 수 있습니다. 또한 크기 조정 인수, 내게 필요한 옵션 설정 등을 기준으로 이미지 파일의 다운로드 및 설치를 최소화 하는 등의 다른 MRT.LOG 기능을 사용할 수 있습니다.

## <a name="phase-0-build-an-application-package"></a>0 단계: 응용 프로그램 패키지 빌드

응용 프로그램의 리소스를 변경 하기 전에 먼저 현재 패키징 및 설치 기술을 표준 UWP 패키징 및 배포 기술로 바꾸어야 합니다. 이때 다음과 같은 세 가지 방법을 사용할 수 있습니다.

* 복잡 한 설치 관리자를 사용 하는 대용량 데스크톱 응용 프로그램이 있거나 많은 OS 확장성 지점이 사용 되는 경우 데스크톱 앱 변환기 도구를 사용 하 여 기존 앱 설치 관리자 (예: MSI)에서 UWP 파일 레이아웃 및 매니페스트 정보를 생성할 수 있습니다.
* 비교적 적은 수의 파일이 나 간단한 설치 관리자가 있고 확장성 후크가 없는 소규모 데스크톱 응용 프로그램을 사용 하는 경우 파일 레이아웃 및 매니페스트 정보를 수동으로 만들 수 있습니다.
* 원본에서 다시 작성 하는 경우 응용 프로그램을 순수 UWP 응용 프로그램으로 업데이트 하려면 Visual Studio에서 새 프로젝트를 만들고 IDE를 사용 하 여 많은 작업을 수행 합니다.

[데스크톱 앱 변환기](https://www.microsoft.com/store/p/desktopappconverter/9nblggh4skzw)를 사용 하려는 경우 변환 프로세스에 대 한 자세한 내용은 데스크톱 [앱 변환기를 사용 하 여 데스크톱 응용 프로그램 패키지](/windows/msix/desktop/desktop-to-uwp-run-desktop-app-converter) 를 참조 하세요. 데스크톱 변환기 샘플의 전체 집합은 [UWP 샘플 GitHub 리포지토리에 대 한 데스크톱 브리지](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)에서 찾을 수 있습니다.

패키지를 수동으로 만들려면 모든 응용 프로그램 파일 (실행 파일 및 내용, 소스 코드 제외) 및 패키지 매니페스트 파일 (. appxmanifest.xml)을 포함 하는 디렉터리 구조를 만들어야 합니다. 예제는 [Hello, 세계 GitHub 샘플](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/blob/master/Samples/HelloWorldSample/CentennialPackage/AppxManifest.xml)에서 확인할 수 있지만 라는 바탕 화면 실행 파일을 실행 하는 기본 패키지 매니페스트 파일 `ContosoDemo.exe` 은 다음과 같습니다. 여기서 <span style="background-color: yellow">강조 표시 된 텍스트</span> 는 사용자의 값으로 대체 됩니다.

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

패키지 매니페스트 파일 및 패키지 레이아웃에 대 한 자세한 내용은 [앱 패키지 매니페스트](/uwp/schemas/appxpackage/appx-package-manifest)를 참조 하세요.

마지막으로 Visual Studio를 사용 하 여 새 프로젝트를 만들고 기존 코드를로 마이그레이션하 [는 경우 "Hello, 세계" 앱 만들기](../get-started/create-a-hello-world-app-xaml-universal.md)를 참조 하세요. 기존 코드를 새 프로젝트에 포함할 수 있지만 순수 UWP 앱으로 실행 하기 위해 중요 한 코드를 변경 해야 할 수 있습니다 (특히 사용자 인터페이스에서). 이러한 변경 내용은이 문서의 범위를 벗어납니다.

## <a name="phase-1-localize-the-manifest"></a>1 단계: 매니페스트 지역화

### <a name="step-11-update-strings--assets-in-the-manifest"></a>1.1 단계: 매니페스트에서 자산 & 업데이트

0 단계에서 응용 프로그램에 대 한 기본 패키지 매니페스트 (appxmanifest.xml) 파일을 만들었습니다 (변환기에 제공 된 값에 따라, MSI에서 추출 되었거나 수동으로 매니페스트에 입력). 그러나 지역화 된 정보를 포함 하지 않으며 고해상도 시작 타일 자산과 같은 추가 기능을 지원 하지 않습니다.

응용 프로그램의 이름과 설명이 올바르게 지역화 되었는지 확인 하려면 리소스 파일 집합에 일부 리소스를 정의 하 고 패키지 매니페스트를 업데이트 하 여 참조 해야 합니다.

#### <a name="creating-a-default-resource-file"></a>기본 리소스 파일 만들기

첫 번째 단계는 기본 언어로 기본 리소스 파일을 만드는 것입니다 (예: 미국 영어). 텍스트 편집기를 사용 하거나 Visual Studio의 리소스 디자이너를 통해이 작업을 수동으로 수행할 수 있습니다.

리소스를 수동으로 만들려면 다음을 수행 합니다.

1. 이라는 XML 파일을 만들고 `resources.resw` `Strings\en-us` 프로젝트의 하위 폴더에 저장 합니다. 기본 언어가 영어 (미국)가 아닌 경우 적절 한 BCP-47 코드를 사용 합니다.
2. XML 파일에서 다음 콘텐츠를 추가 합니다. 여기서 <span style="background-color: yellow">강조 표시 된 텍스트</span> 는 기본 언어로 앱에 대 한 적절 한 텍스트로 바뀝니다.

> [!NOTE]
> 이러한 문자열의 길이에는 제한이 있습니다. 자세한 내용은 [Visualelements](/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements)를 참조 하세요.

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

Visual Studio에서 디자이너를 사용 하려면 다음을 수행 합니다.

1. `Strings\en-us`프로젝트에서 폴더 (또는 다른 언어)를 만들고 기본 이름인를 사용 하 여 프로젝트의 루트 폴더에 **새 항목** 을 추가 합니다 `resources.resw` . 리소스 **사전이** 아닌 리소스 **파일 (. resw)** 을 선택 해야 합니다. 리소스 사전은 XAML 응용 프로그램에서 사용 되는 파일입니다.
2. 디자이너를 사용 하 여 다음 문자열을 입력 합니다. 동일한를 사용 `Names` 하지만을 `Values` 응용 프로그램에 적합 한 텍스트로 바꿉니다.

:::image type="content" source="images\editing-resources-resw.png" alt-text="원본 코드 레이블, 조회 테이블 레이블 및 디스크의 파일 레이블에 대 한 스크린샷&quot;:::

그래픽에서 응용 프로그램 코드는 세 가지 논리적 리소스 이름을 참조 합니다. 런타임에 `GetResource` 의사 (pseudo) 함수는 mrt.log를 사용 하 여 리소스 테이블 (PRI 파일 이라고 함)에서 이러한 리소스 이름을 확인 하 고 앰비언트 조건 (사용자 언어 및 디스플레이의 배율 요소)에 따라 가장 적합 한 후보를 찾습니다. 레이블의 경우 문자열이 직접 사용 됩니다. 로고 이미지의 경우 문자열은 파일 이름으로 해석 되 고 파일은 디스크에서 읽혀집니다. 

사용자가 영어 또는 독일어 이외의 언어를 사용 하거나 100% 또는 300% 이외의 표시 배율를 사용 하는 경우에는는 대체 규칙 집합을 기준으로 일치 하는 &quot;가장 가까운" :::

> [!NOTE]
> Visual Studio designer를 사용 하 여 시작 하는 경우를 눌러 항상 XML을 직접 편집할 수 있습니다 `F7` . 하지만 최소한의 XML 파일로 시작 하는 경우 디자이너는 추가 메타 데이터가 너무 많기 때문에 *파일을 인식 하지 못합니다* . 디자이너에서 생성 된 파일의 상용구 XSD 정보를 직접 편집 된 XML 파일로 복사 하 여이 문제를 해결할 수 있습니다.

#### <a name="update-the-manifest-to-reference-the-resources"></a>리소스를 참조 하도록 매니페스트를 업데이트 합니다.

파일에 정의 된 값이 있는 후 `.resw` 다음 단계는 리소스 문자열을 참조 하도록 매니페스트를 업데이트 하는 것입니다. XML 파일을 직접 편집 하거나 Visual Studio 매니페스트 디자이너를 사용할 수도 있습니다.

XML을 직접 편집 하는 경우 파일을 열고 `AppxManifest.xml` <span style="background-color: lightgreen">강조 표시 된 값</span> 을 다음과 같이 변경 합니다 .이 텍스트는 응용 프로그램에 특정 한 텍스트가 아닌 *정확한* 텍스트를 사용 합니다. 이러한 정확한 리소스 이름을 사용할 필요는 없습니다 &mdash; . 직접 선택할 수 &mdash; 있지만 선택 하는 것은 파일에 있는 것과 정확히 일치 해야 합니다 `.resw` . 이러한 이름은 파일에서 만든과 일치 해야 합니다 .이 이름은 `Names` `.resw` `ms-resource:` 스키마와 네임 스페이스를 접두사로 사용 합니다 `Resources/` . 

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

:::image type="content" source="images\editing-application-info.png" alt-text="원본 코드 레이블, 조회 테이블 레이블 및 디스크의 파일 레이블에 대 한 스크린샷&quot;:::

그래픽에서 응용 프로그램 코드는 세 가지 논리적 리소스 이름을 참조 합니다. 런타임에 `GetResource` 의사 (pseudo) 함수는 mrt.log를 사용 하 여 리소스 테이블 (PRI 파일 이라고 함)에서 이러한 리소스 이름을 확인 하 고 앰비언트 조건 (사용자 언어 및 디스플레이의 배율 요소)에 따라 가장 적합 한 후보를 찾습니다. 레이블의 경우 문자열이 직접 사용 됩니다. 로고 이미지의 경우 문자열은 파일 이름으로 해석 되 고 파일은 디스크에서 읽혀집니다. 

사용자가 영어 또는 독일어 이외의 언어를 사용 하거나 100% 또는 300% 이외의 표시 배율를 사용 하는 경우에는는 대체 규칙 집합을 기준으로 일치 하는 &quot;가장 가까운" :::

:::image type="content" source="images\editing-packaging-info.png" alt-text="원본 코드 레이블, 조회 테이블 레이블 및 디스크의 파일 레이블에 대 한 스크린샷&quot;:::

그래픽에서 응용 프로그램 코드는 세 가지 논리적 리소스 이름을 참조 합니다. 런타임에 `GetResource` 의사 (pseudo) 함수는 mrt.log를 사용 하 여 리소스 테이블 (PRI 파일 이라고 함)에서 이러한 리소스 이름을 확인 하 고 앰비언트 조건 (사용자 언어 및 디스플레이의 배율 요소)에 따라 가장 적합 한 후보를 찾습니다. 레이블의 경우 문자열이 직접 사용 됩니다. 로고 이미지의 경우 문자열은 파일 이름으로 해석 되 고 파일은 디스크에서 읽혀집니다. 

사용자가 영어 또는 독일어 이외의 언어를 사용 하거나 100% 또는 300% 이외의 표시 배율를 사용 하는 경우에는는 대체 규칙 집합을 기준으로 일치 하는 &quot;가장 가까운" :::

### <a name="step-12-build-pri-file-make-an-msix-package-and-verify-its-working"></a>1.2 단계: PRI 파일을 빌드하고, MSIX 패키지를 만들고 작동 하는지 확인

이제 파일을 빌드하고 `.pri` 응용 프로그램을 배포 하 여 기본 언어로 된 올바른 정보가 시작 메뉴에 나타나는지 확인할 수 있습니다.

Visual Studio에서 빌드하는 경우를 눌러 프로젝트를 `Ctrl+Shift+B` 빌드한 다음 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 `Deploy` 상황에 맞는 메뉴에서 선택 하면 됩니다.

수동으로 빌드하는 경우 다음 단계를 수행 하 여 도구에 대 한 구성 파일을 만들고 `MakePRI` 파일 자체를 생성 합니다 `.pri` (추가 정보는 [수동 앱 포장](/windows/msix/package/manual-packaging-root)에서 찾을 수 있음).

1. 시작 메뉴의 **Visual studio 2017** 또는 **visual studio 2019** 폴더에서 개발자 명령 프롬프트를 엽니다.
2. 프로젝트 루트 디렉터리 (appxmanifest.xml 파일 및 **Strings** 폴더가 포함 된 디렉터리)로 전환 합니다.
3. 다음 명령을 입력 하 여 "contoso_demo.xml"을 프로젝트에 적합 한 이름으로 바꾸고 "en-us"를 앱의 기본 언어로 바꿉니다 (해당 하는 경우 en-us로 유지). XML 파일은 응용 프로그램에 포함 되지 않으므로 부모 디렉터리 (프로젝트 디렉터리가**아님** )에 생성 됩니다. 원하는 다른 디렉터리를 선택할 수 있지만 이후 명령에서 대체 해야 합니다.

    ```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US /pv 10.0 /o
    ```

    를 입력 `makepri createconfig /?` 하 여 각 매개 변수가 수행 하는 작업을 확인할 수 있습니다.
      * `/cf` 구성 파일 이름 (이 명령의 출력)을 설정 합니다.
      * `/dq` 기본 한정자 (이 경우 언어)를 설정 합니다. `en-US`
      * `/pv` 플랫폼 버전 (이 경우 Windows 10)을 설정 합니다.
      * `/o` 출력 파일이 있는 경우이를 덮어쓰도록 설정 합니다.

4. 이제 구성 파일이 있으므로를 `MakePRI` 다시 실행 하 여 디스크에서 리소스를 실제로 검색 하 고이를 PRI 파일로 패키지 합니다. "contoso_demop.xml"을 이전 단계에서 사용한 XML 파일 이름으로 바꾸고 입력 및 출력 모두에 대해 부모 디렉터리를 지정 해야 합니다. 

    ```CMD
    makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
    ```

    를 입력 `makepri new /?` 하 여 각 매개 변수가 수행 하는 작업을 확인할 수 있습니다.
      * `/pr` 프로젝트 루트 (이 경우 현재 디렉터리)를 설정 합니다.
      * `/cf` 이전 단계에서 만든 구성 파일 이름을 설정 합니다.
      * `/of` 출력 파일을 설정 합니다. 
      * `/mf` 매핑 파일을 만듭니다. 따라서 이후 단계에서 패키지의 파일을 제외할 수 있습니다.
      * `/o` 출력 파일이 있는 경우이를 덮어쓰도록 설정 합니다.

5. 이제 `.pri` 기본 언어 리소스 (예: en-us)를 포함 하는 파일이 있습니다. 제대로 작동 했는지 확인 하려면 다음 명령을 실행할 수 있습니다.

    ```CMD
    makepri dump /if ..\resources.pri /of ..\resources /o
    ```

    를 입력 `makepri dump /?` 하 여 각 매개 변수가 수행 하는 작업을 확인할 수 있습니다.
      * `/if` 입력 파일 이름을 설정 합니다. 
      * `/of` 출력 파일 이름을 설정 합니다 ( `.xml` 자동으로 추가 됨).
      * `/o` 출력 파일이 있는 경우이를 덮어쓰도록 설정 합니다.

6. 마지막으로, `..\resources.xml` 텍스트 편집기에서을 `<NamedResource>` (를) 열고 선택한 기본 언어의 값과 함께 값 (예: 및)을 나열 하는지 확인할 수 있습니다 `ApplicationDescription` `PublisherDisplayName` `<Candidate>` . 파일의 시작 부분에는 다른 콘텐츠가 있습니다. 지금은 무시 됩니다.

매핑 파일을 열어 `..\resources.map.txt` 프로젝트에 필요한 파일이 포함 되어 있는지 확인할 수 있습니다 (프로젝트 디렉터리의 일부가 아닌 PRI 파일 포함). 중요 한 점은 파일 *not* `resources.resw` 의 내용이 이미 PRI 파일에 포함 되어 있기 때문에 매핑 파일에 파일에 대 한 참조가 포함 되지 않습니다. 그러나 이미지의 파일 이름과 같은 다른 리소스를 포함 하 게 됩니다.

#### <a name="building-and-signing-the-package"></a>패키지 작성 및 서명 

이제 PRI 파일이 빌드되어 패키지를 작성 하 고 서명할 수 있습니다.

1. 앱 패키지를 만들려면 다음 명령을 실행 하 여 `contoso_demo.appx` 만들려는. a s d/.appx 파일의 이름으로 바꾸고 파일에 대해 다른 디렉터리를 선택 해야 합니다 .이 샘플은 부모 디렉터리를 사용 하는 것은 물론 어디에 나 프로젝트 디렉터리가 될 수 **없습니다** .

    ```CMD
    makeappx pack /m AppXManifest.xml /f ..\resources.map.txt /p ..\contoso_demo.appx /o
    ```

    를 입력 `makeappx pack /?` 하 여 각 매개 변수가 수행 하는 작업을 확인할 수 있습니다.
      * `/m` 사용할 매니페스트 파일을 설정 합니다.
      * `/f` 사용할 매핑 파일을 설정 합니다 (이전 단계에서 만들어짐). 
      * `/p` 출력 패키지 이름을 설정 합니다.
      * `/o` 출력 파일이 있는 경우이를 덮어쓰도록 설정 합니다.

2. 패키지를 만든 후에는 서명 해야 합니다. 서명 인증서를 가져오는 가장 쉬운 방법은 Visual Studio에서 빈 유니버설 Windows 프로젝트를 만들고 해당 프로젝트를 만드는 파일을 복사 하는 것입니다 `.pfx` `MakeCert` `Pvk2Pfx` . 하지만 [앱 패키지 서명 인증서를 만드는 방법](/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate)에 설명 된 대로 및 유틸리티를 사용 하 여 수동으로 만들 수 있습니다.

    > [!IMPORTANT]
    > 서명 인증서를 수동으로 만드는 경우 원본 프로젝트 또는 패키지 원본과 다른 디렉터리에 파일을 저장 해야 합니다. 그렇지 않으면 개인 키를 포함 하 여 패키지의 일부로 포함 될 수 있습니다.

3. 패키지에 서명 하려면 다음 명령을 사용 합니다. `Publisher`의 요소에 지정 된는 `Identity` `AppxManifest.xml` 인증서의와 일치 해야 합니다 `Subject` .이 요소는 사용자에 **not** 게 표시 되는 `<PublisherDisplayName>` 지역화 된 표시 이름입니다. 일반적인 방법으로 파일 이름을 `contoso_demo...` 프로젝트에 적절 한 이름으로 바꾸고 (**매우 중요**) `.pfx` 파일이 현재 디렉터리에 있지 않은지 확인 합니다. 그렇지 않은 경우 개인 서명 키를 포함 하 여 패키지의 일부로 생성 됩니다.

    ```CMD
    signtool sign /fd SHA256 /a /f ..\contoso_demo_key.pfx ..\contoso_demo.appx
    ```

    를 입력 `signtool sign /?` 하 여 각 매개 변수가 수행 하는 작업을 확인할 수 있습니다.
      * `/fd` 파일 다이제스트 알고리즘을 설정 합니다. 즉, SHA256의 기본값은 SHA256입니다.
      * `/a` 에서 자동으로 최상의 인증서를 선택 합니다.
      * `/f` 서명 인증서를 포함 하는 입력 파일을 지정 합니다.

마지막으로, 파일을 두 번 클릭 하 여 `.appx` 설치할 수 있습니다. 또는 명령줄을 선호 하는 경우 PowerShell 프롬프트를 열고 패키지가 포함 된 디렉터리로 변경한 후 다음을 입력 합니다 ( `contoso_demo.appx` 패키지 이름으로 대체).

```CMD
add-appxpackage contoso_demo.appx
```

신뢰할 수 없는 인증서에 대 한 오류가 발생 하는 경우 해당 인증서가 사용자 저장소가**아닌** 컴퓨터 저장소에 추가 되었는지 확인 합니다. 컴퓨터 저장소에 인증서를 추가 하려면 명령줄 또는 Windows 탐색기를 사용할 수 있습니다.

명령줄을 사용 하려면 다음을 수행 합니다.

1. 관리자 권한으로 Visual Studio 2017 또는 Visual Studio 2019 명령 프롬프트를 실행 합니다.
2. 파일이 포함 된 디렉터리로 전환 합니다 `.cer` (원본 또는 프로젝트 디렉터리 외부에 있는지 확인 해야 함).
3. 다음 명령을 입력 하 여를 `contoso_demo.cer` 파일 이름으로 바꿉니다.
    ```CMD
    certutil -addstore TrustedPeople contoso_demo.cer
    ```
    
    를 실행 `certutil -addstore /?` 하 여 각 매개 변수가 수행 하는 작업을 확인할 수 있지만 간단히 확인할 수는 있습니다.
      * `-addstore` 인증서 저장소에 인증서를 추가 합니다.
      * `TrustedPeople` 인증서가 배치 된 저장소를 나타냅니다.

Windows 탐색기를 사용 하려면 다음을 수행 합니다.

1. 파일이 포함 된 폴더로 이동 합니다. `.pfx`
2. 파일을 두 번 클릭 `.pfx` 하면 **Certicicate 가져오기 마법사** 가 나타납니다.
3. `Local Machine`을 선택 하 고 클릭 합니다.`Next`
4. 사용자 계정 컨트롤 관리자 권한 상승 프롬프트 (표시 되는 경우)를 수락 하 고 다음을 클릭 합니다. `Next`
5. 개인 키에 대 한 암호 (있는 경우)를 입력 하 고 다음을 클릭 합니다. `Next`
6. `Place all certificates in the following store` 선택
7. `Browse`을 클릭 하 고 폴더를 선택 합니다 `Trusted People` ("신뢰할 수 있는 게시자"가**아님** ).
8. 클릭 한 `Next` 다음 `Finish`

저장소에 인증서를 추가한 후에 `Trusted People` 는 패키지를 다시 설치 해 보십시오.

이제 앱이 시작 메뉴의 "모든 앱" 목록에 표시 되 고 파일의 올바른 정보가 표시 됩니다 `.resw`  /  `.pri` . 빈 문자열이 나 문자열이 표시 되는 경우 문제가 발생 한 `ms-resource:...` 것입니다. 편집 내용을 확인 하 고 올바른지 확인 합니다. 시작 메뉴에서 앱을 마우스 오른쪽 단추로 클릭 하면 타일로 고정 하 고 올바른 정보가 표시 되는지 확인할 수 있습니다.

### <a name="step-13-add-more-supported-languages"></a>1.3 단계: 지원 되는 언어 추가

패키지 매니페스트를 변경 하 고 초기 파일을 만든 후에 `resources.resw` 는 다른 언어를 추가 하는 것이 쉽습니다.

#### <a name="create-additional-localized-resources"></a>지역화 된 추가 리소스 만들기

먼저, 지역화 된 추가 리소스 값을 만듭니다. 

폴더 내에서 `Strings` 적절 한 BCP-47 코드 (예:)를 사용 하 여 지원 되는 각 언어에 대 한 추가 폴더를 만듭니다 `Strings\de-DE` . 이러한 각 폴더 내에서 `resources.resw` 번역 된 리소스 값을 포함 하는 파일 (XML 편집기 또는 Visual Studio 디자이너를 사용 하 여)을 만듭니다. 지역화 된 문자열을 다른 위치에서 사용할 수 있는 것으로 가정 하 고 파일에 복사 하기만 하면 됩니다. `.resw` 이 문서는 변환 단계 자체를 포함 하지 않습니다. 

예를 들어 `Strings\de-DE\resources.resw` 파일이 다음과 같이 표시 되 고 <span style="background-color: yellow">강조 표시 된 텍스트가</span> 에서 변경 됩니다 `en-US` .

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

다음 단계에서는 및 둘 다에 대해 리소스를 추가 했다고 가정 `de-DE` `fr-FR` 하지만 모든 언어에 대해 동일한 패턴을 따를 수 있습니다.

#### <a name="update-the-package-manifest-to-list-supported-languages"></a>패키지 매니페스트를 업데이트 하 여 지원 되는 언어 나열

앱에서 지 원하는 언어를 나열 하려면 패키지 매니페스트를 업데이트 해야 합니다. 데스크톱 앱 변환기는 기본 언어를 추가 하지만 다른 언어는 명시적으로 추가 해야 합니다. 파일을 직접 편집 하는 경우 `AppxManifest.xml` `Resources` 다음과 같이 노드를 업데이트 하 고, 필요한 만큼 요소를 추가 하 고, 지원 되는 적절 한 <span style="background-color: yellow">언어</span> 를 대체 하 고, 목록의 첫 번째 항목이 기본 (대체) 언어 인지 확인 합니다. 이 예에서 기본값은 영어 (미국)로 독일어 (독일)와 프랑스어 (프랑스) 모두에 대 한 추가 지원을 제공 합니다.

```xml
<Resources>
  <Resource Language="EN-US" />
  <Resource Language="DE-DE" />
  <Resource Language="FR-FR" />
</Resources>
```

Visual Studio를 사용 하는 경우에는 아무것도 수행할 필요가 없습니다. 사용자에 게 `Package.appxmanifest` 표시 되는 경우 특별 한 <span style="background-color: yellow">x 생성</span> 값이 표시 됩니다. 이렇게 하면 빌드 프로세스에서 프로젝트에 검색 한 언어 (BCP-47 코드를 사용 하 여 이름이 지정 된 폴더를 기준으로 함)를 삽입 합니다. 실제 패키지 매니페스트에 대 한 올바른 값이 아닙니다. Visual Studio 프로젝트에 대해서만 작동 합니다.

```xml
<Resources>
  <Resource Language="x-generate" />
</Resources>
```

#### <a name="re-build-with-the-localized-values"></a>지역화 된 값을 사용 하 여 다시 빌드

이제 응용 프로그램을 빌드 및 배포 하 고, Windows에서 언어 기본 설정을 변경 하는 경우 새로 지역화 된 값이 시작 메뉴에 표시 됩니다 (언어를 변경 하는 방법에 대 한 지침은 아래 참조).

Visual Studio의 경우을 (를) 사용 하 여 `Ctrl+Shift+B` 빌드한 다음 프로젝트를 마우스 오른쪽 단추로 클릭 하면 `Deploy` 됩니다.

프로젝트를 수동으로 빌드하는 경우, 구성 파일을 만들 때 위와 동일한 단계를 수행 하지만 밑줄로 구분 된 추가 언어를 기본 한정자 목록 ()에 추가 합니다 `/dq` . 예를 들어 이전 단계에서 추가한 영어, 독일어 및 프랑스어 리소스를 지원할 수 있습니다.

```CMD
makepri createconfig /cf ..\contoso_demo.xml /dq en-US_de-DE_fr-FR /pv 10.0 /o
```

이렇게 하면 테스트에 사용할 수 있는 모든 지정 된 languagesthat를 포함 하는 PRI 파일이 만들어집니다. 리소스의 총 크기가 작은 경우 또는 적은 수의 언어만 지 원하는 경우이는 배송 앱에 허용 될 수 있습니다. 별도의 언어 팩을 빌드하는 추가 작업을 수행 하는 데 필요한 리소스의 설치/다운로드 크기를 최소화 하는 이점을 원하는 경우에만 해당 됩니다.

#### <a name="test-with-the-localized-values"></a>지역화 된 값으로 테스트

새 지역화 된 변경 내용을 테스트 하려면 Windows에 새로운 기본 UI 언어를 추가 하기만 하면 됩니다. 언어 팩을 다운로드 하거나, 시스템을 다시 부팅 하거나, 전체 Windows UI를 외부 언어로 표시 하지 않아도 됩니다. 

1. 앱 실행 `Settings` ( `Windows + I` )
2. [https://editor.swagger.io](`Time & language` ) 으로 이동합니다.
3. [https://editor.swagger.io](`Region & language` ) 으로 이동합니다.
4. `Add a language`을 클릭합니다.
5. 원하는 언어를 입력 하거나 선택 합니다 (예: `Deutsch` 또는 `German` ).
 * 하위 언어가 있으면 원하는 항목을 선택 합니다 (예: `Deutsch / Deutschland` ).
6. 언어 목록에서 새 언어를 선택 합니다.
7. `Set as default`을 클릭합니다.

이제 시작 메뉴를 열고 응용 프로그램을 검색 하면 선택한 언어에 대 한 지역화 된 값이 표시 됩니다. 다른 앱은 지역화 된 것으로 나타날 수도 있습니다. 지역화 된 이름이 바로 표시 되지 않으면 시작 메뉴의 캐시가 새로 고쳐질 때까지 몇 분 정도 기다립니다. 네이티브 언어로 돌아가려면 언어 목록에서 기본 언어로 지정 하면 됩니다. 

### <a name="step-14-localizing-more-parts-of-the-package-manifest-optional"></a>1.4 단계: 패키지 매니페스트의 추가 파트 지역화 (선택 사항)

패키지 매니페스트의 다른 섹션을 지역화할 수 있습니다. 예를 들어 응용 프로그램에서 파일 확장명을 처리 하는 경우, `windows.fileTypeAssociation` 표시 된 대로 <span style="background-color: lightgreen">녹색으로 강조 표시 된 텍스트</span> 를 사용 하 고 (리소스 참조), <span style="background-color: yellow">노란색 강조</span> 표시 된 텍스트를 응용 프로그램에 대 한 정보로 바꿔서 매니페스트에 확장이 있어야 합니다.

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

또한 탭을 사용 하 여 `Declarations` <span style="background-color: lightgreen">강조 표시 된 값</span>을 기록 하 고, Visual Studio 매니페스트 디자이너를 사용 하 여이 정보를 추가할 수 있습니다.

:::image type="content" source="images\editing-declarations-info.png" alt-text="원본 코드 레이블, 조회 테이블 레이블 및 디스크의 파일 레이블에 대 한 스크린샷&quot;:::

그래픽에서 응용 프로그램 코드는 세 가지 논리적 리소스 이름을 참조 합니다. 런타임에 `GetResource` 의사 (pseudo) 함수는 mrt.log를 사용 하 여 리소스 테이블 (PRI 파일 이라고 함)에서 이러한 리소스 이름을 확인 하 고 앰비언트 조건 (사용자 언어 및 디스플레이의 배율 요소)에 따라 가장 적합 한 후보를 찾습니다. 레이블의 경우 문자열이 직접 사용 됩니다. 로고 이미지의 경우 문자열은 파일 이름으로 해석 되 고 파일은 디스크에서 읽혀집니다. 

사용자가 영어 또는 독일어 이외의 언어를 사용 하거나 100% 또는 300% 이외의 표시 배율를 사용 하는 경우에는는 대체 규칙 집합을 기준으로 일치 하는 &quot;가장 가까운" :::

이제 각 파일에 해당 리소스 이름을 추가 하 여 `.resw` <span style="background-color: yellow">강조 표시 된 텍스트</span> 를 앱에 대 한 적절 한 텍스트로 바꿉니다 ( *지원 되는 각 언어*에 대해이 작업을 수행 해야 함).

```xml
... existing content...
<data name="FileTypeDisplayName">
  <value>Contoso Demo File</value>
</data>
<data name="FileTypeInfoTip">
  <value>Files used by Contoso Demo App</value>
</data>
```

그러면 파일 탐색기와 같은 Windows 셸의 일부가 표시 됩니다.

:::image type="content" source="images\file-type-tool-tip.png" alt-text="원본 코드 레이블, 조회 테이블 레이블 및 디스크의 파일 레이블에 대 한 스크린샷&quot;:::

그래픽에서 응용 프로그램 코드는 세 가지 논리적 리소스 이름을 참조 합니다. 런타임에 `GetResource` 의사 (pseudo) 함수는 mrt.log를 사용 하 여 리소스 테이블 (PRI 파일 이라고 함)에서 이러한 리소스 이름을 확인 하 고 앰비언트 조건 (사용자 언어 및 디스플레이의 배율 요소)에 따라 가장 적합 한 후보를 찾습니다. 레이블의 경우 문자열이 직접 사용 됩니다. 로고 이미지의 경우 문자열은 파일 이름으로 해석 되 고 파일은 디스크에서 읽혀집니다. 

사용자가 영어 또는 독일어 이외의 언어를 사용 하거나 100% 또는 300% 이외의 표시 배율를 사용 하는 경우에는는 대체 규칙 집합을 기준으로 일치 하는 &quot;가장 가까운":::

새 UI 문자열을 표시 해야 하는 새 시나리오를 실행 하기 전에 패키지를 빌드하고 테스트 합니다.

## <a name="phase-2-use-mrt-to-identify-and-locate-resources"></a>2 단계: MRT.LOG를 사용 하 여 리소스 식별 및 찾기

이전 섹션에서는 Windows 셸이 응용 프로그램의 이름 및 기타 메타 데이터를 올바르게 표시할 수 있도록 MRT.LOG를 사용 하 여 앱의 매니페스트 파일을 지역화 하는 방법을 살펴보았습니다. 이 경우 코드를 변경할 필요가 없습니다. 단지 `.resw` 파일 및 몇 가지 추가 도구를 사용 해야 합니다. 이 섹션에서는 MRT.LOG를 사용 하 여 기존 리소스 형식에서 리소스를 찾고 최소한의 변경으로 기존 리소스 처리 코드를 사용 하는 방법을 보여 줍니다.

### <a name="assumptions-about-existing-file-layout--application-code"></a>응용 프로그램 코드 & 기존 파일 레이아웃에 대 한 가정

Win32 데스크톱 앱을 지역화 하는 방법에는 여러 가지가 있으므로이 문서에서는 특정 환경에 매핑해야 하는 기존 응용 프로그램의 구조에 대해 몇 가지 사항을 단순화 합니다. MRT.LOG의 요구 사항을 준수 하기 위해 기존 코드 베이스 또는 리소스 레이아웃을 일부 변경 해야 할 수 있으며,이는이 문서의 범위를 벗어났습니다.

#### <a name="resource-file-layout"></a>리소스 파일 레이아웃

이 문서에서는 지역화 된 리소스의 파일 이름 (예: `contoso_demo.exe.mui` 또는 `contoso_strings.dll` 또는)이 동일 `contoso.strings.xml` 하지만 BCP-47 이름 (, 등)이 있는 다른 폴더에 배치 되어 있다고 `en-US` 가정 합니다 `de-DE` . 보유 하 고 있는 리소스 파일의 수, 해당 이름, 파일 형식/관련 Api의 정의 등은 중요 하지 않습니다. 중요 한 점은 모든 *논리적* 리소스의 파일 이름이 동일 하지만 다른 *실제* 디렉터리에 배치 된다는 것입니다. 

예를 들어, 응용 프로그램에서 및 파일을 포함 하는 단일 디렉터리를 포함 하는 플랫 파일 구조를 사용 하는 경우, `Resources` `english_strings.dll` 이는 `french_strings.dll` mrt.log에 잘 매핑되지 않습니다. 더 나은 구조는 하위 디렉터리 `Resources` 및 파일과를 사용 하는 디렉터리 `en\strings.dll` `fr\strings.dll` 입니다. 같은 기본 파일 이름을 사용할 수도 있지만 및과 같은 포함 된 한정자를 사용할 수도 `strings.lang-en.dll` `strings.lang-fr.dll` 있지만 언어 코드를 포함 하는 디렉터리를 사용 하는 것은 개념적으로 간단 하므로 중점적으로 살펴볼 것입니다.

>[!NOTE]
> 이 파일 명명 규칙을 따르지 않는 경우에도 MRT.LOG 및 패키징을 사용할 수 있습니다. 더 많은 작업이 필요 합니다.

예를 들어, 응용 프로그램에는 <span style="background-color: yellow">UICommands</span> 폴더 아래에 배치 된 <span style="background-color: yellow">ui.txt</span>라는 간단한 텍스트 파일에 사용자 지정 UI 명령 (단추 레이블 등에 사용 됨) 집합이 있을 수 있습니다.

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

이 문서에서는 코드의 특정 지점에서 지역화 된 리소스를 포함 하는 파일을 찾고 로드 한 다음 사용 한다고 가정 합니다. 리소스를 로드 하는 데 사용 되는 Api, 리소스를 추출 하는 데 사용 되는 Api 등은 중요 하지 않습니다. 의사 코드에는 기본적으로 세 가지 단계가 있습니다.

<blockquote>
<pre>
set userLanguage = GetUsersPreferredLanguage()
set resourceFile = FindResourceFileForLanguage(MY_RESOURCE_NAME, userLanguage)
set resource = LoadResource(resourceFile) 
    
// now use 'resource' however you want
</pre>
</blockquote>

MRT.LOG만이 프로세스의 처음 두 단계를 변경 해야 합니다. 가장 적합 한 후보 리소스와 검색 방법을 결정 하는 방법입니다. 리소스를 활용 하려는 경우에는 리소스를 로드 하거나 사용 하는 방법을 변경 하지 않아도 됩니다.

예를 들어 응용 프로그램은 Win32 API `GetUserPreferredUILanguages` , CRT 함수 `sprintf` 및 Win32 API를 사용 하 여 `CreateFile` 위의 세 가지 의사 코드 함수를 바꾼 다음, 쌍을 찾는 텍스트 파일을 수동으로 구문 분석할 `name=value` 수 있습니다. 세부 정보는 중요 하지 않습니다 .이는 해당 리소스가 있는 리소스를 처리 하는 데 사용 되는 기술에 영향을 주지 않는다는 것을 보여 주기 위한 것입니다.

### <a name="step-21-code-changes-to-use-mrt-to-locate-files"></a>2.1 단계: 파일을 찾기 위해 MRT.LOG를 사용 하도록 코드 변경

리소스를 찾기 위해 MRT.LOG를 사용 하도록 코드를 전환 하는 것은 어려운 일입니다. 약간의 WinRT 형식과 몇 줄의 코드를 사용 해야 합니다. 사용할 기본 유형은 다음과 같습니다.

* 현재 활성 한정자 값 집합 (언어, 배율 인수 등)을 캡슐화 하는 [Resourcecontext](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceContext)
* [ResourceManager](/uwp/api/windows.applicationmodel.resources.core.resourcemanager) (.net 버전이 아닌 WinRT 버전)는 PRI 파일의 모든 리소스에 액세스할 수 있도록 합니다.
* [Resourcemap](/uwp/api/windows.applicationmodel.resources.core.resourcemap)에 있는 리소스의 특정 하위 집합 (이 예제에서는 파일 기반 리소스 및 문자열 리소스)을 나타내는 resourcemap
* 논리적 리소스 및 모든 가능한 후보를 나타내는 [Namedresource](/uwp/api/Windows.ApplicationModel.Resources.Core.NamedResource)
* [ResourceCandidate](/uwp/api/windows.applicationmodel.resources.core.resourcecandidate)-구체적인 단일 후보 리소스를 나타냅니다. 

의사 코드에서 지정 된 리소스 파일 이름을 확인 하는 방법 (예 `UICommands\ui.txt` : 위의 샘플)은 다음과 같습니다.

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

특히 파일이 디스크에 있는 방법 이더라도 코드에서 특정 언어 폴더를 요청 **하지** 않습니다 `UICommands\en-US\ui.txt` . 대신 *논리적* 파일 이름에 대해 묻는 메시지를 표시 하 `UICommands\ui.txt` 고 mrt.log를 사용 하 여 언어 디렉터리 중 하나에서 적절 한 디스크 파일을 찾습니다.

여기에서 샘플 앱은를 계속 사용 하 여 `CreateFile` 를 로드 하 `absoluteFileName` 고 이전 처럼 쌍을 구문 분석할 수 있습니다 `name=value` . 앱에서 해당 논리를 변경 하지 않아도 됩니다. C # 또는 c + +/CX로 작성 하는 경우 실제 코드는이 보다 훨씬 복잡 하지 않습니다. 즉, 대부분의 중간 변수가 생략 수 있습니다. **.net 리소스 로드**에 대 한 섹션을 참조 하십시오. C + +/WRL-based 응용 프로그램은 WinRT Api를 활성화 하 고 호출 하는 데 사용 되는 하위 수준 COM 기반 Api로 인해 더 복잡 하지만 수행 하는 기본 단계는 동일 합니다. 아래의 **WIN32 MUI 리소스 로드**에 대 한 섹션을 참조 하세요.

#### <a name="loading-net-resources"></a>.NET 리소스 로드

.NET에는 리소스 ("위성 어셈블리")를 찾고 로드 하는 메커니즘이 기본 제공 되기 때문에 위의 가상 예제에서와 같이 바꿀 명시적인 코드가 없습니다. .NET에서는 적절 한 디렉터리에 리소스 Dll이 필요 하 고 자동으로 배치 됩니다. 앱이 리소스 팩을 사용 하 여 MSIX 또는 .appx로 패키지 되는 경우 디렉터리 구조는 리소스 디렉터리가 주 응용 프로그램 디렉터리의 하위 디렉터리가 되도록 하는 것이 아니라 약간 다릅니다. 즉, 사용자가 기본 설정에 나열 된 언어가 없는 경우에는 제공 되지 않습니다. 

예를 들어 모든 파일이 폴더 아래에 있는 다음 레이아웃을 사용 하는 .NET 응용 프로그램을 가정해 보겠습니다 `MainApp` .

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

를 .appx로 변환한 후 레이아웃은 다음과 같이 표시 됩니다 .이는 `en-US` 기본 언어이 고 사용자가 해당 언어 목록에 독일어와 프랑스어를 모두 사용 한다고 가정 합니다.

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

지역화 된 리소스는 주 실행 파일의 설치 위치 아래의 하위 디렉터리에 더 이상 존재 하지 않으므로 기본 제공 .NET 리소스 확인이 실패 합니다. 다행히 .NET에는 실패 한 어셈블리 로드 시도를 처리 하기 위한 잘 정의 된 메커니즘인 `AssemblyResolve` 이벤트가 있습니다. MRT.LOG를 사용 하는 .NET 앱은이 이벤트에 등록 하 고 .NET 리소스 하위 시스템에 대해 누락 된 어셈블리를 제공 해야 합니다. 

WinRT Api를 사용 하 여 .NET에서 사용 되는 위성 어셈블리를 찾는 방법에 대 한 간단한 예제는 다음과 같습니다. 제공 된 코드는 최소한의 구현을 보여 주기 위해 의도적으로 압축 되어 있습니다 .이는 위의 의사 코드와 밀접 하 게 매핑되는 것을 볼 수 있지만, 전달 된를 사용 하 여 `ResolveEventArgs` 찾아야 하는 어셈블리의 이름을 제공 하는 것을 볼 수 있습니다. 이 코드의 실행 가능한 버전 (자세한 설명 및 오류 처리)은 `PriResourceRsolver.cs` [GitHub의 **.net 어셈블리 확인자** 샘플](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DotNetSatelliteAssemblyDemo)에 있는 파일에서 찾을 수 있습니다.

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

    // Note use of 'UnsafeLoadFrom' - this is required for apps installed with .appx, but
    // in general is discouraged. The full sample provides a safer wrapper of this method
    return Assembly.UnsafeLoadFrom(resource.Resolve(resourceContext).ValueAsString);
  }
}
```

위의 클래스가 지정 된 경우 응용 프로그램의 시작 코드에서 다음 위치를 초기에 추가 합니다 (지역화 된 리소스를 로드 해야 하기 전).

```csharp
void EnableMrtResourceLookup()
{
  AppDomain.CurrentDomain.AssemblyResolve += PriResourceResolver.ResolveResourceDll;
}
```

.NET 런타임은 `AssemblyResolve` 리소스 dll을 찾을 수 없을 때마다 이벤트를 발생 시킵니다. 그러면 제공 된 이벤트 처리기가 mrt.log를 통해 원하는 파일을 찾고 어셈블리를 반환 합니다.

> [!NOTE]
> 앱에 `AssemblyResolve` 다른 용도에 대 한 처리기가 이미 있는 경우 리소스 확인 코드를 기존 코드와 통합 해야 합니다.

#### <a name="loading-win32-mui-resources"></a>Win32 MUI 리소스 로드

Win32 MUI 리소스를 로드 하는 것은 기본적으로 .NET 위성 어셈블리를 로드 하는 것과 같지만, 대신 c + +/CX 또는 c + +/WRL 코드를 사용 합니다 C + +/CX를 사용 하면 위의 c # 코드와 거의 일치 하는 훨씬 간단한 코드를 사용할 수 있지만 c + + 언어 확장, 컴파일러 스위치 및 추가 런타임 overheard을 사용 하지 않을 수 있습니다. 해당 하는 경우 c + +/WRL를 사용 하면 보다 자세한 코드를 제공 하는 비용으로 훨씬 낮은 영향을 주는 솔루션이 제공 됩니다. 그럼에도 불구 하 고 ATL 프로그래밍 (또는 일반적으로 COM)에 익숙한 경우 WRL는 잘 알고 있어야 합니다. 

다음 샘플 함수는 c + +/WRL를 사용 하 여 특정 리소스 DLL을 로드 하 고 `HINSTANCE` 일반적인 Win32 리소스 api를 사용 하 여 추가 리소스를 로드 하는 데 사용할 수 있는를 반환 하는 방법을 보여 줍니다. 를 .NET 런타임에서 요청한 언어로 명시적으로 초기화 하는 c # 샘플과 달리 `ResourceContext` 이 코드는 사용자의 현재 언어에 의존 합니다.

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

## <a name="phase-3-building-resource-packs"></a>3 단계: 리소스 팩 빌드

이제 모든 리소스를 포함 하는 "fat 팩"이 있으므로 다운로드 및 설치 크기를 최소화 하기 위해 별도의 주 패키지와 리소스 패키지를 작성 하는 데는 두 가지 경로가 있습니다.

* 기존 fat 팩을 사용 하 고 [번들 생성기 도구](https://www.microsoft.com/store/apps/9nblggh43pmq) 를 통해 실행 하 여 리소스 팩을 자동으로 만듭니다. 이는 이미 fat 팩을 생성 하는 빌드 시스템이 있고이를 사후 처리 하 여 리소스 팩을 생성 하려는 경우에 선호 되는 방법입니다.
* 개별 리소스 패키지를 직접 생성 하 고 번들로 빌드합니다. 빌드 시스템을 보다 세부적으로 제어 하 고 패키지를 직접 작성할 수 있는 경우이 방법이 선호 됩니다.

### <a name="step-31-creating-the-bundle"></a>3.1 단계: 번들 만들기

#### <a name="using-the-bundle-generator-tool"></a>번들 생성기 도구 사용

번들 생성기 도구를 사용 하려면 패키지에 대해 만든 PRI 구성 파일을 수동으로 업데이트 하 여 섹션을 제거 해야 `<packaging>` 합니다.

Visual Studio를 사용 하는 경우, 및 파일을 만들어 기본 패키지에 모든 언어를 빌드하는 방법에 대 한 정보를 제공 [해야 하는지 여부에 관계 없이 장치에 리소스가 설치](/previous-versions/dn482043(v=vs.140)) 되어 있는지 확인 `priconfig.packaging.xml` 합니다 `priconfig.default.xml` .

수동으로 파일을 편집 하는 경우 다음 단계를 수행 합니다. 

1. 올바른 경로, 파일 이름 및 언어를 대체 하 여 이전과 동일한 방식으로 구성 파일을 만듭니다.

    ```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US_de-DE_es-MX /pv 10.0 /o
    ```

2. 만든 파일을 수동으로 열고 `.xml` 전체 섹션을 삭제 합니다 `&lt;packaging&rt;` (다른 모든 항목은 그대로 유지).

    ```xml
    <?xml version="1.0" encoding="UTF-8" standalone="yes" ?> 
    <resources targetOsVersion="10.0.0" majorVersion="1">
      <!-- Packaging section has been deleted... -->
      <index root="\" startIndexAt="\">
        <default>
        ...
        ...
    ```

3. 업데이트 된 `.pri` `.appx` 구성 파일 및 적절 한 디렉터리와 파일 이름을 사용 하 여 이전과 같이 파일 및 패키지를 빌드합니다. 이러한 명령에 대 한 자세한 내용은 위 항목을 참조 하십시오.

    ```CMD
    makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
    makeappx pack /m AppXManifest.xml /f ..\resources.map.txt /p ..\contoso_demo.appx /o
    ```

4. 패키지를 만든 후에는 다음 명령을 사용 하 여 적절 한 디렉터리와 파일 이름을 사용 하 여 번들을 만듭니다.

    ```CMD
    BundleGenerator.exe -Package ..\contoso_demo.appx -Destination ..\bundle -BundleName contoso_demo
    ```

이제 마지막 단계인 서명 (아래 참조)으로 이동할 수 있습니다.

#### <a name="manually-creating-resource-packages"></a>수동으로 리소스 패키지 만들기

리소스 패키지를 수동으로 만들려면 별도의 및 파일을 작성 하기 위해 약간 다른 명령 집합을 실행 해야 합니다 `.pri` `.appx` .이는 모두 fat 패키지를 만들기 위해 위에서 사용한 명령과 비슷하며 최소한의 설명이 제공 됩니다. 참고: 모든 명령은 현재 디렉터리가 파일을 포함 하는 디렉터리 `AppXManifest.xml` 이지만 모든 파일이 부모 디렉터리에 배치 된다고 가정 합니다. 필요한 경우 다른 디렉터리를 사용할 수 있지만 프로젝트 디렉터리를 이러한 파일에 pollute는 안 됩니다. 항상으로 "Contoso" 파일 이름을 사용자 고유의 파일 이름으로 바꿉니다.

1. 다음 명령을 사용 하 여 기본 언어의 이름을 기본 한정자 (이 경우)로 **만** 구성 하는 구성 파일을 만듭니다 `en-US` .

    ```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US /pv 10.0 /o
    ```

2. `.pri` `.map.txt` 다음 명령을 사용 하 여 주 패키지에 대 한 기본 및 파일 및 프로젝트에서 발견 된 각 언어에 대 한 추가 파일 집합을 만듭니다.

    ```CMD
    makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
    ```

3. 다음 명령을 사용 하 여 실행 코드 및 기본 언어 리소스를 포함 하는 주 패키지를 만듭니다. 항상 적절 하 게 표시 되는 대로 이름을 변경 합니다. 단, 나중에 번들을 보다 쉽게 만들 수 있도록 패키지를 별도의 디렉터리에 배치 해야 합니다 (이 예제에서는 `..\bundle` 디렉터리 사용).

    ```CMD
    makeappx pack /m .\AppXManifest.xml /f ..\resources.map.txt /p ..\bundle\contoso_demo.main.appx /o
    ```

4. 주 패키지를 만든 후에는 각 추가 언어에 대해 다음 명령을 한 번 사용 합니다 (즉, 이전 단계에서 생성 된 각 언어 맵 파일에 대해이 명령을 반복). 이 경우에도 출력은 별도의 디렉터리 (주 패키지와 동일한 것)에 있어야 합니다. 이 언어는 옵션과 옵션에서 **모두** 지정 되며 `/f` `/p` , 새 `/r` 인수 (리소스 패키지를 사용 하는 것을 나타냄)를 사용 합니다.

    ```CMD
    makeappx pack /r /m .\AppXManifest.xml /f ..\resources.language-de.map.txt /p ..\bundle\contoso_demo.de.appx /o
    ```

5. 번들 디렉터리의 모든 패키지를 단일 `.appxbundle` 파일로 결합 합니다. 새 `/d` 옵션은 번들의 모든 파일에 사용할 디렉터리를 지정 합니다 .이는 `.appx` 이전 단계에서 파일이 별도의 디렉터리에 배치 되는 이유입니다.

    ```CMD
    makeappx bundle /d ..\bundle /p ..\contoso_demo.appxbundle /o
    ```

패키지 작성을 위한 마지막 단계는 서명입니다.

### <a name="step-32-signing-the-bundle"></a>3.2 단계: 번들 서명

`.appxbundle`번들 생성기 도구를 통해 또는 수동으로 파일을 만든 후에는 주 패키지와 모든 리소스 패키지를 포함 하는 단일 파일을 갖게 됩니다. 마지막 단계는 Windows에서 파일을 설치 하도록 파일에 서명 하는 것입니다.

```CMD
signtool sign /fd SHA256 /a /f ..\contoso_demo_key.pfx ..\contoso_demo.appxbundle
```

그러면 `.appxbundle` 주 패키지와 모든 언어 관련 리소스 패키지를 포함 하는 서명 된 파일이 생성 됩니다. 사용자의 Windows 언어 기본 설정에 따라 앱과 적절 한 언어를 설치 하는 패키지 파일과 마찬가지로 두 번 클릭할 수 있습니다.

## <a name="related-topics"></a>관련 항목

* [언어, 배율, 고대비 및 기타 한정자에 맞게 리소스 조정](tailor-resources-lang-scale-contrast.md)