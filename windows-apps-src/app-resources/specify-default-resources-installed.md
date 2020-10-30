---
description: 앱에 고객 디바이스의 특정 설정과 일치하는 리소스가 없으면 앱의 기본 리소스가 사용됩니다. 이 문서에서는 이러한 기본 리소스를 지정하는 방법에 대해 설명합니다.
title: 앱에서 사용하는 기본 리소스 지정
template: detail.hbs
ms.date: 11/14/2017
ms.topic: article
keywords: Windows 10, UWP, 리소스, 이미지, 자산, MRT, 한정자
ms.localizationpriority: medium
ms.openlocfilehash: 4db2fbce788bac38a0f3a54a108f91c8293de6ba
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031555"
---
# <a name="specify-the-default-resources-that-your-app-uses"></a>앱에서 사용하는 기본 리소스 지정

앱에 고객 디바이스의 특정 설정과 일치하는 리소스가 없으면 앱의 기본 리소스가 사용됩니다. 이 문서에서는 이러한 기본 리소스를 지정하는 방법에 대해 설명합니다.

고객이 Microsoft Store에서 앱을 설치 하면 고객 장치의 설정이 앱의 사용 가능한 리소스와 일치 하 게 됩니다. 이러한 일치는 해당 사용자에 대 한 적절 한 리소스만 다운로드 하 여 설치 해야 합니다. 예를 들어 사용자의 언어 기본 설정에 대 한 가장 적절 한 문자열과 이미지 및 장치의 해상도 및 DPI 설정이 사용 됩니다. 예를 들어 `200` 는의 기본값 이지만 원하는 `scale` 경우이 기본값을 재정의할 수 있습니다.

자신의 리소스 팩으로 이동 하지 않는 리소스 (예: 고대비 설정에 맞게 조정 된 이미지)의 경우에도 사용자의 설정과 일치 하는 리소스를 찾을 수 없는 경우 런타임에 앱에서 사용 해야 하는 기본 리소스를 지정할 수 있습니다. 예를 들어 `standard` 는의 기본값 이지만 원하는 `contrast` 경우이 기본값을 재정의할 수 있습니다.

이러한 기본값은 기본 리소스 한정자 값 형식으로 지정 됩니다. 리소스 한정자의 용도, 용도 및 용도에 대 한 설명은 [언어, 크기 조정, 고대비 및 기타 한정자에 대 한 리소스](tailor-resources-lang-scale-contrast.md)조정을 참조 하세요.

두 가지 방법 중 하나로 이러한 기본값을 구성할 수 있습니다. 프로젝트에 구성 파일을 추가 하거나 프로젝트 파일을 직접 편집할 수 있습니다. 가장 편한 옵션 또는 빌드 시스템에 가장 적합 한 옵션을 사용 합니다.

## <a name="option-1-use-priconfigdefaultxml-to-specify-default-qualifier-values"></a>옵션 1. priconfig.default.xml를 사용 하 여 기본 한정자 값 지정

