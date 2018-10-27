---
author: mijacobs
Description: Use the ParallaxView control to add depth and movement to your app.
title: ParallaxView 컨트롤을 사용하여 앱에 깊이 및 동작을 추가하는 방법입니다.
ms.assetid: ''
label: Parallax View
template: detail.hbs
ms.author: mijacobs
ms.date: 08/9/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: abarlow
design-contact: conrwi
dev-contact: stpete
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 987202969f22a5b1b34efc9089ea7f7f08282d8c
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5702813"
---
# <a name="parallax"></a>시차

시차는 뷰어에 가까이 있는 항목이 배경의 항목보다 빠르게 움직이는 시각 효과입니다. 시차는 깊이감, 원근감 및 운동성을 만듭니다. UWP 앱에서 ParallaxView 컨트롤을 사용하여 시차 효과를 만들 수 있습니다.  

> **중요 API**: [ParallaxView 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview), [VerticalShift 속성](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.VerticalShift), [HorizontalShift 속성](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.HorizontalShift)

## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/ParallaxView">앱을 열고 작동 중인 ParallaxView를 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML 컨트롤 갤러리 앱 다운로드(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">소스 코드 다운로드(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="parallax-and-the-fluent-design-system"></a>시차 및 Fluent 디자인 시스템

 Fluent 디자인 시스템을 사용하면 조명, 깊이, 움직임, 재질 및 배율이 통합된 선명한 현대식 UI를 만들 수 있습니다. 시차는 앱에 동작, 깊이, 배율을 추가하는 Fluent 디자인 시스템 구성 요소입니다. 자세히 알아보려면 [UWP용 Fluent 디자인 개요](../fluent-design-system/index.md)를 확인하십시오.

## <a name="how-it-works-in-a-user-interface"></a>사용자 인터페이스에서 작동하는 방식

UI에서 UI가 스크롤하거나 이동할 때 여러 개체를 서로 다른 속도로 움직여서 시차 효과를 만들 수 있습니다. <!-- Parallax is an important tool in adding depth to applications along with other techniques like transition animations, perspective tilt, and layering. --> 시험을 보이기 위해 콘텐츠의 두 계층인 목록과 배경 이미지를 살펴보겠습니다.  목록은 배경 이미지 맨 위에 있으며 이미 목록이 뷰어와 좀 더 가까이 있는 것 같은 착시 효과를 줍니다.  이제 시차 효과를 얻기 위해 가까이에 있는 개체를 멀리 있는 개체보다 “빠르게” 움직여야 합니다.  사용자가 인터페이스를 스크롤할 때 목록이 배경 이미지보다 빠르게 움직이며, 이로 인해 깊이감이 생성됩니다.

 ![목록 및 배경 이미지를 사용한 시차의 예](images/_Parallax_v2.gif)

 
## <a name="using-the-parallaxview-control-to-create-a-parallax-effect"></a>ParallaxView 컨트롤을 사용하여 시차 효과를 만들려면

시차 효과를 만들려면 [ParallaxView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview) 컨트롤을 사용합니다. 이 컨트롤은 목록 같은 전경 요소의 스크롤 위치를 이미지 같은 배경 요소와 연결합니다. 사용자가 전경 요소를 스크롤할 때 배경 요소에 애니메이션 효과를 적용하여 시차 효과를 만듭니다. 

ParallaxView 컨트롤을 사용하려면 Source 요소를 제공하고, 배경 요소를 제공하고, [VerticalShift](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.VerticalShift)(세로 스크롤에 대한) 및/또는 [HorizontalShift](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.HorizontalShift)(가로 스크롤에 대한) 속성을 0보다 큰 값으로 설정합니다. 
* Source 속성은 전경 요소에 대한 참조를 사용합니다. 시차 효과가 발생하려면 전경은 [ScrollViewer](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) 또는 ScrollViewer가 포함된 요소여야 합니다(예: [ListView](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.listview) 또는 [RichTextBox](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.RichEditBox)). 

* 배경 요소를 설정하려면 해당 요소를 ParallaxView 컨트롤의 자식으로 추가합니다. 배경 요소는 추가 UI 요소를 포함하고 있는 [이미지](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.Image) 또는 패널 같은 모든 [UIElement](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.uielement)일 수 있습니다. 

시차 효과를 만들려면 ParallaxView가 전경 요소 뒤에 있어야 합니다. [그리드](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.grid) 및 [캔버스](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.canvas) 패널을 사용하여 계층 항목을 서로 맨 위에 배치하면 ParallaxView 컨트롤과 함께 원활하게 작동합니다.  

이 예에서는 목록에 대한 시차 효과를 만듭니다.
 
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

이미지가 시차 작업에 대해 잘 작동하도록 ParallaxView가 이미지 크기를 자동으로 조정하므로 보기의 이미지 스크롤에 대해 걱정할 필요가 없습니다.

## <a name="customizing-the-parallax-effect"></a>시차 효과 사용자 지정 

VerticalShift 및 HorizontalShift 속성을 사용하여 시차 효과의 수준을 제어할 수 있습니다.

* VerticalShift 속성은 전체 시차 작업 중에 배경이 세로 방향으로 얼마나 멀리 이동하는지를 지정합니다. 값 0은 배경이 전혀 이동 하지 않습니다.
* HorizontalShift 속성은 전체 시차 작업 중에 배경이 가로 방향으로 얼마나 멀리 이동하는지를 지정합니다. 값 0은 배경이 전혀 이동 하지 않습니다.

값이 클수록 보다 극적인 효과를 만듭니다. 

시차를 사용자 지정하는 방법의 전체 목록은 ParallaxView 클래스를 참조하세요. 

## <a name="dos-and-donts"></a>권장 사항 및 금지 사항

- 배경 이미지가 포함된 목록에는 시차 사용
- ListViewItems에 이미지가 있는 경우 ListViewItems에 시차 사용을 고려
- 아무 곳에나 사용하지 말 것. 시차를 지나치게 많이 사용하면 효과가 반감될 수 있음

## <a name="get-the-sample-code"></a>샘플 코드 다운로드

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 대화형 형식의 모든 XAML 컨트롤을 보여줍니다.

## <a name="related-articles"></a>관련 문서

- [ParallaxView 클래스](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview) 
- [UWP용 Fluent 디자인](../fluent-design-system/index.md)
- [시스템의 과학: Fluent 디자인 및 깊이](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
