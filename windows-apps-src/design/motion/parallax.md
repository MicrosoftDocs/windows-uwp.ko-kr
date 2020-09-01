---
title: 시차를 사용하여 앱에 깊이와 움직임을 추가합니다.
description: UWP 앱에서 ParallaxView 컨트롤을 사용 하 여 뷰어에서 항목이 배경의 항목 보다 빠르게 이동 하는 시각적 효과를 만드는 방법에 대해 알아봅니다.
ms.assetid: ''
label: Parallax View
template: detail.hbs
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: abarlow
design-contact: conrwi
dev-contact: stpete
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 5eac1b5d95dff4887258278f9ff700adaf663194
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175547"
---
# <a name="parallax"></a>시차

시차는 뷰어에 가까운 항목이 배경의 항목 보다 빠르게 이동 하는 시각적 효과입니다. 시차는 깊이, 원근 및 이동의 느낌을 만듭니다. UWP 앱에서 ParallaxView 컨트롤을 사용 하 여 시차 효과를 만들 수 있습니다.  

> **WINDOWS UI 라이브러리 api:** [ParallaxView 클래스](/uwp/api/Microsoft.UI.Xaml.Controls.Parallaxview), [VerticalShift 속성](/uwp/api/Microsoft.UI.Xaml.Controls.Parallaxview.VerticalShift), [HorizontalShift 속성](/uwp/api/Microsoft.UI.Xaml.Controls.Parallaxview.HorizontalShift)
>
> **플랫폼 api**: [ParallaxView 클래스](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview), [VerticalShift 속성](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.VerticalShift), [HorizontalShift 속성](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.HorizontalShift)

