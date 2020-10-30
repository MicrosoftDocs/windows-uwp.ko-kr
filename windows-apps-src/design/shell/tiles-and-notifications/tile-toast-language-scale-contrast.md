---
description: 타일과 알림을는 표시 언어, 표시 배율 비율, 고대비 및 기타 런타임 컨텍스트에 맞게 조정 된 문자열과 이미지를 로드할 수 있습니다.
title: 언어, 규모 및 고대비에 대 한 타일 및 알림 알림 지원
template: detail.hbs
ms.date: 10/12/2017
ms.topic: article
keywords: Windows 10, UWP, 리소스, 이미지, 자산, MRT, 한정자
ms.localizationpriority: medium
ms.openlocfilehash: 0048da25cd1e775391c2523e37cb936243b7308c
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033116"
---
# <a name="tile-and-toast-notification-support-for-language-scale-and-high-contrast"></a>언어, 규모 및 고대비에 대 한 타일 및 알림 알림 지원

타일과 알림을는 표시 언어, [표시 배율 비율](../../layout/screen-sizes-and-breakpoints-for-responsive-design.md), 고대비 및 기타 런타임 컨텍스트에 맞게 조정 된 문자열과 이미지를 로드할 수 있습니다. 리소스 파일 이름에 한정자를 사용 하는 방법에 대 한 자세한 내용은 [언어, 크기 조정 및 기타 한정자](../../../app-resources/tailor-resources-lang-scale-contrast.md) 와 [앱 아이콘 및 로고](../../style/app-icons-and-logos.md)에 대 한 리소스 조정을 참조 하세요.

앱 지역화의 가치 제안에 대한 자세한 내용은 [세계화 및 지역화](../../globalizing/globalizing-portal.md)를 참조하세요.

## <a name="refer-to-a-string-resource-from-a-template"></a>템플릿에서 문자열 리소스를 참조 하세요.

타일 또는 알림 템플릿에서 URI (Uniform Resource Identifier) 체계를 사용 하 여 문자열 리소스를 참조 한 `ms-resource` 다음 단순 문자열 리소스 식별자를 참조할 수 있습니다. 예를 들어 이름이 "인사" 인 리소스 항목을 포함 하는 리소스 .resx 파일이 있는 경우 "인사" 식별자를 포함 하는 문자열 리소스가 있습니다. 문자열 리소스 식별자 및 리소스 파일 (. resw)에 대 한 자세한 내용은 [UI 및 앱 패키지 매니페스트에서 문자열 지역화](../../../app-resources/localize-strings-ui-manifest.md)를 참조 하세요.

이는를 사용 하 여 "인사" 문자열 리소스 식별자에 대 한 참조가 템플릿 콘텐츠의 [텍스트](/uwp/schemas/tiles/tilesschema/element-text?branch=live) 본문에서 확인 되는 방법입니다 `ms-resource` .

```xml
<text id="1">ms-resource:Farewell</text>
```

URI 체계를 생략 하면 `ms-resource` 텍스트 본문은 식별자에 대 한 참조가 *아니라* 문자열 리터럴이어야 합니다.

```xml
<text id="1">Farewell</text>
```

## <a name="refer-to-an-image-resource-from-a-template"></a>템플릿에서 이미지 리소스 참조

타일 또는 알림 템플릿에서 `ms-appx` URI (Uniform Resource identifier) 체계를 사용 하 고 이미지 리소스의 이름을 사용 하 여 이미지 리소스를 참조할 수 있습니다. 이는 XAML 태그에서 이미지 리소스를 참조 하는 방식과 동일 합니다. 자세한 내용은 [xaml 태그 및 코드에서 이미지 또는 기타 자산 참조](../../../app-resources/images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code)를 참조 하세요.

예를 들어 폴더 이름을 다음과 같이로 할 수 있습니다.

```
\Assets\Images\contrast-standard\welcome.png
\Assets\Images\contrast-high\welcome.png
```

