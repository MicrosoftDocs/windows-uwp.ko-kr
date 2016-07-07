---
author: mcleblanc
ms.assetid: A9D54DEC-CD1B-4043-ADE4-32CD4977D1BF
title: "데이터 바인딩 개요"
description: "이 항목에서는 UWP(유니버설 Windows 플랫폼) 앱에서 컨트롤(또는 다른 UI 요소)을 단일 항목에 바인딩하거나 항목 컨트롤을 항목 컬렉션에 바인딩하는 방법을 보여 줍니다."
ms.sourcegitcommit: c5325f0d0a067847bea81a115db4770a39ddd12a
ms.openlocfilehash: 4753c2fc52fa0227b3867685b793a3d6cfc05630

---
데이터 바인딩 개요
=====================

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


이 항목에서는 UWP(유니버설 Windows 플랫폼) 앱에서 컨트롤(또는 다른 UI 요소)을 단일 항목에 바인딩하거나 항목 컨트롤을 항목 컬렉션에 바인딩하는 방법을 보여 줍니다. 또한 항목의 렌더링을 제어하고 선택 항목을 기반으로 세부 정보 보기를 구현하고, 표시할 데이터를 변환하는 방법을 보여 줍니다. 자세한 내용은 [데이터 바인딩 심층 분석](data-binding-in-depth.md)을 참조하세요.

필수 조건
-------------------------------------------------------------------------------------------------------------