## <a name="examples"></a>예

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치 되어 있는 경우 여기를 클릭 하 여 <a href="xamlcontrolsgallery:/item/ParallaxView">앱을 열고 ParallaxView in 작업을 참조</a>하세요.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML Controls Gallery 앱 가져오기(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">소스 코드 가져오기(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="parallax-and-the-fluent-design-system"></a>시차 및 흐름 디자인 시스템

 Fluent 디자인 시스템을 사용하면 조명, 깊이, 움직임, 재질 및 배율이 통합된 선명한 현대식 UI를 만들 수 있습니다. 시차는 응용 프로그램에 동작, 깊이 및 크기 조정을 추가 하는 흐름 디자인 시스템 구성 요소입니다. 자세한 내용은 [Fluent Design 개요](/windows/apps/fluent-design-system)를 참조하세요.

## <a name="how-it-works-in-a-user-interface"></a>사용자 인터페이스에서 작동 방식

Ui에서 UI를 스크롤하거나 계획 때 다른 개체를 다른 속도로 이동 하 여 시차 효과를 만들 수 있습니다. <!-- Parallax is an important tool in adding depth to applications along with other techniques like transition animations, perspective tilt, and layering. --> 이를 설명 하기 위해 두 계층의 콘텐츠, 목록 및 배경 이미지를 살펴보겠습니다.  목록은 배경 이미지의 맨 위에 배치 되며,이는 목록이 뷰어에 가까이 있을 수 있음을 보여 주는 효과를 이미 제공 합니다.  이제 시차 효과를 얻기 위해 더 멀리 떨어져 있는 개체 보다 "빠르게" 이동 하기 위해 가장 가까운 개체를 원합니다.  사용자가 인터페이스를 스크롤하면 목록이 배경 이미지 보다 빠른 속도로 이동 하 여 깊이의 환상 효과를 만듭니다.

 ![목록 및 배경 이미지를 사용한 시차의 예](images/_Parallax_v2.gif)

 
## <a name="using-the-parallaxview-control-to-create-a-parallax-effect"></a>ParallaxView 컨트롤을 사용 하 여 시차 효과 만들기

시차 효과를 만들려면 [ParallaxView](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview) 컨트롤을 사용 합니다. 이 컨트롤은 목록과 같은 전경 요소의 스크롤 위치를 이미지와 같은 배경 요소에 연결 합니다. 전경 요소를 스크롤하면 배경 요소에 애니메이션 효과를 적용 하 여 시차 효과를 만듭니다. 

ParallaxView 컨트롤을 사용 하려면 소스 요소, 배경 요소를 제공 하 고 [VerticalShift](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.VerticalShift) (세로 스크롤의 경우) 및/또는 [HorizontalShift](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.HorizontalShift) (가로 스크롤) 속성을 0 보다 큰 값으로 설정 합니다. 
* Source 속성은 전경 요소에 대 한 참조를 사용 합니다. 시차 효과가 발생 하려면 전경을 [ScrollViewer](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) 또는 [ListView](/uwp/api/windows.ui.xaml.controls.listview) 나 [RichTextBox](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox)와 같은 ScrollViewer를 포함 하는 요소 여야 합니다. 

* 배경 요소를 설정 하려면 해당 요소를 ParallaxView 컨트롤의 자식으로 추가 합니다. 배경 요소는 [이미지](/uwp/api/Windows.UI.Xaml.Controls.Image) 또는 패널 등의 추가 UI 요소를 포함 하는 모든 [UIElement](/uwp/api/windows.ui.xaml.uielement)일 수 있습니다. 

시차 효과를 만들려면 ParallaxView가 전경 요소 뒤에 있어야 합니다. [Grid](/uwp/api/windows.ui.xaml.controls.grid) 및 [Canvas](/uwp/api/windows.ui.xaml.controls.canvas) 패널을 사용 하면 항목을 서로 계층화 하 여 ParallaxView 컨트롤에서 잘 작동 합니다.  

이 예에서는 목록에 대해 시차 효과를 만듭니다.
 
```xaml
<Grid>
    <ParallaxView Source="{x:Bind ForegroundElement}" VerticalShift="50"> 
    
        <!-- Background element --> 
        <Image x:Name="BackgroundImage" Source="Assets/turntable.png"
               Stretch="UniformToFill"/>
    </ParallaxView>
    
    <!-- Foreground element -->
    <ListView x:Name="ForegroundElement">
       <x:String>Item 1</x:String> 
       <x:String>Item 2</x:String> 
       <x:String>Item 3</x:String> 
       <x:String>Item 4</x:String> 
       <x:String>Item 5</x:String>     
       <x:String>Item 6</x:String> 
       <x:String>Item 7</x:String> 
       <x:String>Item 8</x:String> 
       <x:String>Item 9</x:String> 
       <x:String>Item 10</x:String>     
       <x:String>Item 11</x:String> 
       <x:String>Item 13</x:String> 
       <x:String>Item 14</x:String> 
       <x:String>Item 15</x:String> 
       <x:String>Item 16</x:String>     
       <x:String>Item 17</x:String> 
       <x:String>Item 18</x:String> 
       <x:String>Item 19</x:String> 
       <x:String>Item 20</x:String> 
       <x:String>Item 21</x:String>        
    </ListView>
</Grid>
```    

ParallaxView는 이미지의 크기를 자동으로 조정 하 여 시차 작업에 사용할 수 있도록 합니다. 따라서 뷰에서 이미지 스크롤을 걱정할 필요가 없습니다.

## <a name="customizing-the-parallax-effect"></a>시차 효과 사용자 지정 

VerticalShift 및 HorizontalShift 속성을 사용 하 여 시차 효과의 정도를 제어할 수 있습니다.

* VerticalShift 속성은 전체 시차 작업 중 배경을 세로 방향으로 이동 하는 정도를 지정 합니다. 값이 0 이면 배경이 전혀 이동 하지 않음을 의미 합니다.
* HorizontalShift 속성은 전체 시차 작업 중에 배경을 가로로 이동 하는 정도를 지정 합니다. 값이 0 이면 배경이 전혀 이동 하지 않음을 의미 합니다.

값이 클수록 더 극적으로 효과를 만듭니다. 

시차를 사용자 지정 하는 방법의 전체 목록은 ParallaxView 클래스를 참조 하세요. 

## <a name="dos-and-donts"></a>권장 사항 및 금지 사항

- 배경 이미지가 있는 목록에서 시차 사용
- ListViewItems에 이미지가 포함 된 경우 ListViewItems에서 시차를 사용 하는 것이 좋습니다.
- 어디에서 나 사용 하지 마세요. 과도 하 게 영향을 줄 수 있습니다.

## <a name="related-articles"></a>관련된 문서

- [ParallaxView 클래스](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview) 
- [UWP용 흐름 디자인](/windows/apps/fluent-design-system)
- [시스템의 과학: 흐름 디자인 및 깊이](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)