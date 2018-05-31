---
author: stevewhims
Description: If your app doesn't have resources that match the particular settings of a customer device, then the app's default resources are used. This topic explains how to specify what those default resources are.
title: 앱에서 사용하는 기본 리소스 지정
template: detail.hbs
ms.author: stwhi
ms.date: 11/14/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 리소스, 이미지, 자산, MRT, 한정자
ms.localizationpriority: medium
ms.openlocfilehash: 6f88a6d6be6bf938564f0a31eac118983c256a7e
ms.sourcegitcommit: f9a4854b6aecfda472fb3f8b4a2d3b271b327800
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/12/2017
ms.locfileid: "1395992"
---
# <a name="specify-the-default-resources-that-your-app-uses"></a>앱에서 사용하는 기본 리소스 지정

앱에 고객 장치의 특정 설정과 일치하는 리소스가 없으면, 앱의 기본 리소스가 사용됩니다. 이 항목에서는 그러한 기본 리소스를 지정하는 방법에 대해 설명합니다.

고객이 Microsoft Store로부터 앱을 설치하면 고객의 장치 설정이 앱의 사용 가능 리소스와 매칭됩니다. 이러한 매칭은 해당 사용자가 적합한 리소스만 다운로드하고 설치하도록 하기 위해 수행됩니다. 예를 들어, 해당 사용자의 언어 기본 설정, 장치의 해상도와 DPI 설정에 가장 잘 맞는 문자열과 이미지만 사용됩니다. 예를 들어, `200`이 `scale`의 기본값이지만, 원하는 경우 이를 덮어쓸 수 있습니다.

자체 리소스 팩에 포함되지 않을 리소스의 경우에도(고대비 설정용으로 조정된 이미지 등), 사용자의 설정과 일치하는 리소스가 없는 경우 런타임에서 사용되어야 하는 기본 리소스를 지정할 수 있습니다. 예를 들어, `standard`가 `contrast`의 기본값이지만, 원하는 경우 이를 덮어쓸 수 있습니다.

이러한 기본값은 기본값 리소스 한정자 값 형식으로 지정됩니다. 리소스 한정자의 정의, 사용법 및 용도에 대한 자세한 설명은 [언어, 규모, 고대비 및 기타 한정자에 맞게 리소스 조정](tailor-resources-lang-scale-contrast.md)을 참조하세요.

이러한 기본값은 두 가지 방법 중 하나로 구성할 수 있습니다. 즉, 프로젝트에 구성 파일을 추가할 수도 있고, 프로젝트 파일을 직접 편집할 수도 있습니다. 두 가지 중 자신에게 가장 편한 방법이나 해당 빌드 시스템에 가장 적합한 방법을 사용하세요.

## <a name="option-1-use-priconfigdefaultxml-to-specify-default-qualifier-values"></a>옵션 1. priconfig.default.xml을 사용하여 기본값 한정자 값 지정

