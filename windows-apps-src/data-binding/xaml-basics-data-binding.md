---
author: KarlErickson
title: 데이터 바인딩 만들기
description: 이 문서에서는 XAML에서 데이터 바인딩의 기본 사항에 대해 다룹니다.
keywords: XAML, UWP, 시작
ms.author: karler
ms.date: 08/30/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8000d9105481bc177eb2fc64646aec009fd80d36
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5820068"
---
# <a name="create-data-bindings"></a>데이터 바인딩 만들기

자리 표시자 이미지, "lorem ipsum" 상용 텍스트 및 아직 아무 작업도 수행하지 않은 컨트롤으로 가득한 멋진 UI를 디자인하고 구현했다고 가정합니다. 그런 다음 실제 데이터에 연결하여 디자인 프로토타입에서 실제 앱으로 변환하려고 합니다. 

이 자습서에서는 상용구를 데이터 바인딩으로 바꾸고 UI와 데이터 사이에 직접 연결을 만드는 방법을 알아봅니다. 또한 표시할 데이터의 형식을 지정하거나 변환하는 방법을 알아보고 UI와 데이터를 동기화 상태로 유지합니다. 이 자습서를 완료하면 XAML 및 C# 코드의 단순성 및 구성을 향상시켜 유지 관리 및 확장 작업을 더욱 쉽게 수행할 수 있습니다.

PhotoLab 샘플의 간소화된 버전부터 시작합니다. 이 시작 버전에는 전체 데이터 계층 및 기본 XAML 페이지 레이아웃이 포함되어 있으며, 코드를 더 쉽게 탐색할 수 있게 해 주는 여러 보조 기능은 생략하고 있습니다. 이 자습서에서는 전체적인 앱을 빌드하지 않으므로 최종 버전을 확인하여 사용자 지정 애니메이션 및 휴대폰 지원과 같은 기능을 살펴보세요. [Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab) 리포에서 루트 폴더 최종 버전을 찾을 수 있습니다. 

## <a name="prerequisites"></a>필수 조건

