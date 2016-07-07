---
author: mcleblanc
ms.assetid: 0C69521B-47E0-421F-857B-851B0E9605F2
title: "계층적 데이터에 바인딩하고 마스터/자세히 보기 만들기"
description: "체인으로 함께 바인딩된 CollectionViewSource 인스턴스에 항목 컨트롤을 바인딩하여 계층적 데이터에 대한 여러 수준 마스터/세부 정보(목록-세부 정보라고도 함) 보기를 만들 수 있습니다."
translationtype: Human Translation
ms.sourcegitcommit: afb508fcbc2d4ab75188a2d4f705ea0bee385ed6
ms.openlocfilehash: 2ff66a1d6a80bb085f54dec8e35371ba0c9e6b27

---
# 계층적 데이터에 바인딩하고 마스터/자세히 보기 만들기

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


> **참고** [마스터/세부 정보 샘플](http://go.microsoft.com/fwlink/p/?linkid=619991)도 참조하세요.

체인으로 함께 바인딩된 [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) 인스턴스에 항목 컨트롤을 바인딩하여 계층적 데이터에 대한 여러 수준 마스터/세부 정보(목록-세부 정보라고도 함) 보기를 만들 수 있습니다. 이 항목에서는 가능한 경우 [{x:Bind} 태그 확장](https://msdn.microsoft.com/library/windows/apps/Mt204783)을 사용하며, 필요한 경우 보다 유연한(그러나 성능이 낮은) [{Binding} 태그 확장](https://msdn.microsoft.com/library/windows/apps/Mt204782)을 사용합니다.

UWP(유니버설 Windows 플랫폼) 앱의 한 가지 일반적인 구조는 사용자가 마스터 목록에서 항목을 선택할 때 다른 세부 정보 페이지로 이동하는 것입니다. 이 구조는 계층 구조의 모든 수준에서 각 항목의 풍부한 시각적 표시를 제공하려는 경우 유용합니다. 또 다른 옵션은 단일 페이지에 다단계로 된 데이터를 표시하는 것입니다. 이 구조는 사용자가 관심 있는 항목으로 빠르게 드릴다운할 수 있는 몇 개의 간단한 목록을 표시하려는 경우 유용합니다. 이 항목에서는 이 조작을 구현하는 방법을 설명합니다. [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) 인스턴스는 각 계층 수준에서의 현재 선택을 추적합니다.

리그, 지구 및 팀에 대한 목록으로 구성되어 있고 팀 세부 정보 보기가 포함된 스포츠 팀 계층 구조 보기를 만듭니다. 목록에서 항목을 선택하면 이후 보기가 자동으로 업데이트됩니다.

![스포츠 계층 구조의 마스터/세부 정보 보기](images/xaml-masterdetails.png)

## 필수 조건

이 항목에서는 사용자가 기본 UWP 앱을 만드는 방법을 알고 있다고 가정합니다. 첫 UWP 앱을 만드는 방법은 [C# 또는 Visual Basic을 사용하여 첫 UWP 앱 만들기](https://msdn.microsoft.com/library/windows/apps/Hh974581)를 참조하세요.

## 프로젝트 만들기

**비어 있는 응용 프로그램(Windows 유니버설)** 프로젝트를 만듭니다. 이름을 "MasterDetailsBinding"으로 지정합니다.

## 데이터 모델 만들기

프로젝트에 새 클래스를 추가하고 이름을 ViewModel.cs로 지정한 후 다음 코드를 추가합니다. 이것이 바인딩 소스 클래스가 됩니다.

```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace MasterDetailsBinding
{
    public class Team
    {
        public string Name { get; set; }
        public int Wins { get; set; }
        public int Losses { get; set; }
    }

    public class Division
    {
        public string Name { get; set; }
        public IEnumerable<Team> Teams { get; set; }
    }

    public class League
    {
        public string Name { get; set; }
        public IEnumerable<Division> Divisions { get; set; }
    }

    public class LeagueList : List<League>
    {
        public LeagueList()
        {
            this.AddRange(GetLeague().ToList());
        }

        public IEnumerable<League> GetLeague()
        {
            return from x in Enumerable.Range(1, 2)
                   select new League
                   {
                       Name = "League " + x,
                       Divisions = GetDivisions(x).ToList()
                   };
        }

        public IEnumerable<Division> GetDivisions(int x)
        {
            return from y in Enumerable.Range(1, 3)
                   select new Division
                   {
                       Name = String.Format("Division {0}-{1}", x, y),
                       Teams = GetTeams(x, y).ToList()
                   };
        }

        public IEnumerable<Team> GetTeams(int x, int y)
        {
            return from z in Enumerable.Range(1, 4)
                   select new Team
                   {
                       Name = String.Format("Team {0}-{1}-{2}", x, y, z),
                       Wins = 25 - (x * y * z),
                       Losses = x * y * z
                   };
        }
    }
}
```

## 보기 만들기

그런 다음 태그 페이지를 나타내는 클래스에서 바인딩 소스 클래스를 노출합니다. **LeagueList** 형식의 속성을 **MainPage**에 바인딩하면 됩니다.

```cs
namespace MasterDetailsBinding
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.ViewModel = new LeagueList();
        }
        public LeagueList ViewModel { get; set; }
    }
}
```

마지막으로, MainPage.xaml 파일의 내용을 세 개의 [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) 인스턴스를 선언하고 체인으로 함께 바인딩하는 다음 태그로 바꿉니다. 그러면 이후 컨트롤은 계층 구조의 해당 수준에 따라 적절한 **CollectionViewSource**에 바인딩할 수 있습니다.

```xml
<Page
    x:Class="MasterDetailsBinding.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MasterDetailsBinding"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Page.Resources>
        <CollectionViewSource x:Name="Leagues"
            Source="{x:Bind ViewModel}"/>
        <CollectionViewSource x:Name="Divisions"
            Source="{Binding Divisions, Source={StaticResource Leagues}}"/>
        <CollectionViewSource x:Name="Teams"
            Source="{Binding Teams, Source={StaticResource Divisions}}"/>

        <Style TargetType="TextBlock">
            <Setter Property="FontSize" Value="15"/>
            <Setter Property="FontWeight" Value="Bold"/>
        </Style>

        <Style TargetType="ListBox">
            <Setter Property="FontSize" Value="15"/>
        </Style>

        <Style TargetType="ContentControl">
            <Setter Property="FontSize" Value="15"/>
        </Style>

    </Page.Resources>

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

        <StackPanel Orientation="Horizontal">

            <!-- All Leagues view -->

            <StackPanel Margin="5">
                <TextBlock Text="All Leagues"/>
                <ListBox ItemsSource="{Binding Source={StaticResource Leagues}}" 
                    DisplayMemberPath="Name"/>
            </StackPanel>

            <!-- League/Divisions view -->

            <StackPanel Margin="5">
                <TextBlock Text="{Binding Name, Source={StaticResource Leagues}}"/>
                <ListBox ItemsSource="{Binding Source={StaticResource Divisions}}" 
                    DisplayMemberPath="Name"/>
            </StackPanel>

            <!-- Division/Teams view -->

            <StackPanel Margin="5">
                <TextBlock Text="{Binding Name, Source={StaticResource Divisions}}"/>
                <ListBox ItemsSource="{Binding Source={StaticResource Teams}}" 
                    DisplayMemberPath="Name"/>
            </StackPanel>

            <!-- Team view -->

            <ContentControl Content="{Binding Source={StaticResource Teams}}">
                <ContentControl.ContentTemplate>
                    <DataTemplate>
                        <StackPanel Margin="5">
                            <TextBlock Text="{Binding Name}" 
                                FontSize="15" FontWeight="Bold"/>
                            <StackPanel Orientation="Horizontal" Margin="10,10">
                                <TextBlock Text="Wins:" Margin="0,0,5,0"/>
                                <TextBlock Text="{Binding Wins}"/>
                            </StackPanel>
                            <StackPanel Orientation="Horizontal" Margin="10,0">
                                <TextBlock Text="Losses:" Margin="0,0,5,0"/>
                                <TextBlock Text="{Binding Losses}"/>
                            </StackPanel>
                        </StackPanel>
                    </DataTemplate>
                </ContentControl.ContentTemplate>
            </ContentControl>

        </StackPanel>

    </Grid>
</Page>
```

[**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833)에 직접 바인딩하면 컬렉션 자체에서 경로를 찾을 수 없는 바인딩에서 현재 항목에 바인딩할 수 있습니다. **CurrentItem** 속성을 바인딩 경로로 지정할 필요는 없습니다(모호한 경우에는 지정할 수 있음). 예를 들어 팀 보기를 나타내는 [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/BR209365)의 [**Content**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentcontrol.content) 속성은 `Teams`**CollectionViewSource**에 바인딩되어 있습니다. 그러나 필요한 경우 **CollectionViewSource**가 팀 목록에서 현재 선택한 팀을 자동으로 제공하므로 [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242348)의 컨트롤은 `Team` 클래스의 속성에 바인딩됩니다.

 

 




<!--HONumber=Jun16_HO4-->


