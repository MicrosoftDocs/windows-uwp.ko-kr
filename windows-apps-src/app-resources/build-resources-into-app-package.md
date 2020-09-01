---
Description: 몇 가지 종류의 앱(다국어 사전, 번역 도구 등)은 앱 번들의 기본 동작을 재정의하고, 리소스를 별도의 리소스 패키지에 포함하지 않고 앱 패키지에 빌드해야 합니다. 이 문서에서는 이러한 작업을 수행하는 방법에 대해 설명합니다.
title: 앱 패키지에 리소스 빌드
template: detail.hbs
ms.date: 11/14/2017
ms.topic: article
keywords: Windows 10, UWP, 리소스, 이미지, 자산, MRT, 한정자
ms.localizationpriority: medium
ms.openlocfilehash: b975dcf88ecd26dc5a24d602c117b779fa2aada6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174527"
---
# <a name="build-resources-into-your-app-package-instead-of-into-a-resource-pack"></a>리소스 팩 대신 앱 패키지에 리소스 빌드

일부 종류의 앱 (다국어 사전, 번역 도구 등)은 앱 번들의 기본 동작을 재정의 하 고 리소스를 별도의 리소스 패키지 (또는 리소스 팩)에 배치 하는 대신 앱 패키지에 빌드 해야 합니다. 이 문서에서는 이러한 작업을 수행하는 방법에 대해 설명합니다.

기본적으로 [앱 번들 (.appxbundle)](/windows/msix/package/packaging-uwp-apps)을 빌드할 때 언어, 배율 및 DirectX 기능 수준에 대 한 기본 리소스만 앱 패키지에 빌드됩니다. &mdash;기본이 아닌 크기 및/또는 DirectX 기능 수준에 맞게 조정 된 번역 된 리소스 및 리소스 &mdash; 는 리소스 패키지에 기본 제공 되며이를 필요로 하는 장치에만 다운로드 됩니다. 고객이 언어 기본 설정이 스페인어로 설정 된 장치를 사용 하 여 Microsoft Store에서 앱을 구입 하는 경우에는 앱과 스페인어 리소스 패키지만 다운로드 하 고 설치 됩니다. 동일한 사용자가 나중에 **설정**에서 언어 기본 설정을 프랑스어로 변경 하는 경우 앱의 프랑스어 리소스 패키지가 다운로드 되어 설치 됩니다. 크기 조정 및 DirectX 기능 수준에 대 한 리소스를 사용 하는 유사한 상황이 발생 합니다. 대부분의 앱에서이 동작은 중요 한 효율성을 구성 하 고 사용자와 고객이 수행 *하려는* 작업과 정확히 일치 합니다.

그러나 앱에서 사용자가 앱 내에서 즉시 언어를 변경할 수 있도록 허용 하는 경우 ( **설정**대신) 해당 기본 동작이 적절 하지 않습니다. 실제로 모든 언어 리소스를 한 번에 앱과 함께 무조건 다운로드 및 설치 하 고 장치에 그대로 둘 수 있습니다. 이러한 모든 리소스를 별도의 리소스 패키지가 아닌 앱 패키지에 빌드 하려고 합니다.

**참고** 앱 패키지에 리소스를 포함 하면 기본적으로 앱 크기가 늘어납니다. 응용 프로그램의 특성에서 요청 하는 경우에만이 작업을 수행할 가치가 있습니다. 그렇지 않은 경우 일반적인 앱 번들을 빌드하는 것 외에는 작업을 수행할 필요가 없습니다.

다음 두 가지 방법 중 하나로 응용 프로그램 패키지에 리소스를 빌드하기 위해 Visual Studio를 구성할 수 있습니다. 프로젝트에 구성 파일을 추가 하거나 프로젝트 파일을 직접 편집할 수 있습니다. 가장 편한 옵션 또는 빌드 시스템에 가장 적합 한 옵션을 사용 합니다.

## <a name="option-1-use-priconfigpackagingxml-to-build-resources-into-your-app-package"></a>옵션 1. priconfig.packaging.xml를 사용 하 여 앱 패키지에 리소스 빌드

1. Visual Studio에서 프로젝트에 새 항목을 추가 합니다. XML 파일을 선택 하 고 파일의 이름을로 `priconfig.packaging.xml` 합니다.
2. 솔루션 탐색기에서 속성 창를 선택 `priconfig.packaging.xml` 하 고 확인 합니다. 파일의 빌드 작업을 None으로 설정 하 고 출력 디렉터리로 복사를 복사 안 함으로 설정 해야 합니다.
3. 파일의 내용을이 XML로 바꿉니다.
   ```xml
   <packaging>
      <autoResourcePackage qualifier="Language" />
      <autoResourcePackage qualifier="Scale" />
      <autoResourcePackage qualifier="DXFeatureLevel" />
   </packaging>
   ```
4. 각 `<autoResourcePackage>` 요소는 지정 된 한정자 이름에 대 한 리소스를 별도의 리소스 패키지로 자동 분할 하도록 Visual Studio에 지시 합니다. 이를 *자동 분할*이라고 합니다. 지금까지 파일 콘텐츠를 사용 하 여 Visual Studio의 동작을 실제로 변경 하지 않았습니다. 즉, Visual Studio는이 파일이 기본값 이기 때문에이 파일이 있는 *것 처럼 이미 동작* 했습니다. Visual Studio가 한정자 이름에 자동으로 분할 되지 않도록 하려면 `<autoResourcePackage>` 파일에서 해당 요소를 삭제 합니다. 이 파일은 별도의 리소스 패키지로 자동 분할 되는 대신 모든 언어 리소스를 앱 패키지에 제공 하려는 경우에 표시 되는 방법입니다.
   ```xml
   <packaging>
      <autoResourcePackage qualifier="Scale" />
      <autoResourcePackage qualifier="DXFeatureLevel" />
   </packaging>
   ```
