---
description: 이 항목에서는 한정자의 일반적인 개념, 사용 방법 및 각 한정자 이름의 용도에 대해 설명 합니다.
title: 언어, 배율, 고대비 및 기타 한정자에 맞게 리소스 조정
template: detail.hbs
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP, 리소스, 이미지, 자산, MRT, 한정자
ms.localizationpriority: medium
ms.openlocfilehash: 59f0b636384ba133e985f0704e2033c1acc5f15e
ms.sourcegitcommit: efa5f793607481dcae24cd1b886886a549e8d6e5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89412017"
---
# <a name="tailor-your-resources-for-language-scale-high-contrast-and-other-qualifiers"></a>언어, 배율, 고대비 및 기타 한정자에 맞게 리소스 조정

이 문서에서는 리소스 한정자의 일반적인 개념, 사용 방법 및 각 한정자 이름의 목적에 대해 설명합니다. 가능한 모든 한정자 값의 참조 테이블은 [**QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) 를 참조 하세요.

앱은 표시 언어, 고대비, [표시 배율 인수](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md#effective-pixels-and-scale-factor)등의 런타임 컨텍스트에 맞게 조정 된 자산 및 리소스를 로드할 수 있습니다. 이렇게 하는 방법은 해당 컨텍스트에 해당 하는 한정자 이름 및 한정자 값과 일치 하도록 리소스의 폴더 또는 파일 이름을 확인 하는 것입니다. 예를 들어 앱이 고대비 모드에서 다른 이미지 자산 집합을 로드 하는 것을 원할 수 있습니다.

앱 지역화의 가치 제안에 대한 자세한 내용은 [세계화 및 지역화](../design/globalizing/globalizing-portal.md)를 참조하세요.

## <a name="qualifier-name-qualifier-value-and-qualifier"></a>한정자 이름, 한정자 값 및 한정자

한정자 이름은 한정자 값 집합에 매핑되는 키입니다. 다음은 대비 한정자 이름 및 한정자 값입니다.

| Context | 한정자 이름 | 한정자 값 |
| :--------------- | :--------------- | :--------------- |
| 고대비 설정 | 대비 | 표준, 높음, 검정, 흰색 |

한정자 이름을 한정자 값과 결합 하 여 한정자를 형성 합니다. `<qualifier name>-<qualifier value>` 한정자의 형식입니다. `contrast-standard` 는 한정자의 한 예입니다.

따라서 고대비의 경우 한정자 집합은,, 및입니다 `contrast-standard` `contrast-high` `contrast-black` `contrast-white` . 한정자 이름 및 한정자 값은 대/소문자를 구분 하지 않습니다. 예를 들어 `contrast-standard` 및 `Contrast-Standard` 는 동일한 한정자입니다.

## <a name="use-qualifiers-in-folder-names"></a>폴더 이름에 한정자 사용

다음은 한정자를 사용 하 여 자산 파일을 포함 하는 폴더의 이름을 나타내는 예제입니다. 한정자 당 여러 자산 파일이 있는 경우 폴더 이름에 한정자를 사용 합니다. 이렇게 하면 한정자를 폴더 수준에서 한 번 설정 하 고 한정자는 폴더 내의 모든 항목에 적용 됩니다.

```console
\Assets\Images\contrast-standard\<logo.png, and other image files>
\Assets\Images\contrast-high\<logo.png, and other image files>
\Assets\Images\contrast-black\<logo.png, and other image files>
\Assets\Images\contrast-white\<logo.png, and other image files>
```

위의 예에서와 같이 폴더의 이름을 지정 하는 경우 앱은 고대비 설정을 사용 하 여 적절 한 한정자에 대 한 폴더에서 리소스 파일을 로드 합니다. 따라서 설정이 Black 고대비 경우 폴더의 리소스 파일이 `\Assets\Images\contrast-black` 로드 됩니다. 설정이 None (즉, 컴퓨터가 고대비 모드에 있지 않은 경우) 이면 폴더의 리소스 파일이 `\Assets\Images\contrast-standard` 로드 됩니다.

## <a name="use-qualifiers-in-file-names"></a>파일 이름에 한정자 사용

폴더를 만들고 이름을 지정 하는 대신 한정자를 사용 하 여 리소스 파일의 이름을 지정할 수 있습니다. 한정자 당 하나의 리소스 파일만 있는 경우이 작업을 수행 하는 것이 좋습니다. 아래 예를 살펴보세요.

```console
\Assets\Images\logo.contrast-standard.png
\Assets\Images\logo.contrast-high.png
\Assets\Images\logo.contrast-black.png
\Assets\Images\logo.contrast-white.png
```

이름에는 설정에 가장 적합 한 한정자가 포함 된 파일이 로드 됩니다. 이러한 일치 논리는 폴더 이름에 대해 파일 이름에 대해 동일한 방식으로 작동 합니다.

## <a name="reference-a-string-or-image-resource-by-name"></a>이름으로 문자열 또는 이미지 리소스 참조

[Xaml 태그에서 문자열 리소스 식별자 참조](localize-strings-ui-manifest.md#refer-to-a-string-resource-identifier-from-xaml), [코드에서 문자열 리소스 식별자](localize-strings-ui-manifest.md#refer-to-a-string-resource-identifier-from-code)참조 및 [xaml 태그 및 코드에서 이미지 또는 기타 자산 참조](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code)를 참조 하세요.

## <a name="actual-and-neutral-qualifier-matches"></a>실제 및 중립 한정자 일치
*모든* 한정자 값에 대해 리소스 파일을 제공할 필요는 없습니다. 예를 들어 고대비에 대해 하나의 시각적 자산이 필요 하 고 표준 대비를 위해 하나의 시각적 자산이 필요한 경우 이러한 자산의 이름을 다음과 같이 지정할 수 있습니다.

```console
\Assets\Images\logo.contrast-high.png
\Assets\Images\logo.png
```

첫 번째 파일 이름에는 `contrast-high` 한정자가 포함 됩니다. 이 한정자는 고대비가 *설정 된 경우*고대비 설정의 *실제* 일치 항목입니다. 즉, 근접 한 일치 항목 이므로 기본 설정입니다. *실제* 일치는 한정자가 *실제* 값을 포함 하는 경우에만 발생할 수 있습니다. 이 경우 `high` 은의 *실제* 값입니다 `contrast` .

이라는 파일에는 `logo.png` 대비 한정자가 없습니다. 한정자가 없으면 *중립* 값입니다. 선호 하는 일치 항목을 찾을 수 없는 경우 중립 값은 대체 일치 항목으로 사용 됩니다. 이 예에서는 고대비가 *해제*된 경우 실제 일치 항목이 없습니다. *중립* 일치는 찾을 수 있는 가장 일치 하는 항목으로, 자산이 로드 됩니다 `logo.png` .

의 이름을로 변경 하는 경우 `logo.png` `logo.contrast-standard.png` 파일 이름에 실제 한정자 값이 포함 됩니다. 고대비를 사용 하면와의 실제 일치 항목이 `logo.contrast-standard.png` 있으며이는 로드 되는 자산 파일입니다. 따라서 동일한 조건으로 동일한 파일을 로드 하지만 서로 다른 일치 항목으로 인해 로드 됩니다.

고대비에 대해 하나의 자산 집합만 필요 하 고 표준 대비로 설정 된 경우에는 파일 이름 대신 폴더 이름을 사용할 수 있습니다. 이 경우 폴더 이름을 생략 하면 완전히 일치 하는 항목을 갖게 됩니다.

```console
\Assets\Images\contrast-high\<logo.png, and other images to load when high contrast theme is not None>
\Assets\Images\<logo.png, and other images to load when high contrast theme is None>
```

한정자 일치가 작동 하는 방법에 대 한 자세한 내용은 [리소스 관리 시스템](resource-management-system.md)을 참조 하세요.

## <a name="multiple-qualifiers"></a>여러 한정자

폴더 및 파일 이름에 한정자를 결합할 수 있습니다. 예를 들어 고대비 모드가 켜져 *있고* 표시 배율 인수가 400 인 경우 앱에서 이미지 자산을 로드 하도록 할 수 있습니다. 이 작업을 수행 하는 한 가지 방법은 중첩 된 폴더를 사용 하는 것입니다.

```console
\Assets\Images\contrast-high\scale-400\<logo.png, and other image files>
```

`logo.png`및 로드 될 다른 파일의 경우 설정이 *두* 한정자와 모두 일치 해야 합니다.

또 다른 옵션은 한 폴더 이름에 여러 한정자를 결합 하는 것입니다.

```console
\Assets\Images\contrast-high_scale-400\<logo.png, and other image files>
```

폴더 이름에서 여러 한정자를 밑줄로 구분 하 여 결합 합니다. `<qualifier1>[_<qualifier2>...]` 는 형식입니다.

파일 이름에 여러 한정자를 같은 형식으로 결합할 수 있습니다.

```console
\Assets\Images\logo.contrast-high_scale-400.png
```

자산을 만드는 데 사용 하는 도구와 워크플로 또는 읽기 및/또는 관리가 가장 쉬운 방법에 따라 모든 한정자에 대해 단일 명명 전략을 선택 하거나 다른 한정자에 대해 결합할 수 있습니다.

## <a name="alternateform"></a>AlternateForm

한정자는 특정 `alternateform` 목적을 위해 리소스의 대체 형식을 제공 하는 데 사용 됩니다. 일반적으로이 기능은 일본어 앱 개발자만이 값이 예약 된 후리가나 문자열을 제공 하는 데 사용 됩니다 `msft-phonetic` . [지역화를 준비 하는 방법](/previous-versions/windows/apps/hh967762(v=win.10))에서 "정렬할 수 있는 일본어 문자열의 후리가나 지원" 섹션을 참조 하세요.

대상 시스템 또는 앱에서 한정자와 일치 하는 값을 제공 해야 합니다 `alternateform` . `msft-`사용자 지정 한정자 값에 접두사를 사용 하지 마십시오 `alternateform` .

## <a name="configuration"></a>구성

한정자 이름이 필요할 가능성이 거의 `configuration` 없습니다. 테스트 전용 리소스와 같이 지정 된 제작 시간 환경에만 적용 되는 리소스를 지정 하는 데 사용할 수 있습니다.

`configuration`한정자는 환경 변수 값과 가장 일치 하는 리소스를 로드 하는 데 사용 됩니다 `MS_CONFIGURATION_ATTRIBUTE_VALUE` . 따라서 변수를 관련 리소스 (예: 또는)에 할당 된 문자열 값으로 설정할 수 있습니다 `designer` `test` .

## <a name="contrast"></a>이 예와

`contrast`한정자는 고대비 설정과 가장 일치 하는 리소스를 제공 하는 데 사용 됩니다.

## <a name="custom"></a>사용자 지정

앱에서 한정자의 값을 설정할 수 있습니다 `custom` . 그러면 해당 값과 가장 일치 하는 리소스가 로드 됩니다. 예를 들어 앱의 라이선스를 기준으로 리소스를 로드 하는 것이 좋습니다. 앱이 시작 되 면 코드 예제에 표시 된 것 처럼 라이선스를 확인 하 고 `custom` [SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue)를 호출 하 여 한정자 값으로 사용 합니다.

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

이 시나리오에서는 한정자, 및를 포함 하는 리소스 이름을 지정 `custom-premium` `custom-standard` `custom-trial` 합니다.

## <a name="devicefamily"></a>DeviceFamily

한정자 이름이 필요할 가능성이 거의 `devicefamily` 없습니다. 가능 하면 훨씬 더 편리 하 고 강력한 기술로 사용할 수 있는 기술이 있기 때문에 가능 하면 사용 하지 않아야 합니다. 이러한 기술은 [앱이 실행 되](../porting/wpsl-to-uwp-input-and-sensors.md#detecting-the-platform-your-app-is-running-on) 고 있는 플랫폼과 [버전 적응 코드](../debug-test-perf/version-adaptive-code.md)검색에서 설명 합니다.

그러나 마지막 수단으로, XAML 뷰를 포함 하는 폴더의 이름을 지정할 때 devicefamily 한정자를 사용할 수 있습니다. XAML 뷰는 UI 레이아웃 및 컨트롤이 포함 된 XAML 파일입니다.

```console
\devicefamily-desktop\<MainPage.xaml, and other markup files to load when running on a desktop computer>
\devicefamily-mobile\<MainPage.xaml, and other markup files to load when running on a phone>
```

또는 파일의 이름을 지정할 수 있습니다.

```console
\MainPage.devicefamily-desktop.xaml
\MainPage.devicefamily-mobile.xaml
```

두 경우 모두의 각 복사본 `MainPage.[<qualifier>].xaml` 은 공통을 공유 하 고 `MainPage.xaml.cs` 이름, 위치 및 내용 측면에서 프로젝트에 변경 되지 않은 상태로 유지 됩니다.

또한 devicefamily 한정자를 사용 하 여 리소스 파일 () 또는 폴더의 이름을 지정할 수 있습니다 `.resw` . 예를 들어 앱이 모바일 장치 제품군에서 실행 되는 경우 UI 요소는 `<TextBlock x:Uid="DeviceFriendlyName"/>` 파일에 정의 된 텍스트 및 전경 리소스를 사용 합니다 `Resources.devicefamily-mobile.resw` .

```xml
<data name="DeviceFriendlyName.Foreground">
    <value>Red</value>
</data>
<data name="DeviceFriendlyName.Text">
    <value>Mobile device</value>
</data>
```

리소스 파일을 사용 하는 방법에 대 한 자세한 내용은 [UI 문자열 지역화](localize-strings-ui-manifest.md)를 참조 하세요.

## <a name="dxfeaturelevel"></a>DXFeatureLevel

한정자 이름이 필요할 가능성이 거의 `dxfeaturelevel` 없습니다. 이는 특정 하위 하드웨어 구성과 일치 하도록 하위 리소스를 로드 하는 Direct3D 게임 자산과 함께 사용 하도록 설계 되었습니다. 그러나 이제 해당 하드웨어 구성이 전파 되기 때문에이 한정자를 사용 하지 않는 것이 좋습니다.

## <a name="homeregion"></a>HomeRegion

`homeregion`한정자는 국가 또는 지역에 대 한 사용자 설정에 해당 합니다. 사용자의 홈 위치를 나타냅니다. 값에는 유효한 모든 [BCP-47 지역 태그가](https://tools.ietf.org/html/bcp47)포함 됩니다. 즉, **iso 3166-1** 두 문자로 된 지역 코드와 구성 된 지역에 대 한 **iso 3166-1 숫자** 3 자리 지역 코드 집합을 더한 값 (국가 [통계 나누기 M49 지역 코드 컴퍼지션](https://unstats.un.org/unsd/methods/m49/m49regin.htm)참조)입니다. "선택한 경제 및 기타 그룹"에 대 한 코드는 유효 하지 않습니다.

## <a name="language"></a>언어

`language`한정자는 표시 언어 설정에 해당 합니다. 값에는 유효한 모든 [BCP-47 언어 태그가](https://tools.ietf.org/html/bcp47)포함 됩니다. 언어 목록은 [IANA language 하위 태그 레지스트리](https://www.iana.org/assignments/language-subtag-registry)를 참조 하세요.

앱에서 다른 표시 언어를 지원 하 고 코드 또는 XAML 태그에 문자열 리터럴이 있는 경우 해당 문자열을 코드/태그에서 리소스 파일 ()로 이동 `.resw` 합니다. 그러면 앱에서 지원하는 언어별로 해당 리소스 파일의 변환된 복사본을 만들 수 있습니다.

일반적으로 한정자를 사용 `language` 하 여 리소스 파일을 포함 하는 폴더의 이름을로 `.resw` 합니다 ().

```console
\Strings\language-en\Resources.resw
\Strings\language-ja\Resources.resw
```

`language-` `language` 한정자 (한정자 이름)의 일부를 생략할 수 있습니다. 다른 종류의 한정자를 사용 하 여이 작업을 수행할 수 없습니다. 폴더 이름 에서만이 작업을 수행할 수 있습니다.

```console
\Strings\en\Resources.resw
\Strings\ja\Resources.resw
```

폴더의 이름을 지정 하는 대신 한정자를 사용 `language` 하 여 리소스 파일의 이름을 지정할 수 있습니다.

```console
\Strings\Resources.language-en.resw
\Strings\Resources.language-ja.resw
```

문자열 리소스를 사용 하 여 응용 프로그램을 지역화할 수 있도록 하는 방법 및 앱에서 문자열 리소스를 참조 하는 방법에 대 한 자세한 내용은 [UI 문자열 지역화](localize-strings-ui-manifest.md) 를 참조 하세요.

## <a name="layoutdirection"></a>LayoutDirection

`layoutdirection`한정자는 표시 언어 설정의 레이아웃 방향에 해당 합니다. 예를 들어 아랍어 또는 히브리어와 같이 오른쪽에서 왼쪽으로 쓰기 언어에 대해 이미지를 미러링 해야 할 수 있습니다. UI의 레이아웃 패널 및 이미지는 [Flowdirection](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) 속성을 설정 하는 경우 적절 한 레이아웃 방향에 응답 합니다 ( [레이아웃 및 글꼴 조정 및 RTL 지원](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md)참조). 그러나 `layoutdirection` 간단한 이동이 적절 하지 않은 경우에는 한정자를 사용 하 여 특정 읽기 순서와 텍스트 맞춤의 방향에 보다 일반적인 방식으로 응답할 수 있습니다.

## <a name="scale"></a>확장

Windows는 DPI (인치당 도트 수)와 장치의 보기 거리를 기준으로 각 표시에 대 한 배율 인수를 자동으로 선택 합니다. [효과적인 픽셀 및 배율 인수를](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md#effective-pixels-and-scale-factor)참조 하세요. Windows에서 완벽 한 크기를 선택 하거나 가장 가까운 크기를 사용 하 고 크기를 조정할 수 있도록 몇 가지 권장 크기 (최소 100, 200 및 400)로 이미지를 만들어야 합니다. Windows에서 표시 배율 인수에 대 한 이미지의 올바른 크기를 포함 하는 물리적 파일을 식별할 수 있도록 한정자를 사용 `scale` 합니다. 리소스의 소수 자릿수가 DisplayInformation의 값, [ResolutionScale](/uwp/api/windows.graphics.display.displayinformation.ResolutionScale)또는 그 다음으로 가장 큰 크기의 리소스와 일치 합니다.

다음은 폴더 수준에서 한정자를 설정 하는 예제입니다.

```console
\Assets\Images\scale-100\<logo.png, and other image files>
\Assets\Images\scale-200\<logo.png, and other image files>
\Assets\Images\scale-400\<logo.png, and other image files>
```

이 예에서는 파일 수준에서 설정 합니다.

```console
\Assets\Images\logo.scale-100.png
\Assets\Images\logo.scale-200.png
\Assets\Images\logo.scale-400.png
```

및 둘 다에 대해 리소스를 정규화 하는 방법에 대 한 정보는 `scale` `targetsize` [targetsize에 대 한 이미지 리소스 한정](images-tailored-for-scale-theme-contrast.md#qualify-an-image-resource-for-targetsize)을 참조 하세요.

## <a name="targetsize"></a>TargetSize

한정자는 파일 `targetsize` 탐색기에 표시 되는 [파일 형식 연결 아이콘](/windows/desktop/shell/how-to-assign-a-custom-icon-to-a-file-type) 또는 [프로토콜 아이콘](/windows/desktop/search/-search-3x-wds-ph-ui-extensions) 을 지정 하는 데 주로 사용 됩니다. 한정자 값은 raw (실제) 픽셀의 정사각형 이미지의 측면 길이를 나타냅니다. 파일 탐색기에서 보기 설정과 일치 하는 값을 가진 리소스가 로드 됩니다. 또는 정확히 일치 하는 항목이 없을 경우 다음으로 큰 값이 포함 된 리소스입니다.

`targetsize` `/Assets/Square44x44Logo.png` 앱 패키지 매니페스트 디자이너의 시각적 자산 탭에서 앱 아이콘 ()에 대 한 여러 가지 한정자 값 크기를 나타내는 자산을 정의할 수 있습니다.

및 둘 다에 대해 리소스를 정규화 하는 방법에 대 한 정보는 `scale` `targetsize` [targetsize에 대 한 이미지 리소스 한정](images-tailored-for-scale-theme-contrast.md#qualify-an-image-resource-for-targetsize)을 참조 하세요.

## <a name="theme"></a>테마

`theme`한정자는 기본 앱 모드 설정과 가장 일치 하는 리소스를 제공 하거나 [application.requestedtheme](/uwp/api/windows.ui.xaml.application.requestedtheme)를 사용 하 여 앱을 재정의 하는 데 사용 됩니다.


## <a name="shell-light-theme-and-unplated-resources"></a>셸 밝은 테마 및 플레이트 되지 않은 리소스
*Windows 10 5 월 2019 업데이트* 에는 windows 셸에 대 한 새로운 "light" 테마가 도입 되었습니다. 결과적으로 어두운 배경에 이전에 표시 된 일부 응용 프로그램 자산은 이제 밝은 배경에 표시 됩니다. 작업 표시줄 및 창 전환 (Alt + Tab, 작업 보기 등)에 대해 플레이트 되지 않은 자산을 제공 하는 앱이 제공 하는 앱의 경우 밝은 배경에서 허용 되는 대비를 확인 해야 합니다.

### <a name="providing-light-theme-specific-assets"></a>밝은 테마 특정 자산 제공
셸 밝은 테마에 대해 맞춤형 리소스를 제공 하려는 앱은 새로운 대체 양식 리소스 한정자를 사용할 수 있습니다 `altform-lightunplated` . 이 한정자는 기존 altform 한정자를 미러링합니다. 

### <a name="downlevel-considerations"></a>하위 수준 고려 사항
앱은 한정자와 한정자를 함께 사용 하면 안 됩니다 `theme-light` `altform-unplated` . 이렇게 하면 작업 표시줄에 대 한 리소스가 로드 되는 방식 때문에 RS5 및 이전 버전의 Windows에서 예기치 않은 동작이 발생 합니다. 이전 버전의 windows에서는 테마-밝은 버전이 잘못 사용 될 수 있습니다. `altform-lightunplated`한정자는이 문제를 방지 합니다. 

### <a name="compatibility-behavior"></a>호환성 동작
이전 버전과의 호환성을 위해 Windows에는 단색 아이콘을 검색 하 고 원하는 배경과 대조 되는지 여부를 확인 하는 논리가 포함 되어 있습니다. 아이콘이 대비 요구 사항에 맞지 않을 경우 Windows는 대비 흰색 버전의 자산을 찾습니다. 이 기능을 사용할 수 없는 경우 Windows에서는 plated 버전의 자산을 사용 하는 것으로 대체 합니다.



## <a name="important-apis"></a>중요 API

* [ResourceContext](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)
* [SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue)

## <a name="related-topics"></a>관련 항목

* [유효 픽셀 및 배율](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md#effective-pixels-and-scale-factor)
* [리소스 관리 시스템](resource-management-system.md)
* [지역화 준비 방법](/previous-versions/windows/apps/hh967762(v=win.10))
* [앱이 실행 되 고 있는 플랫폼 검색](../porting/wpsl-to-uwp-input-and-sensors.md#detecting-the-platform-your-app-is-running-on)
* [확장 Sdk를 사용한 프로그래밍](/uwp/extension-sdks/device-families-overview)
* [UI 문자열 지역화](localize-strings-ui-manifest.md)
* [BCP-47](https://tools.ietf.org/html/bcp47)
* [국가 통계 나누기 M49 지역 코드 컴퍼지션](https://unstats.un.org/unsd/methods/m49/m49regin.htm)
* [IANA 언어 하위 태그 레지스트리](https://www.iana.org/assignments/language-subtag-registry)
* [레이아웃 및 글꼴 조정, RTL 지원](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md)