이 경우 단일 이미지 리소스와 해당 이름 (절대 경로)이 있습니다 `/Assets/Images/welcome.png` . 템플릿에서 해당 이름을 사용 하는 방법은 다음과 같습니다.

```xml
<image id="1" src="ms-appx:///Assets/Images/welcome.png"/>
```

이 예제 URI에서 스키마 (" `ms-appx` ") 뒤에는 `://` 절대 경로 (절대 경로는 ""로 시작)가 오는 ""가와 야 `/` 합니다.

## <a name="hosting-and-loading-images-in-the-cloud"></a>클라우드에서 이미지 호스팅 및 로드

`ms-resource`및 `ms-appx` URI 체계는 자동 한정자 일치를 수행 하 여 현재 컨텍스트에 가장 적합 한 리소스를 찾습니다. 웹 URI 스키마 (예: `http` , `https` 및 `ftp` )는 이러한 자동 일치를 수행 하지 않습니다.

대신, 요청 된 한정자 값을 설명 하는 쿼리 문자열을 이미지의 URI에 추가 합니다.

```xml
<image id="1" src="http://www.contoso.com/Assets/Images/welcome.png?ms-lang=en-US"/>
```

그런 다음 이미지를 제공 하는 app service에서 쿼리 문자열을 검사 하 고 사용 하 여 반환할 이미지를 결정 하는 HTTP 처리기를 구현 합니다.

또한 [**addImageQuery**](/uwp/schemas/tiles/tilesschema/element-visual?branch=live) `true` [TILE](/uwp/schemas/tiles/tilesschema/schema-root?branch=live) 또는 [toast](/uwp/schemas/tiles/toastschema/schema-root?branch=live) notification XML 페이로드에서 addImageQuery 특성을로 설정 해야 합니다. **AddImageQuery** 특성은 `visual` `binding` `image` 타일 및 알림 스키마의, 및 요소에 표시 됩니다. 요소에 대해 명시적으로 **addImageQuery** 를 설정 하면 상위 항목에 설정 된 모든 값이 재정의 됩니다. 예를 들어, 요소의 **addImageQuery** 값은 `true` `image` 부모 요소의 **addImageQuery** 를 재정의 `false` `binding` 합니다.

사용할 수 있는 쿼리 문자열은 다음과 같습니다.

| 한정자 | 쿼리 문자열 | 예제 |
| --------- | ------------ | ------- |
| 확장 | 밀리초 단위 | ? 밀리초-소수 자릿수 = 400 |
| 언어 | ms-lang | ? ms-lang = en-us |
| 이 예와 | ms-대비 | ? 밀리초-대비 = 높음 |

쿼리 문자열에서 사용할 수 있는 모든 한정자 값의 참조 테이블은 [QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)를 참조 하세요.

## <a name="important-apis"></a>중요 API

* [ResourceContext](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)

## <a name="related-topics"></a>관련 항목

* [반응형 디자인에 대한 화면 크기 및 중단점](../../layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [언어, 크기 조정 및 기타 한정자에 맞게 리소스를 조정 합니다.](../../../app-resources/tailor-resources-lang-scale-contrast.md)
* [타일 및 아이콘 자산에 대 한 지침](../../style/app-icons-and-logos.md)입니다.
* [세계화 및 지역화](../../globalizing/globalizing-portal.md)
* [UI 및 앱 패키지 매니페스트의 문자열 지역화](../../../app-resources/localize-strings-ui-manifest.md)
* [XAML 태그 및 코드에서 이미지 또는 기타 자산 참조](../../../app-resources/images-tailored-for-scale-theme-contrast.md)
* [addImageQuery](/uwp/schemas/tiles/tilesschema/element-visual?branch=live)
* [타일 스키마](/uwp/schemas/tiles/tilesschema/schema-root?branch=live)
* [알림 스키마](/uwp/schemas/tiles/toastschema/schema-root?branch=live)
