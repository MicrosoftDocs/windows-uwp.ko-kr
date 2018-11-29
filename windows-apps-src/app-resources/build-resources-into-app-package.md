---
Description: Some kinds of apps (multilingual dictionaries, translation tools, etc.) need to override the default behavior of an app bundle, and build resources into the app package instead of having them in separate resource packages. This topic explains how to do that.
title: 리소스 팩 대신 앱 패키지에 리소스 빌드
template: detail.hbs
ms.date: 11/14/2017
ms.topic: article
keywords: Windows 10, uwp, 리소스, 이미지, 자산, MRT, 한정자
ms.localizationpriority: medium
ms.openlocfilehash: 8bf2d34bc3dae20750f66c9116499a17444b798c
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8199811"
---
# <a name="build-resources-into-your-app-package-instead-of-into-a-resource-pack"></a>리소스 팩 대신 앱 패키지에 리소스 빌드

일부 앱(다국어 사전, 번역 도구 등)은 앱 번들의 기본 동작을 재정의하고, 리소스를 별도의 리소스 패키지(또는 리소스 팩)에 두는 대신 앱 패키지에 빌드해야 합니다. 이 항목에서는 이러한 작업을 수행하는 방법을 설명합니다.

기본적으로 [앱 번들(.appxbundle)](../packaging/packaging-uwp-apps.md)을 빌드하면, 언어, 크기 및 DirectX 기능 수준에 대한 기본 리소스만 앱 패키지에 빌드됩니다. 번역된 리소스&mdash;및 기본 이외 크기에 맞게 조정된 리소스 및/또는 DirectX 기능 수준&mdash;는 리소스 패키지에 빌드되며, 필요한 장치에만 다운로드됩니다. 고객이 언어 기본 설정이 스페인어인 장치를 사용하여 Microsoft Store로부터 앱을 구매하게 되면, 앱과 스페인어 리소스 패키지만 다운로드 및 설치됩니다. 동일한 사용자가 나중에 **설정**에서 언어 기본 설정을 스페인어로 변경하게 되면, 앱의 프랑스어 리소스 패키지가 다운로드 및 설치됩니다. 크기 및 DirectX 기능 수준에 대한 기준을 충족하는 리소스에 유사한 동작이 발생합니다. 대부분의 앱에서는 이러한 동작 덕분에 효율성이 높아지므로 사용자와 고객 모두가 *원하는* 바를 충족해준다고 볼 수 있습니다.

그러나 앱 내에서 즉시 언어를 변경하도록 허용하는 앱의 경우(**설정**을 통하지 않고), 해당 기본 동작이 적절치 않습니다. 이러한 경우 모든 언어 리소스를 앱과 함께 한 번에 조건 없이 다운로드 및 설치한 다음 장치에 남겨 놓길 원할 것입니다. 리소스 모두를 별도의 리소스 패키지가 아닌 앱 패키지에 빌드하고자 할 것입니다.

**참고** 앱 패키지에 리소스를 포함하면 앱의 크기가 기본적으로 증가하게 됩니다. 따라서 앱의 특성 상 필요한 경우에만 해당 작업을 수행하는 것이 좋습니다. 필요하지 않은 경우에는 평소대로 일반 앱 번들만 빌드하면 됩니다.

Visual Studio가 앱 패키지에 리소스를 빌드하도록 구성하는 데에는 두 가지 방법이 있습니다. 즉, 프로젝트에 구성 파일을 추가할 수도 있고, 프로젝트 파일을 직접 편집할 수도 있습니다. 두 가지 중 자신에게 가장 편한 방법이나 해당 빌드 시스템에 가장 적합한 방법을 사용하세요.

## <a name="option-1-use-priconfigpackagingxml-to-build-resources-into-your-app-package"></a>옵션 1. priconfig.packaging.xml을 사용하여 앱 패키지에 리소스 빌드

1. Visual Studio에서 프로젝트에 새 항목을 추가합니다. XML 파일을 선택하고 파일의 이름을 `priconfig.packaging.xml`로 지정합니다.
2. 솔루션 탐색기에서 `priconfig.packaging.xml`을 선택하고 속성 창을 확인합니다. 해당 파일의 [빌드 작업]을 [없음]으로 설정하고 [출력 디렉터리로 복사]를 [복사 안 함]으로 설정해야 합니다.
3. 파일의 내용을 이 XML로 바꿉니다.
   ```xml
   <packaging>
      <autoResourcePackage qualifier="Language" />
      <autoResourcePackage qualifier="Scale" />
      <autoResourcePackage qualifier="DXFeatureLevel" />
   </packaging>
   ```
4. 각 `<autoResourcePackage>` 요소가 Visual Studio에게 주어진 한정자 이름에 대한 리소스를 별도의 리소스 패키지로 자동으로 분할하도록 지시합니다. 이를 *자동 분할*이라고 합니다. 지금까지의 파일 내용으로는, Visual Studio의 동작이 변경되지 않습니다. 다시 말하면, 이러한 내용은 기본값이므로 Visual Studio는 이 파일에 해당 내용이 있는 것처럼 *이미 동작하고 있습니다*. Visual Studio가 한정자 이름에서 자동 분할되지 않도록 하려면 파일에서 해당 `<autoResourcePackage>` 요소를 삭제하세요. 별도 리포스 패키지로 자동 분할되지 않고 모든 언어 리소스가 앱 패키지에 빌드되길 원했다면 파일이 다음과 같을 것입니다.
   ```xml
   <packaging>
      <autoResourcePackage qualifier="Scale" />
      <autoResourcePackage qualifier="DXFeatureLevel" />
   </packaging>
   ```
