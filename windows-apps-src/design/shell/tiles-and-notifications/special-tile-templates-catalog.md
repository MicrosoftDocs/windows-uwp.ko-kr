---
Description: 특수 타일 템플릿은 애니메이션 효과가 추가되었거나 적응형 타일에서 불가능한 작업을 수행할 수 있도록 하는 고유한 템플릿입니다.
title: 특수 타일 템플릿
ms.assetid: 1322C9BA-D5B2-45E2-B813-865884A467FF
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4368c3d504b01a48b05fcaffdb01a71b916d9d43
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89169307"
---
# <a name="special-tile-templates"></a>특수 타일 템플릿
 

특수 타일 템플릿은 애니메이션 효과가 추가되었거나 적응형 타일에서 불가능한 작업을 수행할 수 있도록 하는 고유한 템플릿입니다. Windows 10 용으로 업데이트 된 클래식 특수 템플릿인 아이콘 타일 템플릿을 제외 하 고, 각 특수 타일 템플릿은 Windows 10 용으로 특별히 빌드 되었습니다. 이 문서에서는 아이콘, 사진 및 인물 이라는 세 가지 특수 한 타일 템플릿을 설명 합니다.

## <a name="iconic-tile-template"></a>아이콘 타일 템플릿


아이콘 템플릿 ("IconWithBadge" 템플릿이 라고도 함)을 사용 하면 타일의 중앙에 작은 이미지를 표시할 수 있습니다. Windows 10은 휴대폰 및 태블릿/데스크톱 모두에서 템플릿을 지원 합니다.

![소형 및 중간 메일 타일](images/iconic-template-mail-2sizes.png)

### <a name="how-to-create-an-iconic-tile"></a>아이콘 타일을 만드는 방법

다음 단계는 Windows 10에 대 한 아이콘 타일을 만들기 위해 알아야 할 모든 사항을 다룹니다. 높은 수준에서 아이콘 image 자산이 필요 하 고, 아이콘 템플릿을 사용 하 여 타일에 알림을 보내고, 마지막으로 타일에 표시 되는 숫자를 제공 하는 배지 알림을 보냅니다.

![아이콘 타일의 개발자 흐름](images/iconic-template-dev-flow.png)

**1 단계: PNG 형식으로 이미지 자산 만들기**

타일에 대 한 아이콘 자산을 만들고 다른 자산과 함께 프로젝트 리소스에 추가 합니다. 200 x 200 픽셀 아이콘을 만듭니다 .이 아이콘은 휴대폰 및 데스크톱에서 소형 및 중간 타일 모두에 적합 합니다. 최상의 사용자 환경을 제공 하려면 각 크기에 대 한 아이콘을 만듭니다. 이러한 자산에는 패딩이 필요 하지 않습니다. 아래 이미지에서 크기 조정 세부 정보를 참조 하세요.

PNG 형식 및 투명도를 사용 하 여 아이콘 자산을 저장 합니다. Windows Phone에서 투명 하지 않은 모든 픽셀은 흰색 (RGB 255, 255, 255)으로 표시 됩니다. 일관성과 단순성을 위해 바탕 화면 아이콘에도 흰색을 사용 합니다.

태블릿, 랩톱 및 데스크톱의 Windows 10은 사각형 아이콘 자산만 지원 합니다. 전화는 넓은 범위의 사각형 및 자산을 모두 지원 합니다. 너비는 최대 2:3 너비입니다. 높이 비율은 휴대폰 아이콘과 같은 이미지에 유용 합니다.

![휴대폰 및 데스크톱의 작은 및 중간 타일에 대 한 아이콘 크기 조정](images/iconic-template-sizing-info.png)

![배지를 포함 하거나 포함 하지 않는 자산의 크기 조정](images/assetguidance24.png)

정사각형 자산의 경우 컨테이너 내에서 자동 가운데 맞춤을 수행 합니다.

![배지를 포함 하거나 포함 하지 않고 사각형 자산 크기 조정](images/assetguidance25.png)

정사각형이 아닌 자산의 경우 컨테이너의 너비/높이에 대 한 자동 가로/세로 가운데 맞춤 및 맞추기가 발생 합니다.

![비 사각형 자산 크기 조정 (배지 포함/제외)](images/assetguidance26a.png)

![비 사각형 자산 크기 조정 (배지 포함/제외)](images/assetguidance26b.png)

**2 단계: 기본 타일 만들기**

기본 및 보조 타일 모두에서 아이콘 템플릿을 사용할 수 있습니다. 보조 타일에서이를 사용 하는 경우 먼저 보조 타일을 만들거나 이미 고정 된 보조 타일을 사용 해야 합니다. 기본 타일은 암시적으로 고정 되며 항상 알림을 보낼 수 있습니다.

**3 단계: 타일에 알림 보내기**

이 단계는 알림을 로컬로 보낼지 아니면 서버 푸시를 통해 보낼지에 따라 달라질 수 있지만 전송 하는 XML 페이로드는 동일 하 게 유지 됩니다. 로컬 타일 알림을 보내려면 타일에 대 한 [**TileUpdater**](/uwp/api/Windows.UI.Notifications.TileUpdater) (기본 또는 보조 타일)을 만든 다음 아래와 같이 아이콘 타일 템플릿을 사용 하는 타일에 알림을 보냅니다. 또한 [적응 타일 템플릿을](create-adaptive-tiles.md)사용 하 여 넓은 및 대규모 타일 크기에 대 한 바인딩을 포함 하는 것이 좋습니다.

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

이 아이콘 타일 템플릿 XML 페이로드는 1 단계에서 만든 이미지를 가리키는 이미지 요소를 사용 합니다. 이제 타일은 아이콘 옆에 배지를 표시할 준비가 되었습니다. 모든 것은 배지 알림을 보내는 것입니다.