이 항목에서는 사용자가 기본 UWP 앱을 만드는 방법을 알고 있다고 가정합니다. 첫 UWP 앱을 만드는 방법은 [C# 또는 Visual Basic을 사용하여 첫 UWP 앱 만들기](https://msdn.microsoft.com/library/windows/apps/Hh974581)를 참조하세요.

프로젝트 만들기
---------------------------------------------------------------------------------------------------------------------------------

**비어 있는 응용 프로그램(Windows 유니버설)** 프로젝트를 만듭니다. 이름을 "Quickstart"로 지정합니다.

단일 항목에 바인딩
---------------------------------------------------------------------------------------------------------------------------------------------------------

모든 바인딩은 바인딩 대상과 바인딩 소스로 구성됩니다. 일반적으로 대상은 컨트롤 또는 기타 UI 요소의 속성이고, 소스는 클래스 인스턴스(데이터 모델 또는 뷰 모델)의 속성입니다. 이 예제에서는 컨트롤을 단일 항목에 바인딩하는 방법을 보여 줍니다. 대상은 **TextBlock**의 **Text** 속성입니다. 소스는 오디오 녹음을 나타내는 **Recording**이라는 단순 클래스의 인스턴스를 나타냅니다. 먼저 클래스를 살펴보겠습니다.

프로젝트에 새 클래스를 추가하고 이름을 Recording.cs(C#을 사용하는 경우)로 지정한 후 다음 코드를 추가합니다.

```csharp
namespace Quickstart
{
    public class Recording
    {
        public string ArtistName { get; set; }
        public string CompositionName { get; set; }
        public DateTime ReleaseDateTime { get; set; }

        public Recording()
        {
            this.ArtistName = "Wolfgang Amadeus Mozart";
            this.CompositionName = "Andante in C for Piano";
            this.ReleaseDateTime = new DateTime(1761, 1, 1);
        }

        public string OneLineSummary
        {
            get
            {
                return $"{this.CompositionName} by {this.ArtistName}, released: "
                    + this.ReleaseDateTime.ToString("d");
            }
        }
    }

    public class RecordingViewModel
    {
        private Recording defaultRecording = new Recording();
        public Recording DefaultRecording { get { return this.defaultRecording; } }
    }
}
```

```cpp
#include <sstream>

namespace Quickstart
{
    public ref class Recording sealed
    {
    private:
        Platform::String^ artistName;
        Platform::String^ compositionName;
        Windows::Globalization::Calendar^ releaseDateTime;

    public:
        Recording(Platform::String^ artistName, Platform::String^ compositionName,
            Windows::Globalization::Calendar^ releaseDateTime) :
            artistName{ artistName },
            compositionName{ compositionName },
            releaseDateTime{ releaseDateTime } {}

        property Platform::String^ ArtistName
        {
            Platform::String^ get() { return this->artistName; }
        }

        property Platform::String^ CompositionName
        {
            Platform::String^ get() { return this->compositionName; }
        }

        property Windows::Globalization::Calendar^ ReleaseDateTime
        {
            Windows::Globalization::Calendar^ get() { return this->releaseDateTime; }
        }

        property Platform::String^ OneLineSummary
        {
            Platform::String^ get()
            {
                std::wstringstream wstringstream;
                wstringstream << this->CompositionName->Data();
                wstringstream << L" by " << this->ArtistName->Data();
                wstringstream << L", released: " << this->ReleaseDateTime->MonthAsNumericString()->Data();
                wstringstream << L"/" << this->ReleaseDateTime->DayAsString()->Data();
                wstringstream << L"/" << this->ReleaseDateTime->YearAsString()->Data();
                return ref new Platform::String(wstringstream.str().c-str());
            }
        }
    };

    public ref class RecordingViewModel sealed
    {
    private:
        Recording^ defaultRecording;

    public:
        RecordingViewModel()
        {
            Windows::Globalization::Calendar^ releaseDateTime = ref new Windows::Globalization::Calendar();
            releaseDateTime->Month = 1;
            releaseDateTime->Day = 1;
            releaseDateTime->Year = 1761;
            this->defaultRecording = ref new Recording{ L"Wolfgang Amadeus Mozart", L"Andante in C for Piano", releaseDateTime };
        }

        property Recording^ DefaultRecording
        {
            Recording^ get() { return this->defaultRecording; };
        }
    };
}
```

그런 다음 태그 페이지를 나타내는 클래스에서 바인딩 소스 클래스를 노출합니다. **RecordingViewModel** 형식의 속성을 **MainPage**에 바인딩하면 됩니다.

```csharp
namespace Quickstart
{
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.ViewModel = new RecordingViewModel();
        }

        public RecordingViewModel ViewModel { get; set; }
    }
}
```

```cpp
namespace Quickstart
{
    public ref class MainPage sealed
    {
    private:
        RecordingViewModel^ viewModel;

    public:
        MainPage()
        {
            InitializeComponent();
            this->viewModel = ref new RecordingViewModel();
        }

        property RecordingViewModel^ ViewModel
        {
            RecordingViewModel^ get() { return this->viewModel; };
        }
    };
}
```

마지막 부분은 **TextBlock**를 **ViewModel.DefaultRecording.OneLiner** 속성에 바인딩하는 것입니다.

```xml
<Page x:Class="Quickstart.MainPage" ... >
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock Text="{x:Bind ViewModel.DefaultRecording.OneLineSummary}"
        HorizontalAlignment="Center"
        VerticalAlignment="Center"/>
    </Grid>
</Page>
```

다음은 결과입니다.

![Textblock 바인딩](images/xaml-databinding0.png)

항목 컬렉션에 바인딩
------------------------------------------------------------------------------------------------------------------

비즈니스 개체 컬렉션에 바인딩하는 것이 일반적인 시나리오입니다. C# 및 Visual Basic에서 일반 [**ObservableCollection&lt;T&gt;**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/ms668604.aspx) 클래스는 [**INotifyPropertyChanged**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/system.componentmodel.inotifypropertychanged.aspx) 및 [**INotifyCollectionChanged**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx) 인터페이스를 구현하므로 데이터 바인딩에 유용한 컬렉션 옵션이 됩니다. 이러한 인터페이스는 항목이 추가 또는 변경되거나 목록 자체의 속성이 변경될 경우 바인딩에 대한 변경 알림을 제공합니다. 또한 바인딩된 컨트롤을 컬렉션에 있는 개체의 속성 변경 사항으로 업데이트하려면 비즈니스 개체에서 **INotifyPropertyChanged**를 구현해야 합니다. 자세한 내용은 [데이터 바인딩 심층 분석](data-binding-in-depth.md)을 참조하세요.

다음 예제에서는 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878)를 `Recording` 개체의 컬렉션에 바인딩합니다. 먼저 컬렉션을 뷰 모델에 추가합니다. 다음과 같은 새 멤버를 **RecordingViewModel** 클래스에 추가하면 됩니다.

```csharp
    public class RecordingViewModel
    {
        ...
        
        private ObservableCollection<Recording> recordings = new ObservableCollection<Recording>();
        public ObservableCollection<Recording> Recordings { get { return this.recordings; } }

        public RecordingViewModel()
        {
            this.recordings.Add(new Recording() { ArtistName = "Johann Sebastian Bach",
            CompositionName = "Mass in B minor", ReleaseDateTime = new DateTime(1748, 7, 8) });
            this.recordings.Add(new Recording() { ArtistName = "Ludwig van Beethoven",
            CompositionName = "Third Symphony", ReleaseDateTime = new DateTime(1805, 2, 11) });
            this.recordings.Add(new Recording() { ArtistName = "George Frideric Handel",
            CompositionName = "Serse", ReleaseDateTime = new DateTime(1737, 12, 3) });
        }
    }
```

```cpp
    public ref class RecordingViewModel sealed
    {
    private:
        ...
        Windows::Foundation::Collections::IVector<Recording^>^ recordings;

    public:
        RecordingViewModel()
        {
            ...

            releaseDateTime = ref new Windows::Globalization::Calendar();
            releaseDateTime->Month = 7;
            releaseDateTime->Day = 8;
            releaseDateTime->Year = 1748;
            Recording^ recording = ref new Recording{ L"Johann Sebastian Bach", L"Mass in B minor", releaseDateTime };
            this->Recordings->Append(recording);

            releaseDateTime = ref new Windows::Globalization::Calendar();
            releaseDateTime->Month = 2;
            releaseDateTime->Day = 11;
            releaseDateTime->Year = 1805;
            recording = ref new Recording{ L"Ludwig van Beethoven", L"Third Symphony", releaseDateTime };
            this->Recordings->Append(recording);

            releaseDateTime = ref new Windows::Globalization::Calendar();
            releaseDateTime->Month = 12;
            releaseDateTime->Day = 3;
            releaseDateTime->Year = 1737;
            recording = ref new Recording{ L"George Frideric Handel", L"Serse", releaseDateTime };
            this->Recordings->Append(recording);
        }

        ...

        property Windows::Foundation::Collections::IVector<Recording^>^ Recordings
        {
            Windows::Foundation::Collections::IVector<Recording^>^ get()
            {
                if (this->recordings == nullptr)
                {
                    this->recordings = ref new Platform::Collections::Vector<Recording^>();
                }
                return this->recordings;
            };
        }
    };
```

그런 다음 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878)를 **ViewModel.Recordings** 속성에 바인딩합니다.

