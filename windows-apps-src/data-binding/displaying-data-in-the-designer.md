---
author: mcleblanc
ms.assetid: 089660A2-7CAE-4911-9994-F619C5D22287
title: 디자인 화면의 샘플 데이터 및 프로토타입 생성용 샘플 데이터
description: 앱에서 Microsoft Visual Studio 또는 Blend for Visual Studio의 디자인 화면에 라이브 데이터를 표시하는 것은 불가능할 수도 있고 아마도 개인 정보 또는 성능상의 이유로 바람직하지 않을 수도 있습니다.
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 269d51ec6005bcd61ac01a66d72c34bdb2901add
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/03/2018
ms.locfileid: "5993181"
---
<a name="sample-data-on-the-design-surface-and-for-prototyping"></a>디자인 화면의 샘플 데이터 및 프로토타입 생성용 샘플 데이터
=============================================================================================



**참고**샘플 데이터가 필요한 정도-그리고 하면 도움이 될 얼마나-바인딩에서 [{Binding} 태그 확장](https://msdn.microsoft.com/library/windows/apps/Mt204782) 또는 [{x: Bind} 태그 확장](https://msdn.microsoft.com/library/windows/apps/Mt204783)을 사용 하는 여부에 따라 달라 집니다. 이 항목에서 설명하는 기술은 [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) 사용을 기준으로 하므로 **{Binding}** 에만 적합합니다. 그러나 **{x:Bind}** 를 사용하는 경우에는 바인딩에서 적어도 항목 컨트롤에 대한 값을 비롯해 자리 표시자 값을 디자인 화면에 표시하므로 샘플 데이터가 똑같이 필요하지는 않습니다.

앱에서 Microsoft Visual Studio 또는 Blend for Visual Studio의 디자인 화면에 라이브 데이터를 표시하는 것은 불가능할 수도 있고 아마도 개인 정보 또는 성능상의 이유로 바람직하지 않을 수도 있습니다. 컨트롤에 데이터를 채워 앱의 레이아웃, 템플릿 및 기타 시각적 속성을 작업할 수 있도록 하기 위해 다양한 방법으로 디자인 타임 샘플 데이터를 사용할 수 있습니다. 스케치(또는 프로토타입) 앱을 빌드하는 경우에도 샘플 데이터를 사용하면 정말 유용하고 시간을 절약할 수 있습니다. 스케치 또는 프로토타입에서 런타임에 샘플 데이터를 사용하면 실제 라이브 데이터에 연결하지 않더라도 아이디어를 설명할 수 있습니다.

**{Binding}을 보여 주는 샘플 앱**

-   [Bookstore1](http://go.microsoft.com/fwlink/?linkid=532950) 앱 다운로드
-   [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952) 앱 다운로드

<a name="setting-datacontext-in-markup"></a>태그에서 DataContext 설정
-----------------------------

개발자가 코드 숨김에 필수적인 코드를 사용하여 페이지 또는 사용자 컨트롤의 [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713)를 뷰 모델 인스턴스로 설정하는 것은 아주 일반적인 방법입니다.

``` csharp
public MainPage()
{
    InitializeComponent();
    this.DataContext = new BookstoreViewModel();
}
```

하지만 이렇게 하면 페이지를 필요한 만큼 "디자인할 수" 없게 됩니다. 그 이유는 XAML 페이지를 Visual Studio 또는 Blend for Visual Studio에서 열면 **DataContext** 값을 할당하는 필수적인 코드가 실행되지 않기 때문입니다(실제로 코드 숨김은 하나도 실행되지 않음). XAML 도구는 물론 태그를 구문 분석하고 태그에 선언된 개체를 인스턴스화하지만 실제로 페이지의 형식 자체를 인스턴스화하지 않습니다. 결과적으로 컨트롤 또는 **데이터 바인딩 만들기** 대화 상자에 데이터가 표시되지 않고 페이지의 스타일 및 레이아웃을 지정하기가 더 어려워집니다.

![스파스 디자인 UI](images/displaying-data-in-the-designer-01.png)

시도해 볼 첫 번째 해결 방법은 해당 **DataContext** 할당을 주석으로 처리하고 대신 페이지 태그에 **DataContext**를 설정하는 것입니다. 이렇게 하면 라이브 데이터가 런타임뿐 아니라 디자인 타임에도 표시됩니다. 이렇게 하려면 먼저 XAML 페이지를 엽니다. 그런 다음 **문서 개요** 창에서 디자인 가능한 루트 요소(일반적으로 **\[Page\]** 레이블 사용)를 클릭하여 선택합니다. **속성** 창에서 **DataContext** 속성(일반 범주 내에 있음)을 클릭한 다음 **새로 만들기** 클릭합니다. **개체 선택** 대화 상자에서 뷰 모델 유형을 클릭한 다음 **확인**을 클릭합니다.

![DataContext 설정을 위한 UI](images/displaying-data-in-the-designer-02.png)

결과 태그는 다음과 같습니다.

``` xaml
<Page ... >
    <Page.DataContext>
        <local:BookstoreViewModel/>
    </Page.DataContext>
```

그리고 바인딩에서 확인할 수 있는 디자인 화면은 이제 다음과 같습니다. **데이터 바인딩 만들기** 대화 상자의 **경로** 선택기는 이제 바인딩할 수 있는 **DataContext** 형식 및 속성을 기준으로 채워집니다.

![디자인할 수 있는 UI](images/displaying-data-in-the-designer-03.png)

**데이터 바인딩 만들기** 대화 상자에서는 작업을 시작할 형식만 필요하지만 바인딩에서는 속성을 값으로 초기화해야 합니다. 성능, 데이터 전송 요금 지불, 개인 정보 문제 등으로 인해 디자인 타임에 클라우드 서비스에 연결하지 않으려는 경우에는 초기화 코드에서 Visual Studio 또는 Blend for Visual Studio와 같은 디자인 도구에서 앱이 실행 중인지 확인하고 그런 경우 디자인 타임에만 사용할 샘플 데이터를 로드할 수 있습니다.

``` csharp
if (Windows.ApplicationModel.DesignMode.DesignModeEnabled)
{
    // Load design-time books.
}
else
{
    // Load books from a cloud service.
}
```

초기화 코드에 매개 변수를 전달해야 하는 경우 뷰 모델 로케이터를 사용할 수 있습니다. 뷰 모델 로케이터는 앱 리소스에 넣을 수 있는 클래스입니다. 이 클래스에는 뷰 모델을 노출하는 속성이 있고 페이지의 **DataContext**가 해당 속성에 바인딩됩니다. 로케이터 또는 뷰 모델에서 사용할 수 있는 다른 패턴은 종속성 주입입니다. 이 패턴은 해당되는 디자인 타임 또는 런타임 데이터 공급자(각각 공용 인터페이스 구현)를 생성할 수 있습니다.

<a name="sample-data-from-class-and-design-time-attributes"></a>"클래스의 샘플 데이터" 및 디자인 타임 특성
---------------------------------------------------------------------------------------

어떤 이유로든 이전 섹션의 옵션이 작동하지 않더라도 XAML 도구 기능 및 디자인 타임 특성을 통해 사용할 수 있는 디자인 타임 데이터 옵션이 여전히 많습니다. 한 가지 좋은 옵션은 Blend for Visual Studio의 **클래스에서 예제 데이터 만들기** 기능입니다. 해당 명령은 **데이터** 패널 위쪽에 있는 단추 중 하나에서 찾을 수 있습니다.

명령에서 사용할 클래스만 지정하면 됩니다. 그러면 명령에서 두 가지 중요한 작업을 수행합니다. 먼저 선택한 클래스의 인스턴스 및 모든 멤버를 재귀적으로 하이드레이션하는 데 적합한 샘플 데이터를 포함하는 XAML 파일을 생성합니다(실제로 이 도구는 XAML 또는 JSON 파일에서 동일하게 제대로 작동함). 둘째 이 명령은 **데이터** 패널에 선택한 클래스의 스키마를 채웁니다. 그러면 **데이터** 패널의 멤버를 디자인 화면으로 끌어와 다양한 작업을 수행할 수 있습니다. 끄는 항목과 항목을 놓는 위치에 따라 기존 컨트롤에 바인딩을 추가하거나(**{Binding}** 사용) 새 컨트롤을 만들어 동시에 바인딩할 수 있습니다. 두 경우 모두 작업을 수행할 때 페이지의 레이아웃 루트에 디자인 타임 데이터 컨텍스트(**d:DataContext**)도 설정할 수 있습니다(아직 설정되지 않은 경우). 해당 디자인 타임 데이터 컨텍스트는 **d:DesignData** 특성을 사용하여 생성된 XAML 파일(원하는 대로 프로젝트에서 찾고 편집할 수 있으므로 원하는 샘플 데이터가 포함됨)에서 샘플 데이터를 가져옵니다.

``` xaml
<Page ...
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">
    <Grid ... d:DataContext="{d:DesignData /SampleData/RecordingViewModelSampleData.xaml}"/>
        <ListView ItemsSource="{Binding Recordings}" ... />
        ...
    </Grid>
</Page>
```

다양한 xmlns 선언은 **d:** 접두사가 있는 특성이 디자인 타임에만 해석되고 런타임에는 무시됨을 의미합니다. 따라서 **d:DataContext** 특성은 디자인 타임에만 [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) 속성 값에 영향을 주고 런타임에는 적용되지 않습니다. 원하는 경우 태그에서 **d:DataContext** 및 **DataContext**를 모두 설정할 수도 있습니다. **d:DataContext**는 디자인 타임에 재정의되고 **DataContext**는 런타임에 재정의됩니다. 이와 같이 동일한 재정의 규칙이 디자인 타임 및 런타임 특성 모두에 적용됩니다.

**d:DataContext** 특성 및 다른 모든 디자인 타임 특성은 [디자인 타임 특성](http://go.microsoft.com/fwlink/p/?LinkId=272504) 항목에 설명되어 있으며, UWP(유니버설 Windows 플랫폼) 앱에 여전히 사용할 수 있습니다.

[**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833)에는 **DataContext** 속성이 없지만 **Source** 속성이 있습니다. 따라서 **CollectionViewSource**에 디자인 타임 전용 샘플 데이터를 설정하는 데 사용할 수 있는 **d:Source** 속성이 있습니다.

``` xaml
    <Page.Resources>
        <CollectionViewSource x:Name="RecordingsCollection" Source="{Binding Recordings}"
            d:Source="{d:DesignData /SampleData/RecordingsSampleData.xaml}"/>
    </Page.Resources>

    ...

        <ListView ItemsSource="{Binding Source={StaticResource RecordingsCollection}}" ... />
    ...
```

이렇게 할 수 있으려면 `Recordings : ObservableCollection<Recording>`이라는 클래스가 있고 여기에 나온 대로 **Recordings** 개체만 포함하도록(**Recording** 개체가 내부에 포함) 샘플 데이터 XAML 파일을 편집해야 합니다.

``` xml
<Quickstart:Recordings xmlns:Quickstart="using:Quickstart">
    <Quickstart:Recording ArtistName="Mollis massa" CompositionName="Cubilia metus"
        OneLineSummary="Morbi adipiscing sed" ReleaseDateTime="01/01/1800 15:53:17"/>
    <Quickstart:Recording ArtistName="Vulputate nunc" CompositionName="Parturient vestibulum"
        OneLineSummary="Dapibus praesent netus amet vestibulum" ReleaseDateTime="01/01/1800 15:53:17"/>
    <Quickstart:Recording ArtistName="Phasellus accumsan" CompositionName="Sit bibendum"
        OneLineSummary="Vestibulum egestas montes dictumst" ReleaseDateTime="01/01/1800 15:53:17"/>
</Quickstart:Recordings>
```

XAML 대신 JSON 샘플 데이터 파일을 사용하는 경우에는 **Type** 속성을 설정해야 합니다.

``` xaml
    d:Source="{d:DesignData /SampleData/RecordingsSampleData.json, Type=local:Recordings}"
```

지금까지는 **d:DesignData**를 사용하여 XAML 또는 JSON 파일에서 디자인 타임 샘플 데이터를 로드했습니다. 다른 방법으로 **d:DesignInstance** 태그 확장이 있는데, 이 경우에는 디자인 타임 원본이 **Type** 속성에서 지정하는 클래스를 기반으로 합니다. 예를 들면 다음과 같습니다.

``` xaml
    <CollectionViewSource x:Name="RecordingsCollection" Source="{Binding Recordings}"
        d:Source="{d:DesignInstance Type=local:Recordings, IsDesignTimeCreatable=True}"/>
```

**IsDesignTimeCreatable** 속성은 디자인 도구에서 실제로 클래스의 인스턴스를 만들어야 함을 의미합니다. 즉, 이 클래스는 공용 기본 생성자가 있고 자체적으로 실제 또는 샘플 데이터로 채워집니다. **IsDesignTimeCreatable**을 설정하지 않거나 **False**로 설정하면 샘플 데이터가 디자인 화면에 표시되지 않습니다. 디자인 도구는 경우에 모든 바인딩 속성에 대 한 클래스를 구문 분석 하 고 이러한 **데이터** 패널 및 **데이터 바인딩 만들기** 대화 상자를 표시 하는 것입니다.

<a name="sample-data-for-prototyping"></a>프로토타입 생성용 샘플 데이터
--------------------------------------------------------

프로토타입 생성을 위해 디자인 타임과 런타임 모두에 샘플 데이터가 필요합니다. 해당 사용 사례를 위해 Blend for Visual Studio에는 **새 샘플 데이터** 기능이 있습니다. 해당 명령은 **데이터** 패널 위쪽에 있는 단추 중 하나에서 찾을 수 있습니다.

클래스를 지정하는 대신 **데이터** 패널에서 바로 샘플 데이터 원본의 스키마를 실제로 디자인할 수 있습니다. 또한 **데이터** 패널에서 샘플 데이터 값을 편집할 수도 있습니다. 원하는 경우 할 수는 있지만 파일을 열고 편집할 필요가 없습니다.

**새 샘플 데이터** 기능은 **d:DataContext**가 아니라 [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713)를 사용하므로 스케치 또는 프로토타입을 디자인할 때뿐 아니라 실행할 때도 샘플 데이터를 사용할 수 있습니다. 그리고 **데이터** 패널을 사용하면 디자인 및 바인딩 작업이 실제로 단축됩니다. 예를 들어 컬렉션 속성을 **데이터** 패널에서 디자인 화면으로 끌어다 놓기만 하면 데이터 바인딩된 항목 컨트롤과 필요한 템플릿이 생성되므로, 곧바로 빌드하고 실행할 수 있습니다.

![프로토타입 생성용 샘플 데이터](images/displaying-data-in-the-designer-04.png)
