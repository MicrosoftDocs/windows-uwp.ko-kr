---
Description: 적응 타일 템플릿은 Windows 10의 새로운 기능으로, 다양 한 화면 밀도에 맞게 조정 되는 간단 하 고 유연한 태그 언어를 사용 하 여 고유한 타일 알림 콘텐츠를 디자인할 수 있습니다.
title: 적응형 타일 만들기
ms.assetid: 1246B58E-D6E3-48C7-AD7F-475D113600F9
label: Create adaptive tiles
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7cee236b385b6129e7ab1a9cacd549f217f6e734
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175607"
---
# <a name="create-adaptive-tiles"></a>적응형 타일 만들기

적응 타일 템플릿은 Windows 10의 새로운 기능으로, 다양 한 화면 밀도에 맞게 조정 되는 간단 하 고 유연한 태그 언어를 사용 하 여 고유한 타일 알림 콘텐츠를 디자인할 수 있습니다. 이 문서에서는 Windows 앱의 적응형 라이브 타일을 만드는 방법을 설명합니다. 적응 요소 및 특성의 전체 목록은 [적응 타일 스키마](../tiles-and-notifications/tile-schema.md)를 참조 하세요.

(원하는 경우 Windows 10에 대 한 알림을 디자인할 때 [windows 8 타일 템플릿 카탈로그](/previous-versions/windows/apps/hh761491(v=win.10)) 에서 미리 설정 된 템플릿을 계속 사용할 수 있습니다.)


## <a name="getting-started"></a>시작

**알림 라이브러리를 설치 합니다.** XML 대신 c #을 사용 하 여 알림을 생성 하려는 경우에 [는 이름이 "](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) notification uwp" 인 NuGet 패키지를 설치 합니다. 이 문서에 제공 된 c # 샘플에서는 NuGet 패키지의 버전 1.0.0을 사용 합니다.

**알림 시각화 도우미를 설치 합니다.** 이 무료 Windows 앱을 사용 하면 Visual Studio의 XAML 편집기/디자인 뷰와 유사 하 게 편집할 때 타일에 대 한 즉각적인 시각적 미리 보기를 제공 하 여 적응 라이브 타일을 디자인할 수 있습니다. 자세한 내용은 [알림 시각화 도우미](notifications-visualizer.md) 를 참조 하거나, [스토어에서 알림 시각화 도우미를 다운로드](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)하세요.


## <a name="how-to-send-a-tile-notification"></a>타일 알림을 보내는 방법

[로컬 타일 알림 전송에 대 한 빠른 시작을](sending-a-local-tile-notification.md)참조 하세요. 아래 설명서에서는 적응 타일에 대 한 모든 시각적 UI 가능성을 설명 합니다.


## <a name="usage-guidance"></a>사용 지침


적응 템플릿은 다양 한 폼 팩터 및 알림 유형 간에 작동 하도록 디자인 되었습니다. Group 및 부분군과 같은 요소는 콘텐츠를 함께 연결 하 고 특정 시각적 동작을 의미 하지는 않습니다. 알림의 최종 모양은 휴대폰, 태블릿, 데스크톱 또는 다른 장치 인지에 관계 없이 표시 되는 특정 장치를 기반으로 해야 합니다.

힌트는 특정 시각적 동작을 얻기 위해 요소에 추가할 수 있는 선택적 특성입니다. 힌트는 장치별 또는 알림과 관련 될 수 있습니다.

## <a name="a-basic-example"></a>기본 예


이 예제에서는 적응 타일 템플릿에서 생성할 수 있는 작업을 보여 줍니다.

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


