---
author: andrewleader
Description: Special tile templates are unique templates that are either animated, or just allow you to do things that aren't possible with adaptive tiles.
title: 특수 타일 템플릿
ms.assetid: 1322C9BA-D5B2-45E2-B813-865884A467FF
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10 uwp
ms.localizationpriority: medium
ms.openlocfilehash: 74b915e4d698503a13c5066348f4e46ebd3125c8
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7159810"
---
# <a name="special-tile-templates"></a>특수 타일 템플릿
 

특수 타일 템플릿은 애니메이션 효과가 추가되었거나 적응형 타일에서 불가능한 작업을 수행할 수 있도록 하는 고유한 템플릿입니다. 각 특수 타일 템플릿은 특별히 용으로 빌드된 Windows10, 아이콘 타일 템플릿을 제외 하 고 Windows10에 대 한 업데이트 된 클래식 특수 템플릿인 합니다. 이 문서에서는 세 가지 특수 타일 템플릿인 아이콘, 사진 및 피플에 대해 설명합니다.

## <a name="iconic-tile-template"></a>아이콘 타일 템플릿


아이콘 템플릿("IconWithBadge" 템플릿이라고도 함)을 사용하면 타일 중앙에 작은 이미지를 표시할 수 있습니다. Windows10 휴대폰과 태블릿/데스크톱 둘 다에서 템플릿을 지원합니다.

![작은 크기 및 중간 크기 메일 타일](images/iconic-template-mail-2sizes.png)

### <a name="how-to-create-an-iconic-tile"></a>아이콘 타일을 만드는 방법

다음 단계를 사용 가린다는 Windows10 아이콘 타일을 만들기 위해 알아야 할 수 있습니다. 대략적으로 설명하면, 아이콘 이미지 자산이 필요하며, 아이콘 템플릿을 사용하여 타일에 알림을 보내고, 마지막으로 타일에 표시할 번호를 제공하는 배지 알림을 보냅니다.

![아이콘 타일의 개발자 흐름](images/iconic-template-dev-flow.png)

**1단계: PNG 형식으로 이미지 자산 만들기**

타일에 대한 아이콘 자산을 만들고 다른 자산과 함께 프로젝트 리소스에 배치합니다. 최소한 휴대폰과 데스크톱에서 작은 타일과 중간 크기 타일에 대해 작동하는 200x200 픽셀 아이콘을 만듭니다. 최상의 사용자 환경을 제공하려면 각 크기에 대한 아이콘을 만듭니다. 이러한 자산에는 안쪽 여백이 필요 없습니다. 아래 이미지에서 크기 조정 세부 정보를 참조하세요.

아이콘 자산을 투명성이 있는 PNG 형식으로 저장합니다. Windows Phone에서 모든 불투명 픽셀은 흰색(RGB 255, 255, 255)으로 표시됩니다. 일관성과 단순성을 위해 데스크톱 아이콘에도 흰색을 사용합니다.

태블릿, 노트북 및 데스크톱의 Windows 10에서는 정사각형 아이콘 자산만 지원합니다. 휴대폰은 정사각형 자산과 너비:높이 비율이 최대 2:3까지 너비보다 높이가 큰 자산(휴대폰 아이콘 등의 이미지에 유용함)을 둘 다 지원합니다.

![휴대폰과 데스크톱에서 작은 타일과 중간 크기 타일의 아이콘 크기 조정](images/iconic-template-sizing-info.png)

![배지가 있는 자산 및 배지가 없는 자산에 대한 크기 조정](images/assetguidance24.png)

정사각형 자산의 경우 컨테이너 내에서 자동으로 가운데에 배치됩니다.

![배지가 있거나 없는 정사각형 자산 크기 조정](images/assetguidance25.png)

정사각형이 아닌 자산의 경우 컨테이너의 너비/높이에 대한 자동 가로/세로 가운데 배치 및 맞추기가 발생합니다.

![배지가 있거나 없는 정사각형이 아닌 자산 크기 조정](images/assetguidance26a.png)

![배지가 있거나 없는 정사각형이 아닌 자산 크기 조정](images/assetguidance26b.png)

**2단계: 기본 타일 만들기**

기본 타일과 보조 타일 둘 다에서 아이콘 템플릿을 사용할 수 있습니다. 보조 타일에서 사용하는 경우 먼저 보조 타일을 만들거나 이미 고정된 보조 타일을 사용해야 합니다. 기본 타일은 암시적으로 고정되어 있으며 항상 알림을 받을 수 있습니다.