* [Visual Studio 2017 및 Windows 10 SDK 최신 버전](https://developer.microsoft.com/windows/downloads).

## <a name="part-0-get-the-code"></a>파트 0: 코드 다운로드
이 실습은 [xaml-basics-starting-points/data-binding](https://github.com/Microsoft/Windows-appsample-photo-lab/tree/master/xaml-basics-starting-points/data-binding) 폴더의 PhotoLab 샘플 리포지토리에서 시작됩니다. 리포지토리를 복제 또는 다운로드 한 후에는 Visual Studio 2017로 PhotoLab.sln을 열어 프로젝트를 편집할 수 있습니다.

PhotoLab 앱에는 두 개의 기본 페이지가 있습니다.

**MainPage.xaml:** 각 이미지 파일에 대한 정보와 함께 사진 갤러리 보기를 표시합니다.
![MainPage](../design/basics/images/xaml-basics/mainpage.png)

**DetailPage.xaml:** 하나의 사진을 선택한 후에 표시합니다. 플라이아웃 편집 메뉴를 사용하면 사진을 수정하고, 이름을 변경하고, 저장할 수 있습니다.
![DetailPage](../design/basics/images/xaml-basics/detailpage.png)

## <a name="part-1-replace-the-placeholders"></a>파트 1: 자리 표시자 바꾸기

여기에서는 데이터 템플릿 XAML에서 일회성 바인딩을 만들어 자리 표시자 콘텐츠 대신 실제 이미지와 이미지 메타데이터를 표시합니다. 

일회성 바인딩은 읽기 전용의 변경되지 않은 데이터를 위한 것이며, 성능이 뛰어나고 쉽게 만들 수 있어 **GridView** 및 **ListView** 컨트롤에 대규모 데이터 집합을 표시할 수 있게 해 줍니다. 

**일회성 바인딩으로 자리 표시자 바꾸기**

1. xaml-basics-starting-points\data-binding 폴더를 열고 PhotoLab.sln 파일을 시작합니다. 

2. 솔루션 플랫폼이 ARM이 아닌 x86 또는 x64로 설정되었는지 확인한 다음 앱을 실행합니다. 이는 바인딩이 추가되기 전에 UI 자리 표시자를 가진 앱의 상태를 보여 줍니다. 

    ![자리 표시자 이미지 및 텍스트가 있는 앱 실행](../design/basics/images/xaml-basics/gallery-with-placeholder-templates.png)

3. MainPage.xaml을 열고 **ImageGridView_DefaultItemTemplate**이라는 이름의 **DataTemplate**을 검색합니다. 데이터 바인딩을 사용하려면 이 템플릿을 업데이트합니다. 

    **이전:**
    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate">
    ```

    **x:Key** 값은 데이터 객체를 표시하기 위해 **ImageGridView**에서 이 템플릿을 선택하는 데 사용됩니다. 

4. **x:DataType** 값을 템플릿으로 추가합니다. 

    **이후:**
    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate" 
                  x:DataType="local:ImageFileInfo">
    ```

    **x:DataType**은 이것이 어떤 유형의 템플릿인지 나타냅니다. 이 경우에는 **ImageFileInfo** 클래스에 대한 템플릿입니다(여기서 "local:"은 파일 상단 근처의 xmlns 선언에 정의된 대로 로컬 네임스페이스를 나타냄).
    
    **x:DataType**은 옆에 나온 설명과 같이 데이터 템플릿에서 **x:Bind** 식을 사용할 때 필요합니다. 

5. **DataTemplate**에서 **ItemImage**라는 이름의 **Image** 요소를 찾고 표시된 대로 **Source** 값을 바꿉니다. 

    **이전:**
    ```xaml
    <Image x:Name="ItemImage" 
           Source="/Assets/StoreLogo.png" 
           Stretch="Uniform" />
    ```
    
    **이후:**
    ```xaml
    <Image x:Name="ItemImage" 
           Source="{x:Bind ImageSource}" 
           Stretch="Uniform" />
    ```
    
    **x:Name**은 XAML 요소를 식별하므로 XAML의 다른 위치와 코드 숨김에서 참조할 수 있습니다. 

    **x:Bind** 식은 **data-object** 속성에서 값을 가져와서 UI 속성에 값을 제공합니다. 템플릿에서, 표시된 속성은 **x:DataType**이 설정된 모든 속성입니다. 따라서 이 경우 데이터 소스는 **ImageFileInfo.ImageSource** 속성입니다. 
    
    > [!NOTE] 
    > **x:Bind** 값은 편집기에서 데이터 형식을 알 수 있게 하므로 **x:Bind** 식에서 속성 이름을 입력하는 대신 IntelliSense를 사용할 수 있습니다. 방금 붙여넣은 코드에서 시도해 봅니다. **x:Bind** 바로 뒤에 커서를 놓고 스페이스 바를 눌러 바인딩할 수 있는 속성 목록을 확인합니다.

6. 다른 UI 컨트롤의 값도 같은 방식으로 바꿉니다. (복사/붙여넣기 대신 IntelliSense를 사용해 보세요!)

    **이전:**
    ```xaml
    <TextBlock Text="Placeholder" ... />
    <StackPanel ... >
        <TextBlock Text="PNG file" ... />
        <TextBlock Text="50 x 50" ... />
    </StackPanel>
    <telerikInput:RadRating Value="3" ... />
    ```
    
    **이후:**
    ```xaml
    <TextBlock Text="{x:Bind ImageTitle}" ... />
    <StackPanel ... >
        <TextBlock Text="{x:Bind ImageFileType}" ... />
        <TextBlock Text="{x:Bind ImageDimensions}" ... />
    </StackPanel>
    <telerikInput:RadRating Value="{x:Bind ImageRating}" ... />
    ```

앱을 실행하여 지금까지의 모습을 확인합니다. 더 이상 자리 표시자가 없습니다! 제대로 시작한 것입니다. 

![자리 표시자 대신 실제 이미지와 텍스트가 있는 앱 실행](../design/basics/images/xaml-basics/gallery-with-populated-templates.png)

> [!Note]
> 추가로 실험하고 싶다면 데이터 템플릿에 새 TextBlock을 추가하고 x:Bind IntelliSense 방법을 사용하여 표시할 속성을 찾습니다. 

## <a name="part-2-use-binding-to-connect-the-gallery-ui-to-the-images"></a>파트 2: 바인딩을 사용하여 갤러리 UI를 이미지에 연결

여기에서는 XAML 페이지에 일회성 바인딩을 생성해서 갤러리 보기를 이미지 컬렉션에 연결하고, 코드 숨김에서 이 작업을 수행하는 기존 절차 코드를 대체합니다. 또한 **삭제** 단추를 생성해서 이미지를 컬렉션에서 제거할 때 갤러리 보기가 어떻게 변경되는지 확인할 수 있습니다. 동시에 이벤트 처리기에 이벤트를 바인딩하여 기존 이벤트 처리기에서 제공하는 것보다 더 유연하게 바인딩하는 방법을 알아봅니다. 

지금까지 다룬 모든 바인딩은 데이터 템플릿 내부에 있으며 **x:DataType** 값으로 표시된 클래스의 속성을 참조합니다. 페이지의 나머지 XAML은 어떻습니까? 

데이터 템플릿 외부의 **x:Bind** 식은 항상 페이지 자체에 바인딩됩니다. 즉, 사용자 지정 속성 및 해당 페이지의 다른 UI 컨트롤 속성을 포함하여 코드 숨김 또는 XAML에 선언한 내용을 참조할 수 있습니다.(**x:Name** 값을 가진 경우). 

PhotoLab 샘플에서 이와 같은 바인딩을 사용하면 코드 숨김 대신 이미지의 컬렉션에 직접 기본 **GridView** 컨트롤을 연결할 수 있습니다. 나중에 다른 예를 확인할 수 있습니다. 

**기본 GridView 컨트롤을 이미지 컬렉션에 바인딩**

1. MainPage.xaml.cs에서 **OnNavigatedTo** 메서드를 찾고 **ItemsSource**를 설정하는 코드를 제거합니다.

    **이전:**
    ```c#
    ImageGridView.ItemsSource = Images;
    ```

    **이후:**
    ```c#
    // Replaced with XAML binding:
    // ImageGridView.ItemsSource = Images;
    ```

2. MainPage.xaml에서 **ImageGridView**라는 이름의 **GridView**를 찾고 **ItemsSource** 특성을 추가합니다. 값을 얻으려면 코드 숨김에 구현된 **Images** 속성을 참조하는 **x:Bind** 식을 사용합니다. 

    **이전:**
    ```xaml
    <GridView x:Name="ImageGridView" 
    ```

    **이후:**
    ```xaml
    <GridView x:Name="ImageGridView" 
              ItemsSource="{x:Bind Images}" 
    ```

    **Images** 속성은 **ObservableCollection\<ImageFileInfo\>** 유형이므로, **GridView**에 표시된 개별 항목은 **ImageFileInfo** 유형입니다. 이는 파트 1에 설명된 **x:DataType** 값과 일치합니다. 

지금까지 살펴본 모든 바인딩은 일회성, 읽기 전용 바인딩입니다. 이 바인딩은 일반 **x:Bind** 식의 기본 동작입니다. 데이터는 초기화 시점에만 로드되므로 고성능 바인딩을 만들 수 있고, 대규모 데이터 집합의 여러 복잡한 보기를 완벽하게 지원할 수 있습니다. 

방금 추가한 **ItemsSource** 바인딩도 변경되지 않는 속성 값에 대한 일회성, 읽기 전용 바인딩이지만 중요한 차이점이 있습니다. **Images** 속성의 변경되지 않는 값은 여기에 표시된 대로 한 번 초기화된 컬렉션의 특정 단일 인스턴스입니다.

```Csharp
private ObservableCollection<ImageFileInfo> Images { get; }
    = new ObservableCollection<ImageFileInfo>();
```

**Images** 속성 값은 절대로 변경되지 않지만 속성이 **ObservableCollection\<T\>** 유형이기 때문에 컬렉션의 *콘텐츠*는 변경될 수 있으며, 바인딩은 자동으로 변경 사항을 확인하고 UI를 업데이트합니다. 

이를 테스트하기 위해 현재 선택된 이미지를 삭제하는 단추를 임시로 추가할 것입니다. 이미지를 선택하면 세부 정보 페이지로 이동하게 되므로 최종 버전에는 이 단추가 없습니다. 하지만 XAML이 페이지 생성자에서 초기화되므로 최종 PhotoLab 샘플에서 **ObservableCollection\<T\>** 의 동작은 여전히 중요하며(**InitializeComponent** 메서드 호출을 통함), **Images** 컬렉션은 나중에 **OnNavigatedTo** 메서드에서 채워집니다. 

**삭제 단추 추가**

1. MainPage.xaml에서 **MainCommandBar**라는 이름의 **CommandBar**를 찾고 확대/축소 단추 전에 새 단추를 추가합니다. (확대/축소 컨트롤은 아직 작동하지 않습니다. 이 컨트롤은 자습서의 다음 부분에서 살펴봅니다.)

    ```xaml
    <AppBarButton Icon="Delete"
                  Label="Delete selected image"
                  Click="{x:Bind DeleteSelectedImage}" />
    ```

    이미 XAML에 익숙한 경우 이 **Click** 값이 생소하게 보일 수도 있습니다. 이전 버전의 XAML에서는 일반적으로 이벤트 발신자의 매개 변수 및 이벤트 관련 인수 개체를 포함하여, 이를 특정 이벤트 처리기 서명이 있는 메서드로 설정해야 했습니다. 이벤트 인수가 필요할 때 이 기술을 사용할 수 있지만 x:Bind를 사용하면 다른 메서드에도 연결할 수 있습니다. 예를 들어 이벤트 데이터가 필요하지 않은 경우 여기에서와 같이 매개 변수가 없는 메서드에 연결할 수 있습니다.

    <!-- TODO add doc links about event binding - and doc links in general? -->

2. MainPage.xaml.cs에서 **DeleteSelectedImage** 메서드를 추가합니다.

    ```c#
    private void DeleteSelectedImage() =>
        Images.Remove(ImageGridView.SelectedItem as ImageFileInfo);
    ```

    이 메서드는 **Images** 컬렉션에서 선택한 이미지를 간단히 삭제합니다. 

이제 앱을 실행하고 단추를 사용하여 몇 개의 이미지를 삭제합니다. 확인할 수 있듯이 UI는 데이터 바인딩 및 **ObservableCollection\<T\>** 유형으로 인해 자동으로 업데이트됩니다. 

> [!Note]
> 문제가 있는 경우, 선택한 이미지를 목록에서 위 또는 아래로 이동하는 두 개의 단추를 추가한 다음 Click 이벤트를 DeleteSelectedImage와 비슷한 두 가지 새로운 메서드에 x:Bind 처리합니다.
 
## <a name="part-3-set-up-the-zoom-slider"></a>파트 3: 확대/축소 슬라이더 설정 

이 파트에서는 데이터 템플릿의 컨트롤에서 템플릿 외부의 확대/축소 슬라이더에 대한 단방향 바인딩을 만듭니다. 또한 **TextBlock.Text** 및 **Image.Source**와 같은 가장 명확한 속성뿐만 아니라 많은 컨트롤 속성을 통해 데이터 바인딩을 사용할 수 있다는 점을 알아봅니다. 

**이미지 데이터 템플릿을 확대/축소 슬라이더에 바인딩**

* **ImageGridView_DefaultItemTemplate**라는 이름의 **DataTemplate**을 찾고 템플릿 상단에서 **Grid** 컨트롤의 **Height** 및 **Width** 값을 바꿉니다.

    **이전**
    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate" 
                  x:DataType="local:ImageFileInfo">
        <Grid Height="200"
              Width="200"
              Margin="{StaticResource LargeItemMargin}">
    ```
    
    **이후**
    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate" 
                  x:DataType="local:ImageFileInfo">
        <Grid Height="{Binding Value, ElementName=ZoomSlider}"
              Width="{Binding Value, ElementName=ZoomSlider}"
              Margin="{StaticResource LargeItemMargin}">
    ```
    
    <!-- TODO talk about dependency properties --> 
    
    이것이 **x:Bind** 식이 아닌 **Binding** 식이라는 점을 인식하셨습니까? 이는 데이터 바인딩에 대한 기존 방식이며, 주로 사용되지 않습니다. **x:Bind**는 **Binding**이 수행하는 작업의 거의 대부분과 그 이상의 작업을 수행합니다. 하지만 데이터 템플릿에서 **x:Bind**를 사용하면 **x:DataType** 값에 선언된 유형에 바인딩됩니다. 그렇다면 템플릿의 항목을 페이지 XAML 또는 코드 숨김에 어떻게 바인딩할 수 있을까요? 기존의 **Binding** 식을 사용해야 합니다. 
    
    **Binding** 식은 **x:DataType** 값을 인식하지 않지만 이러한 **Binding** 식에는 거의 동일한 방식으로 작동하는 **ElementName** 값이 있습니다. 이는 바인딩 엔진에게 **Binding Value**가 페이지상 지정된 요소의 **Value** 속성에 바인딩된다는 점을 알려 줍니다(즉, 해당 **x:Name** 값을 가진 요소). 코드 숨김에 있는 속성에 바인딩하려는 경우, ```{Binding MyCodeBehindProperty, ElementName=page}```와 같은 모습으로 보입니다. 여기에서 **page**는 XAML의 **Page** 요소에 설정된 **x:Name**을 참조합니다. 
    
    > [!NOTE]
    > 기본적으로 **Binding** 식은 *단방향*이며, 이는 바인딩된 속성 값이 변경되면 UI를 자동으로 업데이트한다는 의미입니다. 
    > 
    > 반면 **x:Bind**의 기본값은 *일회성*이며, 이는 바인딩된 속성에 대한 변경 사항은 무시된다는 의미입니다. 가장 높은 성능 옵션이고 대부분의 바인딩은 정적인 읽기 전용 데이터이기 때문에 이것이 기본값입니다. 
    >
    > 여기에서는 값을 변경할 수 있는 속성이 있는 **x:Bind**를 사용하는 경우 ```Mode=OneWay``` 또는 ```Mode=TwoWay```를 추가해야 한다는 점을 알 수 있습니다. 이에 대한 예는 다음 단원에서 확인할 수 있습니다.

앱을 실행하고 슬라이더를 사용하여 이미지 템플릿 크기를 변경합니다. 보시다시피, 많은 코드가 필요 없이 매우 강력한 효과가 있습니다. 

![확대/축소 슬라이더가 있는 앱 실행](../design/basics/images/xaml-basics/gallery-with-zoom-control.png)

> [!NOTE]
> 문제가 있는 경우, 다른 UI 속성을 확대/축소 슬라이더 **Value** 속성 또는 확대/축소 슬라이더 뒤에 추가한 다른 슬라이더에 바인딩해 봅니다. 예를 들어 **TitleTextBlock**의 **FontSize** 속성을 **24**의 기본값으로 새 슬라이더에 바인딩할 수 있습니다. 적절한 최소값 및 최대값을 설정해야 합니다.

## <a name="part-4-improve-the-zoom-experience"></a>파트 4: 확대/축소 환경 개선 

이 파트에서는 **ItemSize** 속성을 코드 숨김에 추가하고 이미지 템플릿에서 새 속성으로 단방향 바인딩을 만듭니다. **ItemSize** 값은 확대/축소 슬라이더 및 **화면에 맞추기** 토글 및 창 크기와 같은 기타 요소에 의해 업데이트되어 더욱 향상된 환경을 제공합니다. 

기본 제공 컨트롤 속성과 달리 사용자 지정 속성은 단방향 및 양방향 바인딩이 있더라도 UI를 자동으로 업데이트하지 않습니다. 이는 **일회성** 바인딩으로 잘 작동하지만, 실제로 UI에 속성 변경 사항을 표시하려면 몇 가지 작업을 수행해야 합니다. 

**UI를 업데이트하도록 ItemSize 속성 만들기**

1. MainPage.xaml.cs에서 **MainPage**의 서명을 변경하여 **INotifyPropertyChanged** 인터페이스를 구현합니다.

    **이전:**
    ```c#
    public sealed partial class MainPage : Page
    ```

    **이후:**
    ```c#
    public sealed partial class MainPage : Page, INotifyPropertyChanged
    ```

    이는 UI를 업데이트하기 위해 바인딩이 수신할 수 있는 PropertyChanged 이벤트(다음에 추가됨)가 MainPage에 있다는 것을 바인딩 시스템에 알려줍니다. 

2. **PropertyChanged** 이벤트를 **MainPage** 클래스에 추가합니다.

    ```c#
    public event PropertyChangedEventHandler PropertyChanged;
    ```

    이 이벤트는 **INotifyPropertyChanged** 인터페이스에 필요한 완전한 구현을 제공합니다. 하지만 효과가 있으려면 사용자 지정 속성에서 이벤트를 명시적으로 발생시켜야 합니다. 

3. **ItemSize** 속성을 추가하고 **PropertyChanged** 이벤트를 setter에서 발생시킵니다.

    ```c#
    public double ItemSize
    {
        get => _itemSize;
        set
        {
            if (_itemSize != value)
            {
                _itemSize = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(nameof(ItemSize)));
            }
        }
    }
    private double _itemSize;
    ```

    **ItemSize** 속성은 비공개 **_itemSize** 필드 값을 공개합니다. 이와 같이 지원 필드를 사용하면 잠재적으로 불필요한 **PropertyChanged** 이벤트가 발생하기 전에 새 값이 이전 값과 같은지 여부를 속성에서 확인할 수 있습니다.

    이벤트 자체는 **Invoke** 메서드에 의해 발생합니다. 물음표는 **PropertyChanged** 이벤트가 null인지 여부를 확인합니다. 즉, 이벤트 처리기가 추가되었는지 여부를 확인합니다. 모든 단방향 또는 양방향 바인딩은 장면 뒤에서 이벤트 처리기를 추가하지만 아무도 수신하지 않는 경우 더 이상 여기에서 발생하지 않습니다. 하지만 **PropertyChanged**가 null이 아니면 **Invoke**는 이벤트 소스(**this** 키워드로 표시되는 페이지 자체) 및 속성 이름을 나타내는 **event-args** 개체에 대한 참조와 함께 호출됩니다. 이 정보를 사용하면 **ItemSize** 속성에 대한 단방향 또는 양방향 바인딩에 변경 내용이 전달되어 바인딩된 UI를 업데이트 할 수 있습니다. 

4. MainPage.xaml에서 **ImageGridView_DefaultItemTemplate**라는 이름의 **DataTemplate**을 찾고 템플릿 상단에서 **Grid** 컨트롤의 **Height** 및 **Width** 값을 바꿉니다. (이 자습서의 이전 파트에서 컨트롤 간 바인딩을 수행한 경우 **Value**를 **ItemSize**로, **ZoomSlider**를 **page**로만 변경할 수 있습니다. Height 및 Width 모두에 대해 이 작업을 수행해야 합니다!)

    **이전**
    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate" 
                  x:DataType="local:ImageFileInfo">
        <Grid Height="{Binding Value, ElementName=ZoomSlider}"
            Width="{Binding Value, ElementName=ZoomSlider}"
            Margin="{StaticResource LargeItemMargin}">
    ```
    
    **이후**
    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate" 
                  x:DataType="local:ImageFileInfo">
        <Grid Height="{Binding ItemSize, ElementName=page}"
              Width="{Binding ItemSize, ElementName=page}"
              Margin="{StaticResource LargeItemMargin}">
    ```

이제 UI가 **ItemSize** 변경 사항에 응답할 수 있게 되었으므로 실제로 변경해야 합니다. 앞서 언급했듯이 **ItemSize** 값은 다양한 UI 컨트롤의 현재 상태에서 계산되지만 이러한 컨트롤이 상태를 변경할 때마다 계산을 수행해야 합니다. 이렇게 하려면 특정 UI 변경 사항이 **ItemSize**를 업데이트하는 도우미 메서드를 호출하도록 이벤트 바인딩을 사용합니다. 

**ItemSize 속성 값 업데이트**

1. **DetermineItemSize** 메서드를 MainPage.xaml.cs에 추가합니다.

    ```c#
    private void DetermineItemSize()
    {
        if (FitScreenToggle != null
            && FitScreenToggle.IsOn == true
            && ImageGridView != null
            && ZoomSlider != null)
        {
            // The 'margins' value represents the total of the margins around the
            // image in the grid item. 8 from the ItemTemplate root grid + 8 from
            // the ItemContainerStyle * (Right + Left). If those values change, 
            // this value needs to be updated to match.
            int margins = (int)this.Resources["LargeItemMarginValue"] * 4;
            double gridWidth = ImageGridView.ActualWidth -
                (int)this.Resources["DesktopWindowSidePaddingValue"];
    
            if (AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Mobile" &&
                UIViewSettings.GetForCurrentView().UserInteractionMode ==
                    UserInteractionMode.Touch)
            {
                margins = (int)this.Resources["SmallItemMarginValue"] * 4;
                gridWidth = ImageGridView.ActualWidth -
                    (int)this.Resources["MobileWindowSidePaddingValue"];
            }
    
            double ItemWidth = ZoomSlider.Value + margins;
            // We need at least 1 column.
            int columns = (int)Math.Max(gridWidth / ItemWidth, 1);
    
            // Adjust the available grid width to account for margins around each item.
            double adjustedGridWidth = gridWidth - (columns * margins);
    
            ItemSize = (adjustedGridWidth / columns);
        }
        else
        {
            ItemSize = ZoomSlider.Value;
        }
    }
    ```

2. MainPage.xaml에서 파일의 맨 위로 이동하여 **SizeChanged** 이벤트 바인딩을 **Page** 요소에 추가합니다.

    **이전:**
    ```xaml
    <Page x:Name="page"  
    ```

    **이후:**
    ```xaml
    <Page x:Name="page" 
          SizeChanged="{x:Bind DetermineItemSize}"
    ```

3. **ZoomSlider**라는 **Slider**를 찾고 **ValueChanged** 이벤트 바인딩을 추가합니다.

    **이전:**
    ```xaml
    <Slider x:Name="ZoomSlider"
    ```

    **이후:**
    ```xaml
    <Slider x:Name="ZoomSlider"
            ValueChanged="{x:Bind DetermineItemSize}"
    ```

4. **FitScreenToggle**이라는 **ToggleSwitch**를 찾고 **Toggled** 이벤트 바인딩을 추가합니다.

    **이전:**
    ```xaml
    <ToggleSwitch x:Name="FitScreenToggle"
    ```

    **이후:**
    ```xaml
    <ToggleSwitch x:Name="FitScreenToggle"
                  Toggled="{x:Bind DetermineItemSize}"
    ```

앱을 실행하고 확대/축소 슬라이더와 **Fit to screen** 토글을 사용하여 이미지 템플릿 크기를 변경합니다. 보시다시피, 최신 변경 사항을 사용하면 코드를 잘 관리하면서 더 정교한 확대/축소 크기를 활성화할 수 있습니다. 

![화면에 맞게 활성화된 앱 실행](../design/basics/images/xaml-basics/gallery-with-fit-to-screen.png)

> [!NOTE]
> 문제가 발생하면 **ZoomSlider** 뒤에 **TextBlock**을 추가하고 **Text** 속성은 **ItemSize** 속성에 바인딩합니다. 데이터 템플릿에 없으므로 이전 **ItemSize** 바인딩 같은 **Binding** 대신 **x:Bind**를 사용할 수 있습니다.  
}

## <a name="part-5-enable-user-edits"></a>파트 5: 사용자 편집 활성화

여기에서는 사용자가 이미지 제목, 평점 및 다양한 시각 효과를 비롯한 값을 업데이트할 수 있도록 양방향 바인딩을 만듭니다. 

이를 위해 단일 이미지 뷰어, 확대/축소 컨트롤 및 UI 편집 기능을 제공하는 기존 **DetailPage**를 업데이트합니다.  

하지만 먼저 갤러리에서 이미지를 클릭할 때 앱이 연결되도록 **DetailPage**를 연결해야 합니다.

**DetailPage 연결**

1. MainPage.xaml에서 **ImageGridView**라는 이름의 **GridView**를 찾고 **ItemClick** 값을 추가합니다. 

    > [!TIP] 
    > 복사/붙여넣기 대신 아래의 변경 내용을 입력하면 "\<New Event Handler\>"라는 IntelliSense 팝업이 표시됩니다. Tab 키를 누르면 기본 메서드 처리기 이름으로 값이 채워지고 다음 단계에 표시된 메서드가 자동으로 추가됩니다. 그런 다음 F12 키를 눌러 코드 숨김의 메서드로 이동합니다. 

    **이전:**
    ```xaml
    <GridView x:Name="ImageGridView"
    ```

    **이후:**
    ```xaml
    <GridView x:Name="ImageGridView"
              ItemClick="ImageGridView_ItemClick"
    ```

    > [!NOTE] 
    > 현재 x:Bind 식 대신 여기에 일반적인 이벤트 처리기를 사용하고 있습니다. 다음과 같이 이벤트 데이터를 확인해야 하기 때문입니다. 

2. MainPage.xaml.cs에서 이벤트 처리기를 추가합니다(또는 마지막 단계에서 팁을 사용한 경우 입력).

    ```c#
    private void ImageGridView_ItemClick(object sender, ItemClickEventArgs e)
    {
        this.Frame.Navigate(typeof(DetailPage), e.ClickedItem);
    }
    ```

    이 메서드는 간단히 클릭된 항목을 전달하는 세부 정보 페이지로 이동합니다. 이 항목은 페이지 초기화를 위해 **DetailPage.OnNavigatedTo**에서 사용하는 **ImageFileInfo** 개체입니다. 이 자습서에서 이 메서드를 구현할 필요는 없지만, 이러한 기능을 살펴볼 수는 있습니다. 
    
3. (선택 사항) 현재 선택한 이미지로 작업하는 이전 플레이 지점에서 추가한 컨트롤을 삭제하거나 주석 처리합니다. 그대로 유지해도 영향은 주지 않지만, 세부 정보 페이지로 이동하지 않고 이미지를 선택하는 것이 이제 훨씬 더 어려워졌습니다. 

이제 두 페이지를 연결했으므로 앱을 실행하고 살펴보겠습니다. 편집 창에서 값을 변경하려고 하면 응답하지 않는 컨트롤을 제외하고 모든 기능이 제대로 작동합니다. 

보시다시피 제목 텍스트 상자에 제목이 표시되고 변경 내용을 입력할 수 있습니다. 변경 내용을 적용하려면 다른 컨트롤로 포커스를 변경해야 하지만 화면의 왼쪽 위 모서리에 있는 제목은 아직 업데이트되지 않습니다. 

모든 컨트롤은 이미 파트 1에서 설명한 일반 **x:Bind** 식을 사용하여 바인딩됩니다. 다시 생각해 보면, 이것은 모두 일회성 바인딩이므로 값의 변경 사항이 등록되지 않은 이유가 됩니다. 이 문제를 해결하려면 양방향 바인딩으로 전환해야 합니다. 

**대화형 편집 컨트롤 만들기**

1. DetailPage.xaml에서 **TitleTextBlock**이라는 **TextBlock** 및 **RadRating** 컨트롤을 찾고, **Mode=TwoWay**를 포함하도록 **x:Bind** 식을 업데이트합니다.

    **이전:**
    ```xaml
    <TextBlock x:Name="TitleTextBlock"
               Text="{x:Bind item.ImageTitle}"
               ... >
    <telerikInput:RadRating Value="{x:Bind item.ImageRating}"
                            ... >
    ```

    **이후:**
    ```xaml
    <TextBlock x:Name="TitleTextBlock" 
               Text="{x:Bind item.ImageTitle, Mode=TwoWay}" 
               ... >
    <telerikInput:RadRating Value="{x:Bind item.ImageRating, Mode=TwoWay}" 
                            ... >
    ```

2. 평점 컨트롤 이후에 제공되는 모든 효과 슬라이더에 대해 동일한 작업을 수행합니다.

    ```xaml
    <Slider Header="Exposure"    ... Value="{x:Bind item.Exposure, Mode=TwoWay}" ...
    <Slider Header="Temperature" ... Value="{x:Bind item.Temperature, Mode=TwoWay}" ... 
    <Slider Header="Tint"        ... Value="{x:Bind item.Tint, Mode=TwoWay}" ... 
    <Slider Header="Contrast"    ... Value="{x:Bind item.Contrast, Mode=TwoWay}" ... 
    <Slider Header="Saturation"  ... Value="{x:Bind item.Saturation, Mode=TwoWay}" ...
    <Slider Header="Blur"        ... Value="{x:Bind item.Blur, Mode=TwoWay}" ... 
    ```

양방향 모드는 예상대로 양쪽에 변경 사항이 있을 때마다 데이터가 양방향으로 이동한다는 것을 의미합니다. 

앞에서 설명한 단방향 바인딩과 마찬가지로 바인딩된 속성이 변경될 때마다 **ImageFileInfo** 클래스의 **INotifyPropertyChanged** 구현 덕분에 이러한 양방향 바인딩이 UI를 업데이트합니다. 하지만 양방향 바인딩을 사용하면 사용자가 컨트롤과 상호 작용할 때마다 값이 UI에서 바인딩된 속성으로 이동합니다. XAML 측면에서는 더 이상 필요한 것이 없습니다. 

앱을 실행하고 편집 컨트롤을 사용해 봅니다. 보시다시피, 변경 사항이 있을 때 이제 이미지 값에 영향을 미치며 기본 페이지로 돌아가면 변경 사항이 유지됩니다. 

## <a name="part-6-format-values-through-function-binding"></a>파트 6: 함수 바인딩을 통해 값 형식 지정

마지막으로 하나의 문제가 남아 있습니다. 효과 슬라이더를 움직이면 옆에 있는 레이블은 변경되지 않습니다. 

![기본 레이블 값이 있는 효과 슬라이더](../design/basics/images/xaml-basics/effect-sliders-before-label-fix.png)

이 자습서의 마지막 파트는 표시할 슬라이더 값의 형식을 지정하는 바인딩을 추가하는 것입니다.

**효과 슬라이더 레이블 바인딩 및 표시할 값의 형식 지정**

1. **Exposure** 슬라이더 뒤의 **TextBlock**을 찾아 여기에 표시된 바인딩 식으로 **Text** 값을 바꿉니다.

    **이전:**
    ```xaml
    <Slider Header="Exposure" ... />
    <TextBlock ... Text="0.00" />
    ```

    **이후:**
    ```xaml
    <Slider Header="Exposure" ... />
    <TextBlock ... Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />
    ```

    이는 메서드의 반환 값에 바인딩하고 있기 때문에 함수 바인딩이라고 합니다. 데이터 템플릿에 있는 경우 이 메서드는 페이지의 코드 숨김 또는 **x:DataType** 유형을 통해 액세스할 수 있어야 합니다. 이 경우 메서드는 익숙한 .NET **ToString** 메서드이며 페이지의 항목 속성을 통해 액세스한 다음 해당 항목의 **Exposure** 속성을 통해 액세스합니다. (연결 체인에 깊이 중첩된 메서드와 속성에 바인딩할 수 있는 방법을 보여 줍니다.)

    함수 바인딩은 다른 바인딩 소스를 메서드 인수로 전달할 수 있으므로 표시 값을 형식화하는 이상적인 방법이며, 바인딩 식은 단방향 모드에서 예상대로 해당 값의 변경 사항을 수신합니다. 이 예에서 **culture** 인수는 코드 숨김으로 구현된 변경되지 않은 필드에 대한 참조이지만 **PropertyChanged** 이벤트를 발생시키는 속성일 수도 있습니다. 이 경우 속성 값을 변경하면 **x:Bind** 식에서 새 값으로 **ToString**을 호출한 다음 그 결과로 UI를 업데이트할 수 있습니다. 

2. 다른 효과 슬라이더에 레이블을 지정하는 **TextBlocks**에 대해 동일한 작업을 수행합니다.

    ```xaml
    <Slider Header="Temperature" ... />
    <TextBlock ... Text="{x:Bind item.Temperature.ToString('N', culture), Mode=OneWay}" />

    <Slider Header="Tint" ... />
    <TextBlock ... Text="{x:Bind item.Tint.ToString('N', culture), Mode=OneWay}" />

    <Slider Header="Contrast" ... />
    <TextBlock ... Text="{x:Bind item.Contrast.ToString('N', culture), Mode=OneWay}" />

    <Slider Header="Saturation" ... />
    <TextBlock ... Text="{x:Bind item.Saturation.ToString('N', culture), Mode=OneWay}" />

    <Slider Header="Blur" ... />
    <TextBlock ... Text="{x:Bind item.Blur.ToString('N', culture), Mode=OneWay}" />
    ```

이제 앱을 실행하면 슬라이더 레이블을 포함하여 모든 것이 작동합니다. 

![작업 레이블이 있는 효과 슬라이더](../design/basics/images/xaml-basics/effect-sliders-after-label-fix.png)

> [!NOTE]
> 마지막 플레이 지점에서 **TextBlock**을 통해 함수 바인딩을 사용하여, **ItemSize** 값을 전달할 때 "000 x 000" 형식의 문자열을 반환하는 새 메서드에 바인딩합니다.


## <a name="conclusion"></a>결론

이 자습서는 데이터 바인딩에 대해 설명하고 사용 가능한 기능 중 일부를 보여 줍니다. 결론을 내리기 전에 한 가지 사항에 주의해야 합니다. 즉, 모든 것이 바인딩 가능한 것은 아니며 때로는 연결하려는 값이 바인딩하려는 속성과 호환되지 않습니다. 바인딩에는 많은 유연성이 있지만 모든 상황에서 작동하지는 않습니다.

바인딩으로 처리되지 않은 문제의 한 예로, 컨트롤에 세부 정보 페이지 확대/축소 기능과 마찬가지로 바인딩할 적절한 속성이 없는 경우가 있습니다. 이 확대/축소 슬라이더는 이미지를 표시하는 **ScrollViewer**와 상호 작용해야 하지만 **ScrollViewer**는 **ChangeView** 메서드를 통해서만 업데이트할 수 있습니다. 이 경우 기존의 이벤트 처리기를 사용하여 **ScrollViewer** 및 확대/축소 슬라이더를 동기화 상태로 유지합니다. 자세한 내용은 **DetailPage** **ZoomSlider_ValueChanged** 및 **MainImageScroll_ViewChanged** 메서드를 참조하세요.

그럼에도 불구하고 바인딩은 코드를 단순화하고 UI 논리를 데이터 논리와 분리하여 유지하는 강력하고 유연한 방법입니다. 이렇게 하면 다른 측면에 버그를 도입할 위험을 줄이면서 이렇게 분리된 양 측면을 조정하는 것이 훨씬 쉬워집니다. 

UI 및 데이터 분리의 한 예는 **ImageFileInfo.ImageTitle** 속성입니다. 이 속성(및 **ImageRating** 속성)은 값이 필드 대신 파일 메타데이터(**ImageProperties** 형식을 통해 노출됨)에 저장되기 때문에 파트 4에서 만든 **ItemSize** 속성과 약간 다릅니다. 또한 **ImageTitle**은 파일 메타데이터에 제목이 없는 경우 **ImageName** 값(파일 이름으로 설정)을 반환합니다. 

```c#
public string ImageTitle
{
    get => String.IsNullOrEmpty(ImageProperties.Title) ? ImageName : ImageProperties.Title;
    set
    {
        if (ImageProperties.Title != value)
        {
            ImageProperties.Title = value;
            var ignoreResult = ImageProperties.SavePropertiesAsync();
            OnPropertyChanged();
        }
    }
}
```

보시다시피 setter는 **ImageProperties.Title** 속성을 업데이트한 다음 **SavePropertiesAsync**를 호출하여 새 값을 파일에 씁니다. (이는 비동기식 방법이지만 속성에 **await** 키워드를 사용할 수 없습니다. 속성 getter 및 setter가 즉시 완성되어야 하기 때문에 필요하지 않을 것입니다. 따라서 그 대신 메서드를 호출하고 이를 반환하는 **Task** 개체를 무시할 수 있습니다.) 

<!-- TODO more screenshots? -->

## <a name="going-further"></a>더 나아가기

이제 이 실습을 완료했으므로 독자적으로 문제를 해결할 수 있는 충분한 바인딩 지식을 얻게 되었습니다.

이전의 세부 정보 페이지에서 확대/축소 수준을 변경하면 뒤로 이동하며, 다시 같은 이미지를 선택하면 자동으로 다시 설정됩니다. 각 이미지의 확대/축소 수준을 개별적으로 보존하고 복원하는 방법을 알 수 있나요? 행운을 빕니다!
    
이 자습서에서 필요한 모든 정보를 얻을 수 있어야 하지만, 더 많은 지침이 필요하다면 한 번의 클릭으로 데이터 바인딩 문서를 확인할 수 있습니다. 여기에서 시작하세요.

+ [{x:Bind} 태그 확장](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)
+ [데이터 바인딩 심층 분석](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)