1. Visual Studio에서 프로젝트에 새 항목을 추가 합니다. XML 파일을 선택 하 고 파일의 이름을로 `priconfig.default.xml` 합니다.
2. 솔루션 탐색기에서 속성 창를 선택 `priconfig.default.xml` 하 고 확인 합니다. 파일의 빌드 작업을 None으로 설정 하 고 출력 디렉터리로 복사를 복사 안 함으로 설정 해야 합니다.
3. 파일의 내용을이 XML로 바꿉니다.
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
   
   **참고** 값은 `LANGUAGE-TAG(S)` 앱의 기본 언어와 동기화 해야 합니다. 단일 [BCP-47 언어 태그](https://tools.ietf.org/html/bcp47)이면 앱의 기본 언어가 동일한 태그 여야 합니다. 쉼표로 구분 된 언어 태그 목록이 면 앱의 기본 언어가 목록의 첫 번째 태그 여야 합니다. 앱 패키지 매니페스트 소스 파일 ()의 **응용 프로그램** 탭에 있는 **기본 언어** 필드에서 앱의 기본 언어를 설정 `Package.appxmanifest` 합니다.

4. 각 `<qualifier>` 요소는 각 한정자 이름에 대해 기본값으로 사용할 값을 Visual Studio에 알립니다. 지금까지 파일 콘텐츠를 사용 하 여 Visual Studio의 동작을 실제로 변경 하지 않았습니다. 즉, Visual Studio는이 파일이 기본 기본값 이기 때문에 이러한 내용과 함께 존재 하는 *것 처럼 이미 동작* 했습니다. 따라서 기본값을 고유 값으로 재정의 하려면 파일의 값을 변경 해야 합니다. 처음 3 개의 값을 편집한 경우 파일이 표시 되는 방법의 예는 다음과 같습니다.
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
5. 파일을 저장 하 고 닫은 다음 프로젝트를 다시 빌드합니다.

재정의 된 기본값을 고려 하 고 있는지 확인 하려면 파일을 찾아 `<ProjectFolder>\obj\<ReleaseConfiguration folder>\priconfig.xml` 해당 내용이 재정의와 일치 하는지 확인 합니다. 이렇게 하면 앱에서 기본적으로 사용 하는 리소스의 한정자 값이 성공적으로 구성 된 것입니다. 사용자의 설정에 대 한 일치 항목을 찾을 수 없는 경우 해당 폴더 또는 파일 이름에 여기서 설정한 기본 qualifer 값이 포함 된 리소스가 사용 됩니다.

### <a name="how-does-this-work"></a>어떻게 작동하나요?

백그라운드에서 Visual Studio는 `MakePri.exe` 기본 리소스를 나타내는 것을 비롯 하 여 모든 앱의 리소스를 설명 하는 PRI (패키지 리소스 인덱스) 라고 하는 파일을 생성 하기 위해 라는 도구를 시작 합니다. 이 도구에 대 한 자세한 내용은 [MakePri.exe를 사용 하 여 수동으로 리소스 컴파일 ](compile-resources-manually-with-makepri.md)을 참조 하세요. Visual Studio는에 구성 파일을 전달 `MakePri.exe` 합니다. 파일의 내용은 `priconfig.default.xml` `<default>` 기본값으로 간주 되는 한정자 값 집합을 지정 하는 부분인 해당 구성 파일의 요소로 사용 됩니다. 따라서를 추가 및 편집 하면 `priconfig.default.xml` Visual Studio에서 앱에 대해 생성 하 고 해당 앱 패키지에 포함 하는 패키지 리소스 인덱스 파일의 내용에 영향을 미칩니다.

**참고** 요소의 값을 변경할 때마다 `<qualifier name="Language" ... />` 해당 변경 내용을 앱의 기본 언어와 동기화 해야 합니다. 이는 앱의 PRI 파일에 인덱싱된 언어 리소스가 앱의 매니페스트 기본 언어와 일치 하도록 하는 것입니다. 요소의 값은 `<qualifier name="Language" ... />` 의 내용과 관련 하 여 매니페스트의 값을 재정의 `<ProjectFolder>\obj\<ReleaseConfiguration folder>\priconfig.xml` 하지만 해당 파일과 응용 프로그램의 매니페스트가 일치 해야 합니다.

### <a name="using-a-different-file-name-than-priconfigdefaultxml"></a>다른 파일 이름 사용 `priconfig.default.xml`

파일의 이름을 `priconfig.default.xml` 로 지정한 경우 Visual Studio에서 해당 파일을 인식 하 여 자동으로 사용 합니다. 다른 이름을 지정 하는 경우 Visual Studio에서 알 수 있도록 해야 합니다. 프로젝트 파일에서 첫 번째 요소의 여는 태그와 닫는 태그 사이에 `<PropertyGroup>` 이 XML을 추가 합니다.

```xml
<AppxPriConfigXmlDefaultSnippetPath>FILE-PATH-AND-NAME</AppxPriConfigXmlDefaultSnippetPath>
```

`FILE-PATH-AND-NAME`파일의 경로 및 이름으로 대체 합니다.

## <a name="option-2-use-your-project-file-to-specify-default-qualifier-values"></a>옵션 2. 프로젝트 파일을 사용 하 여 기본 한정자 값 지정

이는 옵션 1에 대 한 대안입니다. 옵션 1의 작동 방식을 이해 하면 개발 및/또는 빌드 워크플로에 적합 한 경우 옵션 2를 대신 선택할 수 있습니다.

프로젝트 파일에서 첫 번째 요소의 여는 태그와 닫는 태그 사이에 `<PropertyGroup>` 이 XML을 추가 합니다.

```xml
<AppxDefaultResourceQualifiers>Language=LANGUAGE-TAG(S)|Contrast=standard|Scale=200|HomeRegion=001|TargetSize=256|LayoutDirection=LTR|DXFeatureLevel=DX9|Configuration=|AlternateForm=</AppxDefaultResourceQualifiers>
```

처음 세 값을 편집한 후에 표시 되는 방법의 예는 다음과 같습니다.

```xml
<AppxDefaultResourceQualifiers>Language=de-DE|Contrast=black|Scale=400|HomeRegion=001|TargetSize=256|LayoutDirection=LTR|DXFeatureLevel=DX9|Configuration=|AlternateForm=</AppxDefaultResourceQualifiers>
```

프로젝트를 저장 하 고 닫은 후 다시 빌드합니다.

**참고** `Language=` 값을 변경할 때마다 매니페스트 디자이너에서 해당 변경 내용을 응용 프로그램의 기본 언어 (열기)와 동기화 해야 `Package.appxmanifest` 합니다.

## <a name="related-topics"></a>관련 항목

* [언어, 배율, 고대비 및 기타 한정자에 맞게 리소스 조정](tailor-resources-lang-scale-contrast.md)
* [BCP-47 언어 태그](https://tools.ietf.org/html/bcp47)
* [MakePri.exe를 사용하여 수동으로 리소스 컴파일](compile-resources-manually-with-makepri.md)
