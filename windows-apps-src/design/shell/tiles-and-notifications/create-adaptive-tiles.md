---
author: andrewleader
Description: Adaptive tile templates are a new feature in Windows 10, allowing you to design your own tile notification content using a simple and flexible markup language that adapts to different screen densities.
title: 적응형 타일 만들기
ms.assetid: 1246B58E-D6E3-48C7-AD7F-475D113600F9
label: Create adaptive tiles
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10 uwp
ms.localizationpriority: medium
ms.openlocfilehash: 761d87654ef340f4b539dbefa0950c58f627d310
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/22/2018
ms.locfileid: "5409547"
---
# <a name="create-adaptive-tiles"></a>적응형 타일 만들기

적응형 타일 템플릿은 다양한 화면 밀도에 맞게 조정되는 단순하고 유연한 태그 언어를 사용하여 고유한 타일 알림 콘텐츠를 디자인할 수 있도록 하는 Windows 10의 새로운 기능입니다. 이 문서에서는 UWP(유니버설 Windows 플랫폼) 앱의 적응형 라이브 타일을 만드는 방법을 설명합니다. 적응형 요소 및 특성의 전체 목록은 [적응형 타일 스키마](../tiles-and-notifications/tile-schema.md)를 참조하세요.

원할 경우 Windows 10용 알림을 디자인할 때 [Windows 8 타일 템플릿 카탈로그](https://msdn.microsoft.com/library/windows/apps/hh761491)에서 미리 설정된 템플릿을 계속 사용할 수 있습니다.


## <a name="getting-started"></a>시작

**알림 라이브러리 설치.** XML 대신 C#을 사용하여 알림을 생성하려면 [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)("notifications uwp" 검색)라는 NuGet 패키지를 설치합니다. 이 문서에서 제공하는 C# 샘플은 NuGet 패키지 버전 1.0.0을 사용합니다.

**알림 시각화 도우미 설치.** 이 무료 UWP 앱은 Visual Studio의 XAML 편집기/디자인 뷰와 비슷하게 타일을 편집할 때 시각적 미리 보기를 곧바로 제공하여 적응형 라이브 타일을 디자인하는 데 도움이 됩니다. 자세한 내용은 [알림 시각화 도우미](notifications-visualizer.md)를 참조하거나 [Microsoft Store에서 알림 시각화 도우미를 다운로드하세요](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1).


## <a name="how-to-send-a-tile-notification"></a>타일 알림을 보내는 방법

[로컬 타일 알림 보내기에 대한 빠른 시작](sending-a-local-tile-notification.md)을 참조하세요. 아래 문서에서는 적응형 타일에 사용할 수 있는 모든 시각적 UI를 설명합니다.


## <a name="usage-guidance"></a>사용법 지침


적응형 템플릿은 다양한 폼 팩터와 알림 유형에서 작동하도록 설계됩니다. 그룹 및 하위 그룹과 같은 요소는 콘텐츠를 연결하며 자체적으로 특정 시각적 동작을 의미하지 않습니다. 알림의 최종 모양은 휴대폰, 태블릿, 데스크톱 등 알림이 표시될 특정 디바이스에 따라 달라져야 합니다.

힌트는 특정 시각적 동작을 얻기 위해 요소에 추가할 수 있는 선택적 특성입니다. 힌트는 디바이스별 또는 알림별로 다를 수 있습니다.

## <a name="a-basic-example"></a>기본 예제


이 예제에서는 적응형 타일 템플릿으로 무엇을 만들 수 있는지 보여 줍니다.

```xml
<tile>
  <visual>
  
    <binding template="TileMedium">
      ...
    </binding>
  
    <binding template="TileWide">
      <text hint-style="subtitle">Jennifer Parker</text>
      <text hint-style="captionSubtle">Photos from our trip</text>
      <text hint-style="captionSubtle">Check out these awesome photos I took while in New Zealand!</text>
    </binding>
  
    <binding template="TileLarge">
      ...
    </binding>
  
  </visual>
</tile>
```

```csharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileMedium = ...
  
        TileWide = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText()
                    {
                        Text = "Jennifer Parker",
                        HintStyle = AdaptiveTextStyle.Subtitle
                    },
  
                    new AdaptiveText()
                    {
                        Text = "Photos from our trip",
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    },
  
                    new AdaptiveText()
                    {
                        Text = "Check out these awesome photos I took while in New Zealand!",
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    }
                }
            }
        },
  
        TileLarge = ...
    }
};
```

**결과:**

![빠른 샘플 타일](images/adaptive-tiles-quicksample.png)

## <a name="tile-sizes"></a>타일 크기


각 타일 크기에 대한 콘텐츠는 XML 페이로드 내에서 별도의 [TileBinding](../tiles-and-notifications/tile-schema.md#tilebinding) 요소에 개별적으로 지정합니다. 템플릿 특성을 다음 값 중 하나로 설정하여 대상 크기를 선택합니다.

-   TileSmall
-   TileMedium
-   TileWide
-   TileLarge(데스크톱에만 해당)

단일 타일 알림 XML 페이로드에서 이 예제에 나온 대로 지원하려는 각 타일 크기에 대한 &lt;binding&gt; 요소를 제공합니다.

```xml
<tile>
  <visual>
  
    <binding template="TileSmall">
      <text>Small</text>
    </binding>
  
    <binding template="TileMedium">
      <text>Medium</text>
    </binding>
  
    <binding template="TileWide">
      <text>Wide</text>
    </binding>
  
    <binding template="TileLarge">
      <text>Large</text>
    </binding>
  
  </visual>
</tile>
```

```csharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileSmall = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText() { Text = "Small" }
                }
            }
        },
  
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText() { Text = "Medium" }
                }
            }
        },
  
        TileWide = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText() { Text = "Wide" }
                }
            }
        },
  
        TileLarge = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText() { Text = "Large" }
                }
            }
        }
    }
};
```

**결과:**

![적응형 타일 크기: 작음, 중간, 넓음 및 큼](images/adaptive-tiles-sizes.png)

## <a name="branding"></a>브랜딩


알림 페이로드에서 브랜딩 특성을 사용하여 라이브 타일 아래쪽의 브랜딩(표시 이름 및 모서리 로고)을 제어할 수 있습니다. "none," "name"만, "logo"만 또는 "nameAndLogo"를 사용하여 둘 다 표시하도록 선택할 수 있습니다.

**참고**  Windows Mobile에서는 모서리 로고를 지원하지 않으므로 Mobile에서 "logo" 및 "nameAndLogo"는 기본적으로 "name"으로 지정됩니다.

 

```xml
<visual branding="logo">
  ...
</visual>
```

```csharp
new TileVisual()
{
    Branding = TileBranding.Logo,
    ...
}
```

**결과:**

![적응형 타일, 이름 및 로고](images/adaptive-tiles-namelogo.png)

특정 타일 크기에 대한 브랜딩은 다음 두 가지 방법 중 하나로 적용할 수 있습니다.

1. [TileBinding](../tiles-and-notifications/tile-schema.md#tilebinding) 요소에 특성 적용
2. 전체 알림 페이로드에 영향을 주는 [TileVisual](../tiles-and-notifications/tile-schema.md#tilevisual) 요소에 특성 적용. 바인딩에 대한 브랜딩을 지정하지 않으면 시각적 요소에 제공된 브랜딩이 사용됩니다.

```xml
<tile>
  <visual branding="nameAndLogo">
 
    <binding template="TileMedium" branding="logo">
      ...
    </binding>
 
    <!--Inherits branding from visual-->
    <binding template="TileWide">
      ...
    </binding>
 
  </visual>
</tile>
```

```csharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        Branding = TileBranding.NameAndLogo,

        TileMedium = new TileBinding()
        {
            Branding = TileBranding.Logo,
            ...
        },

        // Inherits branding from Visual
        TileWide = new TileBinding()
        {
            ...
        }
    }
};
```

**기본 브랜딩 결과:**

![타일의 기본 브랜딩](images/adaptive-tiles-defaultbranding.png)

알림 페이로드에서 브랜딩을 지정하지 않으면 기본 타일의 속성에 따라 브랜딩이 결정됩니다. 기본 타일에 표시 이름이 표시되는 경우 브랜딩의 기본값은 "name"입니다. 그렇지 않고 표시 이름이 표시되지 않는 경우 브랜딩의 기본값은 "none"입니다.

**참고**  이는 기본 브랜딩이 "logo"이던 Windows 8.x에서 변경된 내용입니다.

 

## <a name="display-name"></a>표시 이름


**displayName** 특성에 원하는 텍스트 문자열을 입력하여 알림의 표시 이름을 재정의할 수 있습니다. 브랜딩과 마찬가지로 표시 이름은 전체 알림 페이로드에 적용되는 [TileVisual](../tiles-and-notifications/tile-schema.md#tilevisual) 요소나 개별 타일에만 적용되는 [TileBinding](../tiles-and-notifications/tile-schema.md#tilebinding) 요소에 지정할 수 있습니다.

**알려진 문제**  Windows Mobile에서 타일의 ShortName을 지정하면 알림에 제공된 표시 이름이 사용되지 않습니다(항상 ShortName이 표시됨). 

```xml
<tile>
  <visual branding="nameAndLogo" displayName="Wednesday 22">
 
    <binding template="TileMedium" displayName="Wed. 22">
      ...
    </binding>
 
    <!--Inherits displayName from visual-->
    <binding template="TileWide">
      ...
    </binding>
 
  </visual>
</tile>
```

```csharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        Branding = TileBranding.NameAndLogo,
        DisplayName = "Wednesday 22",

        TileMedium = new TileBinding()
        {
            DisplayName = "Wed. 22",
            ...
        },

        // Inherits DisplayName from Visual
        TileWide = new TileBinding()
        {
            ...
        }
    }
};
```

**결과:**

![적응형 타일 표시 이름](images/adaptive-tiles-displayname.png)

## <a name="text"></a>텍스트


[AdaptiveText](../tiles-and-notifications/tile-schema.md#adaptivetext) 요소는 텍스트를 표시하는 데 사용됩니다. 힌트를 사용하여 텍스트가 표시되는 방식을 수정할 수 있습니다.

```xml
<text>This is a line of text</text>
```


```csharp
new AdaptiveText()
{
    Text = "This is a line of text"
};
```

**결과:**

![적응형 타일 텍스트](images/adaptive-tiles-text.png)

## <a name="text-wrapping"></a>텍스트 배치


기본적으로 텍스트는 줄 바꿈되지 않고 타일의 가장자리를 벗어나 계속됩니다. **hint-wrap** 특성을 사용하여 텍스트 요소의 텍스트 배치를 설정할 수 있습니다. 또한 둘 다 양의 정수를 사용할 수 있는 **hint-minLines** 및 **hint-maxLines**를 사용하여 최대 및 최소 줄 수를 제어할 수도 있습니다.

```xml
<text hint-wrap="true">This is a line of wrapping text</text>
```


```csharp
new AdaptiveText()
{
    Text = "This is a line of wrapping text",
    HintWrap = true
};
```

**결과:**

![텍스트 배치를 적용한 적응형 타일](images/adaptive-tiles-textwrapping.png)

## <a name="text-styles"></a>텍스트 스타일


스타일은 텍스트 요소의 글꼴 크기, 색 및 두께를 제어합니다. 불투명도 60%로 설정하여 일반적으로 텍스트 색을 밝은 회색의 음영으로 만드는 각 스타일의 "미묘한" 변형을 비롯하여 다양한 스타일이 제공됩니다.

```xml
<text hint-style="base">Header content</text>
<text hint-style="captionSubtle">Subheader content</text>
```

```csharp
new AdaptiveText()
{
    Text = "Header content",
    HintStyle = AdaptiveTextStyle.Base
},

new AdaptiveText()
{
    Text = "Subheader content",
    HintStyle = AdaptiveTextStyle.CaptionSubtle
}
```

**결과:**

![적응형 타일 텍스트 스타일](images/adaptive-tiles-textstyles.png)

**참고**  hint-style을 지정하지 않으면 스타일의 기본값은 caption입니다.

 

**기본 텍스트 스타일**

|                                |                           |             |
|--------------------------------|---------------------------|-------------|
| &lt;text hint-style = "\ *" /&gt; | 글꼴 높이               | 글꼴 두께 |
| 자막                        | 12epx(유효 픽셀) | Regular     |
| 본문                           | 15epx                    | Regular     |
| 하단                           | 15epx                    | Semibold    |
| 부제목                       | 20epx                    | Regular     |
| 제목                          | 24epx                    | Semilight   |
| 하위 머리글                      | 34epx                    | Light       |
| 머리글                         | 46epx                    | Light       |

 

**숫자 텍스트 스타일 변형**

다음 변형은 줄 높이를 줄여 위아래 콘텐츠가 텍스트와 더 가까워지게 합니다.

|                  |
|------------------|
| titleNumeral     |
| subheaderNumeral |
| headerNumeral    |

 

**미묘한 텍스트 스타일 변형**

각 스타일에는 일반적으로 텍스트 색을 밝은 회색의 음영으로 만드는 60% 불투명도를 텍스트에 부여하는 미묘한 변형이 있습니다.

|                        |
|------------------------|
| captionSubtle          |
| bodySubtle             |
| baseSubtle             |
| subtitleSubtle         |
| titleSubtle            |
| titleNumeralSubtle     |
| subheaderSubtle        |
| subheaderNumeralSubtle |
| headerSubtle           |
| headerNumeralSubtle    |

 

## <a name="text-alignment"></a>텍스트 맞춤


텍스트를 가로로 왼쪽, 가운데 또는 오른쪽에 맞출 수 있습니다. 영어와 같이 왼쪽에서 오른쪽으로 쓰는 언어에서는 텍스트가 기본적으로 왼쪽에 맞춰집니다. 아랍어와 같이 오른쪽에서 왼쪽으로 쓰는 언어에서는 텍스트가 기본적으로 오른쪽에 맞춰집니다. 요소에서 **hint-align** 특성을 사용하여 맞춤을 수동으로 설정할 수 있습니다.

```xml
<text hint-align="center">Hello</text>
```


```csharp
new AdaptiveText()
{
    Text = "Hello",
    HintAlign = AdaptiveTextAlign.Center
};
```

**결과:**

![적응형 타일 텍스트 맞춤](images/adaptive-tiles-textalignment.png)

## <a name="groups-and-subgroups"></a>그룹 및 하위 그룹


그룹을 사용하면 그룹 내의 콘텐츠가 관련이 있고 콘텐츠 전체를 표시해야만 이해될 수 있다고 의미상 선언할 수 있습니다. 예를 들어 머리글과 하위 머리글의 두 텍스트 요소가 있을 경우 머리글만 표시하면 의미가 통하지 않을 수 있습니다. 이러한 요소를 하위 그룹 내로 그룹화하면 요소는 모두 표시되거나(맞을 경우) 전혀 표시되지 않습니다(맞지 않을 경우).

디바이스 및 화면 전체에서 최상의 환경을 제공하려면 여러 그룹을 제공합니다. 여러 그룹이 있으면 타일이 큰 화면에 맞게 조정될 수 있습니다.

**참고**  그룹의 유효한 하위 요소는 하위 그룹뿐입니다.

 

```xml
<binding template="TileWide" branding="nameAndLogo">
  <group>
    <subgroup>
      <text hint-style="subtitle">Jennifer Parker</text>
      <text hint-style="captionSubtle">Photos from our trip</text>
      <text hint-style="captionSubtle">Check out these awesome photos I took while in New Zealand!</text>
    </subgroup>
  </group>
 
  <text />
 
  <group>
    <subgroup>
      <text hint-style="subtitle">Steve Bosniak</text>
      <text hint-style="captionSubtle">Build 2015 Dinner</text>
      <text hint-style="captionSubtle">Want to go out for dinner after Build tonight?</text>
    </subgroup>
  </group>
</binding>
```

```csharp
TileWide = new TileBinding()
{
    Branding = TileBranding.NameAndLogo,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            CreateGroup(
                from: "Jennifer Parker",
                subject: "Photos from our trip",
                body: "Check out these awesome photos I took while in New Zealand!"),

            // For spacing
            new AdaptiveText(),

            CreateGroup(
                from: "Steve Bosniak",
                subject: "Build 2015 Dinner",
                body: "Want to go out for dinner after Build tonight?")
        }
    }
}

...

private static AdaptiveGroup CreateGroup(string from, string subject, string body)
{
    return new AdaptiveGroup()
    {
        Children =
        {
            new AdaptiveSubgroup()
            {
                Children =
                {
                    new AdaptiveText()
                    {
                        Text = from,
                        HintStyle = AdaptiveTextStyle.Subtitle
                    },
                    new AdaptiveText()
                    {
                        Text = subject,
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    },
                    new AdaptiveText()
                    {
                        Text = body,
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    }
                }
            }
        }
    };
}
```

**결과:**

![적응형 타일 그룹 및 하위 그룹](images/adaptive-tiles-groups-subgroups.png)

## <a name="subgroups-columns"></a>하위 그룹(열)


또한 하위 그룹을 사용하여 데이터를 그룹 내의 의미 체계 섹션으로 나눌 수 있습니다. 라이브 타일의 경우 이는 시각적으로 열로 변환됩니다.

**hint-weight** 특성을 사용하면 열 너비를 제어할 수 있습니다. **hint-weight**의 값은 **GridUnitType.Star** 동작과 동일하게 사용 가능한 공간의 가중 비율로 표시됩니다. 너비가 같은 열의 경우 각 열에 가중치를 1로 할당합니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">hint-weight</td>
<td align="left">너비의 백분율</td>
</tr>
<tr class="even">
<td align="left">1</td>
<td align="left">25%</td>
</tr>
<tr class="odd">
<td align="left">1</td>
<td align="left">25%</td>
</tr>
<tr class="even">
<td align="left">1</td>
<td align="left">25%</td>
</tr>
<tr class="odd">
<td align="left">1</td>
<td align="left">25%</td>
</tr>
<tr class="even">
<td align="left">총 가중치: 4</td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

![하위 그룹, 고른 열](images/adaptive-tiles-subgroups01.png)

한 열을 다른 열보다 2배 크게 만들려면 작은 열은 가중치 1, 큰 열은 가중치 2를 할당합니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">hint-weight</td>
<td align="left">너비의 백분율</td>
</tr>
<tr class="even">
<td align="left">1</td>
<td align="left">33.3%</td>
</tr>
<tr class="odd">
<td align="left">2</td>
<td align="left">66.7%</td>
</tr>
<tr class="even">
<td align="left">총 가중치: 3</td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

![하위 그룹, 열 하나의 크기가 다른 열의 2배](images/adaptive-tiles-subgroups02.png)

첫 번째 열이 총 너비의 20%를 차지하고 두 번째 열이 총 너비의 80%를 차지하도록 하려면 첫 번째 가중치는 20, 두 번째 가중치는 80으로 할당합니다. 총 가중치가 100과 같으면 백분율로 사용됩니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">hint-weight</td>
<td align="left">너비의 백분율</td>
</tr>
<tr class="even">
<td align="left">20</td>
<td align="left">20%</td>
</tr>
<tr class="odd">
<td align="left">80</td>
<td align="left">80%</td>
</tr>
<tr class="even">
<td align="left">총 가중치: 100</td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

![하위 그룹, 가중치 합계가 100](images/adaptive-tiles-subgroups03.png)

**참고**  열 사이에는 8픽셀 여백이 자동으로 추가됩니다.

 

하위 그룹이 두 개 이상인 경우 양의 정수만 사용할 수 있는 **hint-weight**를 지정해야 합니다. 첫 번째 하위 그룹에 대해 hint-weight를 지정하지 않으면 가중치 50이 할당됩니다. hint-weight를 지정하지 않은 다음 하위 그룹에는 100에서 이전 가중치의 합계를 뺀 값과 동일한 가중치 또는 1(결과가 0인 경우)이 할당됩니다. hint-weight를 지정하지 않은 나머지 하위 그룹에는 가중치 1이 할당됩니다.

다음은 너비가 같은 열 5개를 포함하는 타일을 얻을 수 있는 방법을 보여 주는 날씨 타일의 샘플 코드입니다.

```xml
<binding template="TileWide" displayName="Seattle" branding="name">
  <group>
    <subgroup hint-weight="1">
      <text hint-align="center">Mon</text>
      <image src="Assets\Weather\Mostly Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">63°</text>
      <text hint-align="center" hint-style="captionsubtle">42°</text>
    </subgroup>
    <subgroup hint-weight="1">
      <text hint-align="center">Tue</text>
      <image src="Assets\Weather\Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">57°</text>
      <text hint-align="center" hint-style="captionsubtle">38°</text>
    </subgroup>
    <subgroup hint-weight="1">
      <text hint-align="center">Wed</text>
      <image src="Assets\Weather\Sunny.png" hint-removeMargin="true"/>
      <text hint-align="center">59°</text>
      <text hint-align="center" hint-style="captionsubtle">43°</text>
    </subgroup>
    <subgroup hint-weight="1">
      <text hint-align="center">Thu</text>
      <image src="Assets\Weather\Sunny.png" hint-removeMargin="true"/>
      <text hint-align="center">62°</text>
      <text hint-align="center" hint-style="captionsubtle">42°</text>
    </subgroup>
    <subgroup hint-weight="1">
      <text hint-align="center">Fri</text>
      <image src="Assets\Weather\Sunny.png" hint-removeMargin="true"/>
      <text hint-align="center">71°</text>
      <text hint-align="center" hint-style="captionsubtle">66°</text>
    </subgroup>
  </group>
</binding>
```

```csharp
TileWide = new TileBinding()
{
    DisplayName = "Seattle",
    Branding = TileBranding.Name,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    CreateSubgroup("Mon", "Mostly Cloudy.png", "63°", "42°"),
                    CreateSubgroup("Tue", "Cloudy.png", "57°", "38°"),
                    CreateSubgroup("Wed", "Sunny.png", "59°", "43°"),
                    CreateSubgroup("Thu", "Sunny.png", "62°", "42°"),
                    CreateSubgroup("Fri", "Sunny.png", "71°", "66°")
                }
            }
        }
    }
}

...

private static AdaptiveSubgroup CreateSubgroup(string day, string image, string highTemp, string lowTemp)
{
    return new AdaptiveSubgroup()
    {
        HintWeight = 1,
        Children =
        {
            new AdaptiveText()
            {
                Text = day,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveImage()
            {
                Source = "Assets/Weather/" + image,
                HintRemoveMargin = true
            },
            new AdaptiveText()
            {
                Text = highTemp,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveText()
            {
                Text = lowTemp,
                HintAlign = AdaptiveTextAlign.Center,
                HintStyle = AdaptiveTextStyle.CaptionSubtle
            }
        }
    };
}
```

**결과:**

![날씨 타일의 예](images/adaptive-tiles-weathertile.png)

## <a name="images"></a>이미지


&lt;image&gt; 요소는 타일 알림에 이미지를 표시하는 데 사용됩니다. 이미지는 타일 콘텐츠 내에 인라인으로 배치되거나(기본값) 콘텐츠 뒤에 배경 이미지로 배치되거나 알림 위에서 애니메이션 효과를 주는 미리 보기 이미지로 배치될 수 있습니다.

> [!NOTE]
> 참고: 앱의 패키지, 앱의 로컬 저장소 또는 웹에서 이미지를 사용할 수 있습니다. Fall Creators Update에서 일반 연결의 경우 웹 이미지는 최대 3MB이고 데이터 통신 연결의 경우 1MB입니다. Fall Creators Update를 아직 실행하지 않는 디바이스에서 웹 이미지는 200KB 이하여야 합니다.

 

추가 동작을 지정하지 않으면 이미지는 균일하게 축소 또는 확장되어 사용 가능한 너비를 채웁니다. 이 예제는 두 열과 인라인 이미지를 사용하는 타일을 보여 줍니다. 인라인 이미지는 늘어나서 열 너비를 채웁니다.

```xml
<binding template="TileMedium" displayName="Seattle" branding="name">
  <group>
    <subgroup>
      <text hint-align="center">Mon</text>
      <image src="Assets\Apps\Weather\Mostly Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">63°</text>
      <text hint-style="captionsubtle" hint-align="center">42°</text>
    </subgroup>
    <subgroup>
      <text hint-align="center">Tue</text>
      <image src="Assets\Apps\Weather\Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">57°</text>
      <text hint-style="captionSubtle" hint-align="center">38°</text>
    </subgroup>
  </group>
</binding>
```

```csharp
TileMedium = new TileBinding()
{
    DisplayName = "Seattle",
    Branding = TileBranding.Name,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    CreateSubgroup("Mon", "Mostly Cloudy.png", "63°", "42°"),
                    CreateSubgroup("Tue", "Cloudy.png", "57°", "38°")
                }
            }
        }
    }
}
...
private static AdaptiveSubgroup CreateSubgroup(string day, string image, string highTemp, string lowTemp)
{
    return new AdaptiveSubgroup()
    {
        Children =
        {
            new AdaptiveText()
            {
                Text = day,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveImage()
            {
                Source = "Assets/Weather/" + image,
                HintRemoveMargin = true
            },
            new AdaptiveText()
            {
                Text = highTemp,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveText()
            {
                Text = lowTemp,
                HintAlign = AdaptiveTextAlign.Center,
                HintStyle = AdaptiveTextStyle.CaptionSubtle
            }
        }
    };
}
```

**결과:**

![이미지 예제](images/adaptive-tiles-images01.png)

&lt;binding&gt; 루트 또는 첫 번째 그룹에 배치된 이미지도 사용 가능한 높이에 맞게 늘어납니다.

### <a name="image-alignment"></a>이미지 맞춤

**hint-align** 특성을 사용하여 이미지를 왼쪽, 가운데 또는 오른쪽으로 맞추도록 설정할 수 있습니다. 이 경우 이미지는 너비에 맞게 늘어나지 않고 기본 해상도로 표시됩니다.

```xml
<binding template="TileLarge">
  <image src="Assets/fable.jpg" hint-align="center"/>
</binding>
```

```csharp
TileLarge = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new AdaptiveImage()
            {
                Source = "Assets/fable.jpg",
                HintAlign = AdaptiveImageAlign.Center
            }
        }
    }
}
```

**결과:**

![이미지 맞춤 예제(왼쪽, 가운데, 오른쪽)](images/adaptive-tiles-imagealignment.png)

### <a name="image-margins"></a>이미지 여백

기본적으로 인라인 이미지는 위아래 콘텐츠와의 사이에 8픽셀의 여백이 있습니다. 이미지의 **hint-removeMargin** 특성을 사용하면 이 여백을 제거할 수 있습니다. 그러나 이미지는 타일 가장자리와의 8픽셀 여백을 항상 유지하고 하위 그룹(열)은 열 사이의 8픽셀 안쪽 여백을 항상 유지합니다.

```xml
<binding template="TileMedium" branding="none">
  <group>
    <subgroup>
      <text hint-align="center">Mon</text>
      <image src="Assets\Numbers\4.jpg" hint-removeMargin="true"/>
      <text hint-align="center">63°</text>
      <text hint-style="captionsubtle" hint-align="center">42°</text>
    </subgroup>
    <subgroup>
      <text hint-align="center">Tue</text>
      <image src="Assets\Numbers\3.jpg" hint-removeMargin="true"/>
      <text hint-align="center">57°</text>
      <text hint-style="captionsubtle" hint-align="center">38°</text>
    </subgroup>
  </group>
</binding>
```

```csharp
TileMedium = new TileBinding()
{
    Branding = TileBranding.None,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    CreateSubgroup("Mon", "4.jpg", "63°", "42°"),
                    CreateSubgroup("Tue", "3.jpg", "57°", "38°")
                }
            }
        }
    }
}

...

private static AdaptiveSubgroup CreateSubgroup(string day, string image, string highTemp, string lowTemp)
{
    return new AdaptiveSubgroup()
    {
        HintWeight = 1,
        Children =
        {
            new AdaptiveText()
            {
                Text = day,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveImage()
            {
                Source = "Assets/Numbers/" + image,
                HintRemoveMargin = true
            },
            new AdaptiveText()
            {
                Text = highTemp,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveText()
            {
                Text = lowTemp,
                HintAlign = AdaptiveTextAlign.Center,
                HintStyle = AdaptiveTextStyle.CaptionSubtle
            }
        }
    };
}
```

![Hint remove 여백 예제](images/adaptive-tiles-removemargin.png)

### <a name="image-cropping"></a>이미지 자르기

**hint-crop** 특성을 사용하여 이미지를 원으로 자를 수 있습니다. 현재 이 특성은 "none"(기본값) 또는 "circle" 값만 지원합니다.

```xml
<binding template="TileLarge" hint-textStacking="center">
  <group>
    <subgroup hint-weight="1"/>
    <subgroup hint-weight="2">
      <image src="Assets/Apps/Hipstame/hipster.jpg" hint-crop="circle"/>
    </subgroup>
    <subgroup hint-weight="1"/>
  </group>
 
  <text hint-style="title" hint-align="center">Hi,</text>
  <text hint-style="subtitleSubtle" hint-align="center">MasterHip</text>
</binding>
```

```csharp
TileLarge = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        TextStacking = TileTextStacking.Center,
        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    new AdaptiveSubgroup() { HintWeight = 1 },
                    new AdaptiveSubgroup()
                    {
                        HintWeight = 2,
                        Children =
                        {
                            new AdaptiveImage()
                            {
                                Source = "Assets/Apps/Hipstame/hipster.jpg",
                                HintCrop = AdaptiveImageCrop.Circle
                            }
                        }
                    },
                    new AdaptiveSubgroup() { HintWeight = 1 }
                }
            },
            new AdaptiveText()
            {
                Text = "Hi,",
                HintStyle = AdaptiveTextStyle.Title,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveText()
            {
                Text = "MasterHip",
                HintStyle = AdaptiveTextStyle.SubtitleSubtle,
                HintAlign = AdaptiveTextAlign.Center
            }
        }
    }
}
```

**결과:**

![이미지 자르기 예제](images/adaptive-tiles-imagecropping.png)

### <a name="background-image"></a>배경 이미지

배경 이미지를 설정하려면 이미지 요소를 &lt;binding&gt;의 루트에 배치하고 배치 특성을 "background"로 설정합니다.

```xml
<binding template="TileWide">
  <image src="Assets\Mostly Cloudy-Background.jpg" placement="background"/>
  <group>
    <subgroup hint-weight="1">
      <text hint-align="center">Mon</text>
      <image src="Assets\Weather\Mostly Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">63°</text>
      <text hint-align="center" hint-style="captionsubtle">42°</text>
    </subgroup>
    ...
  </group>
</binding>
```

```csharp
TileWide = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        BackgroundImage = new TileBackgroundImage()
        {
            Source = "Assets/Mostly Cloudy-Background.jpg"
        },

        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    CreateSubgroup("Mon", "Mostly Cloudy.png", "63°", "42°")
                    ...
                }
            }
        }
    }
}

...

private static AdaptiveSubgroup CreateSubgroup(string day, string image, string highTemp, string lowTemp)
{
    return new AdaptiveSubgroup()
    {
        HintWeight = 1,
        Children =
        {
            new AdaptiveText()
            {
                Text = day,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveImage()
            {
                Source = "Assets/Weather/" + image,
                HintRemoveMargin = true
            },
            new AdaptiveText()
            {
                Text = highTemp,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveText()
            {
                Text = lowTemp,
                HintAlign = AdaptiveTextAlign.Center,
                HintStyle = AdaptiveTextStyle.CaptionSubtle
            }
        }
    };
}
```

**결과:**

![배경 이미지 예제](images/adaptive-tiles-backgroundimage.png)

### <a name="peek-image"></a>미리 보기 이미지

타일의 위쪽에서 "움직이는" 이미지를 지정할 수 있습니다. 미리 보기 이미지는 애니메이션을 사용하여 타일 위쪽에서 위아래로 미끄러져 나타났다가 나중에 다시 미끄러져 사라져 타일에 주 콘텐츠를 표시합니다. 미리 보기 이미지를 설정하려면 &lt;binding&gt;의 루트에 이미지 요소를 배치하고 배치 특성을 "peek"로 설정합니다.

```xml
<binding template="TileMedium" branding="name">
  <image placement="peek" src="Assets/Apps/Hipstame/hipster.jpg"/>
  <text>New Message</text>
  <text hint-style="captionsubtle" hint-wrap="true">Hey, have you tried Windows 10 yet?</text>
</binding>
```

```csharp
TileWide = new TileBinding()
{
    Branding = TileBranding.Name,
    Content = new TileBindingContentAdaptive()
    {
        PeekImage = new TilePeekImage()
        {
            Source = "Assets/Apps/Hipstame/hipster.jpg"
        },
        Children =
        {
            new AdaptiveText()
            {
                Text = "New Message"
            },
            new AdaptiveText()
            {
                Text = "Hey, have you tried Windows 10 yet?",
                HintStyle = AdaptiveTextStyle.CaptionSubtle,
                HintWrap = true
            }
        }
    }
}
```

![미리 보기 이미지의 예](images/adaptive-tiles-imagepeeking.png)

**미리 보기 및 배경 이미지에 대한 원 자르기**

미리 보기 및 배경 이미지의 hint-crop 특성을 사용하여 원 자르기 작업을 수행할 수 있습니다.

```xml
<image placement="peek" hint-crop="circle" src="Assets/Apps/Hipstame/hipster.jpg"/>
```

```csharp
new TilePeekImage()
{
    HintCrop = TilePeekImageCrop.Circle,
    Source = "Assets/Apps/Hipstame/hipster.jpg"
}
```

다음과 같은 결과가 표시됩니다.

![미리 보기 및 배경 이미지에 대한 원 자르기](images/circlecrop-image.png)

**미리 보기 및 배경 이미지 둘 다 사용**

타일 알림에 미리 보기 및 배경 이미지 둘 다 사용하려면 알림 페이로드에 미리 보기 이미지와 배경 이미지 둘 다 지정합니다.

다음과 같은 결과가 표시됩니다.

![함께 사용된 미리 보기 및 배경 이미지](images/peekandbackground.png)


### <a name="peek-and-background-image-overlays"></a>미리 보기 및 배경 이미지 오버레이

**hint-overlay**를 사용하여 배경 및 미리 보기 이미지에 검정 오버레이를 설정할 수 있습니다. 이 특성에는 0에서 100 사이의 정수를 사용할 수 있으며, 0이면 오버레이가 없고 100이면 전체 검정 오버레이가 설정됩니다. 오버레이를 사용하여 타일의 텍스트를 읽기 쉽게 만들 수 있습니다.

**배경 이미지에 hint-overlay 사용**

페이로드에 일부 텍스트 요소가 있으면 배경 이미지는 기본적으로 20% 오버레이로 설정됩니다(없으면 기본적으로 0% 오버레이로 설정됨).

```xml
<binding template="TileWide">
  <image placement="background" hint-overlay="60" src="Assets\Mostly Cloudy-Background.jpg"/>
  ...
</binding>
```

```csharp
TileWide = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        BackgroundImage = new TileBackgroundImage()
        {
            Source = "Assets/Mostly Cloudy-Background.jpg",
            HintOverlay = 60
        },

        ...
    }
}
```

**hint-overlay 결과:**

![이미지 hint-overlay의 예](images/adaptive-tiles-image-hintoverlay.png)

**미리 보기 이미지에 hint-overlay 사용**

Windows 10 버전 1511에서는 배경 이미지와 마찬가지로 미리 보기 이미지에 대해서도 오버레이가 지원됩니다. 미리 보기 이미지 요소의 hint-overlay를 0-100 사이의 정수로 지정합니다. 미리 보기 이미지에 대한 기본 오버레이는 0(오버레이 없음)입니다.

```xml
<binding template="TileMedium">
  <image hint-overlay="20" src="Assets\Map.jpg" placement="peek"/>
  ...
</binding>
```

```csharp
TileMedium = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        PeekImage = new TilePeekImage()
        {
            Source = "Assets/Map.jpg",
            HintOverlay = 20
        },
        ...
    }
}
```

이 예제에서는 불투명도가 20%(왼쪽) 및 0%(오른쪽)인 미리 보기 이미지를 보여 줍니다.

![미리 보기 이미지의 hint-overlay](images/hintoverlay.png)

## <a name="vertical-alignment-text-stacking"></a>세로 맞춤(텍스트 스택)


[TileBinding](../tiles-and-notifications/tile-schema.md#tilebinding) 요소와 [AdaptiveSubgroup](../tiles-and-notifications/tile-schema.md#adaptivesubgroup) 요소 모두에서 **hint-textStacking** 속성을 사용하여 타일에서 콘텐츠의 세로 맞춤을 제어할 수 있습니다. 기본적으로 모든 콘텐츠는 세로로 위쪽에 맞춰지지만 콘텐츠를 아래쪽 또는 가운데에 맞출 수도 있습니다.

### <a name="text-stacking-on-binding-element"></a>바인딩 요소에서 텍스트 스택

[TileBinding](../tiles-and-notifications/tile-schema.md#tilebinding) 수준에서 적용하는 경우 텍스트 스택은 알림 콘텐츠 전체의 세로 맞춤을 설정하며, 브랜딩/배지 영역 위의 사용 가능한 세로 공간에서 맞춥니다.

```xml
<binding template="TileMedium" hint-textStacking="center" branding="logo">
  <text hint-style="base" hint-align="center">Hi,</text>
  <text hint-style="captionSubtle" hint-align="center">MasterHip</text>
</binding>
```

```csharp
TileMedium = new TileBinding()
{
    Branding = TileBranding.Logo,
    Content = new TileBindingContentAdaptive()
    {
        TextStacking = TileTextStacking.Center,
        Children =
        {
            new AdaptiveText()
            {
                Text = "Hi,",
                HintStyle = AdaptiveTextStyle.Base,
                HintAlign = AdaptiveTextAlign.Center
            },

            new AdaptiveText()
            {
                Text = "MasterHip",
                HintStyle = AdaptiveTextStyle.CaptionSubtle,
                HintAlign = AdaptiveTextAlign.Center
            }
        }
    }
}
```

![바인딩 요소에서 텍스트 스택](images/adaptive-tiles-textstack-bindingelement.png)

### <a name="text-stacking-on-subgroup-element"></a>하위 그룹 요소에서 텍스트 스택

[AdaptiveSubgroup](../tiles-and-notifications/tile-schema.md#adaptivesubgroup) 수준에서 적용하는 경우 텍스트 스택은 하위 그룹(열) 콘텐츠의 세로 맞춤을 설정하며, 전체 그룹 내의 사용 가능한 세로 공간에서 맞춥니다.

```xml
<binding template="TileWide" branding="nameAndLogo">
  <group>
    <subgroup hint-weight="33">
      <image src="Assets/Apps/Hipstame/hipster.jpg" hint-crop="circle"/>
    </subgroup>
    <subgroup hint-textStacking="center">
      <text hint-style="subtitle">Hi,</text>
      <text hint-style="bodySubtle">MasterHip</text>
    </subgroup>
  </group>
</binding>
```

```csharp
TileWide = new TileBinding()
{
    Branding = TileBranding.NameAndLogo,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    // Image column
                    new AdaptiveSubgroup()
                    {
                        HintWeight = 33,
                        Children =
                        {
                            new AdaptiveImage()
                            {
                                Source = "Assets/Apps/Hipstame/hipster.jpg",
                                HintCrop = AdaptiveImageCrop.Circle
                            }
                        }
                    },

                    // Text column
                    new AdaptiveSubgroup()
                    {
                        // Vertical align its contents
                        TextStacking = TileTextStacking.Center,
                        Children =
                        {
                            new AdaptiveText()
                            {
                                Text = "Hi,",
                                HintStyle = AdaptiveTextStyle.Subtitle
                            },

                            new AdaptiveText()
                            {
                                Text = "MasterHip",
                                HintStyle = AdaptiveTextStyle.BodySubtle
                            }
                        }
                    }
                }
            }
        }
    }
}
```

## <a name="related-topics"></a>관련 항목
* [타일 콘텐츠 스키마](../tiles-and-notifications/tile-schema.md)
* [로컬 타일 알림 보내기](sending-a-local-tile-notification.md)
* [특수 타일 템플릿](special-tile-templates-catalog.md)
* [UWP 커뮤니티 도구 키트 - 알림](https://github.com/Microsoft/UWPCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)
* [GitHub의 Windows 알림](https://github.com/WindowsNotifications)

 

 




