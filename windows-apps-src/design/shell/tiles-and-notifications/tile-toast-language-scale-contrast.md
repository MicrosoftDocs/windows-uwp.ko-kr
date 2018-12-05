---
Description: Your tiles and toasts can load strings and images tailored for display language, display scale factor, high contrast, and other runtime contexts.
title: 언어, 배율, 고대비에 대한 타일 및 알림 메시지
template: detail.hbs
ms.date: 10/12/2017
ms.topic: article
keywords: Windows 10, uwp, 리소스, 이미지, 자산, MRT, 한정자
ms.localizationpriority: medium
ms.openlocfilehash: aa6e93196d30c15374129eee7714604cfab7b82e
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8739925"
---
# <a name="tile-and-toast-notification-support-for-language-scale-and-high-contrast"></a>언어, 배율, 고대비에 대한 타일 및 알림 메시지

타일 및 알림은 표시 언어, [디스플레이 배율 인수](../../layout/screen-sizes-and-breakpoints-for-responsive-design.md), 고대비 및 기타 런타임 컨텍스트에 맞게 조정된 문자열 및 이미지를 로드할 수 있습니다. 리소스 파일 이름에 한정자를 사용 하는 방법에 대 한 배경, [언어, 규모 및 기타 한정자에 대 한 리소스에 맞게 조정](../../../app-resources/tailor-resources-lang-scale-contrast.md) 하 고 [앱 아이콘 및 로고를](/windows/uwp/design/style/app-icons-and-logos)참조 하세요.

앱 지역화의 가치 제안에 대한 자세한 내용은 [세계화 및 지역화](../../globalizing/globalizing-portal.md)를 참조하세요.

## <a name="refer-to-a-string-resource-from-a-template"></a>템플릿에서 문자열 리소스 식별자 참조

타일 또는 알림 템플릿에서 `ms-resource` URI(Uniform Resource Identifier) 스키마 다음에 단순한 문자열 리소스 식별자를 사용하여 문자열 리소스를 참조할 수 있습니다. 예를 들어, 이름이 "Farewell"인 리소스 항목을 포함하는 Resources.resx 파일이 있는 "Farewell" 식별자가 있는 문자열 리소스를 가집니다. 문자열 리소스 식별자 및 리소스 파일(.resw)의 예와 자세한 내용은 [UI와 앱 패키지 매니페스트에 문자열 지역화](../../../app-resources/localize-strings-ui-manifest.md)를 참조하세요.

"Farewell" 문자열 리소스 식별자에 대한 참조가 `ms-resource`를 사용하여 템플릿 콘텐츠의 [텍스트](/uwp/schemas/tiles/tilesschema/element-text?branch=live) 본문에 나타나는 모습은 다음과 같습니다.

```xml
<text id="1">ms-resource:Farewell</text>
```

`ms-resource`URI 스키마를 생략하는 경우 텍스트 본문은 단지 문자열 리터럴이며 식별자에 대한 참조가 *아닙니다*.

```xml
<text id="1">Farewell</text>
```

## <a name="refer-to-an-image-resource-from-a-template"></a>템플릿에서 이미지 리소스 식별자 참조

타일 또는 알림 템플릿에서 `ms-appx` URI(Uniform Resource Identifier) 스키마 다음에 이미지 리소스 이름을 사용하여 이미지 리소스를 참조할 수 있습니다. 이는 XAML 태그의 이미지 리소스를 참조하는 것과 동일한 방식입니다(자세한 내용은 [XAML 태그와 코드에서 이미지 또는 다른 자산 참조](../../../app-resources/images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code) 참조).

예를 들어 다음과 같이 폴더 이름을 지정할 수 있습니다.

```
\Assets\Images\contrast-standard\welcome.png
\Assets\Images\contrast-high\welcome.png
```

이 경우 단일 이미지 리소스 있으며 이름(절대 경로로서)은 `/Assets/Images/welcome.png`입니다. 다음은 템플릿에서 해당 이름을 사용하는 방법입니다.

```xml
<image id="1" src="ms-appx:///Assets/Images/welcome.png"/>
```

이 예 URI에서 스키마("`ms-appx`") 다음에 "`://`", 그 뒤에 절대 경로(절대 경로는 "`/`"로 시작)가 나오는 것을 확인하세요.

## <a name="hosting-and-loading-images-in-the-cloud"></a>클라우드에 이미지 호스팅 및 로드

`ms-resource`및 `ms-appx` URI 스키마는 자동 한정자 일치를 수행하여 현재 컨택스트에 가장 적합한 리소스를 찾을 수 있습니다. 웹 URI 스키마(`http`, `https`, `ftp` 등)는 그러한 자동 일치를 전혀 수행하지 않습니다.

대신, 이미지의 URI에 요청된 한정자 값을 설명하는 쿼리 문자열을 추가합니다.

```xml
<image id="1" src="http://www.contoso.com/Assets/Images/welcome.png?ms-lang=en-US"/>
```

그 다음 이미지를 제공하는 앱 서비스에서 반환할 이미지를 결정하는 쿼리 문자열을 검사하고 사용하는 HTTP 처리기를 구현합니다.

[타일](/uwp/schemas/tiles/tilesschema/schema-root?branch=live) 또는 [알림](/uwp/schemas/tiles/toastschema/schema-root?branch=live) 메시지 XML 페이로드에서 [**addImageQuery**](/uwp/schemas/tiles/tilesschema/element-visual?branch=live) 특성을 `true`로 설정해야 할 수도 있습니다. **addImageQuery** 특성은 타일 및 알림 스키마 둘 다의 `visual`, `binding`, 및 `image` 요소에 나타납니다. 명시적으로 요소의 **addImageQuery** 설정은 상위 항목에 대한 설정 모든 값을 재정의합니다. 예를 들어 `image` 요소에서 `true`인 **addImageQuery** 값은 상위 `binding` 요소에서 `false`인 **addImageQuery**를 재정의합니다.

이들은 사용할 수 있는 쿼리 문자열입니다.

| 한정자 | 쿼리 문자열 | 예 |
| --------- | ------------ | ------- |
| 배율 | ms-scale | ?ms-scale=400 |
| 언어 | ms-lang | ?ms-lang=en-US |
| 대비 | ms-contrast | ?ms-contrast=high |

쿼리 문자열에 사용할 수 있는 모든 한정자 값에 대한 참조 테이블은 [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)를 참조하세요.

## <a name="important-apis"></a>중요 API

* [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)

## <a name="related-topics"></a>관련 항목

* [반응형 디자인에 대한 화면 크기 및 중단점](../../layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [언어, 규모 및 기타 한정자에 맞게 리소스 조정](../../../app-resources/tailor-resources-lang-scale-contrast.md)
* [타일 및 아이콘 자산에 대한 지침](app-assets.md).
* [세계화 및 지역화](../../globalizing/globalizing-portal.md)
* [UI와 앱 패키지 매니페스트에 문자열 지역화](../../../app-resources/localize-strings-ui-manifest.md)
* [XAML 태그와 코드에서 이미지 또는 다른 자산 참조](../../../app-resources/images-tailored-for-scale-theme-contrast.md)
* [addImageQuery](/uwp/schemas/tiles/tilesschema/element-visual?branch=live)
* [타일 스키마](/uwp/schemas/tiles/tilesschema/schema-root?branch=live)
* [알림 스키마](/uwp/schemas/tiles/toastschema/schema-root?branch=live)