**3단계: 타일에 알림 보내기**

이 단계는 알림이 로컬로 전송되는지, 서버 푸시 통해 전송되는지에 따라 다를 수 있지만 보내는 XML 페이로드는 동일합니다. 로컬 타일 알림을 보내려면 타일(기본 타일 또는 보조 타일)에 대한 [**TileUpdater**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater)를 만든 다음 아래와 같이 아이콘 타일 템플릿을 사용하는 타일에 알림을 보냅니다. 이상적일 경우 [적응형 타일 템플릿](create-adaptive-tiles.md)을 사용하여 와이드 타일 및 큰 타일 크기에 대한 바인딩도 포함해야 합니다.

XML 페이로드에 대한 샘플 코드는 다음과 같습니다.

```xml
<tile>
  <visual>

    <binding template="TileSquare150x150IconWithBadge">
      <image id="1" src="Iconic.png" alt="alt text"/>
    </binding>
    
    <binding template="TileSquare71x71IconWithBadge">
      <image id="1" src="Iconic.png" alt="alt text"/>
    </binding>

  </visual>
</tile>
```

이 아이콘 타일 템플릿 XML 페이로드는 1단계에서 만든 이미지를 가리키는 이미지 요소를 사용합니다. 이제 타일이 아이콘 옆에 배지를 표시할 준비가 되었습니다. 배지 알림을 보내기만 하면 됩니다.

**4단계: 타일에 배지 알림 보내기**

3단계와 마찬가지로, 이 단계는 알림이 로컬로 전송되는지, 서버 푸시 통해 전송되는지에 따라 다를 수 있지만 보내는 XML 페이로드는 동일합니다. 로컬 배지 알림을 보내려면 타일(기본 타일 또는 보조 타일)에 대한 [**BadgeUpdater**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.BadgeUpdater)를 만든 다음 원하는 값이 포함된 배지 알림을 보내거나 배지를 지웁니다.

XML 페이로드에 대한 샘플 코드는 다음과 같습니다.

```xml
<badge value="2"/>
```

타일의 배지가 적절하게 업데이트됩니다.

**5단계: 요약**

