---
author: stevewhims
Description: This topic explains the general concept of qualifiers, how to use them, and the purpose of each of the qualifier names.
title: 언어, 규모, 고대비 및 기타 한정자에 맞게 리소스 조정
template: detail.hbs
ms.author: stwhi
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, uwp, 리소스, 이미지, 자산, MRT, 한정자
ms.localizationpriority: medium
ms.openlocfilehash: 018740b9ceaa10425ec71f6a2775d547b7c30e82
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6660356"
---
# <a name="tailor-your-resources-for-language-scale-high-contrast-and-other-qualifiers"></a>언어, 배율, 고대비 및 기타 한정자에 맞게 리소스 조정

이 주제에서는 리소스 한정자의 일반적인 개념, 사용 방법 및 각 한정자 이름의 목적에 대해 설명합니다. 가능한 모든 한정자 값에 대한 참조 테이블은 [**ResourceContext.QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)를 참조하세요.

앱은 표시 언어, 고대비, [디스플레이 배율 인수](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md#effective-pixels-and-scale-factor) 등의 런타임 컨텍스트에 맞게 조정된 자산 및 리소스를 로드할 수 있습니다. 이를 수행하는 방법은 해당 컨텍스트에 해당하는 한정자 이름 및 한정자 값과 일치하도록 리소스의 폴더 또는 파일의 이름을 지정하는 것입니다. 예를 들어, 앱이 고대비 모드에서 다른 이미지 자산 집합을 로드하도록 할 수 있습니다.

앱 지역화의 가치 제안에 대한 자세한 내용은 [세계화 및 지역화](../design/globalizing/globalizing-portal.md)를 참조하세요.

## <a name="qualifier-name-qualifier-value-and-qualifier"></a>한정자 이름, 한정자 값 및 한정자

한정자 이름은 한정자 값 집합에 매핑되는 키입니다. 다음은 대비에 대한 한정자 이름과 한정자 값입니다.

| 컨텍스트 | 한정자 이름 | 한정자 값 |
| :--------------- | :--------------- | :--------------- |
| 고대비 설정 | 대비 | 표준, 높음, 검은색, 흰색 |

한정자 이름을 한정자 값과 결합하여 한정자를 구성합니다. `<qualifier name>-<qualifier value>` 한정자의 형식입니다. `contrast-standard` 한정자의 예입니다.

따라서 고대비의 경우 한정자 집합은 `contrast-standard`, `contrast-high`, `contrast-black` 및 `contrast-white`입니다. 한정자 이름과 한정자 값은 대소문자를 구분하지 않습니다. 예를 들어, `contrast-standard` 및 `Contrast-Standard`는 동일한 한정자입니다.

## <a name="use-qualifiers-in-folder-names"></a>폴더 이름에 한정자 사용

다음은 한정자를 사용하여 자산 파일이 포함된 폴더의 이름을 지정하는 예입니다. 한정자당 여러 개의 자산 파일이 있는 경우 폴더 이름에 한정자를 사용하세요. 이렇게 하면 한정자를 폴더 수준에서 한 번 설정하고 한정자는 폴더 안의 모든 항목에 적용됩니다.

```console
\Assets\Images\contrast-standard\<logo.png, and other image files>
\Assets\Images\contrast-high\<logo.png, and other image files>
\Assets\Images\contrast-black\<logo.png, and other image files>
\Assets\Images\contrast-white\<logo.png, and other image files>
```

위의 예와 같이 폴더의 이름을 지정하면 앱에서 고대비 설정을 사용하여 적절한 한정자로 지정된 폴더에서 리소스 파일을 로드합니다. 따라서 고대비 검정으로 설정되어 있으면 `\Assets\Images\contrast-black` 폴더의 리소스 파일이 로드됩니다. 없음(컴퓨터가 고대비 모드가 아님)으로 설정되어 있으면 `\Assets\Images\contrast-standard` 폴더의 리소스 파일이 로드됩니다.

## <a name="use-qualifiers-in-file-names"></a>파일 이름에 한정자 사용

폴더를 만들고 이름을 지정하는 대신 한정자를 사용하여 리소스 파일 자체의 이름을 지정할 수 있습니다. 한정자당 하나의 리소스 파일만 있는 경우 이 작업을 수행하는 것이 좋습니다. 예를 들면 다음과 같습니다.

```console
\Assets\Images\logo.contrast-standard.png
\Assets\Images\logo.contrast-high.png
\Assets\Images\logo.contrast-black.png
\Assets\Images\logo.contrast-white.png
```

이름이 설정에 대해 가장 적합한 한정자를 포함하고 있는 파일이 로드됩니다. 이 일치 논리는 파일 이름에 대해 폴더 이름과 동일한 방식으로 작동합니다.

## <a name="reference-a-string-or-image-resource-by-name"></a>이름 기준으로 문자열 또는 이미지 리소스 참조

[XAML 태그에서 문자열 리소스 식별자 참조](localize-strings-ui-manifest.md#refer-to-a-string-resource-identifier-from-xaml-markup), [코드에서 문자열 리소스 식별자 참조](localize-strings-ui-manifest.md#refer-to-a-string-resource-identifier-from-code) 및 [XAML 태그와 코드에서 이미지 또는 다른 자산 참조](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code)를 참조하세요.

## <a name="actual-and-neutral-qualifier-matches"></a>실제 및 중립적 한정자 일치
*모든* 한정자 값에 대한 리소스 파일을 제공할 필요가 없습니다. 예를 들어 고대비 및 표준 대비에 대해 하나의 시각적 자산만 필요하다면 이러한 자산의 이름을 다음과 같이 지정할 수 있습니다.

```console
\Assets\Images\logo.contrast-high.png
\Assets\Images\logo.png
```

첫 번째 파일 이름에는 `contrast-high` 한정자가 포함됩니다. 이 한정자는 고대비가 *켜져* 있을 때 고대비 설정에 대한 *실제* 일치입니다. 즉, 거의 일치하기 때문에 선호됩니다. *실제* 일치는 한정자에 *실제* 값이 포함된 경우에만 발생할 수 있습니다. 이 경우 `high`는 `contrast`에 대한 *실제* 값입니다.

`logo.png`라는 파일에는 대비 한정자가 전혀 없습니다. 한정자가 없으면 *중립* 값입니다. 선호하는 일치 항목을 찾을 수 없는 경우 중립 값은 대체 일치 항목의 역할을 합니다. 이 예에서 고대비가 *꺼져* 있으면 실제 일치가 없습니다. *중립* 값이 가장 적합한 일치이므로 자산 `logo.png`가 로드됩니다.

`logo.png`의 이름을 `logo.contrast-standard.png`로 변경하면 파일 이름에 실제 한정자 값이 포함됩니다. 고대비가 꺼져 있으면 `logo.contrast-standard.png`와 실제로 일치하게 되며, 이것이 바로 로드될 자산 파일입니다. 따라서 서로 다른 일치 항목으로 인해, 동일한 조건에서 동일한 파일이 로드됩니다.

고대비와 표준 대비에 대해 각각 하나의 자산 집합만 필요하면 파일 이름 대신 폴더 이름을 사용할 수 있습니다. 이 경우 폴더 이름을 완전히 생략하면 중립적인 일치를 얻을 수 있습니다.

```console
\Assets\Images\contrast-high\<logo.png, and other images to load when high contrast theme is not None>
\Assets\Images\<logo.png, and other images to load when high contrast theme is None>
```

한정자 일치가 작동하는 방법에 대한 자세한 내용 [리소스 관리 시스템](resource-management-system.md)을 참조하세요.

## <a name="multiple-qualifiers"></a>여러 한정자

한정자를 폴더 및 파일 이름으로 결합할 수 있습니다. 예를 들어 고대비 모드가 켜진 경우 *및* 디스플레이 배율 인수가 400인 경우 앱에 이미지 자산을 로드해야 할 수 있습니다. 이를 수행하는 한 가지 방법은 중첩된 폴더를 사용하는 것입니다.

```console
\Assets\Images\contrast-high\scale-400\<logo.png, and other image files>
```

`logo.png` 및 로드할 기타 파일의 경우, 설정은 *두* 한정자와 모두 일치해야 합니다.

또 다른 옵션은 하나의 폴더 이름에 여러 개의 한정자를 결합하는 것입니다.

```console
\Assets\Images\contrast-high_scale-400\<logo.png, and other image files>
```

폴더 이름에서 밑줄로 구분된 여러 한정자를 결합합니다. `<qualifier1>[_<qualifier2>...]` 형식입니다.

동일한 형식의 파일 이름에 여러 한정자를 결합할 수 있습니다.

```console
\Assets\Images\logo.contrast-high_scale-400.png
```

자산 생성에 사용하는 도구와 워크플로 또는 읽기 및/또는 관리가 가장 쉬운 도구에 따라 모든 한정자에 대해 하나의 명명 전략을 선택하거나 다른 한정자에 대해 하나의 명명 전략을 결합할 수 있습니다.

## <a name="alternateform"></a>AlternateForm

`alternateform` 한정자는 특정 목적을 위해 대체 형식의 자원을 제공하는 데 사용됩니다. 이는 일반적으로 일본 앱 개발자가 `msft-phonetic` 값이 예약된 후리가나 문자열을 제공하기 위해서만 사용됩니다([지역화를 준비하는 방법](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh967762)의 '정렬 가능한 일본어 문자열에 대해 후리가나 지원' 섹션 참조).

대상 시스템 또는 앱은 `alternateform` 한정자가 일치하는 값을 제공해야 합니다. 자체적인 사용자 지정 `alternateform` 한정자 값에 대해 `msft-` 접두사는 사용하지 않습니다.

## <a name="configuration"></a>구성

`configuration` 한정자 이름이 필요할 가능성은 없습니다. 이는 테스트 전용 리소스와 같이 주어진 제작 환경에 적용할 수 있는 리소스를 지정하는 데 사용할 수 있습니다.

`configuration` 한정자는 `MS_CONFIGURATION_ATTRIBUTE_VALUE` 환경 변수의 값과 가장 잘 일치하는 리소스를 로드하는 데 사용됩니다. 따라서 변수를 관련 리소스에 할당된 문자열 값으로 설정할 수 있습니다(예: `designer` 또는 `test`).

## <a name="contrast"></a>대비

`contrast` 한정자는 고대비 설정과 가장 잘 일치하는 리소스를 제공하는 데 사용됩니다.

## <a name="custom"></a>사용자 지정

앱은 `custom` 한정자에 대한 값을 설정할 수 있으며 그 값과 가장 잘 일치하는 리소스가 로드됩니다. 예를 들어 앱의 라이선스에 따라 리소스를 로드할 수 있습니다. 코드 예에 나온 바와 같이, 앱이 실행되면 라이선스를 확인하고 [SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)를 호출하여 `custom` 한정자의 값으로 사용합니다.

```csharp
public void SetLicenseLevel(BrandID brand)
{
    if (brand == BrandID.Premium)
    {
        ResourceContext.SetGlobalQualifierValue("Custom", "Premium", ResourceQualifierPersistence.LocalMachine);
    }
    else if (brand == BrandID.Standard)
    {
        ResourceContext.SetGlobalQualifierValue("Custom", " Standard", ResourceQualifierPersistence.LocalMachine);
    }
    else
    {
        ResourceContext.SetGlobalQualifierValue("Custom", "Trial", ResourceQualifierPersistence.LocalMachine);
    }
}
```

이 시나리오에서는 `custom-premium`, `custom-standard` 및 `custom-trial` 한정자를 포함하는 리소스 이름을 제공합니다.

## <a name="devicefamily"></a>DeviceFamily

`devicefamily` 한정자 이름이 필요할 가능성은 없습니다. 대신 사용할 수 있는 훨씬 편리하고 강력한 기술이 있으므로, 가능하다면 사용하지 않아야 합니다. 이러한 기술은 [앱이 실행되고 있는 플랫폼 검색](../porting/wpsl-to-uwp-input-and-sensors.md#detecting-the-platform-your-app-is-running-on) 및 [버전 적응 코드](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code)에서 설명하고 있습니다.

하지만 마지막 수단으로 devicefamily 한정자를 사용하여 XAML 보기가 포함된 폴더의 이름을 지정할 수 있습니다(XAML 보기는 UI 레이아웃과 컨트롤이 포함된 XAML 파일).

```console
\devicefamily-desktop\<MainPage.xaml, and other markup files to load when running on a desktop computer>
\devicefamily-mobile\<MainPage.xaml, and other markup files to load when running on a phone>
```

또는 파일 이름을 지정할 수 있습니다.

```console
\MainPage.devicefamily-desktop.xaml
\MainPage.devicefamily-mobile.xaml
```

두 경우 모두 `MainPage.[<qualifier>].xaml`의 각 복사본은 이름, 위치 및 콘텐츠 측면에서 프로젝트에서 변경되지 않은 공통 `MainPage.xaml.cs`를 공유합니다.

또한 devicefamily 한정자를 사용하여 리소스 파일(`.resw`) 또는 폴더의 이름을 지정할 수 있습니다. 예를 들어 앱이 모바일 장치 패밀리에서 실행될 때 UI 요소 `<TextBlock x:Uid="DeviceFriendlyName"/>`은 포함되어 있는 경우 `Resources.devicefamily-mobile.resw` 파일에 정의된 텍스트 및 전경 리소스를 포함합니다.

```xml
<data name="DeviceFriendlyName.Foreground">
    <value>Red</value>
</data>
<data name="DeviceFriendlyName.Text">
    <value>Mobile device</value>
</data>
```

리소스 파일 사용에 대한 자세한 내용은 [UI 문자열 지역화](localize-strings-ui-manifest.md)를 참조하세요.

## <a name="dxfeaturelevel"></a>DXFeatureLevel

`dxfeaturelevel` 한정자 이름이 필요할 가능성은 없습니다. 이는 Direct3D 게임 자산과 함께 사용되어 하위 수준의 리소스를 로드해서 특정 하위 수준의 하드웨어 구성과 일치하도록 설계되었습니다. 하지만 하드웨어 구성의 사용량이 현재 너무 낮으므로 이 한정자를 사용하지 않는 것이 좋습니다.

## <a name="homeregion"></a>HomeRegion

`homeregion` 한정자는 국가 또는 지역에 대한 사용자 설정에 해당합니다. 이는 사용자의 홈 위치를 나타냅니다. 값에는 유효한 [BCP 47 지역 태그](http://go.microsoft.com/fwlink/p/?linkid=227302)가 포함되어 있습니다. 즉, **ISO 1 3166 alpha-2** 두 자리 문자 지역 코드와 구성된 지역에 대한 **ISO 1 3166 숫자** 세 자리 지리적 코드 집합입니다([United Nations Statistic Division M49 지역 코드의 구성](http://go.microsoft.com/fwlink/p/?linkid=247929) 참조). '선택된 경제 및 기타 그룹화"에 대한 코드는 유효하지 않습니다.

## <a name="language"></a>언어

`language` 한정자는 표시 언어 설정에 해당합니다. 값에는 유효한 [BCP 47 언어 태그](http://go.microsoft.com/fwlink/p/?linkid=227302)가 포함되어 있습니다. 언어 목록은 [IANA 언어 하위 태그 레지스트리](http://go.microsoft.com/fwlink/p/?linkid=227303)를 참조하세요.

앱에서 다른 표시 언어를 지원하길 원하고 코드 또는 XAML 태그에 문자열 리터럴이 있는 경우, 코드/태그에서 해당 문자열을 리소스 파일(`.resw`)로 이동합니다. 그런 다음 앱에서 지원하는 언어별로 해당 리소스 파일의 번역본을 만들 수 있습니다.

일반적으로 `language` 한정자를 사용하여 리소스 파일(`.resw`)을 포함하는 폴더의 이름을 지정합니다.

```console
\Strings\language-en\Resources.resw
\Strings\language-ja\Resources.resw
```

`language` 한정자의 `language-` 부분을 생략할 수 있습니다(한정자 이름). 다른 유형의 한정자로는 이 작업을 수행할 수 없으며, 폴더 이름으로만 수행할 수 있습니다.

```console
\Strings\en\Resources.resw
\Strings\ja\Resources.resw
```

폴더 이름을 지정하는 대신 `language` 한정자를 사용하여 리소스 파일 자체의 이름을 지정할 수 있습니다.

```console
\Strings\Resources.language-en.resw
\Strings\Resources.language-ja.resw
```

문자열 리소스를 사용하여 앱을 지역화할 수 있는 방법 및 앱에서 문자열 리소스를 참조하는 방법에 대한 자세한 내용은 [UI 문자열 지역화](localize-strings-ui-manifest.md)를 참조하세요.

## <a name="layoutdirection"></a>LayoutDirection

`layoutdirection` 한정자는 표시 언어 설정의 레이아웃 방향에 해당합니다. 예를 들어 이미지를 아랍어 또는 히브리어와 같이 오른쪽에서 왼쪽으로 쓰는 언어로 미러링해야 할 수 있습니다. [FlowDirection](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) 속성을 설정하는 경우 UI의 레이아웃 패널 및 이미지가 레이아웃 방향에 적절하게 대응합니다([레이아웃 및 글꼴 조정, RTL 지원](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md) 참조). 하지만 `layoutdirection` 한정자는 단순한 플리핑이 적절하지 않은 경우를 위한 것이며, 보다 일반적인 방법으로 특정 읽기 순서 및 텍스트 정렬의 방향에 대응할 수 있습니다.

## <a name="scale"></a>배율

Windows는 장치의 가시거리와 DPI(인치당 도트 수)를 기준으로 각 디스플레이의 배율을 자동으로 선택합니다. [유효 픽셀 및 배율](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md#effective-pixels-and-scale-factor)을 참조하세요. Windows에서 최적 크기를 선택하거나 가장 유사한 크기를 사용하여 확장할 수 있도록 권장된 여러 크기(최소 100, 200, 및 400)에서 이미지를 만들어야 합니다. Windows에서 디스플레이 배율 인수에 대해 올바른 크기의 이미지를 포함하는 실제 파일을 식별할 수 있도록 `scale`한정자를 사용합니다. 리소스의 배율은 [DisplayInformation.ResolutionScale](/uwp/api/windows.graphics.display.displayinformation.ResolutionScale) 값 또는 다음으로 큰 배율의 리소스와 일치합니다.

다음은 폴더 수준에서 한정자를 설정하는 예입니다.

```console
\Assets\Images\scale-100\<logo.png, and other image files>
\Assets\Images\scale-200\<logo.png, and other image files>
\Assets\Images\scale-400\<logo.png, and other image files>
```

이 예는 파일 수준에서 설정합니다.

```console
\Assets\Images\logo.scale-100.png
\Assets\Images\logo.scale-200.png
\Assets\Images\logo.scale-400.png
```

`scale` 및 `targetsize`에 대한 리소스를 한정하는 방법에 대한 내용은 [targetsize에 대한 이미지 리소스 한정](images-tailored-for-scale-theme-contrast.md#qualify-an-image-resource-for-targetsize)을 참조하세요.

## <a name="targetsize"></a>TargetSize

`targetsize` 한정자는 주로 파일 탐색기에 표시할 [파일 유형 연결 아이콘](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh127427) 또는 [프로토콜 아이콘](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/bb266530)을 지정하는 데 사용됩니다. 한정자 값은 원시(물리적) 픽셀의 정사각형 이미지 측면 길이를 나타냅니다. 파일 탐색기의 보기 설정과 일치하는 값의 리소스 또는 정확한 일치가 없는 경우 다음으로 큰 값을 갖는 리소스가 로드됩니다.

앱 패키지 매니페스트 디자이너의 시각적 자산 탭에서 앱 아이콘(`/Assets/Square44x44Logo.png`)에 대한 `targetsize` 한정자 값의 여러 크기를 나타내는 자산을 정의할 수 있습니다.

`scale` 및 `targetsize`에 대한 리소스를 한정하는 방법에 대한 내용은 [targetsize에 대한 이미지 리소스 한정](images-tailored-for-scale-theme-contrast.md#qualify-an-image-resource-for-targetsize)을 참조하세요.

## <a name="theme"></a>테마

`theme` 한정자는 기본 앱 모드 설정 또는 [Application.RequestedTheme](/uwp/api/windows.ui.xaml.application?branch=master.RequestedTheme)를 사용하는 앱의 재정의 설정과 가장 잘 일치하는 리소스를 제공하는 데 사용됩니다.

## <a name="important-apis"></a>중요 API

* [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)
* [SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)

## <a name="related-topics"></a>관련 항목

* [유효 픽셀 및 배율](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md#effective-pixels-and-scale-factor)
* [리소스 관리 시스템](resource-management-system.md)
* [지역화를 준비하는 방법](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh967762)
* [앱이 실행되고 있는 플랫폼 검색](../porting/wpsl-to-uwp-input-and-sensors.md#detecting-the-platform-your-app-is-running-on)
* [장치 패밀리 개요](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview)
* [UI 문자열 지역화](localize-strings-ui-manifest.md)
* [BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302)
* [United Nations Statistic Division M49 지역 코드의 구성](http://go.microsoft.com/fwlink/p/?linkid=247929)
* [IANA 언어 하위 태그 레지스트리](http://go.microsoft.com/fwlink/p/?linkid=227303)
* [레이아웃 및 글꼴 조정, RTL 지원](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md)
