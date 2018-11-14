---
author: Jwmsft
Description: Learn how to integrate images into your app, including how to use the APIs of the two main XAML classes, Image and ImageBrush.
title: 이미지 및 이미지 브러시
ms.assetid: CEA8780C-71A3-4168-A6E8-6361CDFB2FAF
label: Images and image brushes
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7c0fcd158dac77b3b3322167b82131e51f62390f
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6664940"
---
# <a name="images-and-image-brushes"></a>이미지 및 이미지 브러시

이미지를 표시하려면 **Image** 개체 또는 **ImageBrush** 개체를 사용할 수 있습니다. Image 개체는 이미지를 렌더링하고 ImageBrush 개체는 이미지에 다른 개체를 그립니다. 

> **중요 API**: [Image 클래스](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.aspx), [Source 속성](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.source.aspx), [ImageBrush 클래스](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imagebrush.aspx), [ImageSource 속성](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imagebrush.imagesource.aspx)

## <a name="are-these-the-right-elements"></a>올바른 요소인가요?
앱에 독립 실행형 이미지를 표시하려면 **Image** 요소를 사용합니다.

다른 개체에 이미지를 적용하려면 **ImageBrush**를 사용합니다. ImageBrush는 텍스트의 장식 효과, 컨트롤 또는 레이아웃 컨테이너의 배경 등에 사용됩니다.

## <a name="examples"></a>예

