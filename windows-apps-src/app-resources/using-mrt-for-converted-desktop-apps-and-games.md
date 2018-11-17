---
author: ptorr-msft
title: 변환된 데스크톱 앱과 게임에 MRT 사용
description: .NET 또는 Win32 앱 또는 게임을 AppX 패키지로 패키징하면 리소스 관리 시스템을 사용하여 실행 시 컨텍스트에 맞게 조정된 앱 리소스를 로드할 수 있습니다. 이 항목에서는 그 기술에 대해 자세히 설명합니다.
ms.author: ptorr
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp, mrt, pri. 리소스, 게임, centennial, Desktop App Converter, mui, 위성 어셈블리
ms.localizationpriority: medium
ms.openlocfilehash: 927e0c5438ea11b751fba40cb76210d0bce112d4
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7158244"
---
# <a name="use-the-windows-10-resource-management-system-in-a-legacy-app-or-game"></a>레거시 앱 또는 게임에서 Windows 10 리소스 관리 시스템 사용

## <a name="overview"></a>개요

.NET 및 Win32 응용 프로그램 및 게임은 종종 다른 언어로 지역화되어 전체 대상 시장을 확장합니다. 앱 지역화의 가치 제안에 대한 자세한 내용은 [세계화 및 지역화](../design/globalizing/globalizing-portal.md)를 참조하세요. .NET 또는 Win32 앱 또는 게임을 AppX 패키지로 패키징하면 리소스 관리 시스템을 사용하여 실행 시 컨텍스트에 맞게 조정된 앱 리소스를 로드할 수 있습니다. 이 항목에서는 그 기술에 대해 자세히 설명합니다.