5. 파일을 저장 하 고 닫은 다음 프로젝트를 다시 빌드합니다.

자동 분할 선택을 고려 하 고 있는지 확인 하려면 파일을 찾아 `<ProjectFolder>\obj\<ReleaseConfiguration folder>\split.priconfig.xml` 해당 내용이 선택 내용과 일치 하는지 확인 합니다. 이러한 작업을 수행 하는 경우 앱 패키지에 선택한 리소스를 빌드하기 위해 Visual Studio를 성공적으로 구성 했습니다.

수행 해야 하는 마지막 단계가 하나 있습니다. ** `Language` 한정자 이름을 삭제 한 경우에만 사용할 수**있습니다. 앱의 지원 되는 모든 언어의 공용 구조체를 언어에 대 한 앱 기본값으로 지정 해야 합니다. 자세한 내용은 앱에서 [사용 하는 기본 리소스 지정](specify-default-resources-installed.md)을 참조 하세요. 이는 `priconfig.default.xml` 앱 패키지에 영어, 스페인어 및 프랑스어에 대 한 리소스를 포함 하는 경우에 포함 됩니다.

```xml
   <default>
      <qualifier name="Language" value="en;es;fr" />
      ...
   </default>
```

### <a name="how-does-this-work"></a>어떻게 작동하나요?

백그라운드에서 Visual Studio는 `MakePri.exe` 자동으로 분할할 리소스 한정자 이름을 지정 하는 것을 포함 하 여 모든 앱의 리소스를 설명 하는 패키지 리소스 인덱스 라는 파일을 생성 하기 위해 라는 도구를 시작 합니다. 이 도구에 대 한 자세한 내용은 [MakePri.exe를 사용 하 여 수동으로 리소스 컴파일 ](compile-resources-manually-with-makepri.md)을 참조 하세요. Visual Studio는에 구성 파일을 전달 `MakePri.exe` 합니다. 파일의 내용은 `priconfig.packaging.xml` `<packaging>` 자동 분할을 결정 하는 부분인 해당 구성 파일의 요소로 사용 됩니다. 따라서를 추가 하 고 편집 하면 `priconfig.packaging.xml` Visual Studio에서 앱에 대해 생성 하는 패키지 리소스 인덱스 파일의 내용 뿐만 아니라 앱 번들의 패키지 내용에도 영향을 미칩니다.

### <a name="using-a-different-file-name-than-priconfigpackagingxml"></a>다른 파일 이름 사용 `priconfig.packaging.xml`

파일의 이름을 `priconfig.packaging.xml` 로 지정한 경우 Visual Studio에서 해당 파일을 인식 하 여 자동으로 사용 합니다. 다른 이름을 지정 하는 경우 Visual Studio에서 알 수 있도록 해야 합니다. 프로젝트 파일에서 첫 번째 요소의 여는 태그와 닫는 태그 사이에 `<PropertyGroup>` 이 XML을 추가 합니다.

```xml
<AppxPriConfigXmlPackagingSnippetPath>FILE-PATH-AND-NAME</AppxPriConfigXmlPackagingSnippetPath>
```

`FILE-PATH-AND-NAME`파일의 경로 및 이름으로 대체 합니다.

## <a name="option-2-use-your-project-file-to-build-resources-into-your-app-package"></a>옵션 2. 프로젝트 파일을 사용 하 여 앱 패키지에 리소스 빌드

이는 옵션 1에 대 한 대안입니다. 옵션 1의 작동 방식을 이해 하면 개발 및/또는 빌드 워크플로에 적합 한 경우 옵션 2를 대신 선택할 수 있습니다.

프로젝트 파일에서 첫 번째 요소의 여는 태그와 닫는 태그 사이에 `<PropertyGroup>` 이 XML을 추가 합니다.

```xml
<AppxBundleAutoResourcePackageQualifiers>Language|Scale|DXFeatureLevel</AppxBundleAutoResourcePackageQualifiers>
```

첫 번째 한정자 이름을 삭제 한 후 표시 되는 방법은 다음과 같습니다.

```xml
<AppxBundleAutoResourcePackageQualifiers>Scale|DXFeatureLevel</AppxBundleAutoResourcePackageQualifiers>
```

프로젝트를 저장 하 고 닫은 후 다시 빌드합니다.

수행 해야 하는 마지막 단계가 하나 있습니다. ** `Language` 한정자 이름을 삭제 한 경우에만 사용할 수**있습니다. 앱의 지원 되는 모든 언어의 공용 구조체를 언어에 대 한 앱 기본값으로 지정 해야 합니다. 자세한 내용은 앱에서 [사용 하는 기본 리소스 지정](specify-default-resources-installed.md)을 참조 하세요. 응용 프로그램 패키지에 영어, 스페인어 및 프랑스어에 대 한 리소스를 포함 한 경우 프로젝트 파일에 포함 됩니다.

```xml
<AppxDefaultResourceQualifiers>Language=en;es;fr</AppxDefaultResourceQualifiers>
```

## <a name="related-topics"></a>관련 항목

* [Visual Studio를 사용하여 UWP 앱 패키징](/windows/msix/package/packaging-uwp-apps)
* [MakePri.exe를 사용하여 수동으로 리소스 컴파일](compile-resources-manually-with-makepri.md)
* [앱에서 사용하는 기본 리소스 지정](specify-default-resources-installed.md)