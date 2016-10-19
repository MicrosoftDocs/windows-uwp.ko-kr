---
author: mijacobs
Description: "다음은 적응형 타일을 만드는 데 사용되는 요소 및 특성입니다."
title: "적응형 타일 스키마 및 템플릿"
ms.assetid: 858FB05E-87A2-49CF-BE48-570980AD36C8
label: Adaptive tile schema and templates
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: 08bdb46dba6fc93ada20b3fc585d3e24e29023a0

---
# 적응형 타일 템플릿: 스키마 및 지침

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

다음은 적응형 타일을 만드는 데 사용되는 요소 및 특성입니다. 지침과 예제는 [적응형 타일 만들기](tiles-and-notifications-create-adaptive-tiles.md)를 참조하세요.

## 타일 요소


``` xml
<tile>
  
  <!-- Child elements -->
  visual
  
</tile>
```

## 시각적 요소


``` xml
<visual
  version? = integer
  lang? = string
  baseUri? = anyURI
  branding? = "none" | "logo" | "name" | "nameAndLogo"
  addImageQuery? = boolean
  contentId? = string
  displayName? = string >
    
  <!-- Child elements -->
  binding+

</visual>
```

## 바인딩 요소


``` xml
<binding
  template = tileTemplateNameV3
  fallback? = tileTemplateNameV1
  lang? = string
  baseUri? = anyURI
  branding? = "none" | "logo" | "name" | "nameAndLogo"
  addImageQuery? = boolean
  contentId? = string
  displayName? = string
  hint-textStacking? = "top" | "center" | "bottom"
  hint-overlay? = [0-100] >

  <!-- Child elements -->
  ( image
  | text
  | group
  )*

</binding>
```

## 이미지 요소


``` xml
<image
  src = string
  placement? = "inline" | "background" | "peek"
  alt? = string
  addImageQuery? = boolean
  hint-crop? = "none" | "circle"
  hint-removeMargin? = boolean
  hint-align? = "stretch" | "left" | "center" | "right" />
```

## 텍스트 요소


``` xml
<text
  lang? = string
  hint-style? = textStyle
  hint-wrap? = boolean
  hint-maxLines? = integer
  hint-minLines? = integer
  hint-align? = "left" | "center" | "right" >

  <!-- text goes here -->

</text>
```

textStyle 값: caption captionSubtle body bodySubtle base baseSubtle subtitle subtitleSubtle title titleSubtle titleNumeral subheader subheaderSubtle subheaderNumeral header headerSubtle headerNumber

## 그룹 요소


``` xml
<group>

  <!-- Child elements -->
  subgroup+

</group>
```

## 하위 그룹 요소


``` xml
<subgroup
  hint-weight? = [0-100]
  hint-textStacking? = "top" | "center" | "bottom" >

  <!-- Child elements -->
  ( text
  | image
  )*

</subgroup>
```

## 관련 항목


* [적응형 타일 만들기](tiles-and-notifications-create-adaptive-tiles.md)
 

 







<!--HONumber=Aug16_HO3-->