레거시 Win32 응용 프로그램을 지역화하는 방법은 여러 가지가 있지만 Windows 8에는 여러 프로그래밍 언어와 응용 프로그램 유형에서 사용할 수 있으며 간편한 지역화 이상의 기능을 제공하는 [새로운 리소스 관리 시스템](https://msdn.microsoft.com/en-us/library/windows/apps/jj552947.aspx)이 도입되었습니다. 이 시스템은 이 항목에서 "MRT"라고 부르겠습니다. 이는 지금까지 "최신 리소스 기술"이라고 불렀지만 "최신"이라는 용어는 이제 사용되지 않습니다. 리소스 관리자는 MRM(최신 리소스 관리자) 또는 PRI(패키지 리소스 인덱스)라고도 알려져 있습니다.

AppX 기반 배포(예: Microsoft Store)와 함께 결합되어, MRT는 자동으로 지정된 사용자/디바이스에 대해 가장 적절한 리소스를 제공하여 응용 프로그램의 다운로드 및 설치 크기를 최소화합니다. 이 크기 감소는 지역화된 콘텐츠가 포함된 응용 프로그램에 매우 큰 것으로, AAA 게임의 경우 *기가바이트*의 자릿수가 변경되는 수준입니다. MRT의 추가 장점에는 Windows 셸과 Microsoft Store의 지역화된 목록, 사용자의 원하는 언어가 사용할 수 있는 리소스가 일치하지 않을 때 자동 대체 논리 등이 포함됩니다.

이 문서는 MRT의 간략한 아키텍처를 설명하고, 기존 Win32 응용 프로그램을 최소의 코드 변경으로 MRT로 이식할 수 있는 지침을 제공합니다. MRT로 이전하면, 개발자가 추가 장점(예: 배율 인수나 시스템 테마로 리소스를 분할하는 기능)을 사용할 수 있게 됩니다. MRT 기반 지역화는 데스크톱 브리지(일명 "Centennial")에 의해 처리되는 UWP 응용 프로그램 및 Win32 응용 프로그램 모두와 사용할 수 있습니다.

대부분의 경우 기존 지역화 형식과 소스 코드를 계속 사용할 수 있으며, 런타임에 리소스를 분석하고 다운로드 크기를 최소화하기 위해 MRT와 통합할 수 있습니다. 모두 가지거나 모두 포기하는 접근 방식이 아닙니다. 다음 표는 각 단계의 작업과 예상되는 비용/장점을 요약한 것입니다. 이 표는 고해상도 또는 고대비 응용 프로그램 아이콘과 같은 지역화되지 않는 작업을 포함하지 않습니다. 타일, 아이콘이 등에 대한 여러 자산 제공에 대한 자세한 내용은 [언어, 배율, 고대비 및 기타 한정자에 맞게 리소스 조정](tailor-resources-lang-scale-contrast.md)을 참조하세요.

<table>
<tr>
<th>작업</th>
<th>장점</th>
<th>예상 비용</th>
</tr>
<tr>
<td>AppX 매니페스트 지역화</td>
<td>Windows 셸과 Microsoft Store에 지역화된 콘텐츠를 표시하는 데 필요한 작업이 최소화됨</td>
<td>적음</td>
</tr>
<tr>
<td>MRT를 사용하여 리소스를 식별하고 찾기</td>
<td>다운로드 및 설치 크기 최소화를 위한 필수 조건, 자동 언어 대체</td>
<td>보통</td>
</tr>
<tr>
<td>리소스 팩 빌드</td>
<td>다운로드 및 설치 크기를 최소화하는 최종 단계</td>
<td>적음</td>
</tr>
<tr>
<td>MRT 리소스 형식과 API로 마이그레이션</td>
<td>훨씬 더 작은 파일 크기(기존 리소스 기술에 따름)</td>
<td>큼</td>
</tr>
</table>

## <a name="introduction"></a>소개

대부분의 중요 응용 프로그램에는 응용 프로그램의 코드에서 분리된(소스 코드 자체에서 작성된 *하드 코드된 값*과 반대) *리소스*라고 알려진 사용자 인터페이스 요소가 포함되어 있습니다. 하드 코드된 값을 통한 리소스를 선호하는 이유는 개발자가 아닌 사람에 의한 손쉬운 편집 등 여러 가지가 있지만 핵심적인 하나의 이유는 응용 프로그램이 런타임에 동일 논리적 리소스의 다른 표현을 선택할 수 있는 능력에 있습니다. 예를 들어, 단추에 표시되는 텍스트(또는 아이콘에 표시되는 이미지)는 사용자가 이해하는 언어, 디스플레이 디바이스의 특성 또는 사용자가 보조 기술을 사용하는지에 따라 달라질 수 있습니다.

따라서 모든 리소스 관리 기술의 주 목적은 논리적 또는 기호 *리소스 이름*(예: `SAVE_BUTTON_LABEL`)을 가능한 *후보*(예: "Save", "Speichern" 또는 "저장")에서 런타임에 가장 적절한 실제 *값*(예: "Save")으로 번역하는 것입니다. MRT는 이러한 기능을 제공하며, 응용 프로그램이 사용자의 언어, 디스플레이 배율 인수, 사용자가 선택한 테마 및 기타 환경 요인과 같은 *한정자*로 불리는 다양한 특성을 사용하여 리소스 후보를 식별할 수 있도록 합니다. MRT는 응용 프로그램이 필요할 경우 사용자 지정 한정자도 지원합니다. 예를 들어, 사용자가 계정으로 로그인했는지 게스트 사용자인지 확인하는 내용을 응용 프로그램의 모든 부분에 추가하지 않고도 다른 그래픽 자산을 제공할 수 있습니다. MRT는 문자열 리소스와 파일 기반 리소스 모두 사용할 수 있습니다. 파일 기반 리소스는 외부 데이터(파일 자체)에 대한 참조로 구현됩니다. 

### <a name="example"></a>예

여기 두 단추(`openButton` 및 `saveButton`)의 텍스트 레이블과 로고에 사용되는 PNG 파일(`logoImage`)이 있는 간단한 응용 프로그램의 예를 보겠습니다. 텍스트 레이블은 영어와 독일어로 지역화되어 있고, 로고는 일반 데스크톱 디스플레이(100% 배율 인수) 및 고해상도 휴대폰(300% 배율 인수)에 최적화되어 있습니다. 이 다이어그램은 모델의 개략적인 개념을 보여주는 것으로, 구현을 정확히 표현하지는 않습니다.

<p><img src="images\conceptual-resource-model.png"/></p>

이 그래픽에서 응용 프로그램 코드는 세 개의 논리 리소스 이름을 참조합니다. 런타임에서 `GetResource` 의사 함수는 MRT를 사용하여 리소스 테이블(PRI 파일)에서 리소스 이름을 조회하고 사용자의 언어와 디스플레이의 배율 인수와 같은 주변 조건에 따라 가장 적합한 후보를 찾습니다. 레이블의 경우 문자열이 직접 사용됩니다. 로고 이미지의 경우 문자열이 파일 이름으로 해석되고 파일을 디스크에서 읽습니다. 

사용자의 언어가 영어나 독일어가 아니거나 디스플레이 배율 인수가 100%나 300%가 아닌 경우, MRT는 대체 규칙의 집합을 기반으로 "가장 근접한" 일치 후보([MSDN의 **리소스 관리 시스템** 항목 참조](https://msdn.microsoft.com/en-us/library/windows/apps/jj552947.aspx)를 선택합니다. 

참고로 MRT는 하나 이상의 한정자에 맞춤화된 리소스를 지원합니다. 예를 들어 로고 이미지에 지역화될 텍스트가 포함되어 있는 경우, 로고는 영어/배율-100, 독일어/배율-100, 영어/배율-300, 독일어/배율-300의 네 후보를 가집니다.

### <a name="sections-in-this-document"></a>이 문서의 섹션

다음 섹션에서는 MRT를 응용 프로그램에 통합하는 데 필요한 대략적인 작업을 설명합니다.

**0단계: 응용 프로그램 패키지 제작**

이 섹션에서는 기존 응용 프로그램 빌드를 응용 프로그램을 패키지로 가져오는 방법을 설명합니다. 이 단계에서 MRT 기능은 사용되지 않습니다.

**1단계: 응용 프로그램 매니페스트 지역화**

이 섹션에서는 응용 프로그램의 매니페스트를 지역화하여 Windows 셸에서 올바르게 표시되도록 하고, 기존 리소스 형식과 API를 그대로 사용하여 리소스를 패키징하고 찾는 방법을 알아봅니다. 

**2단계: MRT를 사용하여 리소스를 식별하고 찾기**

이 섹션에서는 응용 프로그램 코드(리소스 레이아웃도 가능)를 수정하여 MRT를 사용해 리소스를 찾고, 기존 리소스 형식와 API를 그대로 사용하여 리소스를 로드하고 소비하는 방법을 설명합니다. 

**3단계: 리소스 팩 빌드**

이 섹션에서는 리소스를 별도의 *리소스 팩*으로 분리하여, 앱의 다운로드 및 설치 크기를 최소화하는 데 필요한 최종 변경 사항을 설명합니다.

### <a name="not-covered-in-this-document"></a>이 문서에서 다루지 않는 내용

위의 0~3단계를 완료한 후에는 Microsoft Store에 제출할 수 있는 응용 프로그램 "번들"을 가지게 되며, 사용자에게 필요하지 않은 리소스(예: 사용자가 사용하지 않는 언어)를 빼 다운로드 및 설치 크기를 최소화할 것입니다. 최종 단계를 수행하여 응용 프로그램 크기와 기능을 더 향상시킬 수 있습니다. 

**4단계: MRT 리소스 형식과 API로 마이그레이션**

이 단계는 이 문서의 범위를 벗어납니다. 이 단계에서는 MUI DLL 또는 .NET 리소스 어셈블리와 같은 기존 형식에서 리소스(특히 문자열)를 PRI 파일로 전환합니다. 이를 통해 다운로드와 설치 크기를 위한 추가 공간을 절약할 수 있습니다. 또한 이를 통해 다른 MRT 기능도 사용할 수 있습니다. 여기에는 배율 인수, 접근성 설정 등에 따라 이미지 파일의 다운로드 및 설치 최소화 등이 포함됩니다.

- - -

## <a name="phase-0-build-an-application-package"></a>0단계: 응용 프로그램 패키지 제작

응용 프로그램의 리소스를 변경하기 전에, 현재 패키징과 설치 기술을 표준 UWP 패키징 및 배포 기술로 바꿔야 합니다. 이 작업을 수행 하는 방법은 세 가지가 있습니다.

0. 복잡한 설치 관리자가 있는 큰 데스크톱 응용 프로그램이 있거나 많은 OS 확장 지점을 활용하는 경우, Desktop App Converter 도구를 사용하여 기존 앱 설치 관리자(예: MSI)에서 UWP 파일 레이아웃 및 매니페스트 정보를 생성할 수 있습니다.
0. 상대적으로 파일이 적은 데스크톱 응용 프로그램이 있거나 간단한 설치 관리자와 확장 연결이 없다면, 수동으로 파일 레이아웃과 매니페스트 정보를 만들 수 있습니다.
0. 소스를 다시 빌드하여 "순수한" UWP 응용 프로그램으로 앱을 업데이트하려면, Visual Studio에서 새 프로젝트를 만들고 IDE를 사용하여 대부분의 작업을 수행할 수 있습니다.

[Desktop App Converter](https://aka.ms/converter)를 사용하려면 [MSDN의 **데스크톱-UWP 브리지: Desktop App Converter** 항목](https://aka.ms/converterdocs)에서 변환 프로세서의 자세한 내용을 참조하세요. 완전한 데스크톱 변환기 샘플 집합은 [**데스크톱-UWP 브리지 샘플** GitHub 리포지토리](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)에서 찾을 수 있습니다.

패키지를 수동으로 생성하려면, 응용 프로그램의 모든 파일(소스 코드를 제외한 실행 파일과 콘텐츠)과 `AppXManifest.xml` 파일을 포함한 디렉터리 구조를 만들어야 합니다. 예는 [**Hello, World** GitHub 샘플](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/blob/master/Samples/HelloWorldSample/CentennialPackage/AppxManifest.xml)에서 찾을 수 있으며 `ContosoDemo.exe`라는 데스크톱 실행 파일을 실행하는 기본 `AppXManifest.xml` 파일은 다음과 같습니다. 여기서 <span style="background-color: yellow">강조 표시된 텍스트</span>는 사용자가 원하는 값으로 바꾸게 됩니다.

<blockquote>
<pre>
&lt;?xml version="1.0" encoding="utf-8" ?&gt;
&lt;Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
         xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
         xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
         IgnorableNamespaces="uap mp rescap"&gt;
    &lt;Identity Name="<span style="background-color: yellow">Contoso.Demo</span>"
              Publisher="<span style="background-color: yellow">CN=Contoso.Demo</span>"
              Version="<span style="background-color: yellow">1.0.0.0</span>" /&gt;
    &lt;Properties&gt;
    &lt;DisplayName&gt;<span style="background-color: yellow">Contoso App</span>&lt;/DisplayName&gt;
    &lt;PublisherDisplayName&gt;<span style="background-color: yellow">Contoso, Inc</span>&lt;/PublisherDisplayName&gt;
    &lt;Logo&gt;Assets\StoreLogo.png&lt;/Logo&gt;
  &lt;/Properties&gt;
    &lt;Dependencies&gt;
    &lt;TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.0" 
                        MaxVersionTested="10.0.14393.0" /&gt;
  &lt;/Dependencies&gt;
    &lt;Resources&gt;
    &lt;Resource Language="<span style="background-color: yellow">en-US</span>" /&gt;
  &lt;/Resources&gt;
    &lt;Applications&gt;
    &lt;Application Id="<span style="background-color: yellow">ContosoDemo</span>" Executable="<span style="background-color: yellow">ContosoDemo.exe</span>" 
                 EntryPoint="Windows.FullTrustApplication"&gt;
    &lt;uap:VisualElements DisplayName="<span style="background-color: yellow">Contoso Demo</span>" BackgroundColor="#777777" 
                        Square150x150Logo="Assets\Square150x150Logo.png" 
                        Square44x44Logo="Assets\Square44x44Logo.png" 
        Description="<span style="background-color: yellow">Contoso Demo</span>"&gt;
      &lt;/uap:VisualElements&gt;
    &lt;/Application&gt;
  &lt;/Applications&gt;
    &lt;Capabilities&gt;
    &lt;rescap:Capability Name="runFullTrust" /&gt;
  &lt;/Capabilities&gt;
&lt;/Package&gt;
</pre>
</blockquote>

`AppXManifest.xml` 파일과 패키지 레이아웃에 대한 자세한 내용은 [MSDN의 **앱 패키지 매니페스트** 항복](https://msdn.microsoft.com/en-us/library/windows/apps/br211474.aspx)을 참조하세요.

마지막으로, Visual Studio로 새 프로젝트를 만들어 기존 코드를 마이그레이션하는 경우, [새 UWP 프로젝트 빌드를 위한 MSDN 문서](https://msdn.microsoft.com/en-us/windows/uwp/get-started/create-a-hello-world-app-xaml-universal)를 참조하세요. 기존 코드를 새 프로젝트에 포함시킬 수 있지만, "순수한" UWP로 실행하려면 상당한 코드 변경(특히 사용자 인터페이스)이 필요할 것입니다. 이러한 변경 내용은 이 문서의 범위를 벗어납니다.

***

## <a name="phase-1-localize-the-application-manifest"></a>1단계: 응용 프로그램 매니페스트 지역화

### <a name="step-11-update-strings--assets-in-the-appxmanifest"></a>1.1단계: AppXManifest에서 문자열 및 자산 업데이트

0단계에서는 응용 프로그램을 위한 기본 `AppXManifest.xml` 파일(변환기에 제공된 값, MSI에서 추출한 값 또는 매니페스트에 수동으로 입력한 값)을 만들지만, 여기에는 지역화된 정보는 없으며 고해상도 시작 타일 자산 등과 같은 추가 기능은 지원하지 않습니다. 

응용 프로그램의 이름과 설명이 제대로 지역화되려면 리소스 파일 세트에 몇 가지 리소스를 정의하고, 이를 참조하도록 AppX 매니페스트 업데이트해야 합니다.

**기본 리소스 파일 만들기**

첫 번째 단계 기본 언어(예: 미국 영어)로 기본 리소스를 만드는 것입니다. 이는 텍스트 편집기를 사용하여 수동으로 편집하거나, Visual Studio에서 리소스 디자이너를 통해 편집할 수 있습니다.

수동으로 메시지를 만들 경우:

0. `resources.resw`라는 이름의 XML 파일을 만들고 프로젝트의 `Strings\en-us` 하위 폴더에 배치합니다. 
 * 기본 언어가 미국 영어가 아닌 경우 해당 BCP-47 코드를 사용합니다. 
0. XML 파일에 다음과 같은 내용을 추가합니다. <span style="background-color: yellow">강조 표시된 텍스트</span>는 기본 언어로 앱에 적절한 텍스트로 교체합니다.

**참고** 이러한 문자열의 일부는 길이에 제한이 있습니다. 자세한 내용은 [VisualElements](/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements?branch=live)를 참조하세요.

<blockquote>
<pre>
&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;root&gt;
  &lt;data name="ApplicationDescription"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Demo app with localized resources (English)</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="ApplicationDisplayName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Demo Sample (English)</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="PackageDisplayName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Demo Package (English)</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="PublisherDisplayName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Samples, USA</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="TileShortName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso (EN)</span>&lt;/value&gt;
  &lt;/data&gt;
&lt;/root&gt;
</pre>
</blockquote>

Visual Studio에서 디자이너를 사용하려면:

0. `Strings\en-us` 폴더(또는 다른 언어)를 프로젝트에 만들고, 프로젝트의 루트 폴더에 **새 항목**을 추가합니다. 이때 다음 파일의 기본 이름을 사용합니다. `resources.resw`
 * **리소스 사전**이 아니라 **리소스 파일(.resw)** 을 선택합니다. 리소스 사전은 XAML 응용 프로그램에서 사용하는 파일입니다.
0. 디자이너를 사용하여 다음 문자열을 입력합니다. 같은 `Names`를 사용하지만 `Values`는 응용 프로그램에 대한 적절한 텍스트로 교체합니다.

<img src="images\editing-resources-resw.png"/>

참고: Visual Studio 디자이너를 시작하면 언제든 `F7`을 눌러 XML을 직접 편집할 수 있습니다. 하지만 최소의 XML 파일로 시작할 경우, 많은 추가 메타데이터가 누락되어 있으므로 *디자이너는 파일을 인식하지 않습니다.* 이를 해결하려면 디자이너가 생성한 파일에서 상용구 XML 정보를 직접 편집한 XML 파일에 복사하면 됩니다. 

**리소스를 참조하도록 매니페스트 업데이트**

`.resw` 파일에 값을 정의했다면 다음 단계는 리소스 문자열을 참조하도록 매니페스트를 업데이트하는 것입니다. 마찬가지로 XML 파일을 직접 편집하거나 Visual Studio 매니페스트 디자이너를 사용할 수 있습니다.

XML을 직접 편집할 경우, `AppxManifest.xml` 파일을 열고 <span style="background-color: lightgreen">강조 표시된 값</span>을 다음과 같이 변경합니다. 응용 프로그램에 대한 텍스트가 아니라 *정확한* 이 텍스트를 사용합니다. 이러한 정확한 리소스 이름을 사용할 필요는 없으며, 자체적으로 선택하면 됩니다. 하지만 선택한 이름은 `.resw` 파일의 이름과 정확히 일치해야 합니다. 이러한 이름은 `.resw` 파일에서 만든 `Names`와 일치해야 합니다. 접두사로 `ms-resource:` 스키마와 `Resources/` 네임스페이스가 붙습니다. 

*참고: 매니페스트의 많은 요소가 이 코드 조각에서 생략되었습니다. 아무것도 삭제하지 마십시오!*

<blockquote>
<pre>
&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;Package&gt;
  &lt;Properties&gt;
    &lt;DisplayName&gt;<span style="background-color: lightgreen">ms-resource:Resources/PackageDisplayName</span>&lt;/DisplayName&gt;
    &lt;PublisherDisplayName&gt;<span style="background-color: lightgreen">ms-resource:Resources/PublisherDisplayName</span>&lt;/PublisherDisplayName&gt;
  &lt;/Properties&gt;
  &lt;Applications&gt;
    &lt;Application&gt;
      &lt;uap:VisualElements DisplayName="<span style="background-color: lightgreen">ms-resource:Resources/ApplicationDisplayName</span>"
        Description="<span style="background-color: lightgreen">ms-resource:Resources/ApplicationDescription</span>"&gt;
        &lt;uap:DefaultTile ShortName="<span style="background-color: lightgreen">ms-resource:Resources/TileShortName</span>"&gt;
          &lt;uap:ShowNameOnTiles&gt;
            &lt;uap:ShowOn Tile="square150x150Logo" /&gt;
          &lt;/uap:ShowNameOnTiles&gt;
        &lt;/uap:DefaultTile&gt;
      &lt;/uap:VisualElements&gt;
    &lt;/Application&gt;
  &lt;/Applications&gt;
&lt;/Package&gt;
</pre>
</blockquote>

Visual Studio 매니페스트 디자이너를 사용하는 경우 `Package.appxmanifest` 파일을 열고 `Application` 탭과 `Packaging` 탭의 <span style="background-color: lightgreen">강조 표시된 값</span>을 변경합니다.

<img src="images\editing-application-info.png"/>
<img src="images\editing-packaging-info.png"/>

### <a name="step-12-build-pri-file-make-an-appx-package-and-verify-its-working"></a>1.2단계: PRI 파일 빌드, AppX 패키지 제작, 작동 확인

이제 `.pri` 파일을 빌드하여 응용 프로그램을 배포, 기본 언어로 올바른 정보가 시작 메뉴에 표시되는지 확인할 수 있습니다. 

Visual Studio에서 빌드하는 경우 `Ctrl+Shift+B`를 눌러 프로젝트를 빌드하고 프로젝트를 마우스 오른쪽 단추로 클릭한 다음 상황에 맞는 메뉴에서 `Deploy`를 선택합니다. 

수동으로 빌드하는 경우, 다음 단계를 따라 `MakePRI` 도구에 대한 구성 파일을 만들고, `.pri` 파일 자체를 생성합니다. 자세한 내용은 [MSDN의 **수동 앱 패키징** 항목](https://docs.microsoft.com/en-us/windows/uwp/packaging/manual-packaging-root)을 참조하세요.

0. 시작 메뉴의 `Visual Studio 2015` 폴더에서 개발자 명령 프롬프트를 엽니다.
0. 프로젝트 루트 디렉터리로 전환합니다. `AppxManifest.xml` 파일과 `Strings` 폴더를 포함한 디렉터리입니다.
0. 다음 명령을 입력합니다. "contoso_demo.xml"은 프로젝트에 적절한 이름으로, "en-US"를 앱의 기본 언어로 변경(또는 해당하는 경우 en-US 그대로 유지)합니다. xml 파일은 응용 프로그램의 일부가 아니기 때문에 프로젝트 디렉터리가 **아닌** 부모 디렉터리에 생성됩니다. 원하는 다른 디렉터리를 선택할 수 있지만, 이후 명령에서 이 디렉터리를 사용해야 합니다.

```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US /pv 10.0 /o
```

0. `makepri createconfig /?`를 입력하면 각 매개 변수가 무슨 용도인지 볼 수 있지만, 요약하면 다음과 같습니다.
 * `/cf` 구성 파일 이름(이 명령의 출력)을 설정
 * `/dq` 기본 한정자(이 경우는 언어)를 설정 `en-US`
 * `/pv` 플랫폼 버전(이 경우는 Windows 10)을 설정
 * `/o` 출력 파일이 이미 있을 경우 덮어쓰도록 설정
0. 이제 구성 파일이 있으므로 `MakePRI`를 다시 실행하여 리소스를 실제로 검색하고, PRI 파일로 패키징합니다. "contoso_demop.xml"을 이전 단계에서 사용한 XML 파일 이름으로 바꾸고, 입력과 출력 모두에 대해 부모 디렉터리를 지정합니다. 

    `makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o`
0. `makepri new /?`를 입력하면 각 매개 변수가 무슨 용도인지 볼 수 있지만, 요약하면 다음과 같습니다.
 * `/pr` 프로젝트 루트(이 경우 현재 디렉터리) 설정
 * `/cf` 이전 단계에서 만든 구성 파일 이름 설정
 * `/of` 출력 파일 설정 
 * `/mf` 매핑 파일 생성(나중 단계에서 패키지에 파일을 제외할 수 있도록)
 * `/o` 출력 파일이 이미 있을 경우 덮어쓰도록 설정
0. 이제 기본 언어 리소스(예: en-US)가 있는 `.pri` 파일이 준비되었습니다. 제대로 작동하는지 확인하려면 다음 명령을 실행합니다.

    `makepri dump /if ..\resources.pri /of ..\resources /o`
0. `makepri dump /?`를 입력하면 각 매개 변수가 무슨 용도인지 볼 수 있지만, 요약하면 다음과 같습니다.
 * `/if` 입력 파일 이름 설정 
 * `/of` 출력 파일 이름 설정(`.xml`이 자동으로 추가)
 * `/o` 출력 파일이 이미 있을 경우 덮어쓰도록 설정
0. 마지막으로, 텍스트 편집기에서 `..\resources.xml`을 열고 `<NamedResource>` 값(`ApplicationDescription`과 `PublisherDisplayName`과 같은)과 선택한 기본 언어에 대한 `<Candidate>` 값이 나열되어 있는지 확인합니다. 파일 시작 부분에 다른 내용이 있지만 여기서는 무시하면 됩니다.

원하는 경우 매핑 파일 `..\resources.map.txt`를 열어 프로젝트에 필요한 파일(프로젝트 디렉터리의 일부가 아닌 PRI 파일 포함)이 포함되어 있는지 확인합니다. 중요한 것은 매핑 파일의 콘텐츠가 PRI 파일에 포함되어 있기 때문에 매핑 파일에는 `resources.resw` 파일에 대한 참조가 포함되어 있지 *않다는* 것입니다. 그러나 이미지의 파일 이름과 같은 다른 리소스는 포함하고 있습니다.

**패키지 빌드 및 서명**

이제 PRI 파일이 빌드되었으므로 패키지를 빌드하고 서명합니다.

0. 앱 패키지를 만들려면 다음 명령을 실행합니다. `contoso_demo.appx`는 만들려는 AppX 파일의 이름으로 대체하고, `.AppX` 파일에 대해서는 다른 디렉터리를 선택합니다. 이 샘플에서는 상위 디렉터리를 사용하지만, 프로젝트 디렉터리가 **않은** 어떤 디렉터리를 사용해도 됩니다.

    `makeappx pack /m AppXManifest.xml /f ..\resources.map.txt /p ..\contoso_demo.appx /o`
0. `makeappx pack /?`를 입력하면 각 매개 변수가 무슨 용도인지 볼 수 있지만, 요약하면 다음과 같습니다.
 * `/m` 사용할 매니페스트 파일 설정
 * `/f` 사용할 매핑 파일(이전 단계에서 만든) 설정 
 * `/p` 출력 패키지 이름 설정
 * `/o` 출력 파일이 이미 있을 경우 덮어쓰도록 설정
0. 패키지를 만들었으면 서명해야 합니다. 서명 인증서를 얻는 가장 쉬운 방법은 Visual Studio에서 빈 유니버설 Windows 프로젝트를 만들고 복사 하는 것의 `.pfx` 수 있지만 파일을 사용 하 여 수동으로 만들 수는 `MakeCert` 및 `Pvk2Pfx` 유틸리티에 설명 된 대로 [는 **만드는 방법 앱 패키지 서명 인증서를** MSDN의 항목] (https://msdn.microsoft.com/en-us/library/windows/desktop/jj835832(v=vs.85).aspx). 
 * **중요:** 서명 인증서를 수동으로 만들 경우, 소스 프로젝트나 패키지 소스와 다른 디렉터리에 저장하십시오. 그렇지 않으면 개인 키를 포함하여 패키지의 일부로 포함될 수 있습니다.
0. 패키지에 서명하려면 다음 명령을 사용합니다. `AppxManifest.xml`의 `Identity` 요소에 지정된 `Publisher`는 인증서의 `Subject`와 일치해야 합니다. 이는 사용자에게 표시되는 지역화된 표시 이름인 `<PublisherDisplayName>` 요소가 **아닙니다**. 늘 그렇듯 `contoso_demo...` 파일 이름을 프로젝트에 적절한 이름으로 바꾸고(**매우 중요**) `.pfx` 파일이 현재 디렉터리에 있지 않도록 합니다. 그렇지 않으면 개인 서명 키를 포함하여 패키지의 일부로 생성됩니다.

    `signtool sign /fd SHA256 /a /f ..\contoso_demo_key.pfx ..\contoso_demo.appx`
0. `signtool sign /?`를 입력하면 각 매개 변수가 무슨 용도인지 볼 수 있지만, 요약하면 다음과 같습니다.
 * `/fd` 파일 다이제스트 알고리즘(AppX에 대한 기본값은 SHA256) 설정
 * `/a` 최적의 인증서를 자동으로 선택
 * `/f` 서명 서명 인증서가 포함된 입력 파일을 지정

마지막으로, `.appx` 파일을 두 번 클릭하여 설치합니다. 명령줄을 선호하는 경우 PowerShell 프롬프트를 열고 패키지가 포함된 디렉터리로 이동한 다음, 다음 명령(`contoso_demo.appx`를 패키지 이름으로 대체)을 입력합니다.

```CMD
    add-appxpackage contoso_demo.appx
```

인증서를 신뢰할 수 없다는 오류가 발생할 경우, 인증서가 사용자 저장소가 **아닌** 컴퓨터 저장소에 추가되어 있는지 되며 있는지 확인하십시오. 인증서에 컴퓨터 저장소에 추가하려면 명령줄 또는 Windows 탐색기를 사용합니다.

명령줄을 사용하려면:

0. 관리자 권한으로 Visual Studio 2015 명령 프롬프트를 실행합니다.
0. `.cer` 파일(소스 또는 프로젝트 디렉터리 외에 있어야 함을 기억하십시오.)이 포함된 디렉터리로 전환합니다.
0. `contoso_demo.cer`을 파일 이름으로 바꾸어 다음 명령을 입력합니다.

    `certutil -addstore TrustedPeople contoso_demo.cer`
0. `certutil -addstore /?`를 실행하면 각 매개 변수가 무슨 용도인지 볼 수 있지만, 요약하면 다음과 같습니다.
 * `-addstore` 인증서 저장소에 인증서 추가
 * `TrustedPeople` 인증서가 배치된 저장소를 표시

Windows 탐색기를 사용하려면:

0. `.pfx` 파일이 있는 폴더로 이동합니다.
0. `.pfx` 파일을 두 번 클릭하면 **인증서 가져오기 마법사**가 표시됩니다.
0. `Local Machine`을 선택하고 다음을 클릭합니다. `Next`
0. 사용자 계정 컨트롤 관리자 권한 상승에 동의하고, 표시되면 다음을 클릭합니다. `Next`
0. 개인 키에 대한 암호를 입력하고 다음을 클릭합니다. `Next`
0. 다음을 선택합니다. `Place all certificates in the following store`
0. `Browse`를 클릭하고 `Trusted People` 폴더("신뢰할 수 있는 게시자"가 **아님**)를 선택합니다.
0. `Next`를 클릭하고 다음을 클릭합니다. `Finish`

`Trusted People` 저장소에 인증서를 추가한 다음, 패키지를 다시 설치합니다.

이제 시작 메뉴의 "모든 앱" 목록에 `.resw` / `.pri` 파일의 올바른 정보로 앱이 표시될 것입니다. 빈 문자열이나 `ms-resource:...` 문자열이 표시될 경우 무언가 잘못된 것입니다. 편집한 내용이 올바른지 다시 확인하십시오. 시작 메뉴에서 앱을 마우스 오른쪽 단추로 클릭하면 타일로 고정하고 올바른 정보가 표시되는지 확인할 수 있습니다.

### <a name="step-13-add-more-supported-languages"></a>1.3단계: 지원되는 언어 더 추가

AppX 매니페스트에 변경 내용이 적용되고 초기 `resources.resw` 파일을 만들면, 언어를 더 추가하는 것은 쉽습니다.

**지역화된 추가 리소스 만들기**

먼저, 지역화된 추가 리소스 값을 만듭니다. 

`Strings` 폴더 내에 적절한 BCP-47 코드(예: `Strings\de-DE`)를 사용하여 지원하려는 각 언어에 대한 추가 폴더를 만듭니다. 이러한 각 폴더 내에서 번역된 리소스 값이 포함된 `resources.resw` 파일(XML 편집기 또는 Visual Studio 디자이너 사용)을 만듭니다. 이미 지역화된 문자열을 가지고 있다고 가정하며, `.resw` 파일에 복사하면 됩니다. 이 문서는 번역 단계를 자체를 다루지는 않습니다. 

예를 들어, `Strings\de-DE\resources.resw` 파일은 <span style="background-color: yellow">강조 표시된 텍스트</span>가 `en-US`에서 변경된 상태로 다음과 같이 표시될 것입니다.

<blockquote>
<pre>
&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;root&gt;
  &lt;data name="ApplicationDescription"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Demo app with localized resources (German)</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="ApplicationDisplayName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Demo Sample (German)</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="PackageDisplayName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Demo Package (German)</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="PublisherDisplayName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Samples, DE</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="TileShortName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso (DE)</span>&lt;/value&gt;
  &lt;/data&gt;
&lt;/root&gt;
</pre>
</blockquote>

다음 단계는 `de-DE` 및 `fr-FR` 모두에 대한 리소스를 추가했다고 가정하지만, 모든 언어에 동일한 패턴을 사용할 수 있습니다.

**지원되는 언어를 나열하도록 AppX 매니페스트 업데이트**

앱이 지원하는 언어를 나열하도록 AppX 매니페스트를 업데이트해야 합니다. Desktop App Converter에서 기본 언어를 추가하지만, 다른 언어는 명시적으로 추가해야 합니다. `AppxManifest.xml` 파일을 직접 편집할 경우, `Resources` 노드를 다음과 같이 업데이트합니다. 필요한 만큼 요소를 많이 추가하고, <span style="background-color: yellow">지원하는 해당 언어</span>로 대체하고, 목록의 첫 항목이 기본(대체) 언어가 되어야 합니다. 이 예에서 기본 언어는 영어(미국)이며, 독일어(독일)와 프랑스어(프랑스) 모두에 대한 지원이 추가되었습니다.

<blockquote>
<pre>
&lt;Resources&gt;
  &lt;Resource Language="<span style="background-color: yellow">EN-US</span>" /&gt;
  &lt;Resource Language="<span style="background-color: yellow">DE-DE</span>" /&gt;
  &lt;Resource Language="<span style="background-color: yellow">FR-FR</span>" /&gt;
&lt;/Resources&gt;
</pre>
</blockquote>

Visual Studio를 사용하는 경우 아무것도 할 필요가 없습니다. `Package.appxmanifest`를 보면 특별한 <span style="background-color: yellow">x-generate</span> 값이 있는데, 이는 빌드 프로세스 중 프로젝트에서 찾은 언어를 BCP-47 코드를 따른 폴더 이름을 기반으로 삽입하도록 하는 것입니다. 이 값은 실제 Appx 매니페스트를 위한 유효한 값이 아니며, Visual Studio 프로젝트에서만 사용되는 값임을 유의하십시오.

<blockquote>
<pre>
&lt;Resources&gt;
  &lt;Resource Language="<span style="background-color: yellow">x-generate</span>" /&gt;
&lt;/Resources&gt;
</pre>
</blockquote>

**지역화된 값을 사용하여 다시 빌드**

이제 응용 프로그램을 다시 빌드하고 배포할 수 있습니다. Windows에서 언어 기본 설정을 변경하려는 경우 시작 메뉴에 표시되는 새로 지역화된 값을 확인해야 합니다. 언어를 변경하는 방법에 대한 지침은 다음을 참조하십시오.

Visual Studio의 경우 `Ctrl+Shift+B`를 사용하여 빌드하고, 프로젝트를 마우스 오른쪽 단추로 클릭하여 `Deploy`를 선택합니다.

수동으로 프로젝트를 빌드하는 경우 위의 단계를 그대로 수행하지만, 구성 파일을 만들 때 기본 한정자 목록(`/dq`)에 밑줄로 구분된 추가 언어를 추가합니다. 예를 들어, 이전 단계에서 추가한 영어, 독일어, 프랑스어 리소스를 지원하려면 다음과 같이 합니다.

```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US_de-DE_fr-FR /pv 10.0 /o
```

이렇게 하면 지정된 언어 모두가 포함된 PRI 파일이 생성되어 쉽게 테스팅에 사용할 수 있습니다. 리소스의 전체 크기가 작거나 약간의 언어만 지원할 경우는 배송 앱에 적절할 수 있습니다. 별도의 언어 팩을 만드는 추가 작업을 할 필요가 있는 리소스를 위해 설치/다운로드 크기를 최소화하는 장점을 활용할 경우만 이렇게 합니다.

**지역화 값으로 테스트**

새롭게 지역화된 변경 내용을 테스트하려면, Windows에 새로운 기본 UI 언어를 추가 합니다. 언어 팩을 다운로드하거나, 시스템을 다시 부팅 하거나, 전체 Windows UI를 다른 언어로 표시할 필요가 없습니다. 

0. 앱 `Settings`(`Windows + I`)를 실행
0. 이동: `Time & language`
0. 이동: `Region & language`
0. 클릭: `Add a language`
0. 원하는 언어를 입력(또는 선택)(예: `Deutsch`또는 `German`)
 * 하위 언어가 있을 경우, 선택(예: `Deutsch / Deutschland`)
0. 언어 목록에서 새 언어 선택
0. 클릭: `Set as default`

이제 시작 메뉴를 열고 응용 프로그램을 검색하면, 선택한 언어에 대한 지역화된 값(다른 앱도 지역화되어 표시됨)이 표시됩니다. 지역화된 이름이 바로 표시되지 않으면, 시작 메뉴의 캐시가 새로 고쳐질 때까지 몇 분만 기다립니다. 원래 언어로 돌아가려면 언어 목록에서 기본 언어로 돌리면 됩니다. 

### <a name="step-14-localizing-more-parts-of-the-appx-manifest-optional"></a>1.4단계: AppX 매니페스트의 더 많은 부분을 지역화(선택 사항)

AppX 매니페스트의 다른 섹션도 지역화할 수 있습니다. 예를 들어, 응용 프로그램이 파일 확장명을 처리하는 경우, 매니페스트에 `windows.fileTypeAssociation` 확장명이 있어야 하며 <span style="background-color: lightgreen">녹색으로 강조 표시된 텍스트</span>(리소스를 참조)를 응용 프로그램에 대한 정보가 있는 <span style="background-color: yellow">노란색 텍스트로 강조 표시</span>된 내용으로 바꿉니다.

<blockquote>
<pre>
&lt;Extensions&gt;
  &lt;uap:Extension Category="windows.fileTypeAssociation"&gt;
    &lt;uap:FileTypeAssociation Name="default"&gt;
      &lt;uap:DisplayName&gt;<span style="background-color: lightgreen">ms-resource:Resources/FileTypeDisplayName</span>&lt;/uap:DisplayName&gt;
      &lt;uap:Logo&gt;<span style="background-color: yellow">Assets\StoreLogo.png</span>&lt;/uap:Logo&gt;
      &lt;uap:InfoTip&gt;<span style="background-color: lightgreen">ms-resource:Resources/FileTypeInfoTip</span>&lt;/uap:InfoTip&gt;
      &lt;uap:SupportedFileTypes&gt;
        &lt;uap:FileType ContentType="<span style="background-color: yellow">application/x-contoso</span>"&gt;<span style="background-color: yellow">.contoso</span>&lt;/uap:FileType&gt;
      &lt;/uap:SupportedFileTypes&gt;
    &lt;/uap:FileTypeAssociation&gt;
  &lt;/uap:Extension&gt;
&lt;/Extensions&gt;
</pre>
</blockquote>

Visual Studio 매니페스트 디자이너의 `Declarations` 탭의 <span style="background-color: lightgreen">강조 표시된 값</span>을 사용하여 이 정보를 추가할 수도 있습니다.

<p><img src="images\editing-declarations-info.png"/></p>

이제 해당 리소스 이름을 각각의 `.resw` 파일에 추가합니다. <span style="background-color: yellow">강조 표시된 텍스트</span>는 앱에 대한 적절한 텍스트(이 작업은 *지원되는 각 언어*에 대해 수행해야 함):

<blockquote>
<pre>
... existing content...

&lt;data name="FileTypeDisplayName"&gt;
  &lt;value&gt;<span style="background-color: yellow">Contoso Demo File</span>&lt;/value&gt;
&lt;/data&gt;
&lt;data name="FileTypeInfoTip"&gt;
  &lt;value&gt;<span style="background-color: yellow">Files used by Contoso Demo App</span>&lt;/value&gt;
&lt;/data&gt;
</pre>
</blockquote>

이렇게 하면 파일 탐색기와 같은 Windows 셸의 일부가 표시됩니다.

<p><img src="images\file-type-tool-tip.png"/></p>

전과 같이 패키지를 빌드하고 테스트하여 새 UI 문자열을 표시하는 새 시나리오 패키지를 테스트합니다.

- - -

## <a name="phase-2-use-mrt-to-identify-and-locate-resources"></a>2단계: MRT를 사용하여 리소스를 식별하고 찾기

이전 섹션에서 MRT를 사용하여 앱의 매니페스트를 지역화하여 Windows 셸이 앱의 이름과 기타 메타데이터를 올바르게 표시할 수 있도록 하는 방법을 설명했습니다. 이를 위해 코드 변경은 필요하지 않습니다. `.resw` 파일과 몇 가지 추가 도구만 있으면 됩니다. 이 섹션에서는 MRT를 사용하여 기존 리소스 형식에서 리소스를 찾고, 최소한의 변경으로 기존 리소스 처리 코드를 사용하는 방법을 설명합니다.

### <a name="assumptions-about-existing-file-layout--application-code"></a>기존 파일 레이아웃 및 응용 프로그램 코드에 대한 가정

Win32 데스크톱 앱을 지역화하는 많은 방법이 있기 때문에, 이 백서에서는 특정 환경을 매핑하는 데 필요한 기존 응용 프로그램의 구조를 간소화한다고 가정합니다. MRT의 요구 사항에 맞게 기존 코드 기반이나 리소스 레이아웃 일부를 변경해야 할 수 있으며, 이러한 내용은 이 문서의 범위에서 벗어납니다.

**리소스 파일 레이아웃**

이 백서는 지역화된 리소스가 모두 동일한 파일 이름(예: `contoso_demo.exe.mui`, `contoso_strings.dll` 또는 `contoso.strings.xml`)을 사용한다고 가정하지만, 이러한 리소스는 BCP-47 이름을 사용하여 모두 다른 폴더(`en-US`, `de-DE` 등)에 저장됩니다. 리소스 파일의 개수, 이름, 파일 형식, 관련된 API 등은 상관이 없습니다. 중요한 유일한 내용은 모든 *논리적* 리소스가 동일한 파일 이름(하지만 다른 *물리적* 디렉터리에 배치)을 가져야 한다는 것입니다. 

반대되는 예로, 응용 프로그램이 `english_strings.dll` 및 `french_strings.dll` 파일이 포함된 하나의 `Resources` 디렉터리 파일이 포함된 플랫 파일 구조를 사용하는 경우, MRT에 잘 매핑되지는 않습니다. 더 나은 구조는 하위 디렉터리와 `en\strings.dll` 및 `fr\strings.dll` 파일이 있는 `Resources` 디렉터리입니다. `strings.lang-en.dll`과 `strings.lang-fr.dll`과 같은 포함 한정자를 가진 같은 기본 파일을 사용하는 것도 가능하지만, 언어 코드를 가진 디렉터리를 사용하는 것이 개념적으로 더 간단하므로 이에 집중합니다.

**참고** 파일 이름 명명 규칙을 따를 수 없는 경우에도 MRT와 AppX 패키지의 장점을 활용할 수 있습니다. 조금 더 많은 작업이 필요할 뿐입니다.

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

**리소스 로드 코드**

이 백서는 코드의 일부 지점에서 지역화된 리소스를 찾고, 로드하고, 사용한다고 가정합니다. 리소스를 로드하는 데 사용되는 API, 리소스를 추출하는 데 사용되는 API는 중요하지 않습니다. 의사 코드에서 기본적으로 세 단계가 있습니다.

```
set userLanguage = GetUsersPreferredLanguage()
set resourceFile = FindResourceFileForLanguage(MY_RESOURCE_NAME, userLanguage)
set resource = LoadResource(resourceFile) 
    
// now use 'resource' however you want
```

MRT는 이 프로세스에서 첫 두 단계만 변경하면 됩니다. 가장 좋은 후보 리소스를 판단하는 것과 찾는 방법입니다. 장점을 활용하기 위해 사용할 수 있는 기능을 제공하지만, 리소스를 로드하거나 사용하는 방법을 변경할 필요는 없습니다.
 
예를 들어, 응용 프로그램에서 Win32 API `GetUserPreferredUILanguages`, CRT 함수 `sprintf`, Win32 API `CreateFile`을 사용하여 위 세 개의 의사 코드 함수를 변경하고, 수동으로 `name=value` 쌍을 찾는 텍스트 파일을 구문 분석할 수 있습니다.의 (세부 정보는 중요하지 않습니다. MRT가 이미 찾은 리소스를 처리하는 기술에 영향을 주지 않는다는 것을 설명하는 것이기 때문입니다.)

### <a name="step-21-code-changes-to-use-mrt-to-locate-files"></a>2.1단계: 파일 찾기를 위해 MRT를 사용하는 코드 변경

리소스를 찾기 위해 MRT를 사용하도록 코드를 바꾸는 것은 어렵지 않습니다. 이를 위해서는 유용한 WinRT 형식과 몇 줄의 코드를 사용해야 합니다. 사용할 주요 형식은 다음과 같습니다.

* [ResourceContext](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceContext), 현재 사용 중인 한정자 값(언어, 배율 인수 등)을 캡슐화합니다.
* [ResourceManager](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.resources.core.resourcemanager)(.NET 버전이 아닌 WinRT 버전), PRI에서 모든 리소스에 액세스할 수 있도록 합니다.
* [ResourceMap](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.resources.core.resourcemap), PRI 파일의 특정 리소스 하위 집합(이 예에서는 파일 기반 리소스 대 문자열 리소스)을 나타냅니다.
* [NamedResource](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Resources.Core.NamedResource), 논리 리소스와 가능한 모든 후보를 나타냅니다.
* [ResourceCandidate](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.resources.core.resourcecandidate), 하나의 구체적 후보 리소스를 나타냅니다. 

의사 코드에서, 지정된 파일 이름(위 샘플의 `UICommands\ui.txt`와 같은)을 해석하는 방법은 다음과 같습니다.

```
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
```

코드에서 `UICommands\en-US\ui.txt`와 같은 특정 언어 폴더가 디스크에 어떻게 존재하는지 관계없이 이를 요청하지 **않는**다는 점을 유의하십시오. 대신, 코드는 *논리적* 파일 이름인 `UICommands\ui.txt`를 요청하며 MRT를 사용하여 언어 디렉터리 중 하나에서 해당 디스크 내 파일을 찾습니다.

여기에서, 샘플 앱은 계속 `CreateFile`을 사용하여 `absoluteFileName`을 로드하고 전과 같이 `name=value` 쌍을 구문 분석합니다. 앱에서 변경이 필요한 논리는 없습니다. C# 또는 C++/CX에서 코드를 작성하는 경우, 실제 코드는 이보다 복잡할 수 있습니다(실제로 많은 매개 변수가 생략될 수 있음). 아래 **.NET 리소스 로드** 섹션을 참조하세요. C++/WRL 기반 응용 프로그램은 WinRT API를 호출하는 데 하위 수준 COM 기반 API를 사용하기 때문에 더 복잡할 수 있지만, 기본 단계는 동일합니다. 아래 **Win32 MUI 리소스 로드** 섹션을 참조하세요.

**.NET 리소스 로드**

.NET에는 리소스를 찾고 로드하는 기본 제공 메커니즘("위성 어셈블리"라고 함)이 있기 때문에, 위 코드 샘플을 굳이 바꿀 필요가 없습니다. .NET에서는 해당 디렉터리에 리소스 DLL이 있으면 자동으로 이를 찾습니다. 앱이 리소스 팩을 사용하여 AppX로 앱 패키징된 경우, 디렉터리 구조는 약간 다릅니다. 리소스 디렉터리가 기본 디렉터리의 하위 디렉터리가 아니라, 동일 계층 디렉터리에 있거나 사용자가 기본 설정에서 언어를 선택하지 안은 경우 디렉터리 자체가 없을 수 있습니다. 

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

.NET에서 위성 어셈블리를 찾는 데 WinRT API를 사용하는 방법을 설명하는 간략한 예는 다음과 같습니다. 코드가 최소 구현을 보여주기 위해 의도적으로 축소되어 있지만, 찾을 어셈블리의 이름을 제공하기 위해 `ResolveEventArgs`를 전달한 위 의사 코드와 유사함을 알 수 있을 것입니다. 이 코드의 실행 가능한 버전(상세한 주석과 오류 처리 포함)은 [GitHub의 **.NET 어셈블리 확인자** 샘플](https://aka.ms/fvgqt4)의 `PriResourceRsolver.cs` 파일에서 찾을 수 있습니다.

```C#
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

```C#
void EnableMrtResourceLookup()
{
  AppDomain.CurrentDomain.AssemblyResolve += PriResourceResolver.ResolveResourceDll;
}
```

.NET 런타임은 리소스 DLL을 찾을 수 없을 경우 `AssemblyResolve` 이벤트를 발생시키며, 이때 제공된 이벤트 처리기가 MRT를 통해 원하는 파일을 찾아 어셈블리를 반환합니다.

**참고** 다른 목적으로 앱에 `AssemblyResolve` 처리기가 이미 있는 경우, 리소스 해석 코드를 기존 코드와 통합해야 합니다.

**Win32 MUI 리소스 로딩**

Win32 MUI 리소스 로딩은 기본적으로 .NET 위성 어셈블리 로딩과 같지만 C++/CX 또는 C++/WRL 코드를 대신 사용합니다. C++/CX를 사용하면 위 C# 코드와 아주 비슷하지만, C++ 언어 확장, 컴파일러 스위치, 추가 런타임 오버헤드를 사용하므로 피하고 싶을 것입니다. 이런 경우, C++/WRL을 사용하는 것이 더 복잡한 코드를 피하고 영향을 줄이는 훨씬 좋은 방법입니다. 하지만 ATL 프로그래밍(또는 일반적인 COM)에 익숙할 경우 WRL이 친숙할 것입니다. 

다음 샘플 함수는 C++/WRL을 사용하여 특정 리소스 DLL을 로드하고 일반적인 Win32 리소스 API를 사용하여 추가 리소스를 로드하는 데 사용되는 `HINSTANCE`를 반환하는 방법을 보여줍니다. 명시적으로 .NET 런타임에서 요청한 언어로 `ResourceContext`를 초기화한 C# 샘플과 달리, 이 코드는 사용자의 현재 언어를 사용함을 유의하십시오.

```CPP
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

0. 리소스 팩을 자동으로 만들기 위해 기존의 큰 팩을 사용하여 [번들 생성기 도구](https://aka.ms/bundlegen)를 실행해 보겠습니다. 이미 큰 팩을 생성한 시스템을 구축하였고, 리소스 팩을 생성하기 위해 사후 처리를 원하는 경우 선호되는 방법입니다.
0. 개별 리소스 패키지를 직접 생성하고 번들로 빌드합니다. 빌드 시스템을 더 자세히 제어하고 패키지를 직접 빌드할 수 있는 경우 선호되는 접근 방식입니다.

### <a name="step-31-creating-the-bundle"></a>3.1단계: 번들 만들기

**번들 생성기 도구 사용**

번들 생성기 도구를 사용하기 위해서는 패키지를 위해 생성된 PRI 구성 파일에서 `<packaging>`섹션을 수동으로 제거하는 업데이트가 필요합니다.

Visual Studio를 사용하는 경우 [MSDN의 **리소스가 설치되어 있는지 확인...** 항목](https://msdn.microsoft.com/en-us/library/dn482043.aspx)을 통해 `priconfig.packaging.xml`과 `priconfig.default.xml` 파일을 만들어 모든 언어를 주 패키지로 빌드하는 방법을 참조하십시오.

파일을 수동으로 편집하는 경우 다음 단계를 따르십시오. 

0. 전과 마찬가지로 구성 파일을 만들고, 경로, 파일 이름, 언어를 바꿉니다.

    `makepri createconfig /cf ..\contoso_demo.xml /dq en-US_de-DE_es-MX /pv 10.0 /o`
0. 생성된 `.xml` 파일을 수동으로 열고 `&lt;packaging&rt;` 섹션 전체를 삭제(나머지는 그대로 유지)합니다.

<blockquote>
<pre>
&lt;?xml version="1.0" encoding="UTF-8" standalone="yes" ?&gt; 
&lt;resources targetOsVersion="10.0.0" majorVersion="1"&gt;
  &lt;!-- Packaging section has been deleted... --&gt;
  &lt;index root="\" startIndexAt="\"&gt;
    &lt;default&gt;
    ...
    ...
</pre>
</blockquote>

0. 전과 같이 업데이트된 구성 파일, 해당 디렉터리 및 파일 이름(이러한 명령에 대해서는 위 내용 참조)으로 `.pri` 파일과 `.appx` 패키지를 빌드합니다.

```CMD
makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
makeappx pack /m AppXManifest.xml /f ..\resources.map.txt /p ..\contoso_demo.appx /o
```

0. 패키지가 생성되면 다음 명령과 해당 디렉터리 및 파일 이름으로 번들을 만듭니다.

    `BundleGenerator.exe -Package ..\contoso_demo.appx -Destination ..\bundle -BundleName contoso_demo`

이제 아래의 최종 단계인 서명으로 이동할 수 있습니다.

**리소스 패키지 수동 생성**

수동으로 리소스 패키지를 만들려면 별도의 `.pri`와 `.appx` 파일을 빌드하기 위한 약간 다른 명령을 실행해야 합니다. 위에서 큰 패키지를 만들기 위해 사용한 코드와 비슷하므로 설명은 최대한 간략하게 하겠습니다. 참고: 모든 명령은 현재 디렉터리가 `AppXManifest.xml` 파일을 포함한 디렉터리라고 가정하지만, 모든 파일은 부모 디렉터리에 위치합니다. 필요하면 다른 디렉터리를 사용해도 되지만, 프로젝트 디렉터리에 불필요한 파일을 넣어서는 안 됩니다. 언제나처럼 "Contoso" 파일 이름을 원하는 파일 이름으로 바꿉니다.

0. 기본 한정자로 기본 언어(이 경우 `en-US`)를 사용할 **경우만** 다음 명령을 사용하여 구성 파일을 만듭니다.

    `makepri createconfig /cf ..\contoso_demo.xml /dq en-US /pv 10.0 /o`
0. 다음 명령을 사용하여 기본 패키지와 프로젝트에서 발견되는 각 언어에 대한 파일 세트를 위한 기본 `.pri`와 `.map.txt` 파일을 만듭니다.

    `makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o`
0. 다음 명령을 사용하여 실행 코드와 기본 언어 리소스를 포함한 주 패키지를 만듭니다. 번들을 나중에 쉽게 만들기 위해(이 예에서는 `..\bundle` 디렉터리 사용) 패키지를 별도의 디렉터리에 넣겠지만, 이름을 알맞게 변경합니다.

    `makeappx pack /m .\AppXManifest.xml /f ..\resources.map.txt /p ..\bundle\contoso_demo.main.appx /o`
0. 주 패키지가 생성되면, 다음 명령을 추가 언어당 한 번씩 사용합니다. 즉 이전 단계에서 생성한 각 언어 맵 파일에 대해 이 명령을 반복합니다. 마찬가지로 출력은 별도 디렉터리(주 패키지와 같은)에 있어야 합니다. 언어는 `/f` 옵션과 `/p` 옵션에 **모두** 지정되며, 원하는 리소스 패키지를 나타내는 새로운 `/r` 인수가 사용되었음을 유의하십시오.

    `makeappx pack /r /m .\AppXManifest.xml /f ..\resources.language-de.map.txt /p ..\bundle\contoso_demo.de.appx /o`
0. 번들 디렉터리의 모든 패키지를 하나의 `.appxbundle` 파일로 결합합니다. 새 `/d` 옵션은 번들의 모든 파일에 사용되는 디렉터리를 지정합니다. 이전 단계에서 `.appx` 파일을 다른 위치에 저장한 이유가 여기에 있습니다.

    `makeappx bundle /d ..\bundle /p ..\contoso_demo.appxbundle /o`

패키지를 빌드하는 마지막 단계는 서명입니다.

### <a name="step-32-signing-the-bundle"></a>3.2단계: 번들에 서명

`.appxbundle` 파일을 만든 후에는(번들 생성기를 사용했거나 수동으로 만들었거나) 주 패키지와 모든 리소스 패키지를 포함하는 하나의 파일을 갖게 됩니다. 마지막 단계는 파일에 서명하여 Windows에서 이를 설치하도록 하는 것입니다.

    signtool sign /fd SHA256 /a /f ..\contoso_demo_key.pfx ..\contoso_demo.appxbundle

이렇게 하면 주 패키지와 모든 언어별 리소스 패키지가 포함된 서명된 `.appxbundle` 파일이 생성됩니다. 이 파일은 패키지 파일과 같이 두 번 클릭하여 설치할 수 있으며, 사용자의 Windows 언어 설정을 기반으로 적절한 언어로 설치됩니다.

## <a name="related-topics"></a>관련 항목

* [언어, 배율, 고대비 및 기타 한정자에 맞게 리소스 조정](tailor-resources-lang-scale-contrast.md)