**4 단계: 타일에 배지 알림 보내기**

3 단계와 마찬가지로이 단계는 알림이 로컬에서 전송 되는지 서버 푸시를 통해 전송 되는지에 따라 달라질 수 있지만 전송 하는 XML 페이로드는 동일 하 게 유지 됩니다. 로컬 배지 알림을 보내려면 타일(기본 타일 또는 보조 타일)에 대한 [**BadgeUpdater**](/uwp/api/Windows.UI.Notifications.BadgeUpdater)를 만든 다음 원하는 값이 포함된 배지 알림을 보내거나 배지를 지웁니다.

XML 페이로드에 대한 샘플 코드는 다음과 같습니다.

```xml
<badge value="2"/>
```

타일의 배지는 그에 따라 업데이트 됩니다.

**5 단계: 모두 함께 배치**

다음 이미지는 다양 한 Api 및 페이로드가 아이콘 타일 템플릿의 각 측면과 연결 되는 방법을 보여 줍니다. 이러한 바인딩 요소를 포함 하는 [타일 알림](/previous-versions/windows/apps/hh779724(v=win.10)) 은 &lt; &gt; 아이콘 템플릿과 이미지 자산을 지정 하는 데 사용 됩니다. [배지 알림은](/previous-versions/windows/apps/hh779719(v=win.10)) 숫자 값을 지정 하 고 타일 속성은 타일의 표시 이름, 색 등을 제어 합니다.

![아이콘 타일 템플릿과 연결 된 api 및 페이로드](images/iconic-template-properties-info.png)

## <a name="photos-tile-template"></a>사진 타일 템플릿


사진 타일 템플릿을 사용 하면 라이브 타일에 사진 슬라이드 쇼를 표시할 수 있습니다. 템플릿은 작은를 포함 하 여 모든 타일 크기에서 지원 되며 각 타일 크기에서 동일 하 게 동작 합니다. 아래 예제에서는 사진 템플릿을 사용 하는 중형 타일의 5 개 프레임을 보여 줍니다. 템플릿에는 선택 된 사진을 순환 하 고 무한 반복 하는 확대/축소 및 교차 페이드 효과가 있습니다.

![사진 타일 템플릿을 사용 하 여 이미지 슬라이드 쇼](images/photo-tile-template-image01.jpg)

### <a name="how-to-use-the-photos-template"></a>사진 템플릿 사용 방법

[알림 라이브러리](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)를 설치한 경우 사진 템플릿을 쉽게 사용할 수 있습니다. 원시 XML을 사용할 수 있지만 라이브러리를 사용 하는 것이 좋습니다. 이렇게 하면 유효한 XML 또는 XML 이스케이프 콘텐츠를 생성 하는 것에 대해 걱정할 필요가 없습니다.

Windows Phone 슬라이드 쇼에서 최대 9 개의 사진을 표시 합니다. 태블릿, 노트북 및 데스크톱은 최대 12 개까지 표시 됩니다.

타일 알림을 보내는 방법에 대 한 자세한 내용은 [알림 보내기 문서](index.md)를 참조 하세요.


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

## <a name="people-tile-template"></a>사람 타일 템플릿


Windows 10의 사용자 앱은 타일의 가로 또는 세로 방향으로 슬라이드를 표시 하는 원으로 이미지 컬렉션을 표시 하는 특별 한 타일 템플릿을 사용 합니다. 이 타일 템플릿은 Windows 10 빌드 10572부터 사용할 수 있으며, 누구나 앱에서 사용 하기 시작 합니다.

사람 타일 템플릿은 다음 크기의 타일에서 작동 합니다.

**중간 타일** (TileMedium)

![보통 사용자 타일](images/people-tile-medium.png)

 

**넓은 타일** (TileWide)

![wide 인물 타일](images/people-tile-wide.png)

 

**큼 타일 (데스크톱 전용)** (TileLarge)

![많은 사람 타일](images/people-tile-large.png)

 

[알림 라이브러리](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)를 사용 하는 경우 사용자 타일 템플릿만 사용 하 여 *TileBinding* 콘텐츠에 대 한 새 *TileBindingContentPeople* 개체를 만들어야 합니다. *TileBindingContentPeople* 클래스에는 이미지를 추가 하는 images 속성이 있습니다.

원시 XML을 사용 하는 경우 *힌트 프레젠테이션을* "사용자"로 설정 하 고 이미지를 바인딩 요소의 자식으로 추가 합니다.

다음 c # 코드 샘플에서는 사용자가 [알림 라이브러리](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)를 사용 하 고 있다고 가정 합니다.

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

최상의 사용자 환경을 위해 각 타일 크기에 대해 다음과 같은 사진 수를 제공 하는 것이 좋습니다.

-   중간 타일: 9 개 사진
-   넓은 타일: 15 개 사진
-   넓은 타일: 20 개 사진

이러한 사진을 포함 하는 경우 몇 개의 빈 원을 허용 하는 것이 좋습니다. 즉, 타일이 너무 시각적으로 사용 되지 않습니다. 사진 수를 자유롭게 조정 하 여 가장 잘 작동 하는 모양을 확인 하세요.

알림을 보내려면 [알림 배달 방법 선택](choosing-a-notification-delivery-method.md)을 참조 하세요.

## <a name="related-topics"></a>관련 항목


* [GitHub의 전체 코드 샘플](https://github.com/WindowsNotifications/quickstart-people-tile-template)
* [알림 라이브러리](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)
* [타일, 배지 및 알림](index.md)
* [적응형 타일 만들기](create-adaptive-tiles.md)
* [타일 콘텐츠 스키마](../tiles-and-notifications/tile-schema.md)
 

 