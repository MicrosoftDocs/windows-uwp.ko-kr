---
author: mijacobs
description: 
title: "인물 사진 컨트롤"
template: detail.hbs
label: Parallax View
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: kevjey
design-contact: kimsea
dev-contact: kefodero
doc-status: Published
ms.openlocfilehash: c71fdf3d19c8c8e43666620df53258825c4a8eb1
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/22/2017
---
# <a name="person-picture-control"></a>인물 사진 컨트롤

> [!IMPORTANT]
> 이 문서에서는 아직 출시되지 않아 상업적으로 출시하기 전에 크게 수정될 수 있는 기능에 대해 설명합니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다.

인물 사진 컨트롤은 사람의 아바타 이미지가 있는 경우 해당 이미지를 표시하고, 없는 경우 사람의 이니셜이나 일반 글자 모양을 표시합니다. 컨트롤을 사용하여 사람의 연락처 정보를 관리하는 개체인 [Contact 개체](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Contacts.Contact)를 표시하거나 수동으로 표시 이름과 프로필 사진 등의 연락처 정보를 제공할 수 있습니다.  

> **중요 API**: [PersonPicture 클래스](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.personpicture), [Contact 클래스](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Contacts.Contact), [ContactManager 클래스](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Contacts.ContactManager)

![인물 사진 컨트롤](images/person-picture/person-picture_hero.png)

## <a name="is-this-the-right-control"></a>올바른 컨트롤인가요?

사람과 해당 연락처 정보를 표시하려면 인물 사진을 사용하세요. 다음은 인물 사진 컨트롤의 사용 예입니다.
* 현재 사용자 표시
* 주소록에서 연락처 표시
* 메시지를 보낸 사람 표시 
* 소셜 미디어 연락처 표시

다음 그림에는 연락처 목록의 인물 사진 컨트롤이 나와 있습니다. ![인물 사진 컨트롤](images/person-picture/person-picture-control.png)

## <a name="how-to-use-the-person-picture-control"></a>인물 사진 컨트롤 사용 방법

인물 사진을 만들려면 PersonPicture 클래스를 사용합니다. 이 예에서는 PersonPicture 컨트롤을 만들고 수동으로 사람의 표시 이름, 프로필 사진 및 이니셜을 제공합니다.

```xaml
<Page
    x:Class="App2.ExamplePage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App2"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <PersonPicture
            DisplayName="Betsy Sherman"
            ProfilePicture="Assets\BetsyShermanProfile.png"
            Initials="BS" />
    </StackPanel>
</Page>
```

## <a name="using-the-person-picture-control-to-display-a-contact-object"></a>인물 사진 컨트롤을 사용하여 Contact 개체 표시

사람 선택 컨트롤을 사용하여 [Contact](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Contacts.Contact) 개체를 표시할 수 있습니다. 

```xaml
<Page
    x:Class="SampleApp.PersonPictureContactExample"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:SampleApp"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

        <PersonPicture
            Contact="{x:Bind CurrentContact, Mode=OneWay}" />
            
        <Button Click="LoadContactButton_Click">Load contact</Button>
    </StackPanel>
</Page>
```

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Runtime.InteropServices.WindowsRuntime;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;
using Windows.ApplicationModel.Contacts;

namespace SampleApp
{
    public sealed partial class PersonPictureContactExample : Page, System.ComponentModel.INotifyPropertyChanged
    {
        public PersonPictureContactExample()
        {
            this.InitializeComponent();
        }

        private Windows.ApplicationModel.Contacts.Contact _currentContact; 
        public Windows.ApplicationModel.Contacts.Contact CurrentContact
        {
            get => _currentContact;
            set
            {
                _currentContact = value;
                PropertyChanged?.Invoke(this,
                    new System.ComponentModel.PropertyChangedEventArgs(nameof(CurrentContact)));
            }

        }
        public event System.ComponentModel.PropertyChangedEventHandler PropertyChanged;

        public static async System.Threading.Tasks.Task<Windows.ApplicationModel.Contacts.Contact> CreateContact()
        {

            var contact = new Windows.ApplicationModel.Contacts.Contact();
            contact.FirstName = "Betsy";
            contact.LastName = "Sherman";

            // Get the app folder where the images are stored.
            var appInstalledFolder = 
                Windows.ApplicationModel.Package.Current.InstalledLocation;
            var assets = await appInstalledFolder.GetFolderAsync("Assets");
            var imageFile = await assets.GetFileAsync("betsy.png");
            contact.SourceDisplayPicture = imageFile;

            return contact;
        }

        private async void LoadContactButton_Click(object sender, RoutedEventArgs e)
        {
            CurrentContact = await CreateContact();
        }
    }
}
```

> [!NOTE]
> 코드를 단순하게 유지하기 위해 이 예에서는 새 Contact 개체를 만듭니다. 실제 앱에서는 사용자가 연락처를 선택하게 하거나 [ContactManager](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Contacts.ContactManager)를 사용하여 연락처 목록을 쿼리합니다. 연락처 검색 및 관리에 대한 자세한 내용은 [연락처 및 일정 문서](../contacts-and-calendar/index.md)를 참조하세요. 

## <a name="determining-which-info-to-display"></a>표시할 정보 결정

[Contact](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Contacts.Contact) 개체를 제공하면 인물 사진 컨트롤이 이를 평가하여 표시할 수 있는 정보를 결정합니다. 

이미지를 사용할 수 있는 경우 컨트롤에서 찾는 첫 번째 이미지를 다음과 같은 순서로 표시합니다.

1.    LargeDisplayPicture
2.    SmallDisplayPicture
3.    Thumbnail

PreferSmallImage 속성을 true로 설정하여 선택되는 이미지를 변경할 수 있습니다. 이렇게 설정하면 SmallDisplayPicture에 LargeDisplayPicture보다 높은 우선 순위가 지정됩니다.

이미지가 없는 경우 컨트롤에서 연락처의 이름이나 이니셜을 표시합니다. 이름 데이터가 없는 경우 컨트롤에서 전자 메일 주소나 전화 번호 등의 연락처 데이터를 표시합니다. 

## <a name="related-articles"></a>관련 문서

* [연락처 및 일정](../contacts-and-calendar/index.md)
* [연락처 카드 샘플](http://go.microsoft.com/fwlink/p/?LinkId=624040)