```xml
<Page x:Class="Quickstart.MainPage" ... >
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <ListView ItemsSource="{x:Bind ViewModel.Recordings}"
        HorizontalAlignment="Center" VerticalAlignment="Center"/>
    </Grid>
</Page>
```

**Recording** 클래스에 대한 데이터 템플릿을 아직 제공하지 않았으므로 UI 프레임워크에서 수행할 수 있는 최상의 작업은 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878)의 각 항목에 대해 [**ToString**](https://msdn.microsoft.com/library/windows/apps/system.object.tostring.aspx)을 호출하는 것입니다. **ToString**의 기본 구현은 형식 이름을 반환하는 것입니다.

![목록 보기 바인딩](images/xaml-databinding1.png)

이를 해결하기 위해 **OneLineSummary** 값을 반환하도록 [**ToString**](https://msdn.microsoft.com/library/windows/apps/system.object.tostring.aspx)을 재정의하거나 데이터 템플릿을 제공할 수 있습니다. 데이터 템플릿 옵션이 더 일반적이며 보다 유연합니다. 콘텐츠 컨트롤의 [**ContentTemplate**](https://msdn.microsoft.com/library/windows/apps/BR209369) 속성 또는 항목 컨트롤의 [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242830) 속성을 사용하여 데이터 템플릿을 지정합니다. 결과에 대한 그림과 함께 **Recording**에 대한 데이터 템플릿을 디자인할 수 있는 두 가지 방법은 다음과 같습니다.

```xml
    <ListView ItemsSource="{x:Bind ViewModel.Recordings}"
        HorizontalAlignment="Center" VerticalAlignment="Center">
        <ListView.ItemTemplate>
            <DataTemplate x:DataType="local:Recording">
                <TextBlock Text="{x:Bind OneLineSummary}"/>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
```

![목록 보기 바인딩](images/xaml-databinding2.png)

```xml
    <ListView ItemsSource="{x:Bind ViewModel.Recordings}"
    HorizontalAlignment="Center" VerticalAlignment="Center">
        <ListView.ItemTemplate>
            <DataTemplate x:DataType="local:Recording">
                <StackPanel Orientation="Horizontal" Margin="6">
                    <SymbolIcon Symbol="Audio" Margin="0,0,12,0"/>
                    <StackPanel>
                        <TextBlock Text="{x:Bind ArtistName}" FontWeight="Bold"/>
                        <TextBlock Text="{x:Bind CompositionName}"/>
                    </StackPanel>
                </StackPanel>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
```

![목록 보기 바인딩](images/xaml-databinding3.png)

XAML 구문에 대한 자세한 내용은 참조 [XAML을 사용하여 UI 만들기](https://msdn.microsoft.com/library/windows/apps/Mt228349)를 참조하세요. 컨트롤 레이아웃에 대한 자세한 내용은 [XAML을 사용하여 레이아웃 정의](https://msdn.microsoft.com/library/windows/apps/Mt228350)를 참조하세요.

자세히 보기 추가
-----------------------------------------------------------------------------------------------------

[**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 항목에서 **Recording** 개체의 모든 세부 정보를 표시할 수 있습니다. 그러나 이러한 정보는 많은 공간을 차지합니다. 따라서 대신 항목을 식별하는 데 충분한 데이터만 표시한 다음, 사용자가 선택한 경우 세부 정보 보기라는 UI의 별도 부분에 선택한 항목의 모든 세부 정보를 표시할 수 있습니다. 이 정렬을 마스터/세부 정보 보기 또는 목록/세부 정보 보기라고도 합니다.

두 가지 방법이 있습니다. 세부 정보 보기를 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878)의 [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/BR209770) 속성에 바인딩할 수 있습니다. 또는 [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833)를 사용할 수 있습니다. **ListView**와 세부 정보 보기를 모두 **CollectionViewSource**에 바인딩합니다(현재 선택된 항목을 처리함). 두 기술 모두 아래에 나와 있으며, 둘 다 그림과 동일한 결과를 제공합니다.

**참고** 지금까지 이 항목에서는 [{x:Bind} 태그 확장](https://msdn.microsoft.com/library/windows/apps/Mt204783)만 사용했지만 아래에서 살펴볼 두 기술에는 모두 보다 유연한(그러나 성능이 낮은) [{Binding} 태그 확장](https://msdn.microsoft.com/library/windows/apps/Mt204782)이 필요합니다.

먼저, 다음은 [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/BR209770) 기술입니다. Visual C++ 구성 요소 확장(C++/CX)을 사용하는 경우 [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782)을 사용할 것이므로 **Recording** 클래스에 [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) 특성을 추가해야 합니다.

```cpp
    [Windows::UI::Xaml::Data::Bindable]
    public ref class Recording sealed
    {
        ...
    };
```

달리 더 변경할 사항은 태그뿐입니다.

```xml
<Page x:Class="Quickstart.MainPage" ... >
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <StackPanel HorizontalAlignment="Center" VerticalAlignment="Center">
            <ListView x:Name="recordingsListView" ItemsSource="{x:Bind ViewModel.Recordings}">
                <ListView.ItemTemplate>
                    <DataTemplate x:DataType="local:Recording">
                        <StackPanel Orientation="Horizontal" Margin="6">
                            <SymbolIcon Symbol="Audio" Margin="0,0,12,0"/>
                            <StackPanel>
                                <TextBlock Text="{x:Bind CompositionName}"/>
                            </StackPanel>
                        </StackPanel>
                    </DataTemplate>
                </ListView.ItemTemplate>
            </ListView>
            <StackPanel DataContext="{Binding SelectedItem, ElementName=recordingsListView}"
            Margin="0,24,0,0">
                <TextBlock Text="{Binding ArtistName}"/>
                <TextBlock Text="{Binding CompositionName}"/>
                <TextBlock Text="{Binding ReleaseDateTime}"/>
            </StackPanel>
        </StackPanel>
    </Grid>
</Page>
```

[**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) 기술의 경우 먼저 **CollectionViewSource**를 페이지 리소스로 추가합니다.

```xml
    <Page.Resources>
        <CollectionViewSource x:Name="RecordingsCollection" Source="{x:Bind ViewModel.Recordings}"/>
    </Page.Resources>
```

그런 다음 [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833)를 사용하도록 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878)(더 이상 이름을 지정할 필요 없음)와 세부 정보 보기에 대한 바인딩을 조정합니다. 세부 정보 보기를 **CollectionViewSource**에 직접 바인딩하면 컬렉션 자체에서 경로를 찾을 수 없는 바인딩에서 현재 항목에 바인딩할 수 있습니다. **CurrentItem** 속성을 바인딩 경로로 지정할 필요는 없습니다(모호한 경우에는 지정할 수 있음).

```xml
    ...

    <ListView ItemsSource="{Binding Source={StaticResource RecordingsCollection}}">

    ...

    <StackPanel DataContext="{Binding Source={StaticResource RecordingsCollection}}" ...>
    ...
```

다음은 각 경우의 동일한 결과입니다.

![목록 보기 바인딩](images/xaml-databinding4.png)

표시할 데이터 값 필터링 또는 변환
--------------------------------------------------------------------------------------------------------------------------------------------

위 렌더링에는 사소한 문제가 하나 있습니다. **ReleaseDateTime** 속성이 날짜가 아니라 [**DateTime**](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetime.aspx)이므로 필요한 것보다 더 정밀하게 표시됩니다. 한 가지 해결 방법은 `this.ReleaseDateTime.ToString("d")`을 반환하는 **Recording** 클래스에 문자열 속성을 추가하는 것입니다. 이 속성의 이름을 **ReleaseDate**로 지정하면 날짜 및 시간이 아니라 날짜가 반환되고, **ReleaseDateAsString**으로 지정하면 문자열이 반환됩니다.

보다 유연한 해결 방법은 값 변환기라는 것을 사용하는 것입니다. 다음은 사용자 고유의 값 변환기를 작성하는 방법에 대한 예제입니다. 이 코드를 Recording.cs 소스 코드 파일에 추가합니다.

```csharp
public class StringFormatter : Windows.UI.Xaml.Data.IValueConverter
{
    // This converts the value object to the string to display.
    // This will work with most simple types.
    public object Convert(object value, Type targetType,
        object parameter, string language)
    {
        // Retrieve the format string and use it to format the value.
        string formatString = parameter as string;
        if (!string.IsNullOrEmpty(formatString))
        {
            return string.Format(formatString, value);
        }

        // If the format string is null or empty, simply
        // call ToString() on the value.
        return value.ToString();
    }

    // No need to implement converting back on a one-way binding
    public object ConvertBack(object value, Type targetType,
        object parameter, string language)
    {
        throw new NotImplementedException();
    }
}
```

이제 **StringFormatter**의 인스턴스를 페이지 리소스로 추가하고 바인딩에서 사용할 수 있습니다. 형식 지정 유연성을 극대화하기 위해 태그에서 변환기로 형식 문자열을 전달합니다.

```xml
    <Page.Resources>
        <local:StringFormatter x:Key="StringFormatterValueConverter"/>
    </Page.Resources>
    ...

    <TextBlock Text="{Binding ReleaseDateTime,
        Converter={StaticResource StringFormatterValueConverter},
        ConverterParameter=Released: \{0:d\}}"/>

    ...
```

다음은 결과입니다.

![사용자 지정 형식으로 날짜 표시](images/xaml-databinding5.png)





<!--HONumber=Jun16_HO5-->