1. Visual Studio에서 프로젝트에 새 항목을 추가합니다. XML 파일을 선택하고 파일의 이름을 `priconfig.default.xml`로 지정합니다.
2. 솔루션 탐색기에서 `priconfig.default.xml`을 선택하고 속성 창을 확인합니다. 해당 파일의 [빌드 작업]을 [없음]으로 설정하고 [출력 디렉터리로 복사]를 [복사 안 함]으로 설정해야 합니다.
3. 파일의 내용을 이 XML로 바꿉니다.
   ```xml
   <default>
      <qualifier name="Language" value="LANGUAGE-TAG(S)" />
      <qualifier name="Contrast" value="standard" />
      <qualifier name="Scale" value="200" />
      <qualifier name="HomeRegion" value="001" />
      <qualifier name="TargetSize" value="256" />
      <qualifier name="LayoutDirection" value="LTR" />
      <qualifier name="DXFeatureLevel" value="DX9" />
      <qualifier name="Configuration" value="" />
      <qualifier name="AlternateForm" value="" />
   </default>
   ```
   
   **참고** 값 `LANGUAGE-TAG(S)`는 앱의 기본 언어와 동기화되어야 합니다. 단일 [BCP 47 언어 태그](http://go.microsoft.com/fwlink/p/?linkid=227302)인 경우, 앱의 기본 언어가 이와 동일한 태그여야 합니다. 쉼표로 구분된 언어 태그 목록인 경우, 앱의 기본 언어가 목록의 첫 번째 태그여야 합니다. 앱 패키지 매니페스트 소스 파일(`Package.appxmanifest`)의 **응용 프로그램** 탭에 있는 **기본 언어** 필드에서 앱의 기본 언어를 설정합니다.

4. 각 `<qualifier>` 요소는 각 한정자 이름의 기본값으로 사용할 값을 Visual Studio에 알려줍니다. 지금까지의 파일 내용으로는, Visual Studio의 동작이 변경되지 않습니다. 다시 말하면, 이러한 내용은 기본값이므로 Visual Studio는 이 파일에 해당 내용이 있는 것처럼 *이미 동작하고 있습니다*. 따라서 자신만의 기본값으로 기본값을 덮어쓰려면 파일의 값을 변경해야 합니다. 다음은 처음 세 값을 편집했을 경우 파일의 모습을 보여주는 예입니다.
   ```xml
   <default>
      <qualifier name="Language" value="de-DE" />
      <qualifier name="Contrast" value="black" />
      <qualifier name="Scale" value="400" />
      <qualifier name="HomeRegion" value="001" />
      <qualifier name="TargetSize" value="256" />
      <qualifier name="LayoutDirection" value="LTR" />
      <qualifier name="DXFeatureLevel" value="DX9" />
      <qualifier name="Configuration" value="" />
      <qualifier name="AlternateForm" value="" />
   </default>
   ```
5. 파일을 저장하고 닫은 다음, 프로젝트를 다시 빌드합니다.

덮어쓴 기본값이 제대로 적용되고 있는지 확인하려면 `<ProjectFolder>\obj\<ReleaseConfiguration folder>\priconfig.xml` 파일을 찾아 내용이 덮어쓴 값과 일치하는지 확인합니다. 일치하는 경우, 앱이 기본적으로 사용할 리소스 한정자 값을 성공적으로 구성한 것입니다. 사용자 설정과 일치하는 값이 없는 경우, 여기서 설정한 기본 한정자 값이 들어 있는 폴더나 파일 이름에 리소스가 사용됩니다.

### <a name="how-does-this-work"></a>어떻게 작동합니까?

백그라운드에서 Visual Studio는 `MakePri.exe`라는 도구를 실행하여 PRI(패키지 리소스 인덱스)라고 하는 파일을 생성합니다. 이 파일에는 기본 리소스임을 나타내는 등 모든 앱 리소스에 대한 설명이 들어 있습니다. 이 도구에 대한 자세한 내용은 [MakePri.exe를 사용하여 수동으로 리소스 컴파일](compile-resources-manually-with-makepri.md)을 참조하세요. Visual Studio가 구성 파일을 `MakePri.exe`에 전달합니다. `priconfig.default.xml` 파일의 내용이 해당 구성 파일의 `<default>` 요소로 사용됩니다. 이는 기본값으로 적용될 한정자 값 설정을 지정하는 부분입니다. 따라서 `priconfig.default.xml`을 추가하고 편집하면 Visual Studio가 앱에 대해 생성하는 패키지 리소스 인덱스 파일의 내용에 영향이 미치며 해당 내용이 앱 패키지에 포함됩니다.

**참고** `<qualifier name="Language" ... />` 요소의 값을 변경하는 경우 언제나 해당 변경 사항을 앱의 기본 언어와 동기화해야 합니다. 이는 앱의 PRI 파일에서 인덱싱된 언어 리소스가 앱의 매니페스트 기본 언어와 일치하도록 하기 위함입니다. `<qualifier name="Language" ... />` 요소의 값은 `<ProjectFolder>\obj\<ReleaseConfiguration folder>\priconfig.xml` 내용과 관련하여 매니페스트의 값을 덮어쓰지만, 해당 파일 및 앱의 매니페스트는 일치해야 합니다.

### <a name="using-a-different-file-name-than-priconfigdefaultxml"></a>`priconfig.default.xml`이 아닌 다른 파일 이름 사용

파일의 이름을 `priconfig.default.xml`로 지정하면, Visual Studio가 이를 인식하고 자동으로 사용합니다. 다른 이름으로 지정하려면, Visual Studio에서 이를 알 수 있도록 해야 합니다. 프로젝트 파일에서 첫 번째 `<PropertyGroup>` 요소의 여는 태그와 닫는 태그 사이에 이 XML을 추가합니다.

```xml
<AppxPriConfigXmlDefaultSnippetPath>FILE-PATH-AND-NAME</AppxPriConfigXmlDefaultSnippetPath>
```

`FILE-PATH-AND-NAME`을 파일의 경로 및 이름으로 바꿉니다.

## <a name="option-2-use-your-project-file-to-specify-default-qualifier-values"></a>옵션 2. 프로젝트 파일을 사용하여 기본 한정자 값 지정

이 방법은 옵션 1에 대한 대안입니다. 옵션 1의 작동 방식을 이해했으면 옵션 2가 개발 방식에 적합하고 워크플로를 더 잘 빌드하는 경우 옵션 2를 대신 선택해도 됩니다.

프로젝트 파일에서 첫 번째 `<PropertyGroup>` 요소의 여는 태그와 닫는 태그 사이에 이 XML을 추가합니다.

```xml
<AppxDefaultResourceQualifiers>Language=LANGUAGE-TAG(S)|Contrast=standard|Scale=200|HomeRegion=001|TargetSize=256|LayoutDirection=LTR|DXFeatureLevel=DX9|Configuration=|AlternateForm=</AppxDefaultResourceQualifiers>
```

다음은 처음 세 값을 편집한 후 파일의 모습을 보여주는 예입니다.

```xml
<AppxDefaultResourceQualifiers>Language=de-DE|Contrast=black|Scale=400|HomeRegion=001|TargetSize=256|LayoutDirection=LTR|DXFeatureLevel=DX9|Configuration=|AlternateForm=</AppxDefaultResourceQualifiers>
```

프로젝트를 저장하고 닫은 다음, 다시 빌드합니다.

**참고** `Language=`의 값을 변경하는 경우 언제나 `Package.appxmanifest`를 열어 매니페스트 디자이너에 있는 앱의 기본 언어와 해당 변경 사항을 동기화해야 합니다.

## <a name="related-topics"></a>관련 항목

* [언어, 규모, 고대비 및 기타 한정자에 맞게 리소스 조정](tailor-resources-lang-scale-contrast.md)
* [BCP-47 언어 태그](http://go.microsoft.com/fwlink/p/?linkid=227302)
* [MakePri.exe를 사용하여 수동으로 리소스 컴파일](compile-resources-manually-with-makepri.md)