<table>
<th align="left">XAML 컨트롤 갤러리<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML 컨트롤 갤러리</strong> 앱이 설치된 경우 여기를 클릭하여 <a href="xamlcontrolsgallery:/item/Image">앱을 열고 작동 중인 이미지를 확인</a>합니다.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML 컨트롤 갤러리 앱 다운로드(Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">소스 코드 다운로드(GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-an-image"></a>이미지 만들기

### <a name="image"></a>이미지
다음 예에서는 [Image](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.aspx) 개체를 사용하여 이미지를 만드는 방법을 보여 줍니다.


```XAML
<Image Width="200" Source="sunset.jpg" />
```

다음은 렌더링된 Image 개체입니다.

![Image 요소의 예](images/Image_Licorice.jpg)

다음 예제에서 [Source](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.source.aspx) 속성은 표시할 이미지의 위치를 지정합니다. 절대 URL을 지정 하 여 소스를 설정할 수 있습니다 (예를 들어 http://contoso.com/myPicture.jpg) 앱 패키징 구조에 상대적인 URL을 지정 하 여 합니다. 이 예제에서는 "licorice.jpg" 이미지 파일을 프로젝트의 루트 폴더에 넣고 이 이미지 파일을 내용으로 포함하는 프로젝트 설정을 선언합니다.

### <a name="imagebrush"></a>ImageBrush

[ImageBrush](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imagebrush.aspx) 개체를 사용하면 [Brush](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.brush.aspx) 개체를 가지는 영역을 이미지로 그릴 수 있습니다. 예를 들어 [Ellipse](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.shape.fill.aspx)의 [Fill](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.ellipse.aspx) 속성 또는 [Canvas](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.background.aspx)의 [Background](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.aspx) 속성 값에 ImageBrush를 사용할 수 있습니다.

다음 예제에서는 ImageBrush를 사용하여 Ellipse를 그리는 방법을 보여 줍니다.

```XAML
<Ellipse Height="200" Width="300">
   <Ellipse.Fill>
     <ImageBrush ImageSource="sunset.jpg" />
   </Ellipse.Fill>
</Ellipse>
```

다음은 ImageBrush로 그린 Ellipse입니다.

![ImageBrush로 그린 Ellipse입니다.](images/Image_ImageBrush_Ellipse.jpg)

### <a name="stretch-an-image"></a>이미지 확대

**Image**의 [Width](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.width.aspx) 또는 [Height](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.height.aspx) 값을 설정하지 않는 경우 이미지는 **Source**에서 지정한 이미지 크기로 표시됩니다. **Width** 및 **Height**를 설정하면 이미지가 표시되는 컨테이너 사각형 영역이 만들어집니다. 이미지가 이 컨테이너 영역을 채우는 방법을 지정하려면 [Stretch](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.stretch.aspx) 속성을 사용합니다. Stretch 속성에는 [Stretch](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.stretch.aspx) 열거형에서 정의하는 다음 값을 사용할 수 있습니다.

-   **None**: 이미지가 출력 크기를 채우기 위해 확대되지 않습니다. 이 Stretch 설정은 주의해서 사용해야 합니다. 원본 이미지가 컨테이너 영역보다 큰 경우 이미지가 잘립니다. 이는 일반적으로 바람직하지 않은데, 의도적 [Clip](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.clip.aspx)을 사용할 때처럼 뷰포트를 제어할 수 없기 때문입니다.
-   **Uniform**: 이미지가 출력 크기에 맞게 크기 조정됩니다. 콘텐츠의 가로 세로 비율은 유지됩니다. 기본값입니다.
-   **UniformToFill**: 이미지가 원래 가로 세로 비율은 유지하고 출력 영역을 완전히 채우도록 크기 조정됩니다.
-   **Fill**: 이미지가 출력 크기에 맞게 크기 조정됩니다. 콘텐츠의 높이와 너비가 따로 조정되므로 이미지의 원래 가로 세로 비율은 유지될 수 없습니다. 즉, 이미지가 출력 영역을 완전히 채우기 위해 왜곡될 수 있습니다.

![확대 설정의 예](images/Image_Stretch.jpg)

### <a name="crop-an-image"></a>이미지 자르기

[Clip](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.clip.aspx) 속성을 사용하여 이미지 출력에서 영역을 자를 수 있습니다. Clip 속성은 [Geometry](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.geometry.aspx)로 설정합니다. 현재 직사각형이 아닌 자르기는 지원되지 않습니다.

다음 예제에서는 [RectangleGeometry](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.rectanglegeometry.aspx)를 이미지의 잘라내기 영역으로 사용하는 방법을 보여 줍니다. 이 예제에서는 Height가 200인 **Image** 개체를 정의합니다. **RectangleGeometry**는 표시될 이미지 영역에 대해 사각형을 정의합니다. [Rect](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.rectanglegeometry.rect.aspx) 속성은 "25,25,100,150"으로 설정됩니다. 즉, 위치 "25,25"에서 시작하며 너비가 100이고 높이가 150인 직사각형을 정의합니다. 직사각형 영역 내에 있는 이미지 부분만 표시됩니다.

```xaml
<Image Source="sunset.jpg" Height="200">
    <Image.Clip>
        <RectangleGeometry Rect="25,25,100,150" />
    </Image.Clip>
</Image>
```

다음은 검은색 배경의 잘린 이미지입니다.

![RectangleGeometry로 잘린 이미지 개체입니다.](images/Image_Cropped.jpg)

### <a name="apply-an-opacity"></a>불투명도 적용

이미지에 [Opacity](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.opacity.aspx)를 적용하여 이미지가 반투명하게 렌더링되도록 할 수 있습니다. 불투명도 값은 0.0에서 1.0 사이이며, 1.0은 완전 불투명, 0.0은 완전 투명을 나타냅니다. 다음 예제에서는 Image에 불투명도 0.5를 적용하는 방법을 보여 줍니다.

```xaml
<Image Height="200" Source="sunset.jpg" Opacity="0.5" />
```

다음은 불투명도 0.5로 렌더링된 이미지이며 부분 불투명도를 통해 검정 배경이 보입니다.

![불투명도가 .5인 이미지 개체](images/Image_Opacity.jpg)

### <a name="image-file-formats"></a>이미지 파일 형식

**Image** 및 **ImageBrush**에는 다음과 같은 이미지 파일 형식이 표시될 수 있습니다.

-   JPEG(Joint Photographic Experts Group)
-   PNG(이동식 네트워크 그래픽)
-   비트맵(BMP)
-   GIF(Graphics Interchange Format)
-   TIFF(Tagged Image File Format)
-   JPEG XR
-   아이콘(ICO)

[Image](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.aspx), [BitmapImage](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.bitmapimage.aspx) 및 [BitmapSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.bitmapsource.aspx)용 API에는 미디어 형식의 인코드 및 디코드 전용 메서드가 없습니다. 모든 인코드 및 디코드 작업이 기본 제공되며 인코드 또는 디코드의 측면을 로드 이벤트에 대한 이벤트 데이터의 일부로 표시할 뿐입니다. 앱이 이미지 변환이나 조작을 수행하고 있는 경우 사용할 수 있는 이미지 인코드나 디코드로 특수한 작업을 수행하려면 [Windows.Graphics.Imaging](https://msdn.microsoft.com/library/windows/apps/xaml/windows.graphics.imaging.aspx) 네임스페이스에서 사용할 수 있는 API를 사용해야 합니다. 이러한 API는 Windows의 WIC(Windows 이미징 구성 요소)에서도 지원됩니다.

Windows 10 버전 1607부터 **Image** 요소는 애니메이션 GIF 이미지를 지원합니다. 이미지 **Source**로 **BitmapImage**를 사용하는 경우 BitmapImage API에 액세스하여 애니메이션 GIF 이미지의 재생을 제어할 수 있습니다. 자세한 내용은 [BitmapImage](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.bitmapimage.aspx) 클래스 페이지의 “설명”을 참조하세요.

> **참고**&nbsp;&nbsp;애니메이션 GIF 지원은 Windows 10 버전 1607용으로 컴파일되고 1607 버전 이상에서 실행하는 앱일 경우에 사용할 수 있습니다. 앱이 이전 버전용으로 컴파일되거나 이전 버전에서 실행되는 경우 GIF의 첫 프레임은 애니메이션이 적용되지 않고 표시됩니다.

앱 리소스 및 앱에서 이미지 리소스를 패키지하는 방법에 대한 자세한 내용은 [앱 리소스 정의](https://msdn.microsoft.com/library/windows/apps/xaml/hh965321)를 참조하세요.

### <a name="writeablebitmap"></a>WriteableBitmap

[WriteableBitmap](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.writeablebitmap.aspx)은 수정할 수 있고 WIC의 기본 파일 기반 디코딩을 사용하지 않는 [BitmapSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.bitmapsource.aspx)를 제공합니다. 따라서 이미지를 동적으로 변경하고 업데이트된 이미지를 다시 렌더링할 수 있습니다. **WriteableBitmap**의 버퍼 콘텐츠를 정의하려면 [PixelBuffer](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.writeablebitmap.pixelbuffer.aspx) 속성을 사용하여 버퍼에 액세스하고 스트림이나 언어별 버퍼 형식을 사용하여 채웁니다. 예제 코드는 [WriteableBitmap](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.writeablebitmap.aspx)을 참조하세요.

### <a name="rendertargetbitmap"></a>RenderTargetBitmap

이 [RenderTargetBitmap](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.rendertargetbitmap.aspx) 클래스는 실행 중인 앱에서 XAML UI 트리를 캡처한 다음 비트맵 이미지 소스를 나타낼 수 있습니다. 캡처 후에 해당 이미지 소스를 앱의 다른 부분에 적용하거나, 사용자가 리소스 또는 앱 데이터로 저장하거나, 다른 시나리오에 사용할 수 있습니다. 한 가지 특히 유용한 시나리오로, 탐색 체계를 위한 XAML 페이지의 런타임 미리 보기를 만드는 것이 있습니다(예: [Hub](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.hub.aspx) 컨트롤에서 이미지 링크 제공). **RenderTargetBitmap**은 캡처된 이미지에 나타나는 콘텐츠에 제한을 두지 않습니다. 자세한 내용은 [RenderTargetBitmap](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.rendertargetbitmap.aspx)의 API 참조 항목을 확인하세요.

### <a name="image-sources-and-scaling"></a>이미지 원본 및 크기 조정

여러 가지 권장 크기로 이미지 원본을 만들어 Windows에서 크기를 조정할 때 앱 모양이 멋지게 보이도록 합니다. **Image**에 **Source**를 지정할 때 현재 크기에 적합한 리소스를 자동으로 참조하는 명명 규칙을 사용할 수 있습니다. 명명 규칙의 특성 및 자세한 내용은 [빠른 시작: 파일 또는 이미지 리소스 사용](https://msdn.microsoft.com/library/windows/apps/xaml/hh965325)을 참조하세요.

크기 조정을 위해 디자인하는 방법에 대한 자세한 내용은 [레이아웃 및 크기 조정에 대한 UX 지침](https://msdn.microsoft.com/library/windows/apps/dn611863)을 참조하세요.

### <a name="image-and-imagebrush-in-code"></a>코드의 Image 및 ImageBrush

Image 및 ImageBrush 요소는 코드가 아닌 XAML을 사용하여 지정하는 것이 일반적입니다. 왜냐하면 이러한 요소는 XAML UI 정의의 일부로 디자인 도구에서 출력되는 경우가 자주 있기 때문입니다.

코드를 사용하여 Image 또는 ImageBrush를 정의하는 경우 기본 생성자를 사용한 다음 관련 원본 속성([Image.Source](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.source.aspx) 또는 [ImageBrush.ImageSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imagebrush.imagesource.aspx))을 설정합니다. 코드를 사용하여 설정할 때 원본 속성에는 [BitmapImage](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.bitmapimage.aspx)(URI 아님)가 필요합니다. 원본이 스트림이면 [SetSourceAsync](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync.aspx) 메서드를 사용하여 값을 초기화합니다. 소스가 **ms-appx** 또는 **ms-resource** 구성표를 사용하는 앱에 콘텐츠를 포함하는 URI이면 URI를 사용하는 [BitmapImage](https://msdn.microsoft.com/library/windows/apps/xaml/br243238.aspx) 생성자를 사용합니다. 또한 이미지 소스를 검색하거나 디코딩하는 데 타이밍 문제가 있는 경우 [ImageOpened](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imaging.bitmapimage.imageopened.aspx) 이벤트 처리를 고려할 수 있으며, 이미지 소스를 사용할 수 있을 때까지 표시할 대체 콘텐츠가 필요할 수 있습니다. 예제 코드는 [XAML 이미지 샘플](http://go.microsoft.com/fwlink/p/?linkid=238575)을 참조하세요.

> [!NOTE]
> 코드를 사용하여 이미지를 설정하면 현재 크기 및 문화권 한정자로 비정규화된 리소스에 액세스하는 데 자동 처리를 사용하거나 문화권 및 크기에 대한 한정자와 함께 [ResourceManager](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.resources.core.resourcemanager.aspx) 및 [ResourceMap](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.resources.core.resourcemap.aspx)을 사용하여 리소스를 직접 가져올 수 있습니다. 자세한 내용은 [리소스 관리 시스템](https://msdn.microsoft.com/library/windows/apps/xaml/jj552947.aspx)을 참조하세요.

## <a name="get-the-sample-code"></a>샘플 코드 다운로드

- [XAML 컨트롤 갤러리 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 대화형 형식의 모든 XAML 컨트롤을 보여줍니다.

## <a name="related-articles"></a>관련 문서

-   [오디오, 동영상 및 카메라](https://msdn.microsoft.com/windows/uwp/audio-video-camera/index)
-   [Image 클래스](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.aspx)
-   [ImageBrush 클래스](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imagebrush.aspx)