5. 파일을 저장하고 닫은 다음, 프로젝트를 다시 빌드합니다.

선택한 자동 분할 방법이 제대로 적용되고 있는지 확인하려면 `<ProjectFolder>\obj\<ReleaseConfiguration folder>\split.priconfig.xml` 파일을 찾아 내용이 선택 방법과 일치하는지 확인합니다. 일치하면 선택한 방법대로 리소스가 앱 패키지에 빌드되도록 Visual Studio를 구성한 것입니다.

이제 마지막 한 단계만 수행하면 됩니다. **단, `Language` 한정자 이름을 삭제한 경우에만 그렇습니다**. 앱의 지원되는 언어 모두를 앱의 기본 언어로 통합하도록 지정해야 합니다. 자세한 내용은 [앱에서 사용하는 기본 리소스 지정](specify-default-resources-installed.md)을 참조하세요. 이는 영어, 스페인어 및 프랑스어를 앱 패키지에 포함하고 있었던 경우 `priconfig.default.xml`에 들어 있었을 내용입니다.

```xml
   <default>
      <qualifier name="Language" value="en;es;fr" />
      ...
   </default>
```

### <a name="how-does-this-work"></a>어떻게 작동합니까?

백그라운드에서 Visual Studio는 `MakePri.exe`라는 도구를 실행하여 패키지 리소스 인덱스라고 하는 파일을 생성합니다. 이 파일에는 자동 분할의 기준이 되는 리소스 한정자 이름을 나타내는 등 모든 앱 리소스에 대한 설명이 들어 있습니다. 이 도구에 대한 자세한 내용은 [MakePri.exe를 사용하여 수동으로 리소스 컴파일](compile-resources-manually-with-makepri.md)을 참조하세요. Visual Studio가 구성 파일을 `MakePri.exe`에 전달합니다. `priconfig.packaging.xml` 파일의 내용이 자동 분할을 결정하는 부분인 구성 파일의 `<packaging>` 요소로 사용됩니다. 따라서 `priconfig.packaging.xml`을 추가하고 편집하면 Visual Studio가 앱에 대해 생성하는 패키지 리소스 인덱스 파일의 내용뿐만 아니라 앱 번들에 있는 패키지의 내용에도 영향이 미칩니다.

### <a name="using-a-different-file-name-than-priconfigpackagingxml"></a>`priconfig.packaging.xml`이 아닌 다른 파일 이름 사용

파일의 이름을 `priconfig.packaging.xml`로 지정하면, Visual Studio가 이를 인식하고 자동으로 사용합니다. 다른 이름으로 지정하려면, Visual Studio에서 이를 알 수 있도록 해야 합니다. 프로젝트 파일에서 첫 번째 `<PropertyGroup>` 요소의 여는 태그와 닫는 태그 사이에 이 XML을 추가합니다.

```xml
<AppxPriConfigXmlPackagingSnippetPath>FILE-PATH-AND-NAME</AppxPriConfigXmlPackagingSnippetPath>
```

`FILE-PATH-AND-NAME`을 파일의 경로 및 이름으로 바꿉니다.

## <a name="option-2-use-your-project-file-to-build-resources-into-your-app-package"></a>옵션 2. 프로젝트 파일을 사용하여 앱 패키지에 리소스 빌드

이 방법은 옵션 1에 대한 대안입니다. 옵션 1의 작동 방식을 이해했으면 옵션 2가 개발 방식에 적합하고 워크플로를 더 잘 빌드하는 경우 옵션 2를 대신 선택해도 됩니다.

프로젝트 파일에서 첫 번째 `<PropertyGroup>` 요소의 여는 태그와 닫는 태그 사이에 이 XML을 추가합니다.

```xml
<AppxBundleAutoResourcePackageQualifiers>Language|Scale|DXFeatureLevel</AppxBundleAutoResourcePackageQualifiers>
```

첫 번째 한정자 이름을 삭제하면 다음과 같이 표시됩니다.

```xml
<AppxBundleAutoResourcePackageQualifiers>Scale|DXFeatureLevel</AppxBundleAutoResourcePackageQualifiers>
```

프로젝트를 저장하고 닫은 다음, 다시 빌드합니다.

이제 마지막 한 단계만 수행하면 됩니다. **단, `Language` 한정자 이름을 삭제한 경우에만 그렇습니다**. 앱의 지원되는 언어 모두를 앱의 기본 언어로 통합하도록 지정해야 합니다. 자세한 내용은 [앱에서 사용하는 기본 리소스 지정](specify-default-resources-installed.md)을 참조하세요. 이는 영어, 스페인어 및 프랑스어를 앱 패키지에 포함하고 있었던 경우 프로젝트 파일에 들어 있었을 내용입니다.

```xml
<AppxDefaultResourceQualifiers>Language=en;es;fr</AppxDefaultResourceQualifiers>
```

## <a name="related-topics"></a>관련 항목

* [Visual Studio를 사용하여 UWP 앱 패키징](../packaging/packaging-uwp-apps.md)
* [MakePri.exe를 사용하여 수동으로 리소스 컴파일](compile-resources-manually-with-makepri.md)
* [앱에서 사용하는 기본 리소스 지정](specify-default-resources-installed.md)