다음 이미지는 다양한 API 및 페이로드가 아이콘 타일 템플릿의 각 측면과 연결되는 방식을 보여 줍니다. &lt;binding&gt; 요소를 포함하는 [타일 알림](https://msdn.microsoft.com/library/windows/apps/hh779724)은 아이콘 템플릿과 이미지 자산을 지정하는 데 사용되고, [배지 알림](https://msdn.microsoft.com/library/windows/apps/hh779719)은 숫자 값을 지정합니다. 타일 속성은 타일의 표시 이름, 색 등을 제어합니다.

![아이콘 타일 템플릿과 연결된 API 및 페이로드](images/iconic-template-properties-info.png)

## <a name="photos-tile-template"></a>사진 타일 템플릿


사진 타일 템플릿을 사용하면 라이브 타일에 사진 슬라이드 쇼를 표시할 수 있습니다. 템플릿은 작은 크기를 포함하여 모든 타일 크기에서 지원되며 각 타일 크기에서 동일하게 동작합니다. 아래 예제에서는 사진 템플릿을 사용하는 중간 크기 타일의 5개 프레임을 보여 줍니다. 템플릿에는 선택한 사진을 순환하고 무한 반복되는 확대/축소 및 크로스 페이드 애니메이션이 있습니다.

![사진 타일 템플릿을 사용하는 이미지 Slideshow](images/photo-tile-template-image01.jpg)

### <a name="how-to-use-the-photos-template"></a>사진 템플릿을 사용하는 방법

[알림 라이브러리](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)를 설치한 경우 사진 템플릿을 쉽게 사용할 수 있습니다. 원시 XML을 사용할 수도 있지만 유효한 XML 또는 XML 이스케이프 콘텐츠 생성에 대해 걱정할 필요가 없도록 라이브러리를 사용하는 것이 좋습니다.

Windows Phone은 슬라이드 쇼 하나에 최대 9장의 사진을 표시하고 태블릿, 노트북 및 데스크톱은 최대 12장을 표시합니다.

타일 알림을 보내는 방법에 대한 자세한 내용은 [알림 보내기 문서](index.md)를 참조하세요.


```xml
<!--
 
To use the Photos template...
 
 - On any adaptive tile binding (like TileMedium or TileWide)
   - Set the hint-presentation attribute to "photos"
   - Add up to 12 images as children of the binding.
    
-->
 
<tile>
  <visual>
     
    <binding template="TileMedium" hint-presentation="photos">
       
      <image src="Assets/1.jpg" />
      <image src="ms-appdata:///local/Images/2.jpg"/>
      <image src="http://msn.com/images/3.jpg"/>
       
      <!--TODO: Can have 12 images total-->
       
    </binding>
     
    <!--TODO: Add bindings for other tile sizes-->
     
  </visual>
</tile>
```

```csharp
/*
 
To use the Photos template...
 
 - On any TileBinding object
   - Set Content property to new instance of TileBindingContentPhotos
   - Add up to 12 images to Images property of TileBindingContentPhotos.
 
*/
 
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentPhotos()
            {
                Images =
                {
                    new TileBasicImage() { Source = "Assets/1.jpg" },
                    new TileBasicImage() { Source = "ms-appdata:///local/Images/2.jpg" },
                    new TileBasicImage() { Source = "http://msn.com/images/3.jpg" }
 
                    // TODO: Can have 12 images total
                }
            }
        }
 
        // TODO: Add other tile sizes
    }
};
```

## <a name="people-tile-template"></a>피플 타일 템플릿


Windows 10의 피플 앱 타일은 타일에 세로 또는 가로로 슬라이드되는 원 안의 이미지 컬렉션을 표시하는 특수 타일 템플릿을 사용합니다. 이 타일 템플릿은 Windows10 빌드 10572부터 사용할 수 있었던 및 모든 사용자가 앱에서 사용할 수입니다.

피플 타일 템플릿은 다음 크기의 타일에서 작동합니다.

**중간 크기 타일**(TileMedium)

![중간 크기 피플 타일](images/people-tile-medium.png)

 

**와이드 타일**(TileWide)

![와이드 피플 타일](images/people-tile-wide.png)

 

**큰 타일(데스크톱에만 해당)** (TileLarge)

![큰 피플 타일](images/people-tile-large.png)

 

[알림 라이브러리](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)를 사용하는 경우 *TileBinding* 콘텐츠에 대한 새 *TileBindingContentPeople* 개체를 만들기만 하면 피플 타일 템플릿을 사용할 수 있습니다. *TileBindingContentPeople* 클래스에는 Images 속성이 있으며, 여기서 이미지를 추가합니다.

원시 XML을 사용하는 경우 *hint-presentation*을 "people"로 설정하고 이미지를 binding 요소의 자식으로 추가합니다.

다음 C# 코드 샘플에서는 [알림 라이브러리](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)를 사용 중이라고 가정합니다.

```csharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentPeople()
            {
                Images =
                {
                    new TileBasicImage() { Source = "Assets/ProfilePics/1.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/2.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/3.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/4.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/5.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/6.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/7.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/8.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/9.jpg" }
                }
            }
        }
    }
};
```

```xml
<tile>
  <visual>
 
    <binding template="TileMedium" hint-presentation="people">
      <image src="Assets/ProfilePics/1.jpg"/>
      <image src="Assets/ProfilePics/2.jpg"/>
      <image src="Assets/ProfilePics/3.jpg"/>
      <image src="Assets/ProfilePics/4.jpg"/>
      <image src="Assets/ProfilePics/5.jpg"/>
      <image src="Assets/ProfilePics/6.jpg"/>
      <image src="Assets/ProfilePics/7.jpg"/>
      <image src="Assets/ProfilePics/8.jpg"/>
      <image src="Assets/ProfilePics/9.jpg"/>
    </binding>
 
  </visual>
</tile>
```

최상의 사용자 환경을 얻으려면 각 타일 크기에 대해 다음 개수의 사진을 제공하는 것이 좋습니다.

-   중간 크기 타일: 사진 9장
-   와이드 타일: 사진 15장
-   큰 타일: 사진 20장

해당 개수의 사진을 제공하면 빈 원이 몇 개 남기 때문에 타일이 시각적으로 너무 혼잡하지 않습니다. 자유롭게 사진 수를 조정하여 가장 적합한 모양을 만들 수 있습니다.

알림을 보내려고 [알림 전달 방법 선택](choosing-a-notification-delivery-method.md)을 참조하세요.

## <a name="related-topics"></a>관련 항목


* [GitHub의 전체 코드 샘플](https://github.com/WindowsNotifications/quickstart-people-tile-template)
* [알림 라이브러리](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)
* [타일, 배지 및 알림](index.md)
* [적응형 타일 만들기](create-adaptive-tiles.md)
* [타일 콘텐츠 스키마](../tiles-and-notifications/tile-schema.md)
 

 