각 타일 크기의 콘텐츠는 XML 페이로드 내의 개별 [TileBinding](../tiles-and-notifications/tile-schema.md#tilebinding) 요소에 개별적으로 지정 됩니다. 템플릿 특성을 다음 값 중 하나로 설정 하 여 대상 크기를 선택 합니다.

-   TileSmall
-   TileMedium
-   TileWide
-   TileLarge (데스크톱 전용)

단일 타일 알림 XML 페이로드의 경우 &lt; &gt; 다음 예와 같이 지원 하려는 각 타일 크기에 대 한 바인딩 요소를 제공 합니다.

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

![적응 타일 크기: 작음, 중형, 넓게 및 큼](images/adaptive-tiles-sizes.png)

## <a name="branding"></a>브랜딩


알림 페이로드의 브랜딩 특성을 사용 하 여 라이브 타일 (표시 이름 및 모퉁이 로고)의 맨 아래에 있는 브랜딩을 제어할 수 있습니다. "none," "name"만, "logo"만 또는 "nameAndLogo"를 사용하여 둘 다 표시하도록 선택할 수 있습니다.

**참고**    Windows Mobile은 모퉁이 로고를 지원 하지 않으므로 "로고" 및 "nameAndLogo"는 모바일의 "이름"으로 기본 설정 됩니다.

 

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

![적응 타일, 이름 및 로고](images/adaptive-tiles-namelogo.png)

브랜딩은 다음 두 가지 방법 중 하나로 특정 타일 크기에 적용 될 수 있습니다.

1. [TileBinding](../tiles-and-notifications/tile-schema.md#tilebinding) 요소에 특성을 적용 하 여
2. 바인딩에 대해 브랜딩을 지정 하지 않는 경우 전체 알림 페이로드에 영향을 주는 [TileVisual](../tiles-and-notifications/tile-schema.md#tilevisual) 요소에 특성을 적용 하 여 시각적 요소에 제공 된 브랜딩을 사용 합니다.

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

알림 페이로드에 브랜딩을 지정 하지 않으면 기본 타일의 속성에 따라 브랜딩이 결정 됩니다. 기본 타일에 표시 이름이 표시 되 면 브랜딩은 기본적으로 "이름"으로 표시 됩니다. 그렇지 않으면 표시 이름이 표시 되지 않는 경우 브랜딩은 기본적으로 "없음"으로 표시 됩니다.

**참고**    이는 기본 브랜딩을 "로고" 였던 Windows 8.x에서 변경 된 내용입니다.

 

## <a name="display-name"></a>표시 이름


**DisplayName** 특성을 사용 하 여 선택한 텍스트 문자열을 입력 하 여 알림의 표시 이름을 재정의할 수 있습니다. 브랜딩을 사용 하는 것 처럼 전체 알림 페이로드에 영향을 주는 [TileVisual](../tiles-and-notifications/tile-schema.md#tilevisual) 요소 또는 개별 타일에만 영향을 주는 [TileBinding](../tiles-and-notifications/tile-schema.md#tilebinding) 요소에 대해이를 지정할 수 있습니다.

**알려진 문제**  Windows Mobile에서 타일에 대해 짧은 이름를 지정 하면 알림에 제공 된 표시 이름이 사용 되지 않습니다 (짧은 이름 항상 표시 됨). 

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

![적응 타일 표시 이름](images/adaptive-tiles-displayname.png)

## <a name="text"></a>텍스트


[AdaptiveText](../tiles-and-notifications/tile-schema.md#adaptivetext) 요소는 텍스트를 표시 하는 데 사용 됩니다. 힌트를 사용 하 여 텍스트가 표시 되는 방식을 수정할 수 있습니다.

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

![적응 타일 텍스트](images/adaptive-tiles-text.png)

## <a name="text-wrapping"></a>텍스트 줄 바꿈


기본적으로 텍스트는 줄 바꿈되지 않고 타일의 가장자리에서 계속 됩니다. **힌트 줄 바꿈** 특성을 사용 하 여 텍스트 요소에 텍스트 줄 바꿈을 설정 합니다. **힌트-minlines** 및 **Hint-maxlines**를 사용 하 여 최소 및 최대 줄 수를 제어할 수도 있습니다. 둘 다 양의 정수를 허용 합니다.

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

![텍스트 줄 바꿈이 있는 적응 타일](images/adaptive-tiles-textwrapping.png)

## <a name="text-styles"></a>텍스트 스타일


스타일은 텍스트 요소의 글꼴 크기, 색 및 두께를 제어 합니다. 불투명도를 60%로 설정 하는 각 스타일의 "미묘한" 변형을 포함 하는 다양 한 스타일이 있습니다. 일반적으로 텍스트 색을 연한 회색 음영으로 만듭니다.

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

![적응 타일 텍스트 스타일](images/adaptive-tiles-textstyles.png)

**참고**    힌트 스타일을 지정 하지 않은 경우 스타일은 기본적으로 caption로 설정 됩니다.

 

**기본 텍스트 스타일**

|                                |                           |             |
|--------------------------------|---------------------------|-------------|
| &lt;텍스트 힌트-스타일 = " \* "/&gt; | 글꼴 높이               | 글꼴 두께 |
| caption                        | 12 개의 유효 픽셀 (epx) | 주기적     |
| 본문                           | 15 window.epx.codesnippet                    | 주기적     |
| base                           | 15 window.epx.codesnippet                    | 약간 굵게    |
| 제목                       | 20 window.epx.codesnippet                    | 주기적     |
| title                          | 24 window.epx.codesnippet                    | 굴림   |
| subheader.aboutdocs                      | 34 window.epx.codesnippet                    | 밝음       |
| header                         | 46 window.epx.codesnippet                    | 밝음       |

 

**숫자 텍스트 스타일 변형**

이러한 변형은 줄 높이를 줄여 해당 내용이 텍스트에 훨씬 더 가깝게 배치 되도록 합니다.

|                  |
|------------------|
| titleNumeral     |
| subheaderNumeral |
| headerNumeral    |

 

**미묘한 텍스트 스타일 변형**

각 스타일에는 일반적으로 텍스트 색을 연한 회색으로 표시 하는 60% 불투명도를 텍스트에 제공 하는 미묘한 변형이 있습니다.

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


텍스트를 가로 방향으로 왼쪽, 가운데 또는 오른쪽에 맞출 수 있습니다. 영어와 같이 왼쪽에서 오른쪽으로의 언어에서 텍스트 기본값은 왼쪽 맞춤입니다. 아랍어와 같은 오른쪽에서 왼쪽으로의 언어에서 텍스트 기본값은 오른쪽 맞춤입니다. 요소에 대 한 **힌트 맞춤** 특성을 사용 하 여 수동으로 맞춤을 설정할 수 있습니다.

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

![적응 타일 텍스트 맞춤](images/adaptive-tiles-textalignment.png)

## <a name="groups-and-subgroups"></a>그룹 및 하위 그룹


그룹을 사용 하 여 그룹 내부의 콘텐츠가 서로 관련 되어 있는지를 의미 있게 선언 하 고 콘텐츠를 이해 하기 쉽도록 전체에 표시 해야 합니다. 예를 들어 두 개의 텍스트 요소, 헤더 및 하위 헤더가 있을 수 있으며 표시 되는 헤더에 대해서만 의미가 없습니다. 하위 그룹 내에서 이러한 요소를 그룹화 하 여 요소를 모두 표시 하거나 (해당 하는 경우에는 맞지 않음) 전혀 표시 하지 않을 수 있습니다.

장치 및 화면에서 최상의 환경을 제공 하려면 여러 그룹을 제공 합니다. 그룹이 여러 개인 경우 타일이 큰 화면에 맞게 조정 될 수 있습니다.

**참고**    그룹의 올바른 자식은 하위 그룹입니다.

 

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

![적응 타일 그룹 및 하위 그룹](images/adaptive-tiles-groups-subgroups.png)

## <a name="subgroups-columns"></a>하위 그룹 (열)


또한 하위 그룹을 사용 하 여 그룹 내의 의미 체계 섹션으로 데이터를 나눌 수 있습니다. 라이브 타일의 경우이는 열로 시각적으로 변환 됩니다.

**힌트 가중치** 특성을 사용 하면 열의 너비를 제어할 수 있습니다. **힌트 가중치** 값은 사용 가능한 공간의 가중치 비율로 표현 되며 **system.windows.gridunittype>** 동작과 동일 합니다. 너비가 같은 열의 경우 각 가중치를 1로 할당 합니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">힌트-가중치</td>
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
<td align="left">총 무게: 4</td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

![하위 그룹, 열 짝수](images/adaptive-tiles-subgroups01.png)

한 열을 한 열에서 다른 열로 두 배로 만들려면 작은 열에 가중치 1을 할당 하 고 큰 열에 가중치 2를 할당 합니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">힌트-가중치</td>
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
<td align="left">총 무게: 3</td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

![하위 그룹, 한 열이 다른 열 크기의 2 배](images/adaptive-tiles-subgroups02.png)

첫 번째 열이 전체 너비의 20%를 차지 하 고 두 번째 열에서 총 너비의 80%를 차지 하도록 하려면 첫 번째 가중치를 20으로, 두 번째 가중치를 80에 할당 합니다. 총 가중치가 100 인 경우 백분율로 작용 합니다.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">힌트-가중치</td>
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
<td align="left">총 무게: 100</td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

![가중치가 합하면 100 인 하위 그룹](images/adaptive-tiles-subgroups03.png)

**참고**    열 사이에 8 픽셀 여백이 자동으로 추가 됩니다.

 

세 개 이상의 하위 그룹이 있는 경우 양의 정수만 허용 하는 **힌트 가중치**를 지정 해야 합니다. 첫 번째 하위 그룹에 대 한 힌트 가중치를 지정 하지 않으면 가중치가 50으로 할당 됩니다. 지정 된 힌트 가중치가 없는 다음 하위 그룹에는 100에 해당 하는 가중치 (이전 가중치의 합계를 뺀 값) 또는 결과가 0 인 경우 1로 할당 됩니다. 지정 된 힌트 가중치가 없는 나머지 하위 그룹에는 가중치가 1로 할당 됩니다.

너비가 같은 5 개의 열이 있는 타일을 달성할 수 있는 방법을 보여 주는 날씨 타일에 대 한 샘플 코드는 다음과 같습니다.

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


&lt;이미지 &gt; 요소는 타일 알림에 이미지를 표시 하는 데 사용 됩니다. 이미지는 타일 콘텐츠 내에 인라인으로 배치 될 수 있습니다 (기본값), 콘텐츠 뒤의 배경 이미지 또는 알림의 위쪽에서 애니메이션을 적용 하는 피킹 (peeking) 이미지

> [!NOTE]
> 이미지는 앱의 패키지, 앱의 로컬 저장소 또는 웹에서 사용할 수 있습니다. 가 중 작성자 업데이트를 기준으로 웹 이미지는 일반 연결의 경우 최대 3mb, 요금제 연결의 경우 1mb가 될 수 있습니다. 아직가 중 작성자 업데이트를 실행 하지 않는 장치에서는 웹 이미지가 200 KB 보다 크지 않아야 합니다.

 

추가 동작을 지정 하지 않으면 이미지는 사용 가능한 너비를 채우도록 균등 하 게 축소 또는 확장 됩니다. 이 예제에서는 두 개의 열과 인라인 이미지를 사용 하는 타일을 보여 줍니다. 인라인 이미지를 늘려 열의 너비를 채웁니다.

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

&lt; &gt; 또한 바인딩 루트 또는 첫 번째 그룹에 배치 된 이미지는 사용 가능한 높이에 맞게 확장 됩니다.

### <a name="image-alignment"></a>이미지 맞춤

**힌트 맞춤** 특성을 사용 하 여 왼쪽, 가운데 또는 오른쪽 맞춤으로 이미지를 설정할 수 있습니다. 이렇게 하면 이미지가 채우기 너비로 확장 되는 대신 네이티브 해상도로 표시 됩니다.

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

![이미지 맞춤 예제 (왼쪽, 가운데, 오른쪽)](images/adaptive-tiles-imagealignment.png)

### <a name="image-margins"></a>이미지 여백

기본적으로 인라인 이미지는 이미지의 위 또는 아래에 있는 모든 내용 사이에 8 픽셀의 여백을 갖습니다. 이 여백은 이미지에서 **힌트-removeMargin** 특성을 사용 하 여 제거할 수 있습니다. 그러나 이미지는 항상 타일의 가장자리에서 8 픽셀 여백을 유지 하 고 하위 그룹 (열)은 항상 열 사이의 8 픽셀 안쪽 여백을 유지 합니다.

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

![힌트 여백 제거 예](images/adaptive-tiles-removemargin.png)

### <a name="image-cropping"></a>이미지 자르기

현재 "none" (기본값) 또는 "circle" 값만 지 원하는 **힌트-자르기** 특성을 사용 하 여 이미지를 원형으로 자를 수 있습니다.

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

![이미지 자르기 예](images/adaptive-tiles-imagecropping.png)

### <a name="background-image"></a>배경 이미지

배경 이미지를 설정 하려면 이미지 요소를 바인딩의 루트에 배치 하 &lt; &gt; 고 배치 특성을 "background"로 설정 합니다.

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

![배경 이미지 예](images/adaptive-tiles-backgroundimage.png)

### <a name="peek-image"></a>이미지 피킹

타일의 위쪽에서 "피킹" 하는 이미지를 지정할 수 있습니다. Peek 이미지는 애니메이션을 사용 하 여 타일 위쪽에서 아래쪽/위쪽으로 이동 하 고 보기를 표시 한 다음 나중에 다시 이동 하 여 타일의 주 콘텐츠를 표시 합니다. Peek 이미지를 설정 하려면 이미지 요소를 바인딩의 루트에 배치 하 &lt; &gt; 고 배치 특성을 "peek"로 설정 합니다.

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

![이미지 피킹 예](images/adaptive-tiles-imagepeeking.png)

**피킹 (peeking) 및 배경 이미지에 대 한 원형 자르기**

피킹 (peeking) 및 배경 이미지의 힌트-자르기 특성을 사용 하 여 원 자르기를 수행 합니다.

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

결과는 다음과 같습니다.

![피킹 (peeking) 및 배경 이미지에 대 한 원형 자르기](images/circlecrop-image.png)

**피킹 (peeking) 및 배경 이미지 모두 사용**

타일 알림에 피킹 (peeking) 및 배경 이미지를 모두 사용 하려면 알림 페이로드의 피킹 (peeking) 이미지와 배경 이미지를 모두 지정 합니다.

결과는 다음과 같습니다.

![함께 사용된 미리 보기 및 배경 이미지](images/peekandbackground.png)


### <a name="peek-and-background-image-overlays"></a>피킹 (peeking) 및 배경 이미지 오버레이

0-100의 정수를 허용 하는 **힌트 오버레이**를 사용 하 여 배경에 검정색 오버레이를 설정 하 고, 0으로 오버레이 및 100이 완전 한 검은색 오버레이로 표시 되는 이미지를 피킹할 수 있습니다. 오버레이를 사용 하 여 타일의 텍스트를 읽을 수 있도록 할 수 있습니다.

**배경 이미지에서 힌트 오버레이 사용**

페이로드에 일부 텍스트 요소가 있으면 배경 이미지는 기본적으로 20% 오버레이로 표시 됩니다 (그렇지 않은 경우 기본값은 0% 오버레이).

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

**힌트-오버레이 결과:**

![이미지 힌트 오버레이의 예](images/adaptive-tiles-image-hintoverlay.png)

**피킹 (peeking) 이미지에서 힌트 오버레이 사용**

Windows 10 버전 1511에서는 배경 이미지와 마찬가지로 피킹 (peeking) 이미지에 대 한 오버레이도 지원 합니다. Peek image 요소에서 힌트-오버레이를 0-100의 정수로 지정 합니다. 이미지 피킹 (peeking)의 기본 오버레이는 0 (오버레이 없음)입니다.

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

이 예제에서는 20% 불투명 (왼쪽) 및 0% 불투명도 (오른쪽)의 피킹 (peeking) 이미지를 보여 줍니다.

![힌트-피킹 (peeking) 이미지의 오버레이](images/hintoverlay.png)

## <a name="vertical-alignment-text-stacking"></a>세로 맞춤 (텍스트 쌓기)


[TileBinding](../tiles-and-notifications/tile-schema.md#tilebinding) 요소와 [AdaptiveSubgroup](../tiles-and-notifications/tile-schema.md#adaptivesubgroup) 요소 모두에서 **힌트-textstacking** 특성을 사용 하 여 타일의 내용에 대 한 세로 맞춤을 제어할 수 있습니다. 기본적으로 모든 항목은 위쪽에 세로로 정렬 되지만 콘텐츠를 아래쪽 또는 가운데에 맞출 수도 있습니다.

### <a name="text-stacking-on-binding-element"></a>바인딩 요소에 대 한 텍스트 쌓기

[TileBinding](../tiles-and-notifications/tile-schema.md#tilebinding) 수준에서 적용 되는 경우 텍스트 맞춤은 알림 콘텐츠의 세로 맞춤을 전체로 설정 하 고, 사용 가능한 세로 공간을 브랜딩/배지 영역 위에 맞춥니다.

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

![바인딩 요소에 대 한 텍스트 쌓기](images/adaptive-tiles-textstack-bindingelement.png)

### <a name="text-stacking-on-subgroup-element"></a>하위 그룹 요소의 텍스트 쌓기

[AdaptiveSubgroup](../tiles-and-notifications/tile-schema.md#adaptivesubgroup) 수준에서 적용 되는 경우 텍스트 맞춤은 하위 그룹 (열) 콘텐츠의 세로 맞춤을 설정 하 여 전체 그룹 내의 사용 가능한 세로 공간에 맞춥니다.

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
* [UWP 커뮤니티 도구 키트-알림](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)
* [GitHub의 Windows 알림](https://github.com/WindowsNotifications)

 

 